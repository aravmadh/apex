# Enable Push Notifications

## Introduction

Enable push notifications for ESS. Let each employee opt in on each device.

Then update the Module 16 overdue-task automation to respect notification topics. Do not trigger a browser permission prompt at login.

Estimated Time: 9 minutes

### Objectives

- Enable APEX push notifications and create a user-controlled opt-in experience.
- Store ESS business-topic preferences with the employee identity.
- Add a preference check before the overdue-task automation sends a push notification.

## Task 1: Enable push notifications and opt-in

1. Obtain a push-notification signing credential from the instance or workspace administrator.

    Keep its private key in the credential store. Never put it in JavaScript, static files, or a page item.

2. In ESS, open **Shared Components**, select **Progressive Web App**, then open **Push Notifications**.

    Set **Enable Push Notifications** to **On**. Select the approved credential and click **Apply Changes**.

3. In **Push Notifications**, click **Add Settings Page**. APEX adds a settings page and navigation entry.

    Run ESS on a test device. Open **User Settings** and explicitly opt in.

4. Do not call a push-subscription API after login. A custom opt-in button can call `apex.pwa.subscribePushNotifications()`.

    Invoke it only from an employee click. Treat a declined permission as a normal choice.

## Task 2: Add employee notification-topic preferences

1. Verify whether `TMS_NOTIFICATION_PREFERENCES` exists. Use an approved equivalent store if one exists.

2. If no equivalent exists, have the schema owner apply this additive table definition:

    ```sql
    <copy>
    -- Create the preference table.
    CREATE TABLE tms_notification_preferences (
        employee_id       NUMBER       NOT NULL,
        notification_type VARCHAR2(30) NOT NULL,
        -- Store the employee choice.
        enabled_yn        VARCHAR2(1)  DEFAULT 'Y' NOT NULL,
        -- Require one row per topic.
        CONSTRAINT tms_notification_preferences_pk
            PRIMARY KEY (employee_id, notification_type),
        -- Link the preference to its employee.
        CONSTRAINT tms_notification_preferences_emp_fk
            FOREIGN KEY (employee_id)
            REFERENCES tms_employees (employee_id),
        -- Restrict the enabled flag.
        CONSTRAINT tms_notification_preferences_enabled_ck
            CHECK (enabled_yn IN ('Y', 'N'))
    )
    -- End of table definition.
    </copy>
    ```

3. Create an ESS **Interactive Grid** page named `Notification Preferences`. Use `TMS_NOTIFICATION_PREFERENCES` as its source.

    Keep `EMPLOYEE_ID` hidden. Let employees edit only these fields:

    | Notification Type. | Label | Default |
    | --- | --- | --- |
    | `TASK_OVERDUE` | Task Overdue Alerts. | On |
    | `LEAVE_STATUS` | Leave Approved or Rejected. | On |
    | `PAYSLIP_AVAILABLE` | Payslip Available. | On |
    | `ONBOARDING_REMINDER` | Onboarding Reminders. | On |

4. Add a **Before Header** process. It creates missing preference rows for the authenticated employee.

    Use the existing email-to-employee mapping. This idempotent operation can run on every page visit:

    ```sql
    <copy>
    -- Start the preference merge.
    MERGE INTO tms_notification_preferences p
    USING (
        SELECT e.employee_id,
               n.notification_type
          FROM tms_employees e
          -- Seed the four preference topics.
          CROSS JOIN (
              SELECT 'TASK_OVERDUE' AS notification_type FROM dual
              UNION ALL SELECT 'LEAVE_STATUS' FROM dual
              -- Continue the preference topics.
              UNION ALL SELECT 'PAYSLIP_AVAILABLE' FROM dual
              UNION ALL SELECT 'ONBOARDING_REMINDER' FROM dual
          ) n
         -- Match the current application user.
         WHERE LOWER(e.email) = LOWER(:APP_USER)
    ) source_rows
       -- Match an existing preference.
       ON (p.employee_id = source_rows.employee_id
       AND p.notification_type = source_rows.notification_type)
    -- Add only missing preferences.
    WHEN NOT MATCHED THEN
        INSERT (employee_id, notification_type, enabled_yn)
        VALUES (source_rows.employee_id, source_rows.notification_type, 'Y')
    -- End of merge.
    </copy>
    ```

5. Filter the grid to the authenticated employee rows. Do not expose an item that chooses another employee ID.

## Task 3: Send an overdue-task notification only when enabled

1. In ESS, open the Module 16 **Onboarding Task Overdue** automation. Keep its email action when email remains required.

2. Add **Execute Code** after the automation identifies the employee and overdue task.

    Adapt `v_employee_email`, `v_task_name`, and `v_due_date` to the variables or action-source columns in your automation.

    ```sql
    <copy>
    -- Start the notification check.
    DECLARE
        l_push_enabled tms_notification_preferences.enabled_yn%TYPE;
    BEGIN
        -- Read the employee preference.
        SELECT p.enabled_yn
          INTO l_push_enabled
          FROM tms_notification_preferences p
        -- Join the employee record.
          JOIN tms_employees e
            ON e.employee_id = p.employee_id
        -- Filter the chosen employee and topic.
         WHERE LOWER(e.email) = LOWER(v_employee_email)
           AND p.notification_type = 'TASK_OVERDUE';

        -- Queue a notification only when enabled.
        IF l_push_enabled = 'Y' THEN
            apex_pwa.send_push_notification(
                -- Set notification payload values.
                p_user_name => v_employee_email,
                p_title     => 'Overdue Task: ' || v_task_name,
                -- Build the notification body.
                p_body      => 'Due date was '
                               || TO_CHAR(v_due_date, 'DD-Mon-YYYY')
                               || '. Please complete it today.');
        END IF;
    EXCEPTION
        WHEN NO_DATA_FOUND THEN
            NULL;
    END;
    -- End of notification code.
    </copy>
    ```

3. Run the automation with an opted-in test employee. Test with `TASK_OVERDUE` enabled and disabled.

    Use `APEX_PUSH_NOTIFICATIONS_QUEUE` to inspect queued notifications when delivery fails.

## Learn More

- [Delivering Push Notifications](https://docs.oracle.com/en/database/oracle/apex/26.1/apxdc/delivering-push-notifications.html).
- [Letting Users Manage Notification Settings](https://docs.oracle.com/en/database/oracle/apex/26.1/apxdc/letting-users-manage-notification-settings.html).
- [Pushing Notification from Business Logic](https://docs.oracle.com/en/database/oracle/apex/26.1/apxdc/pushing-notification-business-logic.html).

## Acknowledgements

- **Author -** Aravind Madhavan, Senior Product Manager.
- **Last Updated By/Date** - Aravind Madhavan, Senior Product Manager, September 2026.
