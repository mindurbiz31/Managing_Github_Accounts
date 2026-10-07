# 🧠 Guide: Using Multiple GitHub Accounts on One Machine (Windows)

> 👤 **Example Accounts**

* Personal GitHub: `ifeelpankaj` → Email: `pankaj.personal@gmail.com`
* Work GitHub: `myself-pankaj` → Email: `pankaj.work@company.com`

---

## 🔧 Prerequisites

* Git & Git Bash installed
* Two GitHub accounts (e.g., one personal, one work)
* Basic Git knowledge (init, add, commit, push)

---

## 🔐 Step 1: Generate SSH Keys for Each GitHub Account

### 🔸 Personal Account

Open **Git Bash**:

```bash
ssh-keygen -t ed25519 -C "pankaj.personal@gmail.com"
```

When prompted for file path, save as:

```
/c/Users/YourUsername/.ssh/id_ed25519_personal
```

### 🔸 Work Account

```bash
ssh-keygen -t ed25519 -C "pankaj.work@company.com"
```

Save as:

```
/c/Users/YourUsername/.ssh/id_ed25519_work
```

---

## 🔑 Step 2: Add SSH Keys to SSH Agent

Start the SSH agent and add both keys:

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519_personal
ssh-add ~/.ssh/id_ed25519_work
```

---

## 🗝️ Step 3: Add SSH Public Keys to GitHub

Get public key content:

```bash
cat ~/.ssh/id_ed25519_personal.pub
cat ~/.ssh/id_ed25519_work.pub
```

Then:

1. Go to **GitHub → Settings → SSH and GPG keys** (on both accounts).
2. Click **New SSH key**
3. Name them clearly (`Personal Laptop`, `Work Machine`, etc.)
4. Paste the key content and save.

---

## ⚙️ Step 4: Create SSH Config File with Aliases

This step tells Git which key to use for each GitHub account.

### 📁 Location:

Create/edit file:

```bash
nano ~/.ssh/config
```

Paste this content (with **2 spaces indentation**):

```ssh
# Personal GitHub
Host github-personal
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519_personal
  IdentitiesOnly yes

# Work GitHub
Host github-work
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519_work
  IdentitiesOnly yes
```

✅ Now, SSH will use the correct key **based on the alias** `github-personal` or `github-work`.

---

## 🧪 Step 5: Test SSH Connection

In Git Bash:

```bash
ssh -T git@github-personal
ssh -T git@github-work
```

Expected output:

```
Hi ifeelpankaj! You've successfully authenticated...
Hi myself-pankaj! You've successfully authenticated...
```

---

## 📦 Step 6: Clone Repos Using the Correct SSH Alias

### 🔹 Personal account:

```bash
git clone git@github-personal:ifeelpankaj/project-name.git
```

### 🔹 Work account:

```bash
git clone git@github-work:myself-pankaj/project-name.git
```

---

## 🆕 Step 7: Create a New Project and Push to GitHub (Personal or Work)

### 📁 Create and Initialize New Git Project

```bash
mkdir my-portfolio
cd my-portfolio
git init
```

### 📄 Add Files and Make Initial Commit

```bash
echo "# My Portfolio" > README.md
git add .
git commit -m "Initial commit"
```

---

### 🔗 Add GitHub Remote Based on Account

#### 🔸 Personal Account

```bash
git remote add origin git@github-personal:ifeelpankaj/my-portfolio.git
```

#### 🔸 Work Account

```bash
git remote add origin git@github-work:myself-pankaj/my-portfolio.git
```

✅ This tells Git which GitHub account and SSH key to use.

---

### 🚀 Push Your Code

```bash
git push -u origin master
```

(or `main` depending on your branch)

---

## 👤 Step 8: Set Git Identity Per Project (Optional but Recommended)

Set this inside each Git repo to ensure proper commit authorship.

### 🔸 Personal Repo:

```bash
git config user.name "Pankaj Kholiya"
git config user.email "pankaj.personal@gmail.com"
```

### 🔸 Work Repo:

```bash
git config user.name "Pankaj Kholiya"
git config user.email "pankaj.work@company.com"
```

---

## 🔁 Step 9: Move a Repo from Work to Personal GitHub

### Clone work repo:

```bash
git clone git@github-work:myself-pankaj/old-repo.git
cd old-repo
```

### Create new repo on **personal GitHub** and set new remote:

```bash
git remote remove origin
git remote add origin git@github-personal:ifeelpankaj/new-repo.git
```

### Push to personal GitHub:

```bash
git push -u origin master
```

---

## 🧼 Bonus Tip: View or Edit Remotes

```bash
git remote -v       # View current remotes
git remote set-url origin <new-url>  # Update remote URL
```

---

## 🧯 Common Errors & Fixes

| Error                                        | Reason                                           | Fix                                                                                   |
| -------------------------------------------- | ------------------------------------------------ | ------------------------------------------------------------------------------------- |
| `Could not resolve hostname github-personal` | `~/.ssh/config` file is missing or misconfigured | Ensure the file is named `config` (not `.txt`) and stored in `C:\Users\YourName\.ssh` |
| `Permission denied (publickey)`              | SSH key mismatch or missing key                  | Make sure the correct key is added to GitHub & SSH agent                              |
| `403 error on push`                          | Using HTTPS instead of SSH or wrong account      | Use `git@github-personal:` SSH URL, not `https://github.com/...`                      |

---

## ✅ Summary of Commands

| Task                         | Command Example                                           |
| ---------------------------- | --------------------------------------------------------- |
| Generate SSH Key             | `ssh-keygen -t ed25519 -C "email"`                        |
| Add to SSH agent             | `ssh-add ~/.ssh/id_ed25519_xxx`                           |
| Add GitHub remote (personal) | `git remote add origin git@github-personal:user/repo.git` |
| Push to GitHub               | `git push -u origin master`                               |
| Set Git identity             | `git config user.name/email`                              |

---

[![Use 2 GitHub Accounts on 1 PC | Personal + Work Setup](https://img.youtube.com/vi/_IPSIRqw6KE/maxresdefault.jpg)](https://www.youtube.com/watch?v=_IPSIRqw6KE)

[![Watch Video on YouTube](https://img.youtube.com/vi/cm68GCEcBXU/maxresdefault.jpg)](https://www.youtube.com/watch?v=cm68GCEcBXU)
