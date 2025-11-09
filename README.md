# Anis Actions 🚀

A collection of reusable GitHub Actions for .NET projects, designed to streamline CI/CD workflows with TestContainers integration and security scanning.

## Available Actions

### 🔒 Security Check (`security-check`)

A comprehensive action for scanning Docker images for security vulnerabilities using Trivy. Automatically fails workflows on critical/high severity vulnerabilities and creates annotations for warnings.

#### Features

- 🔍 **Vulnerability Scanning** - Deep security analysis using Aqua Security's Trivy
- 🎯 **Configurable Severity** - Set custom thresholds for failing builds
- 📊 **Detailed Reporting** - Comprehensive vulnerability summaries with severity breakdowns
- 🚨 **GitHub Annotations** - Automatic annotations for all detected vulnerabilities
- 📦 **Artifact Upload** - Scan results saved as artifacts for detailed analysis
- ⚙️ **Flexible Configuration** - Ignore unfixed vulnerabilities, set timeouts, and more

#### Usage

```yaml
name: Security Scan

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  security-check:
    runs-on: ubuntu-latest
    
    steps:
    - name: Checkout code
      uses: actions/checkout@v4
      
    - name: Security scan
      uses: your-username/anis.actions/security-check@v1
      with:
        image: 'myorg/myapp:latest'
        severity: 'CRITICAL,HIGH'
```

#### Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `image` | Docker image to scan (e.g., nginx:latest) | ✅ Yes | - |
| `severity` | Comma-separated severities to fail on | ❌ No | `CRITICAL,HIGH` |
| `format` | Output format (table, json, sarif) | ❌ No | `table` |
| `trivy-version` | Version of Trivy to use | ❌ No | `latest` |
| `ignore-unfixed` | Ignore vulnerabilities without fixes | ❌ No | `false` |
| `timeout` | Timeout for scan (e.g., 5m, 10m) | ❌ No | `5m` |

#### Outputs

| Output | Description |
|--------|-------------|
| `scan-result` | Result of the security scan (passed/failed) |
| `vulnerabilities-found` | Total number of vulnerabilities found |
| `fixed-vulnerabilities` | Number of vulnerabilities with fixes available |
| `unfixed-vulnerabilities` | Number of vulnerabilities without fixes |

#### Behavior

- **Workflow Failure**: The action will **fail the workflow** when vulnerabilities matching the configured severity levels are found
  - Default severities: `CRITICAL,HIGH` (configurable via `severity` input)
  - When `ignore-unfixed: 'false'` (default): Fails on ANY vulnerability matching the severity threshold
  - When `ignore-unfixed: 'true'`: Only fails on vulnerabilities with fixes available, but **still shows all vulnerabilities** (including unfixed ones) in the output
  - The workflow will stop and prevent deployment if fixable vulnerabilities are detected
- **GitHub Annotations**:
  - **CRITICAL/HIGH**: Error annotations (red) 
  - **MEDIUM**: Warning annotations (yellow)
  - **LOW/UNKNOWN**: Notice annotations (blue)
  - Annotations include fix status: `[Fix: version]` for fixable vulnerabilities or `[UNFIXED]` for vulnerabilities without fixes
- **Job Summary**: Detailed vulnerability breakdown with:
  - Severity counts (Total, Fixable, Unfixed)
  - Clear indication of which vulnerabilities can be fixed
  - Full scan results with all vulnerabilities displayed
- **Artifacts**: JSON and text reports uploaded for detailed offline analysis (retained for 30 days)

#### Important Notes

⚠️ **The workflow WILL fail** if vulnerabilities matching your severity threshold are found (unless they are unfixed and `ignore-unfixed: 'true'`). This is by design to prevent insecure deployments.

**Understanding `ignore-unfixed` behavior:**
- When `ignore-unfixed: 'true'`: Unfixed vulnerabilities are **shown in all outputs** (annotations, summary, artifacts) but do NOT cause the workflow to fail
- When `ignore-unfixed: 'false'` (default): All vulnerabilities cause workflow failure, regardless of fix availability
- This allows you to be aware of security issues while only blocking deployments on fixable vulnerabilities

If you need to:
- **See all vulnerabilities without blocking on unfixed ones**: Set `ignore-unfixed: 'true'`
- **Block on all vulnerabilities**: Set `ignore-unfixed: 'false'` (default)
- **Allow specific severities**: Adjust the `severity` input (e.g., `CRITICAL` only)
- **Review before fixing**: Check the job summary and artifacts for detailed vulnerability information

#### Example with Multiple Images

```yaml
jobs:
  security-check:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        image: 
          - 'nginx:latest'
          - 'alpine:latest'
          - 'postgres:15'
    
    steps:
    - uses: actions/checkout@v4
    
    - name: Scan ${{ matrix.image }}
      uses: your-username/anis.actions/security-check@v1
      with:
        image: ${{ matrix.image }}
        severity: 'CRITICAL,HIGH'
        ignore-unfixed: 'true'
```

---

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
