# Lab 1: TAP Requisition Approval Workflow

## Introduction

Create a TAP workflow that routes submitted requisitions by requested headcount. Requisitions for more than three people go to an HR administrator. All other requisitions go to the department head. The task outcome updates the requisition status.

Estimated Workshop Time: 45 minutes

### Objectives

In this lab, you will learn how to:

- Create approval task definitions with task parameters and potential owners.
- Build a workflow that loads requisition details and routes by headcount.
- Start the workflow from the Job Requisition form and test both routes.

## Task 1: Create the task definitions

1. In TAP, open **Shared Components**.
    ![Shared Components page with Task Definitions highlighted](images/task-01-step-01-task-definitions.png)

2. Select **Task Definitions**, then click **Create**.
    ![Task Definitions page with the Create button](images/task-01-step-02-task-definitions-create.png)

3. Create the HR administration task definition with these values:

    **Requisition HR Admin Review**

    | Field | Value |
    | --- | --- |
    | Name | `Requisition HR Admin Review` |
    | Type | Approval Task |
    | Priority | Medium |
    | Subject | `Approve Requisition &P_REQ_ID. for &P_REQUESTED_HEADCOUNT. headcount` |

    ![HR Admin Review task definition details](images/task-01-step-03-task-definitions-details.png)

4. Set **Due On Type** to **Expression** and set **Due On** to `SYSDATE + 2`.
    ![Task deadline expression and Add Participant controls](images/task-01-step-04-task-definitions-expression-add-participants.png)

5. Click **Add Participant** and configure the participant as follows:

    | Participant Type | Identity Type | Value |
    | --- | --- | --- |
    | **Potential Owner** | **Authorization Scheme** |  `IS_TA_ADMIN` |

    ![HR Admin Potential Owner participant configuration](images/task-01-step-05-task-definitions-add-participants-details.png)

6. Add the following task parameters. The static IDs support the subject substitutions.

    - Set each parameter data type to **String** and provide a readable label.

    | **Static ID** | Label | **Required** | **Visible** |
    | --- | --- | --- | --- |
    | `P_REQ_ID` | Requisition ID | Yes | Yes |
    | `P_REQUESTED_HEADCOUNT` | Requested Headcount | Yes | Yes |
    | `P_JOB_TITLE` | Job Title | Yes | Yes |
    | `P_DEPARTMENT_NAME` | Department Name | Yes | Yes |

    ![HR Admin Review task parameter definitions](images/task-01-step-06-task-definitions-add-parameters.png)

7. Keep **Initiator Can Complete** off.

    - Click **Create Task Details Page**, then save the task definition.
    ![HR Admin Review settings with Initiator Can Complete off and Create Task Details Page available](images/task-01-step-07-create-task-definitions-page.png)

8. Create the `Requisition Department Head Review` task definition with these values, then click **Create**:

    | Field | Value |
    | --- | --- |
    | Name | `Requisition Department Head Review` |
    | Type | Approval Task |
    | Priority | Medium |
    | Subject | `Approve Requisition &P_REQ_ID. for &P_REQUESTED_HEADCOUNT. headcount` |

    ![Department Head Review task definition details](images/task-01-step-08-task-definitions-dep-hr-add.png)

9. On the **Task Definition** page, click **Add Participant** and configure the Department Head **Potential Owner** as follows:

    | Participant Type | Identity Type | Value Type | **SQL Query** |
    | --- | --- | --- | --- |
    | **Potential Owner** | User | **SQL Query** | `SELECT e.email FROM tms_employees e JOIN tms_departments d ON d.manager_id = e.employee_id WHERE d.dept_id = (SELECT r.dept_id FROM tms_job_requisitions r WHERE r.req_id = :APEX$TASK_PK)` |

    ![Department Head Potential Owner SQL Query configuration](images/task-01-step-09-task-definitions-dep-add-participants.png)

10. Add the same task parameters to this definition. The static IDs support the subject substitutions.

    - Set each parameter data type to **String** and provide a readable label.

    | **Static ID** | Label | **Required** | **Visible** |
    | --- | --- | --- | --- |
    | `P_REQ_ID` | Requisition ID | Yes | Yes |
    | `P_REQUESTED_HEADCOUNT` | Requested Headcount | Yes | Yes |
    | `P_JOB_TITLE` | Job Title | Yes | Yes |
    | `P_DEPARTMENT_NAME` | Department Name | Yes | Yes |

    ![Department Head Review task parameter definitions](images/task-01-step-10-task-definitions-dep-add-parameters.png)

