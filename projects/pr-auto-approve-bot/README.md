# PR Auto-Approve Bot

Automated GitHub bot for approving version-bump-only pull requests.

## Overview

This bot automatically reviews and approves pull requests that only contain version bumps for Terraform, Terragrunt, and ArgoCD projects. It reduces manual review overhead while maintaining code quality and security.

## Status

**Version:** 1.0
**Status:** ✅ Production Ready
**Last Updated:** 2026-01-08

## Quick Start

### Installation

1. Copy workflow file to your repository:
```bash
cp workflow/auto-approve-version-bumps.yml YOUR_REPO/.github/workflows/
```

2. Copy configuration file:
```bash
cp config/version-bot-config.yml YOUR_REPO/.github/
```

3. Commit and push to enable the bot.

### Quick Control

```yaml
# Enable/Disable in .github/version-bot-config.yml
settings:
  enabled: true   # true/false
  dry_run: false  # true for testing mode
```

## Documentation

- 📚 [Setup Guide](docs/pr-bot-setup-guide.md) - Installation and configuration
- 🎛️ [Control Guide](docs/bot-control-guide.md) - Enable/disable methods
- 📋 [Implementation Plan](docs/implementation-plan.md) - Technical details

## Features

✅ Auto-approves version-only PRs (Terraform, Terragrunt, ArgoCD)
✅ Multiple enable/disable control methods
✅ Dry-run mode for testing
✅ CI check validation
✅ Fork protection
✅ Comprehensive logging and audit trail

## Supported Files

- **Terraform**: `versions.tf`, `terraform.tfvars`, `.terraform.lock.hcl`
- **Terragrunt**: `terragrunt.hcl`
- **ArgoCD/K8s**: `values.yaml`, `Chart.yaml`, `application.yaml`, `kustomization.yaml`

## Control Methods

1. **Repository Variable** (Highest Priority)
   ```bash
   gh variable set AUTO_APPROVE_ENABLED --body "false"
   ```

2. **PR Label** (Per-PR Control)
   ```bash
   gh pr edit <PR-NUMBER> --add-label "skip-auto-approve"
   ```

3. **Configuration File** (Standard Operation)
   - Edit `config/version-bot-config.yml`

## Deployment Options

### Option A: Test First (Recommended)
1. Set `dry_run: true` in config
2. Deploy to test repository
3. Monitor 5-10 PRs
4. Set `dry_run: false` when confident

### Option B: Direct Deployment
1. Copy files to production repositories
2. Bot starts auto-approving immediately
3. Monitor closely for first week

### Option C: Gradual Rollout
1. Deploy to 1-2 repositories
2. Monitor for 1-2 weeks
3. Collect feedback and adjust
4. Roll out to remaining repositories

## Example Scenarios

### ✅ Auto-Approved
- Version bump in `versions.tf`
- Terragrunt module update + lock file
- ArgoCD application version update

### ❌ Manual Review Required
- Version bump + code changes
- Version bump with failing CI
- PR from external fork

## Repository

**GitHub**: [https://github.com/leonliah/CKO](https://github.com/leonliah/CKO)

## License

MIT License - See repository for details
