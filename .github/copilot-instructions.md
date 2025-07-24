<!-- Use this file to provide workspace-specific custom instructions to Copilot. For more details, visit https://code.visualstudio.com/docs/copilot/copilot-customization#_use-a-githubcopilotinstructionsmd-file -->

# GitHub Actions Repository Guidelines

This repository contains reusable GitHub Actions for .NET projects. When working on this codebase:

## Action Development
- Follow GitHub Actions composite action patterns
- Use semantic versioning for releases
- Include comprehensive input validation and error handling
- Add detailed logging with emoji indicators for better UX
- Support multiple operating systems (Linux, Windows, macOS) where applicable

## YAML Formatting
- Use consistent indentation (2 spaces)
- Group related steps with clear comment sections
- Use descriptive step names with emoji prefixes
- Include comprehensive input descriptions and defaults

## Testing Strategy
- Each action should have example workflows in `.github/workflows/`
- Include both success and failure scenarios
- Test with multiple .NET versions and configurations
- Validate Docker/TestContainers compatibility

## Documentation
- Keep README.md up to date with usage examples
- Include comprehensive input/output documentation
- Provide troubleshooting guides for common issues
- Add badges for build status and version info

## Best Practices
- Use `continue-on-error: true` for test steps to allow proper result evaluation
- Always upload artifacts for test results and coverage data
- Include proper cleanup steps for Docker resources
- Use consistent naming conventions across all actions
