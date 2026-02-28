---
title: "Linux command list"
date: 2026-02-28
type: page
---

# Linux Commands (Basic → Advanced)

Format: **Command | Description (including flags used) | Example Command**

---

## File and Directory Commands

| Command | Description (incl. flags) | Example Command |
|---|---|---|
| `pwd` | Print working directory (no flags). | `pwd` |
| `ls` | List directory contents. `-l` long format; `-a` include hidden files. | `ls -la` |
| `cd` | Change directory (no flags). `..` = parent, `~` = home. | `cd /var/log` |
| `mkdir` | Create directory. `-p` create parent dirs as needed (no error if exists). | `mkdir -p projects/app/logs` |
| `rmdir` | Remove **empty** directory (no flags). | `rmdir old_empty_dir` |
| `touch` | Create empty file or update timestamps. `-a` access time; `-m` modify time. | `touch -am notes.txt` |
| `cp` | Copy files/dirs. `-r` recursive; `-i` prompt before overwrite. | `cp -ri src/ backup/` |
| `mv` | Move/rename. `-i` prompt before overwrite; `-n` no clobber (don’t overwrite). | `mv -i report.txt report-2026.txt` |
| `rm` | Remove files/dirs. `-r` recursive; `-f` force (no prompts). **Use carefully.** | `rm -rf build/` |
| `find` | Search for files. `-type f` files only; `-name` pattern; `-mtime -7` modified within 7 days. | `find . -type f -name "*.log" -mtime -7` |
| `locate` | Fast filename search via database. `-i` case-insensitive. | `locate -i ssh_config` |
| `updatedb` | Update `locate` database (often requires sudo). | `sudo updatedb` |
| `stat` | Show detailed file metadata (size, perms, times). | `stat /etc/hosts` |
| `file` | Detect file type by content (no flags). | `file archive.tar.gz` |
| `cat` | Print file contents. `-n` number lines. | `cat -n README.md` |
| `tac` | Print file in reverse (last line first). | `tac /var/log/syslog` |
| `less` | Page through text. `-N` show line numbers; `-S` chop long lines. | `less -NS large.log` |
| `head` | Show first lines. `-n 20` show 20 lines. | `head -n 20 access.log` |
| `tail` | Show last lines. `-n 50` show 50 lines; `-f` follow appended data. | `tail -n 50 -f app.log` |
| `grep` | Search text. `-n` show line numbers; `-i` ignore case; `-r` recursive. | `grep -rin "error" /var/log` |
| `sed` | Stream editor. `-i` edit in place (GNU sed); `s/old/new/g` substitute globally. | `sed -i 's/DEBUG/INFO/g' app.conf` |
| `awk` | Field/column processing. `-F,` set delimiter to comma. | `awk -F, '{print $1,$3}' data.csv` |
| `sort` | Sort lines. `-n` numeric; `-r` reverse. | `sort -nr scores.txt` |
| `uniq` | Remove adjacent duplicates. `-c` prefix counts. (Often used after `sort`.) | `sort words.txt \| uniq -c` |
| `tar` | Archive. `-c` create; `-z` gzip; `-v` verbose; `-f` filename. | `tar -czvf logs.tar.gz /var/log` |

---

## Networking Commands

