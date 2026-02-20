# Today's Learning Notes 📝

---

## 1. Creating a User Without Home Directory

```bash
sudo useradd -M <username>
```

**Key concept:** `-M` flag prevents physical directory creation on disk, but `/etc/passwd` will still show `/home/<username>` as a field — that's just metadata.

**Correct verification:**
```bash
ls /home/<username>   # should say "No such file or directory"
```
❌ Don't rely on `grep /etc/passwd` to confirm absence of home directory.
`NOTE` : consider a google form with address field , its not neccesary for a person to own house physcially at the address he filled in the google form.

---

## 2. Creating a User With Expiry Date

```bash
sudo useradd -e 2027-03-28 <username>
# or
sudo useradd --expiredate 2027-03-28 <username>
```

**Correct verification:**
```bash
chage -l <username>        # human readable expiry info
sudo grep <username> /etc/shadow   # raw expiry data
```
❌ Don't check `/etc/default/useradd` — that's for default settings, not specific users.

---

## 3. Changing Expiry Date of Existing User

```bash
sudo chage -E 2027-03-28 <username>
# or
sudo usermod -e 2027-03-28 <username>
```

❌ `useradd` won't work — user already exists.

---

## 4. Important File Reference

| File | Purpose |
|---|---|
| `/etc/passwd` | Basic user info — shell, home dir field |
| `/etc/shadow` | Password & expiry dates |
| `/etc/default/useradd` | Default settings for future new users |

---

## 5. Key Terminology

| Term | Meaning |
|---|---|
| **RAM** | Temporary workspace, lost on reboot |
| **Disk Space** | Permanent storage, survives reboot |
| **`-M` flag** | No disk space allocated for home dir |

> 💡 RAM = your desk | Disk = your drawer

---

## 6. Command Cheat Sheet

| Task | Command |
|---|---|
| Create user without home dir | `useradd -M <user>` |
| Create user with expiry | `useradd -e YYYY-MM-DD <user>` |
| Change expiry (existing user) | `chage -E YYYY-MM-DD <user>` |
| Modify existing user | `usermod -e YYYY-MM-DD <user>` |
| View expiry details | `chage -l <user>` |
| Verify home dir existence | `ls /home/<user>` |
