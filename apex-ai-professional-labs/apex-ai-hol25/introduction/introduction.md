# Building Mobile-Friendly Apps

## Introduction

### Where We Are

ESS now supports employee self-service. Employees complete onboarding tasks, request leave, view payslips, and manage their profile.

This module makes those pages installable and practical on a phone. It does not rebuild the application.

TAP suits recruiters and hiring managers who review CVs, compare candidates, and schedule interviews on larger screens.

ESS suits employees completing short self-service actions away from a desk.

### Prerequisites

- Complete the preceding ESS modules.
- Verify My Tasks, Leave Request, My Profile, and the onboarding dashboard.
- Run ESS from HTTPS with **Friendly URLs** enabled.
- Oracle APEX renders PWA features only from HTTPS or localhost. PWA attributes require Friendly URLs.
- Obtain the approved ESS icons and install screenshots from the course asset owner before publishing an installable app.
- Map the application user name to `TMS_EMPLOYEES.EMAIL` before the location and preference tasks.
- Use a reviewed server-side mapping when the authentication scheme uses another identifier.

### Objectives

- Enable and install ESS as a Progressive Web App.
- Configure PWA shortcuts, screenshots, native sharing, and consent-based device location capture.
- Test APEX persistent authentication separately from PWA installation.
- Let users opt in to push notifications and honor their business-notification preferences.
- Validate the most-used ESS pages at mobile widths and identify mobile interaction patterns worth adopting.

Estimated Workshop Time: 35 minutes

## Learn More

- [Creating a Progressive Web App (PWA)](https://docs.oracle.com/en/database/oracle/apex/26.1/htmdb/creating-a-progressive-web-app.html).
- [Configuring Progressive Web App Attributes](https://docs.oracle.com/en/database/oracle/apex/26.1/htmdb/configuring-progressive-web-app-attributes.html).

## Acknowledgements

- **Author -** Aravind Madhavan, Senior Product Manager.
- **Last Updated By/Date** - Aravind Madhavan, Senior Product Manager, September 2026.
