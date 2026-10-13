# Lab 1: Create Shared Lists of Values

## Introduction

In Oracle APEX, a list of values (LOV) supplies values to LOV-based page items and report columns. Named LOVs in Shared Components are reusable throughout an application. Create TAP LOVs for recruitment forms, then create ESS LOVs for leave and onboarding tasks.

Estimated Workshop Time: 10 minutes

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

3. Click **Create**.

    ![Create LOV](images/task-01-step-03-create-lov.png)

4. In the **Create List of Values** wizard, select **From Scratch** and click **Next**.

    ![Select From Scratch in the Create List of Values wizard](images/task-01-step-04-create-lov-wizard-name.png)

5. Under **Name and Type**, enter/select the following:

    - Name: **TMS\_INTERVIEWERS.NAME**

    - Type: **Dynamic**

6. Click **Next**.

    ![Enter the LOV name and select Dynamic type](images/task-01-step-05-create-lov-name.png)

7. Under **List of Values Source**, enter/select the following:

    - Source Type: **SQL Query**

    - Enter a SQL SELECT Statement: Copy and paste the following query.

    It returns the display value as `d` and the return value as `r`.

    ```
    <copy>
    SELECT first_name || ' ' || last_name d,
        employee_id r
    FROM tms_employees
    WHERE status = 'Active'
     </copy>
    ```

8. Click **Next**.

    ![Enter the SQL query for the LOV source](images/task-01-step-06-create-lov-source.png)

9. Leave **Column Mappings** as the default and click **Create**.

    ![Confirm the default column mappings and create the LOV](images/task-01-step-07-create-lov-col-mapping.png)

10. Click **Create** to start creating a static LOV.

    ![Create another shared list of values](images/task-01-step-08-create-lov.png)

11. In the **Create List of Values** wizard, select **From Scratch** and click **Next**.

    ![Select From Scratch in the Create List of Values wizard](images/task-01-step-09-create-lov-wizard-name.png)

12. Under **Name and Type**, enter/select the following:

    - Name: **TMS\_INTERVIEWERS.STAGES**

    - Type: **Static**

13. Click **Next**.

    ![Enter the static LOV name and select Static type](images/task-01-step-10-create-static-lov-wizard.png)

14. Enter the following values, using each stage as both the **Display Value** and **Return Value**.

    | Display Value | Return Value |
    | ------------- | ------------ |
    | Applied       | Applied      |
    | Screening     | Screening    |
    | Interview     | Interview    |
    | Offer         | Offer        |
    | Hired         | Hired        |
    | Rejected      | Rejected     |

15. Click **Create List of Values**

    ![Enter static LOV display and return values](images/task-01-step-11-create-static-lov-wizard-name.png)

16. Click **Create** to start creating a static LOV.

    ![Create another shared list of values](images/task-01-step-12-create-lov.png)

17. In the **Create List of Values** wizard, select **From Scratch** and click **Next**.

    ![Select From Scratch in the Create List of Values wizard](images/task-01-step-13-create-lov-wizard-name.png)

18. Under **Name and Type**, enter/select the following:

    - Name: **TMS\_OFFERS.STATUS**

    - Type: **Static**

19. Click **Next**.

    ![Enter the status LOV name and select Static type](images/task-01-step-14-static-value-wiz.png)

20. Enter the following values, using each status as both the **Display Value** and **Return Value**.

    | Display Value    | Return Value     |
    | ---------------- | ---------------- |
    | Draft            | Draft            |
    | Pending Approval | Pending Approval |
    | Sent             | Sent             |
    | Accepted         | Accepted         |
    | Rejected         | Rejected         |
    | Withdrawn        | Withdrawn        |

21. Click **Create List of Values**

    ![Enter static status LOV display and return values](images/task-01-step-15-static-lov-col-disp-ret.png)

22. Confirm that all three LOVs appear in the application’s **Lists of Values** page.

    ![LOVs](images/task-01-step-16-lovs.png)

