# mt-downloader-testfiles
This repository contains testfiles which are candidate downloads for the mt-downloader project done as part of my Operating System coursework at Indian Statistical Institute, Kolkata.
Below you can find a short tutorial on how to use git-lfs to commit very large files such as over 500MBs.

# Git LFS — storing large files in GitHub

## What is Git LFS?

Git LFS (Large File Storage) replaces large files in your repository with small text pointers, while storing the actual file contents on a separate server. This lets you version large binaries (videos, datasets, model weights, archives) without bloating your git history.

GitHub's free plan includes **1 GB** of LFS storage and **1 GB/month** of bandwidth.

---

## Installation

**Ubuntu / Debian**
```bash
sudo apt install git-lfs
```

**macOS**
```bash
brew install git-lfs
```

**Windows**
Download the installer from [git-lfs.com](https://git-lfs.com), or via winget:
```bash
winget install GitHub.GitLFS
```

Verify it installed:
```bash
git lfs version
```

---

## Setup (once per machine)

```bash
git lfs install
```

This registers the LFS hooks globally in your git config. You only need to do this once per machine, not once per repo.

---

## Tracking file types

Inside your repository, tell LFS which file types to manage:

```bash
# Track by extension
git lfs track "*.avi"
git lfs track "*.mp4"
git lfs track "*.zip"

# Track a specific file
git lfs track "data/model_weights.bin"
```

This creates or updates a `.gitattributes` file. **Commit it** — it's how git knows which files LFS manages:

```bash
git add .gitattributes
git commit -m "configure git-lfs tracking"
```

---

## Adding and pushing large files

```bash
# Add your file normally — LFS handles it transparently
git add path/to/largefile.avi
git commit -m "add large file via lfs"
git push origin main
```

During the push you'll see LFS uploading the actual binary:
```
Uploading LFS objects: 100% (1/1), 1.1 GB, 5.2 MB/s
```

---

## Verifying LFS is working

```bash
# List all files tracked by LFS in this repo
git lfs ls-files
```

A `*` next to the filename means the file has been committed through LFS. A `-` means the pointer exists but the content hasn't been pushed yet.

```bash
# Check what patterns are being tracked
git lfs track
```

---

## Cloning a repo that uses LFS

```bash
# Regular clone — LFS files are downloaded automatically
git clone https://github.com/user/repo
```

If you cloned before LFS was set up and files are missing:
```bash
git lfs pull
```

---

## Common mistakes

**Committed a large file without LFS first?**
GitHub will reject pushes over 100 MB. Fix it by rewriting the commit:
```bash
git lfs track "*.avi"
git add .gitattributes
git rm --cached path/to/largefile.avi
git add path/to/largefile.avi
git commit --amend --no-edit
git push origin main
```

**`.gitattributes` not committed?**
LFS tracking only applies to files added *after* `.gitattributes` is committed. Always commit `.gitattributes` first.

**Went over the bandwidth limit?**
LFS downloads count against your monthly quota. For high-traffic public repos, consider mirroring large files to a CDN or object store (S3, Cloudflare R2) and linking directly.

---

## Quick reference

| Command | What it does |
|---|---|
| `git lfs install` | Enable LFS on your machine |
| `git lfs track "*.ext"` | Track a file type |
| `git lfs ls-files` | List LFS-managed files |
| `git lfs pull` | Download LFS files after clone |
| `git lfs status` | Show pending LFS changes |
| `git lfs migrate import --include="*.avi"` | Retroactively move existing files into LFS |
