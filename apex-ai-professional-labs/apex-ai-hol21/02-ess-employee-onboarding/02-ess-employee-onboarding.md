# ESS Employee Onboarding Workflow

## Introduction

Create an ESS onboarding workflow that assigns HR document review and department orientation at the same time. APEX waits for both human tasks before it sends the new employee a welcome email.

Estimated Time: 45 minutes

### Objectives

- Create action task definitions for HR and the department manager.
- Create a parallel APEX workflow that loads employee data and waits for both tasks.
- Start, complete, and monitor an onboarding instance.

## Task 1: Create the Task definition

1. In ESS, open **Shared Components**, select **Task Definitions**, and click **Create**. Create the HR documents task definition with these values:
     
     **New Employee HR Documents**
    | Field | Value |
    | --- | --- |
    | Name | `New Employee HR Documents` |
    | Type | Action Task |
    | Priority | Medium |
    | Subject | `Complete HR documents for &P_EMPLOYEE_NAME.` |
    
    ![](images/task-01-step-01-task-def-hr-doc.png)

- Click **Create**. On the Task Definition page, click **Add Participant** and configure the HR documents Potential Owner as follows:

    | Participant Type | Identity Type | Value Type | Authorization Scheme |
    | --- | --- | --- | --- |
    | Potential Owner | Authorization Scheme |  | `IS_HR_ADMIN` |

    ![](images/task-01-step-01-task-def-hr-doc-add-participant.png)

- Add these String task parameters. Set each parameter to **Required** and **Visible**.

    | Static ID | Label | Required | Visible |
    | --- | --- | --- | --- |
    | `EMPLOYEE_ID` | Employee ID | Yes | Yes |
    | `EMPLOYEE_NAME` | Employee Name | Yes | Yes |
    | `START_DATE` | Start Date | Yes | Yes |

    ![](images/task-01-step-01-task-def-hr-doc-parameters.png)

- Click **Create Task Details Page** button. This will create associated Task Details Form Page and save the Task definition

    ![](images/task-01-step-01-task-def-hr-doc-create-page.png)

    **New Employee Department Orientation**

    | Field | Value |
    | --- | --- |
    | Name | `New Employee Department Orientation` |
    | Type | Action Task |
    | Priority | Medium |
    | Subject | `Schedule department orientation for &P_EMPLOYEE_NAME.` |

    ![](images/task-01-step-01-task-def-hr-dept.png)

- Click **Create**. On the Task Definition page, click **Add Participant** and configure the Department Orientation Potential Owner as follows:

    | Participant Type | Identity Type | Value Type | Task Parameter |
    | --- | --- | --- | --- |
    | Potential Owner | User | Static | `&DEPARTMENT_MANAGER.` |

    ![](images/task-01-step-01-task-def-hr-dept-add-participants.png)

- Add these String task parameters. Set each parameter to **Required** and **Visible**.

    | Static ID | Label | Required | Visible |
    | --- | --- | --- | --- |
    | `EMPLOYEE_ID` | Employee ID | Yes | Yes |
    | `EMPLOYEE_NAME` | Employee Name | Yes | Yes |
    | `START_DATE` | Start Date | Yes | Yes |
    | `DEPARTMENT_MANAGER` | Department Manager | Yes | Yes |

    ![](images/task-01-step-01-task-def-hr-dept-add-parameters.png)

- Click **Create Task Details Page** button. This will create associated Task Details Form Page and save the Task definition
    ![](images/task-01-step-01-task-def-hr-dept-create-page.png)


## Task 2: Create the Onboard New Employee workflow

1. In ESS Shared Components, create a workflow named `Onboard New Employee` with static ID `onboard_new_employee`. Keep the workflow version in **Development**.
![](images/task-02-step-01-shared-component.png)

2. Create the required workflow parameter with **Direction** set to **In**.

    | Static ID | Label | Data Type | Required |
    | --- | --- | --- | --- |
    | `V_EMPLOYEE_ID` | Employee ID | NUMBER | Yes |

    ![](images/task-02-step-02-wf-parameter.png)

3. Create these version variables. The `V_` prefix identifies values that the workflow calculates or changes during runtime.

    | Static ID | Label | Data Type |
    | --- | --- | --- |
    | `V_EMPLOYEE_NAME` | Employee Name | VARCHAR2 |
    | `V_EMPLOYEE_EMAIL` | Employee Email | VARCHAR2 |
    | `V_START_DATE` | Start Date | TIMESTAMP |
    | `V_DEPARTMENT_NAME` | Department Name | VARCHAR2 |
    | `V_DEPARTMENT_MANAGER` | Department Manager | VARCHAR2 |

    ![](images/task-02-step-03-create-variables.png)

