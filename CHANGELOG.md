# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added
- **security-check action**: Comprehensive Docker image vulnerability scanning with Trivy
  - Configurable severity thresholds (CRITICAL, HIGH, MEDIUM, LOW, UNKNOWN)
  - Automatic GitHub annotations for all vulnerabilities
  - Detailed job summaries with vulnerability breakdowns
  - Artifact upload for scan results
  - Support for ignoring unfixed vulnerabilities
  - Configurable scan timeouts
- Initial release of `run-tests` action
- Support for .NET integration tests with TestContainers
- Code coverage collection with XPlat Code Coverage
- Automatic artifact upload for test results and coverage
- Codecov integration for coverage reporting
- Multi-OS support (Linux, Windows, macOS)
- Comprehensive logging with emoji indicators

### Features
- **run-tests action**: Complete integration test runner with TestContainers support
- **Docker integration**: Automatic Docker daemon setup and validation
- **Coverage reporting**: Optional code coverage collection and upload
- **Artifact management**: Automatic test result and coverage artifact uploads
- **Multi-configuration support**: Configurable .NET versions and build configurations

## [1.0.0] - 2025-07-20

### Added
- Initial repository structure
- `run-tests` action for .NET integration testing
- Example workflows and documentation
- MIT License
- Contributing guidelines
