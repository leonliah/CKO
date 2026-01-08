# Plan 002: Automated PR Review Bot for Version Bumps

## Date
2026-01-08

## Request Description
User requested to implement an automated bot that reviews and approves pull requests containing only version bump changes. This is specifically for the Australia team working with Terraform, Terragrunt, and ArgoCD projects on GitHub.

## Problem Context
- Australia team receives many PRs that only bump version numbers
- Manual review of these routine PRs creates unnecessary overhead
- Team needs automation to approve version-only changes while maintaining safety

## Detailed Execution Plan

### Phase 1: Requirements Analysis
**Technology Stack:**
- Platform: GitHub
- Project Types: Terraform (.tf), Terragrunt (.hcl), ArgoCD (YAML manifests)
- Automation Tool: GitHub Actions (native, no external services needed)

**Version File Patterns to Detect:**
- Terraform: `versions.tf`, `terraform.tfvars`, module version constraints
- Terragrunt: `terragrunt.hcl` (version blocks)
- ArgoCD: Application YAML files with `targetRevision` or `chart` version
- Lock files: `.terraform.lock.hcl`

**Auto-Approval Criteria:**
- Only version-related fields changed
- Lock files can be included (user confirmed)
- No other code or configuration changes
- PR must pass all existing CI checks

### Phase 2: Implementation Strategy

**Approach: GitHub Actions Workflow**
1. Trigger on PR open/update events
2. Use `gh` CLI and `git diff` to analyze changes
3. Parse file diffs to detect version-only changes
4. Auto-approve if criteria met
5. Add comment explaining bot action

**Benefits:**
- Native to GitHub, no third-party services
- Full control over approval logic
- Easy to customize and maintain
- Transparent audit trail

### Phase 3: File Structure

```
CKO/
├── .github/
│   └── workflows/
│       └── auto-approve-version-bumps.yml
├── documents/generated/
│   └── pr-bot-setup-guide.md
└── prompts/examples/
    └── goals.txt (updated)
```

### Phase 4: Security Considerations

**Permissions:**
- Workflow needs `pull-requests: write` permission
- Workflow needs `contents: read` permission
- Use `GITHUB_TOKEN` (automatic, no secrets needed)

**Safety Checks:**
- Never approve if CI checks are failing
- Never approve PRs from external contributors (forks)
- Log all auto-approvals for audit
- Allow manual override/re-review

### Phase 5: Testing Strategy

1. Test with historical PRs (dry-run mode)
2. Verify detection logic with various version bump patterns
3. Test edge cases (mixed changes, multiple files)
4. Deploy to a test repository first
5. Monitor first 10-20 PRs manually before full trust

### Phase 6: Rollout Plan

1. Create workflow file with dry-run mode enabled
2. Test on closed historical PRs
3. Deploy to production with notifications
4. Monitor for false positives/negatives
5. Refine rules based on team feedback
6. Document usage and customization

## Implementation Files

### 1. GitHub Actions Workflow
- File: `.github/workflows/auto-approve-version-bumps.yml`
- Triggers: PR opened, synchronized, reopened
- Actions: Analyze diff, approve if version-only, comment

### 2. Configuration (Optional)
- File: `.github/version-bot-config.yml`
- Settings: File patterns, approval rules, exclusions

### 3. Documentation
- File: `documents/generated/pr-bot-setup-guide.md`
- Content: Setup instructions, customization, troubleshooting
- File: `documents/generated/bot-control-guide.md`
- Content: Enable/disable controls, configuration methods

## Success Criteria

✓ Bot correctly identifies version-only PRs in Terraform/Terragrunt/ArgoCD
✓ Bot auto-approves only when changes are strictly version-related
✓ Bot handles lock files appropriately (.terraform.lock.hcl)
✓ Bot does NOT auto-approve if any other code changes present
✓ Bot respects CI check status (no approval if checks failing)
✓ Team can easily customize rules and patterns
✓ Clear audit trail of all bot actions

## Expected Outcomes

- Reduced manual review time for routine version bumps
- Faster PR turnaround for dependency updates
- Consistent approval process across the team
- Maintained code quality and security standards

## Notes

- Start with conservative rules, expand as confidence grows
- Team should review bot actions weekly for first month
- Consider adding Slack/email notifications for bot actions
- Can extend to other routine PR types (documentation-only, etc.)
