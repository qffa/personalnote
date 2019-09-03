# auditd

## 配置

```
/etc/audit/auditd.conf
/etc/audit/rules.d/xxxx.rules
```

##  rules

示例

```
-a always,exit -F arch=b64 -S chmod,fchmod,fchmodat
-a always,exit -F arch=b64 -S chown,fchown,lchown,fchownat
-a always,exit -F arch=b32 -S mknod,mknodat
-a always,exit -F arch=b64 -S mknod,mknodat
-a always,exit -F arch=b64 -S adjtimex,settimeofday
-a always,exit -F arch=b64 -S clock_settime -F a0=0x0
-w /etc/group -p wa -k Linux_audit_etcgroup
-w /etc/passwd -p wa -k Linux_audit_passwd
-w /etc/gshadow -p rwxa -k Linux_audit_gshadow
-w /etc/shadow -p rwxa -k Linux_audit_shadow
-w /etc/security/opasswd -p rwxa -k Linux_audit_opasswd
-w /etc/sudoers -p rwxa -k Linux_audit_sudoers
-w /etc/sudoers.d -p rwxa -k Linux_audit_sudoers
-e 2
```

## 命令

```
$ auditctl -l
$ auditctl -D
$ service auditd restart
```

## Log文件

```
/var/log/audit/audit.log
```

可以通过设置将audit.log转发进rsyslog及远程log server

```
#Configure local file to remote log server
$ModLoad imfile
$InputFileName /var/log/audit/audit.log
$InputFileTag audit_log
$InputFileFacility local6
$InputFileSeverity info
$InputFileStateFile audit.log
$InputFilePollInterval 1
$InputFilePersistStateInterval 1
$InputRunFileMonitor

local6.*                @@10.1.1.1
authpriv.*              @10.1.1.1
```

## 搜索log

```
$ ausearch --interpret --arch c000003e
$ ausearch -i --key sshd_config
```

## log格式

```
type=SYSCALL msg=audit(1364481363.243:24287): arch=c000003e syscall=2 success=no exit=-13 a0=7fffd19c5592 a1=0 a2=7fffd19c4b50 a3=a items=1 ppid=2686 pid=3538 auid=1000 uid=1000 gid=1000 euid=1000 suid=1000 fsuid=1000 egid=1000 sgid=1000 fsgid=1000 tty=pts0 ses=1 comm="cat" exe="/bin/cat" subj=unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023 key="sshd_config"
type=CWD msg=audit(1364481363.243:24287):  cwd="/home/shadowman"
type=PATH msg=audit(1364481363.243:24287): item=0 name="/etc/ssh/sshd_config" inode=409248 dev=fd:00 mode=0100600 ouid=0 ogid=0 rdev=00:00 obj=system_u:object_r:etc_t:s0  objtype=NORMAL cap_fp=none cap_fi=none cap_fe=0 cap_fver=0
type=PROCTITLE msg=audit(1364481363.243:24287) : proctitle=636174002F6574632F7373682F737368645F636F6E666967
```

有4条记录，每条记录以`type=`开始，可以看到他们的时间戳一致。

**log详细解释**

https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/security_guide/sec-understanding_audit_log_files
