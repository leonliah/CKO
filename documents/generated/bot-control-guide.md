# Bot Control Guide - Enable/Disable Auto-Approve Bot

## Quick Reference

### Enable Bot
```yaml
# In .github/version-bot-config.yml
settings:
  enabled: true
  dry_run: false
```

### Disable Bot
```yaml
# In .github/version-bot-config.yml
settings:
  enabled: false
```

### Test Mode (Dry Run)
```yaml
# In .github/version-bot-config.yml
settings:
  enabled: true
  dry_run: true  # Bot analyzes but doesn't approve
```

---

## Control Methods

The bot supports multiple ways to enable/disable functionality, giving you flexibility based on your needs.

### Method 1: Configuration File (Recommended)

**Location:** `.github/version-bot-config.yml`

**To Disable Bot:**
```yaml
settings:
  enabled: false
```

**To Enable Dry-Run Mode (Testing):**
```yaml
settings:
  enabled: true
  dry_run: true
```

**Benefits:**
- Version controlled
- Easy to track changes
- Team visibility
- Can be reviewed in PRs

**Use When:**
- Permanent changes to bot behavior
- Want team review before enabling/disabling
- Need audit trail of configuration changes

---

### Method 2: Repository Variables (Global Override)

**Location:** GitHub Repository Settings > Secrets and variables > Actions > Variables

**To Disable Bot:**
1. Go to repository Settings
2. Navigate to Secrets and variables > Actions
3. Click "Variables" tab
4. Create new variable:
   - Name: `AUTO_APPROVE_ENABLED`
   - Value: `false`

**To Enable Bot:**
- Set `AUTO_APPROVE_ENABLED` to `true`
- Or delete the variable (defaults to enabled)

**Benefits:**
- Immediate effect (no commit needed)
- Highest priority (overrides config file)
- Can be changed by admins quickly
- No PR required

**Use When:**
- Emergency disable needed
- Testing/maintenance period
- Want to override without committing changes

---

### Method 3: PR Labels (Per-PR Control)

**To Skip Bot on Specific PR:**

Add label to the PR: `skip-auto-approve`

```bash
gh pr edit <PR-NUMBER> --add-label "skip-auto-approve"
```

**To Remove Skip:**
```bash
gh pr edit <PR-NUMBER> --remove-label "skip-auto-approve"
```

**Benefits:**
- Granular control per PR
- Doesn't affect other PRs
- Can be done by any contributor
- Flexible for edge cases

**Use When:**
- Specific PR needs manual review
- Testing bot behavior
- PR has unusual version changes
- Want to be extra cautious

---

## Control Priority (Highest to Lowest)

1. **Repository Variable** (`AUTO_APPROVE_ENABLED`)
   - Overrides everything
   - Immediate effect

2. **PR Label** (`skip-auto-approve`)
   - Skips only that specific PR
   - Other PRs unaffected

3. **Configuration File** (`.github/version-bot-config.yml`)
   - Default persistent setting
   - Requires commit to change

4. **Default Behavior**
   - If nothing set: Bot is ENABLED
   - If no config file: Bot uses built-in defaults

---

## Common Scenarios

### Scenario 1: First Time Setup (Testing)

**Recommendation:** Start with dry-run mode

```yaml
# .github/version-bot-config.yml
settings:
  enabled: true
  dry_run: true
```

**Result:** Bot will comment "would have approved" but won't actually approve

**Next Steps:**
1. Monitor 5-10 PRs in dry-run
2. Verify bot correctly identifies version-only PRs
3. Set `dry_run: false` when confident

---

### Scenario 2: Emergency Disable

**Situation:** Bot approved something it shouldn't have

**Action:** Use repository variable for immediate disable

1. Go to Settings > Actions > Variables
2. Set `AUTO_APPROVE_ENABLED=false`
3. Investigate the issue
4. Fix configuration
5. Re-enable when ready

**Timeline:** Takes effect immediately (no new commits needed)

---

### Scenario 3: Maintenance Period

**Situation:** Major infrastructure changes, want manual review for everything

**Option A - Temporary Disable:**
```bash
# Set repository variable
gh variable set AUTO_APPROVE_ENABLED --body "false"

# When maintenance done
gh variable set AUTO_APPROVE_ENABLED --body "true"
```