4. Add a **Workflow Start** activity named `Start`. After it, add an **Execute Code** activity named `Load Employee Details` with this code:

    ```sql
    <copy>
    BEGIN
        SELECT e.first_name || ' ' || e.last_name,
               e.email,
               e.hire_date,
               d.name,
               mgr.email
          INTO :V_EMPLOYEE_NAME,
               :V_EMPLOYEE_EMAIL,
               :V_START_DATE,
               :V_DEPARTMENT_NAME,
               :V_DEPARTMENT_MANAGER
          FROM tms_employees e
          LEFT JOIN tms_departments d ON d.dept_id = e.dept_id
          LEFT JOIN tms_employees mgr ON mgr.employee_id = d.manager_id
         WHERE e.employee_id = :P_EMPLOYEE_ID;
    END;
    </copy>
    ```

    ![](images/task-02-step-04-load-emp-det.png)

5. Add a **Parallel Flow** after the code activity and name it `Complete Onboarding Setup`. Name its two branches `HR Documents` and `Department Orientation`. APEX creates the join behavior; do not add a separate Parallel Join.

## Task 3: Configure the parallel activities and email

1. In the `Complete HR Documents` branch, add a **Human Task - Create** activity named `HR Documents` and select the matching task definition. Configure the parameter mappings:

    | Parameter | Value Type | Variable |
    | --- | --- | --- |
    | `EMPLOYEE_ID` | Item | `V_EMPLOYEE_ID` |
    | `EMPLOYEE_NAME` | Item | `V_EMPLOYEE_NAME` |
    | `START_DATE` | Item | `V_START_DATE` |

    ![](images/task-03-step-01-hr-docs.png)

2. In the `Schedule Department Orientation` branch, add a **Human Task - Create** activity named `Schedule New Employee Department Orientation` and select the matching task definition. Configure the parameter mappings:

    | Parameter | Value Type | Variable |
    | --- | --- | --- |
    | `EMPLOYEE_ID` | Item | `P_EMPLOYEE_ID` |
    | `EMPLOYEE_NAME` | Item | `V_EMPLOYEE_NAME` |
    | `START_DATE` | Item | `V_START_DATE` |
    | `DEPARTMENT_MANAGER` | Item | `V_DEPARTMENT_MANAGER` |

    ![](images/task-03-step-01-dept-orientation.png)

3. Collapse the Parallel Flow. Add a **Send Email** activity named `Send Welcome Email` after it. Select the existing welcome email template, set recipient to `V_EMPLOYEE_EMAIL`, and map these placeholders:

    | Placeholder | Value |
    | --- | --- |
    | `EMPLOYEE_NAME` | `&V_EMPLOYEE_NAME.` |
    | `START_DATE` | `&V_START_DATE.` |
    | `DEPT_NAME` | `&V_DEPARTMENT.` |
    | `MANAGER_NAME`| `&V_DEPARTMENT_MANAGER.` |

    ![](images/task-03-step-03-send-email.png)

4. Add a workflow participant.
![](images/task-03-step-04-wf-owner.png)

5. Add a **Workflow End** activity named `End` after the email and set its end state to **Completed**. Save and activate the workflow.
![](images/task-03-step-05-activate-wf.png)

## Task 4: Add the inbox, console, and start page

1. Create an ESS **Unified Task List** page named `My Workflow Tasks`. Set **Report Context** to **My Tasks** and add it to ESS navigation.
![](images/task-04-step-01-unified-task-list.png)

2. Create a **Workflow Console** page for workflow instances initiated by the signed-in user.
![](images/task-04-step-02-worflow-console.png)

3. Create a blank ESS page named `Start Employee Onboarding` and protect it with `IS_HR_ADMIN`. Add select-list item `PXX_EMPLOYEE_ID`, where `XX` is your page number. Use this LOV:

    ```sql
    <copy>
    SELECT first_name || ' ' || last_name AS display_value,
           employee_id AS return_value
      FROM tms_employees
     WHERE status = 'Active'
     ORDER BY first_name, last_name
    </copy>
    ```
    ![](images/task-04-step-03-blank-page.png)
    ![](images/task-04-step-03-add-auth.png)
    ![](images/task-04-step-04-select-list.png)


4. Add button `START_ONBOARDING` with label **Start Onboarding** and  Add a process that starts workflow `Onboard New Employee` and maps `P_EMPLOYEE_ID` to `PXX_EMPLOYEE_ID`.
    ![](images/task-04-step-04-add-button.png)
    ![](images/task-04-step-04-add-process.png)
    ![](images/task-04-step-04-add-process-parameters.png)


## Task 5: Test parallel completion

1. Run ESS as an HR administrator. Open **Start Employee Onboarding**, select an employee, and click **Start Onboarding**.

2. Open **My Workflow Tasks** and confirm that `Complete New Employee HR Documents` and `Schedule New Employee Department Orientation` were both created. Complete only **Complete New Employee HR Documents**. Confirm the workflow remains active and the welcome email has not been sent.

3. Sign in as the selected employee's department manager. Open **My Workflow Tasks** and complete **Schedule New Employee Department Orientation**.

4. Confirm the Parallel Flow completes, then **Send Welcome Email** runs and the workflow reaches **Completed** in the Workflow Console.

## Acknowledgements

- **Author -** Aravind Madhavan, Senior Product Manager.
- **Last Updated By/Date** - Aravind Madhavan, Senior Product Manager, August 2026
