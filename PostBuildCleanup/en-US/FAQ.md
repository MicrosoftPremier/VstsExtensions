[Back to Overview](./overview.md)

# Frequently Asked Questions

**Q: Why is the task not working when used in a build phase?**
**A:** The current version of the *vsts-node-api* does not support build phases. Since the *Post Build Cleanup* task uses that library to read information about the build
definition, it only recognizes the first build phase. Thus, the task only works correctly when placed in the first build phase. We will fix this issue as soon as possible.

**Q: Why is the task failing when used in a YAML build definition?**
**A:** Similar to the previous question ths issue is related to missing support for YAML definitions in the current version of the *vsts-node-api*. Until we have a fix for the
issue, please keep using "standard" build definitions if you need the *Post Build Cleanup* task.