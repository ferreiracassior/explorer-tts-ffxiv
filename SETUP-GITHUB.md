# 🚀 Setup GitHub/GitLab - Push to Remote Server

This guide walks you through setting up remote repositories and pushing the explorer-tts-ffxiv project to GitHub or GitLab.

## Prerequisites

- Git installed on your machine
- GitHub or GitLab account
- SSH key configured (optional but recommended) or HTTPS access

## Step 1: Create Remote Repositories

### On GitHub:

1. Go to https://github.com/new
2. Create a new repository named: `explorer-tts-ffxiv` (main repo)
3. **Do NOT initialize** with README or .gitignore (we already have these)
4. Click "Create repository"

Copy the HTTPS or SSH URL for the next step.

### For Each Application (Optional - Future Enhancement):

When ready to separate apps as submodules:
1. Create 5 additional repositories:
   - `adventurer-tts-ffxiv-api-server`
   - `adventurer-tts-ffxiv-api-server-infra-storage`
   - `adventurer-tts-ffxiv-api-server-tts-worker`
   - `adventurer-tts-ffxiv-dalamud-plugin`
   - `adventurer-tts-ffxiv-translation-worker`

2. Do NOT initialize these with README/gitignore

## Step 2: Add Remote and Push Main Repository

### Using HTTPS:

```bash
cd /home/cassio/git/explorer-tts-ffxiv

# Add remote (replace YOUR_USERNAME and choose HTTPS or SSH URL)
git remote add origin https://github.com/YOUR_USERNAME/explorer-tts-ffxiv.git

# Verify remote was added
git remote -v

# Push to GitHub
git branch -M main  # Ensure main branch name
git push -u origin main
```

### Using SSH:

```bash
cd /home/cassio/git/explorer-tts-ffxiv

# Add remote (replace YOUR_USERNAME)
git remote add origin git@github.com:YOUR_USERNAME/explorer-tts-ffxiv.git

# Verify remote was added
git remote -v

# Push to GitHub
git branch -M main
git push -u origin main
```

## Step 3: Verify the Push

```bash
# Check remote
git remote -v

# Check log
git log --oneline
```

Expected output:
```
origin  https://github.com/YOUR_USERNAME/explorer-tts-ffxiv.git (fetch)
origin  https://github.com/YOUR_USERNAME/explorer-tts-ffxiv.git (push)

c2ea308 (HEAD -> main) chore: add gitignore
9323d54 docs: add comprehensive README and finalize monorepo structure
6394eb5 chore: add submodules configuration
9134934 docs: add git workflow documentation and project plans
```

## Step 4: (Future) Convert Apps to Submodules

When you're ready to make each app its own repository:

### 4.1 Create Individual App Repositories

For each app on GitHub (listed in Step 1):

```bash
# For each app repository, initialize it
git push -u origin main
```

### 4.2 Update Main Repository with Submodules

From the main repo:

```bash
cd /home/cassio/git/explorer-tts-ffxiv

# Remove current apps directory
git rm -r --cached apps/
git commit -m "chore: remove apps from tracking (preparing for submodules)"

# Re-add each app as submodule
git submodule add https://github.com/YOUR_USERNAME/adventurer-tts-ffxiv-api-server.git apps/adventurer-tts-ffxiv-api-server
git submodule add https://github.com/YOUR_USERNAME/adventurer-tts-ffxiv-api-server-infra-storage.git apps/adventurer-tts-ffxiv-api-server-infra-storage
git submodule add https://github.com/YOUR_USERNAME/adventurer-tts-ffxiv-api-server-tts-worker.git apps/adventurer-tts-ffxiv-api-server-tts-worker
git submodule add https://github.com/YOUR_USERNAME/adventurer-tts-ffxiv-dalamud-plugin.git apps/adventurer-tts-ffxiv-dalamud-plugin
git submodule add https://github.com/YOUR_USERNAME/adventurer-tts-ffxiv-translation-worker.git apps/adventurer-tts-ffxiv-translation-worker

# Commit submodule configuration
git add .gitmodules
git commit -m "chore: add apps as submodules"
git push origin main
```

## Step 5: Troubleshooting

### "remote repository not found"

**Solution**: Check that:
1. You replaced `YOUR_USERNAME` with your actual GitHub username
2. The repository name matches what you created on GitHub
3. Your credentials are configured correctly

```bash
# Test connection
ssh -T git@github.com  # For SSH
# or
curl -u YOUR_USERNAME https://api.github.com/user  # For HTTPS
```

### "Permission denied (publickey)" - SSH Issues

**Solution**: Configure SSH key or use HTTPS:

```bash
# Generate SSH key (if not already done)
ssh-keygen -t ed25519 -C "your-email@example.com"

# Add to GitHub:
# Settings → SSH and GPG keys → New SSH key
# Paste contents of ~/.ssh/id_ed25519.pub

# Test
ssh -T git@github.com
```

### "fatal: remote origin already exists"

**Solution**: Remove and re-add remote:

```bash
git remote remove origin
git remote add origin https://github.com/YOUR_USERNAME/explorer-tts-ffxiv.git
git push -u origin main
```

### If you need to change the remote URL later:

```bash
# View current remote
git remote -v

# Change remote
git remote set-url origin https://github.com/YOUR_USERNAME/explorer-tts-ffxiv.git

# Or remove and re-add
git remote remove origin
git remote add origin https://github.com/YOUR_USERNAME/explorer-tts-ffxiv.git
```

## Step 6: Daily Workflow

After setup, your daily workflow is:

```bash
# Make changes
cd /home/cassio/git/explorer-tts-ffxiv
# ... edit files ...

# Commit changes
git add .
git commit -m "feat: your feature description"

# Push to GitHub
git push origin main
```

## 📖 Further Reading

- [GitHub SSH Setup](https://docs.github.com/en/authentication/connecting-to-github-with-ssh)
- [GitHub HTTPS Setup](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens)
- [Git Submodules Guide](git-readme.md)
- [Git Official Documentation](https://git-scm.com/doc)

---

**Quick Reference:**

```bash
# HTTPS
git remote add origin https://github.com/YOUR_USERNAME/explorer-tts-ffxiv.git
git push -u origin main

# SSH
git remote add origin git@github.com:YOUR_USERNAME/explorer-tts-ffxiv.git
git push -u origin main
```

Replace `YOUR_USERNAME` with your GitHub username.
