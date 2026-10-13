# Lab 3: Build the Interview Feedback Form

## Introduction

Recruiters and hiring managers need a focused form to capture interview feedback. Create a modal Interview Feedback page, configure its page items, and link it from the Interview Schedule interactive grid. A modal dialog displays above the calling page; when it closes, the calling page becomes active again.

Estimated Workshop Time: 5 minutes

### Objectives

In this lab, you will learn how to:

- Create an Interview Feedback form on `TMS_INTERVIEW_STAGES`.

- Configure candidate, interviewer, rating, and outcome items.

- Open the form from the Interview Schedule interactive grid and refresh the grid after the dialog closes.

## Task 1: Create the Interview Feedback page

1. In TAP **App Builder** home page, click **Create Page**.

    ![Page Designer Create Page](images/task-01-step-01-page-designer-create-page.png)

2. Select **Form**.

    ![Create Page Form](images/task-01-step-02-create-page-form.png)

3. Enter/select the following:

    - Under Page Definition:

        - Name: **Interview Feedback**

        - Page Mode: **Modal Dialog**

    - Data Source > Table/View Name: **TMS\_INTERVIEW\_STAGES**

4. Click **Next**.

    ![Page Name Configuration](images/task-01-step-03-page-name-config.png)

5. For **Primary Key Column 1**, select `STAGE_ID(Number)`, then click **Create Page**.

    ![Interview Feedback form primary key and Create Page action](images/task-01-step-04-pk-create.png)

6. Open the new page in **Page Designer**.

    ![Form Page Created](images/task-01-step-05-form-page-created.png)

## Task 2: Configure the form items

> **Note:** These examples use the `P13_` item prefix. If you created another page number, substitute that page’s item prefix throughout this lab.

1. Select **P13\_CANDIDATE\_ID** page item. In the Property Editor, enter/select the following:

    - Identification > Type: **Display Only**.

    ![Display Only](images/task-02-step-01-display-only.png)

2. Select **P13\_INTERVIEWER\_ID** page item. In the Property Editor, enter/select the following:

    - Identification > Type: **Select List**

    - Under List of Values:

        - Type: **Shared Component**

        - List of Values: **TMS\_INTERVIEWERS.NAME**

    ![Select List](images/task-02-step-02-select-list.png)

3. Select the rating item mapped to the `SCORE` database column.

    - The wizard-generated item is **P13_SCORE**. The example screenshot uses the renamed item **P13\_OVERALL\_SCORE**; use the item mapped to `SCORE` in your form.

4. In the Property Editor, enter/select the following:

    - Identification > Type: **Star Rating**

    - Settings > Number of Stars: **5**

    ![Star Rating](images/task-02-step-03-star-rating.png)

5. Select **P13_OUTCOME** page item. In the Property Editor, enter/select the following:

    - Identification > Type: **Select List**

    - Under **List of Values**:

        - Static Values: Enter the following display and return values:

        | Display  | Return  |
        | -------- | ------- |
        | Proceed  | Proceed |
        | Hold     | Hold    |
        | Reject   | Rejected |

    ![Select List Outcome](images/task-02-step-04-select-list-outcome.png)

6. In the left pane, select the page items for `CREATED_BY`, `CREATED_AT`, `UPDATED_BY`, and `UPDATED_AT`.

    - Hold **Ctrl** on Windows or **Command** on macOS to select multiple items.

    - Right-click the selected items and choose **Delete**. Confirm deletion if prompted.

    ![Removed Audit Items](images/task-02-step-05-removed-audit-items.png)

## Task 3: Link the form from Interview Schedule

1. In **Page Designer**, use **Page Browse** to open **Interview Schedule**.

    - This is page `6` in the course application. If it was resequenced, select the page by name.

    ![Interview Schedule Page](images/task-03-step-01-interview-schedule-page.png)

2. Select the **Interview Schedule** interactive grid region.

3. Navigate to **Attributes** tab, disable **Add Row**.

    ![Disable Add Row](images/task-03-step-02-disable-add-row.png)

4. Right-click the **Breadcrumb** region and select **Create Button**.

    ![Add Button 01](images/task-03-step-03-add-button-01.png)

5. In the Property Editor, enter/select the following:

    - Under Identification:

        - Button Name: **ADD_FEEDBACK**

        - Label: **Add Feedback**

        - Slot: **Next**

    - Appearance > Hot: **Toggle On**

    - Under Behavior:

        - Action: **Redirect to a Page in this Application**

        - Target: Enter the following:

            - Page: **13**

            Click **OK**.

    ![Add Button 02](images/task-03-step-04-add-button-02.png)

    ![Redirect](images/task-03-step-05-redirect.png)

    ![Redirect 02](images/task-03-step-06-redirect-02.png)

6. In the left pane, navigate to Rendering tab.

7. Right-click **Add Feedback** button, and select **Create Dynamic Action**.

8. In the Property Editor, enter/select the following:

    - Identification > Name: **DIALOG_CLOSED**

    - Under When:

        - Event: **Dialog Closed**

        - Selection Type: **Button**

        - Button: **ADD_FEEDBACK**

    ![Add Dynamic Action Dialog Closed](images/task-03-step-07-add-da-dialog-closed.png)

9. Select True action of **Refresh**. In the Property Editor, enter/select the following:

    - Under Affected Elements:

        - Selection Type: **Region**

        - Region: **Interview Stages**

    ![Refresh Region](images/task-03-step-08-refresh-region.png)

10. Click **Save and Run**.

11. Select **Add Feedback**, close the dialog, and confirm that the Interview Schedule interactive grid refreshes.

    ![Open Form](images/task-03-step-09-open-form.png)

## Acknowledgements

- **Author -** Aravind Madhavan, Senior Product Manager

- **Last Updated By/Date** - Aravind Madhavan, Senior Product Manager, July 2026
