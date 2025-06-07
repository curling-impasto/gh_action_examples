# Bash GitHub Actions Example

This repository demonstrates how to use GitHub Actions with Bash scripts to automate a simple CI process. The workflow defined in [.github/workflows/actions.yml](.github/workflows/actions.yml) consists of two jobs: build and test. It shows how to pass variables between jobs and steps using GitHub Actions’ environment and output features.

## Workflow Overview

- **Workflow Name:** Github_CI
- **Triggers:**  
  - On push to the main branch  
  - On pull request to the main branch  
  - Manually via workflow_dispatch

## Jobs

### 1. build

- **Runs on:** ubuntu-latest
- **Purpose:** Runs a `build.sh` script and captures its output.
- **Key Features:**
  - Checks out the repository code.
  - Runs `build.sh` from the `scripts` directory.
  - Stores the output of `build.sh` in a job output variable called `op`.
  - Fails the job if the build step fails.

### 2. test

- **Runs on:** ubuntu-latest
- **Depends on:** build job (runs only if build succeeds)
- **Purpose:** Runs test scripts using the output from the build job.
- **Key Features:**
  - Receives the `op` output from build as an environment variable `OPT`.
  - Checks out the repository code.
  - Sets an environment variable `FOO=35`.
  - Executes `test.sh` twice, once with the build output (`$OPT`) and once with `$FOO`.
  - Runs `build.sh` again and sets its output as an environment variable `final`.
  - Echoes the value of `final`.

## How Variables Are Passed

- **Between Jobs:**  
  The build job sets an output (`op`), which is passed to the test job using the `needs` context.

- **Between Steps:**  
  Environment variables are set and shared between steps using `$GITHUB_ENV`.

## Directory Structure

- `scripts/build.sh`: The build script executed in both build and test jobs.
- `scripts/test.sh`: The test script executed in the test job.

## Example

A successful workflow run:

1. **build job:**  
   - Checks out code  
   - Runs `build.sh` and saves the output  
   - Fails if the build does not succeed

2. **test job:**  
   - Receives build output as `$OPT`  
   - Runs `test.sh` with `$OPT` and `$FOO`  
   - Runs `build.sh` again and prints its output

---