## Task 2: Create the ESS LOVs

1. Return to **App Builder** and select **Employee Self-Service Portal (ESS)**.

2. Navigate to **Shared Components**.

    ![Shared Components](images/task-02-step-01-shared-components.png)

3. Under **Other Components**, select **List of Values**.

    ![List of Values](images/task-02-step-02-list-of-values.png)

4. Click **Create**.

    ![Create LOV](images/task-02-step-03-create-lov.png)

5. In the **Create List of Values** wizard, select **From Scratch** and click **Next**.

    ![Select From Scratch in the Create List of Values wizard](images/task-02-step-04-create-lov-wizard-name.png)

6. Under **Name and Type**, enter/select the following:

    - Name: **TMS\_LEAVE.TYPES**

    - Type: **Dynamic**

7. Click **Next**.

    ![Enter the dynamic LOV name and select Dynamic type](images/task-02-step-05-create-dynamic-lov-wizard-name.png)

8. Under **List of Values Source**, enter/select the following:

    - Source Type: **Table**

    - Table / View Name: **TMS\_LEAVE\_TYPES**

    ![Select Local Database, Table, and TMS_LEAVE_TYPES](images/task-02-step-06-create-dynamic-lov-wizard-values.png)

9. Under **Column Mappings**, enter/select the following:

    - Return Column: **LEAVE\_TYPE\_ID**

    - Display Column: **NAME**

10. Click **Create**.

    ![Confirm the return and display column mappings and create the LOV](images/task-02-step-07-create-dynamic-lov-col-mapping.png)

11. Click **Create** to start creating a static LOV.

    ![Create another shared list of values](images/task-02-step-08-create-lov.png)

12. In the **Create List of Values** wizard, select **From Scratch** and click **Next**.

    ![Select From Scratch in the Create List of Values wizard](images/task-02-step-09-create-lov-wizard-name.png)

13. Under **Name and Type**, enter/select the following:

    - Name: **TMS\_TASK.STATUS**

    - Type: **Static**

14. Click **Next**.

    ![Enter the TMS_TASK.STATUS name and select Static type](images/task-02-step-10-select-static-value.png)

15. Enter the following values, using each status as both the **Display Value** and **Return Value**:

    | Display Value | Return Value |
    | ------------- | ------------ |
    | Pending       | Pending      |
    | In Progress   | In Progress  |
    | Done          | Done         |
    | Blocked       | Blocked      |

16. Click **Create List of Values**.

    ![Enter the task status display and return values](images/task-02-step-11-create-static-lov-wizard-values.png)

17. Click **Create** to start creating a static LOV.

    ![Create another shared list of values](images/task-02-step-12-create-lov.png)

18. In the **Create List of Values** wizard, select **From Scratch** and click **Next**.

    ![Select From Scratch in the Create List of Values wizard](images/task-02-step-13-create-lov-wizard-name.png)

19. Under **Name and Type**, enter/select the following:

    - Name: **TMS\_TASK.CATEGORY**

    - Type: **Static**

20. Click **Next**.

    ![Enter the TMS_TASK.CATEGORY name and select Static type](images/task-02-step-14-select-static-value.png)

21. Enter the following values, using each category as both the **Display Value** and **Return Value**:

    | Display Value | Return Value |
    | ------------- | ------------ |
    | IT Setup      | IT Setup     |
    | HR Documents  | HR Documents |
    | Training      | Training     |
    | Benefits      | Benefits     |
    | Facilities    | Facilities   |

22. Click **Create List of Values**.

    ![Enter the task category display and return values](images/task-02-step-15-create-static-lov-wizard-values.png)

23. Confirm that all the LOVs appear in the application’s **Lists of Values** page.

    ![All LOVs listed in Shared Components](images/task-02-step-16-lovs.png)

## Acknowledgements

- **Author -** Aravind Madhavan, Senior Product Manager.

- **Last Updated By/Date** - Aravind Madhavan, Senior Product Manager, July 2026
