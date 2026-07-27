# 🚀 Web Development / Lecture 1: Web Fundamentals & Environment Setup

მოგესალმებით **1-ელ ლექციაში**! ამ დოკუმენტაციაში განხილულია ინტერნეტის/ვების ფუნქციონირების ფუნდამენტური პრინციპები, სერვერების არქიტექტურა, პროტოკოლები, Mac-ზე სამუშაო გარემოს (Git & SSH) გამართვა და Git-ის საბაზო ბრძანებები.

Welcome to **Lecture 1**! This documentation covers web fundamentals, server architectures, network protocols, setting up the development environment (Git & SSH) on macOS, and essential Git commands.

---

## 🌐 1. როგორ მუშაობს ვები? / How the Web Works

### 🌐 ბრაუზერის მუშაობის პრინციპი (How Web Browsers Work)

- **Client (კლიენტი / ბრაუზერი):** მომხმარებლის ინტერფეისი (Chrome, Safari, Firefox), რომელიც აგზავნის მოთხოვნას (Request).
- **Server (სერვერი):** შორს მდებარე კომპიუტერი, სადაც ინახება საიტის ფაილები (HTML, CSS, JS) და აბრუნებს პასუხს (Response).
- **DNS (Domain Name System):** ინტერნეტის „ტელეფონების წიგნი", რომელიც დომენის სახელს (მაგ: `google.com`) გარდაქმნის სერვერის IP მისამართად (მაგ: `142.250.190.46`).

---

### 🔒 HTTP vs HTTPS

| პროტოკოლი                                | განმარტება (Georgian)                                                          | English Description                                                            | უსაფრთხოება               |
| :--------------------------------------- | :----------------------------------------------------------------------------- | :----------------------------------------------------------------------------- | :------------------------ |
| **HTTP** _(HyperText Transfer Protocol)_ | მონაცემთა გადაცემის საბაზო პროტოკოლი. მონაცემები მოგზაურობს ღიად (Plain Text). | Basic data transfer protocol. Data is sent as unencrypted plain text.          | ❌ არაუსაფრთხო (Unsecure) |
| **HTTPS** _(HTTP Secure)_                | HTTP პროტოკოლი შიფრაციით (SSL/TLS სერტიფიკატით). მონაცემები დაშიფრულია.        | HTTP with encryption (SSL/TLS certificate). All transmitted data is encrypted. | ✅ უსაფრთხო (Secure)      |

---

### 🖥️ ცენტრალიზებული vs დეცენტრალიზებული სერვერები

- **ცენტრალიზებული სერვერები (Centralized Servers):**
  - **არსი:** ყველა მონაცემი და ლოგიკა ინახება ერთ ცენტრალურ სერვერზე ან ერთი კომპანიის მართვის ქვეშ (მაგ. Facebook, AWS, ტრადიციული ბანკები).
  - **პლუსი:** მაღალი სიჩქარე, მარტივი მართვა.
  - **მინუსი:** Single Point of Failure (თუ ცენტრალური სერვერი გაითიშა, სისტემა ითიშება).
- **დეცენტრალიზებული / განაწილებული (Decentralized / Distributed):**
  - **არსი:** მონაცემები გადანაწილებულია მრავალ დამოუკიდებელ კვანძს (Node/Peer) შორის (მაგ. Torrent, Blockchain, IPFS).
  - **პლუსი:** მაღალი მდგრადობა, არ არსებობს ერთი ცენტრალური მმართველი ორგანოს კონტროლი.
  - **მინუსი:** რთული არქიტექტურა, შედარებით დაბალი სიჩქარე.

---

## 🛠️ 2. Git & GitHub Fundamentals

- **Git:** ვერსიების კონტროლის ლოკალური სისტემა (Version Control System), რომელიც ინახავს კოდის ცვლილებების ისტორიას შენს კომპიუტერზე.
- **GitHub:** ღრუბლოვანი პლატფორმა (Cloud Hosting), სადაც ვინახავთ Git რეპოზიტორიებს და ვთანამშრომლობთ სხვა დეველოპერებთან.

---

## 🔑 3. SSH Key-ს გენერაცია & კონფიგურაცია (macOS)

SSH (Secure Shell) უზრუნველყოფს უსაფრთხო, დაშიფრულ კავშირს შენს Mac-სა და GitHub-ს შორის პაროლის ყოველჯერზე შეყვანის გარეშე.

### Step 3.1: `.ssh` საქაღალდის შექმნა

```bash
mkdir -p ~/.ssh
```

### Step 3.2: SSH გასაღების შექმნა (Custom სახელებით)

```bash
# შეცვალე ელფოსტა შენი GitHub-ის მეილით
ssh-keygen -t ed25519 -C "your_email@gmail.com"
```

როდესაც ტერმინალი გკითხავს ფაილის შენახვის ადგილს:

```
Enter file in which to save the key (/Users/admin/.ssh/id_ed25519):
```

მიუთითე სრული გზა custom სახელით (მაგ: `/Users/admin/.ssh/my_ssh_key`).

(Passphrase-ის მოთხოვნაზე უბრალოდ დააჭირე ENTER-ს ორჯერ).

### Step 3.3: SSH Config ფაილის გამართვა

ვინაიდან გასაღებს არასტანდარტული სახელი დავარქვით, აუცილებელია Mac-ის SSH აგენტს მივუთითოთ ეს გასაღები:

```bash
echo -e "Host github.com\n  HostName github.com\n  User git\n  IdentityFile ~/.ssh/my_ssh_key\n  AddKeysToAgent yes\n  UseKeychain yes" > ~/.ssh/config
```

(ყურადღება: `my_ssh_key`-ს ნაცვლად ჩაწერე შენ მიერ დარქმეული სახელი).

### Step 3.4: Public Key-ს დაკოპირება და GitHub-ზე დამატება

```bash
# დააკოპირე საჯარო გასაღები კლიპბორდში
pbcopy < ~/.ssh/my_ssh_key.pub
```

GitHub-ზე დამატება:

1. შედი GitHub -> Settings -> SSH and GPG keys.
2. დააჭირე **New SSH key**.
3. Title: მიუთითე მოწყობილობის სახელი (მაგ: My Mac).
4. Key: გააკეთე Paste (Cmd + V).
5. დააჭირე **Add SSH key**.

### Step 3.5: კავშირის შემოწმება

```bash
ssh -T git@github.com
```

(თუ ტერმინალმა გკითხა `Are you sure you want to continue connecting (yes/no)?`, ჩაწერე `yes` და დააჭირე ENTER-ს).

---

## 📦 4. Git-ის ინსტალაცია & Global Config (macOS)

### Step 4.1: ინსტალაცია

```bash
xcode-select --install
```

(გამოტანილ ფანჯარაში დააჭირე Install -> Agree).

შემოწმება:

```bash
git --version
```

### Step 4.2: Global Configuration

```bash
# მომხმარებლის სახელი
git config --global user.name "Your Name"

# ელფოსტა (GitHub-ის მეილი)
git config --global user.email "your_email@gmail.com"

# ნაგულისხმევი Branch-ის დასახელება
git config --global init.defaultBranch main
```

### Step 4.3: კონფიგურაციის შემოწმება

```bash
git config --list
```
