# The Complete Linux Tutorial — Commands, Concepts, and Working with the Tools That Matter

*Every command here is grouped by the actual job it does, not memorized as a flat list — the filesystem model, the permission model, the process model, and the shell, each with the commands that operate on it, why they exist, and the mistake people actually make with them.*

---

## Table of Contents

**Part I — The Mental Model**
1. [What "Linux" actually means](#what-linux-actually-means)
2. [The filesystem hierarchy](#the-filesystem-hierarchy)
3. [Users, groups, and the permission model](#users-groups-and-the-permission-model)
4. [The shell: what it is, and what runs when it starts](#the-shell-what-it-is-and-what-runs-when-it-starts)

**Part II — Everyday Commands**
5. [Navigation and file management](#navigation-and-file-management)
6. [Viewing and editing files](#viewing-and-editing-files)
7. [Text processing: grep, sed, awk, and pipes](#text-processing-grep-sed-awk-and-pipes)

**Part III — Processes & System Management**
8. [Processes: ps, top, kill, jobs, and systemd](#processes-ps-top-kill-jobs-and-systemd)
9. [Package management](#package-management)
10. [Disk and storage](#disk-and-storage)

**Part IV — Networking**
11. [Networking commands](#networking-commands)
12. [SSH: keys, config, and tunneling](#ssh-keys-config-and-tunneling)

**Part V — Security & Automation**
13. [sudo, ACLs, and firewalls](#sudo-acls-and-firewalls)
14. [Shell scripting that survives contact with production](#shell-scripting-that-survives-contact-with-production)
15. [cron: scheduling recurring jobs](#cron-scheduling-recurring-jobs)

**Part VI — Working With the Tools Day to Day**
16. [Logs: journalctl and /var/log](#logs-journalctl-and-varlog)
17. [Archiving and compression](#archiving-and-compression)
18. [Environment variables, PATH, and shell config files](#environment-variables-path-and-shell-config-files)

**Part VII — Practice**
19. [Troubleshooting workflows](#troubleshooting-workflows)
20. [The command cheat sheet](#the-command-cheat-sheet)
21. [A learning roadmap](#a-learning-roadmap)
22. [Projects to build, in order](#projects-to-build-in-order)

---

## Part I — The Mental Model

### What "Linux" actually means

"Linux" strictly refers only to the **kernel** — the program that talks directly to hardware, manages memory, schedules processes, and mediates every other program's access to the machine. What most people call "Linux" (Ubuntu, Debian, Fedora, Alpine) is a **distribution**: that kernel plus a package manager, a set of default utilities (usually GNU coreutils), an init system, and a chosen default shell — bundled and configured by a different organization for each distro.

This matters practically because commands split into two categories: ones that behave **identically everywhere** because they come from the kernel or POSIX standard, and ones that differ **by distro** because they come from whatever tooling that distro chose:

| Layer | Examples | Portable across distros? |
|---|---|---|
| Kernel syscalls / POSIX utilities | `cd`, `ls`, `cat`, `grep`, file permissions | Yes — effectively universal |
| Package manager | `apt` (Debian/Ubuntu), `dnf`/`yum` (Fedora/RHEL), `apk` (Alpine) | No — different command, different package names |
| Init system | `systemctl` (systemd — most modern distros), `service` (older SysV-style) | Mostly yes now (systemd has become near-universal), but not guaranteed |
| Default shell | `bash` (most), `dash` (Debian's `/bin/sh`, faster but stricter POSIX-only), `zsh` (macOS default, some distros) | No — scripts written for `bash` features can silently break under `dash` |

> **Why this matters.** A `#!/bin/bash` shebang line at the top of a script is not decoration — it's what guarantees your script runs under bash specifically, with bash's extensions (arrays, `[[ ]]`, `set -o pipefail`), instead of whatever `/bin/sh` happens to symlink to on that particular machine (which on Debian/Ubuntu is `dash`, not `bash`, and silently rejects bash-only syntax with a confusing error).

### The filesystem hierarchy

Everything in Linux is addressed from a single root, `/` — there's no concept of separate drive letters like `C:\`. The **Filesystem Hierarchy Standard (FHS)** is the convention (not a hard rule, but followed almost universally) for what lives where:

| Path | What lives there |
|---|---|
| `/bin`, `/usr/bin` | Essential user command binaries (`ls`, `cat`, `bash` itself) |
| `/sbin`, `/usr/sbin` | System administration binaries, typically requiring root (`fdisk`, `iptables`) |
| `/etc` | System-wide configuration files — almost never binaries, almost always plain text you can `cat` and edit |
| `/var` | Variable data that changes at runtime: logs (`/var/log`), package caches, mail spools |
| `/home` | Per-user home directories (`/home/pratham`) |
| `/root` | The root user's home directory — deliberately outside `/home` |
| `/tmp` | Temporary files — many distros clear this on every reboot; never store anything you need to survive a restart here |
| `/proc` | Not a real filesystem on disk — a live, virtual view into kernel and process state (see below) |
| `/dev` | Device files — hardware (disks, terminals, `/dev/null`) represented as files you can read/write |
| `/opt` | Third-party/optional software installed outside the package manager's usual locations |
| `/usr` | The bulk of installed software and its supporting files — despite the name, not "user" home directories |

**`/proc` deserves a special mention** because it's genuinely unusual and shows up constantly in debugging: it's a virtual filesystem generated live by the kernel, not stored on disk, that exposes running-system internals as if they were files:

```bash
cat /proc/cpuinfo        # CPU details — model, core count, flags
cat /proc/meminfo        # exact memory breakdown, more detail than `free`
cat /proc/1234/status    # everything about process 1234 — state, memory, open files
ls /proc/1234/fd/        # every file descriptor process 1234 currently has open
```

> **Why this matters.** Tools like `ps`, `top`, and `free` aren't doing anything magic — they're parsing files under `/proc` and formatting the output nicely. Knowing this means when a friendlier tool isn't available (a minimal container image, a locked-down environment) you can always fall back to reading `/proc` directly.

### Users, groups, and the permission model

Every file has exactly one owning user and one owning group, and permissions are expressed as three sets of three bits — read/write/execute for the **owner**, the **group**, and **everyone else**:

```bash
ls -l app.sh
# -rwxr-xr-- 1 pratham devs 220 Jul  3 10:00 app.sh
#  │└┬┘└┬┘└┬┘
#  │ │  │  └─ others: r-- (read only)
#  │ │  └──── group:  r-x (read + execute)
#  │ └─────── owner:  rwx (read + write + execute)
#  └───────── file type: - (regular file), d (directory), l (symlink)
```

```bash
chmod 754 app.sh        # numeric form: owner=7(rwx), group=5(r-x), others=4(r--)
chmod u+x app.sh        # symbolic form: add execute for the owner only
chmod g-w,o-r app.sh    # remove write from group, remove read from others

chown pratham:devs app.sh   # change both owner and group in one command
```

**The numeric permission bits, memorized once:**

| Bit | Permission | Value |
|---|---|---|
| r | read | 4 |
| w | write | 2 |
| x | execute | 1 |

Add the bits you want per category: `rwx` = 4+2+1 = 7, `r-x` = 4+1 = 5, `r--` = 4.

> **Mistake.** `chmod 777` "to make it work." Granting write access to *everyone* on a file — especially a script, a config file, or anything in a shared directory — means any other user or compromised process on the system can modify it. The fix for "permission denied" is almost always to grant the *specific* bit actually needed (often just `+x` to make a script executable) to the *specific* party that needs it (often just the owner), not to open the file to the world.

**`sudo` vs. the root user directly** — `sudo` runs a single command with elevated privileges and logs exactly who ran what and when (`/var/log/auth.log` or equivalent); logging in *as* root directly (or leaving a root shell open) loses that per-command audit trail entirely, which is why production systems almost universally disable direct root login and require `sudo` instead.

**setuid, setgid, and the sticky bit** — three special permission bits that show up rarely but explain otherwise-confusing behavior:

```bash
ls -l /usr/bin/passwd
# -rwsr-xr-x — the 's' instead of 'x' in the owner slot is setuid:
# this program runs with the FILE OWNER's privileges (root), not the
# calling user's — which is exactly how a normal user is able to change
# their own password despite /etc/shadow being unreadable/unwritable by them
```

```bash
chmod +t /shared/dropbox
# the sticky bit on a shared, world-writable directory means users can only
# delete/rename files THEY own inside it, even though everyone can write there —
# this is why /tmp doesn't let one user delete another user's temp files
```

### The shell: what it is, and what runs when it starts

The shell is just another program — it reads commands you type, parses them, and asks the kernel to run them. `bash` is the near-universal default on Linux servers. When bash starts, it reads configuration files in a specific order that trips people up constantly:

| File | Read when |
|---|---|
| `/etc/profile`, `~/.bash_profile` (or `~/.profile`) | A **login shell** starts (SSH-ing in, or a fresh terminal that logs you in) |
| `~/.bashrc` | Every **new interactive shell** (a new terminal tab, but not necessarily a fresh login) |
| `~/.bash_logout` | On logout from a login shell |

> **The classic confusion.** You add an alias or an environment variable to `~/.bashrc`, SSH into the server, and it's not there — because SSH starts a *login* shell, which reads `~/.bash_profile`, not `~/.bashrc`, unless `~/.bash_profile` explicitly sources it (`source ~/.bashrc` — a line present by default on Ubuntu, but not guaranteed on every distro). When "it works in a new terminal tab but not over SSH" (or vice versa), this is almost always the reason.

`PATH` is the environment variable that determines which directories the shell searches, in order, when you type a bare command name:

```bash
echo $PATH
# /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

which node          # shows the exact file that runs when you type `node`
export PATH="$HOME/.local/bin:$PATH"   # prepend a directory — searched FIRST
```

> **Mistake.** Installing a tool and getting `command not found` even though the binary exists on disk. The binary's directory simply isn't in `PATH` — `which` will show nothing, and the fix is adding that directory to `PATH` (usually in `~/.bashrc` for a permanent fix), not reinstalling the tool.

---

## Part II — Everyday Commands

### Navigation and file management

```bash
pwd                      # print current directory
cd /var/log               # absolute path
cd ../sibling-dir          # relative path
cd -                      # jump back to the PREVIOUS directory — genuinely useful, easy to forget
cd ~                      # home directory (also just `cd` with no argument)

ls -la                    # -l long format, -a include hidden (dotfiles) — the combination you'll type constantly
ls -lh                    # -h human-readable sizes (KB/MB/GB instead of raw bytes)
ls -lt                    # sort by modification time, newest first

mkdir -p a/b/c            # -p creates parent directories as needed, no error if they exist
touch newfile.txt          # create an empty file, or update an existing file's modified time

cp file.txt backup.txt
cp -r project/ project-backup/    # -r required to copy a directory recursively

mv old-name.txt new-name.txt      # rename (mv is also how you rename — there's no separate `rename` command in core coreutils)
mv file.txt /var/backups/         # move into a directory

rm file.txt
rm -r old-directory/              # recursive delete — required for non-empty directories
rm -rf old-directory/             # -f suppresses confirmation/errors — the most dangerous common flag combo in Linux

find . -name "*.log"              # find files by name pattern, recursively from current dir
find . -mtime -7                  # files modified in the last 7 days
find . -size +100M                # files larger than 100MB
find . -name "*.tmp" -delete      # find AND delete in one command

ln -s /opt/app/current /opt/app/live   # symlink — a pointer to another path, breaks if the target moves
ln original.txt hardlink.txt            # hard link — a second name for the SAME inode, survives the original being deleted
```

> **`rm -rf` is the single most dangerous everyday command — treat it with active suspicion, not muscle memory.** There is no trash can, no confirmation by default, and no undo. Two specific habits prevent the classic disasters: never run `rm -rf` with a variable that could be empty (`rm -rf $DIR/*` when `$DIR` is unset deletes from `/*`, i.e. the entire filesystem root — this is a real, repeatedly-documented cause of catastrophic data loss), and always double-check the path with `ls` or `echo` before swapping `ls` for `rm` in a command you just verified.

> **Symlink vs. hard link, the difference that actually matters day to day.** A symlink is a separate file containing a path — delete or move the original, and the symlink is now broken (dangling). A hard link is a second directory entry pointing at the exact same inode (the same data on disk) — deleting the "original" name leaves the data fully intact under the hard link's name, because there was never an "original" in a privileged sense, just two names for one thing. Hard links can't cross filesystems/partitions or point at directories; symlinks can do both.

### Viewing and editing files

```bash
cat file.txt              # dump the whole file to stdout — fine for short files
less file.txt              # paginated viewer — use for anything long; arrow keys, / to search, q to quit
head -n 20 file.txt         # first 20 lines
tail -n 20 file.txt         # last 20 lines
tail -f /var/log/app.log    # follow a file live as new lines are appended — the standard way to watch a log during a deploy

diff old.conf new.conf      # line-by-line difference between two files
diff -u old.conf new.conf   # unified format — the format used in patches/PRs, more readable for review
```

**`less` vs. `cat` for anything non-trivial** — `cat`-ing a 50,000-line log file floods your terminal scrollback and is slow; `less` loads the file lazily and lets you search (`/pattern`, then `n` for next match) without ever pulling the whole file into your terminal buffer at once.

**vim, the minimum to not get stuck** — vim (or its lighter cousin, `vi`, present on essentially every Linux system with nothing installed) has two modes, and the #1 beginner problem is being stuck in the wrong one:

```
i        — enter INSERT mode (start typing) from NORMAL mode
Esc      — return to NORMAL mode from INSERT mode
:wq      — write (save) and quit, from NORMAL mode
:q!      — quit WITHOUT saving, from NORMAL mode
```

> **Why vim specifically is worth knowing at all.** `nano` is friendlier but isn't guaranteed to be installed on a minimal server or container image, while `vi`/`vim` essentially always is. Knowing just `i`, `Esc`, `:wq`, and `:q!` is enough to survive editing a config file over SSH on any machine you'll ever encounter, even if you never learn vim's deeper features.

### Text processing: grep, sed, awk, and pipes

The Unix philosophy in one sentence: small tools that each do one thing, connected with `|` (pipe), which feeds one command's stdout into the next command's stdin.

```bash
# grep — find lines matching a pattern
grep "ERROR" app.log
grep -i "error" app.log        # -i case-insensitive
grep -r "TODO" ./src           # -r recursive through a directory
grep -v "DEBUG" app.log        # -v invert — show lines that DON'T match
grep -c "ERROR" app.log         # -c count matching lines instead of printing them
grep -n "ERROR" app.log         # -n show line numbers

# sed — stream editor, most commonly used for find-and-replace
sed 's/foo/bar/' file.txt           # replace the FIRST occurrence per line
sed 's/foo/bar/g' file.txt          # replace EVERY occurrence per line (g = global)
sed -i 's/DEBUG/INFO/g' config.yml  # -i edits the file IN PLACE — no output shown, file is modified directly

# awk — pattern-driven text processing, most commonly used for column extraction
awk '{print $1}' access.log          # print the first whitespace-separated column
awk -F',' '{print $2}' data.csv      # -F sets the field separator to comma
awk '$3 > 100 {print $1}' data.txt   # print column 1, only for rows where column 3 > 100

# combined into a real pipeline — this exact shape is extremely common
cat access.log | grep "GET" | awk '{print $1}' | sort | uniq -c | sort -rn | head -10
# translation: find GET requests, extract the IP column, count occurrences of each
# unique IP, sort by count descending, show the top 10 — a one-liner for
# "who's hitting this endpoint the most"
```

```bash
cut -d',' -f2 data.csv        # extract field 2 from comma-delimited data — simpler than awk for pure column extraction
sort file.txt                 # alphabetical sort
sort -n numbers.txt            # numeric sort (alphabetical sort puts "10" before "9" — -n fixes this)
sort -rn numbers.txt            # reverse numeric (highest first)
uniq -c sorted.txt              # collapse consecutive duplicate lines, prefixed with a count — MUST be sorted first
wc -l file.txt                  # count lines
xargs                           # build and run a command from stdin
find . -name "*.tmp" | xargs rm    # feed found filenames directly as arguments to rm
```

> **`sed -i` mistake worth flagging explicitly.** It edits the file in place with no confirmation and no built-in undo (on GNU sed; BSD/macOS sed even requires an extra argument for backup handling, a frequent cross-platform gotcha). Test a `sed` substitution without `-i` first, confirm the output looks right, and only then add `-i` — or use `sed -i.bak` to keep an automatic backup copy alongside the edit.

> **Why `uniq -c` requires sorted input first.** `uniq` only collapses *consecutive* duplicate lines — it has no memory of lines it saw earlier in the file. `sort | uniq -c` is the standard idiom precisely because sorting guarantees every occurrence of a given line becomes consecutive, which is the only way `uniq` can count them correctly.

---

## Part III — Processes & System Management

### Processes: ps, top, kill, jobs, and systemd

Every running program is a process with a numeric PID (process ID), a parent process, and a state.

```bash
ps aux                     # every process on the system, in the traditional wide format
ps aux | grep node          # find a specific process
ps -ef --forest             # show the parent/child process tree

top                         # live, auto-refreshing view of CPU/memory usage per process
htop                        # a friendlier, colorized version of top (often needs separate install)

kill 1234                   # send SIGTERM to PID 1234 — asks it to shut down gracefully
kill -9 1234                 # send SIGKILL — the kernel force-kills it immediately, no cleanup
killall node                 # kill every process matching a name, not just one PID

nice -n 10 ./heavy-script.sh    # start a process with LOWER scheduling priority (higher number = lower priority)
renice -n 5 -p 1234              # change an already-running process's priority

jobs                          # list background/suspended jobs in the current shell session
./long-task.sh &               # run in the background, return control to the shell immediately
fg %1                          # bring background job 1 to the foreground
Ctrl+Z then bg                 # suspend the foreground process, then resume it running in the background

nohup ./long-task.sh &          # run a command that keeps running even after you log out / close the terminal
```

> **SIGTERM vs. SIGKILL — the distinction that matters for anything stateful.** `kill` (SIGTERM) is a *request* the process can catch and respond to — closing open files, finishing an in-flight database write, releasing a lock — before exiting. `kill -9` (SIGKILL) is handled entirely by the kernel and gives the process **zero chance to clean up anything**, which is why forcefully `-9`-killing a database or any process mid-write is a real, direct cause of corrupted data. Always try plain `kill` first and only escalate to `-9` if the process genuinely won't respond after a reasonable wait.

**systemd/systemctl** — the init system (PID 1, the first process the kernel starts, parent of everything else) on essentially every modern distro, and the standard way services are started, stopped, and kept running:

```bash
systemctl status nginx          # is it running, and its recent log lines
systemctl start nginx
systemctl stop nginx
systemctl restart nginx
systemctl reload nginx           # re-read config WITHOUT dropping current connections — nginx/most servers support this
systemctl enable nginx           # start automatically on boot
systemctl disable nginx

journalctl -u nginx -f           # follow this service's logs live (see Part VI)
```

A minimal service unit file — the thing that turns "run this script" into something `systemctl` can manage, auto-restart, and log:

```ini
# /etc/systemd/system/myapp.service
[Unit]
Description=My Application
After=network.target

[Service]
ExecStart=/usr/bin/node /opt/myapp/server.js
Restart=on-failure
User=appuser
Environment=NODE_ENV=production

[Install]
WantedBy=multi-user.target
```

```bash
systemctl daemon-reload    # required after creating/editing a unit file, before systemctl can see it
systemctl enable --now myapp.service   # enable on boot AND start it immediately
```

> **`Restart=on-failure` is the single line that turns a script into a real, production-grade service.** Without it, if your Node process crashes at 3 AM, it simply stays dead until a human notices and restarts it by hand — with it, systemd restarts the process automatically, and `journalctl -u myapp` gives you a complete, timestamped history of every crash and restart to investigate later.

### Package management

```bash
# Debian/Ubuntu — apt
sudo apt update                  # refresh the list of available packages/versions (does NOT install anything)
sudo apt upgrade                 # actually upgrade installed packages to their latest available version
sudo apt install nginx
sudo apt remove nginx             # remove the package, keep its config files
sudo apt purge nginx              # remove the package AND its config files
apt list --installed | grep nginx

# RHEL/Fedora/CentOS — dnf (yum on older systems)
sudo dnf install nginx
sudo dnf remove nginx
sudo dnf update

# Alpine (common in minimal Docker images) — apk
apk add curl
apk del curl
```

> **`apt update` vs. `apt upgrade` — the single most common package-management confusion.** `update` only refreshes apt's local *index* of what versions exist in the configured repositories — it changes nothing on disk beyond that index. `upgrade` is the command that actually installs newer versions of packages you already have. Running `upgrade` without an `update` first upgrades against a stale, possibly outdated index; forgetting this two-step is why "I ran apt but nothing updated" is almost always a missing `apt update`.

### Disk and storage

```bash
df -h                   # disk space used/free PER MOUNTED FILESYSTEM — the first command to run when something mysteriously fails
du -sh /var/log           # total size of a specific directory (-s summarize, -h human-readable)
du -sh /var/log/* | sort -rh | head -10   # the 10 largest things inside /var/log — the standard "what's eating my disk" one-liner

lsblk                    # list block devices (disks and their partitions) in a tree view
mount /dev/sdb1 /mnt/data    # attach a filesystem at a mount point
umount /mnt/data              # detach it — must not be in use (a process with an open file/cwd there) or it fails

fdisk -l                 # list partition tables — requires root
```

> **"No space left on device" with `df -h` showing free space — the inode exhaustion gotcha.** A filesystem can run out of **inodes** (the fixed-size table entries that track file metadata) even with plenty of raw byte space left, typically from directories accumulating millions of tiny files (a common failure mode for unrotated log/cache/session directories). `df -i` (not `df -h`) shows inode usage specifically — checking only `df -h` and seeing free space is a real, recurring source of "but there's space left, why is it saying disk full?" confusion.

---

## Part IV — Networking

### Networking commands

```bash
ip addr show              # modern replacement for `ifconfig` — shows every interface and its IP addresses
ip route show               # the routing table — which interface/gateway traffic for a given destination goes through

ping google.com             # basic reachability + latency check
ping -c 4 google.com         # send exactly 4 pings then stop (default is to run forever until Ctrl+C)

curl -I https://example.com    # -I fetch headers only, fast way to check if a site/API is up and what it returns
curl -v https://example.com     # -v verbose — shows the full TLS handshake and redirect chain, essential for debugging HTTPS issues
curl -X POST -d '{"key":"value"}' -H "Content-Type: application/json" https://api.example.com/endpoint

wget https://example.com/file.tar.gz   # download a file directly to disk

ss -tulpn                   # modern replacement for `netstat` — TCP/UDP listening sockets, with process names (-p needs root)
# -t TCP, -u UDP, -l listening only, -p show process, -n numeric ports (skip slow DNS lookups on service names)

dig example.com                # full DNS resolution detail — the record found, the authoritative server, timing
dig example.com +short          # just the resolved IP, nothing else
nslookup example.com            # older, simpler alternative to dig

scp file.txt user@server:/remote/path/       # copy a file TO a remote server over SSH
scp user@server:/remote/file.txt ./          # copy a file FROM a remote server
rsync -avz ./localdir/ user@server:/remote/dir/   # sync directories — only transfers CHANGED data, far faster than scp for repeated syncs
# -a archive mode (preserves permissions/timestamps/symlinks), -v verbose, -z compress during transfer
```

> **Mistake.** Trusting `ping` alone to diagnose "the server is down." Many production servers and load balancers deliberately block ICMP (what `ping` uses) for security/DDoS-mitigation reasons while the actual service (HTTP on port 443) is completely healthy — a failed `ping` with a working `curl` to the same host is common and not a contradiction. Diagnose application-layer reachability with `curl`, not `ping`.

> **`rsync` vs. `scp` for anything beyond a one-off single file.** `scp` re-transfers the entire file every time, full stop. `rsync` compares source and destination and transfers only the bytes that actually changed, which matters enormously for repeated deploys of a large directory (most of which hasn't changed since the last deploy) — this is why deployment scripts almost universally use `rsync`, not `scp`, once a project grows past trivial size.

### SSH: keys, config, and tunneling

```bash
ssh-keygen -t ed25519 -C "you@example.com"   # generate a new keypair (ed25519 — modern, fast, preferred over older RSA)
ssh-copy-id user@server                        # copy your public key to a server's authorized_keys, enabling passwordless login
ssh user@server                                 # connect
ssh -i ~/.ssh/specific_key user@server            # use a specific private key file instead of the default
```

**`~/.ssh/config`** — the file that turns a long, error-prone `ssh` command into a memorable alias, and is worth setting up on day one of using SSH regularly:

```
# ~/.ssh/config
Host prod
    HostName 203.0.113.42
    User deploy
    Port 2222
    IdentityFile ~/.ssh/prod_key
```

```bash
ssh prod          # now equivalent to: ssh -i ~/.ssh/prod_key -p 2222 deploy@203.0.113.42
```

**SSH tunneling** — using an SSH connection to forward traffic, most commonly to reach an internal service (a database, an admin dashboard) that isn't directly exposed to your machine:

```bash
# Local port forwarding: "make the remote server's port 5432 available on MY machine's port 5433"
ssh -L 5433:localhost:5432 user@server
# now connecting to localhost:5433 on your own machine actually reaches the
# remote server's port 5432 — the standard way to connect a local DB client
# to a production database that only listens on localhost on the server itself
```

> **Why key-based auth over passwords is the actual production standard, not just a suggestion.** SSH keys are effectively immune to brute-force and credential-stuffing attacks that plague password auth, and disabling password authentication entirely (`PasswordAuthentication no` in `/etc/ssh/sshd_config`) is one of the single highest-leverage server-hardening steps available — most successful SSH brute-force compromises target servers that still allow password login at all.

---

## Part V — Security & Automation

### sudo, ACLs, and firewalls

```bash
sudo visudo                     # the ONLY safe way to edit /etc/sudoers — validates syntax before saving,
                                 # preventing a typo from locking every user out of sudo entirely
```

```bash
# Access Control Lists (ACLs) — permissions beyond the basic owner/group/other model,
# for when a specific ADDITIONAL user or group needs access without changing the file's owner/group
setfacl -m u:alice:rw file.txt     # grant user alice read+write, without changing the file's actual owner
getfacl file.txt                    # view the ACLs currently set on a file
```

```bash
# ufw — the simplified firewall front-end on Ubuntu/Debian (wraps iptables/nftables underneath)
sudo ufw allow 22/tcp        # allow SSH
sudo ufw allow 443/tcp        # allow HTTPS
sudo ufw enable
sudo ufw status verbose
```

> **Mistake that locks people out of their own server.** Enabling `ufw` (or any firewall) *before* explicitly allowing the SSH port you're currently connected through. The firewall activates immediately and your existing session may survive, but any *new* SSH connection attempt is blocked — including your own next login — leaving the server unreachable except through a cloud provider's separate console/serial access. Always `allow 22/tcp` (or whatever port SSH runs on) as the very first rule, before `enable`.

### Shell scripting that survives contact with production

```bash
#!/usr/bin/env bash
set -euo pipefail
# -e: exit immediately if any command fails, instead of continuing past it
# -u: treat any reference to an unset variable as an error, instead of silently
#     substituting an empty string (this is what prevents the `rm -rf $DIR/*` disaster)
# -o pipefail: a pipeline (`cmd1 | cmd2`) fails if ANY stage fails, not just the last one

APP_DIR="${1:?Usage: $0 <app-directory>}"   # ${var:?message} exits with an error if $1 wasn't provided

if [ ! -d "$APP_DIR" ]; then
  echo "Error: $APP_DIR does not exist" >&2   # >&2 sends error output to stderr, not stdout
  exit 1
fi

cd "$APP_DIR"

for file in *.log; do
  if [ -f "$file" ]; then
    gzip "$file"
  fi
done

count=$(ls -1 *.gz 2>/dev/null | wc -l)
echo "Compressed logs. Total archives: $count"
```

**The constructs that come up constantly:**

```bash
# Conditionals
if [ "$ENV" = "production" ]; then
  echo "prod"
elif [ "$ENV" = "staging" ]; then
  echo "staging"
else
  echo "unknown"
fi

# Loops
for i in 1 2 3; do echo "$i"; done
while read -r line; do echo "$line"; done < file.txt

# Functions
deploy() {
  local target="$1"     # `local` scopes it to the function — without it, variables are global by default
  echo "Deploying to $target"
}
deploy "production"

# Command substitution and exit codes
current_branch=$(git rev-parse --abbrev-ref HEAD)
if ! command -v docker &> /dev/null; then
  echo "docker is not installed" >&2
  exit 1
fi
echo $?   # the exit code of the LAST command run — 0 means success, anything else means failure
```

> **Always quote variable expansions: `"$file"`, not `$file`.** An unquoted variable undergoes word-splitting and glob expansion — a filename containing a space (`my file.txt`) silently becomes two separate arguments to whatever command follows, which is a common, hard-to-notice source of scripts that work in testing (with simple filenames) and break in the real world (with realistic ones).

### cron: scheduling recurring jobs

```bash
crontab -e         # edit the current user's scheduled jobs
crontab -l          # list them
```

```
# minute hour day-of-month month day-of-week   command
0  2  *  *  *   /opt/scripts/backup.sh          # every day at 2:00 AM
*/15  *  *  *  *   /opt/scripts/healthcheck.sh   # every 15 minutes
0  0  1  *  *   /opt/scripts/monthly-report.sh    # midnight on the 1st of every month
```

> **Mistake.** A cron job that works perfectly when run manually from an interactive shell, then fails silently when run by cron. Cron runs jobs with a **minimal environment** — no `PATH` beyond a bare default, none of your `~/.bashrc` customizations, no assumption about the current working directory. The fix is always to use absolute paths for every command and file reference inside a cron script (`/usr/bin/node`, not `node`; `cd /opt/myapp &&`, not relying on the script's own directory), and to redirect output to a log file (`>> /var/log/myjob.log 2>&1`) so a silent failure is at least visible after the fact.

---

## Part VI — Working With the Tools Day to Day

### Logs: journalctl and /var/log

```bash
journalctl -u nginx              # all logs for the nginx service, oldest first
journalctl -u nginx -f            # follow live, like `tail -f`
journalctl -u nginx --since "1 hour ago"
journalctl -u nginx -p err         # only entries at "error" priority or more severe
journalctl --disk-usage             # how much disk the journal itself is consuming
journalctl --vacuum-time=7d          # delete journal entries older than 7 days — logs themselves can fill a disk

tail -f /var/log/syslog             # traditional plain-text system log (path varies: /var/log/messages on RHEL-family)
tail -f /var/log/auth.log            # authentication attempts — the first place to check for suspicious SSH activity
```

> **Why journalctl matters beyond "it's the modern way."** Plain-text logs under `/var/log` are just files — grep/tail/sed all work directly. `journalctl`'s structured, indexed format is what makes `--since`, `-p` (priority filtering), and `-u` (per-service filtering) fast and precise even across gigabytes of history, instead of grepping a giant flat file by hand every time.

### Archiving and compression

```bash
tar -czvf archive.tar.gz ./project/     # create: -c create, -z gzip, -v verbose, -f filename (always last)
tar -xzvf archive.tar.gz                 # extract
tar -tzvf archive.tar.gz                  # list contents WITHOUT extracting — check before you extract into a messy directory

gzip file.txt          # compress in place — produces file.txt.gz, deletes the original
gunzip file.txt.gz      # decompress in place

zip -r archive.zip ./project/
unzip archive.zip
```

> **`tar` flag order mnemonic.** `czvf` reads as "create, gzip, verbose, file" — and `f` (specifying the filename) must always come last in the flag group, immediately before the filename argument, because everything after `f` is interpreted as that filename. Forgetting this and writing `-fczv archive.tar.gz` treats `czv` as part of the filename instead of as flags — a common, confusing beginner error.

### Environment variables, PATH, and shell config files

```bash
export API_KEY="secret"          # set for the current shell session AND any child processes it spawns
echo $API_KEY
env                                # list every environment variable currently set
unset API_KEY

# Persisting across sessions — add the export line to ~/.bashrc (see Part I)
echo 'export API_KEY="secret"' >> ~/.bashrc
source ~/.bashrc                    # re-read it into the CURRENT shell without opening a new terminal
```

> **Mistake.** Putting real secrets in `~/.bashrc` or any shell config file expecting them to be "private." Any process running as that user can read the file, it's frequently committed to dotfiles repos by habit, and it has no rotation or audit trail. This is the same gap Part VII of the DevOps guide addresses with a real secrets manager — shell config files are fine for non-sensitive defaults (a default AWS region, a preferred editor), not credentials.

---

## Part VII — Practice

### Troubleshooting workflows

**"The server is out of disk space."**
```bash
df -h                              # confirm which mounted filesystem is actually full
du -sh /var/log/* | sort -rh | head -10   # find the largest offenders
journalctl --disk-usage             # check if the journal itself is the culprit
df -i                               # also check inodes, not just bytes — see Part III
```

**"A service is using 100% CPU."**
```bash
top                                  # identify the PID
ps -p <PID> -o %cpu,%mem,cmd          # confirm what it actually is
strace -p <PID>                       # (if installed) see what syscalls it's making right now — often reveals a busy-loop
kill <PID>                            # graceful first; -9 only if it doesn't respond
```

**"I can't tell if the app is actually running or just the process exists."**
```bash
systemctl status myapp                # is systemd reporting it healthy?
curl -I http://localhost:3000/health    # does the app actually respond?
journalctl -u myapp -n 50               # the last 50 log lines — what was it doing right before now?
ss -tulpn | grep 3000                    # is anything actually listening on the expected port?
```

**"Something changed and I don't know what."**
```bash
history | tail -50                    # what commands were run recently, in this shell
last                                    # recent login sessions — who logged in, from where, when
diff /etc/nginx/nginx.conf /etc/nginx/nginx.conf.bak   # if a backup exists, compare directly
```

### The command cheat sheet

| Category | Commands |
|---|---|
| Navigate & manage files | `pwd`, `cd`, `ls -la`, `mkdir -p`, `cp -r`, `mv`, `rm -rf`, `find`, `ln -s` |
| View & edit | `cat`, `less`, `head`/`tail -f`, `diff -u`, `vim` |
| Text processing | `grep -rn`, `sed -i`, `awk`, `cut`, `sort`, `uniq -c`, `wc -l`, `xargs` |
| Processes | `ps aux`, `top`/`htop`, `kill`/`kill -9`, `nohup`, `systemctl` |
| Packages | `apt update && apt upgrade`, `apt install`, `dnf install` |
| Disk | `df -h`, `df -i`, `du -sh`, `lsblk`, `mount`/`umount` |
| Networking | `ip addr`, `curl -v`, `ss -tulpn`, `dig +short`, `scp`, `rsync -avz` |
| SSH | `ssh-keygen -t ed25519`, `ssh-copy-id`, `~/.ssh/config`, `ssh -L` |
| Security | `sudo visudo`, `setfacl`, `ufw allow`/`enable` |
| Logs | `journalctl -u <svc> -f`, `tail -f /var/log/...` |
| Archives | `tar -czvf` / `-xzvf`, `gzip`/`gunzip` |
| Scheduling | `crontab -e` |

### A learning roadmap

**Weeks 1–2 — the mental model**
Filesystem hierarchy, the permission model (`chmod`/`chown` until they're automatic), navigating and manipulating files without hesitation, and the difference between a login and non-login shell.

**Weeks 3–4 — text and processes**
`grep`/`sed`/`awk`/pipes fluently enough to build one-liners on the fly, plus `ps`/`top`/`kill` and reading `systemctl status` output confidently.

**Weeks 5–6 — networking and remote access**
`curl`, `ss`, `dig`, and SSH key-based auth with a real `~/.ssh/config`; practice SSH tunneling to reach a service that isn't directly exposed.

**Weeks 7–8 — automation and services**
Write a real shell script with `set -euo pipefail` and proper quoting, wire it into a systemd unit file with `Restart=on-failure`, and schedule a second one with cron, using absolute paths and logged output.

**Weeks 9–10 — putting it together**
Practice the four troubleshooting workflows above against a real VM (a free-tier cloud instance is enough) until each diagnostic sequence is second nature — the goal is speed under pressure, not just theoretical knowledge.

### Projects to build, in order

**01. A log-rotation and cleanup script**
Find and compress `.log` files older than 7 days in a directory, delete anything older than 30 days, and log its own actions.
*Skills: `find`, `tar`/`gzip`, `set -euo pipefail`, cron scheduling.*

**02. A "what's using my disk" report**
A script that reports the top 10 largest directories, current disk and inode usage, and emails or logs the result — scheduled to run daily.
*Skills: `du`, `df -h`/`df -i`, `sort`, cron.*

**03. A systemd-managed application**
Take any long-running script (a simple HTTP server is fine) and wrap it in a proper systemd unit with auto-restart, then break it on purpose and confirm systemd brings it back.
*Skills: systemd unit files, `journalctl`.*

**04. A hardened SSH setup**
Generate an ed25519 key, disable password authentication on a test VM, set up `~/.ssh/config` aliases, and firewall the box with `ufw` (allowing SSH first!).
*Skills: SSH keys, `sshd_config`, `ufw`.*

**05. A log-analysis one-liner toolkit**
Given a real access log, build the pipeline (grep → awk → sort → uniq -c) to answer: top 10 IPs by request count, top 10 requested paths, and total 5xx error count in the last hour.
*Skills: `grep`, `awk`, `sort`, `uniq -c`, `journalctl --since`.*

**06. A tunneled database connection**
Set up a database that only listens on `localhost` on a remote server, then connect to it from your local machine's DB client entirely through an SSH local port forward.
*Skills: `ssh -L`, understanding what's actually exposed vs. tunneled.*

---

*This guide covers the commands and mental models that come up constantly in real day-to-day server work. The man pages (`man <command>`) and `<command> --help` remain the authoritative reference for every flag not covered here — the goal of this guide is knowing which fifteen commands to reach for and why, not memorizing every flag of every one of them.*
