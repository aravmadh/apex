# Lab 3: Build the Interview Feedback Form

## Introduction

Recruiters and hiring managers need a focused form to capture interview feedback. Create a modal Interview Feedback page, configure its page items, and link it from the Interview Schedule interactive grid. A modal dialog displays above the calling page; when it closes, the calling page becomes active again.

Estimated Workshop Time: 5 minutes

### Objectives

In this lab, you will learn how to:

- Create an Interview Feedback form on `TMS_INTERVIEW_STAGES`.
- Configure candidate, interviewer, rating, and outcome items.
- Open the form from the Interview Schedule interactive grid and refresh the grid after close.

## Task 1: Create the Interview Feedback page

1. In TAP **App Builder**, select **Create Page**.

    ![Page Designer Create Page](images/task-01-step-01-page-designer-create-page.png)

2. Select **Form** as the page type and click **Next**.

    ![Create Page Form](images/task-01-step-02-create-page-form.png)

3. Name the page **Interview Feedback**.

    - Set **Page Mode** to **Modal Dialog**.

    - Use page number `13` if it is available.
    - Set **Table / View Name** to `TMS_INTERVIEW_STAGES`.
    - Click **Next**.

    ![Page Name Configuration](images/task-01-step-03-page-name-config.png)

4. For **Primary Key Column 1**, select `STAGE_ID`, then click **Create Page**.

    ![Interview Feedback form primary key and Create Page action](images/task-01-step-04-pk-create.png)

5. Open the new page in **Page Designer**.

    ![Form Page Created](images/task-01-step-05-form-page-created.png)

## Task 2: Configure the form items

> **Page numbers:** These examples use the `P13_` item prefix. If you created another page number, substitute that page’s item prefix throughout this lab.

1. Select `P13_CANDIDATE_ID`.

    - Set its type to **Display Only**.

    - This non-enterable item displays the value that the **Interview Schedule** page passes in session state.

    ![Display Only](images/task-02-step-01-display-only.png)

2. Select `P13_INTERVIEWER_ID`.

    - Under **Identification**, set **Type** to **Select List**.
    - Under **List of Values**, set **Type** to **Shared Component** and select `TMS_INTERVIEWERS.NAME`.

    ![Select List](images/task-02-step-02-select-list.png)

3. Select the rating item mapped to the `SCORE` database column.

    - The wizard-generated item is `P13_SCORE`. The example screenshot uses the renamed item `P13_OVERALL_SCORE`; use the item mapped to `SCORE` in your form.
    - Under **Identification**, set **Type** to **Star Rating**.
    - Under **Settings**, set **Number of Stars** to `5`.

    ![Star Rating](images/task-02-step-03-star-rating.png)

4. Select `P13_OUTCOME`.

    - Under **Identification**, set **Type** to **Select List**.
    - Under **List of Values**, select **Static Values** and enter the following display and return values:

        | Display  | Return  |
        | -------- | ------- |
        | Proceed  | Proceed |
        | Hold     | Hold    |
        | Reject   | Rejected |

    ![Select List Outcome](images/task-02-step-04-select-list-outcome.png)

5. In the left pane, select the page items for `CREATED_BY`, `CREATED_AT`, `UPDATED_BY`, and `UPDATED_AT`.

    - Hold **Ctrl** on Windows or **Command** on macOS to select multiple items.
    - Right-click the selected items and choose **Delete**. Confirm deletion if prompted.

    ![Removed Audit Items](images/task-02-step-05-removed-audit-items.png)

## Task 3: Link the form from Interview Schedule

1. In **Page Designer**, use **Page Browse** to open **Interview Schedule**.

    - This is page `6` in the course application. If it was resequenced, select the page by name.

    ![Interview Schedule Page](images/task-03-step-01-interview-schedule-page.png)

2. Select the **Interview Schedule** interactive grid region.

    - On the **Attributes** tab, clear **Add Row**.

    ![Disable Add Row](images/task-03-step-02-disable-add-row.png)

3. Right-click the **Breadcrumb** region and select **Create Button**.

    ![Add Button 01](images/task-03-step-03-add-button-01.png)

4. Configure the new button:

    - **Button Name:** `ADD_FEEDBACK`.
    - **Slot:** Next.
    - **Hot:** On.

    ![Add Button 02](images/task-03-step-04-add-button-02.png)

5. Set the button **Action** to **Redirect to a Page in this Application**.

    ![Redirect](images/task-03-step-05-redirect.png)

6. Configure the target as page `13`, **Interview Feedback**.

    - If you used another page number, select that page instead.

    ![Redirect 02](images/task-03-step-06-redirect-02.png)

7. Create a dynamic action for the **Add Feedback** button.

    - Set **Event** to **Dialog Closed** and **Selection Type** to **Button**.

    - This event occurs on the calling page after the modal dialog closes.

    ![Add Dynamic Action Dialog Closed](images/task-03-step-07-add-da-dialog-closed.png)

8. Add a true action of **Refresh**.

    - Set **Selection Type** to **Region** and select the Interview Schedule interactive grid region. The example screenshot names it **Interview Stages**.

    ![Refresh Region](images/task-03-step-08-refresh-region.png)

9. Save and run the **Interview Schedule** page.

    - Select **Add Feedback**, close the dialog, and confirm that the Interview Schedule interactive grid refreshes.

    ![Open Form](images/task-03-step-09-open-form.png)

## Acknowledgements

 - **Author -** Aravind Madhavan, Senior Product Manager.
 - **Last Updated By/Date** - Aravind Madhavan, Senior Product Manager, July 2026