| Command | Description (incl. flags) | Example Command |
|---|---|---|
| `ip` | Modern network tool. `addr` show addresses; `link` show interfaces. | `ip addr show` |
| `ip link` | Manage links. `set` change state; `up` enable. | `sudo ip link set eth0 up` |
| `ip route` | Show routing table (no flags). | `ip route` |
| `ping` | ICMP reachability. `-c 4` send 4 packets. | `ping -c 4 8.8.8.8` |
| `traceroute` | Trace route hops. `-n` don’t resolve DNS (faster). | `traceroute -n example.com` |
| `mtr` | Combined ping/traceroute. `-r` report mode; `-c 20` 20 cycles. | `mtr -r -c 20 example.com` |
| `nslookup` | DNS lookup (no flags). | `nslookup example.com` |
| `dig` | DNS query. `+short` compact output; `@8.8.8.8` choose resolver. | `dig @8.8.8.8 example.com A +short` |
| `host` | DNS lookup (simple). `-t MX` query MX records. | `host -t MX example.com` |
| `curl` | HTTP client. `-I` headers only; `-L` follow redirects. | `curl -IL https://example.com` |
| `wget` | Downloader. `-O` output file; `-q` quiet. | `wget -q -O index.html https://example.com` |
| `ssh` | Secure remote shell. `-p` port; `-i` identity key file. | `ssh -p 2222 -i ~/.ssh/id_ed25519 user@server` |
| `scp` | Secure copy. `-P` port; `-r` recursive. | `scp -P 2222 -r ./site user@server:/var/www/` |
| `sftp` | Interactive file transfer over SSH (no flags). | `sftp user@server` |
| `ss` | Socket statistics (replacement for `netstat`). `-t` TCP; `-u` UDP; `-l` listening; `-n` numeric; `-p` process. | `ss -tulnp` |
| `netstat` | Legacy sockets/routes. `-tulnp` similar meaning: TCP/UDP/listen/numeric/process. | `sudo netstat -tulnp` |
| `lsof` | List open files. `-i :80` show processes using TCP/UDP port 80. | `sudo lsof -i :80` |
| `nc` | Netcat for testing. `-z` zero-I/O scan; `-v` verbose; `-w 2` timeout. | `nc -zvw 2 example.com 443` |
| `nmap` | Network scanner. `-sS` SYN scan; `-p 1-1000` ports; `-Pn` no ping. | `sudo nmap -sS -p 1-1000 -Pn 10.0.0.5` |
| `tcpdump` | Packet capture. `-i` interface; `-nn` no name/port resolve; `-c 50` packets. | `sudo tcpdump -i eth0 -nn -c 50 port 53` |
| `iptables` | Legacy firewall. `-A` append rule; `-p` protocol; `--dport` dest port; `-j` action. | `sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT` |
| `ufw` | Uncomplicated firewall. `allow 22/tcp` allow SSH. | `sudo ufw allow 22/tcp` |
| `nmcli` | NetworkManager CLI. `dev status` device status. | `nmcli dev status` |
| `ethtool` | NIC details. (Example reads settings; some options require root.) | `sudo ethtool eth0` |
| `openssl s_client` | Inspect TLS connection. `-connect host:port` target; `-servername` SNI. | `openssl s_client -connect example.com:443 -servername example.com` |

---

## Process and System Monitoring Commands

| Command | Description (incl. flags) | Example Command |
|---|---|---|
| `uname` | Kernel/system info. `-a` all details. | `uname -a` |
| `uptime` | System uptime and load averages (no flags). | `uptime` |
| `date` | Show/set date. `+FORMAT` custom output format. | `date +"%Y-%m-%d %H:%M:%S"` |
| `whoami` | Current username (no flags). | `whoami` |
| `w` | Who is logged in + activity (no flags). | `w` |
| `top` | Interactive process viewer (no flags). | `top` |
| `htop` | Enhanced interactive process viewer (no flags; if installed). | `htop` |
| `ps` | Process snapshot. `aux` = all users, user-oriented format, include processes without TTY. | `ps aux` |
| `pgrep` | Find PID(s) by name. `-a` show full command line. | `pgrep -a nginx` |
| `pkill` | Kill by name. `-f` match full command line (use carefully). | `pkill -f "python app.py"` |
| `kill` | Send signal to PID. `-9` SIGKILL (force); prefer `-15` (default) when possible. | `kill -9 12345` |
| `killall` | Kill processes by name. `-i` interactive prompt. | `killall -i chrome` |
| `nice` | Start process with priority. `-n 10` set niceness +10 (lower priority). | `nice -n 10 make` |
| `renice` | Change running process priority. `-n` new niceness; `-p` target PID. | `sudo renice -n -5 -p 12345` |
| `free` | Memory usage. `-h` human-readable. | `free -h` |
| `vmstat` | Virtual memory & CPU stats. `1 5` every 1s, 5 times. | `vmstat 1 5` |
| `iostat` | CPU/disk I/O stats (sysstat). `-x` extended; `1 3` interval/count. | `iostat -x 1 3` |
| `df` | Disk free. `-h` human-readable; `-T` filesystem type. | `df -hT` |
| `du` | Disk usage. `-h` human-readable; `-s` summary; `--max-depth=1` one level. | `du -hs --max-depth=1 /var` |
| `lsblk` | List block devices. `-f` include filesystem info. | `lsblk -f` |
| `mount` | Show mounts (no flags). | `mount` |
| `dmesg` | Kernel ring buffer. `-T` human timestamps; `-l err,warn` filter levels. | `dmesg -T -l err,warn` |
| `journalctl` | systemd logs. `-u` unit; `-p` priority; `-b` current boot. | `journalctl -u ssh -p warning -b` |
| `systemctl` | systemd service manager. `status` show state; `--no-pager` avoid pager. | `systemctl status nginx --no-pager` |
| `sar` | Historic stats (sysstat). `-u` CPU; `1 3` interval/count. | `sar -u 1 3` |

