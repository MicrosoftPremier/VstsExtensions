[Back to Overview](./overview.md)

# Post Build Cleanup - Change Notes

#### 3.0.2
- Fix links to documentation.

#### 3.0.1
- Fix typo in code.

#### 3.0.0
- Added support for build phases.
- Added support for YAML builds.
- Moved official documentation to https://github.com/MicrosoftPremier (thanks for the feedback, Jens J.).
- Removed support for Team Foundation Server 2015 due to incompatibility with the latest vsts-node-api.

--------------------------------------------------------------------------------
**Note:** Version 2.x is the last version compatible with Team Foundation Server 2015!

#### 2.1.2
- Fixed an issue with NodeJS module loading that prevented the task from running on Linux/Unix.

#### 2.1.0
- Added support for disabling NodeJS certificate check for the task. See [advanced settings](./overview.md#advanced) for more information.

#### 2.0.3
- Updated docs to new build UI.

#### 2.0.2
- Added internal configuration options (for support purposes).

#### 2.0.1
- Fixed extension manifest to work with latest Visual Studio Marketplace behavior.

#### 2.0.0
- Added partial support for Team Foundation Server 2015 (see [Support for Team Foundation Server 2015](./overview.md#support-for-team-foundation-server-2015)
  for more information).

#### 1.0.1
- Updated task requirements. Due to a missing feature in Team Foundation Server 2015, the task only works on Team Foundation
  Server 2017 and later as well as Visual Studio Team Services.

#### 1.0.0
- First public version.