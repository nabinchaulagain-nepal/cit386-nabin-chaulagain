# Two Failures, Diagnosed

**Course:** CIT 386 · Module 3 · Assignment 3.2
**Author:** Nabin Chaulagain
**Date:** `<date>`

---

> **This document is AI free.** The assignment states that the reading of the
> log line is the skill being assessed and must be my own, and that log output
> must not be pasted into an AI tool because it contains the server address,
> account names and client addresses. Everything below is my own work.

---

## Failure 1

### Symptom

<!--
Write this FIRST, before you fix anything and before you know the cause.
The assignment specifically marks recording the symptom before the cause,
and warns against reconstructing it from memory two days later.
Record both of these:
  - exactly what the browser showed (the error text and the status code)
  - exactly what systemctl status said
-->

**What the browser showed:**

```
<exact text and status code>
```

**What the service status said:**

```
<exact output>
```

### The Command That Found It

<!--
Useful places to look, depending on the symptom:
  sudo journalctl -u apache2 -n 50 --no-pager     <- service failed to start
  sudo tail -50 /var/log/apache2/error.log        <- service runs, page fails
  sudo apache2ctl configtest                      <- configuration is invalid
Record the one that actually found it.
-->

```bash
<the command you ran>
```

### The Line That Identified It

<!--
ONE line. The assignment says if you have quoted twelve lines you have not
finished the work.
-->

```
<the single identifying line, quoted exactly>
```

### What That Line Named

**Resource named:** `<which file, port, directory or process>`

**Reason given:** `<what the line said was wrong with it, in your own words>`

### The Fix

**What I did:**

```bash
<the command or change>
```

**How I confirmed it worked:** `<what you checked, and what it then said>`

### On an Unfamiliar Machine

<!-- One sentence: what would you check FIRST if you met this same symptom? -->

`<one sentence>`

---

## Failure 2

### Symptom

**What the browser showed:**

```
<exact text and status code>
```

**What the service status said:**

```
<exact output>
```

### The Command That Found It

```bash
<the command you ran>
```

### The Line That Identified It

```
<the single identifying line, quoted exactly>
```

### What That Line Named

**Resource named:** `<which file, port, directory or process>`

**Reason given:** `<in your own words>`

### The Fix

**What I did:**

```bash
<the command or change>
```

**How I confirmed it worked:** `<what you checked>`

### On an Unfamiliar Machine

`<one sentence>`

---

*No AI assistance was used in this document, in line with the assignment's
AI policy. No log output was shared with any AI tool.*
