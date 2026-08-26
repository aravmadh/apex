# Create TAP Email Templates

## Introduction

Create TAP email templates for offer and interview events. The static identifiers let page processes and automations send formatted messages with query values supplied as placeholders.

### Objectives

- Create three TAP email templates.
- Configure identifiers and placeholders for automation use.
- Prepare offer and interview notifications.

Estimated Time: 5 minutes

## Task 1: Create the templates

1. In TAP, select **Shared Components**, then select **Email Templates** and **Create**. Select **From Scratch**.

2. Create the following templates:

    | Name | Static Identifier | Subject | Placeholders |
    | --- | --- | --- | --- |
    | Offer Sent | `OFFER_SENT` | Offer of Employment — #JOB_TITLE# | `#CANDIDATE_NAME#`, `#JOB_TITLE#`, `#OFFERED_SALARY#`, `#START_DATE#`, `#EXPIRY_DATE#` |
    | Offer Accepted | `OFFER_ACCEPTED` | Offer accepted by #CANDIDATE_NAME# | `#CANDIDATE_NAME#`, `#JOB_TITLE#`, `#START_DATE#` |
    | Interview Reminder | `INTERVIEW_REMINDER` | Interview tomorrow: #CANDIDATE_NAME# | `#INTERVIEWER_NAME#`, `#CANDIDATE_NAME#`, `#INTERVIEW_DATE#`, `#STAGE_NAME#` |

3. Add these HTML and plain-text bodies.

    **Offer Sent**

    ```html
    <copy><p>Dear <strong>#CANDIDATE_NAME#</strong>,</p>
<p>We are pleased to offer you the position of <strong>#JOB_TITLE#</strong> with a salary of <strong>#OFFERED_SALARY#</strong>. Your proposed start date is <strong>#START_DATE#</strong>.</p>
<p>Please review and accept the attached offer by <strong>#EXPIRY_DATE#</strong>. We look forward to having you on the team.</p>
<p>Recruiting Team</p></copy>
    ```

    ```text
    <copy>Dear #CANDIDATE_NAME#,

We are pleased to offer you the position of #JOB_TITLE# with a salary of #OFFERED_SALARY#.
Your proposed start date is #START_DATE#.

Please review and accept the attached offer by #EXPIRY_DATE#.

Recruiting Team</copy>
    ```

    **Offer Accepted**

    ```html
    <copy><p><strong>#CANDIDATE_NAME#</strong> has accepted the offer for <strong>#JOB_TITLE#</strong>. Start date: <strong>#START_DATE#</strong>.</p>
<p>Please trigger the onboarding workflow.</p></copy>
    ```

    ```text
    <copy>#CANDIDATE_NAME# has accepted the offer for #JOB_TITLE#.
Start date: #START_DATE#.

Please trigger the onboarding workflow.</copy>
    ```

    **Interview Reminder**

    ```html
    <copy><p>Hello <strong>#INTERVIEWER_NAME#</strong>,</p>
<p>This is a reminder of your <strong>#STAGE_NAME#</strong> interview with <strong>#CANDIDATE_NAME#</strong> scheduled for <strong>#INTERVIEW_DATE#</strong>.</p>
<p>Please review the candidate profile beforehand.</p></copy>
    ```

    ```text
    <copy>Hello #INTERVIEWER_NAME#,

This is a reminder of your #STAGE_NAME# interview with #CANDIDATE_NAME# scheduled for #INTERVIEW_DATE#.

Please review the candidate profile beforehand.</copy>
    ```

4. Select **Create Template** and save each template. Confirm that `OFFER_SENT`, `OFFER_ACCEPTED`, and `INTERVIEW_REMINDER` appear in the Email Templates list.

5. Use `OFFER_SENT` when an offer is generated. Use `OFFER_ACCEPTED` when a candidate accepts an offer. The Interview Reminder automation in Lab 8 uses `INTERVIEW_REMINDER`.

6. In PL/SQL business logic, reference a template by its static identifier, for example `p_template_static_id => 'OFFER_SENT'` in `APEX_MAIL.SEND`.

    > **Screenshot placeholder:** TAP Email Templates list showing the three static identifiers.

## Acknowledgements

- **Author -** Aravind Madhavan, Senior Product Manager.
- **Last Updated By/Date** - Aravind Madhavan, Senior Product Manager, July 2026
