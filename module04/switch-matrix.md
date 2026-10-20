# Hyper-V Switch Reachability Matrix

**Course:** CIT 386 · Module 4 · Assignment 4.1, Part Two
**Author:** Nabin Chaulagain
**Laboratory session date:** `<date>`

---

> The matrix in section 2 records results from **testing on the laboratory
> machines**, not from documentation. Where the machine disagreed with what
> the documentation led me to expect, the machine is recorded and the
> disagreement is noted in section 3.

---

## 1. Switch Types Tested

| Switch type | What it connects |
|---|---|
| External, management OS sharing the adapter | Guests to the physical network, with the host also using that adapter |
| External, management OS **not** sharing the adapter | Guests to the physical network, with the adapter handed entirely to the virtual switch |
| Internal | Guests to each other and to the host, with no path to the physical network |
| Private | Guests to each other only, with no path to the host or anywhere else |

## 2. Reachability Matrix

<!--
Fill from your own test results. Use Yes / No, and note anything partial.
The five targets below are the usual set - replace them with the exact
five from your laboratory handout if they differ.
-->

| Target | External (sharing) | External (not sharing) | Internal | Private |
|---|---|---|---|---|
| The host (management OS) | | | | |
| Another guest on the same switch | | | | |
| Another guest on a different switch | | | | |
| Another physical machine on the laboratory network | | | | |
| The internet | | | | |

### The Row That Changed Between the Two External Variants

<!--
One row differs between "sharing" and "not sharing". Name it and say what
happened, from your test.
-->

**Row that changed:** `<which row>`

**What changed and why:** `<your explanation, from what you observed>`

## 3. Results That Surprised Me

<!--
If something surprised you, explain it. If nothing did, the assignment asks
you to name the result you would have got WRONG before the session, and why.
Either answer is acceptable; leaving this blank is not.
-->

`<your answer>`

## 4. Mapping onto the VirtualBox Modes

From Module 2, mapped onto the Hyper-V switch types tested above.

| Hyper-V switch type | VirtualBox mode that behaves the same way | Shared behaviour |
|---|---|---|
| External | Bridged Adapter | The guest appears on the physical network with its own address, and other machines on that network can open connections to it |
| Internal | Host-only Adapter | The guest can reach the host and other guests on the same network, but has no route off the machine |
| Private | Internal Network | The guest can reach only other guests on the same network; the host is not reachable and there is no external route |

### The Pairing That Does Not Line Up

**VirtualBox NAT has no counterpart among the three Hyper-V switch types.**

In VirtualBox, NAT is a per-adapter mode in which the guest sits behind the
host and shares the host's address, with outbound access and no inbound
connections unless port forwarding is configured. Hyper-V's three switch types
do not provide this. Its nearest equivalent is the **Default Switch**, which
does perform network address translation, but it is a special pre-created
switch rather than one of the three types a user can create, its address range
is managed by Hyper-V and changes on reboot, and it cannot be configured the
way a VirtualBox NAT adapter can.

The mapping is therefore three-to-three for External, Internal and Private, with
VirtualBox's NAT mode left without a clean partner on the Hyper-V side.

## 5. Timing Figures: VirtualBox With the Hyper-V Platform Off and On

<!--
Record your own two figures from the laboratory. Say what you timed - guest
boot to login prompt is the usual measurement - so the two numbers are
comparable.
-->

**What I timed:** `<e.g. guest boot from power-on to login prompt>`

| Hyper-V platform | Time |
|---|---|
| Off | `<your figure>` |
| On | `<your figure>` |

### What Changed Underneath

When the Hyper-V platform is disabled, VirtualBox runs as a type 2 hypervisor
with direct access to the processor's hardware virtualization extensions, and
its own hypervisor is the only one managing them. Enabling the Hyper-V platform
makes Hyper-V the root partition and takes exclusive ownership of those
extensions, so VirtualBox can no longer use them directly and instead runs its
guests through the Windows Hypervisor Platform API, which adds a layer of
translation between VirtualBox and the hardware and is what the slower figure is
measuring.

---

*AI disclosure: I used Claude (Anthropic) for the Markdown layout of this
document and for the explanation of the Hyper-V switch types and the Hyper-V
platform's effect on VirtualBox, as permitted by the assignment's AI policy. The
matrix results, the observed row change, the surprise, and the timing figures
are my own readings from the laboratory machines.*
