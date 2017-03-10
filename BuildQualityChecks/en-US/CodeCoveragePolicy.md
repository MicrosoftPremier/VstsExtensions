[Back to Overview](./overview.md)

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
  used with any combintaion of build variables. If you use code coverage with multi-config builds, please mind the behavior of the build
  system as described here:

  - **Visual Studio Test**  
    When you run mutliple VSTest tasks with code coverage enabled, coverage values will be aggregated into a single value, unless you run
    the tests with different configurations (i.e., debug/release) and/or platforms (i.e., any cpu/x86/x64). Running multiple build jobs with
    the same configuration/platform in a single build will lead to undefined policy results depending on the speed and configuration of your
    build.
  - **3rd Party Test Runners**  
    When you run multiple 3rd party test runners with code coverage enabled (e.g., Maven with JaCoCo), every test run will overwrite
    previously published code coverage results. Therefore, running multi-config builds with 3rd party test runners will most likely lead
    to unexpected code coverage summary data as well as unexpected policy results.

### Parameters of the Code Coverage Policy

![Code Coverage Policy](../assets/CodeCoveragePolicy.png "Parameters of the Code Coverage Policy")

- **Enabled:** Use this checkbox to enable or disable the policy. If the policy is disabled, none of the following parameters is
  visible.

- **Fail Build On:** Set this option to `Fixed Threshold` to fail the build if the code coverage value falls below a specific value.
  This is useful if you want to allow some variance in code coverage but always keep a minimum coverage. If you set this option to
  `Previous Value`, the build will fail, whenever the code coverage falls below that of the previous build.

- **Coverage Type:** Select the type of code coverage you want to evaluate. Most code coverage tools calculate coverage based on
  several different elements of your code. For example, the *Visual Studio Test* task evaluates how many lines of code as well as how
  many code blocks have been covered by tests. Other tools like *Cobertura* use different coverage types like branch coverage, which is
  similar to block coverage. We recommend choosing `Block Coverage` together with the *Visual Studio Test* task or `Branch Coverage`
  for code coverage tools that support calculating branch coverage, as those coverage values are more exact than others. If block and
  branch coverage is not available, use `Line Coverage`. For third party code coverage tools like *JaCoCo*, which support additional
  coverage types, select `Custom Coverage` and specify the coverage type name (see below).

- **Coverage Type Name:** Specify the name of the custom coverage type you want to evaluate. If you are unsure about the exact
  coverage type name, look at the code coverage section of the build summary page. The coverage type name must match one of the coverage
  types displayed in this section. This parameter is only visible if *Coverage Type* is set to `Custom Coverage`.

- **Code Coverage Threshold:** Specify the minimum code coverage value in percentage terms. This parameter is only visible if
  *Fail Build On* is set to `Fixed Threshold`.

- **Force Coverage Improvement:** Check this option if you want the current build to always have higher code coverage than the previous one.
  This option is only visible if *Fail Build On* is set to `Previous Value`.

- **Upper Threshold:** Specify the upper threshold for code coverage improvements. It is generally not recommended to strive for 100% code coverage,
  as this would force you to test even trivial code (e.g., getters/setters). Set this parameter to a reasonable high value (e.g., 70%-80%). The build
  will fail as long as the code coverage stays below this value and will pass, as soon as it is reached or exceeded. This parameter is only visible if
  the option *Force Coverage Improvement* is checked.

- **Delta Type:** Set this option to `Percentage Value` if the comparison between the current and previous code coverage value should be based
  on the percentage value of code coverage. If you set this option to `Absolute Value`, the absolute number of covered blocks will be used during
  comparison.

- **Configuration:** Specify the configuration for which code coverage should be checked. Usually, one build compiles and tests just a single
  configuration (e.g., debug), which is defined by the build variable `$(BuildConfiguration)`. Thus, the default value should be suitable for most
  situations, esp. for multi-configuration builds that use *BuildConfiguration* as a multiplier. If you compile and test multiple configurations
  in a single build job, either specify a specific configuration here, or specify no value to check the aggregated code coverage of all configurations.

- **Platform:** Specify the platform for which code coverage should be checked. Usually, one build compiles and tests just a single
  platform (e.g., any cpu), which is defined by the build variable `$(BuildPlatform)`. Thus, the default value should be suitable for most
  situations, esp. for multi-configuration builds that use *BuildPlatform* as a multiplier. If you compile and test multiple platforms
  in a single build job, either specify a specific platform here, or specify no value to check the aggregated code coverage of all platforms.
