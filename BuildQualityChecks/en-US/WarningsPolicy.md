[Back to Overview](./overview.md) | [Limitations](#limitations-and-special-cases) | [Base Parameters](#base-parameters-of-the-build-warnings-policy) | [Task Warnings](#task-warnings-parameters) | [File Warnings](#file-warnings-parameters)

# Warnings Policy
Many software projects, especially older ones, that have grown over time end up with hundreds or thousands of build warnings.
Getting rid of these warnings through refactoring or cleaning up the code can be challenging, since new warnings get lost in
the existing ones. Choosing to treat warnings as errors is often not a feasible solution as this will force teams to bring the
warnings to zero or live with ever failing builds for a long time.

The *Warnings Policy* helps you keep track of your warnings and reduce them over time. This is done by failing the build
if the number of warnings exceeds a specific value or increases between builds.

### Policy result variable
The *Warnings Policy* creates an output variable named **WarningsPolicyResult**. If the policy passed successfully, the variable value is set to *passed*, otherwise it's set to *failed*.

### Pull Request Status
When running in a pull request validation build, the *Warnings Policy* publishes its result as a pull request status named `warnings-policy`. The full status policy name is `bqc/warnings-policy`. To distinguish between multiple *Build Quality Checks* instances, configure the [Run Title](https://github.com/MicrosoftPremier/VstsExtensions/blob/master/BuildQualityChecks/en-US/overview.md#reporting-options) and use the policy name `bqc-{runTitle}/warnings-policy`. If you run title contains whitespaces, please replace them with dashes (e.g., run title = "My Run Title" -> policy name = `bqc-my-run-title/warnings-policy`).

### Limitations and Special Cases
- **Warnings must be build issues**  
  The *Warnings Policy* only counts warnings that have been logged as build issues. If a task writes warning messages to
  its output but does not log them as build issues in the build system, the policy does not automatically pick those warnings up.
  You may use *Warning Filters (Tasks)* (see below) to add those warnings to the policy. 

### Base Parameters of the Build Warnings Policy

![Warnings Policy](../assets/WarningsPolicy.png "Base Parameters of the Warnings Policy")

- <a name="enabled">**Enabled:**</a> Use this checkbox to enable or disable the policy. If the policy is disabled, none of the following parameters is
  visible.

  **YAML: checkWarnings** - Default is *false*. Set to *true* to enable the option.

- <a name="failOption">**Fail Build On:**</a> Set this option to `Fixed Threshold` to fail the build if the number of warnings exceeds a specific value.
  This is useful if you want to allow a low number of warnings but keep them from getting out of hand, or if you want to follow a
  "no warnings policy" (i.e., *Warning Threshold* = 0). To bring down the number of warnings over time, set this option to
  `Previous Value`. This will fail the build if the number of warnings has increased since the previous build.

  **YAML: warningFailOption** - Default is *build*. Set to *build* for the `Previous Value` option or to *fixed* for the `Fixed Threshold` option.

- <a name="threshold">**Warning Threshold:**</a> Specify the number of warnings that must not be exceeded. This parameter is only visible if *Fail Build On*
  is set to `Fixed Threshold`.

  **YAML: warningThreshold** - Default is 0. Required if **warningFailOption* is set to *fixed*.

- <a name="forceFewer">**Force Fewer Warnings:**</a> Check this option if you want the current build to always have fewer warnings than the previous one. This
  option is only visible if *Fail Build On* is set to `Previous Value`.

  **YAML: forceFewerWarnings** - Default is *false*. Set to *true* to enable the option.

- <a name="allowWarningVariance">**Allow Variance:**</a> Check this option to allow a temporary increase of warnings. Enabling this option will allow the policy to pass
  even though the number of warnings has increased. The allowed increase is configured using the *Variance* parameter. This option is only available if the parameter
  *Fail Build On* is set to `Previous Value` and the parameter *Force Fewer Warnings* is not enabled.

  **YAML: allowWarningVariance** - Default is *false*. Set to *true* to enable the option.

- <a name="warningVariance">**Variance:**</a> Specify by how many the current warning count may exceed the previous value before the policy fails. Please be aware that
  the number of warnings may slowly but steadily increase from build to build if you allow a warning variance. Thus, you should keep this value as low as possible.

  **YAML: warningVariance** - Default is empty. Required if **allowWarningVariance** is set to *true*.

### Task Warnings Parameters

![Task Warnings Parameters](../assets/WarningsPolicyTasks.png "Task Warnings Parameters of the Warnings Policy")

- <a name="evaluateTaskWarnings">**Evaluate Task Warnings:**</a> Check this option to evaluate warnings from other pipeline tasks. In most cases you should leave this option checked. If you just want to evaluate warnings from log files (see *Evalutate File Warnings*), you need to uncheck this option. Otherwise, the task will always count all warnings from pipeline tasks in addition to warnings from log files. You need to either check this or the *Evaluate File Warnings* option.

  **YAML: evaluateTaskWarnings** - Default is *true*. Set to *false* to disable the option.

- <a name="taskFilters">**Task Filters:**</a> Since the build system can run all kinds of tasks during the build process and any of these tasks can create warnings, the *Warnings Policy* needs to know, which tasks it should look at and which to ignore. *Task Filters* takes a list of regular expressions (one per line). The policy will only look at build tasks that match one of the task filters. The matching is done against the name (e.g., _VSBuild_) and display name (e.g, _Build solution **/*.sln_) of each task. The default value `/^((vs|ms)build|ant(\\s+.+)?|gradle(w)?(\\s+.+)?|grunt|gulp|maven(\\s+.+)?|xamarin(android|ios)|xcode(\\s+.+)?|cmake|build\\s+.+)$/i` matches most of the standard build tasks in Azure DevOps Server/Services. This setting is only visible if *Evaluate Task Warnings* is checked.

  **YAML: warningTaskFilters** - Default is shown above. Set to one or more task filter values. Start multiple entires with a pipe sign and keep each entry on a separate indented line.

  **Note:** Regular expressions must use the [JavaScript RegExp](http://www.regular-expressions.info/javascript.html) syntax.

- <a name="warnFilters">**Warning Filters (Tasks):**</a> In some cases, you may want to analyze only specific types of warnings (e.g., unreachable code warnings, static code analysis warnings). *Warning Filters (Tasks)* allow you to do just that. Specify a list of regular expressions (one per line) that only match the types of warnings you are looking for and the policy will evaluate only those warnings. The policy result will show the total number of warnings as well as the number of filtered warnings. Keep in mind that *Warning Filters (Tasks)* analyze the log file of build tasks and does a simple text match. Thus, you need to make sure that your regular expressions match each warning only once. This setting is only visible if *Evaluate Task Warnings* is checked.

  **YAML: warningFilters** - Default is empty. Set to one or more filter values. Start multiple entries with a pipe sign and keep each entry on a separate indented line.
  
  **Examples:**
  - Unused variables (.NET): `/##\[warning\].+CS0219:/i`
  - Static code analysis (.NET): `/##\[warning\].+CA.+:/i`
  - StyleCop warnings (MSBuild and Roslyn): `/##\[warning\].+SA.+:/i`

  *Warning Filters (Tasks)* can also be used to count warnings if a build task does not log warnings as build issues, as long as the task logs
  the number of warnings to its log file. To do so, specify a warning filter that contains exactly **one** matching group that captures
  the number of warnings. The captured number will be added to the total warning count as well was the filtered warning count.

  **Example (based on _StyleCop Runner_ task):**
  - Log output: `StyleCop found [28448] violations warnings across [82] projects`
  - Filter value: `/\[(\d+)\]\s+violations/i`

  **Note:** Regular expressions must use the [JavaScript RegExp](http://www.regular-expressions.info/javascript.html) syntax.

- <a name="inclusiveFiltering">**Make Warning Filters Inclusive:**</a> Checking this option changes the behavior of *Warning Filters (Tasks)*. When unchecked (default), the policy only counts warnings matching the regular expressions listed in the *Warning Filters (Tasks)* parameter (*exclusive filtering*; see above). In some cases, though, you might want to count warnings that are not properly logged by build tasks in addition to the regular warnings (*inclusive filtering*). This can be achieved by activating the *Make Warning Filters Inclusive* option. This setting is only visible if *Evaluate Task Warnings* is checked.

  **YAML: inclusiveFiltering** - Default is *false*. Set to *true* to enable the option.

- <a name="statistics">**Show Warning Statistics:**</a> Enable this options to generate statistical information about warning changes. When enabled the policy not only shows the total number of warnings but also the changes in number of warnings per code file. To keep statistics short, only files with actual changes in the number of warnings are listed. If you combine this option with *Warning Filters (Tasks)*, the filters will be applied first and only matching warnings will appear in the warning statistics. This option is only visible if *Evaluate Task Warnings* is checked and *Evaluate File Warnings* is not checked.

  **YAML: showStatistics** - Default is *false*. Set to *true* to enable the option.
  
  ![Warning Statistics](../assets/WarningStatisticsResult.png "Policy Result with Warning Statistics")
  
  **Note:** Statistics currently only work for MSBuild builds (i.e., the *Visual Studio Build* and *MSBuild* tasks) and only on Team Foundation Server 2017 or later and Azure DevOps Services. If you need support for additional tasks, please let us know and preferably send us a sample log of the corresponding build task to <a href='&#109;&#97;&#105;&#108;&#116;&#111;&#58;&#112;&#115;&#103;&#101;&#114;&#101;&#120;&#116;&#115;&#117;&#112;&#112;&#111;&#114;&#116;&#64;&#109;&#105;&#99;&#114;&#111;&#115;&#111;&#102;&#116;&#46;&#99;&#111;&#109;'>&#112;&#115;&#103;&#101;&#114;&#101;&#120;&#116;&#115;&#117;&#112;&#112;&#111;&#114;&#116;&#64;&#109;&#105;&#99;&#114;&#111;&#115;&#111;&#102;&#116;&#46;&#99;&#111;&#109;</a>.

### File Warnings Parameters

![File Warnings Parameters](../assets/WarningsPolicyFiles.png "File Warnings Parameters of the Warnings Policy")

- <a name="evaluateFileWarnings">**Evaluate File Warnings:**</a> Check this option to evaluate warnings from arbitrary log files. If you want to evaluate warnings from log files, check this option and specify *File Warning Filters*. The task will then parse the log files based on the provided regular expressions and count every match as a warning. If you check this option in addition to the *Evaluate Task Warnings* option, the task counts all warnings from log files in addition to warnings from pipeline tasks. You need to either check this or the *Evaluate Task Warnings* option. This option is only available if *Fail Build On* is set to `Fixed Threshold`.

  **YAML: evaluateFileWarnings** - Default is *false*. Set to *true* to enable the option.

- <a name="warningFilesFolder">**Source Folder:**</a> Folder that contains the log files you want to analyze. If you leave this empty, the task looks for files in the root folder of the repo (same as if you had specified `$(Build.SourcesDirectory)`). This setting is only visible if *Evaluate File Warnings* is checked.

  **YAML: warningFilesFolder** - Default is empty.

- <a name="warningFiles">**Files:**</a> (Required) File paths to include in the evaluation. Supports multiple lines of match patterns. See [file matching patterns reference](https://docs.microsoft.com/en-us/azure/devops/pipelines/tasks/file-matching-patterns?view=azure-devops) for more information about supported patterns. This setting is only visible if *Evaluate File Warnings* is checked.

  **YAML: warningFiles** - (Required) Default is empty. Set to one or more file matching patterns. Start multiple entries with a pipe sign and keep each entry on a separate indented line.

- <a name="warningFileFilters">**Warning Filters (Files):**</a> (Required) Specify a list of regular expressions (one per line) that match the warnings in your log files. Make sure that your regular expressions match each warning only once, otherwise they will be counted multiple times. If you need separate regular expressions for specific log files, please use multiple instances of the *Build Quality Checks* task. This setting is only visible if *Evaluate File Warnings* is checked.

  **YAML: warningFilters** - (Required) Default is empty. Set to one or more filter values. Start multiple entries with a pipe sign and keep each entry on a separate indented line.

  **Note:** Regular expressions must use the [JavaScript RegExp](http://www.regular-expressions.info/javascript.html) syntax.