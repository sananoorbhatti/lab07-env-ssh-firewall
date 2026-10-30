# Lab 07 Challenge — Environment Audit and Ping Block

## Job 1 — Display Environment Variables

**Answer:** PATH contains directories searched for executable commands. LANG defines language and locale settings. PWD shows the current working directory.

**Commands:**
```bash
printenv | grep '^PATH='
printenv | grep '^LANG='
printenv | grep '^PWD='
```

**Recorded values:**
- PATH: /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:/home/sananoorbhatti
- LANG: en_US.UTF-8
- PWD: /home/sananoorbhatti

## Job 2 — Create Temporary Student Variables

**Answer:** Exported variables are available in the current shell and its child processes. They disappear after logging out and starting a new login session.

**Commands:**
```bash
export STUDENT_NAME="Sana Noor Bhatti"
export STUDENT_ROLL_NUMBER="FA24B1-SE-022"
export STUDENT_SEMESTER="5"
printenv | grep '^STUDENT_'
```

To test after logout and login:

```bash
exit
ssh sananoorbhatti@192.168.227.129
printenv | grep '^STUDENT_'
```

**Result:** The temporary variables disappeared after logging in again.

## Job 3 — Make Student Variables Permanent

**Answer:** The variables were added to the `.bashrc` file so they are automatically available whenever a new Bash session starts.

**Commands:**
```bash
nano ~/.bashrc
```

Add these lines at the end of the file:

```bash
export STUDENT_NAME="Sana Noor Bhatti"
export STUDENT_ROLL_NUMBER="FA24B1-SE-022"
export STUDENT_SEMESTER="5"
```

Apply and verify:

```bash
source ~/.bashrc
printenv | grep '^STUDENT_'
```

To verify after a new login:

```bash
exit
ssh sananoorbhatti@192.168.227.129
printenv | grep '^STUDENT_'
```

**Result:** All three student variables remained available after logging in again.

## Job 4 — Block and Restore Ping

**Answer:** The incoming ICMP echo-request rule in `/etc/ufw/before.rules` was changed from ACCEPT to DROP to block ping requests. It was then changed back to ACCEPT to restore ping access.

**Commands to block ping:**
```bash
sudo cp /etc/ufw/before.rules ~/before.rules.backup
sudo vim /etc/ufw/before.rules
```

Change the incoming echo-request rule from ACCEPT to DROP.

Reload the firewall:
```bash
sudo ufw reload
```

Test from Windows Git Bash:
```bash
ping -n 4 192.168.227.129
```

**Result:** Ping requests timed out while the rule was set to DROP.

**Commands to restore ping:**

Change DROP back to ACCEPT in `/etc/ufw/before.rules`.

Reload the firewall:
```bash
sudo ufw reload
```

Test again:
```bash
ping -n 4 192.168.227.129
```

**Result:** Ping replies were received again after restoring the rule.
