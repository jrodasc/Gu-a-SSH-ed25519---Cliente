# 🔐 SSH Hardening with ed25519 Keys (Client & Server Side)

This repository provides a practical and secure guide to harden SSH access using **ed25519 keys**, covering both **client-side** and **server-side** configurations following modern security best practices.

It is intended for DevOps, Cloud, and Infrastructure engineers who want to reduce the SSH attack surface on production systems.

---

## 🧠 Why ed25519?

`ed25519` is a modern elliptic-curve algorithm with multiple advantages over traditional RSA keys:

- 🔒 Strong cryptographic security
- ⚡ Faster authentication
- 🗜️ Smaller key size
- ✅ Recommended by modern security standards

---

## 🎯 Purpose

The main goals of this guide are to:

- Secure SSH authentication using modern keys
- Eliminate weak or legacy access methods
- Standardize SSH access across environments
- Provide a repeatable and production-ready setup

---

## 🛠️ Technologies & Tools

- Linux
- OpenSSH
- ed25519 cryptography
- SSH client and server configuration
- Secure server access

---

## 🚀 Client-Side Hardening

### 1️⃣ Generate an ed25519 SSH Key

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```
- Use the default location
- Always protect your key with a strong passphrase

## 2️⃣ Start SSH Agent and Load Key

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

## 3️⃣ Copy Public Key to Server

```bash
ssh-copy-id -i ~/.ssh/id_ed25519.pub user@server_ip
```

## 4️⃣ Configure SSH Client

Edit the SSH client configuration file:

```bash
nano ~/.ssh/config
```

Example configuration:

```bash
Host production-server
    HostName server_ip
    User user
    IdentityFile ~/.ssh/id_ed25519
    IdentitiesOnly yes
```

## 5️⃣ Test Connection
```bash
ssh production-server
```
## 🔐 Server-Side Hardening

⚠️ Always test changes in a separate session before restarting SSH.

---

## 1️⃣ Edit SSH Daemon Configuration
```bash
sudo nano /etc/ssh/sshd_config
```

Apply the following recommended settings:

```bash
# Disable root login
PermitRootLogin no

# Disable password authentication
PasswordAuthentication no
ChallengeResponseAuthentication no

# Enable public key authentication
PubkeyAuthentication yes

# Limit authentication attempts
MaxAuthTries 3

# Disable empty passwords
PermitEmptyPasswords no

# Use modern crypto
HostKeyAlgorithms ssh-ed25519
PubkeyAcceptedAlgorithms ssh-ed25519

# Log level
LogLevel VERBOSE
```

## 2️⃣ Restart SSH Service
```bash
sudo systemctl restart sshd
```

Or on some systems:

```bash
sudo systemctl restart ssh
```

3️⃣ Validate Configuration

Open a new terminal session and test:

```bash
ssh user@server_ip
```

❗ If login fails, do not close your existing session.

## 🔥 Optional Server Hardening (Recommended)
🔹 Change Default SSH Port

```bash
Port 2222

sudo systemctl restart sshd
```
🔹 Restrict SSH Access to Specific Users

```bash
AllowUsers user1 user2
```

🔹 Firewall Example (UFW)
```bash
sudo ufw allow 2222/tcp
sudo ufw reload
```
## 🛡️ Security Best Practices

Use one SSH key per user and per environment

Rotate keys periodically

Remove unused or compromised keys

Never expose private keys

Combine SSH hardening with firewall rules and intrusion prevention tools

## 📌 Use Cases

Securing cloud servers

Standardizing access for DevOps teams

Hardening production environments

Personal and enterprise infrastructure

---

# 🛡️ Fail2Ban Hardening

Fail2Ban is an intrusion prevention tool that monitors log files and bans IP addresses that show malicious behavior, such as repeated failed SSH login attempts.

It adds an additional security layer on top of SSH hardening and firewall rules.

---

## 📦 Install Fail2Ban

### Debian / Ubuntu

```bash
sudo apt update
sudo apt install fail2ban -y
RHEL / CentOS / Rocky
sudo dnf install fail2ban -y
```
## ⚙️ Basic Configuration
Fail2Ban should be configured using local override files.

Create Local Configuration
```bash
sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
```

Edit the local configuration:

```bash
sudo nano /etc/fail2ban/jail.local
```
## 🔐 Enable SSH Protection
Locate the [sshd] section and ensure it is enabled:

```bash
[sshd]
enabled = true
port = 2222
logpath = %(sshd_log)s
maxretry = 3
bantime = 1h
findtime = 10m
Adjust the port if you changed the default SSH port.
```

## ▶️ Start and Enable Fail2Ban
```bash
sudo systemctl enable fail2ban
sudo systemctl start fail2ban
```
🔍 Verify Fail2Ban Status
Check Fail2Ban overall status:

```bash
sudo fail2ban-client status
```
Check SSH jail status:
```bash
sudo fail2ban-client status sshd
```
## 🚫 Unban an IP (if needed)
```bash
sudo fail2ban-client set sshd unbanip <IP_ADDRESS>
```

## 🧠 Best Practices
Combine Fail2Ban with firewall rules

Monitor banned IPs periodically

Adjust ban times based on security requirements

Avoid overly aggressive bans that may lock out legitimate users

⚠️ Important Notes
Always whitelist your own IP if working from a fixed location

Test Fail2Ban rules before applying them in production

Keep Fail2Ban updated

---

## 👤 Author

Juan Rodas
DevOps & Cloud Engineer

🔗 LinkedIn: https://linkedin.com/in/juanrodas

---
⭐ Final Note

SSH hardening is one of the simplest and most effective security improvements you can apply.

Small configuration changes can significantly reduce infrastructure risk.
