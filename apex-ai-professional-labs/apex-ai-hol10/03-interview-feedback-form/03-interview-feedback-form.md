# Build the Interview Feedback Form

## Introduction

Recruiters and hiring managers need a focused form to capture interview feedback. Create a modal Interview Feedback page, configure its page items, and link it from the Interview Schedule interactive grid. A modal dialog displays above the calling page; when it closes, the calling page becomes active again.

Estimated Time: 5 minutes

### Objectives

- Create an Interview Feedback form on `TMS_INTERVIEW_STAGES`.
- Configure interviewer, date, rating, outcome, and notes items.
- Open the form from the Interview Schedule interactive grid and refresh the grid after close.

## Task 1: Create the Interview Feedback page

1. In TAP App Builder, select **Create Page** and create a **Form** page.
    ![Page Designer Create Page](images/task-01-step-01-page-designer-create-page.png)
    ![Create Page Form](images/task-01-step-01-create-page-form.png)

2. Name the page **Interview Feedback**. Set **Page Mode** to **Modal Dialog**. Use page number `13` if it is available, and set `TMS_INTERVIEW_STAGES` as the data source.
    ![Page Name Configuration](images/task-01-step-02-page-name-config.png)

3. Open the new page in Page Designer.
    ![Form Page Created](images/task-01-step-03-form-page-created.png)
    

## Task 2: Configure the form items

1. Select `P13_CANDIDATE_ID`. Set its type to **Display Only**. This non-enterable item displays the value that the Interview Schedule page passes in session state.
    ![Display Only](images/task-02-step-01-display-only.png)

2. Select `P13_INTERVIEWER_ID`. Set its type to **Select List** and choose `TMS_INTERVIEWERS.NAME` as the LOV.
    ![Select List](images/task-02-step-02-select-list.png)

3. Select `P13_SCHEDULED_DATE`. Set its type to **Date Picker**. The calendar icon lets users select a date, or they can enter the date directly.
    ![Date Picker](images/task-02-step-03-date-picker.png)

4. Select `P13_OVERALL_SCORE`. Set its type to **Star Rating** and configure a range of 1 to 5 stars. A Star Rating item lets users click a star to set a numeric value.
    ![Star Rating](images/task-02-step-04-star-rating.png)

5. Select `P13_OUTCOME`. Set its type to **Select List** and create static values: `Proceed`, `Hold`, and `Reject`.
    ![Select List Outcome](images/task-02-step-05-select-list-outcome.png)

6. Select `P13_FEEDBACK_NOTES`. Set its type to **Textarea** and set **height** to `5` lines. A Textarea displays a multiple-row text area.
    ![Text Area](images/task-02-step-06-text-area.png)

7. Delete the audit items: `CREATED_BY`, `CREATED_AT`, `UPDATED_BY`, and `UPDATED_AT`.
    ![Removed Audit Items](images/task-02-step-07-removed-audit-items.png)

## Task 3: Link the form from Interview Schedule

1. In Page Designer, use **Page Browse** to open the **Interview Schedule** page.
    ![Interview Schedule Page](images/task-03-step-01-interview-schedule-page.png)

2. Select the Interview Schedule interactive grid region. In its attributes, disable **Add Row**.
    ![Disable Add Row](images/task-03-step-02-disable-add-row.png)

3. Right-click the Breadcrumb region, select **Add Button**, and set the button name to **ADD_FEEDBACK**. Set **Slot** to **Next** and enable **Hot**.
    ![Add Button 01](images/task-03-step-03-add-button-01.png)
    ![Add Button 02](images/task-03-step-03-add-button-02.png)

4. Set the button action to **Redirect to a Page in this Application**. Target Page `13` (**Interview Feedback**).
    ![Redirect](images/task-03-step-04-redirect.png)
    ![Redirect 02](images/task-03-step-04-redirect-02.png)

5. Create a dynamic action for the **Add Feedback** button. Set **Event** to **Dialog Closed** and **Selection Type** to **Button**. This event occurs on the calling page after the modal dialog closes.
    ![Add Dynamic Action Dialog Closed](images/task-03-step-05-add-da-dialog-closed.png)

6. Add a true action of **Refresh**. Set **Selection Type** to **Region** and select the **Interview Stages** region.
    ![Refresh Region](images/task-03-step-06-refresh-region.png)

7. Save and run the Interview Schedule page. Select **Add Feedback**, close the dialog, and confirm that the Interview Stages grid refreshes.
    ![Open Form](images/task-03-step-07-open-form.png)

## Acknowledgements

 - **Author -** Aravind Madhavan, Senior Product Manager.
 - **Last Updated By/Date** - Aravind Madhavan, Senior Product Manager, July 2026
