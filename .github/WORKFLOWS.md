# GitHub Actions Workflows

This repository contains two automated workflows for continuous integration and deployment.

## CI Workflow (`.github/workflows/ci.yml`)

**Purpose**: Validates repository structure and content on every push and pull request.

**Triggers**:
- Push to `main` branch
- Push to `copilot/**` branches
- Pull requests

**Jobs**:
1. **Validate**: Checks that the repository structure is correct and README.md contains expected content

## Build and Deploy Workflow (`.github/workflows/build-and-deploy.yml`)

**Purpose**: Builds and deploys the repository to GitHub Pages.

**Triggers**:
- Push to `main` branch
- Pull requests to `main` branch
- Manual workflow dispatch

**Jobs**:
1. **Build**: 
   - Validates README.md
   - Sets up GitHub Pages
   - Builds site with Jekyll
   - Uploads artifacts

2. **Deploy** (only runs on main branch):
   - Deploys the built site to GitHub Pages

## Required Settings

For the deploy workflow to work, ensure that:
1. GitHub Pages is enabled in the repository settings
2. Pages is set to deploy from "GitHub Actions"
3. The workflow has the necessary permissions (already configured in the workflow file)

## Local Testing

You can test the validation logic locally by running:

```bash
bash -c '
if [ ! -f README.md ]; then
  echo "ERROR: README.md not found"
  exit 1
fi
echo "✓ README.md exists"

if [ ! -s README.md ]; then
  echo "ERROR: README.md is empty"
  exit 1
fi
echo "✓ README.md is not empty"

if grep -q "OpenManus project has moved" README.md; then
  echo "✓ README.md contains project moved message"
else
  echo "ERROR: README.md does not contain expected content"
  exit 1
fi

echo "All validations passed!"
'
```
