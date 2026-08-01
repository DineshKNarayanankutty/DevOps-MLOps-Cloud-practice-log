# Linux Level 1 Certification Practice Log

**Date:** 01-Aug-2026
**Platform:** KodeKloud Engineer – Stratos Datacenter
**Certification Track:** Linux Level 1

---

# Objective

Completed multiple Linux administration tasks focusing on package management, services, firewall configuration, cron scheduling, file permissions, ACLs, text processing, archiving, and Apache web server administration. These tasks simulate real-world Linux system administrator responsibilities.

---

# Task 1: Configure Cron Jobs on Application Servers

## Objective

Install the cron service and schedule automated jobs on all application servers.

### Activities Performed

* Installed `cronie`.
* Started and enabled `crond`.
* Configured cron jobs for the root user.
* Verified scheduled jobs.

### Commands Used

```bash
yum install -y cronie

systemctl enable --now crond

crontab -e

*/5 * * * * echo hello > /tmp/cron_text

30 21 * * * /usr/bin/touch test_passed

crontab -l

systemctl status crond
```

### Skills Practiced

* Package installation
* Service management
* Cron scheduling
* Root crontab configuration

### Key Learning

* `*/5 * * * *` executes every five minutes.
* `30 21 * * *` executes daily at 9:30 PM.
* Always verify cron entries using `crontab -l`.

---

# Task 2: Configure Firewalld

## Objective

Configure Linux firewall rules to allow application traffic.

### Activities Performed

* Installed firewalld.
* Started and enabled service.
* Configured default zone.
* Opened TCP port 5000.

### Commands Used

```bash
yum install -y firewalld

systemctl enable --now firewalld

firewall-cmd --set-default-zone=public

firewall-cmd --permanent --zone=public --add-port=5000/tcp

firewall-cmd --reload

firewall-cmd --zone=public --list-ports

firewall-cmd --get-default-zone
```

### Skills Practiced

* Firewall management
* Service management
* Network security

### Key Learning

* Permanent rules require `--reload`.
* The default zone determines how interfaces are protected.

---

# Task 3: Configure ACL Permissions

## Objective

Modify Access Control Lists on `/etc/hosts`.

### Activities Performed

Configured permissions so that:

* `virat` → No permissions
* `vivek` → Read only
* `sysadmin` group → Read and Write

### Commands Used

```bash
getfacl /etc/hosts

setfacl -m u:virat:--- /etc/hosts

setfacl -m u:vivek:r-- /etc/hosts

setfacl -m g:sysadmin:rw- /etc/hosts

getfacl /etc/hosts
```

### Skills Practiced

* ACL management
* Linux permissions
* Security hardening

### Issue Faced

Initially `/etc/hosts` contained only default permissions.

### Resolution

Applied ACL entries manually using `setfacl`.

### Key Learning

ACLs provide more granular permissions than standard Linux ownership and permission bits.

---

# Task 4: Create Website Landing Page

## Objective

Create the default web page for Apache.

### Activities Performed

Created:

```
/var/www/html/index.html
```

Added:

```
Welcome to the KKE labs!
```

### Commands Used

```bash
mkdir -p /var/www/html

echo "Welcome to the KKE labs!" > /var/www/html/index.html

cat /var/www/html/index.html
```

### Skills Practiced

* File creation
* Redirection
* Apache document root

---

# Task 5: Archive Log Files

## Objective

Create compressed and uncompressed archives of `/var/log`.

### Activities Performed

Created:

* logs.tar
* logs.tar.gz

### Commands Used

```bash
tar -cf /home/natasha/logs.tar /var/log

tar -czf /home/natasha/logs.tar.gz /var/log

ls -lh /home/natasha/
```

### Skills Practiced

* Backup creation
* Archive management
* Compression

### Key Learning

Common tar options:

* `-c` Create archive
* `-f` Filename
* `-z` gzip compression

---

# Task 6: Linux Text Processing

## Objective

Manipulate file contents using Linux utilities.

### Activities Performed

Deleted lines containing:

```
code
```

Created:

```
BSD_DELETE.txt
```

Replaced standalone word:

```
and → is
```

Created:

```
BSD_REPLACE.txt
```

### Commands Used

```bash
grep -v 'code' /home/BSD.txt > /home/BSD_DELETE.txt

sed 's/\<and\>/is/g' /home/BSD.txt > /home/BSD_REPLACE.txt
```

### Skills Practiced

* grep
* sed
* Text manipulation
* Regular expressions

### Key Learning

Word boundaries prevent replacing partial words like:

```
android
candy
standard
```

---

# Task 7: Install Apache HTTP Server

## Objective

Prepare application server for website hosting.

### Activities Performed

* Installed Apache.
* Started service.
* Enabled auto-start.

### Commands Used

```bash
yum install -y httpd

systemctl enable --now httpd

systemctl status httpd

rpm -q httpd
```

### Skills Practiced

* Package management
* Service management

---

# Task 8: Remove Unused Package

## Objective

Remove `logrotate` from all application servers.

### Activities Performed

Removed package using package manager.

### Commands Used

```bash
yum remove -y logrotate

rpm -q logrotate
```

### Skills Practiced

* Package removal
* Package verification

---

# Linux Commands Practiced Today

### Package Management

```bash
yum install
yum remove
rpm -q
```

### Service Management

```bash
systemctl start
systemctl enable
systemctl enable --now
systemctl status
systemctl is-active
```

### Firewall

```bash
firewall-cmd
```

### Cron

```bash
crontab -e
crontab -l
```

### ACL

```bash
setfacl
getfacl
```

### Text Processing

```bash
grep
grep -v
sed
```

### Archive

```bash
tar
```

### File Operations

```bash
mkdir
echo
cat
ls
```

---

# Problems Encountered

### 1. ACL Not Present

Observed only default permissions:

```
user::rw-
group::r--
other::r--
```

**Solution**

Applied ACL entries using `setfacl`.

---

### 2. Firewall Configuration

Needed to ensure:

* Zone = `public`
* Rule added permanently
* Firewall reloaded after changes

---

### 3. Cron Verification

Verified scheduled jobs using:

```bash
crontab -l
```

instead of assuming successful configuration.

---

# Linux Concepts Reinforced

* Linux package management using YUM
* Managing system services with systemctl
* Cron job scheduling
* Firewalld configuration
* Linux ACLs
* File permissions
* Archive creation using tar
* Apache installation and management
* Text processing using grep and sed
* Package removal
* Verification of Linux configurations
