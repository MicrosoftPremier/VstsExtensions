# Build Quality Checks
The *Build Quality Checks* task allows you to add quality gates to your build process.

### Adding the Task to a Build Definition
The *Build Quality Checks* task needs to be placed after the tasks it should inspect. In a Visual Studio build definition, e.g., an
appropriate place would be after build, test, and symbol indexing/publishing. This ensures that, even if the policy breaks the
build, you still get test results as well as the compile output and symbols.

![Task Placement](../assets/AddTask.png "Proper placement of the Build Quality Checks task")

### Change Notes
You can find the changes notes for this task [here](https://github.com/almtcger/VstsExtensions/blob/master/BuildQualityChecks/en-US/changeNotes.md).

### Support
If you need help with the extension or run into issues, please contact us at <a href='&#109;&#97;&#105;&#108;&#116;&#111;&#58;&#112;&#115;&#103;&#101;&#114;&#101;&#120;&#116;&#115;&#117;&#112;&#112;&#111;&#114;&#116;&#64;&#109;&#105;&#99;&#114;&#111;&#115;&#111;&#102;&#116;&#46;&#99;&#111;&#109;'>&#112;&#115;&#103;&#101;&#114;&#101;&#120;&#116;&#115;&#117;&#112;&#112;&#111;&#114;&#116;&#64;&#109;&#105;&#99;&#114;&#111;&#115;&#111;&#102;&#116;&#46;&#99;&#111;&#109;</a>.

## Warnings Policy
Many software projects, especially older ones, that have grown over time end up with hundreds or thousands of build warnings.
Getting rid of these warnings through refactoring or cleaning up the code can be challenging, since new warnings get lost in
the existing ones. Choosing to treat warnings as errors is often not a feasible solution as this will force teams to bring the
warnings to zero or live with ever failing builds for a long time.

The *Warnings Policy* helps you keep track of your warnings and reduce them over time. This is done by failing the build
if the number of warnings exceeds a specific value or increases between builds.

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

- **Task Filters:** Since the build system can run all kinds of tasks during the build process and any of these tasks can create
warnings, the *Warnings Policy* needs to know, which tasks it should look at and which to ignore. *Task Filters* takes a list of
regular expressions (one per line). The policy will only look at build tasks that match one of the task filters. The matching is done by
looking at the timeline name of each task, which is displayed below the task name in the build definition. The default value
`/^(((android|xcode|gradlew)\\s+)?build|ant|maven|cmake|gulp)/i` matches most of the standard build tasks in Team Foundation Server/Visual
Studio Team Services. **Note:** Regular expressions must use the JavaScript RegExp syntax. Click
[here](http://www.regular-expressions.info/javascript.html) for more information.

## Code Coverage Policy
Most teams that do unit testing as part of their development calculate code coverage during test execution. While code coverage should
never be used as the only metric for test/code quality, it is an easy to use indicator that shows if the team keeps up with automated
testing while developing new code.

The *Code Coverage Policy* allows breaking the build if code coverage falls below a certain value or decreases between builds.

### Parameters of the Code Coverage Policy

![Code Coverage Policy](../assets/CodeCoveragePolicy.png "Parameters of the Code Coverage Policy")

- **Enabled:** Use this checkbox to enable or disable the policy. If the policy is disabled, none of the following parameters is
visible.

- **Fail Build On:** Set this option to `Fixed Threshold` to fail the build if the code coverage value falls below a specific value.
This is useful if you want to allow some variance in code coverage but always keep a minimum coverage. If you set this option to
`Previous Value`, the build will fail, whenever the code coverage falls below that of the previous build.

- **Code Coverage Threshold:** Specify the minimum code coverage value in percentage terms. This parameter is only visible if
*Fail Build On* is set to `Fixed Threshold`.

- **Force Coverage Improvement:** Check this option if you want the current build to always have higher code coverage than the previous one.
This option is only visible if *Fail Build On* is set to `Previous Value`.

- **Upper Threshold:** Specify the upper threshold for code coverage improvements. It is generally not recommended to strive for 100% code coverage,
as this would force you to test even trivial code (e.g., getters/setters). Set this parameter to a reasonable high value (e.g., 70%-80%). The build
will fail as long as the code coverage stays below this value and will pass, as soon as it is reached or exceeded. This parameter is only visible if
the option *Force Coverage Improvement* is checked.

- **Delta Type:** Set this option to `Percentage Value` if the comparison between the current and previous code coverage value should be based
on the percentage value of code coverage. If you set this otion to `Absolute Value`, the absolute number of covered blocks will be used during
comparison.

- **Module Filters:** By default, the policy checks the aggregated code coverage of all modules that have been analyzed during the test runs.
*Module Filters* takes a list of regular expressions (one per line) that allow you to limit the policy only to specific modules. The default value
`/^(?!.*test)/i` excludes all modules that have the term *test* in their name, which matches the recommended naming of test assemblies in .NET.
**Note:** Regular expressions must use the JavaScript RegExp syntax. Click [here](http://www.regular-expressions.info/javascript.html) for more
information.

[Checklist board icon](https://www.vexels.com/vectors/png-svg/129767/checklist-board-icon) | Icon designed by Vexels.com
