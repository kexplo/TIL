# systemd

## The '@' symbol in unit name

> A template unit must have a single "@" at the end of the name (right before the type suffix). The name of the full unit is formed by inserting the instance name between "@" and the unit type suffix. In the unit file itself, the instance parameter may be referred to using "%i"

from : https://www.freedesktop.org/software/systemd/man/systemd.unit.html

example:

`/etc/systemd/system/echo@.service`

```conf
[Unit]
Description=Echo '%I'

[Service]
Type=oneshot
ExecStart=/bin/echo %i
StandardOutput=syslog
```

```bash
systemctl start echo@foo.service
systemctl start echo@bar.service
```

## '-'(minus) prefix

- `ExecStartPre=`, `ExecStartPost=` : If any of those commands (not prefixed with "-") fail, the rest are not executed and the unit is considered failed.

Table 1. Special executable prefixes

| prefix | Effect |
|--------|--------|
| "@" | If the executable path is prefixed with "@", the second specified token will be passed as "argv[0]" to the executed process (instead of the actual filename), followed by the further arguments specified. |
| "-" | If the executable path is prefixed with "-", an exit code of the command normally considered a failure (i.e. non-zero exit status or abnormal exit due to signal) is recorded, but has no further effect and is considered equivalent to success. |
| ":" | If the executable path is prefixed with ":", environment variable substitution (as described by the "Command Lines" section below) is not applied. |
| "+" | If the executable path is prefixed with "+" then the process is executed with full privileges. In this mode privilege restrictions configured with User=, Group=, CapabilityBoundingSet= or the various file system namespacing options (such as PrivateDevices=, PrivateTmp=) are not applied to the invoked command line (but still affect any other ExecStart=, ExecStop=, … lines). |
| "!" | Similar to the "+" character discussed above this permits invoking command lines with elevated privileges. However, unlike "+" the "!" character exclusively alters the effect of User=, Group= and SupplementaryGroups=, i.e. only the stanzas that affect user and group credentials. Note that this setting may be combined with DynamicUser=, in which case a dynamic user/group pair is allocated before the command is invoked, but credential changing is left to the executed process itself. |
| "!!" | This prefix is very similar to "!", however it only has an effect on systems lacking support for ambient process capabilities, i.e. without support for AmbientCapabilities=. It's intended to be used for unit files that take benefit of ambient capabilities to run processes with minimal privileges wherever possible while remaining compatible with systems that lack ambient capabilities support. Note that when "!!" is used, and a system lacking ambient capability support is detected any configured SystemCallFilter= and CapabilityBoundingSet= stanzas are implicitly modified, in order to permit spawned processes to drop credentials and capabilities themselves, even if this is configured to not be allowed. Moreover, if this prefix is used and a system lacking ambient capability support is detected AmbientCapabilities= will be skipped and not be applied. On systems supporting ambient capabilities, "!!" has no effect and is redundant. |

from : https://www.freedesktop.org/software/systemd/man/systemd.service.html

---

## Snippets

```conf
[Unit]
Description=Redis Container
After=docker.service
Requires=docker.service

[Service]
TimeoutStartSec=0
Restart=always
ExecStartPre=-/usr/bin/docker stop %n
ExecStartPre=-/usr/bin/docker rm %n
ExecStartPre=/usr/bin/docker pull redis
ExecStart=/usr/bin/docker run --rm --name %n redis

[Install]
WantedBy=multi-user.target
```