11. Keep **Initiator Can Complete** off.

    - Click **Create Task Details Page**, then save the task definition.
    ![Department Head Task Details page creation](images/task-01-step-11-task-definitions-dep-create-task-details.png)

## Task 2: Create the requisition workflow

1. In TAP, open **Shared Components** and select **Workflows**.
    ![Shared Components page with Workflows highlighted](images/task-02-step-01-wf-shared.png)

2. Click **Create**.

    - Name the workflow `Approve Job Requisition` and keep its version in **Development**.

3. Create the required workflow parameter:

    | **Static ID** | Label | **Data Type** | **Direction** | **Required** |
    | --- | --- | --- | --- | --- |
    | `P_REQ_ID` | Requisition ID | NUMBER | In | Yes |

    ![Workflow parameter configuration](images/task-02-step-03-wf-parameter.png)

4. Create these workflow variables.

    - Use the `V_` prefix for values that can change at runtime.

    - Add the label shown for each variable.

    | **Static ID** | Label | **Data Type** |
    | --- | --- | --- |
    | `V_REQUESTED_HEADCOUNT` | Requested Headcount | NUMBER |
    | `V_DEPARTMENT_ID` | Department ID | NUMBER |
    | `V_REQUESTER_ID` | Requester ID | VARCHAR2 |
    | `V_JOB_TITLE` | Job Title | VARCHAR2 |
    | `V_DEPARTMENT_NAME` | Department Name | VARCHAR2 |

    ![Workflow variable definitions](images/task-02-step-04-add-wf-variables.png)

5. In **Workflow Designer**, add a **Workflow Start** activity named `Start`.

6. Add an **Execute Code** activity named `Load Requisition Details`.

    - Enter:

        ```sql
        <copy>
        BEGIN
            SELECT r.headcount,
                   r.dept_id,
                   r.requested_by,
                   j.title,
                   d.name
              INTO :V_REQUESTED_HEADCOUNT,
                   :V_DEPARTMENT_ID,
                   :V_REQUESTER_ID,
                   :V_JOB_TITLE,
                   :V_DEPARTMENT_NAME
              FROM tms_job_requisitions r
              JOIN tms_jobs j ON j.job_id = r.job_id
              JOIN tms_departments d ON d.dept_id = r.dept_id
             WHERE r.req_id = :P_REQ_ID;
        END;
        </copy>
        ```
    ![Load Requisition Details Execute Code activity](images/task-02-step-06-add-wf-start-activity.png)

## Task 3: Route and complete the approval

1. Add a **Switch** activity named `Headcount Review Route` after **Load Requisition Details**.

    - Set **Type** to **True False Check** and **Condition Type** to **Rows Returned**.

    - Use this SQL query for the true branch:

        ```sql
        <copy>
        SELECT 1
          FROM tms_job_requisitions
         WHERE req_id = :P_REQ_ID
           AND headcount > 3
        </copy>
        ```
    ![Headcount Review Route switch configuration](images/task-03-step-01-add-if-els-switch.png)

2. On the true route, add a **Human Task - Create** activity named `HR Admin Review`.

    - Select task definition `Requisition HR Admin Review`.

    - Set **Details Primary Key Item** to `P_REQ_ID` and **Outcome** to `TASK_OUTCOME`.

    - Configure these task parameter mappings:

    | Task Parameter | Value Type | Workflow Item |
    | --- | --- | --- |
    | `P_REQ_ID` | Item | `P_REQ_ID` |
    | `P_REQUESTED_HEADCOUNT` | Item | `V_REQUESTED_HEADCOUNT` |
    | `P_JOB_TITLE` | Item | `V_JOB_TITLE` |
    | `P_DEPARTMENT_NAME` | Item | `V_DEPARTMENT_NAME` |

    ![HR Admin Review Human Task activity configuration](images/task-03-step-02-hr-admin-review.png)

3. On the false route, add a **Human Task - Create** activity named `Department Head Review`.

    - Select task definition `Requisition Department Head Review`.

    - Set **Details Primary Key Item** to `P_REQ_ID` and **Outcome** to `TASK_OUTCOME`.

    - Use the same four task parameter mappings.

    ![Department Head Review Human Task activity configuration](images/task-03-step-03-dep-head-review.png)

4. Connect the true route from `Headcount Review Route` to **HR Admin Review**. Label the connection **HR Admin**.
    ![True connection to the HR Admin Review activity](images/task-03-step-04-true-connector-hr-admin.png)