---

## User and Permission Management Commands

| Command | Description (incl. flags) | Example Command |
|---|---|---|
| `id` | Show user/group IDs. `-u` user ID; `-n` name instead of number. | `id -un` |
| `who` | Show logged-in users. `-a` all details. | `who -a` |
| `groups` | Show groups for a user (no flags). | `groups alice` |
| `useradd` | Create user. `-m` create home; `-s` login shell. | `sudo useradd -m -s /bin/bash devuser` |
| `usermod` | Modify user. `-aG` append to supplementary groups (don’t replace). | `sudo usermod -aG sudo devuser` |
| `userdel` | Delete user. `-r` remove home and mail spool. | `sudo userdel -r devuser` |
| `passwd` | Set/change password. `-l` lock; `-u` unlock. | `sudo passwd -l devuser` |
| `chage` | Password aging. `-l` list settings. | `sudo chage -l devuser` |
| `groupadd` | Create group (no flags). | `sudo groupadd developers` |
| `groupmod` | Modify group. `-n` rename group. | `sudo groupmod -n devs developers` |
| `groupdel` | Delete group (no flags). | `sudo groupdel devs` |
| `gpasswd` | Manage group membership. `-a` add user; `-d` delete user. | `sudo gpasswd -a devuser docker` |
| `su` | Switch user. `-` start a login shell (load target env). | `su - root` |
| `sudo` | Run as another user (usually root). `-u` specify user; `-k` invalidate cached credentials. | `sudo -u postgres psql` |
| `visudo` | Safely edit sudoers (syntax-checked). | `sudo visudo` |
| `chmod` | Change permissions. `-R` recursive. (Example uses numeric mode.) | `chmod -R 750 /opt/app` |
| `chown` | Change owner/group. `-R` recursive; `user:group` ownership spec. | `sudo chown -R appuser:appgroup /opt/app` |
| `chgrp` | Change group owner. `-R` recursive. | `sudo chgrp -R developers /srv/repos` |
| `umask` | Set default permission mask (no flags). (Example sets mask to 027.) | `umask 027` |
| `getfacl` | View ACLs (no flags). | `getfacl /srv/shared` |
| `setfacl` | Set ACLs. `-m` modify ACL; `u:user:perms` user entry. | `sudo setfacl -m u:devuser:rwx /srv/shared` |
| `sudoers.d` | Best practice: drop-in file for sudo rules (no flags). | `echo "devuser ALL=(ALL) NOPASSWD:/usr/bin/systemctl" \| sudo tee /etc/sudoers.d/devuser` |
| `ssh-keygen` | Generate SSH keypair. `-t` type; `-b` bits; `-C` comment. | `ssh-keygen -t ed25519 -C "devuser@laptop"` |
| `ssh-copy-id` | Copy SSH public key. `-i` identity file to install. | `ssh-copy-id -i ~/.ssh/id_ed25519.pub user@server` |

---

### Notes

- Some commands (e.g., `iptables`, `tcpdump`, user management) typically require `sudo`.
- Flags can vary slightly between GNU/Linux distributions (especially `sed -i` behaviour); check `man <command>` for your system.
