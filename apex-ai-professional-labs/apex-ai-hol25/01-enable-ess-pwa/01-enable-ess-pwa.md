# Lab 1: Enable PWA on ESS

## Introduction

Enable ESS as an installable Progressive Web App (PWA). Oracle APEX generates the web app manifest and default service worker.

You configure the attributes for the installed experience.

Estimated Time: 5 minutes

### Objectives

- Verify the PWA prerequisites for ESS.
- Enable installability and set the ESS PWA identity and colors.
- Install ESS and verify the standalone experience.

## Task 1: Verify the PWA prerequisites

1. In App Builder, open **ESS** and click **Edit Application Definition**. On **Properties**, set **Friendly URLs** to **On**. Click **Apply Changes**.

2. Confirm that the ESS runtime URL uses HTTPS. Use `localhost` only for local development. APEX does not render PWA features from an unsecured environment.

3. Return to the ESS Application home page. Use either option:

    - **Option one.** Click **Edit Application Definition**, then select the **Progressive Web App** tab.
    - Click **Shared Components** and, under **User Interface**, select **Progressive Web App**.

## Task 2: Configure the installed app

1. In the **General** section, set **Enable Progressive Web App** to **On** and set **Installable** to **On**.

2. In **Installability**, use these values. Keep the existing ESS application name.

    **Short Name** labels the installed app when space is scarce.

    - **Short Name:** `ESS Portal`.
    - **Display:** Standalone.
    - **Background Color:** `#0F6E56`.
    - **Theme Color:** `#10B981`.
    - **App Description:** Employee self-service for Acme Corp employees.

3. Use the PWA icon controls to add approved ESS icons at 192 by 192 and 512 by 512 pixels.

    Do not substitute an unapproved Acme Corp logo or a third-party icon. Course managers supply these assets; this repository does not.

4. Keep **Service Worker Configuration** at the APEX default. APEX generates and manages that service worker.

    This module needs no custom hooks.

5. Click **Apply Changes**.

## Task 3: Install and verify ESS

1. Run ESS in a PWA-capable browser. Select **Install App** or the browser install control. Accept the prompt to install **ESS Portal**.

2. Launch ESS from its installed icon. Browsers that honor **Standalone** open a distinct app window. That window has no normal URL bar or browser controls.

3. If **Install App** does not appear, check HTTPS, Friendly URLs, PWA settings, and browser support. APEX shows it only when the device supports PWA installation.

## Learn More

- [Creating a Progressive Web App (PWA)](https://docs.oracle.com/en/database/oracle/apex/26.1/htmdb/creating-a-progressive-web-app.html).
- [Configuring Progressive Web App Attributes](https://docs.oracle.com/en/database/oracle/apex/26.1/htmdb/configuring-progressive-web-app-attributes.html).

## Acknowledgements

- **Author -** Aravind Madhavan, Senior Product Manager.
- **Last Updated By/Date** - Aravind Madhavan, Senior Product Manager, September 2026.
