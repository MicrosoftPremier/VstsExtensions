# Build Quality Checks - Change Notes

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