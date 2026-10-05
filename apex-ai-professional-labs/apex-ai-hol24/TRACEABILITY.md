# Module 24 Source Traceability

Estimated Time: 5 minutes (author review)

## Source Classification

All material used is Oracle-authored or supplied as requirements by the workshop author. Module 23 is hosted on GitHub Pages but credits Ankita Beri, Oracle APEX product management; the internal architecture source corroborates the course flow. This resolves the initial ownership uncertainty. No external or unclear assets are adapted, and no external-source approval gate remains.

The Confluence sources are classified **Oracle Highly Restricted**. This authoring record is internal and must be excluded from a public learner package. Learner-facing markdown contains no internal URLs, private source excerpts, or source metadata. Publication must follow the source owner's release process; this task creates local workshop files only.

## Source Map

| Area | Source | Classification | Use and evidence |
| --- | --- | --- | --- |
| Course context | https://confluence.oraclecorp.com/confluence/pages/viewpage.action?pageId=20790293620 | Oracle-owned/internal; Oracle Highly Restricted | Retrieved through Oracle Central Confluence, version 131, updated October 5, 2026. Module 24 is incomplete. User requirements override its old duration, translation scope, and assumption that both applications were exported in Module 23. |
| Selected file import | https://confluence.oraclecorp.com/confluence/pages/viewpage.action?pageId=21939898928 | Oracle-owned/internal; Oracle Highly Restricted | Retrieved through Oracle Central Confluence, version 28, updated October 5, 2026. Supports Import File for the open file; requires target app to exist, APEX 26.2+ on target and export; SQLcl -files accepts space-separated paths relative to current directory; shared dependencies precede page imports in VS Code. |
| Import boundaries | Same selected file import source | Oracle-owned/internal | App Builder selected file import is unsupported; themes/templates/plug-ins/workspace components excluded. Compare/pull requires SQL Developer for VS Code 26.3+. |
| Module 23 continuity | https://ankberi.github.io/apex/apex-ai-professional-labs/apex-ai-hol23/workshops/tenancy/index.html?lab=lab-1-export-tap | Oracle-authored, third-party hosted | Manifest and lab markdown fetched headlessly. Author credit identifies Oracle product management. Codex setup, skills installation, TAP export, Candidate Search Help, and full TAP import establish the starting state. No screenshots copied. |
| Module 4 prerequisites | ../apex-ai-hol4/1-introduction-to-apexlang/1-introduction-to-apexlang.md | Oracle-owned repository material | SQL Developer extension setup, parsing-schema connection, application export, and deployment identity. No assets copied. |
| Validation terminology | https://docs.oracle.com/en/database/oracle/apex/26.1/htmdb/understanding-validations.html | Oracle-owned/public | Associated Item, inline display, Always Execute, Execute Validations, and submission conditions. |
| Button terminology | https://docs.oracle.com/en/database/oracle/apex/26.1/htmdb/creating-buttons.html | Oracle-owned/public | Button Name/REQUEST relationship and existing button actions. Labels change while request values and references remain stable. |
| Export/import baseline | https://blogs.oracle.com/apex/apexlang-in-practice-export-edit-validate-and-import-oracle-apex-applications | Oracle-owned/public | VS Code export path, import diagnostics, Problems panel, and project layout. |
| Optional SQLcl baseline | https://docs.oracle.com/en/database/oracle/apex/26.1/apxdc/using-sqlcl-apexlang.html | Oracle-owned/public | Parsing-schema connection and apex validate -input syntax. 26.2 -files syntax comes from the internal feature source, not the 26.1 documentation. |
| Business exercise rules | Current build request and author-created teaching examples | Workshop-author supplied/original | Bulk validations/button edits, separate ESS export, extension notes, VS Code priority, optional SQLcl. Proposed validation examples are teaching requirements, not claims that every source application lacks those checks. |

## Embedded Assets and Attribution

