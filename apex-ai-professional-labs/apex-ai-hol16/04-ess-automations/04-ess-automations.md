# Lab 4: Create ESS Scheduled Automations

## Introduction

Create three scheduled automations in ESS. Automations run a sequential set of actions when query results identify data to process. The actions can use columns from the current query row.

Estimated Workshop Time: 10 minutes

### Objectives

In this lab, you will learn how to:

- Create the Task Overdue automation.
- Create the Monthly Leave Accrual automation.
- Create the Probation End Alert automation.

## Task 1: Create the Task Overdue automation

1. In ESS, navigate to **Shared Components**.

    ![Shared Components breadcrumb on the ESS Email Templates page](images/task-01-step-01-shared-components.png)

2. Under **Workflows and Automations**, select **Automations**.

    ![Task 1: Automations](images/task-01-step-02-automations.png)

3. Click **Create**.

    ![Task 1: Create automation](images/task-01-step-03-automations-create.png)

4. In the **Create Automation** wizard, enter or select:

    - **Name:** Task Overdue.
    - **Type:** Scheduled.
    - **Actions initiated on:** Query.
    - **Execution Schedule:** Custom.
    - **Frequency:** Daily.
    - **Interval:** `1`.
    - **Execution Time:** `08:00`.
    - Click **Next**.

    - The schedule runs in the database server time zone.

    ![Task Overdue automation settings with Daily highlighted](images/task-01-step-04-task-overdue-automation-annotated.svg)

5. Set **Source Type** to **SQL Query**.

    ![SQL Query selected as the automation source type](images/task-01-step-05-source-type-sql-query.svg)

6. For **Enter a SQL SELECT Statement**, copy and paste the following SQL query:

    ```sql
    <copy>
        SELECT t.task_id,
           t.task_name,
           t.due_date,
           TO_CHAR(t.due_date, 'DD-Mon-YYYY') AS due_date_text,
           e.employee_id,
           e.email AS employee_email,
           e.first_name || ' ' || e.last_name AS employee_name
      FROM tms_onboarding_tasks t
      JOIN tms_employees e ON e.employee_id = t.employee_id
     WHERE t.due_date < TRUNC(SYSDATE)
       AND t.status NOT IN ('Completed', 'Cancelled', 'Overdue')
    </copy>
    ```

    ![Task Overdue source query and Create button](images/task-01-step-06-task-overdue-sql-query.png)

7. Click **Create** to create the automation.

8. Scroll down to **Actions** and click **Create Action**.

    ![Create Action control in the Task Overdue automation](images/task-01-step-08-create-action.png)

9. Enter or select the following attributes:

    - **Name:** Update Task.
    - **Type:** Execute Code.
    - **Code:** Copy and paste the following PL/SQL code:

        ```sql
        <copy>
            BEGIN
            UPDATE tms_onboarding_tasks
                SET status = 'Overdue'
                WHERE task_id = :TASK_ID;
            END;
        </copy>
        ```

    ![Update Task action name, PL/SQL code, and Create button](images/task-01-step-09-update-task.png)

10. Click **Create** to save the **Update Task** action.

11. Under **Actions**, click **Create Action** and configure the following:

    - Under **Identification** (shown as **Action** in the screenshot):
        - **Name:** Send Email.
        - **Type:** Send E-Mail.
    - Under **Send Email Settings**:
        - **From:** `&APP_EMAIL.`.
        - **To:** `&EMPLOYEE_EMAIL.`.
        - **Email Template:** Task Overdue Notification.
    - Click **Set Placeholder Values**.

    ![Send E-Mail action type, recipient, template, and Set Placeholder Values button](images/task-01-step-11-send-email-settings.png)

12. Enter the following values in the **Placeholder Grid**:

    | Placeholder | Value |
    | --- | --- |
    | `EMPLOYEE_NAME` | `&EMPLOYEE_NAME.` |
    | `TASK_NAME` | `&TASK_NAME.` |
    | `DUE_DATE` | `&DUE_DATE_TEXT.` |

    - Click **Save**, then close the placeholder dialog.

    ![Email placeholder mappings with Save highlighted](images/task-01-step-12-email-placeholders.svg)

13. Click **Create** to save the email action.

    ![Saved placeholder values and Create button for the email action](images/task-01-step-13-create-email-action.png)

14. Set **Schedule Status** to **Active** and click **Save and Run**.

    ![Task 1: Task overdue set active run](images/task-01-step-14-task-overdue-set-active-run.png)

## Task 2: Create Monthly Leave Accrual

1. Create a second automation. Navigate to **Automations**.

    - Click **Create**.
    - In the **Create Automation** wizard, enter or select:
        - **Name:** Monthly Leave Accrual.
        - **Type:** On Demand.
        - **Actions initiated on:** Query.
    - Click **Next**.
    - After creating the automation, change its type to **Scheduled** as described below.

    ![Task 2: Monthly leave accrual](images/task-02-step-01-monthly-leave-accrual.png)

2. Set **Source Type** to **SQL Query**.

    - For **Enter a SQL SELECT Statement**, copy and paste the following SQL query:

        ```sql
        <copy>
        SELECT employee_id
          FROM tms_employees
         WHERE status = 'Active'
         </copy>
        ```
    ![Task 2: Monthly leave accrual SQL query](images/task-02-step-02-monthly-leave-accrual-sql-query.png)

