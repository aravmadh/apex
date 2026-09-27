# Lab 1: Create Shared Lists of Values

## Introduction

In Oracle APEX, a list of values (LOV) supplies values to LOV-based page items and report columns. Named LOVs in Shared Components are reusable throughout an application. Create TAP LOVs for recruitment forms, then create ESS LOVs for leave and onboarding tasks.

Estimated Workshop Time: 5 minutes

### Objectives

- Create TAP interviewer, interview-stage, and offer-status LOVs.
- Create ESS leave-type, onboarding-task status, and task-category LOVs.
- Verify that each LOV is available when you configure page items.

## Task 1: Create the TAP LOVs

1. In TAP:

    a) Navigate to **Shared Components**.
        ![Shared Components](images/task-01-step-01-shared-components.png)

    b) Under Other Components, select **List of Values**.
        ![List of Values](images/task-01-step-01-list-of-values.png)

    c) Select **Create**.
        ![Create LOV](images/task-01-step-01-create-lov.png)

2. Create a dynamic LOV named `TMS_INTERVIEWERS.NAME`:

    a) In the **Create List of Values** wizard, select **From Scratch** and click **Next**.
        ![Select From Scratch in the Create List of Values wizard](images/task-01-step-02-create-lov-wizard-name.png)

    b) For **Name**, enter `TMS_INTERVIEWERS.NAME`. Select **Dynamic** as the **Type**, then click **Next**.
        ![Enter the LOV name and select Dynamic type](images/task-01-step-02-create-lov-name.png)

    c) Choose **SQL Query** as the source type, then for **Enter a SQL SELECT statement**, copy and paste the SQL query below, which returns a display value (`d`) and a return value (`r`), and click **Next**:
        ```
        <copy>
        SELECT first_name || ' ' || last_name d,
               employee_id r
          FROM tms_employees
         WHERE status = 'Active'
         </copy>
        ```
    ![Enter the SQL query for the LOV source](images/task-01-step-02-create-lov-source.png)

    e) Leave **Column Mappings** as the default and click **Create**.
        ![Confirm the default column mappings and create the LOV](images/task-01-step-02-create-lov-col-mapping.png)

3. Create the static LOV named `TMS_INTERVIEW.STAGES`:

    a) Click **Create** to start creating a static LOV.

    b) In the **Create List of Values** wizard, select **From Scratch** and click **Next**.
        ![Select From Scratch in the Create List of Values wizard](images/task-01-step-02-create-lov-wizard-name.png)

    c) For **Name**, enter `TMS_INTERVIEW.STAGES`. Select **Static** as the **Type**, then click **Next**.
        ![Enter the static LOV name and select Static type](images/task-01-step-03-create-static-lov-wizard.png)

    d) Enter the following values, using each stage as both the **Display Value** and **Return Value**, then click **Create List of Values**:

    | Display Value | Return Value |
    | ------------- | ------------ |
    | Applied       | Applied      |
    | Screening     | Screening    |
    | Interview     | Interview    |
    | Offer         | Offer        |
    | Hired         | Hired        |
    | Rejected      | Rejected     |

    ![Enter static LOV display and return values](images/task-01-step-03-create-static-lov-wizard-name.png)

4. Create the static LOV named `TMS_OFFER.STATUS`:

    a) Click **Create** to start creating a static LOV.

    b) In the **Create List of Values** wizard, select **From Scratch** and click **Next**.
        ![Select From Scratch in the Create List of Values wizard](images/task-01-step-02-create-lov-wizard-name.png)

    c) For **Name**, enter `TMS_OFFER.STATUS`. Select **Static** as the **Type**, then click **Next**.
        ![Enter the status LOV name and select Static type](images/task-01-step-04-static-value-wiz.png)

    d) Enter the following values, using each status as both the **Display Value** and **Return Value**, then click **Create List of Values**:

    | Display Value    | Return Value     |
    | ---------------- | ---------------- |
    | Draft            | Draft            |
    | Pending Approval | Pending Approval |
    | Sent             | Sent             |
    | Accepted         | Accepted         |
    | Rejected         | Rejected         |
    | Withdrawn        | Withdrawn        |

    ![Enter static status LOV display and return values](images/task-01-step-04-static-lov-col-disp-ret.png)

