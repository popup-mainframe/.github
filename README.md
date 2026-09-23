# .github
Default template files and workflows
 
# Enforce dev → main
 
This reusable GitHub Actions workflow ensures that only the `dev` branch can create a Pull Request to the `main` branch.
 
## Shared Workflow Location
 
The reusable workflow is maintained in:
 
- **Repository:** `popup-mainframe/.github`
- **Branch:** `main`
- **File:** `.github/workflows/enforce-dev-to-main.yml`
 
## How to Use It in Another Repository
 
To enable this workflow in another repository, add the following workflow file:
 
`.github/workflows/enforce-dev-to-main.yml`
 
> **Important:** The workflow must be added and merged into the `main` branch of the repository where the rule needs to be enforced.
 
Add the following:
 
```yaml
name: Enforce PRs to main only from dev
 
on:
  pull_request:
    branches:
      - main
 
jobs:
  validate:
    uses: popup-mainframe/.github/.github/workflows/enforce-dev-to-main.yml@main
```
 
The workflow references the reusable workflow from the `main` branch of the central `.github` repository.
 
## Expected Behavior
 
| Source Branch | Target Branch | Result |
|---|---|---|
| `dev` | `main` | Pass |
| `feature/*` | `main` |  Fail |
| `bugfix/*` | `main` |  Fail |
| Any branch other than `dev` | `main` | Fail |
 
## Example
 
- `dev → main` —  Workflow passes.
- `feature/test → main` —  Workflow fails.
 
## Testing
 
After adding the workflow to the repository's `main` branch, create test Pull Requests to verify:
 
1. `dev → main` — should pass.
2. Any other branch → `main` — should fail.
 
> For the workflow to prevent merging, the workflow check should be configured as a required status check for the `main` branch.
 