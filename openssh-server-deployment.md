
## SSH Server Deployment on AlmaLinux 9

This guide covers the complete setup and basic architecture of an SSH server on AlmaLinux 9, including best practices for secure deployment.

---

### 🧩 Step 1: Understand Basic SSH Architecture

#### What is SSH?
SSH (Secure Shell) is a cryptographic network protocol to securely:
- Access remote systems
- Execute commands remotely
- Transfer files (SCP, SFTP)
- Forward ports (tunnels)

#### Components:
| Component | Purpose |
|-----------|----------|
| SSH Server (sshd) | Runs on server, listens on port (default 22) |
| SSH Client (ssh) | Connects to server to start a secure session |
| Authentication | Password-based or SSH key-based |
| Encryption | Data between client and server is encrypted |
| Optional | Port forwarding, X11 forwarding, agent forwarding |

---

### 🏗️ Step 2: Install OpenSSH Server on AlmaLinux 9

```
sudo dnf install -y openssh-server
sudo systemctl enable sshd
sudo systemctl start sshd
sudo systemctl status sshd
```

---

### 🔥 Step 3: Configure Firewall

```
sudo firewall-cmd --permanent --add-service=ssh
sudo firewall-cmd --reload
```

---

### ⚙️ Step 4: Check SSH Server Configuration

The main config file is `/etc/ssh/sshd_config`.

Important settings:

```
Port 22
PermitRootLogin no
PasswordAuthentication yes
PubkeyAuthentication yes
AllowUsers user1 user2
```

Restart sshd after changes:

```
sudo systemctl restart sshd
```

---

### 👤 Step 5: Create Users

```
sudo useradd username
sudo passwd username
sudo mkdir -p /home/username
sudo chown username:username /home/username
```

---

### 🔑 Step 6: Set up SSH Key Authentication (Recommended)

1. Generate SSH key pair on local machine:

```
ssh-keygen -t rsa -b 4096
```

2. Copy public key to server:

```
ssh-copy-id username@server_ip
```

Or manually:

```
cat ~/.ssh/id_rsa.pub | ssh username@server_ip 'mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys'
```

3. Test SSH login:

```
ssh username@server_ip
```

---

## 🛡️ Step 7: Secure SSH Server (Best Practices)

- Change default port:
  ```
  Port 2222
  ```

- Disable root login:
  ```
  PermitRootLogin no
  ```

- Disable password login (after setting up keys):
  ```
  PasswordAuthentication no
  ```

- Install Fail2Ban to prevent brute-force attacks.
- Restrict users:
  ```
  AllowUsers username
  ```

---

## 🔍 Step 8: Verify SSH Service

Check if SSH is listening:

```
sudo ss -tuln | grep ssh
```

Check active connections:

```
sudo who
sudo w
```

Check logs:

```
sudo journalctl -u sshd
sudo tail -f /var/log/secure
```

---

## ✅ Summary

| Step | Action |
|------|--------|
| 1.  | Install OpenSSH |
| 2.  | Start and enable service |
| 3.  | Open firewall port |
| 4.  | Configure sshd_config |
| 5.  | Create users |
| 6.  | Set up SSH keys |
| 7.  | Harden SSH server |
| 8.  | Test and verify |

---

