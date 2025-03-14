# Actions SonarQube

## Links

- [Code Repository](https://github.com/SonarSource/sonarqube-scan-action)

## Workflow

```yaml
name: Run Python Tests

on: ['pull_request', 'push']

jobs:
  test:
    name: pytest (${{ matrix.os }}, ${{ matrix.python-version }})
    runs-on: ${{ matrix.os }}
    strategy:
      fail-fast: false
      matrix:
        os: ['ubuntu-latest']
        python-version: ['3.9']
    steps:
    - name: Checkout
      uses: actions/checkout@v2
      with:
        # Disabling shallow clone is recommended for improving relevancy of reporting
        fetch-depth: 0

    # ...

    # FIXME: Someone is overwrite of the SonarQube binary path.
    # https://community.sonarsource.com/t/github-action-sonarsource-sonarcloud-github-action-stopped-working/36665/4
    - run: |
        echo '/opt/sonar-scanner/bin' >> $GITHUB_PATH
        echo '/opt/java/openjdk/bin' >> $GITHUB_PATH

    - name: SonarQube Scan
      uses: sonarsource/sonarqube-scan-action@v1.0.0
      env:
        SONAR_TOKEN: ${{ secrets.SONARQUBE_TOKEN }}
        SONAR_HOST_URL: ${{ secrets.SONARQUBE_HOST_URL }}
```
