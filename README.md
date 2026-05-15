# fssh

`fssh` (`Fast SSH`) is an interactive SSH host selector written in Bash for `~/.ssh/config`.
It is built for terminal-heavy workflows: fast host filtering, live reachability probes, weighted history, reconnect handling, and quick re-search after a session ends.

## Features

- Interactive host search from `~/.ssh/config`
- `fzf` support with preview, auto-used when available
- Fallback numbered menu when `fzf` is not installed
- History weighting so frequently used hosts float higher
- Ultra-clean fixed header with current time, IP address, internet status, and filter
- Aligned host table with address, latency, and status
- Probe result latency in milliseconds
- Parallel reachability checks for faster status refresh
- Post-disconnect flow to list again, search another client, or quit
- Optional pre-ping before SSH connect attempt
- List-only mode for discovery without connecting
- Scrollback-safe by default; fullscreen alternate-screen mode is opt-in

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

Opt into fullscreen redraw mode:

```bash
fssh --fullscreen api
FSSH_FULLSCREEN=1 fssh api
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
| `--fullscreen` | Use alternate-screen mode and clear/redraw the menu |

## Example Output

```text
FSSH // 172.16.21.236 • Connected • fzf • 2026-03-16 13:45:22 WIB
Filter: sdet

ID    HOST                     ADDRESS           LATENCY   STATUS
[1]   jenkins-sdet-local       172.16.18.201        12ms   SSH-READY
[2]   jenkins-sdet-zt          10.90.1.2         TIMEOUT   DOWN

fssh-select >
```

After disconnect from a successful SSH session, `fssh` can continue without restarting:

```text
Disconnected: jenkins-sdet-local (172.16.18.201)

Next: [l] list [s] search [q] quit
next > prox
```

After disconnect, typing a query at `next >` starts a new search immediately. You can also type `s` for an explicit filter prompt:

```text
filter >
```

In search prompts, `all`, `*`, `.`, or an empty filter shows every host. Type `q` at `filter >` to quit.

## Reachability Status

When `fssh` probes hosts, it renders one of these table statuses:

- `SSH-READY`: SSH port is reachable
- `ICMP-OK`: ping responds, but SSH port was not confirmed
- `DOWN`: host is unreachable, timed out, DNS failed, or SSH port is closed

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
- By default, `fssh` preserves normal terminal scrollback. Set `FSSH_FULLSCREEN=1` or pass `--fullscreen` to use the old alternate-screen redraw behavior.
- `fssh` does not print or clear over remote login banners; a ParkeeOS welcome banner remains part of the successful SSH session output.
