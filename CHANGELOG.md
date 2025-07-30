# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.3.1] - 2025-07-30

### Changed
- Jira integration now only triggers when issues are found in the review
- Review notes are now always created, even when no issues are found

### Fixed
- Fixed exit code handling in script execution
- Improved README documentation

## [0.3.0] - 2025-07-24

### Added
- `--auto-merge` flag to automatically merge MRs that pass review
- Command line option to control automatic merging behavior
- Environment variable support for auto-merge configuration

### Changed
- Updated documentation for new auto-merge feature
- Improved error handling during merge operations

## [0.2.0] - 2025-07-23

### Added
- Support for checking existing discussions in GitLab MRs
- Logic to prevent creating duplicate discussions
- Automatic resolution of discussions for fixed issues
- Enhanced error handling for GitLab API calls
- Verification of resolvable discussions before attempting resolution

### Changed
- Updated `resolve_discussion` method to use the new GitLab API
- Improved extraction of positions from discussion notes
- Filtering of issues by severity before creating discussions

### Fixed
- Fixed handling of discussions without position
- Fixed verification of already resolved discussions
- Improved line mapping for discussions

## [0.1.4] - 2025-07-23

### Added
- Initial release of Gemini-based code reviewer for GitLab MRs
- Support for code review in English and Portuguese
- Jira integration for issue tracking
- JSON report generation
- Configuration via environment variables

[Unreleased]: https://github.com/alairjt/gitlab-gemini-reviewer/compare/v0.3.1...HEAD
[0.3.1]: https://github.com/alairjt/gitlab-gemini-reviewer/compare/v0.3.0...v0.3.1
[0.3.0]: https://github.com/alairjt/gitlab-gemini-reviewer/compare/v0.2.0...v0.3.0
[0.2.0]: https://github.com/alairjt/gitlab-gemini-reviewer/compare/v0.1.4...v0.2.0
[0.1.4]: https://github.com/alairjt/gitlab-gemini-reviewer/releases/tag/v0.1.4
