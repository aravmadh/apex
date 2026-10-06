# Lab 4: Optional Application Updates

## Introduction

You have added bulk validations and improved button prominence in TAP and ESS. You can also try the prompts below to explore other changes. Use one prompt at a time, review the edited source, and import only the affected files.

Estimated Workshop Time: 15 minutes (choose an experiment; outside the core workshop)

### Objectives

In this lab, you will learn how to:

- Explore more validations and page improvements with short prompts.
- Review the scope and behavior of each change.
- Try single-file and multiple-file imports with SQLcl as an alternative.

### Prerequisites

Selected file imports in VS Code and SQLcl require **APEX 26.2 or later**. On APEX 26.1, try the editing prompts and use full application import from Module 23 instead.

## Task 1: Try More Prompts in TAP

1. Open the TAP project in **Codex**. Choose one prompt below.

    **Explore the application:**

    ```text
    <copy>
    Show me the forms in TAP and the validations they already have. Do not edit yet.
    </copy>
    ```

    **Add another validation:**

    ```text
    <copy>
    Check email addresses on Candidate forms and reject names that contain only spaces.
    </copy>
    ```

    **Standardize button labels:**

    ```text
    <copy>
    Make the labels for common create, save, cancel, and delete actions consistent across TAP.
    </copy>
    ```

    **Improve field help:**

    ```text
    <copy>
    Add useful field help to the Candidate, Job Requisition, and Offer forms.
    </copy>
    ```

2. Review the changed files. Preserve existing business rules, component identifiers, and button actions.

    - If **Codex** adds a shared dependency, import that dependency before the page.

3. Import the affected files through **Import File** and test the result. Extend this pattern to other page-level improvements in TAP.

## Task 2: Try Selected File Imports for TAP with SQLcl

1. Use a SQLcl version that supports APEXlang selected file import. Connect to the parsing schema for the development workspace using the setup from Module 23.

    - Confirm the TAP target in `deployments/default.json`.

2. At the SQLcl prompt, change to the TAP project directory. Replace the sample path with your project path.

    ```text
    cd /path/to/tap-project
    ```

3. Validate the project and resolve reported errors before import.

    ```text
    apex validate -input .
    ```

4. Import one changed page. Replace the illustrative filename with your actual changed page filename.

    ```text
    apex import -files pages/p00010-candidate-form.apx
    ```

5. To import several changed files in one command, list their paths after `-files`, separated by spaces:

    ```text
    apex import -files pages/p00010-candidate-form.apx pages/p00011-requisition-form.apx pages/p00012-offer-form.apx
    ```

6. Wait for success and repeat the relevant runtime checks. Paths after `-files` are relative to the current SQLcl directory. Reimporting these pages is optional. The VS Code tasks in Lab 1 complete the TAP exercise.

## Task 3: Try More Prompts in ESS

1. Open the ESS project in **Codex**. Choose one prompt below.

    **Explore the application:**

    ```text
    <copy>
    Show me the forms in ESS and the validations they already have. Do not edit yet.
    </copy>
    ```

    **Add another validation:**

    ```text
    <copy>
    Reject names and leave reasons that contain only spaces, and check email addresses on editable profile forms.
    </copy>
    ```

    **Improve validation messages:**

    ```text
    <copy>
    Make the validation messages on My Profile, Leave Request, and Onboarding Task clearer and easier to act on.
    </copy>
    ```

    **Standardize button labels:**

    ```text
    <copy>
    Make the labels for common create, save, cancel, and delete actions consistent across ESS.
    </copy>
    ```

2. Review the edits and preserve existing leave rules, onboarding logic, and authorization checks.

    - If a prompt affects an interactive grid, review its row validations separately from page-item validations.

3. Import the affected files through **Import File** and test the result.

    - Use this workflow for other page-level improvements in ESS as well.

## Task 4: Try Selected File Imports for ESS with SQLcl

1. Use a SQLcl version with APEXlang selected file import support. Connect to the same parsing schema for the development workspace and confirm the ESS target in `deployments/default.json`.

2. At the SQLcl prompt, change to the ESS project directory. Replace the sample path with your project path.

    ```text
    cd /path/to/ess-project
    ```

3. Validate the project and resolve reported errors before import.

    ```text
    apex validate -input .
    ```

4. Import one changed page. Replace the illustrative filename with your actual changed page filename.

    ```text
    apex import -files pages/p00010-my-profile.apx
    ```

5. Import several selected files together by listing their paths after `-files`:

    ```text
    apex import -files pages/p00010-my-profile.apx pages/p00011-leave-request.apx pages/p00012-onboarding-task.apx
    ```

6. Wait for success and repeat the relevant ESS runtime checks. These illustrative filenames are not fixed course page names. Paths are relative to the current SQLcl directory, and the application must already exist in the target workspace.

## Acknowledgements

- **Author** - Aravind Madhavan, Senior Product Manager, Oracle
- **Last Updated By/Date** - Aravind Madhavan, October 2026
