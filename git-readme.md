# 📚 Git Workflow - Explorer TTS FFXIV

This repository uses **Git Submodules** to manage multiple applications as independent repositories while keeping everything synchronized.

## ⚙️ Initial Setup

### Clone the repository with submodules:
```bash
git clone --recurse-submodules https://github.com/seu-usuario/explorer-tts-ffxiv.git
cd explorer-tts-ffxiv
```

If you already cloned without `--recurse-submodules`, initialize the submodules:
```bash
git submodule update --init --recursive
```

## 📁 Repository Structure

```
explorer-tts-ffxiv/                    ← Main Repo
├── apps/
│   ├── adventurer-tts-ffxiv-api-server/               ← Submodule (independent repo)
│   ├── adventurer-tts-ffxiv-api-server-infra-storage/ ← Submodule (independent repo)
│   ├── adventurer-tts-ffxiv-api-server-tts-worker/    ← Submodule (independent repo)
│   ├── adventurer-tts-ffxiv-dalamud-plugin/           ← Submodule (independent repo)
│   └── adventurer-tts-ffxiv-translation-worker/       ← Submodule (independent repo)
├── projects-plans/                    ← Main repo files
└── .gitmodules                        ← Submodules configuration
```

## 🔄 Basic Workflow - What's DIFFERENT from Normal Git

### 1️⃣ Making Commits in an Application (Submodule)

```bash
# Enter the application folder
cd apps/adventurer-tts-ffxiv-api-server

# Make your changes and commit normally
git add .
git commit -m "feat: new API feature"

# Push to the application's repo
git push origin main
```

**⚠️ IMPORTANT**: Commits in submodules do NOT automatically affect the main repo!

### 2️⃣ Synchronize Submodule Changes in Main Repo

After you commit and push in a submodule, the main repo needs to be updated:

```bash
# Go back to project root
cd /home/cassio/git/explorer-tts-ffxiv

# VS Code Source Control will show changes in .gitmodules
# You need to commit these changes
git add .gitmodules apps/adventurer-tts-ffxiv-api-server
git commit -m "chore: update api-server submodule reference"
git push origin main
```

### 3️⃣ Update All Submodules (Pull)

If someone updated a submodule remotely:

```bash
# Update all submodules to the latest version
git submodule update --remote --recursive

# Then commit the changes in main
git add .
git commit -m "chore: update submodules to latest versions"
git push origin main
```

## 🎯 Common Scenarios

### Scenario 1: Changed something in an app and want to commit

```bash
cd apps/APP-NAME
git add .
git commit -m "your message"
git push origin main

# Go back to root and sync main repo
cd ../..
git add apps/APP-NAME
git commit -m "chore: update APP-NAME"
git push origin main
```

### Scenario 2: Need to clone everything from scratch

```bash
git clone --recurse-submodules https://github.com/seu-usuario/explorer-tts-ffxiv.git
cd explorer-tts-ffxiv
# Done! All apps are already cloned
```

### Scenario 3: Forgot to clone with --recurse-submodules

```bash
git submodule update --init --recursive
# Wait for all submodules to download...
```

### Scenario 4: A submodule is stuck on main branch?

```bash
cd apps/APP-NAME
git checkout main
git pull origin main
cd ../..
git add apps/APP-NAME
git commit -m "chore: update APP-NAME to main"
git push origin main
```

## ⚡ VS Code Source Control

VS Code (with the Git Graph extension, for example) can track multiple repositories simultaneously:

- **Source Control Panel**: You'll see changes in each submodule and in main
- **Commits**: You can commit at any level
- **Push/Pull**: Works in each submodule and main separately

### Tip: Use "Source Control" → "Repositories" to switch between repos

## 🚨 Important Warnings

❌ **DON'T DO THIS**:
```bash
cd apps/APP-NAME
git add .
git commit -m "change"
# And then don't push main - it will get out of sync
```

✅ **DO THIS**:
```bash
cd apps/APP-NAME
git add .
git commit -m "change"
git push origin main

cd ../..
git add apps/APP-NAME
git commit -m "chore: update reference"
git push origin main
```

## 📖 References

- [Git Submodules Documentation](https://git-scm.com/book/en/v2/Git-Tools-Submodules)
- [Working with Submodules](https://github.blog/2016-02-01-working-with-submodules/)

## ❓ Troubleshooting

### "fatal: No url found for submodule path"
It means `.gitmodules` points to repositories that don't exist or are not accessible.

**Solution**: Create the remote repos manually on GitHub/GitLab

### Submodule is detached?
```bash
cd apps/APP-NAME
git checkout main
```

### Lost in submodules?
```bash
# See status of all submodules
git submodule status

# See URLs of submodules
git config --file .gitmodules --get-regexp path
```
