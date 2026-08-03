# 🚀 Web Development / Lecture 4: HTML Clean Code, W3C Validation & Git Workflow

მოგესალმებით **მე-4 ლექციაში**! ამ დოკუმენტაციაში განხილულია HTML-ის სუფთა კოდის სტანდარტები, W3C ვალიდაცია და Git/GitHub-ის სრული სამუშაო ციკლი — ლოკალური რეპოზიტორიის შექმნიდან GitHub Pages-ზე Live დეპლოიმენტამდე.

Welcome to **Lecture 4**! This documentation covers HTML clean code standards, W3C markup validation, and the complete Git/GitHub workflow — from initializing a local repository to deploying a live project on GitHub Pages.

---

## 🧹 1. HTML Clean Code Standard

სუფთა კოდი არა მხოლოდ ესთეტიკური მოთხოვნაა — ის პირდაპირ გავლენას ახდენს კოდის წაკითხვადობასა და გუნდურ მუშაობაზე.

### ძირითადი პრინციპები:

- **Indentation (გამოწევა):** ყოველი ჩადგმული (nested) ელემენტი უნდა იყოს 2 ან 4 space-ით გამოწეული მშობელი ელემენტისგან. Tab-ისა და Space-ის შერევა დაუშვებელია.
- **Lowercase ტეგები და ატრიბუტები:** `<DIV CLASS="Box">` ❌ → `<div class="box">` ✅. HTML5 სტანდარტი lowercase-ს მოითხოვს კონსისტენციისთვის.
- **"Div Soup"-ის თავიდან აცილება:** ზედმეტი, უაზრო `<div>`-ების გამოყენების ნაცვლად გამოიყენე სემანტიკური ტეგები — `<header>`, `<nav>`, `<main>`, `<section>`, `<article>`, `<footer>`.
- **VS Code Format Shortcut:** `Shift + Option + F` (macOS) / `Shift + Alt + F` (Windows) — ავტომატურად აწესრიგებს ინდენტაციასა და ფორმატირებას.

```html
<!-- ❌ ცუდი მაგალითი (Div Soup) -->
<div class="header">
  <div class="nav">
    <div class="nav-item">Home</div>
  </div>
</div>

<!-- ✅ სწორი მაგალითი (Semantic + Clean) -->
<header>
  <nav>
    <a href="#" class="nav-item">Home</a>
  </nav>
</header>
```

---

## ✅ 2. W3C Markup Validator

**W3C Validator** ([validator.w3.org](https://validator.w3.org)) — ოფიციალური ინსტრუმენტია HTML/CSS კოდის სინტაქსური სისწორის შესამოწმებლად.

### გამოყენების 3 გზა:

1. **By URI** — უკვე დეპლოირებული საიტის ბმულის შემოწმება.
2. **By File Upload** — ლოკალური `.html` ფაილის ატვირთვა.
3. **By Direct Input** — კოდის პირდაპირ ჩასმა textarea-ში.

### რატომ არის მნიშვნელოვანი:

- ეხმარება SEO-ს (სუფთა Markup სჯობს საძიებო სისტემებისთვის).
- ხსნის Cross-browser თავსებადობის პრობლემებს.
- ავითარებს დისციპლინას სწორი სინტაქსის დაწერაში.

---

## 🛠️ 3. Git & GitHub — შესავალი

- **VCS (Version Control System):** სისტემა, რომელიც აკონტროლებს ფაილების ცვლილებების ისტორიას დროში, საშუალებას გაძლევს დაბრუნდე ნებისმიერ წინა ვერსიაზე.
- **Git:** ლოკალური, დისტრიბუციული VCS — მუშაობს შენს კომპიუტერზე ინტერნეტის გარეშეც.
- **GitHub:** ღრუბლოვანი პლატფორმა Git რეპოზიტორიების ჰოსტინგისთვის, თანამშრომლობისა და კოდის სარეზერვო შენახვისთვის.

|                     | Git                        | GitHub                  |
| :------------------ | :------------------------- | :---------------------- |
| **რა არის**         | პროგრამა/ინსტრუმენტი       | ვებ-სერვისი/პლატფორმა   |
| **სად მუშაობს**     | ლოკალურად (შენს მანქანაზე) | ღრუბელში (remote)       |
| **მთავარი ფუნქცია** | ვერსიების კონტროლი         | Hosting + თანამშრომლობა |

---

## 💻 4. Git-ის საბაზო ბრძანებები

```bash
# რეპოზიტორიის ინიციალიზაცია მიმდინარე საქაღალდეში
git init

# მიმდინარე სტატუსის შემოწმება (staged/unstaged ცვლილებები)
git status

# ყველა ცვლილების staging area-ში დამატება
git add .

# დამატებული ცვლილებების commit-ი შეტყობინებით
git commit -m "Initial commit"
```

---

## ☁️ 5. პროექტის ატვირთვა GitHub-ზე

```bash
# ლოკალური branch-ის სახელის დარქმევა/გადარქმევა main-ზე
git branch -M main

# Remote repository-ის დაკავშირება (GitHub-ზე შექმნილი ცარიელი repo)
git remote add origin https://github.com/username/repo-name.git

# ლოკალური commit-ების ატვირთვა GitHub-ზე
git push -u origin main
```

**GitHub-ზე ცარიელი repo-ს შექმნის ნაბიჯები:**

1. GitHub → **New Repository**
2. Repository name-ის მითითება
3. **არ** მონიშნო "Add README" (რადგან ლოკალურად უკვე გვაქვს კოდი)
4. **Create repository**

---

## 🌍 6. GitHub Pages — Deploy & Live Link

GitHub Pages საშუალებას გაძლევს, static საიტი (HTML/CSS/JS) უფასოდ დააჰოსტო პირდაპირ GitHub რეპოზიტორიიდან.

### ნაბიჯები:

1. Repository → **Settings** → **Pages**
2. **Branch** სექციაში აირჩიე `main` და საქაღალდე `/root`
3. დააჭირე **Save**
4. რამდენიმე წუთში მიიღებ Live ბმულს ფორმატით:
   `https://username.github.io/repo-name/`

> ⚠️ **მნიშვნელოვანი:** `index.html` ფაილი უნდა იყოს repository-ის root საქაღალდეში, რომ GitHub Pages-მა სწორად ამოიცნოს entry point.

---

## 📌 შემაჯამებელი Workflow

```
1. HTML დაწერე სუფთა სტანდარტით
2. W3C Validator-ში შეამოწმე
3. git init → git add . → git commit -m "..."
4. git branch -M main
5. git remote add origin <url>
6. git push -u origin main
7. GitHub Pages-ზე ჩართე Deploy
8. გააზიარე Live ბმული
```
