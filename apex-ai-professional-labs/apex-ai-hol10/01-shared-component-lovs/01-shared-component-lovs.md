# Lab 1: Create Shared Lists of Values

## Introduction

In Oracle APEX, a list of values (LOV) supplies values to LOV-based page items and report columns. Named LOVs in Shared Components are reusable throughout an application. Create TAP LOVs for recruitment forms, then create ESS LOVs for leave and onboarding tasks.

Estimated Workshop Time: 5 minutes

### Objectives

In this lab, you will learn how to:

- Create TAP interviewer, interview-stage, and offer-status LOVs.
- Create ESS leave-type, onboarding-task status, and task-category LOVs.
- Verify that each LOV is available when you configure page items.

## Task 1: Create the TAP LOVs

1. In TAP, navigate to **Shared Components**.

    ![Shared Components](images/task-01-step-01-shared-components.png)

2. Under **Other Components**, select **List of Values**.

    ![List of Values](images/task-01-step-02-list-of-values.png)

3. Select **Create**.

    ![Create LOV](images/task-01-step-03-create-lov.png)

4. Create a dynamic LOV named `TMS_INTERVIEWERS.NAME`. In the **Create List of Values** wizard, select **From Scratch** and click **Next**.

    ![Select From Scratch in the Create List of Values wizard](images/task-01-step-04-create-lov-wizard-name.png)

5. For **Name**, enter `TMS_INTERVIEWERS.NAME`.

    - Select **Dynamic** as the **Type**, then click **Next**.

    ![Enter the LOV name and select Dynamic type](images/task-01-step-05-create-lov-name.png)

6. Set **Source Type** to **SQL Query**.

    - For **Enter a SQL SELECT Statement**, copy and paste the following query. It returns the display value as `d` and the return value as `r`.
    ```
    <copy>
    SELECT first_name || ' ' || last_name d,
           employee_id r
      FROM tms_employees
     WHERE status = 'Active'
     </copy>
    ```
    - Click **Next**.

    ![Enter the SQL query for the LOV source](images/task-01-step-06-create-lov-source.png)

7. Leave **Column Mappings** as the default and click **Create**.

    ![Confirm the default column mappings and create the LOV](images/task-01-step-07-create-lov-col-mapping.png)

8. Create the static LOV named `TMS_INTERVIEW.STAGES`.

    - Click **Create** to start creating a static LOV.

    ![Create another shared list of values](images/task-01-step-08-create-lov.png)

9. In the **Create List of Values** wizard, select **From Scratch** and click **Next**.

    ![Select From Scratch in the Create List of Values wizard](images/task-01-step-09-create-lov-wizard-name.png)

10. For **Name**, enter `TMS_INTERVIEW.STAGES`.

    - Select **Static** as the **Type**, then click **Next**.

    ![Enter the static LOV name and select Static type](images/task-01-step-10-create-static-lov-wizard.png)

11. Enter the following values, using each stage as both the **Display Value** and **Return Value**, then click **Create List of Values**:

    | Display Value | Return Value |
    | ------------- | ------------ |
    | Applied       | Applied      |
    | Screening     | Screening    |
    | Interview     | Interview    |
    | Offer         | Offer        |
    | Hired         | Hired        |
    | Rejected      | Rejected     |

    ![Enter static LOV display and return values](images/task-01-step-11-create-static-lov-wizard-name.png)

12. Create the static LOV named `TMS_OFFER.STATUS`.

    - Click **Create** to start creating a static LOV.

    ![Create another shared list of values](images/task-01-step-12-create-lov.png)

13. In the **Create List of Values** wizard, select **From Scratch** and click **Next**.

    ![Select From Scratch in the Create List of Values wizard](images/task-01-step-13-create-lov-wizard-name.png)

14. For **Name**, enter `TMS_OFFER.STATUS`.

    - Select **Static** as the **Type**, then click **Next**.

    ![Enter the status LOV name and select Static type](images/task-01-step-14-static-value-wiz.png)

15. Enter the following values, using each status as both the **Display Value** and **Return Value**, then click **Create List of Values**:

    | Display Value    | Return Value     |
    | ---------------- | ---------------- |
    | Draft            | Draft            |
    | Pending Approval | Pending Approval |
    | Sent             | Sent             |
    | Accepted         | Accepted         |
    | Rejected         | Rejected         |
    | Withdrawn        | Withdrawn        |

    ![Enter static status LOV display and return values](images/task-01-step-15-static-lov-col-disp-ret.png)

