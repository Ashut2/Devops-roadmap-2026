# SSH & Ansible Setup — Field Notes

> Mistakes made, lessons learned. Quick reference for next time.

---

## 1. WSL Path Format

In WSL, Windows paths are written differently — backslashes don't work.

| Context | Wrong | Correct |
|---|---|---|
| WSL / Linux | `C:\Users\a\Downloads\key.pem` | `/mnt/c/Users/a/Downloads/key.pem` |

WSL maps your C: drive to `/mnt/c/`. Always use forward slashes.

---

## 2. SSH Connection Timeout

**Symptom:** `ssh: connect to host x.x.x.x port 22: Connection timed out`

**Causes & Fixes:**

### Using a Private IP
- `172.31.x.x` is an **internal AWS IP** — unreachable from outside
- Always use the **Public IPv4** from EC2 console (looks like `13.x.x.x`, `54.x.x.x`)

### Port 22 Not Open
- EC2 → Security Groups → Inbound Rules
- Add rule: **SSH | TCP | 22 | 0.0.0.0/0**

### IP Changed After Stop/Start
- AWS assigns a **new public IP** every time you stop and start an instance
- **Reboot** does NOT change the IP
- Fix: Allocate an **Elastic IP** and associate it → IP never changes

### No Internet Gateway
- EC2 → Instance → Networking → Subnet → Route Table
- Must have: `0.0.0.0/0 → igw-xxxxxxxx`

---

## 3. SSH Key Errors

### "Server refused our key"
- Wrong username for the AMI:

| AMI | Username |
|---|---|
| Ubuntu | `ubuntu` |
| Amazon Linux / AL2 | `ec2-user` |
| Debian | `admin` |
| CentOS | `centos` |

- Wrong `.pem` file — check EC2 → Instance → **Key pair name** matches your file
- Corrupted `.pem` — must start with `-----BEGIN RSA PRIVATE KEY-----`

### IPv4 vs IPv6 Rule Error
- AWS doesn't allow mixing IPv4 and IPv6 in the same rule
- Create **two separate rules**: one for `0.0.0.0/0` (IPv4), one for `::/0` (IPv6)
- For most home connections, just the IPv4 rule is enough

---

## 4. SSH Keygen — Understanding the Files

```bash
ssh-keygen -t rsa -f ~/.ssh/id_rsa
# Press Enter for file path (saves to ~/.ssh/)
# Press Enter twice for passphrase (no passphrase)
```

**Files generated:**

```
~/.ssh/id_rsa            ← Private key (never share this)
~/.ssh/id_rsa.pub        ← Public key (copy this to target servers)
~/.ssh/authorized_keys   ← Public keys allowed to SSH into THIS machine
```

**Why you might get extra files:**
- Running `ssh-keygen` twice generates keys for two algorithms (`id_rsa` + `id_ed25519`)
- Newer Ubuntu defaults to Ed25519; older Ubuntu defaults to RSA
- For consistency with tutorials, explicitly use: `ssh-keygen -t rsa`

**Always press Enter** when asked for file location — saves to correct `~/.ssh/` directory.

---

## 5. How Ansible SSH Auth Works

```
Ansible Server                        Target Server
~/.ssh/id_rsa (private key)  →→→→→   ~/.ssh/authorized_keys (must contain id_rsa.pub)
```

The target server must have the **Ansible server's public key** in its `authorized_keys`.

### Setup Steps

**On Ansible server — copy the public key:**
```bash
cat ~/.ssh/id_rsa.pub
```

**On Target server — add it to authorized_keys:**
```bash
vim ~/.ssh/authorized_keys
# Paste the id_rsa.pub content as a new line
```

**Test the connection:**
```bash
ssh ubuntu@<TARGET-PUBLIC-IP>
```

> ⚠️ Never delete the original AWS key from `authorized_keys` — you'll lock yourself out!

---

## 6. Common Mistakes Summary

| Mistake | What Happens | Fix |
|---|---|---|
| Using private IP `172.31.x.x` | Connection timeout | Use Public IPv4 from EC2 console |
| Port 22 not in Security Group | Connection timeout | Add SSH inbound rule |
| Stop/start instance | IP changes, timeout | Use Elastic IP |
| Wrong username | Key refused | Check AMI username table above |
| `ssh-keygen` run in wrong directory | Keys saved in wrong place | Always run from `~`, press Enter for default path |
| Pasting wrong key in authorized_keys | Permission denied | Paste `id_rsa.pub` from Ansible server, not AWS key |

---

*Notes from hands-on AWS + Ansible setup session.*
