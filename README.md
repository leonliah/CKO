# CKO - Claude Code Projects

Personal repository for Claude-assisted development, automation, and documentation.

## 🚀 Quick Navigation

- **[Projects](projects/)** - All project files and code
- **[Plans](plans/execution/)** - Implementation and execution plans
- **[Prompts](prompts/)** - Prompt templates and examples (gitignored)
- **[Documents](documents/)** - Documentation and notes

## 📦 Projects

### [PR Auto-Approve Bot](projects/pr-auto-approve-bot/)
**Status:** ✅ Production Ready | **Version:** 1.0

Automated GitHub Actions bot that reviews and approves version-bump-only pull requests.

**Quick Start:**
```bash
# Copy to your repository
cp projects/pr-auto-approve-bot/workflow/auto-approve-version-bumps.yml YOUR_REPO/.github/workflows/
cp projects/pr-auto-approve-bot/config/version-bot-config.yml YOUR_REPO/.github/
```

**Documentation:**
- [Project README](projects/pr-auto-approve-bot/README.md)
- [Setup Guide](projects/pr-auto-approve-bot/docs/pr-bot-setup-guide.md)
- [Control Guide](projects/pr-auto-approve-bot/docs/bot-control-guide.md)

---

## 📁 Repository Structure

```
CKO/
├── projects/           # All project files and code
│   ├── pr-auto-approve-bot/
│   │   ├── README.md
│   │   ├── workflow/   # GitHub Actions workflow
│   │   ├── config/     # Configuration files
│   │   └── docs/       # Project documentation
│   └── README.md       # Projects overview
│
├── prompts/           # Prompt templates (gitignored)
│   ├── templates/     # Reusable prompt patterns
│   └── examples/      # Example prompts
│
├── documents/         # Documentation and outputs
│   ├── generated/     # AI-generated documents
│   └── notes/         # Manual notes
│
├── plans/             # Execution planning
│   └── execution/     # Implementation plans (plan-001.md, plan-002.md, etc.)
│
└── README.md          # This file
```

## 🎯 Directory Guide

### `/projects`
All project files organized by project name. Each project has its own:
- README with overview and quick start
- Source code, workflows, or scripts
- Configuration files
- Project-specific documentation

### `/prompts`
Store your prompt files here (excluded from git).
- **templates/** - Reusable prompt patterns
- **examples/** - Example prompts that worked well

### `/documents`
General documentation and notes.
- **generated/** - AI-generated documents
- **notes/** - Your manual notes and observations

### `/plans`
Execution plans and task breakdowns.
- **execution/** - Numbered plans (plan-001.md, plan-002.md, etc.)
- Each plan includes: date, request, steps, and results

## 🛠️ Getting Started

1. **Browse Projects**: Check [projects/](projects/) for available tools and bots
2. **Follow Setup**: Each project has its own README with setup instructions
3. **Use Plans**: Review [plans/execution/](plans/execution/) for implementation details
4. **Add Prompts**: Store your own prompts in `prompts/` (gitignored)

## 📝 Adding a New Project

1. Create project directory structure:
```bash
mkdir -p projects/project-name/{docs,src,config}
```

2. Add project README with:
   - Overview and purpose
   - Status and version
   - Quick start guide
   - Documentation links

3. Update [projects/README.md](projects/README.md) with project entry

4. Create execution plan in `plans/execution/`

## 📋 Naming Conventions

- **Projects**: `lowercase-with-hyphens`
- **Plans**: `plan-001.md`, `plan-002.md`, `plan-003.md`
- **Files**: Use lowercase and hyphens for clarity

## 🔒 Security

- `claud-api-key.txt` is excluded from version control
- `prompts/` directory is gitignored for privacy
- Keep sensitive information out of tracked files

## 📊 Project Status Badges

- ✅ **Production Ready** - Fully tested and deployed
- 🚧 **In Development** - Active development
- 🧪 **Experimental** - Proof of concept
- 📦 **Archived** - No longer maintained

## 🌐 Repository

**GitHub**: [https://github.com/leonliah/CKO](https://github.com/leonliah/CKO)

## 📞 About

This repository contains projects and tools developed with Claude Code assistance. Each project is documented with implementation plans, setup guides, and usage instructions.
