# Post Build Cleanup
Did you ever run out of disk space on your build machine, because there were so many sources and binaries from previous build
runs? Then this extension is for you!

By default, the clean option of a build definition deletes files **before** the build starts. Thus, every build leaves behind
sources and binaries on your build machine, and those files accumulate and eat up your disk space. If you're not running
incremental builds, those files are not needed after the build has finished.

The *Post Build Cleanup* task deletes unwanted files from your build agent, **after** your build has run, thus, saving precious
disk space.

### Change Notes
You can find the changes notes for this task [here](https://github.com/almtcger/VstsExtensions/blob/master/PostBuildCleanup/en-US/changeNotes.md).

### Support
If you need help with the extension or run into issues, please contact us at <a href='&#109;&#97;&#105;&#108;&#116;&#111;&#58;&#112;&#115;&#103;&#101;&#114;&#101;&#120;&#116;&#115;&#117;&#112;&#112;&#111;&#114;&#116;&#64;&#109;&#105;&#99;&#114;&#111;&#115;&#111;&#102;&#116;&#46;&#99;&#111;&#109;'>&#112;&#115;&#103;&#101;&#114;&#101;&#120;&#116;&#115;&#117;&#112;&#112;&#111;&#114;&#116;&#64;&#109;&#105;&#99;&#114;&#111;&#115;&#111;&#102;&#116;&#46;&#99;&#111;&#109;</a>.

### Adding the Task to a Build Definition
The *Post Build Cleanup* task (in task category *Utility*) must always be the last (enabled) task in your build definition!

![Task Placement](../assets/AddTask.png "Proper placement of the Post Build Cleanup task")

### Behavior of the Task
The task behavior is directly linked to the clean options you select in your build definition's repository settings. If the
**Clean** option is unchecked, the task does nothing. Otherwise, the task behavior depends on the selected value for
**Clean options** and mimics the pre-build cleanup behavior:

- **Sources:** If this option is selected, the task does not delete any files from the agent in order to allow incremental
  gets of source files.

- **Sources and output directory:** If this option is selected, the task deletes all contents of the $(Build.BinariesDirectory).

- **Sources directory:** If this option is selected, the task deletes all contents of the $(Build.SourcesDirectory).

  **Note:** If your build uses a Git repository, a minimal portion of the .git folder will remain on disk. Those remaining files
  are needed by the build agent's built-in *Post Job Cleanup*.

- **All build directories:** If this option is selected, the task deletes all contents within the $(Agent.BuildDirectory) and
  recreates the internal folder structure (i.e., the subfolders a, s, and b).

Icon made by [Freepik](http://www.freepik.com "Freepik") from [www.flaticon.com](http://www.flaticon.com "Flaticon") is
licensed by [CC 3.0 BY](http://creativecommons.org/licenses/by/3.0/ "Creative Commons BY 3.0")
