# Lab 3: Create ESS Email Templates

## Introduction

Create email templates for leave, onboarding, and probation events. Each template has a static identifier. Automations populate its placeholders from a query row.

Estimated Workshop Time: 10 minutes

### Objectives

In this lab, you will learn how to:

- Verify instance email prerequisites.
- Create five ESS email templates.
- Use stable static identifiers and placeholders.

## Task 1: Set up instance email

1. Ask your instance administrator to configure the SMTP host and port in **Administration Services**. Ask them to set a default From address, such as `noreply@acmecorp.com`.

2. Follow the [Oracle email setup guidance](https://docs.oracle.com/en/database/oracle/apex/26.1/aeadm/configuring-email.html) for your instance.

3. If you use Oracle APEX Service (`https://www.oracleapex.com`), ignore this task. The service manages instance email.

4. Confirm that the application can send a test email before you continue.

## Task 2: Create the ESS templates

> **Screenshot guidance:** The template screenshots show **Leave Approved**. Use the same fields and creation process for the other templates, with the values supplied in their respective steps.

1. In ESS, open **Shared Components**.

    ![Task 2: Shared components](images/task-02-step-01-shared-components.png)

2. Select **Email Templates**.

    ![Task 2: Email template](images/task-02-step-02-email-template.png)

3. Select **Create Email Template**.

    ![Task 2: Create email template](images/task-02-step-03-create-email-template-03.png)

4. Create the **Leave Approved** email template using these values:

    - **Name:** Leave Approved
    - **Static ID:** `leave-approved` (Auto-Generated)
    - **Subject:** HR approved your leave request

    ![Task 2: Email template details](images/task-02-step-04-enter-details.png)

5. Enter the HTML and plain-text content for **Leave Approved**:

    - **HTML Format > Body:**

        ```html
        <copy><p>Hello <strong>#EMPLOYEE_NAME#</strong>,</p>

        <p>HR approved your <strong>#LEAVE_TYPE#</strong> leave from <strong>#START_DATE#</strong> to <strong>#END_DATE#</strong>.</p>

        <p>Please enjoy your time off and remember to hand over any critical tasks in advance.</p>

        <p>HR Team</p></copy>
        ```

    - **Plain-Text Format > Content:**

        ```text
        <copy>Hello #EMPLOYEE_NAME#,

        HR approved your #LEAVE_TYPE# leave from #START_DATE# to #END_DATE#.

        Please enjoy your time off and remember to hand over any critical tasks in advance.

        HR Team</copy>
        ```

    - Select **Create Email Template** to save this template before starting the next one.

    ![Task 2: Email template content](images/task-02-step-05-enter-details-02.png)

6. Create the **Leave Rejected** email template using these values:

    - **Name:** Leave Rejected
    - **Static ID:** `leave-rejected` (Auto-Generated)
    - **Subject:** HR could not approve your leave request

    - **HTML Format > Body:**

        ```html
        <copy><p>Hello <strong>#EMPLOYEE_NAME#</strong>,</p>

        <p>HR could not approve your recent leave request.</p>

        <p><em>Reason:</em> #REJECTION_REASON#</p>

        <p>If you believe this is an error or have questions, please contact HR.</p></copy>
        ```

    - **Plain-Text Format > Content:**

        ```text
        <copy>Hello #EMPLOYEE_NAME#,

        HR could not approve your recent leave request.

        Reason: #REJECTION_REASON#

        If you have questions, please contact HR.</copy>
        ```

    - Select **Create Email Template** to save this template before starting the next one.

7. Create the **Task Overdue Notification** email template using these values:

    - **Name:** Task Overdue Notification
    - **Static ID:** `task-overdue-notification` (Auto-Generated)
    - **Subject:** Action needed: Overdue onboarding task

    - **HTML Format > Body:**

        ```html
        <copy><p>Hello <strong>#EMPLOYEE_NAME#</strong>,</p>

        <p>Your onboarding task <strong>"#TASK_NAME#"</strong> was due on <strong>#DUE_DATE#</strong> and is now <span style="color:#ef4444;">overdue</span>.</p>

        <p>Please complete it as soon as possible or reach out to your manager if you need help.</p></copy>
        ```

    - **Plain-Text Format > Content:**

        ```text
        <copy>Hello #EMPLOYEE_NAME#,

        Your onboarding task "#TASK_NAME#" was due on #DUE_DATE# and is now OVERDUE.

        Please complete it as soon as possible or reach out to your manager if you need help.</copy>
        ```

    - Select **Create Email Template** to save this template before starting the next one.

8. Create the **Welcome to Acme Corp** email template using these values:

    - **Name:** Welcome to Acme Corp
    - **Static ID:** `welcome-to-acme-corp` (Auto-Generated)
    - **Subject:** Welcome to Acme Corp, #EMPLOYEE_NAME#!

    - **HTML Format > Body:**

        ```html
        <copy><h2 style="margin-bottom:0">Welcome to Acme Corp, #EMPLOYEE_NAME#!</h2>

        <p>We look forward to your first day on <strong>#START_DATE#</strong>.</p>

        <ul>
                <li><strong>Department:</strong> #DEPT_NAME#</li>
                <li><strong>Manager:</strong> #MANAGER_NAME#</li>
        </ul>

        <p>Please sign in to the Employee Self-Service portal to view your onboarding tasks and upload any required documents.</p>

        <p>If you have questions before you start, just reply to this email.</p>

        <p>HR Team</p></copy>
        ```

    - **Plain-Text Format > Content:**

        ```text
        <copy>WELCOME TO ACME CORP, #EMPLOYEE_NAME#!

        We look forward to your first day on #START_DATE#.

        Department: #DEPT_NAME#
        Manager: #MANAGER_NAME#

        Sign in to the ESS portal to view your onboarding tasks and upload documents.

        If you have questions before you start, just reply to this email.

        HR Team</copy>
        ```

    - Select **Create Email Template** to save this template before starting the next one.

9. Create the **Probation Alert** email template using these values:

    - **Name:** Probation Alert
    - **Static ID:** `probation-alert` (Auto-Generated)
    - **Subject:** Probation ends soon for #EMPLOYEE_NAME#

    - **HTML Format > Body:**

        ```html
        <copy><p><strong>Probation Review Reminder</strong></p>

        <p>Employee: <strong>#EMPLOYEE_NAME#</strong><br>
        Hire Date: <strong>#HIRE_DATE#</strong><br>
        Probation End Date: <strong>#PROBATION_END#</strong></p>

        <p>The probation period ends in <strong>7 days</strong>. Schedule the confirmation meeting and update the system with the outcome.</p>

        <p>HR Team</p></copy>
        ```

    - **Plain-Text Format > Content:**

        ```text
        <copy>PROBATION REVIEW REMINDER

        Employee: #EMPLOYEE_NAME#
        Hire Date: #HIRE_DATE#
        Probation End Date: #PROBATION_END#

        The probation period ends in 7 days.
        Please schedule the confirmation meeting and update the system.

        HR Team</copy>
        ```

    - Select **Create Email Template** to save this template before starting the next one.

10. Return to **Email Templates** and confirm that all five static identifiers appear.
    ![Task 2: Verify email templates](images/task-02-step-10-check-templates-exists.png)

11. The static identifier is what you reference with `p_template_static_id` in `APEX_MAIL.SEND` or select in a native **Send E-Mail** action.
    ![Task 2: Sample API usage](images/task-02-step-11-sample-api-usage.png)

## Acknowledgements

- **Author -** Aravind Madhavan, Senior Product Manager.
- **Last Updated By/Date** - Aravind Madhavan, Senior Product Manager, July 2026