**Option B - Commit Config Change:**
```yaml
# Commit this change
settings:
  enabled: false

# Later, commit to re-enable
settings:
  enabled: true
```

---

### Scenario 4: Specific PR Needs Review

**Situation:** One PR has version changes but you want manual review

**Action:**
```bash
# Add skip label to that PR
gh pr edit 123 --add-label "skip-auto-approve"
```

**Result:** Only that PR skipped, others continue auto-approving

---

### Scenario 5: Gradual Rollout

**Phase 1: Testing (Week 1)**
```yaml
settings:
  enabled: true
  dry_run: true
```

**Phase 2: Soft Launch (Week 2-3)**
```yaml
settings:
  enabled: true
  dry_run: false
```
- Monitor closely
- Use `skip-auto-approve` label liberally for uncertain PRs

**Phase 3: Full Production (Week 4+)**
```yaml
settings:
  enabled: true
  dry_run: false
```
- Review bot actions weekly
- Refine patterns as needed

---

## Monitoring Bot Status

### Check If Bot Is Enabled

**Method 1: Check Config File**
```bash
cat .github/version-bot-config.yml | grep -A2 "settings:"
```

**Method 2: Check Repository Variable**
```bash
gh variable list
```

**Method 3: Check Workflow Run**
- Go to Actions tab
- Click recent workflow run
- Check step "Check if bot is enabled"

### Verify Bot Behavior

Create a test PR with only version changes and observe:
- **Dry-run mode:** Bot comments but doesn't approve
- **Enabled:** Bot approves
- **Disabled:** Bot comments that it's disabled
- **Skip label:** Bot comments that it's skipped

---

## Best Practices

### ✅ DO:
- Start with dry-run mode for testing
- Monitor bot actions regularly (especially first month)
- Use repository variable for emergency disables
- Document why you're disabling in commit messages
- Use skip labels for edge cases

### ❌ DON'T:
- Disable without team communication
- Leave in dry-run mode indefinitely
- Forget to re-enable after maintenance
- Ignore false positives/negatives
- Use skip label as default behavior

---

## Troubleshooting

### Bot Isn't Running At All

**Check:**
1. Is GitHub Actions enabled?
2. Is workflow file present in `.github/workflows/`?
3. Check repository settings: Settings > Actions > General

### Bot Runs But Doesn't Approve

**Check:**
1. Is `enabled: true` in config?
2. Is `dry_run: false` in config?
3. Is repository variable `AUTO_APPROVE_ENABLED` set to false?
4. Does PR have `skip-auto-approve` label?
5. Check workflow logs for details

### Bot Approves When Disabled

**Check:**
1. Verify config file was committed and pushed
2. Check repository variable isn't overriding
3. Check if there's a typo in YAML (wrong indentation)
4. Review workflow logs to see what values it read

---

## Quick Commands

```bash
# Check bot configuration
cat .github/version-bot-config.yml

# Check repository variables
gh variable list

# Set repository variable to disable
gh variable set AUTO_APPROVE_ENABLED --body "false"

# Set repository variable to enable
gh variable set AUTO_APPROVE_ENABLED --body "true"

# Delete repository variable (use config file default)
gh variable delete AUTO_APPROVE_ENABLED

# Add skip label to PR
gh pr edit 123 --add-label "skip-auto-approve"

# Remove skip label from PR
gh pr edit 123 --remove-label "skip-auto-approve"

# View recent workflow runs
gh run list --workflow=auto-approve-version-bumps.yml

# View workflow logs
gh run view <run-id> --log
```

---

## Summary Table

| Method | Scope | Speed | Requires Commit | Priority | Best For |
|--------|-------|-------|----------------|----------|----------|
| Repository Variable | Global | Immediate | No | Highest | Emergency disable |
| PR Label | Per-PR | Immediate | No | High | One-off exceptions |
| Config File | Global | Next commit | Yes | Normal | Standard operation |
| Default | Global | - | - | Lowest | Fallback |

---

## Support

For issues or questions about bot control:
1. Check workflow logs in Actions tab
2. Review [pr-bot-setup-guide.md](pr-bot-setup-guide.md)
3. Check [plans/execution/plan-002.md](../../plans/execution/plan-002.md)
4. Review configuration in [.github/version-bot-config.yml](../../.github/version-bot-config.yml)
