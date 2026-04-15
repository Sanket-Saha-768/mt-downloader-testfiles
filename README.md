# mt-downloader-testfiles

Large test assets for the [mt-downloader](https://github.com/Sanket-Saha-768/mt-downloader) project made by [me](https://github.com/Sanket-Saha-768) and [Aniket Das](https://github.com/AniketDas13), stored via Git LFS.

This repository is part of my Operating Systems course project at Indian Statistical Institute, Kolkata.

## 📥 Downloading a test file

Always use the `raw` URL, not the `blob` URL. The `blob` URL serves an HTML preview page — the downloader needs the actual file bytes.

❌ `https://github.com/Sanket-Saha-768/mt-downloader-testfiles/blob/main/Abhijan.1962.SD.avi`  
✅ `https://github.com/Sanket-Saha-768/mt-downloader-testfiles/raw/main/Abhijan.1962.SD.avi`

```bash
uv run mt-downloader \
  "https://github.com/Sanket-Saha-768/mt-downloader-testfiles/raw/main/Abhijan.1962.SD.avi" \
  --threads 8 \
  --out Abhijan.1962.SD.avi
```

---

## 📤 Adding a new file to this repository

### Prerequisites

Make sure Git LFS is installed and initialized on your machine (one-time setup):

```bash
# Install (Ubuntu/Debian)
sudo apt install git-lfs

# Install (macOS)
brew install git-lfs

# Initialize globally
git lfs install
```

### Steps

**1. Clone this repository**

```bash
git clone https://github.com/Sanket-Saha-768/mt-downloader-testfiles.git
cd mt-downloader-testfiles
```

**2. Track the file type you want to add** (skip if already tracked)

```bash
git lfs track "*.avi"
git lfs track "*.mp4"
git lfs track "*.bin"
```

This updates `.gitattributes`. Commit it before adding your file:

```bash
git add .gitattributes
git commit -m "track <extension> files with lfs"
```

**3. Add your file and push**

```bash
git add path/to/your/largefile.avi
git commit -m "add largefile.avi"
git push origin main
```

During the push you'll see LFS uploading the binary:

```
Uploading LFS objects: 100% (1/1), 1.1 GB, 5.2 MB/s
```

**4. Verify it was tracked correctly**

```bash
git lfs ls-files
```

A `*` next to the filename means the file was committed through LFS correctly. A `-` means the pointer exists but the content hasn't been pushed yet.

---

## 🚑 Common mistakes

**Committed a large file without LFS first?**  
GitHub rejects pushes over 100 MB. Fix it by rewriting the commit:

```bash
git lfs track "*.avi"
git add .gitattributes
git rm --cached path/to/largefile.avi
git add path/to/largefile.avi
git commit --amend --no-edit
git push origin main
```

**`.gitattributes` not committed before adding the file?**  
LFS tracking only applies to files added *after* `.gitattributes` is committed. Always commit `.gitattributes` first.

**Went over the bandwidth limit?**  
GitHub's free plan includes 1 GB of LFS storage and 1 GB/month of bandwidth. LFS downloads count against this quota. Keep test files reasonably sized.

---

## 📋 Quick reference

| Command | What it does |
|---|---|
| `git lfs install` | Enable LFS on your machine (once per machine) |
| `git lfs track "*.ext"` | Track a file type |
| `git lfs ls-files` | List LFS-managed files in this repo |
| `git lfs pull` | Download LFS files after a clone |
| `git lfs status` | Show pending LFS changes |
| `git lfs migrate import --include="*.avi"` | Retroactively move existing files into LFS |
