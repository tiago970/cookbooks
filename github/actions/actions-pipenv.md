# Actions Pipenv

## Guides

- [Python - pipenv](https://github.com/actions/cache/blob/main/examples.md#python---pipenv)

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
    - name: Install Python 3
      uses: actions/setup-python@v2
      with:
        python-version: ${{ matrix.python-version }}

    - name: Install dependencies
      env:
        LC_ALL: C.UTF-8
        LANG: C.UTF-8
      run: |
        python3 -m pip install -U pip
        python3 -m pip install pipenv
        pipenv install --deploy -d

    - name: Run tests with pytest
      run: pipenv run pytest --cov ./ --cov-report xml:./coverage.xml
```

## Issues

### Missing Cache Service URL

```log
[CI/pytest ("ubuntu-latest", "3.9")]   💬  ::debug::getCacheEntry - Attempt 1 of 2 failed with error: Cache Service Url not found, unable to restore cache.
[CI/pytest ("ubuntu-latest", "3.9")]   💬  ::debug::getCacheEntry - Attempt 2 of 2 failed with error: Cache Service Url not found, unable to restore cache.
| [warning]getCacheEntry failed: Cache Service Url not found, unable to restore cache.
[CI/pytest ("ubuntu-latest", "3.9")]   ⚙  ::set-output:: cache-hit=false
```

TODO
