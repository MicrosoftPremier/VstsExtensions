[Back to Overview](./overview.md) | [General](#general) | [Warnings Policy](#warnings-policy) | [Code Coverage Policy](#code-coverage-policy)

# Frequently Asked Questions

## General
**Q: Can I have multiple *Build Quality Checks* in one build?**  
**A:** Yes. To better distinguish between the results of each instance use [run titles](./overview.md#reporting-options).

**Q: Why have there been so many security scope changes for the *Build Quality Checks* extension and how do they affect the security and privacy of my data?**  
**A:** [Security Scopes](https://docs.microsoft.com/en-us/azure/devops/extend/develop/manifest#scopes) are set by extension developers to request **desired** access to specific data of your Azure DevOps organization or server. Simply put, Azure DevOps creates a special security token for each extension (similar to a [PAT](https://docs.microsoft.com/en-us/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate)) and only grants the scopes that have been requested by the extension and approved upon extension installation or update. This ensures that an extension cannot simply access data it shouldn't have access to.

This general rule only applies to UI extensions, though (e.g., dashboard widgets, hub or hub group extensions, context menu extensions, work item form extensions, etc.). Pipeline tasks that are executed on an agent have an access token with all available scopes. Thus, pipeline tasks can **potentially** access all data in an Azure DevOps organization or on your server.

However, a security scope doesn't automatically grant access to the requested data. Access to data is **always** governed by Azure DevOps' permission system. For a UI extension, which is executed in the security context of the user using the extension, this essentially means that it can only do whatever the current user is allowed to do. In other words: if an extension request write access to code but the current user does have no access to code at all, the extension can neither read nor write code. For a build task this means the following:

There are two security contexts for a build task:
1. The context in which a task is executed.
2. The [build authorization scope](https://docs.microsoft.com/en-us/azure/devops/pipelines/build/options?view=azure-devops#build-job-authorization-scope) that you configure in your pipeline definition.

The first is defined by the account running the agent service. If you set up your private build agent correctly or if you use our hosted agents, this account does not have access to Azure DevOps at all. Thus, a task cannot simply open a network connection back to Azure DevOps and grab data unless it has specific credentials to do so. These can either be a [PAT](https://docs.microsoft.com/en-us/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate) or [Service Connection](https://docs.microsoft.com/en-us/azure/devops/pipelines/library/service-endpoints?view=azure-devops) that you'd have to explicitly provide, or the task can ask for the build's OAuth token, which moves it into the second security context.

Script tasks that want to access the build's OAuth token need to have an [explicit permission](https://docs.microsoft.com/en-us/azure/devops/pipelines/build/options#allow-scripts-to-access-the-oauth-token) for that. All packaged tasks (like the _Build Quality Checks_ task) can use the _Azure Pipelines Task Library_ to acquire this security token to connect back to Azure DevOps.

By default, a build's OAuth token only has limited permissions (read all source code in the current project or organization/collection, read and edit build information, trigger pipelines, create and edit work items). Unless you explicitly grant more permissions to the project or project collection build service account, you don't have to be worried about a build task arbitrarily accessing data.

## Warnings Policy
**Q: Why does the policy report zero warnings although I see warnings in the build log?**  
**A:** The policy only picks up warnings that are reported as build issues (see [limitations](./WarningsPolicy.md#limitations-and-special-cases)) and cannot see warnings that are
only written to a build task's log file. You might be able to use [warning filters](./WarningsPolicy.md#warnFilters) to make the policy pick up those warnings as well.

**Q: Why does the policy report fewer warnings than the build summary?**  
**A:** The build summary shows all warnings that have been reported as build issues, while the policy only counts warnings from specific build tasks. Make sure that the
[task filters](./WarningsPolicy.md#taskFilters) match all relevant tasks in your build definition.

**Q: How do warning statistics work and why is it reporting incorrect warning changes?**  
**A:** The detailed warning analysis within the statistics is based on a heuristic mechanism and might not be accurate. Since the *Build Quality Checks* task solely relies on log information, it doesn't have enough context per warning (e.g., method or class containing the warning) to accuarately find removed or new warnings. We still do our best and want to share the mechanics behind the analysis.

1. We first eliminate all warnings that exactly match a previous warning (i.e., same file, line, column, identifier, and message).
2. Then we search for partial matches, which are warnings for the same file, identifier, and message on a different line and/or column.
3. Those partial matches are clustered into groups that have moved in the same way (e.g., a block of ten warnings that has moved five lines down, which has most likely been caused by added code above the block of warnings), and we eliminate all clusters per file.
4. Finally, from the remaining partial matches we eliminate the ones that are closest to the original warning. This is the most inaccurate part of the heuristic that will result in wrong warning details if you restructure a code file completely.

All warnings that still remain after performing the above four steps are listed as either removed (if the warning existed in the previous build and we couldn't find a match in the current build) or added (if the warning only exists in the current build).

If you encounter too many inaccuracies or you believe you have an idea how to improve our heuristic, please contact us via email at <a href='&#109;&#97;&#105;&#108;&#116;&#111;&#58;&#112;&#115;&#103;&#101;&#114;&#101;&#120;&#116;&#115;&#117;&#112;&#112;&#111;&#114;&#116;&#64;&#109;&#105;&#99;&#114;&#111;&#115;&#111;&#102;&#116;&#46;&#99;&#111;&#109;'>&#112;&#115;&#103;&#101;&#114;&#101;&#120;&#116;&#115;&#117;&#112;&#112;&#111;&#114;&#116;&#64;&#109;&#105;&#99;&#114;&#111;&#115;&#111;&#102;&#116;&#46;&#99;&#111;&#109;</a>.

## Code Coverage Policy
**Q: Can I exclude specific files/folders or namespaces/modules/members from code coverage?**  
**A:** Yes, but not through the code coverage policy. To only calculate coverage for specific parts of your code, use [run settings](https://msdn.microsoft.com/en-us/library/jj159530.aspx)
within the *Visual Studio Test* task or similar configuration settings for 3<sup>rd</sup> party code coverage tools.