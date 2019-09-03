# rsyslog 配置文件

/etc/rsyslog.conf

## 配置文件分为三个节

- MODULES
- GLOBAL DIRECTIVES
- RULES

```
#### MODULES ####

$ModLoad imuxsock #   提供本地系统日志支持（如通过logger命令）
$ModLoad imjournal # 提供对systemd journal的访问
#$ModLoad imklog # 提供内核日志支持（相当于systemed的systemd-journald.service）
#$ModLoad immark  # 提供-MARK-消息功能


#### GLOBAL DIRECTIVES ####

# Use default timestamp format
$ActionFileDefaultTemplate RSYSLOG_TraditionalFileFormat

# Include all config files in /etc/rsyslog.d/
$IncludeConfig /etc/rsyslog.d/*.conf


#### RULES ####

#kern.*                                                 /dev/console
*.info;mail.none;authpriv.none;cron.none                /var/log/messages
authpriv.*                                              /var/log/secure
```

## 配置规则（RULE）

**格式**

```
facility.priority     action
```

**facility字段**

| facility  | description       |
|:--------- |:----------------- |
| auth      | pam log           |
| authpriv  | ssh, ftp, etc..   |
| cron      | cronjob           |
| ftp       | ftp log           |
| kern      | kernel log        |
| lpr       | printer log       |
| mail      | mail log          |
| news      | news              |
| user      | user process log  |
| uucp      | unix to unix copy |
| local 1~7 | custom log        |

**priority**

| priority | description                       |
|:-------- |:--------------------------------- |
| debug    | debug level                       |
| info     | normal level                      |
| notice   | notice level                      |
| warning  | warning level                     |
| err      | error level                       |
| crit     | critical level                    |
| alert    | alert level                       |
| emerg    | emergency, such as kernel crashed |
| none     | nothing logged                    |

**action**

| action          | description                          |
|:--------------- |:------------------------------------ |
| filename        | path to save the log                 |
| :omusrmsg:users | send log to user                     |
| device          | send to device, such as /dev/console |
| named-pipe      | send to pipe file                    |
| @hostname       | send to host UDP/514 port            |
| @@hostname      | send to host TCP/514 port            |
