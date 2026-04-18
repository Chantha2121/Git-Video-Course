# របៀបបង្កើត SSH Key ក្នុង GitHub

ឯកសារនេះបង្ហាញពីជំហានបង្កើត SSH Key រួចភ្ជាប់ទៅ GitHub ដើម្បីអាច `push/pull` ដោយមិនចាំបាច់វាយពាក្យសម្ងាត់រៀងរាល់ដង។

## 1) ពិនិត្យថាមាន SSH Key រួចឬនៅ

បើក Terminal ហើយវាយ៖

```bash
ls -al ~/.ssh
```

បើឃើញឯកសារ `id_ed25519` និង `id_ed25519.pub` មានន័យថាអ្នកមាន key រួចហើយ។

## 2) បង្កើត SSH Key ថ្មី

ប្រើ email ដូចគណនី GitHub របស់អ្នក៖

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```

ក្រោយពេលរត់ command:

- ចុច `Enter` ដើម្បីប្រើទីតាំង default (`~/.ssh/id_ed25519`)
- បញ្ចូល `passphrase` (អាចទុកទទេបាន ប៉ុន្តែដាក់ passphrase មានសុវត្ថិភាពជាង)

## 3) បើក SSH Agent និងបន្ថែម Key

### សម្រាប់ macOS / Linux

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

### សម្រាប់ Windows (Git Bash)

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

## 4) ចម្លង Public Key

### macOS

```bash
pbcopy < ~/.ssh/id_ed25519.pub
```

### Linux

```bash
cat ~/.ssh/id_ed25519.pub
```

### Windows (Git Bash)

```bash
cat ~/.ssh/id_ed25519.pub
```

បើប្រើ `cat` សូមចម្លងអក្សរទាំងមូល ចាប់ពី `ssh-ed25519 ...` រហូតដល់ចប់។

## 5) បន្ថែម Key ចូល GitHub

1. ចូល GitHub
2. ចូលទៅ `Settings`
3. ចុច `SSH and GPG keys`
4. ចុច `New SSH key`
5. ដាក់ Title (ឧ. `My MacBook`)
6. Paste public key ចូលប្រអប់ `Key`
7. ចុច `Add SSH key`

## 6) តេស្តការភ្ជាប់

```bash
ssh -T git@github.com
```

បើជោគជ័យ អ្នកនឹងឃើញសារ៖

```text
Hi username! You've successfully authenticated, but GitHub does not provide shell access.
```

## 7) ប្ដូរ Remote URL ទៅ SSH (បើ repository នៅប្រើ HTTPS)

ពិនិត្យ remote បច្ចុប្បន្ន៖

```bash
git remote -v
```

ប្ដូរ URL ទៅ SSH៖

```bash
git remote set-url origin git@github.com:USERNAME/REPOSITORY.git
```

ពិនិត្យម្ដងទៀត៖

```bash
git remote -v
```

## 8) Create/update SSH config

Open:

```
nano ~/.ssh/config
```

Add this:
```
Host github.com
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/id_ed25519
```
Save (CTRL+O, ENTER, CTRL+X)

## បញ្ហាដែលជួបញឹកញាប់

- `Permission denied (publickey)`:
	- ពិនិត្យថា key ត្រូវបាន `ssh-add` រួច
	- ពិនិត្យថា public key ត្រូវបាន paste ត្រឹមត្រូវក្នុង GitHub
	- ពិនិត្យថា remote URL ជា SSH មិនមែន HTTPS
- មាន key ច្រើន:
	- ត្រូវបញ្ជាក់ key តាមឯកសារ `~/.ssh/config`

---

ចប់ជំហានទាំងនេះ អ្នកអាចប្រើ GitHub តាម SSH បានរួចរាល់។
