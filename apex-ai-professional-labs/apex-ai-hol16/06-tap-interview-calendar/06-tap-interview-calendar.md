# Create the TAP Interview Calendar and Capture Offer Signatures

## Introduction

Create a native Calendar page for interviewers. Retain Signature Capture on the Offer Management Form. The calendar filters interviews by the signed-in interviewer. The form stores a hiring manager signature before offer approval.

### Objectives

- Create the TAP Interview Calendar.
- Link calendar events to the Interview Feedback page.
- Configure the approved Signature Capture plug-in on the Offer Management Form.

Estimated Time: 5 minutes

## Task 1: Create the Interview Calendar

1. In TAP App Builder, select **Create Page**, then select **Component** and **Calendar**. Name the page **Interview Calendar**.

2. Enable navigation. Set **Hiring Process** as the navigation parent and select the `fa-calendar` icon.

3. Select **Local Database** and **SQL Query**. Enter:

    ```sql
    <copy>SELECT i.stage_id AS event_id,
           c.first_name || ' ' || c.last_name ||
               ' - ' || i.stage_name AS event_title,
           i.scheduled_date AS start_date,
           i.scheduled_date + (1 / 24) AS end_date,
           i.candidate_id,
           i.outcome
      FROM tms_interview_stages i
      JOIN tms_candidates c ON c.candidate_id = i.candidate_id
      JOIN tms_employees e ON e.employee_id = i.interviewer_id
     WHERE UPPER(e.email) = UPPER(:APP_USER)</copy>
    ```

4. Map `EVENT_TITLE`, `START_DATE`, and `END_DATE` to the Calendar settings.

5. In the Calendar attributes, configure the **View / Edit Link** as the Event Click link to page `13`, **Interview Feedback**. Set `P13_CANDIDATE_ID` to `&CANDIDATE_ID.`. Set `P13_SCHEDULED_DATE` to `&START_DATE.`.

6. Save and run the page. Confirm that each interviewer sees only their assigned interviews.

    > **Screenshot placeholder:** TAP Interview Calendar with an event link to Interview Feedback.

## Task 2: Configure Signature Capture on the Offer Management Form

1. In TAP, open the **Offer Management Form** in Page Designer. Confirm that the approved **Signature Capture** plug-in is available as a page-item type. If needed, obtain the approved plug-in export from the course materials.

2. In SQL Workshop, add a BLOB column if the offers table does not already have one:

    ```sql
    <copy>ALTER TABLE tms_offers ADD (signature_blob BLOB);</copy>
    ```

3. Create `PXX_HIRING_MANAGER_SIGNATURE` below the `STATUS` item. Set its type to **Signature Capture** and its label to **Hiring Manager Signature**.

4. Configure the plug-in source and storage attributes to store the captured signature in `SIGNATURE_BLOB`.

5. Apply the `IS_HIRING_MANAGER` authorization scheme. Add a server-side condition so the item renders only when the offer status is `Pending Approval`.

6. Save and run the page as a hiring manager. Confirm that the item appears only for pending offers.

    > **Screenshot placeholder:** Signature Capture item on a pending offer in the TAP Offer Management Form.

## Acknowledgements

- **Author -** Aravind Madhavan, Senior Product Manager.
- **Last Updated By/Date** - Aravind Madhavan, Senior Product Manager, July 2026
