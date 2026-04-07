# 🎮 Explorer TTS FFXIV

A comprehensive text-to-speech solution for Final Fantasy XIV that brings voice to your adventures.

## 📚 Project Overview

Explorer TTS FFXIV is a modular ecosystem designed to provide seamless text-to-speech integration with FFXIV. The project consists of multiple interconnected services:

- **Dalamud Plugin** - In-game FFXIV client integration
- **API Server** - Central orchestration and request handling  
- **TTS Worker** - Audio synthesis and voice generation
- **Translation Worker** - Multi-language support and localization
- **Infrastructure Storage** - Database and cache management

## 🏗️ Architecture

```
explorer-tts-ffxiv/
├── apps/
│   ├── adventurer-tts-ffxiv-api-server/
│   ├── adventurer-tts-ffxiv-api-server-infra-storage/
│   ├── adventurer-tts-ffxiv-api-server-tts-worker/
│   ├── adventurer-tts-ffxiv-dalamud-plugin/
│   └── adventurer-tts-ffxiv-translation-worker/
├── projects-plans/
└── git-readme.md (Git workflow & submodules guide)
```

Each application is self-contained with its own README and can eventually be versioned as independent submodules.

## 🚀 Getting Started

### Prerequisites
- Git (for cloning and version control)
- Development environment for your target application
- See individual app READMEs for specific requirements

### Clone the Repository

```bash
git clone https://github.com/seu-usuario/explorer-tts-ffxiv.git
cd explorer-tts-ffxiv
```

### Next Steps

**For Dalamud Plugin Development:**
```bash
cd apps/adventurer-tts-ffxiv-dalamud-plugin
# See README.md for setup
```

**For API Server Development:**
```bash
cd apps/adventurer-tts-ffxiv-api-server
# See README.md for setup
```

For other applications, follow similar patterns. Each app has its own README with specific instructions.

## 📖 Git Workflow

⚠️ **This repository uses a special Git workflow for managing multiple applications as growing submodules.**

👉 **IMPORTANT**: Read [git-readme.md](git-readme.md) for complete instructions on:
- Cloning with submodules
- Committing changes to individual apps
- Synchronizing the main repository
- Troubleshooting common issues

## 📋 Project Planning

Detailed project plans are available in the `projects-plans/` directory:
- `01-adventurer-tts-ffxiv-dalamud-plugin.md`
- `02-adventurer-tts-ffxiv-api-server.md`
- `03-adventurer-tts-ffxiv-translation-worker.md`
- `04-adventurer-tts-ffxiv-api-server-tts-worker.md`
- `05-adventurer-tts-ffxiv-api-server-infra-storage.md`
- `explorer-tts-ffxiv-main.md`

## 📝 Application READMEs

Each application directory contains a README with specific information:

- [API Server](apps/adventurer-tts-ffxiv-api-server/README.md)
- [Infrastructure Storage](apps/adventurer-tts-ffxiv-api-server-infra-storage/README.md)
- [TTS Worker](apps/adventurer-tts-ffxiv-api-server-tts-worker/README.md)
- [Dalamud Plugin](apps/adventurer-tts-ffxiv-dalamud-plugin/README.md)
- [Translation Worker](apps/adventurer-tts-ffxiv-translation-worker/README.md)

## 🤝 Contributing

1. **Read the git workflow guide** - [git-readme.md](git-readme.md)
2. **Choose your target app** - Navigate to the app directory
3. **Follow the app's contributing guidelines** - Each app has specific requirements
4. **Commit changes** - Remember to sync the main repository after committing to individual apps

## 🔄 From Monorepo to Submodules

Currently, this project is structured as a monorepo. Over time, each application will be converted to independent submodules. When that happens:

1. Create individual repositories on GitHub/GitLab
2. Push each app's code to its repository
3. Replace the monorepo structure with Git submodules
4. Track submodule versions in the main repository

For now, all applications are tracked in this single repository with plans documented in `projects-plans/`.

## 📞 Support

For issues or questions:
1. Check individual app READMEs first
2. Review [git-readme.md](git-readme.md) for Git-related questions
3. Check project plans for feature status and roadmap

## 📄 License

Part of the Explorer TTS FFXIV project.

---

**Last Updated**: April 7, 2026
