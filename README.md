# Anis Actions 🚀

A collection of reusable GitHub Actions for .NET projects, designed to streamline CI/CD workflows with TestContainers integration.

## Available Actions

### 🧪 Run Tests (`run-tests`)

A comprehensive action for running .NET integration tests with TestContainers support, code coverage collection, and detailed reporting.

#### Features

- ✅ **Multi-version .NET support** - Configurable .NET SDK versions
- 🐳 **TestContainers ready** - Automatic Docker setup and validation  
- 📊 **Code coverage** - Optional XPlat code coverage collection
- 📝 **Detailed logging** - Comprehensive test output and reporting
- 📦 **Artifact upload** - Automatic test results and coverage uploads
- 🔄 **Codecov integration** - Optional coverage reporting to Codecov

#### Usage

```yaml
name: Integration Tests

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest
    
    steps:
    - name: Checkout code
      uses: actions/checkout@v4
      
    - name: Run integration tests
      uses: your-username/anis.actions/run-tests@v1
      with:
        dotnet-version: '9.0.x'
        test-project-path: './tests/MyProject.IntegrationTests'
        configuration: 'Release'
        collect-coverage: 'true'
```

#### Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `dotnet-version` | The .NET version to use | ✅ Yes | `9.0.x` |
| `test-project-path` | Path to the test project | ✅ Yes | - |
| `configuration` | Build configuration | ❌ No | `Release` |
| `collect-coverage` | Whether to collect code coverage | ❌ No | `true` |

#### Outputs

The action automatically uploads the following artifacts:

- **Test Results** - TRX files with detailed test execution results
- **Code Coverage** - Cobertura XML coverage reports (when enabled)
- **Codecov Reports** - Automatic upload to Codecov (when coverage is enabled)

#### Requirements

- **Docker** - Required for TestContainers functionality
- **.NET SDK** - Automatically installed based on `dotnet-version` input
- **Test Project** - Must be a valid .NET test project with TestContainers dependencies

#### Example with Multiple Configurations

```yaml
jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        dotnet-version: ['8.0.x', '9.0.x']
        configuration: ['Debug', 'Release']
    
    steps:
    - uses: actions/checkout@v4
    
    - name: Run tests - .NET ${{ matrix.dotnet-version }} (${{ matrix.configuration }})
      uses: your-username/anis.actions/run-tests@v1
      with:
        dotnet-version: ${{ matrix.dotnet-version }}
        test-project-path: './tests/MyProject.IntegrationTests'
        configuration: ${{ matrix.configuration }}
        collect-coverage: ${{ matrix.configuration == 'Release' }}
```

## 🛠️ Development

### Local Testing

To test actions locally, you can use [act](https://github.com/nektos/act):

```bash
# Install act
curl https://raw.githubusercontent.com/nektos/act/master/install.sh | sudo bash

# Run workflow locally
act -W .github/workflows/example.yml
```

### Contributing

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/new-action`
3. Make your changes and add tests
4. Commit your changes: `git commit -am 'Add new action'`
5. Push to the branch: `git push origin feature/new-action`
6. Submit a pull request

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🤝 Support

- Create an [issue](https://github.com/your-username/anis.actions/issues) for bug reports or feature requests
- Check the [documentation](https://github.com/your-username/anis.actions/wiki) for detailed guides
- Join discussions in [GitHub Discussions](https://github.com/your-username/anis.actions/discussions)

---

Made with ❤️ for the .NET community
