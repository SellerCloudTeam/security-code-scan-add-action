# Security-Code-Scan Add Action

This action is designed to run as part of a workflow that builds projects referencing NuGet [SecurityCodeScan.VS2019](https://www.nuget.org/packages/SecurityCodeScan.VS2019/).

It produces a GitHub compatible SARIF file for uploading to the repository 'Code scanning alerts'.

# Usage

See [action.yml](action.yml)

### Workflow Examples

The recommended way to add this action to your workflow is with a subsequent action that uploads the prepared SARIF files to the repository 'Code scanning alerts'.

```yaml
on:
  push:

jobs:
  SCS:
    runs-on: ubuntu-latest
    steps:     
      - uses: actions/checkout@v2
      
      - name: Set up projects
        uses: security-code-scan/security-code-scan-add-action@v1

      - name: Build
        run: |
          dotnet restore
          dotnet build
        
      - name: Convert sarif for uploading to GitHub
        uses: security-code-scan/security-code-scan-results-action@v1
        
      - name: Upload sarif	
        uses: github/codeql-action/upload-sarif@v1
```

For .NET 4.x example see [FullDotNetWebApp demo repository](https://github.com/security-code-scan/FullDotNetWebApp).
