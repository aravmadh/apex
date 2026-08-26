# Create ESS Scheduled Automations

## Introduction

Create three scheduled automations in ESS. Each automation runs actions in the background. When a query returns rows, actions can use the current-row columns.

### Objectives

- Create the Task Overdue automation.
- Create the Monthly Leave Accrual automation.
- Create the Probation End Alert automation.

Estimated Time: 10 minutes

## Task 1: Create the Task Overdue automation

1. In ESS, select **Shared Components**, then select **Automations** and **Create**. Select **Scheduled**.
![](images/task-01-step-01-shared-components.png)
![](images/task-01-step-01-automations.png)
![](images/task-01-step-01-automations-create.png)


2. Set **Name** to **Task Overdue**. Select **Query** as the source type. Schedule it daily at `08:00 UTC`.
![](images/task-01-step-02-task-overdue-automation.png)


3. Enter the Source Query and click on `Create`

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
    ![](images/task-01-step-03-task-overdue-sql-query.png)

4. Once the Automation is created Add **Execute Code** action that marks the current task as overdue:

    ```sql
    <copy>
        UPDATE tms_onboarding_tasks
            SET status = 'Overdue'
            WHERE task_id = :TASK_ID;
    </copy>
    ```
    ![](images/task-01-step-04-task-overdue-add-action.png)
    ![](images/task-01-step-04-task-overdue-update-task-action.png)

5. Now Add a new **Send E-Mail** action. Set **To** to `&EMPLOYEE_EMAIL.`. Select `TASK_OVERDUE`. In the placeholder grid, set these values:

    | Placeholder | Value |
    | --- | --- |
    | `EMPLOYEE_NAME` | `&EMPLOYEE_NAME.` |
    | `TASK_NAME` | `&TASK_NAME.` |
    | `DUE_DATE` | `&DUE_DATE_TEXT.` |

    ![](images/task-01-step-05-task-overdue-send-email-action.png)
    ![](images/task-01-step-05-task-overdue-send-email-action-placeholders.png)

6. Set the schedule to **Active** and click **Save and Run**.

    ![](images/task-01-step-06-task-overdue-set-active-run.png)

## Task 2: Create Monthly Leave Accrual

1. Create a second scheduled automation named **Monthly Leave Accrual**. Select `On Demand` for now we will change it later to Schduled to run on 1st day of every month.
![](images/task-02-step-01-monthly-leave-accrual.png)

2. Use the below sql query for the Source Type set as `SQL Query`

    ```sql
    <copy>
    SELECT employee_id
      FROM tms_employees
     WHERE status = 'Active'
     </copy>
    ```
    ![](images/task-02-step-02-monthly-leave-accrual-sql-query.png)
3. Change the Type to Scheduled and give the below `Schedule Expression`

    ```text
    FREQ=MONTHLY;BYMONTHDAY=1;BYHOUR=0;BYMINUTE=0;BYSECOND=0
    ```
    ![](images/task-02-step-03-monthly-leave-accrual-schedule.png)

4. Add an **Execute Code** action. This example uses the `MAX_LEAVE_DAYS` application setting and leave type ID `1`. Create or adjust them to match your application.

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
    ![](images/task-02-step-04-monthly-leave-accrual-add-execute-action.png)


5. Set the schedule to **Active** and click **Save and Run**.
![](images/task-02-step-05-monthly-leave-accrual-set-active-run.png)

## Task 3: Create Probation End Alert

1. Create a scheduled automation named **Probation End Alert**. Schedule it daily at `07:00 UTC`.
![](images/task-03-step-01-probation-end-alert.png)

2. Use this query for Source Type SQL Query:

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
    ![](images/task-03-step-02-probation-end-alert-sql-query.png)

3. Add a **Send E-Mail** action. Set **To** to the value of the `SUPPORT_EMAIL` application setting. Select `PROBATION_ALERT`. Map `EMPLOYEE_NAME`, `HIRE_DATE`, and `PROBATION_END` in the placeholder grid.
![](images/task-03-step-03-probation-end-alert-send-email-action-01.png)
![](images/task-03-step-03-probation-end-alert-send-email-action-02.png)
![](images/task-03-step-03-probation-end-alert-send-email-action-03.png)

4. Set the schedule to **Active** and click **Save and Run**.
![](images/task-03-step-04-probation-end-alert-set-active.png)


## Task 4: Monitor automation runs

1. Open **Shared Components**, select **Automations**, then select **Execution Log**. Review the run history and messages for each automation.
![](images/task-04-step-01-execution-log.png)

2. Open the **System Logs** page from next lab (Lab 5) for an HR administrator. Confirm that it displays the same automation-run data from `APEX_AUTOMATION_LOG`.

3. If an email does not arrive, check `APEX_MAIL_LOG` and the mail queue to identify the failure.

## Acknowledgements

- **Author -** Aravind Madhavan, Senior Product Manager.
- **Last Updated By/Date** - Aravind Madhavan, Senior Product Manager, July 2026
