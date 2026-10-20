# Serving a Page

**Course:** CIT 386 · Module 3 · Assignment 3.2
**Author:** Nabin Chaulagain
**Date:** `<date>`

---

## 1. Command Sequence

Recorded as text, in the order the commands were run.

<!--
IMPORTANT, in this order, and do not skip step 3:
The assignment awards marks specifically for checking the service status
BEFORE starting it, and says the exact wording is on the quiz. Run it and
copy what it actually said.

Also: this is a SHARED server. Installing a package affects the whole
machine, and the assignment says to ask the instructor before making a
change that is wider than your own service. Run step 2b first - if Apache
is already installed because another student installed it, skip the
install and say so in your notes.
-->

```bash
# 1. Confirm where I landed before doing anything
pwd

# 2. Refresh the package list
sudo apt update

# 2b. Check whether the web server is already installed on this shared machine
dpkg -l | grep apache2

# 3. Check the service status BEFORE starting it - record the wording
systemctl status apache2

# 4. Install the web server package (only if 2b showed it was absent)
sudo apt install apache2 -y

# 5. Start the service
sudo systemctl start apache2

# 6. Check the status again
systemctl status apache2

# 7. Create my directory at the assigned location and place the page
sudo mkdir -p <assigned location from the session handout>
sudo cp index.html <assigned location>/index.html

# 8. Confirm the file is in place with the right ownership
ls -l <assigned location>/

# 9. Confirm the server answers locally before trying a browser
curl -I http://localhost/<my path>/
```

## 2. What the Status Said Before Starting

<!--
Quote it exactly. The assignment says the wording matters and is on the
quiz. It will say something like "inactive (dead)" or "could not be found",
and those two mean different things - one is installed but stopped, the
other is not installed at all.
-->

```
<paste the exact output of the status check from step 3>
```

**What that wording means:** `<your reading of it, in one sentence>`

## 3. What the Status Said After Starting

```
<paste the exact output of the status check from step 6>
```

## 4. The Page

Committed alongside this document as `index.html`, and placed at:

| Item | Value |
|---|---|
| Path on the server | `<assigned location>/index.html` |
| URL it loads from | `<the URL you used in the browser>` |
| Owner and permissions | `<from ls -l>` |

Source:

```html
<paste the contents of index.html here, or reference the committed file>
```

## 5. Confirmation

- Loaded in a browser from: `<URL>`
- What appeared: `<what you saw>`
- Reported as up in the course shell on: `<date>`

---

*AI disclosure: I used Claude (Anthropic) to draft the HTML page and to lay out
this Markdown document. The command sequence was run by me on the server and
the recorded output is my own.*
