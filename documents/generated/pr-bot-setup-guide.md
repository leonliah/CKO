# Auto-Approve Version Bumps Bot - Setup Guide

## Overview

This automated bot reviews and approves pull requests that contain only version bump changes for Terraform, Terragrunt, and ArgoCD projects. It helps reduce manual review overhead for routine dependency updates.

## How It Works

1. **Triggers**: Bot activates when a PR is opened, updated, or reopened
2. **Analysis**: Examines changed files and their content
3. **Validation**: Checks if changes are version-only and CI passes
4. **Action**: Auto-approves qualifying PRs or comments explaining why not

## What Gets Auto-Approved

### Approved File Types
- **Terraform**: `versions.tf`, `terraform.tfvars`, `.terraform.lock.hcl`
- **Terragrunt**: `terragrunt.hcl`
- **ArgoCD/K8s**: `values.yaml`, `Chart.yaml`, `application.yaml`, `kustomization.yaml`

### Approval Criteria
✅ Only version-related files are changed
✅ All CI checks have passed
✅ PR is from internal repository (not a fork)
✅ No code/logic changes detected

## What Does NOT Get Auto-Approved

❌ PRs with mixed changes (version + code)
❌ PRs from external contributors (forks)
❌ PRs with failing CI checks
❌ PRs modifying excluded paths (if configured)
❌ PRs containing new functions, resources, or logic

## Installation

### Step 1: Copy Files to Your Repository

Copy these files from the CKO repository to your target repository:

```bash
# Copy workflow
cp .github/workflows/auto-approve-version-bumps.yml YOUR_REPO/.github/workflows/

# Copy configuration (optional)
cp .github/version-bot-config.yml YOUR_REPO/.github/
```

### Step 2: Verify Permissions

The workflow needs these permissions (already configured in the YAML):
- `pull-requests: write` - To approve PRs
- `contents: read` - To read repository content

These permissions work with the default `GITHUB_TOKEN`, no secrets needed.

### Step 3: Enable GitHub Actions

Ensure GitHub Actions is enabled in your repository:
1. Go to repository Settings > Actions > General
2. Set "Actions permissions" to allow workflows
3. Save changes

### Step 4: Test with a Sample PR

Create a test PR that only changes a version file:
```bash
# Example: Update Terraform version
echo 'terraform { required_version = "~> 1.6.0" }' > versions.tf
git checkout -b test-version-bump
git add versions.tf
git commit -m "Update Terraform version"
git push origin test-version-bump
```

Open a PR and watch the bot in action!

## Customization

### Adding More File Patterns

Edit [.github/workflows/auto-approve-version-bumps.yml](.github/workflows/auto-approve-version-bumps.yml):

```yaml
VERSION_PATTERNS=(
  "versions.tf$"
  "terragrunt.hcl$"
  "your-new-pattern.yaml$"  # Add your pattern here
)
```

### Excluding Specific Paths

To exclude certain directories from auto-approval, you can modify the workflow to check paths:

```bash
# Add to the analyze step
EXCLUDED_PATHS=("production/critical/" "main/important/")
for path in "${EXCLUDED_PATHS[@]}"; do
  if echo "$CHANGED_FILES" | grep -q "$path"; then
    IS_VERSION_ONLY="false"
    break
  fi
done
```

### Requiring Specific Labels

Add a label check before approval:

```yaml
- name: Check labels
  id: check_labels
  run: |
    LABELS=$(gh pr view ${{ github.event.pull_request.number }} --json labels --jq '.labels[].name')
    if echo "$LABELS" | grep -q "version-bump"; then
      echo "has_label=true" >> $GITHUB_OUTPUT
    else
      echo "has_label=false" >> $GITHUB_OUTPUT
    fi
```

## Monitoring and Maintenance

### Reviewing Bot Actions

1. Check the "Actions" tab in your repository
2. Look for "Auto-Approve Version Bumps" workflow runs
3. Review logs for each PR the bot processed

### Audit Trail

All bot approvals are:
- Logged in GitHub Actions workflow runs
- Visible in PR timeline with bot comment
- Attributed to the GitHub Actions bot account

### Adjusting Sensitivity

If you get **false positives** (bot approves when it shouldn't):
- Tighten the `VERSION_PATTERNS` regex
- Add more content analysis checks
- Require specific labels

If you get **false negatives** (bot doesn't approve when it should):
- Broaden the `VERSION_PATTERNS` regex
- Review the diff analysis logic
- Check CI check requirements

## Troubleshooting

### Bot Doesn't Approve Qualifying PRs

**Check:**
1. Are CI checks passing? Bot waits for CI
2. Is PR from a fork? Bot only works on internal PRs
3. Check workflow logs in Actions tab for details

### Bot Approves Wrong PRs

**Solution:**
1. Review the workflow logs to see what was detected
2. Adjust `VERSION_PATTERNS` to be more restrictive
3. Add additional content analysis checks
4. Consider requiring manual labels

### Workflow Doesn't Run

**Check:**
1. Is GitHub Actions enabled in repository settings?
2. Is workflow file in `.github/workflows/` directory?
3. Does repository have the correct permissions set?

## Security Considerations

### What's Safe
- Using `GITHUB_TOKEN` (automatic, scoped to repository)
- Only approving internal PRs (fork check included)
- Requiring CI checks to pass
- Transparent audit trail

### What's Risky
- Disabling the fork check (allows external approvals)
- Skipping CI check requirements
- Adding too broad file patterns
- Not monitoring bot actions

### Best Practices
1. Start conservative, expand gradually
2. Review bot actions weekly for first month
3. Keep audit trail of all auto-approvals
4. Document any customizations
5. Train team on bot behavior

## Example Scenarios

### Scenario 1: Simple Terraform Version Bump
**Files Changed:**
- `versions.tf` - Updated Terraform version from 1.5.0 to 1.6.0

**Bot Action:** ✅ Auto-approves after CI passes

### Scenario 2: Terragrunt with Lock File
**Files Changed:**
- `terragrunt.hcl` - Updated module version
- `.terraform.lock.hcl` - Updated lock file hashes

**Bot Action:** ✅ Auto-approves after CI passes

### Scenario 3: Version + Code Change
**Files Changed:**
- `versions.tf` - Updated version
- `main.tf` - Added new resource

**Bot Action:** ❌ Comments that manual review needed

### Scenario 4: ArgoCD Chart Update
**Files Changed:**
- `Chart.yaml` - Updated chart version
- `values.yaml` - Updated image tag

**Bot Action:** ✅ Auto-approves after CI passes

## Support and Feedback

### Getting Help
- Review workflow logs in Actions tab
- Check this documentation
- Review the configuration in `.github/version-bot-config.yml`

### Improving the Bot
- Collect feedback from team members
- Monitor false positives/negatives
- Adjust patterns and rules accordingly
- Document any customizations

### Reporting Issues
If you encounter issues or have suggestions:
1. Document the specific PR and bot behavior
2. Check workflow logs for error messages
3. Review this guide for solutions
4. Adjust configuration as needed

## Version History

- **v1.0** (2026-01-08) - Initial implementation
  - Support for Terraform, Terragrunt, ArgoCD
  - Basic version-only detection
  - CI check integration
  - Fork protection

## Related Files

- Workflow: [.github/workflows/auto-approve-version-bumps.yml](../../.github/workflows/auto-approve-version-bumps.yml)
- Config: [.github/version-bot-config.yml](../../.github/version-bot-config.yml)
- Plan: [plans/execution/plan-002.md](../../plans/execution/plan-002.md)
- Goals: [prompts/examples/goals.txt](../../prompts/examples/goals.txt)
