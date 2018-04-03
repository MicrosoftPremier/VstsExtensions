[Back to Overview](./overview.md) | [General](#general) | [Warnings Policy](#warnings-policy) | [Code Coverage Policy](#code-coverage-policy)

# Frequently Asked Questions

## General
**Q: Can I have multiple *Build Quality Checks* in one build?**  
**A:** Yes. To better distinguish between the results of each instance use [run titles](./overview.md#reporting-options).

## Warnings Policy
**Q: Why does the policy report zero warnings although I see warnings in the build log?**  
**A:** The policy only picks up warnings that are reported as build issues (see [limitations](./WarningsPolicy.md#limitations-and-special-cases)) and cannot see warnings that are
only written to a build task's log file. You might be able to use [warning filters](./WarningsPolicy.md#warnFilters) to make the policy pick up those warnings as well.

**Q: Why does the policy report fewer warnings than the build summary?**  
**A:** The build summary shows all warnings that have been reoprted as build issues, while the policy only counts warnings from specific build tasks. Make sure that the
[task filters](./WarningsPolicy.md#taskFilters) match all relevant tasks in your build definition.

## Code Coverage Policy
**Q: Can I exclude specific files/folders or namespaces/modules/members from code coverage?**  
**A:** Yes, but not through the code coverage policy. To only calculate coverage for specific parts of your code, use [run settings](https://msdn.microsoft.com/en-us/library/jj159530.aspx)
within the *Visual Studio Test* task or similar configuration settings for 3<sup>rd</sup> party code coverage tools.