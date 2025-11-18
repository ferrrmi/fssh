# fssh

**fssh (Fast SSH)** is an enhanced interactive SSH selector written in Bash.
It helps you quickly search, filter, and connect to SSH hosts using features like fuzzy search, host history weighting, automatic previews, and smart reconnection.

---

## 🚀 Features

* **Fuzzy finder mode (fzf)** — auto-enabled when available
* **History weighting** — frequently used hosts appear at the top
* **Colorized interface** with clean UX
* **Automatic HostName/IP preview** (with `-d` or fzf)
* **Reconnect loop** — retry, try another host, or quit
* **Pre-ping mode** (`-p`) to check reachability before SSH
* **Case-insensitive search** (`-i`)
* **List-only mode** (`-l`)
* **Show all hosts** (`-a`)
* Full fallback to **non-fzf select menu** if fzf not installed

---

## 📦 Installation

Copy the script into your system path and make it executable:

```bash
sudo cp fssh /usr/bin/fssh
sudo chmod +x /usr/bin/fssh
```

That's it — you're ready to go.

---

## 🧭 Usage

### Basic search

```bash
fssh yx
```

Example:

```
1) pyx
2) jyx
3) server-9yx
#?
```

### Show all hosts

```
fssh -a
```

### Case-insensitive search

```
fssh -i jen
```

### Show with HostName/IP details

```
fssh -d jenkins
```

### List only (no connection)

```
fssh -l supeng
```

### Force fuzzy finder mode

```
fssh -f parkee
```

### Pre-ping host before connecting

```
fssh -p qa
```

---

## 🏷️ Flags Summary

| Flag | Description                                |
| ---- | ------------------------------------------ |
| `-i` | Case-insensitive search                    |
| `-l` | List matched hosts only (no SSH)           |
| `-a` | Show all hosts                             |
| `-d` | Show HostName/IP details                   |
| `-f` | Force fzf mode (fail if fzf not installed) |
| `-p` | Ping host before SSH                       |
| `-h` | Show help menu                             |

---

## 💡 Tips

* Install **fzf** for the best experience (preview pane + fuzzy search):

```bash
sudo apt install fzf
```

* Your SSH `~/.ssh/config` must contain proper `Host` and `HostName` entries.

* The script automatically tracks your most used hosts, making them easier to access.

---

## 🛠️ Example SSH Config

```ssh
Host jenkins-sdet-local
    HostName 172.16.18.50
    User ubuntu

Host qa-agent-zt
    HostName 10.20.3.44
    User root
```

---

## 🔥 Enjoy faster SSH access!

If you want an even more advanced version (jump-host detection, key preview, filtering metadata, or TUI mode), feel free to ask.
