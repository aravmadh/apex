# Managing Applications Using APEXlang

## Introduction

> **Version prerequisite:** **Single-file import** requires **Oracle APEX 26.2 or later**. Export the application source from APEX 26.2 or later as well.

> **APEX 26.1 alternative:** You can still complete this module on APEX 26.1. Instead of importing individual files, import the entire updated application using the workflow from Module 23.

In Module 23, you exported the Talent Acquisition Portal (TAP) in APEXlang format. You edited its source and imported the complete application back into your workspace. TAP now includes the Candidate Search Help region. The Employee Self-Service Portal (ESS) is also available in the workspace. You will export its APEXlang source in this module.

Acme Corp wants more prominent buttons and clearer validation checks on form submissions across both applications. Use Codex to edit the exported page files and review the differences. First import one page, then import the complete application to apply the remaining changes. Oracle APEX 26.2 adds selected file import to this workflow.

### Objectives

In this module, you will learn how to:

- Add bulk validations to existing applications.
- Edit buttons across the applications.
- Import the pages that you updated.

Estimated Workshop Time: 60 minutes

### Prerequisites

> **Free Developer Tier:** Direct database connections from Oracle SQL Developer for VS Code are not supported for Oracle APEX Free Developer Tier workspaces. If you are using this tier, you can skip this module.

1. Complete **Module 4, Lab 1: Export and Inspect an Application with APEXlang**. Configure SQL Developer for VS Code to connect to the parsing schema for your workspace.
2. Complete Module 23, including Codex setup, Oracle APEXlang skills installation, and the complete TAP import.
3. For single-file import, use **Oracle APEX 26.2 or later** and **SQL Developer for VS Code 26.3.0 or later**. Export the projects from APEX 26.2 or later. On APEX 26.1, use full application import as learned in Module 23.
4. Have the course TAP and ESS applications, their existing schema objects, and test accounts available in the development workspace.
5. Keep the current TAP source folder available. If you move from APEX 26.1 to 26.2, export TAP again from the 26.2 environment before editing. Use the course TAP with Candidate Search Help, rather than the comparison app from Module 4.

### Workshop Outline

| Activity | Time | Outcome |
| --- | --- | --- |
| Introduction and prerequisites | 5 minutes | Confirm the existing apps, tools, and versions |
| Lab 1: Add bulk validations and make TAP buttons more prominent | 25 minutes | Apply bulk edits and verify selected page imports |
| Lab 2: Export ESS and inspect its source | 10 minutes | Prepare an ESS project linked to the existing app |
| Lab 3: Add bulk validations and make ESS buttons more prominent | 20 minutes | Repeat the workflow for employee forms |

Lab 4 is optional: try more application updates and SQLcl selected file imports. Allow 15 additional minutes outside the 60-minute estimate.

## Acknowledgements

- **Author** - Aravind Madhavan, Senior Product Manager, Oracle
- **Last Updated By/Date** - Aravind Madhavan, October 2026
