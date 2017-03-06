# Build Quality Checks
The *Build Quality Checks* task allows you to add quality gates to your build process.

### Change Notes
You can find the changes notes for this task [here][Notes].

[Notes]: https://github.com/almtcger/VstsExtensions/blob/master/BuildQualityChecks/en-US/changeNotes.md

### Known Issues
- If you run your build agent behind a proxy server, the task will fail and you will see `connect ETIMEDOUT` somewhere in the
  task log, even if you configured the agent correctly as described [here][Proxy]. To fix this issue, create two environment variables
  called `HTTP_PROXY` and `HTTPS_PROXY` (either in system scope or the user scope of your agent service account) and set their
  value to your proxy server address including the port (e.g., http://myproxy:8080).  
  **Note:** Build tasks currently do not support proxy authentication.
- The result section on the build summary page displays a wrong icon (trash can instead of red X) for failed policies on Team Foundation
  Server 2015. We will not fix this issue, as it does not occur on Team Foundation Server 2017 or Visual Studio Team Services.

[Proxy]: https://github.com/Microsoft/vsts-agent/blob/master/docs/start/proxyconfig.md

### Support
If you need help with the extension or run into issues, please contact us at <a href='&#109;&#97;&#105;&#108;&#116;&#111;&#58;&#112;&#115;&#103;&#101;&#114;&#101;&#120;&#116;&#115;&#117;&#112;&#112;&#111;&#114;&#116;&#64;&#109;&#105;&#99;&#114;&#111;&#115;&#111;&#102;&#116;&#46;&#99;&#111;&#109;'>&#112;&#115;&#103;&#101;&#114;&#101;&#120;&#116;&#115;&#117;&#112;&#112;&#111;&#114;&#116;&#64;&#109;&#105;&#99;&#114;&#111;&#115;&#111;&#102;&#116;&#46;&#99;&#111;&#109;</a>.

### Adding the Task to a Build Definition
The *Build Quality Checks* task needs to be placed after the tasks it should inspect. In a Visual Studio build definition, e.g., an
appropriate place would be after build, test, and symbol indexing/publishing. This ensures that, even if the policy breaks the
build, you still get test results as well as the compile output and symbols.

![Task Placement](../assets/AddTask.png "Proper placement of the Build Quality Checks task")

### Policies
The *Build Quality Checks* task currently supports two policies (click the link for details):

- **[Warnings Policy][WarnPol]** - Allows you to fail builds based on the number of build warnings.
- **[Code Coverage Policy][CoveragePol]** - Allows you to fail builds based on the code coverage value of your tests.

### Common Usage Scenarios
*Coming soon*

### Policy Results
The *Build Quality Checks* task creates its own summary section in the build summary view. This section displays all success,
warning, and error messages for all activated policies. If you run a multi-configuration build, a subsection for each build
job is created so you can see exactly which configuration might have quality issues.

![Policy Result](../assets/PolicyResult.png "Build Quality Checks Summary Section")

**Note:** Please see the [Limitations and Special Cases][CoveragePol] section of the *Code Coverage Policy* for possible
issues with multi-configuration builds and code coverage.

[WarnPol]: https://github.com/almtcger/VstsExtensions/blob/master/BuildQualityChecks/en-US/WarningsPolicy.md
[CoveragePol]: https://github.com/almtcger/VstsExtensions/blob/master/BuildQualityChecks/en-US/CodeCoveragePolicy.md

[Checklist board icon](https://www.vexels.com/vectors/png-svg/129767/checklist-board-icon) | Icon designed by Vexels.com
