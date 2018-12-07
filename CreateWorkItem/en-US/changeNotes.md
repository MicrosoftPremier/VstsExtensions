[Back to Overview](./overview.md)

# Create Work Item - Change Notes

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