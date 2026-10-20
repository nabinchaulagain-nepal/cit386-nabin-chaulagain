# Issuing a Shared Access Signature, and Proving What It Does

**Course:** CIT 386 · Module 4 · Assignment 4.2
**Author:** Nabin Chaulagain
**Date:** `<date>`

---

> **Redaction notice.** Every URL in this document has the signature value cut
> after `sig=`. The parameters are shown because they are the interesting part;
> the signature is the credential and appears nowhere in this repository, in any
> screenshot, or in anything posted to the course shell.

---

## 1. Token One: Read-Only, Term-Bounded

### Parameters Set Before Generating

<!--
Record every parameter you actually set, before you click Generate.
The grading criterion is specifically: read permission only, an expiry inside
the term, and HTTPS required. Make sure those three are visibly correct here.
-->

| Parameter | Value I set | Why |
|---|---|---|
| Scope | `<my own directory only>` | The token must not reach anyone else's content |
| Signed resource type | `<Object / Container / Service>` | `<your reason>` |
| Permissions | **Read only** | The recipient needs to open the file, nothing more |
| Allowed protocols | **HTTPS only** | The signature travels in the URL; over HTTP it would cross the network in clear text |
| Start time | `<value>` | `<your reason>` |
| Expiry time | `<value, inside the term>` | `<your reason>` |
| Allowed IP addresses | `<value or "not set">` | `<your reason>` |
| Signing key | `<which key>` | `<your reason>` |

### The URL, Redacted

```
https://<account>.blob.core.windows.net/<container>/<my-directory>/index.html
  ?sp=r
  &st=<start>
  &se=<expiry>
  &spr=https
  &sv=<version>
  &sr=<resource>
  &sig=[REDACTED]
```

## 2. Evidence: The Permitted Operation Works

<!--
Open the URL in a browser that is NOT signed into the Azure portal.
A private or incognito window is the easiest way to be sure.
-->

**Browser used:** `<e.g. Safari private window, never signed into the portal>`

**What happened:** `<what loaded, and the HTTP status if you checked it>`

**Evidence:** `<screenshot filename, with the signature redacted in the image too>`

## 3. Evidence: A Refused Operation

<!--
The assignment says attempting to LIST the container is the easiest refusal to
test, and warns that people assume this is refused without trying it. Two
attempts a term come back ALLOWED because a permission was ticked in passing
or the scope was set at the container rather than the folder.

Try it. Record what actually came back, even if it was allowed.

To attempt a list, request the container with ?restype=container&comp=list
appended, using the same token.
-->

**What I attempted:** `<the operation>`

**The request, redacted:**

```
<the URL with sig cut>
```

**What came back:**

```xml
<paste the exact response, including the error code and message>
```

**Error code returned:** `<exact code>`

**What that code means:** `<your reading of it>`

<!--
For reference while you compare - record what YOU actually got, not this list:
  AuthorizationPermissionMismatch  the token is valid but lacks the permission
                                   for the operation attempted
  AuthenticationFailed             the signature itself was not accepted
  ResourceNotFound                 the path does not resolve at this scope
-->

## 4. Token Two: Expiry Tested

### Parameters

| Parameter | Value |
|---|---|
| Permissions | Read only |
| Allowed protocols | HTTPS only |
| Start time | `<value>` |
| Expiry time | `<a few minutes after the start>` |

### Before Expiry

**Time of attempt:** `<time>`

**Result:** `<what happened>`

### After Expiry

**Time of attempt:** `<time, after the expiry passed>`

**What came back:**

```xml
<paste the exact response>
```

**Error code returned:** `<exact code>`

**What that code means:** `<your reading of it>`

## 5. The Documentation

Four questions, one short section each.

### What this token permits

`<your answer>`

### What it forbids

`<your answer>`

### From where

`<your answer - protocol restriction, and IP restriction if you set one>`

### Between which two moments

`<your answer - the exact start and expiry, and what happens either side>`

## 6. If a Recruiter Needed Access for Six Months

A six-month expiry is not a different setting so much as a different risk
posture, and three things change.

The first is that **the window stops being supervised**. A token expiring inside
the term is one I will still remember issuing. One expiring in six months
outlives my attention, and the failure mode of a long-lived credential is that
nobody is watching it by the time it matters.

The second is that **the URL spreads**. A link that works for six months gets
forwarded, saved in a browser's history, pasted into a message thread, and
indexed anywhere it is posted publicly. Every copy is a working credential, and
none of them can be individually withdrawn.

The third is that **revocation becomes the only control, and it is blunt**. A
shared access signature cannot be cancelled on its own; the only way to stop one
early is to rotate the signing key, which invalidates every other token signed
with it at the same time. With a short expiry that never arises. With six months
it is the only lever, and pulling it affects everything.

What I would actually change: sign the token with a **stored access policy** on
the container rather than directly with the account key, so that the policy can
be modified or deleted to revoke that one token without rotating the key and
breaking everything else. I would also restrict it by IP address if the
recipient's address is known and stable, and keep the permission at read only
against a single file rather than a directory.

## 7. If I Handed Over the Storage Account Key Instead

**The one thing that goes wrong: the key is not scoped, and it is not read
only.** A shared access signature answers four separate questions — what, what
not, from where, between when — and every one of them is a boundary. The account
key answers none of them. It grants full control of every container, every blob,
every file and every queue in the account, with no expiry and no protocol
restriction, to anyone who holds it. A recruiter who only needed to open one PDF
would be holding something that could delete the entire account's contents.

The token is the better answer even for someone I trust, because the question is
not whether they would misuse it. It is what happens if their laptop is stolen,
their email is breached, or they forward the message to a colleague without
thinking. Trust in a person does not extend to every system that person's
credentials pass through. The token limits the damage of an accident; the key
guarantees that any accident is total.

---

*AI disclosure: I used Claude (Anthropic) for the Markdown layout of this
document and for the closing analysis in sections 6 and 7, as permitted by the
assignment's AI policy. All parameters, evidence and error codes recorded above
are from my own attempts against my own content. No token, no URL containing
one, and no storage account key was shared with any AI tool.*
