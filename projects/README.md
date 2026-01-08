# Projects

This directory contains all projects and tools developed with Claude Code assistance.

## Active Projects

### 1. [PR Auto-Approve Bot](pr-auto-approve-bot/)
**Status:** ✅ Production Ready | **Version:** 1.0 | **Updated:** 2026-01-08

Automated GitHub Actions bot that reviews and approves version-bump-only pull requests for Terraform, Terragrunt, and ArgoCD projects.

**Key Features:**
- Auto-approves version-only PRs
- Multiple enable/disable controls
- Dry-run testing mode
- CI validation and fork protection

**Quick Links:**
- [Project README](pr-auto-approve-bot/README.md)
- [Setup Guide](pr-auto-approve-bot/docs/pr-bot-setup-guide.md)
- [Control Guide](pr-auto-approve-bot/docs/bot-control-guide.md)

---

## Project Template

When starting a new project, create a directory with this structure:

```
projects/
└── your-project-name/
    ├── README.md           # Project overview and quick start
    ├── docs/               # Documentation
    ├── src/                # Source code (if applicable)
    ├── config/             # Configuration files
    ├── tests/              # Test files
    └── examples/           # Usage examples
```

## Adding a New Project

1. Create project directory: `mkdir -p projects/project-name/{docs,src,config}`
2. Add project README with:
   - Overview and purpose
   - Status and version
   - Quick start guide
   - Key features
   - Documentation links
3. Update this file with project entry
4. Update main repository README

## Project Status Badges

- ✅ **Production Ready** - Fully tested and deployed
- 🚧 **In Development** - Active development
- 🧪 **Experimental** - Proof of concept
- 📦 **Archived** - No longer maintained
- 🔄 **Maintenance** - Occasional updates only

## Repository Structure

```
CKO/
├── projects/           # All project files (this directory)
├── prompts/           # Prompt templates and examples
├── documents/         # General documentation
│   ├── generated/     # AI-generated documents
│   └── notes/         # Manual notes
├── plans/             # Execution planning
│   └── execution/     # Implementation plans
└── README.md          # Repository home
```

## Getting Started

1. Browse [Active Projects](#active-projects) above
2. Click on a project to view its README
3. Follow project-specific setup instructions
4. Check documentation for detailed guides

## Contributing

This is a personal repository for Claude-assisted development. Projects are developed through interactive sessions with Claude Code.

## Support

For issues or questions about specific projects, refer to the project's README and documentation.