- Internal source attachments were listed but not downloaded, copied, or recreated.
- Module 23 screenshots and third-party logos were not reused.
- No copied datasets, raw application exports, schema DDL, or third-party code are included.
- Codex prompts and test matrices are original teaching material.
- Optional SQLcl commands adapt the Oracle feature source with clearly illustrative page filenames.
- Learner links point only to public documentation and published course modules.
- Public Oracle sources require provenance tracking; no third-party permission acknowledgement is needed.

## Technical Validation and Remaining Gaps

- The user authorized the installed db skill for optional SQLcl validation. Read db/SKILL.md and db/sqlcl/sqlcl-basics.md; checked command syntax against the Oracle documentation and the 26.2 feature source.
- No application export or live database connection was supplied. AI-generated .apx code, actual changed-page coverage, import diagnostics, and runtime behavior remain unexecuted.
- Guided prompts discover page and item identities, preserve IDs, reuse existing checks, and avoid unsupported grid/page-item substitutions.
- The published Module 4 URL returned 404, so learner prerequisites cite its course lab title without a broken link. Module 23 returned HTTP 200. The CDN request timed out; the local loader smoke test passed, but rendered preview remains unverified.
- Validate the workshop in a compatible 26.2 development environment before release. Confirm that at least two applicable form pages per application can demonstrate the requested bulk changes.

## Current Revision

The workshop author requested two short prompts per application: one for validations across two or three pages, and one to make buttons more prominent across all pages. Labs 1 and 3 now separate those tasks. The button prompt leaves styling choices to Codex; runtime checks use the author's examples of red Delete and green Create buttons. Additional validation, label, help, and exploration prompts move to optional Lab 4, along with both applications' optional SQLcl tasks. The 60-minute core and the author's edited prerequisites remain intact. No new external sources or assets are used.

## Objective and Prerequisite Update

The workshop author requested outcome-based objectives: add bulk validations, edit buttons across the application, and import updated pages. Removed single-prompt and natural-language wording from learner-facing titles and instructions. Added the author-supplied Free Developer Tier direct database connection limitation and skip guidance to Module 24 prerequisites and Module 4 introduction and export lab prerequisites. The restriction names the hosted Free Developer Tier workspace rather than all free Oracle databases.

## Lab 1 Screenshot Review

The workshop author added 18 local screenshots to Lab 1. Reviewed all 18 to check UI labels, file paths, comparison views, import notifications, the headcount error, and button appearance. These workshop-author-supplied screenshots show the author's Oracle APEX application and development workflow; no external image source was used. Image files were preserved, and the lab now includes descriptive alt text and proper step indentation.

The screenshots show three form files updated for validations and 16 pages updated for buttons in this example. They show a single Job Requisition page import followed by the author's complete application import. The rewritten instructions preserve that flow and describe counts as examples. Screenshots provide evidence of the captured results, but this edit does not constitute a fresh live execution of the lab.

## October 6 Consistency Review

Reviewed the introduction and all four labs after the workshop author's manual edits. Preserved the two edit tasks, the page-then-application import flow, the short prompts, and all image files. Updated the introduction and workshop details to match that flow. Lab 2 now describes the shortened export procedure and the project inspection shown in its screenshot.

Reviewed the two ESS export screenshots and all 16 new ESS editing/import screenshots. Added descriptive alt text and step indentation. The Leave Request runtime screenshot shows the existing date rule, so its caption and instructions distinguish that check from the newly added reason validation. The onboarding source accepts Done or Completed; the runtime test now reflects the available status values.

Ran the bundled skill validator and its Lanham heuristic on all five learner-facing Markdown files. Formatting, task numbering, image references, and manifest consistency were checked. No live database commands or active browser navigation were used. Screenshot evidence supports the documented captured examples, but a fresh live end-to-end run remains outside this review.

## Acknowledgements

- **Author** - Aravind Madhavan, Senior Product Manager, Oracle
- **Last Updated By/Date** - Aravind Madhavan, October 2026
