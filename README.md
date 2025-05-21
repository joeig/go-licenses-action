# Go licenses check action

A GitHub Action that uses [github.com/google/go-licenses](https://github.com/google/go-licenses) to analyze the dependency tree of a Go package/binary and check license compliance.

## Description

This action helps you ensure that all dependencies in your Go project comply with your license requirements. It can:

- Report all licenses used in your project dependencies
- Check if all licenses are allowed based on your criteria
- Filter out specific packages from the check
- Include or exclude test dependencies

## Usage

Add this action to your GitHub workflow:

```yaml
on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]
jobs:
  license-check:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4    
      - uses: joeig/go-licenses-action@v1
        with:
          # Optional: Specify disallowed license types
          disallowed-types: 'forbidden,unknown'
```

## Inputs

| Input                  | Description                                                                                                                                                                                                                                                                 | Default             |
|------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------|
| `allowed-licenses`     | Comma-separated list of allowed license names. Can't be used in combination with `disallowed-types`. See [google/licenseclassifier](https://github.com/google/licenseclassifier/blob/e6a9bb99b5a6f71d5a34336b8245e305f5430f99/license_type.go) for supported license names. | `""`                |
| `application-version`  | The desired version of `github.com/google/go-licenses`.                                                                                                                                                                                                                     | `v1.6.0`            |
| `confidence-threshold` | Minimum confidence required to positively identify a license                                                                                                                                                                                                                | `0.9`               |
| `disallowed-types`     | Comma-separated list of disallowed license types. Can't be used in combination with `allowed-licenses`. Allowed values: `forbidden`, `notice`, `permissive`, `reciprocal`, `restricted`, `unencumbered`, `unknown`                                                          | `forbidden,unknown` |
| `ignore-packages`      | Comma-separated list of package path prefixes to be ignored. Dependencies from the ignored packages are still checked.                                                                                                                                                      | `""`                |
| `include-tests`        | Include packages only imported by testing code                                                                                                                                                                                                                              | `true`              |
| `package`              | The package to be checked.                                                                                                                                                                                                                                                  | `./...`             |
| `working-directory`    | The working directory.                                                                                                                                                                                                                                                      | `./`                |

## Examples

### Basic usage with default settings

```yaml
steps:
  - uses: actions/checkout@v4
  - uses: joeig/go-licenses-action@v1
```

### Allow only specific licenses

```yaml
steps:
  - uses: actions/checkout@v4
  - uses: joeig/go-licenses-action@v1
    with:
      allowed-licenses: "MIT,Apache-2.0,BSD-3-Clause"
      disallowed-types: ""
```

### Disallow specific license types

```yaml
steps:
  - uses: actions/checkout@v4
  - uses: joeig/go-licenses-action@v1
    with:
      disallowed-types: "forbidden,restricted,unknown"
```

### Ignore specific packages

```yaml
steps:
  - uses: actions/checkout@v4
  - uses: joeig/go-licenses-action@v1
    with:
      ignore-packages: "github.com/example/package1,github.com/example/package2"
```
