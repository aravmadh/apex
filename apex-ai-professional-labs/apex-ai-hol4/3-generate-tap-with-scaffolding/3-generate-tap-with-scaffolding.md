# Generate a TAP Application Blueprint with Scaffolding

## Introduction

You now have the business requirements, schema details, and generation rules in one folder. Use them to generate a blueprint for a TAP comparison app.

### Objectives

- Review the TAP source files before you generate the blueprint.
- Prompt a coding agent to create a constrained blueprint.
- Inspect the blueprint for a TAP-focused page layout.

Estimated Time: 25 minutes

## Task 1: Review the inputs before generation

1. In the `tap_sdd` folder, verify that these files are present:

    - `tms_module_04_functional_spec.md`
    - `tap_schema_metadata.md`
    - `blueprint_prompt.md`
    - `apex-fa-icons-allowlist.txt`
    
    ![TAP SDD project folder with the required input files](images/task-01-step-01-files.png)
2. Read the functional specification and confirm that it describes the Talent Acquisition Portal, not the Employee Self-Service Portal or HR Analytics App.

3. Review the schema metadata and confirm that it includes every table and column used by the app.

## Task 2: Generate the Application Blueprint

1. Give all four source files to your coding agent.

2. Submit the following prompt:

    ```
    <copy>Use tap_schema_metadata.md for the database schema, tms_module_04_functional_spec.md for business context, and apex-fa-icons-allowlist.txt for icon choices. Generate a complete Application Blueprint that follows blueprint_prompt.md exactly. Use only allowlisted APEX icons. Save the final blueprint as tap_generated_blueprint.md.
    </copy>
    ```
    ![Blueprint-generation prompt in the coding agent](images/task-02-step-02-prompt.png)

3. Review each file-write request. Approve it only when the agent creates or updates `tap_application_blueprint.md` in `tap_sdd`.

4. Wait for the agent to finish writing the blueprint. 

    ![Completed TAP Application Blueprint generation](images/task-02-step-04-completion.png)

## Task 3: Inspect the generated blueprint

1. Open `tap_application_blueprint.md`.

2. Confirm that the blueprint names the app `Talent Acquisition Portal` and includes only TAP pages and components.
    ![Generated TAP Application Blueprint file](images/task-03-step-03-gen-bp.png)

## Task 4: Enable REST for Your Schema

**Prerequisite before moving to the next lab**

1. In APEX, open **SQL Workshop**, then select **RESTful Services**.
    ![RESTful Services option in SQL Workshop](images/task-04-step-01-rest.png)
2. Click **Register Schema** with **ORDS (Oracle REST Data Services)**.
    ![Register Schema action for ORDS](images/task-04-step-02-register.png)

3. APEX fills in the **Schema Alias**. Change it only if needed. Turn off **Install Sample Service**, then click **Save Schema Attributes**.
    ![ORDS schema registration settings](images/task-04-step-03-schema.png)

> **Note:** Complete this step before you import the app from the blueprint.


## Acknowledgements

 - **Author -** Aravind Madhavan, Senior Product Manager.
 - **Last Updated By/Date** - Aravind Madhavan, Senior Product Manager, July 2026
