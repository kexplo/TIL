# WSL

Windows Subsystem for Linux

## Installation

Open PowerShell as Admin:

```powershell
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
```

## Fix "Could not load host key: ..." Errors

```bash
$ sudo service ssh start
 * Starting OpenBSD Secure Shell server sshd
Could not load host key: /etc/ssh/ssh_host_rsa_key
Could not load host key: /etc/ssh/ssh_host_ecdsa_key
Could not load host key: /etc/ssh/ssh_host_ed25519_key
```

Fix:

```bash
$ sudo ssh-keygen -A
```