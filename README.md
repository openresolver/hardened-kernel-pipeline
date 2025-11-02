# kernel-hardening-pipeline
Documenting, automating and making kernel hardening a simple process


# Kernel Hardening Pipeline

This repository automates fetching, verifying, patching, and building a Linux kernel
with the [linux-hardened](https://github.com/anthraxx/linux-hardened) patchset.

## Features
- Automatic GPG signature verification for upstream and hardened tags
- Smart update detection (builds only when new releases exist)
- Config diffing with `scripts/diffconfig`
- Optional Secure Boot signing hooks

- ## Future To-Do
- Help document process for those just getting into compiling and patching kernels
- Help document sane defaults to keep a secure workstation or server without breaking everything (via building a wiki)
