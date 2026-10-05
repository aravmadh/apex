# Module 24 Workshop Details

Estimated Time: 5 minutes (author review)

## Short Description

Use Codex and APEXlang to add bulk validations and improve button prominence across TAP and ESS. Compare the changes, import a page, then import the complete application through SQL Developer for VS Code.

## Long Description

Continue the TAP export, edit, and complete application import workflow from Module 23. Add bulk validations to existing form pages and edit buttons across the application. Review the changed source and use single-file import in Oracle APEX 26.2 to apply one page. Then import the complete application to apply the remaining changes.

Export the existing ESS application in a separate lab, then repeat the same workflow for employee profile, leave, and onboarding forms. Test invalid and valid submissions, cancellation behavior, and unchanged pages. An optional lab provides more prompts for validations, labels, and field help, plus SQLcl single-file and multiple-file imports.

## Workshop Outline

| Sequence | Activity | Duration |
| --- | --- | --- |
| Introduction | Confirm prerequisites, versions, and application identities | 5 minutes |
| Lab 1 | Add Bulk Validations and Make TAP Buttons More Prominent | 25 minutes |
| Lab 2 | Export ESS and Inspect Its Source | 10 minutes |
| Lab 3 | Add Bulk Validations and Make ESS Buttons More Prominent | 20 minutes |
| Lab 4 (optional) | Optional Application Updates | 15 additional minutes |

Total: 60 minutes. Lab 4: Optional Application Updates adds 15 minutes outside this estimate.

## Workshop Prerequisites

- Direct database connections from SQL Developer for VS Code are not supported for Oracle APEX Free Developer Tier workspaces. Learners using this tier can skip this module.
- Complete Module 4 SQL Developer for VS Code installation and connection setup.
- Complete Module 23 Codex/Oracle APEXlang skills setup and the TAP application import.
- Have the course TAP and ESS applications and their underlying schema available.
- For single-file import, use Oracle APEX 26.2 or later and SQL Developer for VS Code 26.3.0 or later. On APEX 26.1, follow the edits and import the entire application as learned in Module 23.
- For single-file import, export the projects from APEX 26.2 or later; refresh an older TAP export first. APEX 26.1 projects use full application import.
- Use test accounts and records in a development workspace.

## Authoring and Review Notes

- Mode: publish-ready authoring, with environment verification still pending.
- APEX 26.1 documentation governs component terminology; selected file import is a 26.2 feature.
- VS Code imports the open file. The guided labs demonstrate a single-page import followed by a full application import. SQLcl demonstrates selected file imports as optional practice.
- The guided comparison uses SQL Developer for VS Code 26.3 or later.
- Discover actual page IDs, item names, and requests from each export. Optional command filenames are illustrative and require substitution.
- Reuse existing rules and preserve stronger validations from earlier modules.
- FreeSQL is not used because these tasks modify and import an installed APEX application rather than run standalone schema exercises.
- The published Module 4 URL returned 404. Prerequisites refer to its lab title in the course materials instead.
- The labs include workshop-author-supplied screenshots. A live 26.2 environment is needed to verify the complete workflow again.

## Acknowledgements

- **Author** - Aravind Madhavan, Senior Product Manager, Oracle
- **Last Updated By/Date** - Aravind Madhavan, October 2026