5. Connect the false route from `Headcount Review Route` to **Department Head Review**. Label the connection **Department Head**.
    ![False connection to the Department Head Review activity](images/task-03-step-05-false-connector-dept-head.png)

6. Connect both Human Task activities to an **Execute Code** activity named `Update Requisition Status`.

    - Enter this code:

        ```sql
        <copy>
        BEGIN
            UPDATE tms_job_requisitions
               SET status = CASE UPPER(:TASK_OUTCOME)
                                WHEN 'APPROVED' THEN 'Open'
                                WHEN 'REJECTED' THEN 'Rejected'
                                ELSE status
                            END
             WHERE req_id = :P_REQ_ID;
        END;
        </copy>
        ```

    ![Update Requisition Status Execute Code activity](images/task-03-step-06-update-req-status.png)

7. Create a workflow participant and set the workflow owner to `sofia.garcia@acme.example`.
    ![Workflow participant configuration for the workflow owner](images/task-03-step-07-wf-owner.png)

8. Add a **Workflow End** activity named `End` after **Update Requisition Status**.

    - Set its end state to **Completed**.
    ![Workflow End activity configuration](images/task-03-step-08-wf-end.png)

9. Save and activate the workflow version.
    ![Activated workflow version](images/task-03-step-09-wf-activate.png)

## Task 4: Add task and monitoring pages

1. In **App Builder**, click **Create Page**, then select **Unified Task List**.

    ![Create Page wizard with Unified Task List selected](images/task-04-step-01-create-page.png)

2. Set **Page Name** to `My Approvals` and **Report Context** to **My Tasks**.

    - Enable navigation, then click **Create Page**.

    ![My Approvals Unified Task List page configuration](images/task-04-step-02-unified-task-list.png)

3. Create a second **Unified Task List** page.

    - Set its name to `Tasks Initiated by Me` and **Report Context** to **Initiated by Me**.

    - Enable navigation, then create the page.

    ![Tasks Initiated by Me Unified Task List page configuration](images/task-04-step-03-unified-task-list-initiated-by-me.png)

4. Click **Create Page**, then select **Workflow Console**.

    ![Create Page wizard with Workflow Console selected](images/task-04-step-04-wf-console.png)

5. Set **Name** to `Workflow Console`, **Report Context** to **My Workflows**, and enable **Include Dashboard Page**.

    - Name the generated dashboard `Workflow Dashboard` and the generated details page `Workflow Form`.

    - Enable navigation, then click **Create Page**.

    ![Workflow Console page definition and report context](images/task-04-step-05-wf-console-det.png)

6. In **Page Designer**, apply authorization scheme `IS_TA_ADMIN` to the **Workflow Console** page.

7. In **Page Designer**, open the TAP **Job Requisition Form** and select **Processing**. Create a **Workflow** page process after the **Form DML** process.
    ![Workflow page process created after Form DML](images/task-04-step-07-add-wf-process.png)

8. Set **Workflow** to `Approve Job Requisition` and **Operation** to **Start**.
    ![Approve Job Requisition Workflow process settings](images/task-04-step-08-add-wf-process-02.png)

9. Under **Parameters**, select **Requisition ID** and map it to page item `PXX_REQ_ID`.
    ![Workflow process parameter mapping for requisition ID](images/task-04-step-09-add-param.png)

10. Add a server-side condition to the Workflow process.

    - Run it only when the Create button is pressed and `PXX_STATUS` equals `Open`.

    ![Workflow process server-side condition](images/task-04-step-10-server-side.png)

## Task 5: Test both approval routes

1. Run TAP and submit a requisition with headcount `2`. Sign in as the department head for that department.

    - Open **My Approvals**, then approve the `Requisition Department Head Review` task.

2. Return to the requisition and confirm status `Open`. Sign in as `sofia.garcia@acme.example`, who must be both the workflow owner and an `IS_TA_ADMIN` user.

    - Open **Workflow Console** and confirm that the instance completed.

3. Submit another requisition with headcount `5`. Sign in as a TA administrator who can own the task.

    - Open **My Approvals** and claim `Requisition HR Admin Review` if required. Approve or reject the task.

4. Verify that approval sets `TMS_JOB_REQUISITIONS.STATUS` to `Open`. Verify that rejection sets it to `Rejected`.

    - Confirm that the workflow instance completed in **Workflow Console**.

## Acknowledgements

- **Author -** Aravind Madhavan, Senior Product Manager.
- **Last Updated By/Date** - Aravind Madhavan, Senior Product Manager, August 2026
