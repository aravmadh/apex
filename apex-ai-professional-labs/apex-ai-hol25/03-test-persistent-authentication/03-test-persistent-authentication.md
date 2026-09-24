# Lab 3: Test Persistent Authentication

## Introduction

PWA installation and persistent login are separate capabilities. Instance settings enable APEX persistent authentication.

The authentication scheme uses it when an employee selects **Remember Me**. Test it and retain a firm expiry.

Estimated Time: 4 minutes

### Objectives

- Identify the APEX controls required for persistent authentication.
- Verify the employee experience after an installed-app restart.
- Confirm that expiry still requires re-authentication.

## Task 1: Configure persistent authentication with an administrator

1. Ask an instance administrator to open **Administration Services**, select **Manage Instance**, and open **Security**.

2. Set **Allow Persistent Auth** to **Yes**. Agree the **Persistent Authentication Lifetime Days** value with the security owner.

    This setting permits persistent authentication. It does not make every application login persistent.

3. Review the ESS authentication scheme and Login page. Use its supported **Remember Me** behavior only when the employee selects it.

    For a custom login process, pass the checkbox value to the APEX persistent-authentication option. Never store a password or token in page items, static files, or local storage.

## Task 2: Test the login journey

1. Install ESS as a PWA if you have not already done so. On the ESS Login page, sign in as a test employee and explicitly select **Remember Me**.

2. Close the installed app and reopen it. Confirm that the employee resumes under the configured persistent-authentication policy.

3. Test after the configured persistent-authentication lifetime. In non-production, you can use an approved short test lifetime.

    Confirm that ESS asks the employee to authenticate again.

4. Record the result. If the user remains signed in without selecting **Remember Me**, inspect authentication and session settings.

    Do not call that behavior PWA persistence.

## Learn More

- [Controlling Persistent Authentication](https://docs.oracle.com/en/database/oracle/apex/26.1/aeadm/configuring-service-level-security-settings.html)

## Acknowledgements

- **Author -** Aravind Madhavan, Senior Product Manager.
- **Last Updated By/Date** - Aravind Madhavan, Senior Product Manager, September 2026.
