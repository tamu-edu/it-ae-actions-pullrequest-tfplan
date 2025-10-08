# Running ARG_MAX Tests Locally

This document explains how to easily run the ARG_MAX unit tests in your local development environment.

## Method 1: VS Code with GitHub Actions Extension

### Prerequisites
1. Install [VS Code](https://code.visualstudio.com/)
2. Install the [GitHub Actions extension](https://marketplace.visualstudio.com/items?itemName=GitHub.vscode-github-actions)
3. Have this repository cloned locally

### Steps to Run Tests

1. **Open the repository in VS Code**
   ```bash
   code /path/to/it-ae-actions-pullrequest-tfplan
   ```

2. **Open the GitHub Actions workflow file**
   - Navigate to `.github/workflows/test-argmax-core.yaml`
   - The file will open in the editor

3. **Run the workflow using the GitHub Actions extension**
   - Open the Command Palette (`Ctrl+Shift+P` or `Cmd+Shift+P`)
   - Type "GitHub Actions: Run Workflow"
   - Select the workflow file `test-argmax-core.yaml`
   - Choose the branch (usually your current branch)
   - Click "Run Workflow"

4. **View results**
   - The extension will show the workflow run status
   - Click on the workflow run to see detailed logs
   - You can watch the tests execute in real-time

## Method 2: Direct Command Line Testing

You can also run the core tests directly in your terminal to see the ARG_MAX behavior:

### Test 1: Safe small payload
```bash
# This should work fine
env MYVAR="$(head -c 1000 < /dev/zero | tr '\0' 'A')" cat /dev/null
echo "Small payload test: PASSED"
```

### Test 2: Demonstrate ARG_MAX failure
```bash
# This should fail with "Argument list too long"
ARG_MAX=$(getconf ARG_MAX)
LARGE_SIZE=$((ARG_MAX + 100000))
echo "Testing with payload size: $LARGE_SIZE bytes"
env MYVAR="$(head -c $LARGE_SIZE < /dev/zero | tr '\0' 'B')" cat /dev/null
# Expected: bash: /usr/bin/env: Argument list too long
```

### Test 3: File-based fix
```bash
# This should work by using files instead of environment variables
ARG_MAX=$(getconf ARG_MAX)
LARGE_SIZE=$((ARG_MAX + 100000))
head -c $LARGE_SIZE < /dev/zero | tr '\0' 'C' > /tmp/large_payload.txt
cat /tmp/large_payload.txt > /dev/null
echo "File-based approach test: PASSED"
rm -f /tmp/large_payload.txt
```

## Method 3: GitHub Actions Web Interface

1. **Push your changes to GitHub**
2. **Go to the Actions tab** in your GitHub repository
3. **Find the "Test ARG_MAX Limits - Core Problem Demonstration" workflow**
4. **Click "Run workflow"** to trigger it manually
5. **Watch the results** in the web interface

## Understanding the Tests

- **Test 1 (Safe)**: Uses a 1KB payload that's well within system limits
- **Test 2 (Failure)**: Intentionally exceeds ARG_MAX to demonstrate the "Argument list too long" error
- **Test 3 (Fixed)**: Shows how using files instead of environment variables solves the problem

These tests isolate the core ARG_MAX problem without involving Terraform complexity, making it easy to understand and reproduce the issue and its solution.