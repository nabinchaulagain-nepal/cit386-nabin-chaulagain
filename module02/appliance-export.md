# Appliance Export and Import

**Course:** CIT 386 · Module 2 · Assignment 2.2
**Author:** Nabin Chaulagain
**Date:** September 24, 2026

## 1. Snapshot Before Export

- Snapshot name: `pre-export`
- Taken: September 23, 2026, 10:34 AM
- Reason: a way back to the working machine if the export changed anything about it.

## 2. Export Record

| Item | Value |
|---|---|
| Machine exported | `ubuntu-server` (Ubuntu Server 24.04.5 LTS, ARM64) |
| Format chosen | Open Virtualization Format 2.0, written as a single `.ova` file (confirmed with `tar -xOf ubuntu-server.ova ubuntu-server.ovf | head -3`, which shows `ovf:version="2.0"`) |
| Why that format | A single `.ova` file is one thing to hand over, instead of an `.ovf` plus a separate disk image and manifest. |
| Exported file | `/Users/shristishrestha/Documents/ubuntu-server.ova` |
| Exported file size | 1.7 GB |
| MAC address policy | Include only NAT network adapter MAC addresses |
| Manifest file | Written (`ubuntu-server.mf`) |
| Contents of the .ova | `ubuntu-server.ovf`, `ubuntu-server.nvram`, `ubuntu-server-disk001.vmdk`, `ubuntu-server.mf` |

## 3. Checksum

Command used:

```
shasum -a 256 ~/Documents/ubuntu-server.ova
```

Result:

```
2e5ec406d1088fd1cfaee76a6d3dc411586f31e90d5be90336145cd9b9f42e7b  /Users/shristishrestha/Documents/ubuntu-server.ova
```

Purpose: whoever receives the file can run the same command. If their value matches, the file arrived complete and unmodified; if it differs by a single character, the transfer is bad and the import should not be trusted.

## 4. Import of a Classmate's Appliance

<!-- Fill this in once you have a classmate's .ova. Delete this comment afterwards. -->

- Appliance imported: `<file name>`
- Started successfully: `<yes/no>`
- What behaved differently: `<what you actually saw>`
- What I did about it: `<the fix, or "nothing needed">`

## 5. Size Comparison

| Item | Size |
|---|---|
| Original VM folder (`~/VirtualBox VMs/ubuntu-server`) | 3.9 GB |
| Exported appliance (`ubuntu-server.ova`) | 1.7 GB |

**Why they differ:** The `.ova` is about 2.2 GB smaller because the export writes a compressed copy of only the disk blocks actually in use, and packs just the machine definition plus that disk. The VM folder holds more than the disk: the dynamically allocated `ubuntu-server.vdi` uncompressed, the snapshot files from `pre-export`, the `.vbox` machine configuration, and the VirtualBox log files. None of the snapshot or log data is carried into the appliance, so the handover file is smaller than the folder it came from.

## 6. Notes

- The export was run on macOS. The GUI wizard would not enable **Next**, so the export was also attempted from the command line with `VBoxManage export`, which reported the file already existed — confirming the GUI export had completed.
