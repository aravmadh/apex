# Lab 2: Add PWA Enhancements

## Introduction

Add PWA shortcuts and install screenshots to ESS. Then use declarative APEX actions for sharing and location capture.

Run the location action after authentication. Update only the authenticated employee record.

Estimated Time: 8 minutes

### Objectives

- Add deep-link PWA shortcuts and install screenshots.
- Add a declarative Share action to My Profile.
- Capture a device location only after the employee grants permission.

## Task 1: Add shortcuts and install screenshots

1. In ESS, open **Edit Application Definition**, select **Security**, and open **Session Management**.

    Enable **Rejoin Sessions** only for session types approved by the security owner. PWA shortcuts require Rejoin Sessions.

    Review authorization and session policy before you enable broader rejoin behavior.

2. Open **Progressive Web App**. In **Installability**, click **Add Shortcut** three times.

    Use the actual ESS page URLs. Page numbers and aliases vary by course environment.

    - **My Tasks:** target the My Tasks page.
    - **Apply for Leave:** target the Leave Request page.
    - **My Payslip:** target the My Payslip page.

3. Click **Add Screenshot** twice. Upload approved screenshots with the same aspect ratio:

    - ESS Home showing the KPI cards.
    - My Tasks in the mobile layout.

4. Click **Apply Changes**. On a touch device, long-press the installed ESS icon to inspect shortcuts.

    On a desktop operating system, right-click the icon. The browser and operating system control screenshot and shortcut presentation.

## Task 2: Add a declarative Share action

1. In Page Designer, open **My Profile**. Create a button named `Share Profile`.

    Place it where employees can find it. Do not expose one employee profile to another employee.

2. Create a Dynamic Action for the button **Click** event. Add the native **Share** true action.

    Configure it to share the current page with these values:

    - **Title:** `My Acme Corp Profile`.
    - **Text:** `View my profile on the Acme Corp Employee Portal`.
    - **URL:** Current Page.

3. Run the page in a supported mobile browser. Select **Share Profile** and verify the system sharing choices.

    Keep the native action unless a tested requirement needs custom `navigator.share` code.

## Task 3: Capture location after authentication

1. First, verify whether the location columns exist. Run this query as the ESS parsing schema owner.

    ```sql
    <copy>
    -- Query columns.
    SELECT column_name
      FROM user_tab_columns
    -- Search the TMS employee table.
     WHERE table_name = 'TMS_EMPLOYEES'
       AND column_name IN ('GEO_LAT', 'GEO_LNG', 'GEO_UPDATED_AT')
    ORDER BY column_name;
    -- End of query.
    </copy>
    ```

2. If the query does not return the three columns, ask the schema owner to apply this additive migration once.

    Do not run it when the columns already exist.

    ```sql
    <copy>
    ALTER TABLE tms_employees ADD (
        geo_lat        NUMBER(9,6),
        geo_lng        NUMBER(9,6),
        geo_updated_at TIMESTAMP WITH TIME ZONE
    )
    </copy>
    ```

3. On the first authenticated ESS page, usually ESS Home, create hidden number items `P1_GEO_LAT` and `P1_GEO_LNG`.

    Replace `P1` with the actual page number. Do not request a location on the Login page.

    The browser runs this action after authentication and can ask the employee for permission.

4. Create a **Page Load** Dynamic Action. Add **Get Current Position** as its true action.

    Return latitude and longitude to the two hidden items. Explain that ESS uses the location for the employee-location map.

    Let employees decline without blocking ESS.

5. Add **Execute Server-side Code** after location capture. Submit the two location items and use this code.

    This code identifies the row with `:APP_USER`. It never accepts an employee ID from the browser.

    ```sql
    <copy>
    -- Update the employee row.
    UPDATE tms_employees
       SET geo_lat        = :P1_GEO_LAT,
           geo_lng        = :P1_GEO_LNG,
           geo_updated_at = SYSTIMESTAMP
     WHERE LOWER(email) = LOWER(:APP_USER)
    -- End of update.
    </copy>
    ```

6. Run ESS as an employee with a matching `TMS_EMPLOYEES.EMAIL` value. Grant location permission.

    Verify that only that employee row changes. If the application user name is not the employee email, use a reviewed server-side identity mapping.

## Learn More

- [Configuring Progressive Web App Attributes](https://docs.oracle.com/en/database/oracle/apex/26.1/htmdb/configuring-progressive-web-app-attributes.html).
- [Oracle APEX JavaScript API: apex.pwa](https://docs.oracle.com/en/database/oracle/apex/26.1/aexjs/apex.pwa.html).

## Acknowledgements

- **Author -** Aravind Madhavan, Senior Product Manager.
- **Last Updated By/Date** - Aravind Madhavan, Senior Product Manager, September 2026.
