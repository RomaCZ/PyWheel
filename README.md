# Python Wheel Builder

Build Python wheels for multiple platforms using GitHub Actions.

## Overview

This repository automates building Python wheels for packages with C extensions across:
- **Linux** (manylinux2014 - compatible with most distributions)
- **Windows** (64-bit)
- **macOS** (x86_64)

Currently configured to build:
- `confluent-kafka==2.4.0`
- `lxml==5.1.1`

All wheels are built for **Python 3.13**.

## How to Use

### 1. Push to GitHub

1. Initialize git repository (if not already done):
   ```bash
   git init
   git add .
   git commit -m "Initial commit"
   ```

2. Create a repository on GitHub

3. Push to GitHub:
   ```bash
   git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPO.git
   git branch -M main
   git push -u origin main
   ```

### 2. Run the Workflow

The workflow runs automatically on:
- Push to `main` or `master` branch
- Pull requests
- Manual trigger (go to Actions tab → Build Wheels → Run workflow)

### 3. Download Built Wheels

After the workflow completes:
1. Go to the **Actions** tab in your GitHub repository
2. Click on the latest workflow run
3. Scroll to **Artifacts** section
4. Download wheels for each platform:
   - `wheels-confluent-kafka-ubuntu-22.04` (Linux)
   - `wheels-confluent-kafka-windows-2022` (Windows)
   - `wheels-confluent-kafka-macos-13` (macOS)
   - `wheels-lxml-ubuntu-22.04` (Linux)
   - `wheels-lxml-windows-2022` (Windows)
   - `wheels-lxml-macos-13` (macOS)

Artifacts are kept for 30 days.

## Adding More Packages

Edit `.github/workflows/build-wheels.yml` and add to the `matrix.package` section:

```yaml
- name: your-package
  version: "1.0.0"
  test: "import your_package; print(your_package.__version__)"
```

If the package has C dependencies, you'll need to add installation steps similar to confluent-kafka or lxml.

## Cost

GitHub Actions provides **2,000 free minutes per month** for private repositories and **unlimited for public repositories**. Each build matrix (3 OS × 2 packages = 6 jobs) typically takes 5-15 minutes per job.

## Local Wheels

The `wheels/` directory contains locally built wheels:
- Windows wheels built with local librdkafka installation
- Can be used for testing before pushing to GitHub

## Files

- `.github/workflows/build-wheels.yml` - GitHub Actions workflow configuration
- `packages.txt` - List of packages to build (for reference)
- `wheels/` - Locally built wheels directory
- `build_wheel.ps1` - PowerShell script for local Windows builds
- `.venv/` - Python virtual environment (local only, not committed)

## Notes

- All builds use **Python 3.13.9**
- Linux wheels use manylinux2014 for broad compatibility
- Windows builds are 64-bit only (no 32-bit)
- macOS builds are for x86_64 (Intel Macs)
- ARM/Apple Silicon builds not currently configured
