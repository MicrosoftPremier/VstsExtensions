[Back to Overview](./overview.md)

# Create Work Item - Change Notes

#### 2.3.2
- Add proper support for the old URL format of Azure DevOps Services (i.e., `https://yourOrg.visualstudio.com`)

#### 2.3.1
- Fix OIDC authentication on classic release.
- Fix minimum agent version.

#### 2.3.0
- Add support for bulk work item creation. (fixes GitHub issue #156)

#### 2.2.0
- Add support for cross-organization creation of work items. (fixes GitHub issue #157)

#### 2.1.0
- Add support for workload identity federation (OIDC) authentication.

#### 2.0.1
- Fix task editors for classic pipelines.

#### 2.0.0
- Upgrade task to Node 16.
- **BREAKING CHANGE:** Allow linking to work items outside the target project when using WIQL unless link limiting is enabled. (fixes GitHub issue #167)
- Add support for identity mentions (i.e., `@<display name>`, `@<upn>`, or `@<domain\username>`), work item mentions (i.e., `#id`), and pull request mentions (i.e., `!pr-id`).
- **BREAKING CHANGE:** Remove support for Team Foundation Server 2018 and Azure DevOps Server 2019.
- **BREAKING CHANGE:** Remove the `LinksFiltered` variable. Use `WorkItemLinksFiltered` instead.
- Add support for more complex duplicate rules. (fixes GitHub issue 211)

#### 1.17.1
- Allow manual specification of the link type when enabling work item linking. (workaround for GitHub issues #191 and #192)

#### 1.17.0
- Add additional output variables to check linking status. (fixes final discussion points from GitHub issue #167)

#### 1.16.1
- Fix error `Cannot read property 'id' of null` when trying to link a non-existent work item. (fixes remaining part of GitHub issue #167)

#### 1.16.0
- Add new output variable `LinksFiltered` to check if work item links have been filtered to the current project. (fixes GitHub issue #167)

#### 1.15.0
- Add support for limiting work item links to the same project. (fixes GitHub issue #166)

#### 1.14.0
- Add variable support to the project field in classic pipelines. (fixes GitHub issue #163)
- Remove unnecessary quotes from field mappings.

#### 1.13.0
- Fix WIQL generation error when identity fields are used as key fields for duplicate prevention.
- Add support for setting/updating extension fields (i.e., Kanban column, swimlane, and doing/done state). (fixes GitHub issue #148)

#### 1.12.0
- Fix task result when task fails. (was set to failed and then succeeded again)
- Fix WIQL generation when using empty fields as key fields like *Closed Date* in duplicate prevention. (fixes GitHub issue #147)
- Add parameter to specify the type of link used when associating work items with the current pipeline. (fixes GitHub issue #146)
- Add option to link to specific commits or changesets. (fixes GitHub issue #142)

#### 1.11.0
- Add option to allow HTTPS to HTTP downgrade redirects.

#### 1.10.2
- Bump version of azure-devops-node-api to fix proxy bug.
- Fix error `Cannot read property 'find' of undefined` on Team Foundation Server 2017.3. (fixes GitHub issue #137)

#### 1.10.1
- Bump version of the azure-pipelines-task-lib to get rid of NodeJS warning `Warning: Use Cipheriv for counter mode of aes-256-ctr`

#### 1.10.0
- Clarify missing task editors (work item type, area, iteration, additional fields) when used in task assistant view for YAML pipelines.
- Support team name instead of team ID for specifying current iteration.

#### 1.9.0
- Allow identity IDs (e.g., $(Build.RequestedForId)) for identity fields.

#### 1.8.4
- Fix a null reference (`Cannot read property 'replace' of null`) when empty secret variables exist in your pipeline.
- **NOTE:** You might need to approve installation of this version because we finally changed our publisher name from _Microsoft Premier Services_ to _Microsoft_. The approval does not show any scope changes (there are none).

#### 1.8.3
- Increase number of projects listed in the drop-down. (fixes GitHub issue #108)

#### 1.8.2
- Fix extension packaging issue.

#### 1.8.1
- Fix error message in task editor when additional fields contains empty lines.
- Fix issue where additional fields could not be saved when variable was used in task editor.

#### 1.8.0
- Created work item can now be associated with the current build or release.

#### 1.7.1
- Force minimum agent version v2.144.0 to support new Node10 execution handler.

#### 1.7.0
- Add support for using a Personal Access Token (PAT) for authentication.

#### 1.6.2
- Doc update

#### 1.6.1
- Fix null reference when task is configured with invalid project or work item type.
- Add proper validation for enumerated task inputs (typo handling for YAML pipelines).
- Log warning and skip task on pull request builds for forked repositories (security context does not allow working with work items).
- Mask all secrets in work item fields.

#### 1.6.0
- Add support for work item attachments.

#### 1.5.5
- Fix null reference when duplicate handling is enabled and input data for work item creation is invalid.
- Log errors to standard log output instead of debug output.

#### 1.5.4
- Fix error when trying to set iteration path to the special value {team ID}@currentIteration during work item duplicate update.

#### 1.5 3
- Fix compatibility issues with Team Foundation Server versions prior to 2018 Update 2.

#### 1.5.2
- Fix error **Cannot read property 'isNew' of null** when new work item could not be created.
.
#### 1.5.1
- Fix error **Cannot read property '...' of undefined** in output variable creation.

#### 1.5.0
- Fix an error **Cannot read property 'relations' of null**
- Output variables are now also created when duplicate handling is active and duplicates are found.
- Add new special value *CWI.WorkItemEditUrl* to access the editor URL of a work item for output variable creation.
- Special values and field names for output variable creation are now case-insensitive.
- Fix creation of output variables for identity values. Ouput variables for identity values now have the format *Display Name &lt;Uniqe Name&gt;* where unique name is the UPN for Azure DevOps Services or the domain account for Team Foundation Server/Azure DevOps Server.
- Add support for updating a single duplicate work item.

#### 1.4.0
- Add support for linking PR with WorkItem.

#### 1.3.5
- Fix issue in WIQL generation that sometimes failed the duplicate prevention.

#### 1.3.4
- Fix another issue with identity validation (failed for on-prem identities in domain format (e.g., "Some User <Domain\\SUser>")).

#### 1.3.3
- Fix an issue with identity value validation (failed for email addresses that have mixed casing).

#### 1.3.2
- Fix performance issue when testing identity values.

#### 1.3.1
- Update docs (fix link to on-prem extension).

#### 1.3.0
- Fix compatibility issue with latest Azure DevOps deployment leading to the error **VS403666: The email of the identity is not viewable**.
- Remove support for Team Foundation Server 2017 due to library incompatibility.

#### 1.2.2
- Fix bug in output variable creation that led to empty variables.

#### 1.2.1
- Remove error log message when not items to link are found.

#### 1.2.0
- Add support for creating work items in different team projects.
- Add new link option to link all work items associated with the current build/release to the created work item.

#### 1.1.1
- Fix bug in variable substitution that could lead to variables not being replaced by their actual values.
- Update extension manifest to include links to license, GitHub repository for issue tracking.
- Add YAML documentation.

#### 1.1.0
- Add support for creating work items in a team's current iteration.
- Add support for output variables.
- Fix nested variable substitution and handling of variables with equal signs (e.g., URLs).

#### 1.0.7
- Fix error in additional fields editor due to new "magic fields" in work items.

#### 1.0.5
- Fix null references when linking or preventing duplicates.

#### 1.0.4
- Update docs to reflect the new *Azure DevOps Services* brand.

#### 1.0.3
- Fix invalid handling of empty patch operations.

#### 1.0.2
- Fix input checking - Assigned To should be optional as documented.

#### 1.0.1
- Fix publisher ID.

#### 1.0.0
- Initial version
- Supports creating work items, linking new work item to build, linking new work item to other work items, preventing duplicates based on key fields.