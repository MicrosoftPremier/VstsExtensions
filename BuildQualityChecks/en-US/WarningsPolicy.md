[Back to Overview](./overview.md)

# Warnings Policy
Many software projects, especially older ones, that have grown over time end up with hundreds or thousands of build warnings.
Getting rid of these warnings through refactoring or cleaning up the code can be challenging, since new warnings get lost in
the existing ones. Choosing to treat warnings as errors is often not a feasible solution as this will force teams to bring the
warnings to zero or live with ever failing builds for a long time.

The *Warnings Policy* helps you keep track of your warnings and reduce them over time. This is done by failing the build
if the number of warnings exceeds a specific value or increases between builds.

### Limitations and Special Cases
- **Warnings must be build issues**  
  The *Warnings Policy* only counts warnings that have been logged as build issues. If a task writes warning messages to
  its output but does not log them as build issues in the build system, the policy is not able to see them. 

### Parameters of the Build Warnings Policy

![Warnings Policy](../assets/WarningsPolicy.png "Parameters of the Warnings Policy")

- **Enabled:** Use this checkbox to enable or disable the policy. If the policy is disabled, none of the following parameters is
  visible.

- **Fail Build On:** Set this option to `Fixed Threshold` to fail the build if the number of warnings exceeds a specific value.
  This is useful if you want to allow a low number of warnings but keep them from getting out of hand, or if you want to follow a
  "no warnings policy" (i.e., *Warning Threshold* = 0). To bring down the number of warnings over time, set this option to
  `Previous Value`. This will fail the build if the number of warnings has increased since the previous build.

- **Warning Threshold:** Specify the number of warnings that must not be exceeded. This parameter is only visible if *Fail Build On*
  is set to `Fixed Threshold`.

- **Force Fewer Warnings:** Check this option if you want the current build to always have fewer warnings than the previous one. This
  option is only visible if *Fail Build On* is set to `Previous Value`.

- **Warning Filters:** In some cases, you may want to analyze only specific types of warnings (e.g., unreachable code warnings, static code
  analysis warnings). *Warning Filters* allow you to do just that. Specify a list of regular expressions (one per line) that only match
  the types of warnings you are looking for and the policy will evaluate only those warnings. The policy result will show the total number
  of warnings as well as the number of filtered warnings. Keep in mind that *Warning Filters* analyze the log file of build tasks and does
  a simple text match. Thus, you need to make sure that your regular expressions match each warning only once.
  
  **Examples:**
  - Unused variables (.NET): `/##\[warning\].+CS0219:/i`
  - Static code analysis (.NET): `/##\[warning\].+CA.+:/i`
  - StyleCop warnings (MSBuild and Roslyn): `/##\[warning\].+SA.+:/i`

  *Warning Filters* can also be used to count warnings if a build task does not log warnings as build issues, as long as the task logs
  the number of warnings to its log file. To do so, specify a warning filter that contains exactly **one** matching group that captures
  the number of warnings. The captured number will be added to the total warning count as well was the filtered warning count.

  **Example (based on _StyleCop Runner_ task:**
  - Log output: `StyleCop found [28448] violations warnings across [82] projects`
  - Filter value: `/\[(\d+)\]\s+violations/i`

  **Note:** Regular expressions must use the JavaScript RegExp syntax. Click [here](http://www.regular-expressions.info/javascript.html)
  for more information.

- **Task Filters:** Since the build system can run all kinds of tasks during the build process and any of these tasks can create
  warnings, the *Warnings Policy* needs to know, which tasks it should look at and which to ignore. *Task Filters* takes a list of
  regular expressions (one per line). The policy will only look at build tasks that match one of the task filters. The matching is done by
  looking at the timeline name of each task, i.e., the name displayed in the list on the left hand side of the build summary view. If you
  change the display name of a task in your build definition, make sure to update the task filters appropriately. The default value
  `/^(((android|xcode|gradlew)\\s+)?build|ant|maven|cmake|gulp)/i` matches most of the standard build tasks in Team Foundation Server/Visual
  Studio Team Services.

  **Note:** Regular expressions must use the JavaScript RegExp syntax. Click [here](http://www.regular-expressions.info/javascript.html)
  for more information.
