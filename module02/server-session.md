# Server Session: My Working Directory

**Course:** CIT 386 · Module 2 · Assignment 2.2
**Author:** Nabin Chaulagain
**Date:** September 24, 2026

## Notes

- I connected using Terminal on macOS instead of PuTTY, because PuTTY is a Windows-only program and my laptop is a Mac. The commands and the result are the same.
- My first connection dropped ("broken pipe"), so a few commands after it ran on my Mac instead of the server. I reconnected and the working session follows below.
- The folder structure from the session handout was not available to me, so my directory contains `hello.txt` only.

## What I did

1. Connected to the shared server with my key.
2. Ran `pwd` **before creating anything**, to confirm where I had landed.
3. Ran `ls -l` to see what was already there.
4. Created my working directory `nabin-chaulagain`.
5. Created an empty file `hello.txt` inside it.
6. Listed the contents in long form to show ownership and dates.

## Transcript

<!-- Paste your full Terminal window below, between the ``` marks. -->

```
<paste the whole session here>
```

## Confirmation

- My working directory is `/home/azureuser/nabin-chaulagain`
- `ls -l` shows the owner as `azureuser`, so everything I created belongs to my account.
- I stayed inside my own directory and did not open anyone else's.