5. Confirm if all the LOV's created appears in the application’s **Lists of Values** page.
    ![LOVs](images/task-01-step-05-lovs.png)


## Task 2: Create the ESS LOVs

1. Return to App Builder and select the **Employee Self Service Portal (ESS)** application:

    a) Navigate to **Shared Components**.
        ![Shared Components](images/task-02-step-01-shared-components.png)

    b) Under Other Components, select **List of Values**.
        ![List of Values](images/task-02-step-01-list-of-values.png)

    c) Select **Create**.
        ![Create LOV](images/task-02-step-01-create-lov.png)

2. Create the dynamic LOV named `TMS_LEAVE.TYPES`:

    a) In the **Create List of Values** wizard, select **From Scratch** and click **Next**.
        ![Select From Scratch in the Create List of Values wizard](images/task-01-step-02-create-lov-wizard-name.png)

    b) For **Name**, enter `TMS_LEAVE.TYPES`. Select **Dynamic** as the **Type**, then click **Next**.
        ![Enter the dynamic LOV name and select Dynamic type](images/task-02-step-02-create-dynamic-lov-wizard-name.png)

    c) Keep **Data Source** set to **Local Database**. Under **Source Type**, select **Table**. For **Table / View Name**, select `TMS_LEAVE_TYPES`, then click **Next**.
        ![Select Local Database, Table, and TMS_LEAVE_TYPES](images/task-02-step-02-create-dynamic-lov-wizard-values.png)

    d) Leave the **Return Column** and **Display Column** mappings as shown, then click **Create**.
        ![Confirm the return and display column mappings and create the LOV](images/task-02-step-02-create-dynamic-lov-col-mapping.png)

3. Create the static LOV named `TMS_TASK.STATUS`:

    a) Click **Create** to start creating a static LOV.

    b) In the **Create List of Values** wizard, select **From Scratch** and click **Next**.
        ![Select From Scratch in the Create List of Values wizard](images/task-01-step-02-create-lov-wizard-name.png)

    c) For **Name**, enter `TMS_TASK.STATUS`. Select **Static** as the **Type**, then click **Next**.
    ![Enter the TMS_TASK.STATUS name and select Static type](images/task-02-step-03c-select-static-value.png)

    d) Enter the following values, using each status as both the **Display Value** and **Return Value**, then click **Create List of Values**:

    | Display Value | Return Value |
    | ------------- | ------------ |
    | Pending       | Pending      |
    | In Progress   | In Progress  |
    | Done          | Done         |
    | Blocked       | Blocked      |

    ![Enter the task status display and return values](images/task-02-step-03-create-static-lov-wizard-values.png)

4. Create the static LOV named `TMS_TASK.CATEGORY`:

    a) Click **Create** to start creating a static LOV.

    b) In the **Create List of Values** wizard, select **From Scratch** and click **Next**.
        ![Select From Scratch in the Create List of Values wizard](images/task-01-step-02-create-lov-wizard-name.png)

    c) For **Name**, enter `TMS_TASK.CATEGORY`. Select **Static** as the **Type**, then click **Next**.
    ![Enter the TMS_TASK.CATEGORY name and select Static type](images/task-02-step-04c-select-static-value.png)


    d) Enter the following values, using each category as both the **Display Value** and **Return Value**, then click **Create List of Values**:

    | Display Value | Return Value |
    | ------------- | ------------ |
    | IT Setup      | IT Setup     |
    | HR Documents  | HR Documents |
    | Training      | Training     |
    | Benefits      | Benefits     |
    | Facilities    | Facilities   |

    ![Enter the task category display and return values](images/task-02-step-04-create-static-lov-wizard-values.png)

5. Confirm that all the LOVs appear in the application’s **Lists of Values** page.
    ![All LOVs listed in Shared Components](images/task-02-step-05-lovs.png)


## Acknowledgements

 - **Author -** Aravind Madhavan, Senior Product Manager.
 - **Last Updated By/Date** - Aravind Madhavan, Senior Product Manager, July 2026
