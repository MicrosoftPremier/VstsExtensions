# Build Quality Checks - Change Notes

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