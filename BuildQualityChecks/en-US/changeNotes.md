[Back to Overview](./overview.md)

# Build Quality Checks - Change Notes

#### 7.4.5
- Always show all warning changes in statistics output.
- **NOTE:** You might need to approve installation of this version because we finally changed our publisher name from _Microsoft Premier Services_ to _Microsoft_. The approval does not show any scope changes (there are none).

#### 7.4.4
- Further improve statistics output.

#### 7.4.3
- Improve statistics output (rearrange some messages).
- Fix artifact download issue on non-Windows agents.

#### 7.4.2
- Fix sporadic incorrect message in warning statistics that warnings haven't changed even if changes are displayed.

#### 7.4.1
- Fix null reference in new warnings statistics. (fixes GitHub issue #107)

#### 7.4.0
- Move summary markdown to temporary files instead of `$(Build.SourcesDirectory)`.
- Enhance warning statistics with detailed information about added and removed warnings.
- Fix error with summary upload when run title includes invalid characters. (fixes GitHub issue #106)

#### 7.3.0
- Fix: Create warning statistics when only file warnings analysis is enabled.
- Improvements to policy result output:
  - Make run title more prominent
  - Display subtotals in warning statistics
  - Add warning file analysis title

#### 7.2.2
- Fix typo in warning message.
- Additional fixes for GitHub baseline handling. (GitHub issue #102)
- Remove warning about missing permissions when repository is not in Azure DevOps.

#### 7.2.1
- Fix typo in docs.
- Fix handling of missing baseline build during file warnings analysis. (fixes GitHub issue #100)
- Fix issue with baseline handling for GitHub repositories. (fixes GitHub issues #101, #102)

#### 7.2.0
- Add warning statistics support for more pipeline tasks. Every task logging issues using the logging commands (i.e., `##vso[task.logissue ...]`) should be automatically picked up by the statistics (thanks to Gerwin J. for the suggestion!).
- Include .NET Core task in the default task filters.
- Add warning statistics support for custom warning filters with predefined named groups.
- Fix doc issues.

#### 7.1.0
- Automatically fall back to pull request target branch when looking for baseline builds.

#### 7.0.1
- Force minimum agent version v2.144.0 to support new Node10 execution handler.

#### 7.0.0
- Add support for evaluating file-based warnings from previous builds.
- Add option to treat 0/0 covered elements as 100% coverage instead of 0%.
- Add coverage delta type to log outputs.
- **BREAKING CHANGE** Change default coverage delta type to percentage.

#### 6.4.4
- Fix bug on older Team Foundation Servers where system variable _System.CollectionUri_ is not properly set.

#### 6.4.3
- Fix pull request status feature for project containing whitespace in the project name.

#### 6.4.2
- Fail code coverage policy if *Use Uncovered Elements* is enabled and there is no code coverage data or the task cannot read the data from Azure DevOps.
- **!SCOPE CHANGE!** Since we received a lot of feedback after adding the additional security scopes *vso.code_write* and *vso.code_status*, we decided to remove them from the extension again. Those scopes do not affect pipeline tasks as their abilities are only tied to the permissions granted to the build service accounts. As security and privacy are important topics everybody should be concerned about, we put together detailed information in our [FAQ](https://github.com/MicrosoftPremier/VstsExtensions/blob/master/BuildQualityChecks/en-US/FAQ.md#general).

#### 6.4.1
- Add policy result output variables.
- Publish policy results as PR statuses.
- Task filters now match against the display name as well as the task name.
- Add better error handling around enumeration-based task parameters (e.g., coverageType) to deal with typos in YAML files.
- Add support for evaluating warnings from arbitrary log files based on regular expression warning filters.
- **!SCOPE CHANGE!** This version of the task requires two additional security scopes for Azure DevOps: *vso.code_write* and *vso.code_status*. These scopes are required in order to publish pull request statuses and create pull request comments (see [Pull Request Status API](https://docs.microsoft.com/en-us/rest/api/azure/devops/git/pull%20request%20statuses/create?view=azure-devops-rest-5.1) for more information).

#### 6.4.0
- Skipped due to publishing error.

#### 6.3.0
- Fix comparison issue with warnings policy.
- Add support for using uncovered elements when evaluating the code coverage policy (thanks for the suggestion, mjvk!).

#### 6.2.4
- Fix warning statistics creation when new baseline is forced or no baseline exists.

#### 6.2.3
- Fail policies with correct error message when invalid YAML inputs are provided.
- Fix policy result for warnings policy when baseline build is missing.

#### 6.2.2
- Fix baseline build handling for old build pipelines.

#### 6.2.0
- Fix handling of parallel build jobs to ensure that warnings are only compared to warnings from the correct job of a previous build.
- Add option to log task warnings and errors as build issue (enabled by default).
- Fix bug that would not fail the coverage policy if delta type was absolute, ignoreDecreaseAboveUpperThreshold was enabled, and coverage value was above 100.
- Add precision parameter.
- Fix default task filters to properly work with YAML pipelines.
- Fix MSBuild log analysis to ensure that all warnings are captured.

#### 6.1.2
- Support using the global regex flag (/g) for warning filters.

#### 6.1.1
- Fix double counting of warnings when warning filters were combined with warning statistics.

#### 6.1.0
- Add option to include filtered warnings in addition to regularly detected warnings.

#### 6.0.0
- Change default behavior of the "Force Coverage Improvement" setting. The policy does not fail if the code coverage decreases but still remains above the upper threshold. To revert back to the old behavior, unselect the new "Ignore Decrease Above Threshold" option.
- Change default behavior of the "Force Filter" setting. The policy now fails if it cannot find code coverage data associated with the specified configuration and platform. 
- Add support for new code coverage backend in Azure DevOps (better performance and reliability).
- Fix a bug that kept the task waiting for skipped previous tasks.

#### 5.2.4
- Increase default wait timeouts.

#### 5.2.3
- Add YAML documentation.

#### 5.2.2
- Update extension manifest to include links to license, GitHub repository for issue tracking, and build badge.

#### 5.2.1
- Fix a null reference in the new force filter logic.

#### 5.2.0
- Add support for only evaluating code coverage data that is not associated with a platform and configuration as created by 3rd party tools like Cobertura.
- Add variable BQC.ForceNewBaseline to force a new baseline build.

#### 5.1.0
- Group warnings by build task in the task output and summary section (thanks to Mike W. for the suggestion).
- Add support for StyleCop log analysis.

#### 5.0.6
- Update docs to reflect the new *Azure DevOps Services* brand.

#### 5.0.5
- Disable generic log analysis if no warning filters are configured. Previous behavior resulted in the task not counting warnings from non-MSBuild-based build tasks if warning statistics were enabled.

#### 5.0.4
- Fix a bug that failed the code coverage policy if the 'Force Coverage Improvement' option was set and the percentage coverage value increased while the absolute coverage value decreased (e.g., baseline was 180 of 200 blocks covered = 90%, current build has 143 of 150 blocks covered = 95%).

#### 5.0.2
- Fix a bug that would sometimes compare the current build against the wrong build phases/jobs from the previous build.
- Fix links to pictures in the Visual Studio Marketplace.
 
#### 5.0.0
- Update vsts-node-api and vsts-task-lib to latest versions. This also fixes a localization issue that lead to warnings about missing resource files.
- Increase precision of comparison of code coverage percentage values. If no variance is supplied, percentage values will use four decimals instead of the previous two. This picks up a change of one element (e.g., block) in a million elements. When used with variance, the precision will be taken from the variance value.

#### 4.0.4
- Fix typo in the code that might lead to code coverage policy waiting indefinitely when there is no code coverage data in a build.

#### 4.0.3
- Fix vsts-node-api initialization bug in code coverage policy that sometimes leads to the error "Cannot read property 'getCodeCoverageSummary' of undefined".

#### 4.0.2
- Fix extension target definition. Due to new vsts-node-api the lowest compatible version of Team Foundation Server is 2017 update 3. For earlier versions of Team Foundation Server, please keep using version 3.0.1.

#### 4.0.1
- Fix summary section for multi-config builds (missing job information)

#### 4.0.0
- Added support for build phases.
- Added support for YAML builds.
- Removed the incorrect default value for build platform.
- Moved official documentation to https://github.com/MicrosoftPremier (thanks for the feedback, Jens J.).
- Removed support for Team Foundation Server 2015 due to incompatibility with the latest vsts-node-api.

--------------------------------------------------------------------------------
**Note:** Version 3.x is the last version compatible with Team Foundation Server 2015!

#### 3.0.1
- Fixed a bug that could cause the error "Cannot read property 'label' of undefined" when using multiple test adapters.

#### 3.0.0
- We're re-releasing v2.9.3 as v3.0.0 due to a breaking change we missed during testing.

#### 2.9.4
- We're re-releasing v2.9.2 with a newer version because it contained a breaking change. Version 2.9.3 will be re-released as v3.0.0.

#### 2.9.3
- Fixed a bug that caused code coverage policy to read wrong data when multiple test adapters were used.

#### 2.9.2
- Fixed a rare bug that caused the code coverage policy to assume zero coverage even though coverage data was available.

#### 2.9.0
- Add warning if NodeJS certificate check has been disabled on an agent that supports configuration of custom CA certificates.

#### 2.8.2
- Fixed the error "Cannot read property 'records' of undefined" thrown by the warnings policy when warning statistics are enabled
  and no baseline build exists.

#### 2.8.1
- Fixed the error "Unexpected token ..." on older build agent versions.

#### 2.8.0
- Added configurable variance for warnings and code coverage policy.

#### 2.7.0
- Added an option to search for specific baseline build definitions.

#### 2.6.3
- Fixed an issue with NodeJS module loading that prevented the task from running on Linux/Unix.

#### 2.6.2
- Added navigation and FAQ sections to extension documentation.

#### 2.6.1
- Fixed typos in messages.

#### 2.6.0
- The default values for the code coverage policy parameters _Configuration_ and _Platform_ have been changed to be empty to match the default
  parameters of the _Visual Studio Test_ task.
  
#### 2.5.1
- Fixed a bug that caused the warnings policy to wait for log information of unfinished tasks when warning statistics are enabled and
  task filters are empty.
  
#### 2.5.0
- Added support for warning statistics.

#### 2.4.2
- Added support for disabling NodeJS certificate check for the task. See [advanced settings](./overview.md#advanced) for more
  information.

#### 2.4.1
- Added details about the NodeJS certificate issue to the docs.

#### 2.4.0
- Added support for run titles to distinguish between multiple task runs in the same build.
- Added support for warning filters.
- Added links to documentation to every task parameter.

#### 2.3.1
- Fixed duplicate logging of warning.

#### 2.3.0
- Added a warning if there are no code coverage data sets matching the specified build configuration and platform.

#### 2.2.0
- Added an option to prevent comparing policy values to partially succeeded builds.
- Added variables to configure waiting behavior for code coverage data.
- Updated screenshots to new build editor.
- Removed German documentation.

#### 2.1.2
- Added internal configuration options (for support purposes).

#### 2.1.1
- Fixed extension manifest to work with latest Visual Studio Marketplace behavior.

#### 2.1.0
- Fixed a race condition that sometimes resulted in zero code coverage for the current build.

#### 2.0.2
- Added back support for Team Foundation Server 2015 (was broken since v1.5.0). See [known issues](./overview.md#known-issues)
  for limitations on Team Foundation Server 2015!

#### 2.0.1
- Fixed a rare issue that could have caused the policies to report false results if the build steps of the definition was
  changed while a build of that definition was running.
  
#### 2.0.0
- You can now choose a baseline build definition and branch for the comparison of policy values. See
  [Baseline Task Parameters](./overview.md#baseline)
  for more information.
- Policies will now pass by default if the previous build is not available.
- Module filters have been removed from the code coverage policy. Please use run settings (or similar settings specific to your test
  and code coverage tools) to filter code coverage.

#### 1.5.1
- Fixed documentation to work with Visual Studio Marketplace.

#### 1.5.0
- Added support for multi-configuration builds.

#### 1.4.2
- Added additional checks for module filters.

#### 1.4.1
- Small change in code coverage data inspection.

#### 1.4.0
- Code coverage policy now supports multiple code coverage types (e.g., blocks, lines, branches).
- Fixed race conditions when used with task groups.
- Module filters for code coverage policy are now deprecated. The filters were more confusing than helpful and will be removed in
  the next major version of the extension. Please use run settings (or similar settings specific to your test and code coverage tools)
  to filter code coverage.
- The default value for module filters has been removed.

#### 1.3.0
- Fixed race condition due to asynchronous build information upload.
- Code coverage policy now works correctly for builds with multiple code coverage calculation steps (e.g., multiple VSTest tasks).
- Added information about tasks and modules that are evaluated by the policies to the task log.

#### 1.2.2
- Fixed bug in code coverage policy that resulted in the error **Cannot read property 'percentage' of undefined**.

#### 1.2.1
- Updated task to work without enabled OAuth token access.

#### 1.1.5
- First public version.