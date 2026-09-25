# Metasploit: Introduction

> TryHackMe room: [Metasploit: Introduction](https://tryhackme.com/room/metasploitintro)
> Difficulty: Easy · Category: Exploitation Frameworks

Notes and hands-on walkthrough for learning the core components of the Metasploit Framework. Based on authorized TryHackMe training labs.

---

## Table of Contents

- [Task 1 — Introduction](#task-1--introduction)
- [Task 2 — Main Components of Metasploit](#task-2--main-components-of-metasploit)
- [Task 3 — Msfconsole](#task-3--msfconsole)
- [Task 4 — Working with Modules](#task-4--working-with-modules)
- [Task 5 — Exploitation Walkthrough (EternalBlue)](#task-5--exploitation-walkthrough-eternalblue)
- [Command Reference](#command-reference)
- [Lessons Learned](#lessons-learned)

---

## Task 1 — Introduction

**Metasploit** is the most widely used exploitation framework. It supports every phase of a penetration test — from information gathering through post-exploitation.

Two main versions:

| Version | Description |
|---------|-------------|
| **Metasploit Pro** | Commercial version with a GUI; automates and manages tasks. |
| **Metasploit Framework** | Open-source, command-line version. This is what the room uses. |

The three tool categories inside the Framework:

- **msfconsole** — the main command-line interface.
- **Modules** — exploits, scanners, payloads, and other supporting components.
- **Tools** — standalone utilities such as `msfvenom`, `pattern_create`, and `pattern_offset`.

---

## Task 2 — Main Components of Metasploit

### Core vocabulary

Three terms that are easy to confuse but mean different things:

| Term | Definition | Analogy |
|------|-----------|---------|
| **Vulnerability** | A design, coding, or logic flaw in the target system. | An unlocked window |
| **Exploit** | Code that takes advantage of a vulnerability. | Climbing through the window |
| **Payload** | Code that runs on the target *after* the exploit succeeds. | What you do once inside |

**Key idea:** an exploit gets you *access*, but the payload decides *what happens next* — open a shell, run a command, launch `calc.exe` as proof of code execution.

### Module categories

| Module | Purpose |
|--------|---------|
| **Exploits** | Take advantage of a vulnerability on the target. |
| **Payloads** | Code that runs on the target after exploitation. |
| **Auxiliary** | Supporting modules that don't exploit — scanners, fuzzers, crawlers. |
| **Encoders** | Re-encode payloads to try to evade signature-based AV (limited success). |
| **Evasion** | Actively attempt to bypass AV (a step beyond encoders). |
| **NOPs** | "No operation" bytes (`0x90` on x86) used to pad payloads to a consistent size. |
| **Post** | Post-exploitation modules, used after access is gained. |

### Payload types

Under the `payloads/` directory there are four sub-types:

- **Singles** (aka *inline*) — self-contained; everything needed is in one package, no download step.
- **Stagers** — set up a connection channel between Metasploit and the target.
- **Stages** — the larger payload downloaded by the stager once the channel is open.
- **Adapters** — wrap a single payload into another format (e.g. a PowerShell one-liner).

**Why staged payloads exist:** send a tiny *stager* first (small footprint), which then pulls down the bigger *stage*. Smaller initial payload, more flexibility.

### The naming trick (single vs staged)

The separator character in the payload name tells you the type:

| Example | Separator | Type |
|---------|-----------|------|
| `generic/shell_reverse_tcp` | `_` between `shell` and `reverse` | **Single / inline** |
| `windows/x64/shell/reverse_tcp` | `/` between `shell` and `reverse` | **Staged** |

> Rule of thumb: `_` = single, `/` = staged.

### Task 2 — Questions

- **What is the name of the code taking advantage of a flaw on the target system?**
  `<your answer>`
- **What is the name of the code that runs on the target system to achieve the attacker's goal?**
  `<your answer>`
- **What are self-contained payloads called?**
  `<your answer>`
- **Is `windows/x64/pingback_reverse_tcp` among singles or staged payload?**
  `<your answer — apply the naming trick>`

---

## Task 3 — Msfconsole

`msfconsole` is the main interface to the Framework. Launch it from the terminal:

```bash
msfconsole
```

The prompt changes to `msf6 >` (or `msf5 >` depending on version).

### Key behaviours

- **Runs Linux commands** — `ls`, `ping`, `clear`, etc. work inside the console.
  (Note: `ping` needs `-c 1` on Linux to send a single packet; otherwise use `CTRL+C` to stop.)
- **No output redirection** — `help > help.txt` fails with `No such command`.
- **Tab completion** — start typing `he`, press Tab, it completes to `help`.
- **Context-based** — parameters set in one module are lost when you switch modules, unless set globally.

### Essential commands

| Command | What it does |
|---------|-------------|
| `help` / `help set` | General help, or help for a specific command. |
| `history` | Show previously typed commands. |
| `use <module>` | Enter a module's context (prompt changes to show it). |
| `show options` | List the parameters for the current context. |
| `show payloads` | List payloads compatible with the current exploit. |
| `info` | Detailed info on a module (author, sources, description). |
| `back` | Leave the current module context. |
| `search <term>` | Search the module database. |

### Search

Search by CVE, exploit name, or target system:

```
search ms17-010
```

Refine with keyword filters like `type:` and `platform:`:

```
search type:auxiliary telnet
```

The **rank** column rates exploit reliability — but remember a low-ranked exploit may still work, and a high-ranked one may fail or crash the target.

### Task 3 — Questions

- **How would you search for a module related to Apache?**
  `<your answer>`
- **Who provided the `auxiliary/scanner/ssh/ssh_login` module?**
  `<your answer — find with the info command>`

---

## Task 4 — Working with Modules

### Setting parameters

All parameters use the same syntax:

```
set PARAMETER_NAME VALUE
```

Always check context with `show options` before running anything.

### The five prompts you'll see

| Prompt | Meaning |
|--------|---------|
| `root@ip-...:~#` | Regular shell — Metasploit commands don't work here. |
| `msf6 >` | msfconsole, no context set. |
| `msf6 exploit(...) >` | Inside a module context. |
| `meterpreter >` | Meterpreter payload session on the target. |
| `C:\Windows\system32>` | A shell running on the target system. |

### Common parameters

| Parameter | Meaning |
|-----------|---------|
| **RHOSTS** | Remote host(s) — target IP, range, CIDR, or `file:/path`. |
| **RPORT** | Remote port the vulnerable service runs on. |
| **PAYLOAD** | The payload to use with the exploit. |
| **LHOST** | Local host — your attacking machine's IP. |
| **LPORT** | Local port for the reverse connection to come back to. |
| **SESSION** | Session ID of an existing connection (used by post modules). |

### Managing parameter values

| Command | Effect |
|---------|--------|
| `set RHOSTS <ip>` | Set a value in the current context. |
| `unset RHOSTS` | Clear one value. |
| `unset all` | Clear all set values (flushes the datastore). |
| `setg RHOSTS <ip>` | Set a **global** value used across all modules. |
| `unsetg RHOSTS` | Clear a global value. |

### Running modules

| Command | Effect |
|---------|--------|
| `exploit` | Launch the module. |
| `run` | Alias for `exploit` (makes sense for scanners etc.). |
| `exploit -z` | Run and immediately background the session. |
| `check` | Test if the target is vulnerable *without* exploiting (some modules only). |

### Sessions

| Command | Effect |
|---------|--------|
| `background` (or `CTRL+Z`) | Background the current session. |
| `sessions` | List active sessions. |
| `sessions -i <id>` | Interact with a specific session. |

### Hands-on — module setup

> _Paste your real terminal output here once you run it: `use` the exploit, `show options`, `set` your RHOSTS/LHOST._

```
<your msfconsole output here>
```

**What happened:** `<to be filled in after you run it>`

### Task 4 — Questions

- **How would you set the LPORT value to 6666?**
  `<your answer>`
- **How would you set the global value for RHOSTS to 10.10.19.23?**
  `<your answer>`
- **What command would you use to clear a set payload?**
  `<your answer>`
- **What command do you use to proceed with the exploitation phase?**
  `<your answer>`

---

## Task 5 — Exploitation Walkthrough (EternalBlue)

The exploitation process has three steps: **find the exploit → customize the exploit → exploit the service.**

Target vulnerability: **MS17-010 "EternalBlue"** — an SMBv1 flaw in older Windows systems (the same one used in WannaCry).

### Method (reproduce independently)

1. Search for the exploit:
   ```
   search ms17-010
   ```
2. Select it:
   ```
   use exploit/windows/smb/ms17_010_eternalblue
   ```
3. Review and set parameters:
   ```
   show options
   set RHOSTS <target-ip>
   set LHOST <your-ip>
   ```
4. (Optional) verify vulnerability:
   ```
   check
   ```
5. Run:
   ```
   exploit
   ```
6. Confirm the Meterpreter session and interact.

### Hands-on — actual run

> _Paste your real output from the exploit here — the `exploit` output, the `Meterpreter session opened` line, and whatever you ran inside the session._

```
<your exploitation output here>
```

**What happened:** `<analysis to be filled in from your real output>`

**Proof of access (e.g. `getuid`, `sysinfo`):**

```
<your session commands and output here>
```

---

## Command Reference

Quick lookup of everything used in this room.

```bash
# Launch
msfconsole

# Search & select
search <term>
search type:auxiliary <term>
use <module-path>
info <module-path>
back

# Parameters
show options
set PARAM value
setg PARAM value        # global
unset PARAM
unset all
unsetg PARAM

# Run
exploit
run
exploit -z              # run + background
check                   # test vuln only

# Sessions
background              # or CTRL+Z
sessions
sessions -i <id>
```

---

## Lessons Learned

> _Fill in after completing the room — a few notes in your own words about what clicked._

- Exploit vs payload: `<lesson>`
- Single vs staged payloads: `<lesson>`
- `set` vs `setg` and why context matters: `<lesson>`