16. Confirm that all three LOVs appear in the application’s **Lists of Values** page.

    ![LOVs](images/task-01-step-16-lovs.png)

## Task 2: Create the ESS LOVs

1. Return to **App Builder** and select **Employee Self-Service Portal (ESS)**.

    - Open **Shared Components**.

    ![Shared Components](images/task-02-step-01-shared-components.png)

2. Under **Other Components**, select **List of Values**.

    ![List of Values](images/task-02-step-02-list-of-values.png)

3. Select **Create**.

    ![Create LOV](images/task-02-step-03-create-lov.png)

4. Create the dynamic LOV named `TMS_LEAVE.TYPES`. In the **Create List of Values** wizard, select **From Scratch** and click **Next**.

    ![Select From Scratch in the Create List of Values wizard](images/task-02-step-04-create-lov-wizard-name.png)

5. For **Name**, enter `TMS_LEAVE.TYPES`.

    - Select **Dynamic** as the **Type**, then click **Next**.

    ![Enter the dynamic LOV name and select Dynamic type](images/task-02-step-05-create-dynamic-lov-wizard-name.png)

6. Keep **Data Source** set to **Local Database**.

    - Under **Source Type**, select **Table**.

    - For **Table / View Name**, select `TMS_LEAVE_TYPES`, then click **Next**.

    ![Select Local Database, Table, and TMS_LEAVE_TYPES](images/task-02-step-06-create-dynamic-lov-wizard-values.png)

7. Configure **Column Mappings**:

    - **Return Column:** `LEAVE_TYPE_ID`.
    - **Display Column:** `NAME`.
    - Click **Create**.

    ![Confirm the return and display column mappings and create the LOV](images/task-02-step-07-create-dynamic-lov-col-mapping.png)

8. Create the static LOV named `TMS_TASK.STATUS`.

    - Click **Create** to start creating a static LOV.

    ![Create another shared list of values](images/task-02-step-08-create-lov.png)

9. In the **Create List of Values** wizard, select **From Scratch** and click **Next**.

    ![Select From Scratch in the Create List of Values wizard](images/task-02-step-09-create-lov-wizard-name.png)

10. For **Name**, enter `TMS_TASK.STATUS`.

    - Select **Static** as the **Type**, then click **Next**.

    ![Enter the TMS_TASK.STATUS name and select Static type](images/task-02-step-10-select-static-value.png)

11. Enter the following values, using each status as both the **Display Value** and **Return Value**, then click **Create List of Values**:

    | Display Value | Return Value |
    | ------------- | ------------ |
    | Pending       | Pending      |
    | In Progress   | In Progress  |
    | Done          | Done         |
    | Blocked       | Blocked      |

    ![Enter the task status display and return values](images/task-02-step-11-create-static-lov-wizard-values.png)

12. Create the static LOV named `TMS_TASK.CATEGORY`.

    - Click **Create** to start creating a static LOV.

    ![Create another shared list of values](images/task-02-step-12-create-lov.png)

13. In the **Create List of Values** wizard, select **From Scratch** and click **Next**.

    ![Select From Scratch in the Create List of Values wizard](images/task-02-step-13-create-lov-wizard-name.png)

14. For **Name**, enter `TMS_TASK.CATEGORY`.

    - Select **Static** as the **Type**, then click **Next**.

    ![Enter the TMS_TASK.CATEGORY name and select Static type](images/task-02-step-14-select-static-value.png)

15. Enter the following values, using each category as both the **Display Value** and **Return Value**, then click **Create List of Values**:

    | Display Value | Return Value |
    | ------------- | ------------ |
    | IT Setup      | IT Setup     |
    | HR Documents  | HR Documents |
    | Training      | Training     |
    | Benefits      | Benefits     |
    | Facilities    | Facilities   |

    ![Enter the task category display and return values](images/task-02-step-15-create-static-lov-wizard-values.png)

16. Confirm that all the LOVs appear in the application’s **Lists of Values** page.

    ![All LOVs listed in Shared Components](images/task-02-step-16-lovs.png)

## Acknowledgements

 - **Author -** Aravind Madhavan, Senior Product Manager.
 - **Last Updated By/Date** - Aravind Madhavan, Senior Product Manager, July 2026
