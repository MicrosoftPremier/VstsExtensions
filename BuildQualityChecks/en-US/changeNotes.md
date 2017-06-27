[Back to Overview](./overview.md)

# Build Quality Checks - Change Notes

#### 2.3.1
- Fixed duplicate logging of warning.

#### 2.3.0
- Added a warning if there are no code coverage data sets matching the specified build configuration and platform.

#### 2.2.0
- Added an option to prevent comparing policy values to partially succeeded builds.
- Added variables to configure waiting behavior for code coverage data.
- Updated screenshots to new build editor.
- Removed German documentation.

#### 2.1.2
- Added internal configuration options (for support purposes).

#### 2.1.1
- Fixed extension manifest to work with latest Visual Studio Marketplace behavior.

#### 2.1.0
- Fixed a race condition that sometimes resulted in zero code coverage for the current build.

#### 2.0.2
- Added back support for Team Foundation Server 2015 (was broken since v1.5.0). See [known issues](./overview.md#known-issues)
  for limitations on Team Foundation Server 2015!

#### 2.0.1
- Fixed a rare issue that could have caused the policies to report false results if the build steps of the definition was
  changed while a build of that definition was running.
  
#### 2.0.0
- You can now choose a baseline build definition and branch for the comparison of policy values. See
  [Baseline Task Parameters](https://github.com/almtcger/VstsExtensions/blob/master/BuildQualityChecks/en-US/overview.md#baseline)
  for more information.
- Policies will now pass by default, if the previous build is not available.
- Module filters have been removed from the code coverage policy. Please use run settings (or similar settings specific to your test
  and code coverage tools) to filter code coverage.

#### 1.5.1
- Fixed documentation to work with Visual Studio Marketplace.

#### 1.5.0
- Added support for multi-configuration builds.

#### 1.4.2
- Added additional checks for module filters.

#### 1.4.1
- Small change in code coverage data inspection.

#### 1.4.0
- Code coverage policy now supports multiple code coverage types (e.g., blocks, lines, branches).
- Fixed race conditions when used with task groups.
- Module filters for code coverage policy are now deprecated. The filters were more confusing than helpful and will be removed in
  the next major version of the extension. Please use run settings (or similar settings specific to your test and code coverage tools)
  to filter code coverage.
- The default value for module filters has been removed.

#### 1.3.0
- Fixed race condition due to asynchronous build information upload.
- Code coverage policy now works correctly for builds with multiple code coverage calculation steps (e.g., multiple VSTest tasks).
- Added information about tasks and modules that are evaluated by the policies to the task log.

#### 1.2.2
- Fixed bug in code coverage policy that resulted in the error **Cannot read property 'percentage' of undefined**.

#### 1.2.1
- Updated task to work without enabled OAuth token access.

#### 1.1.5
- First public version.