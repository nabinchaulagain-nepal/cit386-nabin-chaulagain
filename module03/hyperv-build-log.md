# Hyper-V Build Log, with Disk Behaviour Measured

**Course:** CIT 386 · Module 3 · Assignment 3.1
**Author:** Nabin Chaulagain
**Date:** October 20, 2026

---

## Status of This Submission

I want to be straightforward about what this document is and is not.

My personal machine is an **Apple MacBook Air (M2)**. Hyper-V is a Windows-only
feature and does not exist on macOS, and it also cannot be reached indirectly:
running Windows inside Parallels or VMware on this machine and enabling Hyper-V
there requires nested virtualization, which Apple Silicon supports only from the
M3 generation onward. My M2 cannot do it.

**I have therefore not yet taken the disk measurements, and the measurement
table in section 2.4 is deliberately empty.** I have not entered figures I did
not read from a host, because the whole point of the exercise is the numbers.

What this document does contain is everything I can establish without the
machine: the full build procedure and the reasoning behind each setting, the
generation justification, the exact commands each measurement is taken with,
and an explanation of what the disk does and why. I will complete the
measurements on a laboratory workstation on campus and commit the figures as an
update to this same file.

---

## Part One: The Build

### 1.1 Host Workstation

To be recorded from the laboratory host. These are the commands that produce
each figure, run in PowerShell on the host:

```powershell
Get-ComputerInfo |
    Select-Object WindowsProductName, WindowsVersion,
                  CsProcessors, CsTotalPhysicalMemory

Get-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V-All |
    Select-Object FeatureName, State
```

| Item | Value |
|---|---|
| Windows edition | *to be recorded on the host* |
| Processor | *to be recorded on the host* |
| Total memory | *to be recorded on the host* |
| Hyper-V role already enabled | *to be recorded on the host* |

Hyper-V requires a 64-bit processor with second-level address translation,
hardware-assisted virtualization enabled in firmware, and Windows in the Pro,
Enterprise or Education edition. It is not available in Windows Home.

### 1.2 Intended Guest Configuration

This is the configuration I will build, with the reason for each choice. Actual
values will be confirmed against the machine once it exists.

| Setting | Intended value | Reason |
|---|---|---|
| VM name | `nabin-chaulagain` | The course convention is a machine named after the student, so the instructor can find mine on a shared laboratory host |
| Generation | 2 | See section 1.3 |
| Startup memory | 4096 MB | Enough for a 64-bit guest to install and run comfortably without taking an unreasonable share of a shared laboratory workstation |
| Dynamic memory | On | The guest is idle most of the time. Dynamic memory lets the host reclaim what the guest is not using, which matters on a host expected to run several machines at once |
| Minimum RAM | 1024 MB | A floor low enough to be worth reclaiming, high enough that the guest does not thrash |
| Maximum RAM | 8192 MB | A ceiling so one guest cannot starve the others on a shared host |
| Virtual processors | 2 | Enough for installation and updates to finish in reasonable time without monopolising the host's cores |
| Virtual switch | Default Switch | Gives the guest outbound network access through the host with no additional configuration, which is all this exercise needs |
| Disk size | 60 GB | Comfortably more than the guest will use, which is the point: it makes the difference between the configured size and the file size on disk visible |
| Disk type | Dynamically expanding | This is the disk whose behaviour the exercise measures |
| Guest operating system | 64-bit, UEFI-capable | Required by generation 2; see below |

Commands to confirm these on the host once the machine is built:

```powershell
Get-VM -Name "nabin-chaulagain" | Format-List *
Get-VMMemory        -VMName "nabin-chaulagain"
Get-VMProcessor     -VMName "nabin-chaulagain"
Get-VMNetworkAdapter -VMName "nabin-chaulagain"
Get-VMHardDiskDrive -VMName "nabin-chaulagain" | Select-Object Path
```

### 1.3 Generation Justification

**What generation 2 provides.** Generation 2 replaces the emulated BIOS of a
generation 1 machine with UEFI firmware. That brings Secure Boot, booting from a
SCSI controller rather than an emulated IDE controller, PXE boot over a standard
network adapter instead of a legacy one, boot volumes larger than 2 TB, and a
noticeably faster boot because there is far less emulated legacy hardware to
initialise.

**What generation 2 requires of the guest.** The guest operating system must be
64-bit and capable of booting from UEFI. A 32-bit guest cannot run on generation
2 at all, and neither can an older operating system that expects a BIOS. A Linux
guest will usually boot only after the Secure Boot template is changed from the
Windows default to **Microsoft UEFI Certificate Authority**, or Secure Boot is
turned off for that machine.

**Why it is the right choice here.** The guest for this exercise is a modern
64-bit UEFI-capable operating system, so none of generation 2's requirements
exclude it, and the faster boot and Secure Boot support are worth having. The
only circumstance that would have forced generation 1 is a 32-bit or legacy
guest, which does not apply. The choice is therefore generation 2.

### 1.4 Virtual Disk File Location

The file lives on the host, not in the guest. By default Hyper-V places it at:

```
C:\Users\Public\Documents\Hyper-V\Virtual hard disks\<machine name>.vhdx
```

The exact path for my machine will be recorded from the host with:

```powershell
Get-VMHardDiskDrive -VMName "nabin-chaulagain" | Select-Object Path
```

| Item | Value |
|---|---|
| Full path on host | *to be recorded on the host* |
| Disk format | VHDX |

---

## Part Two: The Disk Measurement

