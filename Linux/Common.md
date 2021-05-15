# Linux

## What is the system user(group)?

You can create a system user(or group) by `adduser --system`(`addgroup --system` for the group). There is no technical difference between the system user and the normal user.

It is a convention for the UID range.

- `SYS_UID_MIN <= UID <= SYS_UID_MAX` is the system user
- `SYS_UID_MAX < UID` is the normal user
- `SYS_GID_MIN <= GID <= SYS_GID_MAX` is the system group
- `SYS_GID_MAX < GID` is the normal group

You can see the boundary values from `/etc/login.defs`.

```
# /etc/login.defs

#
# Min/max values for automatic uid selection in useradd
#
UID_MIN          1000
UID_MAX         60000
# System accounts
#SYS_UID_MIN          100
#SYS_UID_MAX          999

#
# Min/max values for automatic gid selection in groupadd
#
GID_MIN          1000
GID_MAX         60000
# System accounts
#SYS_GID_MIN          100
#SYS_GID_MAX          999
```

references:
  - https://askubuntu.com/a/524010
  - https://unix.stackexchange.com/a/80279