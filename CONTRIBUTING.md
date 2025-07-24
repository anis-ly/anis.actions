# Contributing to Anis Actions

Thank you for your interest in contributing to Anis Actions! This guide will help you get started.

## Development Environment Setup

1. **Fork the repository** and clone your fork:
   ```bash
   git clone https://github.com/your-username/anis.actions.git
   cd anis.actions
   ```

2. **Install dependencies** (if any):
   ```bash
   # No specific dependencies for GitHub Actions
   # But ensure you have Docker for testing TestContainers
   docker --version
   ```

## Project Structure

```
anis.actions/
├── .github/
│   ├── workflows/          # Example workflows
│   └── copilot-instructions.md
├── run-tests/              # Run tests action
│   └── action.yml
├── README.md
├── CHANGELOG.md
├── CONTRIBUTING.md
└── LICENSE
```

## Creating a New Action

1. **Create a new directory** for your action:
   ```bash
   mkdir my-new-action
   cd my-new-action
   ```

2. **Create `action.yml`** with the action definition:
   ```yaml
   name: 'My New Action'
   description: 'Description of what the action does'
   
   inputs:
     input-name:
       description: 'Description of the input'
       required: true
       default: 'default-value'
   
   runs:
     using: 'composite'
     steps:
       - name: 🚀 Step name with emoji
         shell: bash
         run: echo "Hello from my action!"
   ```

3. **Follow the guidelines**:
   - Use emoji prefixes in step names
   - Include comprehensive input descriptions
   - Add proper error handling
   - Support multiple operating systems when applicable

## Testing Your Changes

### Local Testing with Act

Use [act](https://github.com/nektos/act) to test workflows locally:

```bash
# Install act
curl https://raw.githubusercontent.com/nektos/act/master/install.sh | sudo bash

# Test a workflow
act -W .github/workflows/example.yml
```

### Manual Testing

1. **Create a test repository** with a sample .NET project
2. **Use your action** in a workflow
3. **Verify the behavior** across different scenarios

## Code Style Guidelines

### YAML Formatting
- Use **2 spaces** for indentation
- Use **descriptive step names** with emoji prefixes
- Group related steps with **comment sections**
- Include **comprehensive input descriptions**

### Emoji Usage
- 🔧 Setup/Configuration
- 📦 Package/Dependency management
- 🏗️ Building
- 🐳 Docker operations
- 🧪 Testing
- ✅ Validation/Success
- ❌ Failure/Error
- 📊 Status/Information
- 📥 Input/Download
- 📤 Output/Upload
- 🚀 Execution/Running

### Error Handling
- Use `continue-on-error: true` for steps that should not fail the workflow
- Include proper exit codes and error messages
- Provide helpful debugging information

## Documentation

### README Updates
- Update the main `README.md` with new actions
- Include usage examples
- Document all inputs and outputs
- Add troubleshooting information

### Changelog
- Update `CHANGELOG.md` following [Keep a Changelog](https://keepachangelog.com/en/1.0.0/) format
- Document breaking changes, new features, and bug fixes

## Submitting Changes

### Pull Request Process

1. **Create a feature branch**:
   ```bash
   git checkout -b feature/my-new-feature
   ```

2. **Make your changes** following the guidelines above

3. **Test your changes** thoroughly

4. **Update documentation** (README, CHANGELOG, etc.)

5. **Commit your changes**:
   ```bash
   git commit -am "feat: add new action for X"
   ```

6. **Push to your fork**:
   ```bash
   git push origin feature/my-new-feature
   ```

7. **Create a Pull Request** with:
   - Clear description of changes
   - Link to any related issues
   - Screenshots or examples if applicable

### Commit Message Format

We follow [Conventional Commits](https://www.conventionalcommits.org/):

- `feat:` New features
- `fix:` Bug fixes
- `docs:` Documentation changes
- `style:` Code style changes
- `refactor:` Code refactoring
- `test:` Test additions or modifications
- `chore:` Maintenance tasks

### Review Process

1. **Automated checks** must pass
2. **Code review** by maintainers
3. **Testing** in real-world scenarios
4. **Documentation review**

## Release Process

### Versioning

We use [Semantic Versioning](https://semver.org/):

- **MAJOR** version for incompatible API changes
- **MINOR** version for backward-compatible functionality additions
- **PATCH** version for backward-compatible bug fixes

### Tagging

After merging to main:

1. Update version in relevant files
2. Update CHANGELOG.md
3. Create and push tags:
   ```bash
   git tag v1.2.3
   git push origin v1.2.3
   ```

## Getting Help

- **Issues**: Create an issue for bugs or feature requests
- **Discussions**: Use GitHub Discussions for questions
- **Documentation**: Check the README and action documentation

## Code of Conduct

Please note that this project is released with a Contributor Code of Conduct. By participating in this project you agree to abide by its terms.

Thank you for contributing! 🎉
