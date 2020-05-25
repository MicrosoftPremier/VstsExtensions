[Known Issues](#known-issues) | [Support](#support) | [YAML](#adding-the-task-to-a-yaml-build-definition) | [Policies](#policies) | [Task Parameters](#task-parameters) | [Task Variables](#task-variables) | [Policy Results](#policy-results) | [Common Usage Scenarios](#common-usage-scenarios) | [FAQ](#faq)

# Build Quality Checks
The *Build Quality Checks* task allows you to add quality gates to your build process.

### Change Notes
You can find the changes notes for this task [here](https://github.com/MicrosoftPremier/VstsExtensions/blob/master/BuildQualityChecks/en-US/changeNotes.md).

### Known Issues
- We are phasing out Team Foundation Server 2015. If you're still using Team Foundation Server 2015, please stay on version 3.x of the extension which can be downloaded [here](https://github.com/MicrosoftPremier/VstsExtensions/blob/master/BuildQualityChecks/archive/MSPremier.BuildQualityChecks-3.0.1.vsix). For newer versions of Team Foundation Server or Azure DevOps Services please use version 4.x and higher.

### Support
If you need help with the extension or run into issues, please contact us at <a href='&#109;&#97;&#105;&#108;&#116;&#111;&#58;&#112;&#115;&#103;&#101;&#114;&#101;&#120;&#116;&#115;&#117;&#112;&#112;&#111;&#114;&#116;&#64;&#109;&#105;&#99;&#114;&#111;&#115;&#111;&#102;&#116;&#46;&#99;&#111;&#109;'>&#112;&#115;&#103;&#101;&#114;&#101;&#120;&#116;&#115;&#117;&#112;&#112;&#111;&#114;&#116;&#64;&#109;&#105;&#99;&#114;&#111;&#115;&#111;&#102;&#116;&#46;&#99;&#111;&#109;</a> or create an issue [here](https://github.com/MicrosoftPremier/VstsExtensions/issues).

### Adding the Task to a Build Definition
The *Build Quality Checks* (in task category *Build*) task needs to be placed after the tasks it should inspect. In a Visual Studio build definition, e.g., an appropriate place would be after build, test, and symbol indexing/publishing. This ensures that, even if the policy breaks the build, you still get test results as well as the compile output and symbols.

![Task Placement](../assets/AddTask.png "Proper placement of the Build Quality Checks task")

**Note:** When you want to use the [Code Coverage Policy](https://github.com/MicrosoftPremier/VstsExtensions/blob/master/BuildQualityChecks/en-US/CodeCoveragePolicy.md) you need to make sure that you publish code coverage values calculated by your test tool first. The Policy itself does **not** calculate the code coverage. Before you add the task and activate the policy, please make sure that you can already see code coverage values in your build summary. If you are using a test tool other then MSTest (i.e., anything other than the _Visual Studio Test_ task), please use the _Publish Code Coverage Results_ task to publish your coverage data before you add the policy.

### Adding the Task to a YAML Build Definition
To add the *Build Quality Checks* task to a YAML build definition, use the  task name and major version like this `- task: BuildQualityChecks@6` and set a display name using the `displayName` property. Then add all task inputs as described under [Task Parameters](#task-parameters) or in the [Policies](#policies) section.

YAML snippet:

``` yaml
- task: BuildQualityChecks@6
  displayName: 'Check build quality'
  inputs:
    # ===== Warnings Policy Inputs =====
    #checkWarnings: false # Optional
    #warningFailOption: build # Optional; Valid values: build, fixed
    #warningThreshold: '0' # Optional
    #forceFewerWarnings: false # Optional
    #allowWarningVariance: false # Optional
    #warningVariance: # Required if allowWarningVariance = true
    #evaluateTaskWarnings: true # Optional
    #warningTaskFilters: '/^((vs|ms)build|ant(\\s+.+)?|gradle(w)?(\\s+.+)?|grunt|gulp|maven(\\s+.+)?|xamarin(android|ios)|xcode(\\s+.+)?|cmake|build\\s+.+)$/i' # Optional
    #warningFilters: # Optional
    #inclusiveFiltering: false # Optional
    #showStatistics: false # Optional
    #evaluateFileWarnings: false # Optional
    #warningFilesFolder: # Optional
    #warningFiles: # Required if evaluateFileWarnings = true
    #warningFileFilters: # Required if evaluateFileWarnings = true
    #warningFilesArtifact: # Required if evaluateFileWarnings = true and warningFailOption = build
    # ===== Code Coverage Policy Inputs =====
    #checkCoverage: false # Optional
    #coverageFailOption: build # Optional; Valid values: build, fixed
    #coverageType: blocks # Optional; Valid values: blocks, lines, branches, custom
    #customCoverageType: # Required if coverageType = custom
    #treat0of0as100: false # Optional
    #coverageThreshold: '60' # Optional
    #forceCoverageImprovement: false # Optional
    #coverageUpperThreshold: '80' # Optional
    #ignoreDecreaseAboveUpperThreshold: true # Optional
    #useUncoveredElements: false # Optional
    #allowCoverageVariance: false # Optional
    #coverageVariance: # Required if allowCoverageVariance = true
    #coverageDeltaType: percentage # Optional; Valid values: absolute, percentage
    #coveragePrecision: '4' # Optional
    #buildConfiguration: # Optional
    #buildPlatform: # Optional
    #explicitFilter: false # Optional
    # ===== Baseline Inputs =====
    #includePartiallySucceeded: true # Optional
    #baseDefinitionFilter: # Ignored - only used by UI editor
    #baseDefinitionId: # Optional
    #baseRepoId: # Ignored - only used by UI editor
    #baseBranchRef: # Optional
    # ===== Reporting Inputs =====
    #runTitle: # Optional
    # ===== Advanced Inputs =====
    #disableCertCheck: false # Optional
    #createBuildIssues: true # Optional
```

### Policies
The *Build Quality Checks* task currently supports two policies (click the link for details):

- **[Warnings Policy](https://github.com/MicrosoftPremier/VstsExtensions/blob/master/BuildQualityChecks/en-US/WarningsPolicy.md)** - Allows you to fail builds based on the number of build warnings.
- **[Code Coverage Policy](https://github.com/MicrosoftPremier/VstsExtensions/blob/master/BuildQualityChecks/en-US/CodeCoveragePolicy.md)** - Allows you to fail builds based on the code coverage value of your tests.

### Task Parameters

#### Baseline
If you choose `Previous Value` for the *Fail Build On* option for one of the policies, the policy value (e.g., number of warnings) is, by default, compared to the corresponding value from the last build that ran for the current build definition. If the build definition targets a Git repository hosted in Azure DevOps Services or Team Foundation Server, the policy will look for the last build that ran against the same branch as the current build. This behavior can be customized with the following parameters:

![Baseline Parameters](../assets/Baseline.png "Choosing baseline build definition and branch")

- <a name="partial">**Include Partially Succeeded Builds:**</a> Uncheck this option if policy values should only be compared to successful baseline builds. In most cases, including partially succeeded builds is the best option, so you can use the default setting.

  **YAML: includePartiallySucceeded** - (Optional) Default value is *true*.

- <a name="baseDefFilter">**Definition Filter**</a> If you have lots of build definitions, it can get hard to find the right one in the *Build Definition* drop-down list. You can use the *Definition Filter* to limit the number of definitions shown in the list. Either enter a specific definition name or use the asterisk (\*) wildcard to search for definitions starting with (e.g., Def\*), ending with (e.g., \*def), or containing (e.g., \*def\*) a specific value. If you want to list all build definitions in your project, use the default value "\*".

  **YAML: baseDefinitionFilter** - (Ignored) This parameter is ignored in YAML definitions.

- <a name="baseDef">**Build Definition:**</a> Select the build definition that should be used to search for the baseline build. If you do not set a value, the last build of the current build definition will be used when comparing policy values. If the drop-down list is empty, please click the refresh icon to reload the list of available build definitions. The drop-down list shows a maximum of 1000 build definitions. If your definition is not visible, please enter the build definition ID manually. See [TFVC Feature Branches](https://github.com/MicrosoftPremier/VstsExtensions/blob/master/BuildQualityChecks/en-US/PullRequests.md#tfvc-feature-branches) for examples for when using a different build definition might be useful.

  **YAML: baseDefinitionId** - (Optional) Default is empty. Provide a build definition ID.

  **Note:** To always compare policy values to builds from the current build definition that target a specific branch, you need to choose the current build definition here and then select the appropriate values for *Repository* and *Branch (Git)*.

- <a name="baseRepo">**Repository:**</a> Select the repository that is used to search for baseline branches. The drop-down list is populated after you chose the *Build Definition* and will always contain the repository that the selected build definition is connected to. If the drop-down list is empty after selecting the *Build Definition*, please click the refresh icon to reload the repository information.

  **YAML: baseRepoId** - (Ignored) This parameter is ignored in YAML definitions.

  **Note:** When you change the *Build Definition* after selecting a repository, you might see a GUID value in the repository parameter. This is a refresh limitation of the build UI. Please select the repository again to correct this.

- <a name="baseBranch">**Branch (Git):**</a> Select the branch that should be used to search for the baseline build. If you do note set a value, the last build targeting the currently built branch will be used when comparing policy values, unless the build runs in the context of a pull request. In that case, the task looks for the last build targeting the pull request target branch if it cannot find a build for the currently built branch. See [Pull Request Policy](https://github.com/MicrosoftPremier/VstsExtensions/blob/master/BuildQualityChecks/en-US/PullRequests.md#configuring-build-quality-checks) for more information.

  If the drop-down list is empty after selecting the *Repository*, please click the refresh icon to reload the list of available branches. Branches are shown with their Git ref name, e.g. refs/heads/master or refs/heads/myTopicBranch.

  **YAML: baseBranchRef** - (Optional) Default is empty. Provide a Git branch ref (e.g., *refs/heads/master*).

  **Note:** When you change the *Build Definition* and *Repository* after selecting a branch, you need to select the branch again. Otherwise, a wrong branch name may be saved to the task configuration. This is a refresh limitation of the build UI.

  **Note:** Whenever you choose a different baseline branch, make sure that the [retention policy](https://www.visualstudio.com/en-us/docs/build/concepts/policies/retention) is configured to keep at least one successful build for your baseline branch! 

#### Reporting Options
If you are using multiple *Build Quality Checks* tasks within the same build, you may use the **Run Title** parameter to specify a title that is associated with a specific instance of the task. This will help distinguishing between the task results in the summary section. The run title is added to the subsection header in the summary in the format \<Build Job Name\> - \<Run Title\>.

**YAML: runTitle** - (Optional) Default is empty. Provide a run title.

#### Advanced
- <a name="noCertCheck">**Disable NodeJS certificate check:**</a> Check this option if your Team Foundation Server is using a self-signed or corporate SSL certificate and your build agent version is lower than 2.117.0. The option disables the certificate chain validation of NodeJS. Please read [here](https://github.com/MicrosoftPremier/VstsExtensions/blob/master/BuildQualityChecks/en-US/NodeJSAndCertificates.md) for details.

  **YAML: disableCertCheck** - (Optional) Default is *false*.

- <a name="createBuildIssues">**Log task results as build issues:**</a> When enabled, task results are logged as build warnings/errors in addition to the summary section output. This option is enabled by default.

  **YAML: createBuildIssues** - (Optional) Default is *true*.

### Task Variables
In addition to parameters visible in the task UI there are a few variables you can set to affect the tasks behavior:

- **BQC.ForceNewBaseline:** You can set this variable (usually when queueing a new build) to _true_ to force the policies to ignore an existing baseline build. This essentially creates a new baseline for future builds. This is especially helpful if there was a larger refactoring or redesign that resulted in a big change in code coverage and you need to establish a new base value. The default value is _false_.

- **BQC.LogRawData:** You can set this variable to _true_ to enable logging of raw JSON data read by the task. In most cases you should not use this option as it does increase the log size of the task and may show sensitive data in the log file. However, this data is very helpful in support scenarios. Note that this variable only takes effect if the variable _System.Debug_ is also set to _true_. The default value is _false_.

### Policy Results
Policy results are returned in various ways to make it easy for you to see and use them in different scenarios:

#### Extension Summary Section
The *Build Quality Checks* task creates its own summary section in the build summary view. This section displays all success, warning, and error messages for all activated policies. If you run a multi-configuration build, a subsection for each build job is created so you can see exactly which configuration might have quality issues.

![Policy Result](../assets/PolicyResult.png "Build Quality Checks Summary Section")

**Note:** Please see the [Limitations and Special Cases](https://github.com/MicrosoftPremier/VstsExtensions/blob/master/BuildQualityChecks/en-US/CodeCoveragePolicy.md) section of the *Code Coverage Policy* for possible issues with multi-configuration builds and code coverage.

#### Policy Result Variables
In addition, each policy you run creates an output variable that you can use to conditionally run other tasks in your pipeline (e.g., create a technical debt work item if quality metrics get worse). See policy documentation for more information.

#### Pull Request Status
If you run the task as part of a pull request validation build, each policy also publishes its result as a pull request status. With this you can keep your build from failing (i.e., set *Continue on error* option for *Build Quality Checks* task) for regular builds (e.g, nightly builds), but still prevent people from merging code of lesser quality into a protected branch through a pull request. See [Pull Requests and TFVC Feature Branches](https://github.com/MicrosoftPremier/VstsExtensions/blob/master/BuildQualityChecks/en-US/PullRequests.md) for more information.

**Important:** If you want to use the pull request features of the task, please ensure that your build account has the *Contribute to pull requests* permission! See [Git repository permissions](https://docs.microsoft.com/en-us/azure/devops/organizations/security/permissions?view=azure-devops&tabs=preview-page#git-repository-object-level) for more information. Depending on your *build job authorization scope* you need to either give that permission to your project build account (e.g., *YourProject Build Service (YourOrganization)*) or your project collection build account (e.g., *Project Collection Build Service (YourOrganization)*).

### Common Usage Scenarios

- [Pull Requests and TFVC Feature Branches](https://github.com/MicrosoftPremier/VstsExtensions/blob/master/BuildQualityChecks/en-US/PullRequests.md)

### FAQ
We have put together a list of frequently asked questions and answers in our [FAQ](https://github.com/MicrosoftPremier/VstsExtensions/blob/master/BuildQualityChecks/en-US/FAQ.md) document. If you feel we need to add a specific question to the list, feel free to send it to our [support](#support) address.

[Checklist board icon](https://www.vexels.com/vectors/png-svg/129767/checklist-board-icon) | Icon designed by Vexels.com