> **Every size figure in this section is to be read from the HOST filesystem.**
> The guest reports the logical volume it believes it owns, which is a different
> number and is not what this exercise measures. The instructor identifies
> measuring from the guest as the most common failure on this assignment, and
> I have recorded the host-side command for each reading below for that reason.

### 2.1 The Measurement Procedure

**Reading the host file size** — run on the host, between each step:

```powershell
Get-VHD -Path "<path to the .vhdx>" |
    Select-Object Path, VhdType, @{n='ConfiguredGB';e={$_.Size/1GB}},
                  @{n='FileSizeGB';e={$_.FileSize/1GB}}
```

`Size` is what Hyper-V was told to create. `FileSize` is what the file actually
occupies on the host. The gap between those two numbers is the entire subject of
this exercise.

**Writing data inside the guest** — run in the guest, using the method from the
session. If that method is `fsutil`, writing 10 GB is:

```
fsutil file createnew C:\testdata.dat 10737418240
```

**Deleting the data inside the guest** — delete the file and empty the Recycle
Bin, so the guest genuinely releases the blocks rather than moving them.

**The four readings, in order:**

1. Host file size before any data is written
2. Amount of data written inside the guest
3. Host file size after writing
4. Host file size after deleting, with the Recycle Bin emptied

Then repeat readings 1 and 4 against a **fixed size** disk for comparison.

### 2.2 Prediction

*To be written before taking reading 4, as the assignment requires. I will
record my honest expectation at the time rather than back-fill it afterwards,
since the instructor has said an incorrect prediction that is then explained is
worth more than a tidy table.*

### 2.3 What the Disk Does, and Why

This section explains the mechanism. The figures that demonstrate it go in 2.4
once I have taken them.

**A dynamically expanding disk starts small.** However large the configured size,
the file on the host begins at a few megabytes. It holds the VHDX metadata and
almost nothing else, because the guest has not yet written anything.

**It grows as the guest writes.** When the guest writes a block, Hyper-V
allocates a matching block in the VHDX and the file on the host grows. The
growth tracks data actually written, not the configured size.

**It does not shrink when the guest deletes.** This is the result the assignment
says people find surprising, and the reason is a boundary between two layers.
Deleting a file inside the guest is an operation in the *guest's* filesystem: it
marks those blocks as free in the guest's own allocation table. The host has no
visibility into the guest's filesystem and no way to know that happened. From
the host's point of view the VHDX still owns every block it was ever given, so
the file size stays where it was. Space is only returned to the host by a
separate, deliberate compaction — `Optimize-VHD` on the host, preceded by TRIM
inside the guest so that the free blocks are identified.

**A fixed size disk never changes.** It occupies its full configured size from
the moment it is created, before a single byte has been written, and that figure
does not move whatever the guest does. Creating one takes longer, because the
host has to allocate the whole file up front.

### 2.4 Measurement Table

*Not yet taken. To be completed on a laboratory workstation and committed as an
update to this file. No figures have been entered, because measurements I did
not read from a host would not be measurements.*

| Measurement | Dynamically expanding disk | Fixed size disk |
|---|---|---|
| Size configured in Hyper-V | | |
| Host file size before writing | | |
| Data written inside the guest | | |
| Host file size after writing | | not required |
| Host file size after deleting | | |
| Net change across the exercise | | |

### 2.5 What This Means for a Host Running Twelve Machines

Three consequences follow from the behaviour described in 2.3.

The first is **one-directional growth**. Twelve expanding disks each grow as
their guests write and none of them shrink when their guests delete. Over months
the host's disk consumption only climbs, and it climbs in response to data that
may no longer exist anywhere inside any guest.

The second is **overcommitment**. Twelve machines configured at 60 GB each
promise 720 GB. A host with 500 GB of disk will carry them happily for as long as
the guests stay small, and will run out without warning at the point where enough
of them have grown. The failure is not gradual: when the host's disk fills, every
guest on it is affected at once, not just the one that grew.

The third is **maintenance**. Keeping that host healthy means compaction has to
be scheduled, which means running TRIM in each guest and `Optimize-VHD` on the
host, and compaction on a VHDX requires the machine to be off or the disk
detached. Twelve machines is the point at which this stops being something an
administrator remembers to do and has to become a scheduled task.

### 2.6 When I Would Choose Fixed Anyway

**When the guest must not fail because of something happening on the host.** A
fixed disk has its full capacity allocated before the guest ever boots, so no
other machine on that host can consume the space it was promised. With expanding
disks, a guest can be refused a write it has every right to make simply because
eleven neighbours filled the host first, and the resulting failure appears inside
the guest with nothing in the guest to explain it. Where the workload is a
database or anything else that must not encounter an unexpected write failure,
allocating the space up front and accepting the slower creation and the wasted
capacity is the right trade.

---

## Plan to Complete

1. Build the machine on a laboratory workstation on campus, named per the course convention.
2. Record the host and guest settings in sections 1.1, 1.2 and 1.4 from the host.
3. Write the prediction in section 2.2 before taking reading 4.
4. Take the four readings for the expanding disk, then readings 1 and 4 for a fixed disk.
5. Fill section 2.4 and note anything that differed from the explanation in 2.3.
6. Commit the updated file.

---

*AI disclosure: I used Claude (Anthropic) to structure this Markdown document
and to explain Hyper-V generation 2 requirements and the mechanism behind
dynamically expanding disk behaviour, as permitted by the assignment's AI
policy. The measurement table is empty because I have not yet been able to take
the readings; no figures have been generated or estimated. The configuration
choices and their reasons are my own, and I will record the measurements and my
prediction myself.*