3. Click **Create**.

    ![Create button for the Monthly Leave Accrual automation](images/task-02-step-03-create-automation.svg)

4. Change **Type** to **Scheduled**.

    - Enter this **Schedule Expression**:

        ```text
        FREQ=MONTHLY;BYMONTHDAY=1;BYHOUR=0;BYMINUTE=0;BYSECOND=0
        ```
    ![Task 2: Monthly leave accrual schedule](images/task-02-step-04-monthly-leave-accrual-schedule.png)

5. Add an **Execute Code** action named **Merge Leave Balances**.

    - This example uses the `MAX_LEAVE_DAYS` application setting and leave type ID `1`. Create or adjust them to match your application.

        ```sql
        <copy>

        DECLARE
        l_accrual NUMBER := ROUND(apex_app_setting.get_value('MAX_LEAVE_DAYS') / 12, 1);
        BEGIN
            MERGE INTO tms_leave_balances b
            USING (
                SELECT :EMPLOYEE_ID AS employee_id,
                    1 AS leave_type_id,
                    EXTRACT(YEAR FROM SYSDATE) AS leave_year
                FROM dual
            ) s
            ON (b.employee_id = s.employee_id
            AND b.leave_type_id = s.leave_type_id
            AND b.year = s.leave_year)
            WHEN MATCHED THEN
                UPDATE SET accrued = NVL(b.accrued, 0) + l_accrual,
                        remaining = NVL(b.remaining, 0) + l_accrual
            WHEN NOT MATCHED THEN
                INSERT (employee_id, leave_type_id, year, accrued, remaining)
                VALUES (s.employee_id, s.leave_type_id, s.leave_year, l_accrual, l_accrual);
        END;

        </copy>
        ```
    ![Task 2: Monthly leave accrual add execute action](images/task-02-step-05-monthly-leave-accrual-add-execute-action.png)

6. Set the schedule to **Active** and click **Save and Run**.
    ![Task 2: Monthly leave accrual set active run](images/task-02-step-06-monthly-leave-accrual-set-active-run.png)

## Task 3: Create Probation End Alert

1. Create a scheduled automation named **Probation End Alert**.

    - Select **Query**, **Custom**, **Daily**, and an interval of `1`.

    - Set **Execution Time** to `07:00`. The resulting schedule expression is `FREQ=DAILY;INTERVAL=1;BYHOUR=7;BYMINUTE=0` in the database server time zone.
    ![Task 3: Probation end alert](images/task-03-step-01-probation-end-alert.png)

2. Set **Source Type** to **SQL Query**.

    - Enter:

        ```sql
        <copy>SELECT e.employee_id,
               e.first_name || ' ' || e.last_name AS employee_name,
               e.hire_date,
               ADD_MONTHS(e.hire_date,
                   apex_app_setting.get_value('PROBATION_MONTHS')) AS probation_end
          FROM tms_employees e
         WHERE e.status = 'Active'
           AND ADD_MONTHS(e.hire_date,
               apex_app_setting.get_value('PROBATION_MONTHS'))
               BETWEEN TRUNC(SYSDATE) AND TRUNC(SYSDATE) + 7</copy>
        ```
    ![Task 3: Probation end alert SQL query](images/task-03-step-02-probation-end-alert-sql-query.png)

3. Add a **Send E-Mail** action named **Send Probation Alert**.

    ![Task 3: Probation alert email action, part 1](images/task-03-step-03-probation-end-alert-send-email-action-01.png)

4. Set **From** to `&APP_EMAIL.` and **To** to `apex_app_setting.get_value('SUPPORT_EMAIL')`.

    - Select the **Probation Alert** email template.

    ![Task 3: Probation alert email action, part 2](images/task-03-step-04-probation-end-alert-send-email-action-02.png)

5. In the placeholder grid, map these values:

    | Placeholder | Column or Value |
    | --- | --- |
    | `EMPLOYEE_NAME` | `&EMPLOYEE_NAME.` |
    | `HIRE_DATE` | `&HIRE_DATE.` |
    | `PROBATION_END` | `&PROBATION_END.` |

    ![Task 3: Probation alert email action, part 3](images/task-03-step-05-probation-end-alert-send-email-action-03.png)

6. Set the schedule to **Active** and click **Save and Run**.
    ![Task 3: Probation end alert set active](images/task-03-step-06-probation-end-alert-set-active.png)

## Task 4: Monitor automation runs

1. Open **Shared Components**, select **Automations**, then select **Execution Log**.

    - Review the run history and messages for each automation.
    ![Task 4: Execution log](images/task-04-step-01-execution-log.png)

2. Open the **System Logs** page from next lab (Lab 5) for an HR administrator.

    - Confirm that it displays the same automation-run data from `APEX_AUTOMATION_LOG`.

3. If an email does not arrive, check `APEX_MAIL_LOG` and the mail queue to identify the failure.

## Acknowledgements

- **Author -** Aravind Madhavan, Senior Product Manager.
- **Last Updated By/Date** - Aravind Madhavan, Senior Product Manager, July 2026
