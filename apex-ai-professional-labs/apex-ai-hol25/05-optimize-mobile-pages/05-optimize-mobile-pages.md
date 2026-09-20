# Optimize Key ESS Pages

## Introduction

PWA installation alone does not create a mobile-friendly layout. Test key employee journeys at a narrow viewport.

Make small page-specific adjustments. Do not create duplicate pages by device.

Estimated Time: 5 minutes

### Objectives

- Stack ESS KPI cards cleanly at phone widths.
- Reduce nonessential My Tasks columns at small widths.
- Validate the calendar and leave-request form on a mobile viewport.

## Task 1: Stack the ESS Home KPI cards

1. In Page Designer, open ESS Home or the Onboarding Dashboard. Apply `kpi-row` to the KPI-card parent layout.

2. Add this CSS to **Inline CSS** or the approved application stylesheet:

    ```css
    @media (max-width: 640px) {
        .kpi-row {
            grid-template-columns: 1fr;
        }
    }
    /* End of CSS. */
    ```

3. Run the page at 640 pixels or narrower. Confirm that each card occupies one row and no value overflows.

    Keep every card label visible. A two-column rule does not meet the single-column requirement.

## Task 2: Simplify My Tasks at a narrow width

1. Open the My Tasks Interactive Grid in Page Designer. Set the region **Static ID** to `my-tasks`.

2. Set `mobile-hide` on nonessential columns, such as due date, category, and assigned-to.

    Keep Task Name and Status visible.

3. Add this CSS to the page or approved application stylesheet:

    ```css
    @media (max-width: 640px) {
        #my-tasks .a-GV-cell.mobile-hide,
        #my-tasks .a-GV-header.mobile-hide {
            display: none;
        }
    }
    /* End of CSS. */
    ```

4. Run My Tasks at a narrow viewport. Confirm that Task Name and Status remain readable.

    Confirm the grid still scrolls. At desktop width, confirm the columns reappear.

## Task 3: Validate calendar and leave-request behavior

1. Open ESS Leave Calendar at phone and tablet widths. Use **Month** for the phone test.

    Verify that Calendar navigation remains usable. If available, test **List** as a compact view.

    Promise a device-specific view switch only after you implement and test it.

2. Open Leave Request at a phone width. Verify the responsive APEX grid and associated labels.

    Verify required fields, error messages, and the Submit button. Avoid horizontal scrolling.

3. Record each remaining layout exception as a page fix. Do not apply global CSS to a one-page problem.

## Learn More

- [Creating a Progressive Web App (PWA)](https://docs.oracle.com/en/database/oracle/apex/26.1/htmdb/creating-a-progressive-web-app.html).

## Acknowledgements

- **Author -** Aravind Madhavan, Senior Product Manager.
- **Last Updated By/Date** - Aravind Madhavan, Senior Product Manager, September 2026.
