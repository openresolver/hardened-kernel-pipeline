# 🔷 Hardened Kernel Pipeline

> Fully automated, auditable pipeline for rebasing, building, signing, and releasing linux-hardened kernels.
* Currently only supports Clang / LLVM builds - will expand to GCC config
* If you're just looking for a kernel, head over to https://github.com/openresolver/hardened-kernel
* **Hardened patches from https://github.com/anthraxx/linux-hardened** 🤘

This repository provides the **reproducible, transparent, and automated workflow** for maintaining and distributing a hardened Linux kernel. The goal is to allow anyone to **inspect, reproduce, and build** the exact same artifacts that this pipeline produces and releases at <ins>openresolver/hardened-kernel</ins>

### 🔧 <ins>Please Note:</ins>
* This is still under active development and not everything listed is ready for use - <ins>**the script is not ready to run**</ins>

The project supports two main workflows:

1. **Pipeline / Automation** – For maintainers or server environments to continuously rebase, build, sign, and release `.deb` packages.
2. **User-Friendly Build** – For individuals to build hardened kernels in their own environment or container without relying on the full automation pipeline. *(Eventually move to own repo)*

---

## 🔵 Table of Contents

- [Features](#features)  
- [Requirements](#requirements)  
- [Pipeline / Automation Workflow](#pipeline--automation-workflow)  
- [User-Friendly Build Workflow](#user-friendly-build-workflow)  
- [Auditing & Transparency](#auditing--transparency)  
- [Container Builds](#container-builds)
- [License](#license)  

---

## ➡️ Features

- Automatically rebase linux-hardened patches onto upstream stable releases  
- Handles patch conflicts (e.g., EXTRAVERSION commits) automatically  
- Builds `.deb` kernel packages with Clang or GCC  
- GPG signing of packages for verification  
- Automatic tagging and GitHub releases  
- Fully auditable scripts with sensitive information redacted  
- Optional containerized build environment for safe, reproducible builds
- :x: Does not cover minor releases at this time due to high chance of rebase conflicts - planning to add logic for this in the future

### 🟢 Coming Soon!
- Wiki Documentation
- GCC builds
- RPM builds
- ARM64 cross compile
- Raspberry Pi compatability
- Minor release upgrade logic and patch handling
- Containerized builds

---

## ➡️ Requirements

- Linux development environment (Debian/Ubuntu or Fedora-based)  
- Python 3.9+  
- Git CLI  
- `make`, `fakeroot`, `gcc`/`clang`  
- `gpg` for signing  
- GitHub CLI (`gh`) for release automation  

Optional for containerized builds:

- Podman or Docker
  > Coming Soon

---

## 🖥️ Pipeline / Automation Workflow

This workflow is the exact logic used to generate point release patches and build the kernel **fully automated weekly rebase + build + release pipeline**.

**Script:** `build_pipeline.py`  

**Steps:**

1. Fetch upstream tags and checkout hardened branch  
2. Rebase onto latest upstream stable tag, automatic handling of known conflicts and notify on unexpected conflicts
3. Build `.deb` kernel packages  
4. GPG sign packages  
5. Push rolling hardened branch to your fork  
6. Tag the release (`v6.12-hardened-YYYY-MM-DD`)  
7. Create GitHub release with all artifacts  

---

## 😎 User-Friendly Build Workflow

This workflow is designed for **users who want to build their own Hardened kernel** without using the full automation pipeline. It’s simple, portable, and can be run locally or in a container.

**Script:** `scripts/user_build.py`  

**Steps:**

1. Checkout the desired linux-hardened tag or branch (or automate tarball download and GPG verification from kernel.org).  
2. Apply patches and specify server or desktop configuration.  
3. Run `make` and `make modules_install install` or optionally build `.deb` kernel packages using your environment’s compiler (Clang or GCC).  
4. Optionally sign the packages locally with GPG for verification.
5. Optionally sign the kernel for Secure Boot 

**Example Command:**

```bash
python3 scripts/user_build.py v6.12-hardened-YYYY-MM-DD


---

**Example Command:**

```bash
python3 scripts/build_pipeline.py v6.12.65 v6.12.63 v6.12.63-hardened1
