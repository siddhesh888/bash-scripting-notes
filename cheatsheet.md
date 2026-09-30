[Linux-DB-DevOps-DevSecOps-Docker-K8s-Complete-Reference.md](https://github.com/user-attachments/files/32840923/Linux-DB-DevOps-DevSecOps-Docker-K8s-Complete-Reference.md)
# The Complete Linux, Database, DevOps & DevSecOps Command Reference
### Linux Administration • MariaDB • PostgreSQL • Docker • Kubernetes • DevOps • DevSecOps

> A single deep-dive reference, organized **basic → intermediate → advanced**, for daily operations, troubleshooting, automation, and production hardening.

---

## Table of Contents

1. [Linux Fundamentals](#1-linux-fundamentals)
2. [Linux System Administration](#2-linux-system-administration)
3. [Advanced Linux Administration & Performance](#3-advanced-linux-administration--performance)
4. [Linux Security Hardening](#4-linux-security-hardening)
5. [Shell Scripting & Automation](#5-shell-scripting--automation)
6. [MariaDB / MySQL Database Administration](#6-mariadb--mysql-database-administration)
7. [PostgreSQL Database Administration](#7-postgresql-database-administration)
8. [DevOps Toolchain (Git, CI/CD, IaC, Config Mgmt, Monitoring)](#8-devops-toolchain)
9. [DevSecOps (Security-in-Pipeline)](#9-devsecops)
10. [Docker — Basic to Advanced](#10-docker)
11. [Kubernetes — Basic to Advanced](#11-kubernetes)
12. [Quick-Reference Cheat Sheets](#12-quick-reference-cheat-sheets)

---

# 1. Linux Fundamentals

## 1.1 Getting Help
```bash
man <command>            # manual page
man -k keyword           # search man pages (same as apropos)
<command> --help         # quick usage
info <command>           # GNU info docs
whatis <command>         # one-line description
tldr <command>           # community examples (needs tldr installed)
type <command>           # shows if alias/builtin/binary
which <command>          # path to executable
```

## 1.2 Navigating the Filesystem
```bash
pwd                       # print working directory
ls -la                    # list all files, long format
ls -lh                    # human-readable sizes
cd /path/to/dir           # change directory
cd -                      # go to previous directory
cd ~                      # home directory
tree -L 2                 # directory tree, 2 levels deep
```

## 1.3 File & Directory Operations
```bash
touch file.txt            # create empty file / update timestamp
mkdir dirname             # create directory
mkdir -p a/b/c            # create nested directories
cp file1 file2            # copy file
cp -r dir1 dir2           # copy directory recursively
cp -a src dst             # archive copy (preserves perms, links, times)
mv old new                # move/rename
rm file                   # delete file
rm -rf dir/               # force recursive delete (DANGEROUS)
rmdir dir                 # remove empty directory
ln -s target linkname     # create symbolic link
ln target linkname        # create hard link
find /path -name "*.log"  # find files by name
find /path -mtime -7      # modified in last 7 days
find /path -size +100M    # files larger than 100MB
locate filename           # fast search using mlocate DB (updatedb first)
```

## 1.4 Viewing & Editing File Content
```bash
cat file                  # print whole file
tac file                  # print file reversed (line order)
less file                 # paginated view (q to quit, / to search)
head -n 20 file           # first 20 lines
tail -n 20 file           # last 20 lines
tail -f file              # follow file (live logs)
tail -F file              # follow, re-attach on rotation
nl file                   # print with line numbers
wc -l file                # count lines
wc -w file                # count words
grep "pattern" file       # search lines matching pattern
grep -r "pattern" dir/    # recursive search
grep -i "pattern" file    # case-insensitive
grep -v "pattern" file    # invert match
grep -E "regex" file      # extended regex
diff file1 file2          # compare files
vimdiff file1 file2       # visual diff
sed 's/foo/bar/g' file    # stream editor, replace text
awk '{print $1}' file     # column/field processing
sort file                 # sort lines
sort -n file              # numeric sort
sort -k2 -t, file         # sort by 2nd column, comma delimiter
uniq file                 # remove adjacent duplicate lines
cut -d',' -f1,3 file      # extract columns 1 and 3 (CSV)
tr 'a-z' 'A-Z' < file     # translate characters
xxd file                  # hex dump
strings file              # print printable strings (binaries)
nano file / vim file      # text editors
```

## 1.5 Permissions & Ownership
```bash
chmod 755 file                 # rwxr-xr-x
chmod u+x file                 # add execute for owner
chmod -R 644 dir/              # recursive change
chown user:group file          # change owner and group
chown -R user:group dir/       # recursive ownership change
chgrp group file                # change group only
umask                           # show default permission mask
umask 022                       # set default mask
stat file                       # detailed file metadata
getfacl file                    # view ACLs
setfacl -m u:user:rwx file      # set ACL for specific user
lsattr file                     # list extended attributes
chattr +i file                  # make file immutable
```

## 1.6 Compression & Archiving
```bash
tar -cvf archive.tar dir/         # create tar archive
tar -xvf archive.tar              # extract tar archive
tar -czvf archive.tar.gz dir/     # create gzip-compressed tar
tar -xzvf archive.tar.gz          # extract gzip tar
tar -cjvf archive.tar.bz2 dir/    # bzip2 compressed
tar -tvf archive.tar              # list contents without extracting
zip -r archive.zip dir/           # create zip
unzip archive.zip                 # extract zip
gzip file                         # compress file (.gz)
gunzip file.gz                    # decompress
xz -9 file                        # high-ratio compression
7z a archive.7z dir/              # 7-zip archive
```

---

# 2. Linux System Administration

## 2.1 User & Group Management
```bash
whoami                         # current user
id                              # UID/GID and groups
who                              # logged in users
w                                # who + what they're doing
last                             # login history
useradd -m -s /bin/bash john     # create user with home dir + shell
useradd -m -G sudo,docker john   # create user, add to secondary groups
passwd john                      # set/change password
passwd -l john                   # lock account
passwd -u john                   # unlock account
usermod -aG docker john          # append user to group (ALWAYS use -aG, not -G)
usermod -s /bin/zsh john         # change shell
usermod -L john                  # lock user
userdel -r john                  # delete user + home dir
groupadd developers               # create group
groupdel developers               # delete group
gpasswd -a john developers        # add user to group
groups john                       # list groups for user
chage -l john                     # view password aging info
chage -M 90 john                  # max password age = 90 days
su - john                         # switch user (login shell)
sudo command                      # run as root
sudo -u john command              # run as specific user
visudo                            # safely edit /etc/sudoers
sudo -l                           # list allowed sudo commands
```

## 2.2 Process Management
```bash
ps aux                          # list all processes (BSD style)
ps -ef                          # list all processes (SysV style)
ps -ef --forest                 # process tree
pstree                          # visual process tree
top                             # real-time process monitor
htop                            # enhanced interactive process viewer
kill PID                        # send SIGTERM
kill -9 PID                     # send SIGKILL (force)
kill -l                         # list all signals
killall processname             # kill by name
pkill -f "pattern"              # kill by matching pattern
pgrep -f "pattern"              # find PID by pattern
nice -n 10 command               # start process with lower priority
renice -n 5 -p PID               # change priority of running process
nohup command &                  # run immune to hangups
command &                        # run in background
jobs                             # list background jobs
fg %1                            # bring job 1 to foreground
bg %1                            # resume job 1 in background
disown                           # detach job from shell
&&                               # run next command if first succeeds
;                                # run commands sequentially
lsof                             # list open files
lsof -i :80                     # what's using port 80
lsof -u john                     # files opened by user
fuser -k 8080/tcp                # kill process using a port
strace -p PID                    # trace syscalls of running process
ltrace command                   # trace library calls
```

## 2.3 System Services (systemd)
```bash
systemctl status nginx            # check service status
systemctl start nginx             # start service
systemctl stop nginx              # stop service
systemctl restart nginx           # restart service
systemctl reload nginx            # reload config without downtime
systemctl enable nginx            # start on boot
systemctl disable nginx           # don't start on boot
systemctl enable --now nginx      # enable + start immediately
systemctl is-active nginx         # check if running
systemctl is-enabled nginx        # check if enabled at boot
systemctl daemon-reload           # reload unit files after edit
systemctl list-units --type=service   # list all services
systemctl list-unit-files --state=enabled  # list enabled services
systemctl mask nginx              # prevent service from starting at all
systemctl unmask nginx            # undo mask
journalctl -u nginx               # logs for a unit
journalctl -u nginx -f            # follow logs live
journalctl -b                     # logs since last boot
journalctl --since "1 hour ago"   # time-filtered logs
journalctl -p err                 # only error-level logs
journalctl --disk-usage           # journal size on disk
journalctl --vacuum-time=7d       # purge logs older than 7 days
systemctl edit nginx              # create override.conf (drop-in)
systemctl get-default             # current boot target (runlevel)
systemctl set-default multi-user.target
init 0 / shutdown -h now          # power off
init 6 / reboot                   # restart
```

## 2.4 Package Management

### Debian/Ubuntu (APT)
```bash
apt update                        # refresh package index
apt upgrade                       # upgrade all packages
apt full-upgrade                  # upgrade + handle dependency changes
apt install nginx                 # install package
apt install nginx=1.18.0-0ubuntu1 # install specific version
apt remove nginx                  # remove package, keep configs
apt purge nginx                   # remove package + configs
apt autoremove                    # remove unused dependencies
apt list --installed              # list installed packages
apt search keyword                # search repo
apt show nginx                    # package details
apt-cache policy nginx            # show available versions
dpkg -l                           # list installed packages (dpkg)
dpkg -i package.deb               # install local .deb
dpkg -L nginx                     # list files owned by package
apt-get clean                     # clear downloaded package cache
add-apt-repository ppa:name/ppa   # add PPA repo
```

### RHEL/CentOS/Rocky/Alma (YUM/DNF)
```bash
dnf update                        # update all packages
dnf install nginx                 # install package
dnf remove nginx                  # remove package
dnf search keyword                 # search
dnf info nginx                     # package info
dnf list installed                # list installed
dnf history                        # transaction history
dnf history undo <id>              # rollback a transaction
rpm -qa                            # list all installed rpms
rpm -ivh package.rpm               # install local rpm
rpm -qf /path/to/file              # which package owns a file
dnf repolist                       # list enabled repos
dnf config-manager --add-repo URL  # add a repo
```

### Others
```bash
snap install code                 # Snap packages
flatpak install org.app.Name      # Flatpak
pacman -Syu                       # Arch Linux update
zypper install pkg                # openSUSE
```

## 2.5 Networking
```bash
ip a                              # show IP addresses (modern)
ip link show                      # show network interfaces
ip route                          # show routing table
ip route add default via 192.168.1.1
ifconfig                          # legacy interface info
hostname                          # show hostname
hostname -I                       # show all IPs
hostnamectl set-hostname newname  # change hostname persistently
ping -c 4 google.com              # test connectivity
traceroute google.com             # trace route to host
mtr google.com                    # live traceroute+ping
dig example.com                   # DNS lookup
dig +short example.com            # short DNS answer
nslookup example.com              # DNS lookup (legacy)
host example.com                  # DNS lookup
curl -I https://example.com       # HTTP headers only
curl -sSL https://example.com     # silent, follow redirects
wget https://example.com/file     # download file
netstat -tulnp                    # listening ports (legacy)
ss -tulnp                         # listening ports (modern, faster)
ss -s                              # socket summary statistics
nmap -sT -p 1-1000 host            # port scan
nc -zv host 22                     # check if port open (netcat)
nc -lvp 4444                       # listen on port (netcat)
tcpdump -i eth0 port 80            # capture packets on port 80
tcpdump -i eth0 -w capture.pcap    # save capture to file
iptables -L -n -v                  # list firewall rules (legacy)
nft list ruleset                   # list nftables rules (modern)
firewall-cmd --list-all            # firewalld status (RHEL)
firewall-cmd --add-port=80/tcp --permanent
ufw allow 22/tcp                   # UFW (Ubuntu firewall)
ufw enable / ufw status
scp file user@host:/path           # secure copy
rsync -avz src/ user@host:/dest/   # sync files (delta transfer)
rsync -avz --delete src/ dest/     # mirror, delete extras
ssh user@host                      # remote shell
ssh -i key.pem user@host           # login with key
ssh -L 8080:localhost:80 user@host # local port forward (tunnel)
ssh -D 1080 user@host              # SOCKS proxy tunnel
scp -P 2222 file user@host:/path   # custom SSH port
ssh-keygen -t ed25519 -C "email"   # generate SSH keypair
ssh-copy-id user@host              # copy public key to remote
```

## 2.6 Disk & Storage Management
```bash
df -h                              # disk space usage (human readable)
du -sh /path                       # size of a directory
du -sh * | sort -rh                # sizes sorted, biggest first
lsblk                              # list block devices
lsblk -f                            # + filesystem types & UUIDs
fdisk -l                            # list partitions
fdisk /dev/sdb                      # partition a disk (interactive)
parted /dev/sdb                     # advanced partitioning tool
mkfs.ext4 /dev/sdb1                 # format as ext4
mkfs.xfs /dev/sdb1                  # format as XFS
mount /dev/sdb1 /mnt/data           # mount a partition
umount /mnt/data                    # unmount
mount -a                            # mount everything in /etc/fstab
blkid                               # show UUIDs of block devices
/etc/fstab                          # persistent mount configuration
swapon -s                           # show active swap
swapoff -a / swapon -a              # disable/enable swap
mkswap /swapfile && swapon /swapfile # create swap file
fsck /dev/sdb1                      # check/repair filesystem
tune2fs -l /dev/sdb1                # ext filesystem tunables
resize2fs /dev/sdb1                 # grow/shrink ext filesystem
xfs_growfs /mnt/data                # grow XFS filesystem
iostat -xz 1                        # disk I/O stats every 1s
```

### LVM (Logical Volume Management)
```bash
pvcreate /dev/sdb1                  # create physical volume
vgcreate vg_data /dev/sdb1          # create volume group
lvcreate -L 20G -n lv_data vg_data  # create logical volume
lvextend -L +10G /dev/vg_data/lv_data  # grow LV
resize2fs /dev/vg_data/lv_data      # resize filesystem after LV grow
pvs / vgs / lvs                     # list PVs, VGs, LVs
pvdisplay / vgdisplay / lvdisplay   # detailed info
```

## 2.7 Log Management
```bash
/var/log/syslog                     # main system log (Debian)
/var/log/messages                   # main system log (RHEL)
/var/log/auth.log                   # authentication log (Debian)
/var/log/secure                     # authentication log (RHEL)
/var/log/dmesg or dmesg              # kernel ring buffer
tail -f /var/log/nginx/access.log   # live tail
logrotate -d /etc/logrotate.conf    # dry-run logrotate config
logrotate -f /etc/logrotate.conf    # force rotation
rsyslogd -N1                        # test rsyslog config
journalctl -k                       # kernel messages via journal
```

## 2.8 Cron & Scheduled Tasks
```bash
crontab -e                          # edit current user's crontab
crontab -l                          # list current user's crontab
crontab -r                          # remove crontab
crontab -u john -e                  # edit another user's crontab
# Format: minute hour day month weekday command
# */5 * * * *  -> every 5 minutes
# 0 2 * * *    -> daily at 2 AM
# 0 0 * * 0    -> weekly on Sunday
/etc/cron.d/, /etc/cron.daily/      # system-wide cron locations
at 10:00 PM                         # one-time scheduled job
atq                                  # list pending at jobs
systemctl list-timers                # list systemd timers
systemd-run --on-calendar="daily" --unit=myjob /path/script.sh
```

---

# 3. Advanced Linux Administration & Performance

## 3.1 Performance Monitoring & Tuning
```bash
vmstat 1                           # virtual memory stats every 1s
mpstat -P ALL 1                    # per-CPU utilization
sar -u 1 5                         # CPU usage history (sysstat)
sar -r 1 5                         # memory usage history
free -h                            # memory usage, human readable
free -m -s 2                       # refresh every 2s
uptime                             # load average + uptime
nproc                              # number of CPU cores
lscpu                              # detailed CPU info
numactl --hardware                 # NUMA topology
dstat                              # combined resource stats
iotop                              # per-process disk I/O
iftop -i eth0                      # per-connection bandwidth usage
nethogs                            # per-process bandwidth usage
perf top                           # live kernel/userspace profiling
perf stat -p PID                   # performance counters for a process
ulimit -a                          # show resource limits
ulimit -n 65536                    # raise open file limit (session)
/etc/security/limits.conf          # persistent ulimit configuration
sysctl -a                          # list all kernel parameters
sysctl -w net.ipv4.ip_forward=1    # set kernel parameter at runtime
/etc/sysctl.conf, /etc/sysctl.d/*  # persistent kernel tuning
sysctl -p                          # reload sysctl.conf
```

## 3.2 Common Kernel Tuning Parameters
```bash
net.core.somaxconn = 65535           # max connection backlog
net.ipv4.tcp_tw_reuse = 1            # reuse TIME_WAIT sockets
net.ipv4.ip_local_port_range = 1024 65535
vm.swappiness = 10                   # reduce swap usage preference
vm.max_map_count = 262144            # needed for Elasticsearch/DBs
fs.file-max = 2097152                # system-wide open file limit
net.ipv4.tcp_fin_timeout = 15
kernel.pid_max = 65536
```

## 3.3 Kernel & Module Management
```bash
uname -a                            # kernel version + system info
uname -r                            # kernel release
lsmod                               # list loaded kernel modules
modinfo module_name                 # module details
modprobe module_name                # load a module
modprobe -r module_name             # remove a module
dmesg -T                            # kernel log with human timestamps
update-grub                          # regenerate GRUB config (Debian)
grub2-mkconfig -o /boot/grub2/grub.cfg  # (RHEL)
```

## 3.4 Boot & Recovery
```bash
systemd-analyze                     # boot time breakdown
systemd-analyze blame               # slowest boot services
systemd-analyze critical-chain      # critical path of boot
runlevel                            # current/previous runlevel
systemctl rescue                    # boot into rescue mode
systemctl emergency                 # emergency shell
journalctl -b -1                    # logs from previous boot
```

## 3.5 High Availability & Clustering Basics
```bash
keepalived                          # VRRP-based failover (config in /etc/keepalived)
pcs status                          # Pacemaker cluster status
crm status                          # Corosync/Pacemaker status
haproxy -c -f /etc/haproxy/haproxy.cfg  # validate HAProxy config
systemctl status haproxy
```

## 3.6 Remote & Bulk Server Management
```bash
ansible all -m ping                              # test connectivity to fleet
pssh -h hosts.txt -i "uptime"                    # parallel-ssh execute
clustershell (clush -w node[1-5] "uptime")       # cluster shell
tmux new -s session                              # persistent terminal sessions
tmux attach -t session
screen -S session                                # alternative to tmux
ansible-playbook site.yml --limit webservers     # scoped playbook run
```

---

# 4. Linux Security Hardening

## 4.1 SSH Hardening
```bash
# /etc/ssh/sshd_config
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
Port 2222
AllowUsers deployer admin
MaxAuthTries 3
ClientAliveInterval 300
X11Forwarding no
systemctl restart sshd
```

## 4.2 Firewall Configuration
```bash
# iptables
iptables -A INPUT -p tcp --dport 22 -j ACCEPT
iptables -A INPUT -p tcp --dport 443 -j ACCEPT
iptables -P INPUT DROP
iptables-save > /etc/iptables/rules.v4

# firewalld
firewall-cmd --permanent --add-service=https
firewall-cmd --permanent --add-port=8080/tcp
firewall-cmd --reload

# UFW
ufw default deny incoming
ufw allow ssh
ufw allow 443/tcp
ufw enable
```

## 4.3 SELinux / AppArmor
```bash
getenforce                          # SELinux current mode
setenforce 0                        # set permissive (temporary)
sestatus                            # detailed SELinux status
semanage port -l                    # list SELinux port contexts
restorecon -Rv /var/www             # restore default file context
audit2allow -a                      # generate policy from denials
aa-status                           # AppArmor profile status
aa-enforce /etc/apparmor.d/profile  # enforce a profile
aa-complain /etc/apparmor.d/profile # set profile to complain mode
```

## 4.4 Auditing & Intrusion Detection
```bash
auditctl -l                          # list audit rules
auditctl -w /etc/passwd -p wa -k passwd_changes  # watch file for writes
ausearch -k passwd_changes           # search audit logs by key
aureport                             # summarized audit report
fail2ban-client status                # fail2ban jail status
fail2ban-client status sshd
fail2ban-client set sshd unbanip 1.2.3.4
rkhunter --check                     # rootkit hunter scan
chkrootkit                           # rootkit checker
lynis audit system                   # security auditing tool
aide --check                         # file integrity checking
tripwire --check                     # file integrity monitoring
```

## 4.5 User & Access Hardening
```bash
passwd -S john                        # check password status
awk -F: '$3 == 0 {print $1}' /etc/passwd   # find all UID 0 accounts
faillock --user john                  # view failed login attempts (PAM)
pam_tally2 --user john                # legacy failed login tracking
find / -perm -4000 -type f 2>/dev/null   # find all SUID binaries
find / -perm -2000 -type f 2>/dev/null   # find all SGID binaries
find / -nouser -o -nogroup            # orphaned files
```

## 4.6 TLS / Certificates
```bash
openssl req -new -newkey rsa:2048 -nodes -keyout key.pem -out csr.pem
openssl x509 -in cert.pem -text -noout       # inspect certificate
openssl s_client -connect host:443 -servername host   # test TLS handshake
openssl verify cert.pem
certbot certonly --standalone -d example.com  # Let's Encrypt cert
certbot renew --dry-run
```

## 4.7 Secrets Management
```bash
vault kv put secret/db password=xyz         # HashiCorp Vault write secret
vault kv get secret/db                      # read secret
vault login -method=aws                     # dynamic auth
aws secretsmanager get-secret-value --secret-id mysecret
kubectl create secret generic db-creds --from-literal=password=xyz
sops -e secrets.yaml > secrets.enc.yaml     # encrypt file with SOPS
gpg -c file                                  # symmetric encrypt file
gpg --decrypt file.gpg
```

---

# 5. Shell Scripting & Automation

## 5.1 Bash Scripting Essentials
```bash
#!/bin/bash
set -euo pipefail        # exit on error, undefined var, pipe failure

VAR="value"               # variable assignment (no spaces)
echo "$VAR"                # reference variable
if [ "$VAR" == "value" ]; then echo "match"; fi
for i in {1..5}; do echo "$i"; done
for f in *.txt; do echo "$f"; done
while read -r line; do echo "$line"; done < file.txt
case "$1" in
  start) echo "starting" ;;
  stop) echo "stopping" ;;
  *) echo "usage: $0 {start|stop}" ;;
esac
function myfunc() { echo "Hello $1"; }
myfunc "World"
ARR=(one two three)
echo "${ARR[@]}"            # all elements
echo "${#ARR[@]}"            # array length
$(command)                   # command substitution
$?                            # exit status of last command
$#                            # number of script arguments
$@                            # all arguments
trap 'echo Cleaning up' EXIT # run cleanup on exit
```

## 5.2 Text Processing One-Liners
```bash
awk -F: '{print $1}' /etc/passwd                 # list all usernames
grep -c "ERROR" app.log                          # count matches
sed -i 's/old/new/g' file.txt                    # in-place replace
sed -n '10,20p' file.txt                         # print lines 10-20
awk '{sum+=$1} END {print sum}' numbers.txt      # sum a column
sort -u file.txt                                  # unique sorted lines
comm -3 file1 file2                               # lines unique to either file
xargs -I{} cp {} /backup/ < filelist.txt          # apply command per line
find . -name "*.tmp" -exec rm {} \;               # find + delete
find . -name "*.log" | xargs gzip                 # find + compress
```

## 5.3 Configuration Management (Ansible)
```bash
ansible --version
ansible all -i inventory.ini -m ping
ansible webservers -m shell -a "uptime"
ansible-playbook -i inventory.ini site.yml
ansible-playbook site.yml --check          # dry run
ansible-playbook site.yml --diff           # show changes
ansible-playbook site.yml --tags "deploy"
ansible-vault encrypt secrets.yml          # encrypt sensitive vars
ansible-vault view secrets.yml
ansible-galaxy install role_name           # install community role
ansible-doc -l                             # list all modules
```

## 5.4 Infrastructure as Code (Terraform)
```bash
terraform init                     # initialize working directory
terraform validate                 # validate syntax
terraform plan                     # preview changes
terraform apply                    # apply changes
terraform apply -auto-approve
terraform destroy                  # tear down infrastructure
terraform fmt                      # format code
terraform show                     # show current state
terraform state list               # list resources in state
terraform state show <resource>    # show resource details
terraform import <resource> <id>   # import existing resource
terraform workspace list           # manage environments
terraform workspace new staging
terraform output                   # show output values
terraform taint <resource>         # force recreation on next apply
```

---

# 6. MariaDB / MySQL Database Administration

## 6.1 Installation & Service Management
```bash
apt install mariadb-server mariadb-client     # Debian/Ubuntu
dnf install mariadb-server                    # RHEL
systemctl enable --now mariadb
mysql_secure_installation                     # harden default install
mysql -u root -p                              # connect as root
mysql -u root -p -h host -P 3306              # connect to remote host
mysql -u user -p dbname < dump.sql            # import SQL file
systemctl status mariadb
mariadb --version
```

## 6.2 Basic SQL Operations
```sql
SHOW DATABASES;
CREATE DATABASE mydb CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
USE mydb;
SHOW TABLES;
DESCRIBE table_name;
SHOW CREATE TABLE table_name;
CREATE TABLE users (
  id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(100) NOT NULL,
  email VARCHAR(150) UNIQUE,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
) ENGINE=InnoDB;
ALTER TABLE users ADD COLUMN age INT;
ALTER TABLE users MODIFY COLUMN age SMALLINT;
ALTER TABLE users DROP COLUMN age;
DROP TABLE table_name;
DROP DATABASE mydb;
INSERT INTO users (name, email) VALUES ('John', 'john@x.com');
SELECT * FROM users WHERE id = 1;
UPDATE users SET name='Jane' WHERE id=1;
DELETE FROM users WHERE id=1;
```

## 6.3 User & Privilege Management
```sql
CREATE USER 'app'@'%' IDENTIFIED BY 'StrongPass123!';
GRANT ALL PRIVILEGES ON mydb.* TO 'app'@'%';
GRANT SELECT, INSERT, UPDATE ON mydb.users TO 'app'@'localhost';
REVOKE INSERT ON mydb.* FROM 'app'@'%';
SHOW GRANTS FOR 'app'@'%';
FLUSH PRIVILEGES;
DROP USER 'app'@'%';
ALTER USER 'app'@'%' IDENTIFIED BY 'NewPass456!';
SELECT user, host FROM mysql.user;
```

## 6.4 Backup & Restore
```bash
mysqldump -u root -p mydb > mydb_backup.sql               # single DB
mysqldump -u root -p --all-databases > all_backup.sql     # all DBs
mysqldump -u root -p --single-transaction mydb > backup.sql  # InnoDB, no lock
mysqldump -u root -p mydb table1 table2 > tables.sql       # specific tables
mysqldump -u root -p --no-data mydb > schema_only.sql      # schema only
mysql -u root -p mydb < mydb_backup.sql                    # restore
mariabackup --backup --target-dir=/backup/                 # physical hot backup
mariabackup --prepare --target-dir=/backup/
xtrabackup --backup --target-dir=/backup/                  # Percona alternative
```

## 6.5 Replication
```sql
-- On master:
SHOW MASTER STATUS;
-- my.cnf on master
[mysqld]
server-id=1
log_bin=mysql-bin

-- On replica: my.cnf
[mysqld]
server-id=2

-- On replica:
CHANGE MASTER TO
  MASTER_HOST='master_ip',
  MASTER_USER='repl',
  MASTER_PASSWORD='pass',
  MASTER_LOG_FILE='mysql-bin.000001',
  MASTER_LOG_POS=154;
START SLAVE;
SHOW SLAVE STATUS\G
STOP SLAVE;
```
```bash
# Galera Cluster (multi-master)
systemctl start mariadb --wsrep-new-cluster   # bootstrap first node
SHOW STATUS LIKE 'wsrep_cluster_size';
SHOW STATUS LIKE 'wsrep_local_state_comment';
```

## 6.6 Performance Tuning & Monitoring
```sql
SHOW PROCESSLIST;                            -- active queries
SHOW FULL PROCESSLIST;
KILL <process_id>;
SHOW ENGINE INNODB STATUS\G
SHOW VARIABLES LIKE 'innodb_buffer_pool_size';
SET GLOBAL slow_query_log = 'ON';
SET GLOBAL long_query_time = 2;
SHOW VARIABLES LIKE 'slow_query_log_file';
EXPLAIN SELECT * FROM users WHERE email='x@y.com';
EXPLAIN ANALYZE SELECT ...;                  -- MariaDB 10.4+
SHOW INDEX FROM users;
ANALYZE TABLE users;
OPTIMIZE TABLE users;
CHECK TABLE users;
REPAIR TABLE users;
SHOW GLOBAL STATUS LIKE 'Threads_connected';
SHOW GLOBAL STATUS LIKE 'Slow_queries';
```
```bash
mysqltuner                          # automated tuning recommendations
pt-query-digest slow.log            # Percona Toolkit slow-query analysis
mytop -u root -p                    # live MySQL process viewer
```

## 6.7 Key Config File Tuning (`/etc/mysql/my.cnf`)
```ini
[mysqld]
innodb_buffer_pool_size = 4G        # ~70-80% of RAM on dedicated DB server
innodb_log_file_size = 512M
innodb_flush_log_at_trx_commit = 1  # 1=ACID safe, 2=faster, less durable
max_connections = 200
query_cache_type = 0                # deprecated in modern MariaDB
innodb_file_per_table = 1
character-set-server = utf8mb4
```

## 6.8 High Availability Tools
```bash
# ProxySQL / MaxScale for load balancing & failover
maxctrl list servers
maxctrl list services
```

---

# 7. PostgreSQL Database Administration

## 7.1 Installation & Service Management
```bash
apt install postgresql postgresql-contrib     # Debian/Ubuntu
dnf install postgresql-server postgresql-contrib  # RHEL
postgresql-setup --initdb                     # RHEL initialization
systemctl enable --now postgresql
sudo -u postgres psql                         # connect as postgres superuser
psql -U user -d dbname -h host -p 5432        # connect to remote DB
psql -U user -d dbname -f script.sql          # run SQL script
pg_lsclusters                                 # (Debian) list clusters
pg_ctl status -D /var/lib/postgresql/data
```

## 7.2 psql Meta-Commands
```
\l                 -- list databases
\c dbname          -- connect to database
\dt                -- list tables
\d table_name      -- describe table structure
\du                -- list roles/users
\dn                -- list schemas
\di                -- list indexes
\dv                -- list views
\df                -- list functions
\x                 -- toggle expanded display
\timing            -- toggle query timing
\q                 -- quit
\i file.sql        -- execute file
\e                 -- edit query in $EDITOR
\conninfo          -- show connection info
\copy table TO 'file.csv' CSV HEADER   -- export
\copy table FROM 'file.csv' CSV HEADER -- import
```

## 7.3 Basic SQL Operations
```sql
CREATE DATABASE mydb;
DROP DATABASE mydb;
CREATE TABLE users (
  id SERIAL PRIMARY KEY,
  name VARCHAR(100) NOT NULL,
  email VARCHAR(150) UNIQUE,
  created_at TIMESTAMPTZ DEFAULT now()
);
ALTER TABLE users ADD COLUMN age INT;
ALTER TABLE users ALTER COLUMN age TYPE SMALLINT;
ALTER TABLE users DROP COLUMN age;
INSERT INTO users (name,email) VALUES ('John','john@x.com') RETURNING id;
SELECT * FROM users WHERE id=1;
UPDATE users SET name='Jane' WHERE id=1;
DELETE FROM users WHERE id=1;
CREATE INDEX idx_users_email ON users(email);
CREATE UNIQUE INDEX idx_users_email_unique ON users(email);
CREATE INDEX CONCURRENTLY idx_users_name ON users(name);  -- no table lock
```

## 7.4 Roles & Privilege Management
```sql
CREATE ROLE app WITH LOGIN PASSWORD 'StrongPass123!';
CREATE USER app WITH PASSWORD 'StrongPass123!' LOGIN;  -- alias for role+login
ALTER ROLE app WITH SUPERUSER;
ALTER ROLE app WITH PASSWORD 'NewPass456!';
GRANT ALL PRIVILEGES ON DATABASE mydb TO app;
GRANT SELECT, INSERT ON ALL TABLES IN SCHEMA public TO app;
GRANT USAGE ON SCHEMA public TO app;
REVOKE INSERT ON users FROM app;
ALTER DEFAULT PRIVILEGES IN SCHEMA public GRANT SELECT ON TABLES TO app;
DROP ROLE app;
\du                                    -- list roles (psql)
SELECT * FROM pg_roles;
```

## 7.5 Backup & Restore
```bash
pg_dump -U postgres -d mydb -F c -f mydb.dump         # custom format (compressed)
pg_dump -U postgres -d mydb -F p -f mydb.sql          # plain SQL
pg_dump -U postgres -d mydb -t users -f users.sql     # single table
pg_dumpall -U postgres -f all_databases.sql           # entire cluster
pg_restore -U postgres -d mydb mydb.dump              # restore custom format
pg_restore -U postgres -d mydb --clean --if-exists mydb.dump
psql -U postgres -d mydb -f mydb.sql                  # restore plain SQL
pg_basebackup -D /backup -Ft -z -P -U replicator      # physical base backup
```

## 7.6 Replication & High Availability
```bash
# postgresql.conf on primary
wal_level = replica
max_wal_senders = 10
max_replication_slots = 10
archive_mode = on
archive_command = 'cp %p /archive/%f'

# pg_hba.conf on primary — allow replica to connect
host replication replicator 10.0.0.2/32 md5

# On replica:
pg_basebackup -h primary_ip -D /var/lib/postgresql/data -U replicator -P -R
# -R writes standby.signal + primary_conninfo automatically (PG12+)
```
```sql
SELECT * FROM pg_stat_replication;        -- run on primary
SELECT pg_is_in_recovery();               -- true on standby
SELECT pg_last_wal_receive_lsn();         -- replication lag check
```
```bash
# Tools for HA/failover
patronictl list                            # Patroni cluster status
repmgr cluster show                        # repmgr cluster topology
pg_ctlcluster 14 main promote              # promote standby (Debian)
```

## 7.7 Performance Tuning & Monitoring
```sql
EXPLAIN SELECT * FROM users WHERE email='x@y.com';
EXPLAIN ANALYZE SELECT * FROM users WHERE email='x@y.com';
EXPLAIN (ANALYZE, BUFFERS, FORMAT JSON) SELECT ...;
SELECT * FROM pg_stat_activity;             -- active connections/queries
SELECT pg_terminate_backend(pid);           -- kill a query
SELECT pg_cancel_backend(pid);              -- cancel a query (softer)
SELECT * FROM pg_stat_user_tables;          -- table statistics
SELECT * FROM pg_stat_user_indexes;         -- index usage stats
SELECT relname, n_dead_tup FROM pg_stat_user_tables ORDER BY n_dead_tup DESC;
VACUUM;                                     -- reclaim dead tuple space
VACUUM FULL users;                          -- full rewrite, locks table
VACUUM ANALYZE users;                       -- vacuum + refresh planner stats
ANALYZE users;                              -- update planner statistics
REINDEX TABLE users;                        -- rebuild indexes
SELECT * FROM pg_stat_bgwriter;
SHOW ALL;                                    -- all runtime settings
SHOW shared_buffers;
ALTER SYSTEM SET shared_buffers = '2GB';    -- persist config change
SELECT pg_reload_conf();                    -- reload config w/o restart
```
```bash
pg_stat_statements                          -- extension for query-level stats
CREATE EXTENSION pg_stat_statements;
SELECT query, calls, total_time FROM pg_stat_statements ORDER BY total_time DESC LIMIT 10;
pgbench -i -s 50 mydb                       # initialize benchmark data
pgbench -c 10 -j 2 -T 60 mydb               # run benchmark: 10 clients, 60s
```

## 7.8 Key Config Tuning (`postgresql.conf`)
```ini
shared_buffers = 4GB              # ~25% of RAM
effective_cache_size = 12GB       # ~50-75% of RAM
work_mem = 64MB                   # per sort/hash operation
maintenance_work_mem = 512MB
max_connections = 200
wal_buffers = 16MB
checkpoint_completion_target = 0.9
random_page_cost = 1.1            # lower for SSD storage
```

## 7.9 Partitioning & Extensions
```sql
CREATE TABLE sales (id SERIAL, sale_date DATE) PARTITION BY RANGE (sale_date);
CREATE TABLE sales_2026 PARTITION OF sales
  FOR VALUES FROM ('2026-01-01') TO ('2027-01-01');
CREATE EXTENSION postgis;          -- geospatial
CREATE EXTENSION pgcrypto;         -- encryption functions
CREATE EXTENSION "uuid-ossp";      -- UUID generation
```

---

# 8. DevOps Toolchain

## 8.1 Git — Version Control
```bash
git init                                # initialize repo
git clone <url>                         # clone repo
git status                              # working tree status
git add file / git add .                # stage changes
git commit -m "message"                 # commit staged changes
git commit --amend                      # amend last commit
git push origin main                    # push to remote
git pull origin main                    # fetch + merge
git fetch --all                         # fetch without merging
git branch                              # list branches
git branch feature-x                    # create branch
git checkout feature-x                  # switch branch
git checkout -b feature-x               # create + switch
git switch feature-x                    # modern switch command
git merge feature-x                     # merge branch into current
git rebase main                         # rebase current onto main
git rebase -i HEAD~3                    # interactive rebase (squash etc.)
git log --oneline --graph --all         # visual commit history
git diff                                # unstaged changes
git diff --staged                       # staged changes
git stash                               # shelve changes
git stash pop                           # restore shelved changes
git reset --soft HEAD~1                 # undo commit, keep changes staged
git reset --hard HEAD~1                 # undo commit, discard changes
git revert <commit>                     # create inverse commit (safe undo)
git tag v1.0.0                          # create tag
git tag -a v1.0.0 -m "release"          # annotated tag
git push --tags
git cherry-pick <commit>                # apply specific commit
git blame file                          # who changed each line
git bisect start                        # binary search for bad commit
git submodule add <url> path            # add submodule
git remote -v                           # list remotes
git remote add origin <url>
git worktree add ../hotfix hotfix-branch  # multiple working trees
```

## 8.2 CI/CD — GitHub Actions / GitLab CI / Jenkins
```yaml
# .github/workflows/ci.yml
name: CI
on: [push, pull_request]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Run tests
        run: |
          npm install
          npm test
      - name: Build image
        run: docker build -t myapp:${{ github.sha }} .
```
```yaml
# .gitlab-ci.yml
stages: [build, test, deploy]
build:
  stage: build
  script: docker build -t myapp:$CI_COMMIT_SHA .
test:
  stage: test
  script: npm test
deploy:
  stage: deploy
  script: kubectl apply -f k8s/
  only: [main]
```
```groovy
// Jenkinsfile
pipeline {
  agent any
  stages {
    stage('Build') { steps { sh 'mvn clean package' } }
    stage('Test')  { steps { sh 'mvn test' } }
    stage('Deploy'){ steps { sh 'kubectl apply -f deploy.yaml' } }
  }
}
```
```bash
jenkins-cli build job-name                 # trigger job via CLI
gh workflow run ci.yml                     # trigger GitHub Actions via CLI
gh run list / gh run watch                 # GitHub CLI monitoring
gitlab-runner exec docker test             # run GitLab CI job locally
```

## 8.3 Monitoring & Observability
```bash
# Prometheus
promtool check config prometheus.yml       # validate config
promtool query instant http://localhost:9090 'up'
curl localhost:9090/-/reload               # hot reload config

# Node exporter, cAdvisor, alertmanager are common companions.

# Grafana
grafana-cli plugins install <plugin>
curl -X POST http://admin:pass@localhost:3000/api/dashboards/db -d @dashboard.json

# ELK / EFK Stack
curl -X GET "localhost:9200/_cluster/health?pretty"   # Elasticsearch health
curl -X GET "localhost:9200/_cat/indices?v"           # list indices
filebeat test config                                  # test Filebeat config
logstash -f pipeline.conf --config.test_and_exit      # test Logstash config

# Loki / Promtail
logcli query '{job="varlogs"}'                        # query Loki logs
```

## 8.4 Load Balancing / Reverse Proxy
```bash
nginx -t                              # test config syntax
nginx -s reload                       # reload config
nginx -s stop
systemctl reload nginx
haproxy -c -f /etc/haproxy/haproxy.cfg
systemctl reload haproxy
envoy --mode validate -c envoy.yaml   # validate Envoy config
```

## 8.5 Message Queues & Caching
```bash
# Redis
redis-cli ping
redis-cli -h host -p 6379
redis-cli monitor                     # live command stream
redis-cli info                        # server stats
redis-cli config get maxmemory
redis-cli flushall                    # clear all data (careful!)

# Kafka
kafka-topics.sh --bootstrap-server localhost:9092 --list
kafka-topics.sh --create --topic mytopic --bootstrap-server localhost:9092 --partitions 3 --replication-factor 2
kafka-console-producer.sh --topic mytopic --bootstrap-server localhost:9092
kafka-console-consumer.sh --topic mytopic --from-beginning --bootstrap-server localhost:9092
kafka-consumer-groups.sh --bootstrap-server localhost:9092 --list

# RabbitMQ
rabbitmqctl status
rabbitmqctl list_queues
rabbitmqctl add_user user pass
rabbitmqctl set_permissions -p / user ".*" ".*" ".*"
```

## 8.6 Cloud CLI Basics
```bash
# AWS
aws configure                              # setup credentials
aws s3 ls                                  # list buckets
aws s3 cp file.txt s3://bucket/            # upload
aws ec2 describe-instances
aws sts get-caller-identity                # who am I

# Azure
az login
az group list
az vm list

# GCP
gcloud auth login
gcloud config set project my-project
gcloud compute instances list
```

---

# 9. DevSecOps

## 9.1 Static Application Security Testing (SAST)
```bash
semgrep --config auto .                    # pattern-based SAST
bandit -r ./app                            # Python security linter
gosec ./...                                # Go security scanner
snyk code test                             # Snyk SAST
sonar-scanner                              # SonarQube static analysis
eslint --plugin security .                 # JS security linting
brakeman                                   # Ruby on Rails scanner
```

## 9.2 Dependency / Software Composition Analysis (SCA)
```bash
snyk test                                  # scan dependencies for CVEs
npm audit                                  # Node.js dependency audit
npm audit fix
pip-audit                                  # Python dependency audit
safety check                               # Python known-vuln check
trivy fs .                                 # filesystem/dependency scan
grype dir:.                                # vulnerability scanner
dependency-check --project app --scan .    # OWASP Dependency-Check
```

## 9.3 Container Image Scanning
```bash
trivy image myapp:latest                   # scan image for CVEs
trivy image --severity HIGH,CRITICAL myapp:latest
grype myapp:latest                         # Anchore Grype scan
docker scout cves myapp:latest             # Docker Scout scan
clair-scanner --ip localhost myapp:latest  # Clair scanner
cosign sign myapp:latest                   # sign image (supply chain security)
cosign verify myapp:latest
syft myapp:latest -o json                  # generate SBOM
```

## 9.4 Infrastructure as Code Security
```bash
checkov -d .                               # scan Terraform/K8s/CFN for misconfig
tfsec .                                     # Terraform-specific security scan
terrascan scan -i terraform                 # policy-as-code scanner
kics scan -p .                              # Keeping Infra as Code Secure
conftest test deployment.yaml               # OPA-based policy testing
```

## 9.5 Kubernetes Security
```bash
kube-bench run                             # CIS Kubernetes benchmark
kube-hunter --remote <ip>                  # penetration testing tool
trivy k8s cluster                          # scan running cluster
kubesec scan deployment.yaml               # manifest risk scoring
polaris audit --audit-path ./manifests     # best-practice auditing
falco                                      # runtime threat detection
opa eval -i input.json -d policy.rego "data.main.deny"  # OPA policy check
gatekeeper (OPA Gatekeeper CRDs applied via kubectl)     # admission control
```

## 9.6 Dynamic Application Security Testing (DAST)
```bash
zap-cli quick-scan http://target.com               # OWASP ZAP scan
zap-baseline.py -t http://target.com               # ZAP baseline scan (Docker)
nikto -h http://target.com                         # web server scanner
nuclei -u http://target.com -t cves/               # template-based scanner
sqlmap -u "http://target.com/page?id=1" --batch    # SQL injection testing
```

## 9.7 Secrets Detection
```bash
gitleaks detect --source . -v                      # scan repo for leaked secrets
trufflehog filesystem .                            # secret scanning
detect-secrets scan                                # Yelp's secret scanner
git-secrets --scan                                 # AWS git-secrets
```

## 9.8 Compliance & Policy as Code
```bash
inspec exec profile/                       # Chef InSpec compliance testing
opa test policies/                         # test Rego policies
conftest verify --policy policy/ manifests/
cis-cat-lite -a                            # CIS benchmark assessment
oscap xccdf eval --profile <profile> ssg-rhel8-ds.xml   # OpenSCAP compliance
```

## 9.9 CI/CD Pipeline Security Example
```yaml
# Security gate stage in GitLab CI
security_scan:
  stage: test
  script:
    - trivy fs --exit-code 1 --severity CRITICAL .
    - gitleaks detect --source . --exit-code 1
    - checkov -d terraform/ --compact
  allow_failure: false
```

---

# 10. Docker

## 10.1 Basic Commands
```bash
docker --version
docker info                          # system-wide info
docker run hello-world               # test installation
docker run -d -p 8080:80 --name web nginx   # run detached, port map, named
docker run -it ubuntu bash           # interactive terminal session
docker ps                            # list running containers
docker ps -a                         # list all (including stopped)
docker start container_name
docker stop container_name
docker restart container_name
docker rm container_name             # remove stopped container
docker rm -f container_name          # force remove running container
docker logs container_name
docker logs -f container_name        # follow logs
docker exec -it container_name bash  # shell into running container
docker inspect container_name        # detailed JSON metadata
docker top container_name            # processes inside container
docker stats                         # live resource usage of all containers
docker cp file.txt container:/path/  # copy file into container
docker cp container:/path/file .     # copy file out of container
docker pause / docker unpause container_name
docker rename old_name new_name
```

## 10.2 Images
```bash
docker images                        # list local images
docker pull nginx:1.25               # pull specific tag
docker build -t myapp:1.0 .          # build image from Dockerfile
docker build -t myapp:1.0 -f Dockerfile.prod .
docker build --no-cache -t myapp .   # rebuild ignoring cache
docker tag myapp:1.0 myapp:latest
docker push myrepo/myapp:1.0
docker rmi image_id                  # remove image
docker image prune                   # remove dangling images
docker image prune -a                # remove all unused images
docker history myapp:1.0             # show image layers
docker save myapp:1.0 -o myapp.tar   # export image to tar
docker load -i myapp.tar             # import image from tar
docker export container_name -o container.tar  # export container filesystem
```

## 10.3 Dockerfile Essentials
```dockerfile
FROM node:20-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production
COPY . .
RUN npm run build

FROM node:20-alpine
WORKDIR /app
COPY --from=builder /app/dist ./dist
COPY --from=builder /app/node_modules ./node_modules
EXPOSE 3000
USER node
HEALTHCHECK --interval=30s CMD wget -qO- http://localhost:3000/health || exit 1
ENTRYPOINT ["node", "dist/server.js"]
```

## 10.4 Volumes & Storage
```bash
docker volume create myvolume
docker volume ls
docker volume inspect myvolume
docker volume rm myvolume
docker volume prune                             # remove unused volumes
docker run -v myvolume:/data nginx              # named volume
docker run -v /host/path:/container/path nginx  # bind mount
docker run --mount type=bind,src=/host,dst=/container nginx  # modern syntax
docker run --tmpfs /tmp nginx                   # in-memory ephemeral storage
```

## 10.5 Networking
```bash
docker network ls
docker network create mynet
docker network create --driver bridge --subnet 172.20.0.0/16 mynet
docker network inspect mynet
docker network connect mynet container_name
docker network disconnect mynet container_name
docker run --network mynet --name db postgres
docker run --network host nginx                 # use host networking
docker network rm mynet
docker network prune
```

## 10.6 Docker Compose
```bash
docker compose up -d                            # start all services detached
docker compose down                             # stop and remove
docker compose down -v                          # also remove volumes
docker compose ps
docker compose logs -f service_name
docker compose build
docker compose exec service_name bash
docker compose restart service_name
docker compose config                           # validate/render config
```
```yaml
# docker-compose.yml
version: "3.9"
services:
  web:
    build: .
    ports: ["8080:80"]
    environment:
      - DB_HOST=db
    depends_on: [db]
  db:
    image: postgres:16
    environment:
      POSTGRES_PASSWORD: secret
    volumes:
      - dbdata:/var/lib/postgresql/data
volumes:
  dbdata:
```

## 10.7 Docker Swarm (Native Orchestration)
```bash
docker swarm init                               # initialize swarm
docker swarm join --token <token> <manager-ip>:2377
docker node ls                                  # list swarm nodes
docker service create --name web --replicas 3 -p 80:80 nginx
docker service ls
docker service scale web=5
docker service update --image nginx:1.26 web
docker stack deploy -c docker-compose.yml mystack
docker stack ls
docker stack services mystack
docker stack rm mystack
```

## 10.8 Advanced: Security, Debugging, Resource Limits
```bash
docker run --memory=512m --cpus=1.0 nginx       # resource limits
docker run --read-only nginx                    # read-only root filesystem
docker run --cap-drop=ALL --cap-add=NET_BIND_SERVICE nginx  # least privilege
docker run --security-opt=no-new-privileges nginx
docker run --user 1000:1000 nginx               # non-root user
docker scan myapp:latest                        # vulnerability scan (deprecated in favor of Scout)
docker system df                                # disk usage by docker objects
docker system prune -a --volumes                # clean everything unused (careful!)
docker events                                    # real-time event stream
docker buildx build --platform linux/amd64,linux/arm64 -t myapp .  # multi-arch build
docker context ls                                # manage remote docker contexts
DOCKER_BUILDKIT=1 docker build .                 # enable BuildKit
```

## 10.9 Troubleshooting
```bash
docker logs --tail 100 --timestamps container_name
docker inspect --format='{{.State.ExitCode}}' container_name
docker inspect --format='{{json .NetworkSettings.Networks}}' container_name
docker events --filter 'container=name'
docker diff container_name                       # filesystem changes vs image
journalctl -u docker.service -f                  # docker daemon logs
```

---

# 11. Kubernetes

## 11.1 Cluster Basics
```bash
kubectl version --client                   # client version
kubectl cluster-info                       # cluster endpoint info
kubectl get nodes                          # list nodes
kubectl get nodes -o wide                  # more detail
kubectl describe node <node>               # node details
kubectl top nodes                          # node resource usage (metrics-server)
kubectl top pods                           # pod resource usage
kubectl config get-contexts                # list contexts
kubectl config use-context <name>          # switch context
kubectl config current-context
kubectl config set-context --current --namespace=myns
```

## 11.2 Pods, Deployments, Services
```bash
kubectl get pods                           # list pods in current namespace
kubectl get pods -A                        # all namespaces
kubectl get pods -o wide                   # + node/IP info
kubectl describe pod <pod>                 # detailed pod info + events
kubectl logs <pod>                         # container logs
kubectl logs -f <pod>                      # follow logs
kubectl logs <pod> -c <container>          # specific container in multi-container pod
kubectl logs --previous <pod>              # logs from crashed container
kubectl exec -it <pod> -- bash             # shell into pod
kubectl exec -it <pod> -c <container> -- sh
kubectl delete pod <pod>                   # delete pod
kubectl run nginx --image=nginx            # quick ad-hoc pod
kubectl apply -f deployment.yaml           # create/update from manifest
kubectl get deployments
kubectl describe deployment <name>
kubectl scale deployment <name> --replicas=5
kubectl rollout status deployment <name>
kubectl rollout history deployment <name>
kubectl rollout undo deployment <name>              # rollback
kubectl rollout undo deployment <name> --to-revision=2
kubectl rollout restart deployment <name>           # rolling restart
kubectl set image deployment/<name> container=image:v2  # update image
kubectl get services
kubectl expose deployment <name> --port=80 --type=LoadBalancer
kubectl get endpoints
kubectl port-forward pod/<pod> 8080:80              # local access to pod
kubectl port-forward svc/<service> 8080:80
```

## 11.3 Example Manifests
```yaml
# deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web
  labels: {app: web}
spec:
  replicas: 3
  selector:
    matchLabels: {app: web}
  template:
    metadata:
      labels: {app: web}
    spec:
      containers:
        - name: web
          image: myapp:1.0
          ports: [{containerPort: 8080}]
          resources:
            requests: {cpu: "250m", memory: "256Mi"}
            limits:   {cpu: "500m", memory: "512Mi"}
          livenessProbe:
            httpGet: {path: /healthz, port: 8080}
            initialDelaySeconds: 10
          readinessProbe:
            httpGet: {path: /ready, port: 8080}
---
apiVersion: v1
kind: Service
metadata: {name: web-svc}
spec:
  selector: {app: web}
  ports: [{port: 80, targetPort: 8080}]
  type: ClusterIP
```

## 11.4 ConfigMaps & Secrets
```bash
kubectl create configmap app-config --from-literal=ENV=production
kubectl create configmap app-config --from-file=config.properties
kubectl get configmap app-config -o yaml
kubectl create secret generic db-secret --from-literal=password=xyz
kubectl create secret docker-registry regcred --docker-server=... --docker-username=... --docker-password=...
kubectl create secret tls tls-secret --cert=cert.pem --key=key.pem
kubectl get secret db-secret -o jsonpath='{.data.password}' | base64 -d
```

## 11.5 Namespaces, RBAC, Resource Quotas
```bash
kubectl create namespace staging
kubectl get namespaces
kubectl delete namespace staging
kubectl create serviceaccount myapp-sa
kubectl create role pod-reader --verb=get,list,watch --resource=pods
kubectl create rolebinding read-pods --role=pod-reader --serviceaccount=default:myapp-sa
kubectl create clusterrole cluster-admin-lite --verb=get,list --resource=nodes
kubectl create clusterrolebinding admin-binding --clusterrole=cluster-admin --user=jane
kubectl auth can-i create pods --as=jane
kubectl auth can-i list secrets --namespace=staging
kubectl describe quota -n staging
kubectl describe limitrange -n staging
```

## 11.6 Storage
```bash
kubectl get pv                             # persistent volumes
kubectl get pvc                            # persistent volume claims
kubectl get storageclass
```
```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata: {name: data-pvc}
spec:
  accessModes: ["ReadWriteOnce"]
  resources: {requests: {storage: 10Gi}}
  storageClassName: fast-ssd
```

## 11.7 StatefulSets, DaemonSets, Jobs, CronJobs
```bash
kubectl get statefulsets
kubectl get daemonsets
kubectl get jobs
kubectl get cronjobs
kubectl create job manual-run --image=busybox -- echo hello
kubectl delete job manual-run
```
```yaml
apiVersion: batch/v1
kind: CronJob
metadata: {name: nightly-backup}
spec:
  schedule: "0 2 * * *"
  jobTemplate:
    spec:
      template:
        spec:
          containers:
            - name: backup
              image: backup-tool:latest
          restartPolicy: OnFailure
```

## 11.8 Autoscaling
```bash
kubectl autoscale deployment web --cpu-percent=70 --min=2 --max=10
kubectl get hpa
kubectl describe hpa web
# Requires metrics-server installed for CPU/memory metrics
kubectl get --raw /apis/metrics.k8s.io/v1beta1/nodes
```
```yaml
# VerticalPodAutoscaler / KEDA ScaledObject are common extensions
apiVersion: keda.sh/v1alpha1
kind: ScaledObject
metadata: {name: queue-scaler}
spec:
  scaleTargetRef: {name: worker}
  triggers:
    - type: rabbitmq
      metadata: {queueName: tasks, host: amqp://rabbitmq:5672}
```

## 11.9 Networking & Ingress
```bash
kubectl get ingress
kubectl describe ingress <name>
kubectl get networkpolicy
```
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: web-ingress
  annotations: {nginx.ingress.kubernetes.io/rewrite-target: /}
spec:
  ingressClassName: nginx
  rules:
    - host: app.example.com
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service: {name: web-svc, port: {number: 80}}
  tls:
    - hosts: [app.example.com]
      secretName: tls-secret
---
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata: {name: deny-all}
spec:
  podSelector: {}
  policyTypes: [Ingress, Egress]
```

## 11.10 Helm (Package Manager)
```bash
helm version
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update
helm search repo postgresql
helm install myrelease bitnami/postgresql
helm install myrelease ./mychart -f values-prod.yaml
helm upgrade myrelease ./mychart
helm upgrade --install myrelease ./mychart  # install if not exists
helm rollback myrelease 1
helm list
helm status myrelease
helm uninstall myrelease
helm template ./mychart                     # render manifests locally
helm lint ./mychart                         # validate chart
helm show values bitnami/postgresql         # view default values
helm diff upgrade myrelease ./mychart       # (plugin) preview changes
```

## 11.11 Debugging & Troubleshooting
```bash
kubectl get events --sort-by='.lastTimestamp'       # cluster-wide events
kubectl get events -n staging --field-selector type=Warning
kubectl describe pod <pod>                          # check Events section
kubectl get pod <pod> -o yaml                       # full manifest, incl. status
kubectl debug <pod> -it --image=busybox             # ephemeral debug container
kubectl debug node/<node> -it --image=busybox       # debug a node
kubectl get pods --field-selector=status.phase=Pending
kubectl get pods --field-selector=status.phase!=Running
kubectl cp <pod>:/path/file ./file                  # copy from pod
kubectl explain pod.spec.containers                 # inline API docs
kubectl api-resources                               # list all resource types
kubectl api-versions
kubectl get all -n staging                          # everything in namespace
crictl ps                                            # container runtime level (on node)
crictl logs <container-id>
journalctl -u kubelet -f                            # kubelet logs on node
```

## 11.12 Advanced: Admission Control, CRDs, Operators
```bash
kubectl get crd                                     # list Custom Resource Definitions
kubectl get validatingwebhookconfigurations
kubectl get mutatingwebhookconfigurations
kubectl apply -f crd.yaml                           # register a CRD
kubectl get <custom-resource-name>                  # list custom resources
kustomize build overlays/production | kubectl apply -f -   # Kustomize overlays
kubectl apply -k overlays/production/               # kubectl native kustomize
argocd app sync myapp                               # GitOps: ArgoCD sync
argocd app list
flux get kustomizations                             # GitOps: Flux
istioctl analyze                                     # service mesh config check
istioctl proxy-config routes <pod>                   # inspect Envoy sidecar routes
```

## 11.13 Cluster Administration
```bash
kubeadm init --pod-network-cidr=10.244.0.0/16       # bootstrap control plane
kubeadm join <ip>:6443 --token <token> --discovery-token-ca-cert-hash sha256:<hash>
kubeadm upgrade plan
kubeadm upgrade apply v1.30.0
kubectl drain <node> --ignore-daemonsets --delete-emptydir-data  # prepare for maintenance
kubectl cordon <node>                                # mark unschedulable
kubectl uncordon <node>                              # mark schedulable again
etcdctl snapshot save backup.db                      # backup etcd (control plane)
etcdctl snapshot status backup.db
velero backup create my-backup                       # cluster-wide backup tool
velero restore create --from-backup my-backup
```

---

# 12. Quick-Reference Cheat Sheets

## 12.1 File Permission Numbers
| Number | Permission | Symbol |
|---|---|---|
| 0 | none | `---` |
| 1 | execute | `--x` |
| 2 | write | `-w-` |
| 3 | write+execute | `-wx` |
| 4 | read | `r--` |
| 5 | read+execute | `r-x` |
| 6 | read+write | `rw-` |
| 7 | read+write+execute | `rwx` |

Common combos: `644` (files: owner rw, others r), `755` (dirs/scripts: owner rwx, others rx), `600` (private files, e.g. SSH keys), `700` (private dirs).

## 12.2 Signal Reference
| Signal | Number | Meaning |
|---|---|---|
| SIGHUP | 1 | Hangup / reload config |
| SIGINT | 2 | Interrupt (Ctrl+C) |
| SIGKILL | 9 | Force kill, cannot be caught |
| SIGTERM | 15 | Graceful terminate (default) |
| SIGSTOP | 19 | Pause process |
| SIGCONT | 18 | Resume paused process |

## 12.3 Exit Code Quick Reference
| Code | Meaning |
|---|---|
| 0 | Success |
| 1 | General error |
| 126 | Command not executable |
| 127 | Command not found |
| 130 | Terminated by Ctrl+C (128+SIGINT) |
| 137 | Killed (128+SIGKILL, often OOM in containers) |

## 12.4 Docker vs Kubernetes Terminology
| Docker | Kubernetes Equivalent |
|---|---|
| Container | Pod (wraps one or more containers) |
| `docker run` | Deployment + Pod |
| `docker-compose.yml` | Deployment + Service manifests |
| `docker network` | Service / NetworkPolicy |
| `docker volume` | PersistentVolume / PersistentVolumeClaim |
| Swarm | Kubernetes cluster |
| `docker service scale` | `kubectl scale` / HPA |

## 12.5 MariaDB vs PostgreSQL Command Parity
| Task | MariaDB/MySQL | PostgreSQL |
|---|---|---|
| Connect | `mysql -u user -p` | `psql -U user -d db` |
| List DBs | `SHOW DATABASES;` | `\l` |
| List tables | `SHOW TABLES;` | `\dt` |
| Describe table | `DESCRIBE table;` | `\d table` |
| Create user | `CREATE USER 'u'@'%' IDENTIFIED BY 'p';` | `CREATE USER u WITH PASSWORD 'p';` |
| Grant all | `GRANT ALL ON db.* TO 'u'@'%';` | `GRANT ALL PRIVILEGES ON DATABASE db TO u;` |
| Backup | `mysqldump db > f.sql` | `pg_dump db -f f.sql` |
| Restore | `mysql db < f.sql` | `psql db -f f.sql` |
| Explain query | `EXPLAIN SELECT ...;` | `EXPLAIN ANALYZE SELECT ...;` |
| Replication status | `SHOW SLAVE STATUS\G` | `SELECT * FROM pg_stat_replication;` |

## 12.6 Useful One-Liners for Incident Response
```bash
# Top memory-consuming processes
ps aux --sort=-%mem | head -10

# Top CPU-consuming processes
ps aux --sort=-%cpu | head -10

# Find what's filling up disk
du -ahx / | sort -rh | head -20

# Watch a command every 2 seconds
watch -n 2 'df -h'

# Find largest log files
find /var/log -type f -exec du -h {} \; | sort -rh | head -10

# Check open connections by state
ss -tan | awk '{print $1}' | sort | uniq -c | sort -rn

# Kubernetes: find pods restarting frequently
kubectl get pods -A --sort-by='.status.containerStatuses[0].restartCount'

# Kubernetes: find pods pending scheduling with reason
kubectl get pods -A --field-selector=status.phase=Pending -o wide

# Docker: find containers exiting immediately
docker ps -a --filter "status=exited" --format "table {{.Names}}\t{{.Status}}"

# PostgreSQL: find long-running queries
# SELECT pid, now()-query_start AS duration, query FROM pg_stat_activity ORDER BY duration DESC;

# MariaDB: find long-running queries
# SELECT id, time, info FROM information_schema.processlist ORDER BY time DESC;
```

## 12.7 Recommended Learning Path
1. **Linux Fundamentals → System Admin** — master the shell, permissions, processes, systemd, networking.
2. **Shell scripting** — automate repetitive admin tasks in Bash.
3. **One database deeply** — pick MariaDB or PostgreSQL, learn backup/restore/replication cold.
4. **Git** — non-negotiable for any DevOps role.
5. **Docker** — containerize an app end-to-end (Dockerfile → Compose → registry push).
6. **CI/CD** — wire a pipeline (GitHub Actions/GitLab CI) that builds, tests, and deploys the containerized app.
7. **Kubernetes** — deploy the same app to a cluster (kubeadm/minikube/kind locally, then a managed cluster).
8. **DevSecOps** — bolt on SAST/SCA/image scanning/secret detection to the pipeline built in step 6.
9. **IaC + Config Mgmt** — provision the cluster/VMs with Terraform, configure with Ansible.
10. **Observability** — instrument everything with Prometheus/Grafana + centralized logging (ELK/Loki).

---

*This reference favors the most commonly used flags and commands for real production work. Always verify command availability and exact flags against your specific distribution/version (`man <command>` or `--help`) before running unfamiliar commands in production, especially destructive ones (`rm -rf`, `DROP DATABASE`, `kubectl delete`, `docker system prune`).*
