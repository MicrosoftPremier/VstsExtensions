[Back to Overview](./overview.md) | [Limitations](#limitations-and-special-cases) | [Parameters](#parameters-of-the-code-coverage-policy)

# Code Coverage Policy
Most teams that do unit testing as part of their development calculate code coverage during test execution. While code coverage should
never be used as the only metric for test/code quality, it is an easy to use indicator that shows if the team keeps up with automated
testing while developing new code.

The *Code Coverage Policy* allows breaking the build if code coverage falls below a certain value or decreases between builds.

### Limitations and Special Cases
- **Using Test Impact Analysis (TIA)**  
  We recently announced [Test Impact Analysis](https://blogs.msdn.microsoft.com/visualstudioalm/2017/03/02/accelerated-continuous-testing-with-test-impact-analysis-part-1/)
  that helps accelerating continuous testing. While you can technically use the *Code Coverage Policy* together with TIA, we do not recommend
  it. TIA will automatically detect the tests that need to be run based on code changes and only executes those relevant tests. Thus, code
  coverage values might vary greatly between builds, rendering the policy essentially useless.

- **Code Coverage and Multi-Configuration Builds**  
  Multi-configuration builds trigger multiple build jobs (based on the *Multipliers* option) from a single build. This is mostly used for
  building combinations of different configurations (e.g., debug, release) and platforms (e.g., x86, x64) in parallel, although it may be
  used with any combination of build variables. If you use code coverage with multi-config builds, please mind the behavior of the build
  system as described here:

  - **Visual Studio Test**  
    When you run multiple VSTest tasks with code coverage enabled, coverage values will be aggregated into a single value, unless you run
    the tests with different configurations (i.e., debug/release) and/or platforms (i.e., any cpu/x86/x64). Running multiple build jobs with
    the same configuration/platform in a single build will lead to undefined policy results depending on the speed and configuration of your
    build.
  - **3<sup>rd</sup> Party Test Runners**  
    When you run multiple 3<sup>rd</sup> party test runners with code coverage enabled (e.g., Maven with JaCoCo), every test run will overwrite
    previously published code coverage results. Therefore, running multi-config builds with 3<sup>rd</sup> party test runners will most likely lead
    to unexpected code coverage summary data as well as unexpected policy results.

### Parameters of the Code Coverage Policy

![Code Coverage Policy](../assets/CodeCoveragePolicy.png "Parameters of the Code Coverage Policy")

- <a name="enabled">**Enabled:**</a> Use this checkbox to enable or disable the policy. If the policy is disabled, none of the following parameters is
  visible.

  **YAML: checkCoverage** - Default is *false*. Set to *true* to enable the option.

- <a name="failOption">**Fail Build On:**</a> Set this option to `Fixed Threshold` to fail the build if the code coverage value falls below a specific value.
  This is useful if you want to allow some variance in code coverage but always keep a minimum coverage. If you set this option to
  `Previous Value`, the build will fail, whenever the code coverage falls below that of the previous build.

  **YAML: coverageFailOption** - Default is *build*. Set to *build* for the `Previous Value` option or to *fixed* for the `Fixed Threshold` option.

- <a name="coverType">**Coverage Type:**</a> Select the type of code coverage you want to evaluate. Most code coverage tools calculate coverage based on
  several different elements of your code. For example, the *Visual Studio Test* task evaluates how many lines of code as well as how
  many code blocks have been covered by tests. Other tools like *Cobertura* use different coverage types like branch coverage, which is
  similar to block coverage. We recommend choosing `Block Coverage` together with the *Visual Studio Test* task or `Branch Coverage`
  for code coverage tools that support calculating branch coverage, as those coverage values are more exact than others. If block and
  branch coverage is not available, use `Line Coverage`. For third party code coverage tools like *JaCoCo*, which support additional
  coverage types, select `Custom Coverage` and specify the coverage type name (see below).

  **YAML: coverageType** - Default is *blocks*. Set to *blocks* for the `Block Coverage` option, *lines* for the `Line Coverage` option, *branches* for the `Branch Coverage` option, or *custom* for the `Custom Coverage` option.

- <a name="coverTypeName">**Coverage Type Name:**</a> Specify the name of the custom coverage type you want to evaluate. If you are unsure about the exact
  coverage type name, look at the code coverage section of the build summary page. The coverage type name must match one of the coverage
  types displayed in this section. This parameter is only visible if *Coverage Type* is set to `Custom Coverage`.

  **YAML: customCoverageType** - Default is empty. Required if **coverageType** is set to *custom*.

- <a name="threshold">**Code Coverage Threshold:**</a> Specify the minimum code coverage value in percentage terms. This parameter is only visible if
  *Fail Build On* is set to `Fixed Threshold`.

  **YAML: coverageThreshold** - Default is 60.

- <a name="forceImprove">**Force Coverage Improvement:**</a> Check this option if you want the current build to always have higher code coverage than the previous one.
  This option is only visible if *Fail Build On* is set to `Previous Value`.

  **YAML: forceCoverageImprovement** - Default is *false*. Set to *true* to enable the option.

- <a name="upperThreshold">**Upper Threshold:**</a> Specify the upper threshold for code coverage improvements. It is generally not recommended to strive for 100%
  code coverage, as this would force you to test even trivial code (e.g., getters/setters). Set this parameter to a reasonable high value (e.g., 70%-80%). The build
  will fail as long as the code coverage stays below this value and will pass, as soon as it is reached or exceeded. This parameter is only visible if
  the option *Force Coverage Improvement* is checked.

  **YAML: coverageUpperThreshold** - Default is 80.

- <a name="ignoreDecreaseAboveUpperThreshold">**Ignore Decrease Above Threshold:**</a> Uncheck this option to fail the build if your code coverage value decreases even though it stays above the configured *Upper Threshold*. This option is only visible if
  the option *Force Coverage Improvement* is checked.

  **YAML: ignoreDecreaseAboveUpperThreshold** - Default is *true*.

- <a name="allowCoverageVariance">**Allow Variance:**</a> Check this option to allow a temporary decrease of code coverage. Enabling this option will allow the policy
  to pass even though the code coverage value has decreased. The allowed decrease is configured using the *Variance* parameter. This option is only available if the parameter *Fail Build On* is set to `Previous Value` and the parameter *Force Coverage Improvement* is not enabled.

  **YAML: allowCoverageVariance** - Default is *false*. Set to *true* to enable the option.

- <a name="coverageVariance">**Variance:**</a> Specify by how much the current code coverage may fall below the previous value before the policy fails. Please be
  aware that the code coverage may slowly but steadily decrease from build to build if you allow a code coverage variance. Thus, you should keep this value as low as possible.

  **YAML: coverageVariance** - Default is empty. Required if **allowCoverageVariance** is set to *true*.

- <a name="coverageDeltaType">**Delta Type:**</a> Set this option to `Percentage Value` if the comparison between the current and previous code coverage value should be based
  on the percentage value of code coverage. If you set this option to `Absolute Value`, the absolute number of covered blocks will be used during
  comparison.

  **YAML: coverageDeltaType** - Default is *absolute*. Set to *absolute* for the `Absolute Value` option, or *percentage* for the `Percentage Value` option.

- <a name="coveragePrecision">**Precision:**</a> The number of significant decimals used when comparing coverage percentage values. The default precision is 4, thus, the policy recognizes coverage value changes of 0.0001 % (e.g., one block in a million). If you want to use higher or lower precision, provide a higher or lower number of significant decimals.

  **YAML: coveragePrecision** - Default is *4*.

  **Note:** When you allow *variance*, the task uses the highest precision specified by either the *precision* or the *variance* parameter. E.g., when you specify a precision of 4 but a variance of 0.00001, the precision is taken from the variance parameter since it has more significant decimals than specified by the precision parameter.

- <a name="config">**Configuration:**</a> Specify the configuration for which code coverage should be checked. Usually, one build compiles and tests just a single
  configuration (e.g., debug). Thus, the empty default value should be suitable for most situations. If you compile and test multiple configurations in
  a single build job or have a multi-configuration build, either specify a specific configuration, use the variable `$(BuildConfiguration)` (esp. for
  multi-config builds), or specify no value to check the aggregated code coverage of all configurations.

  **YAML: buildConfiguration** - Default is empty.

  **Note:** If you configure this parameter, make sure its value matches the value of the _Build configuration_ parameter in the _Visual Studio Test_
  task's _Reporting options_ section! Otherwise the policy will not pick up the generated code coverage values.

- <a name="platform">**Platform:**</a> Specify the platform for which code coverage should be checked. Usually, one build compiles and tests just a single
  platform (e.g., any cpu). Thus, the empty default value should be suitable for most situations. If you compile and test multiple platforms in a
  single build job or have a multi-configuration build, either specify a specific platform, use the variable `$(BuildPlatform)` (esp. for multi-config
  builds), or specify no value to check the aggregated code coverage of all platforms.

  **YAML: buildPlatform** - Default is empty.

  **Note:** If you configure this parameter, make sure its value matches the value of the _Build platform_ parameter in the _Visual Studio Test_
  task's _Reporting options_ section! Otherwise the policy will not pick up the generated code coverage values.

- <a name="explicitFilter">**Force Filter:**</a> Check this option to force the policy to only evaluate coverage data associated with the configured _Configuration_
  and _Platform_. If this option is not checked, the policy aggregates coverage data from all test runs unless both _Configuration_ and _Platform_ are specified. This
  is usually a good default value unless you have multiple code coverage tools like the _Visual Studio Test_ task and _Cobertura_. In such a scenario you have to
  check this option and leave both _Platform_ and _Configuration_ empty when you only want to evaluate _Cobertura_ coverage data. Otherwise, the policy would
  evaluate the aggregated values from _Visual Studio Test_ and _Cobertura_ resulting in too high coverage values.

  **YAML: explicitFilter** - Default is *false*. Set to *true* to enable the option.

  **Note:** If the policy cannot find any code coverage data associated with the configured *Configuration* and *Platform* it will fail.