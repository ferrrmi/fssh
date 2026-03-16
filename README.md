# fssh

`fssh` (`Fast SSH`) is an interactive SSH host selector written in Bash for `~/.ssh/config`.
It is built for terminal-heavy workflows: fast host filtering, live reachability probes, weighted history, reconnect handling, and quick re-search after a session ends.

## Features

- Interactive host search from `~/.ssh/config`
- `fzf` support with preview, auto-used when available
- Fallback numbered menu when `fzf` is not installed
- History weighting so frequently used hosts float higher
- Session banner with current date/time, IP address, and internet status
- Detail mode with HostName/IP plus live host probe status
- Parallel reachability checks for faster status refresh
- Post-disconnect flow to list again, search another client, or quit
- Optional pre-ping before SSH connect attempt
- List-only mode for discovery without connecting

## Installation

Copy the script into your path and make it executable:

```bash
sudo cp fssh /usr/bin/fssh
sudo chmod +x /usr/bin/fssh
```

## Usage

Basic search:

```bash
fssh jenkins
```

Case-insensitive search:

```bash
fssh -i jen
```

Show all configured hosts:

```bash
fssh -a
```

Detail mode with live probe status:

```bash
fssh -d sdet
```

List only:

```bash
fssh -l jenkins
```

Force `fzf`:

```bash
fssh -f qa
```

Pre-ping before SSH:

```bash
fssh -p api
```

## Flags

| Flag | Description |
| ---- | ----------- |
| `-i` | Case-insensitive search |
| `-l` | List matched hosts only |
| `-a` | Show all hosts |
| `-d` | Show HostName/IP and live status |
| `-f` | Force `fzf` mode |
| `-p` | Pre-ping host before SSH |
| `-h` | Show help |

## Example Output

```text
============================================================
FSSH // FIELD SSH CONSOLE
[2026-03-16 13:45:22 WIB]
MODE   : FILTER
QUERY  : sdet
DETAIL : ENABLED
============================================================
:: SESSION
Current IP : 172.16.21.236
Internet   : Connected

:: DISCOVERY // 2 MATCHED HOST(S)
 1) jenkins-sdet-local (172.16.18.201) [PROBING]
 2) jenkins-sdet-zt    (10.90.1.2)      [PROBING]
running parallel reachability probes...

:: PROBE RESULT // FINAL HOST STATUS
 1) jenkins-sdet-local (172.16.18.201) [SSH-READY]
 2) jenkins-sdet-zt    (10.90.1.2)      [NO-ROUTE]

fssh-select>
```

After disconnect from a successful SSH session, `fssh` can continue without restarting:

```text
:: DISCONNECT
Target    : jenkins-sdet-local
Timestamp : 2026-03-16 13:50:31 WIB

next-action [l=list, s=search, q=quit]>
```

If you choose `s`, you can immediately search another client:

```text
client-search>
```

## Reachability Status

In detail mode, `fssh` probes hosts and shows one of these statuses:

- `SSH-READY`: SSH port is reachable
- `PORT-CLOSED`: target is up but SSH port is closed/refused
- `NO-ROUTE`: network path to target is unavailable
- `TIMEOUT`: target did not answer before timeout
- `DNS-FAIL`: hostname resolution failed
- `ICMP-OK`: ping responds, but SSH port was not confirmed
- `UNREACHABLE`: no successful probe result
- `PROBING`: temporary status while background checks are running

## Requirements

- Bash
- A valid `~/.ssh/config`
- Optional: `fzf` for fuzzy host selection
- Optional: `nc` for fast SSH port probing
- Optional: `ping` for ICMP checks and pre-ping mode

## Example SSH Config

```ssh
Host jenkins-sdet-local
    HostName 172.16.18.201
    User ubuntu

Host jenkins-sdet-zt
    HostName 10.90.1.2
    User root
```

## Notes

- Host usage is stored in `~/.fssh_history`
- Host display order is influenced by prior successful connections
- The script reads hosts from `~/.ssh/config`
