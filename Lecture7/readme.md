# 📱 Web Development / Lecture 7: CSS Grid Fundamentals & Responsive Web Design (RWD)

მოგესალმებით **მე-7 ლექციაში**! ამ შეხვედრაზე ჩვენ ვისწავლით ორგანზომილებიან (2D) Layout ინსტრუმენტს — **CSS Grid**-ს და შევიძენთ უნარს, გავხადოთ ჩვენი ვებ-გვერდები **Responsive** (ადაპტური) ნებისმიერი ზომის ეკრანისთვის (მობილური, ტაბლეტი, დესკტოპი) **Media Queries**-ისა და **Mobile-First** მიდგომის გამოყენებით.

---

## 🧭 1. ლექციის მიმოხილვა (Lecture Overview)

1. **CSS Grid-ის არქიტექტურა:** Grid Container vs Grid Items, Rows & Columns.
2. **Grid-ის ძირითადი თვისებები:** `grid-template-columns`, `grid-template-rows`, `fr` ერთეული, `repeat()`, `gap`.
3. **Advanced Grid Layouts:** `auto-fit`, `auto-fill`, `minmax()` და `grid-template-areas`.
4. **Responsive Web Design (RWD):** რა არის RWD და რატომ არის ის აუცილებელი?
5. **Media Queries (`@media`):** Breakpoints, Viewport Dimensions.
6. **Mobile-First Strategy:** რატომ ვიწყებთ დიზაინს მობილურის ეკრანით.
7. **CSS Animations (`@keyframes`):** `transition`-ისგან განსხვავება, ანიმაციის სცენარის წერა და Performance-ული best practice-ები.

---

## 🏁 2. Flexbox vs CSS Grid (როდის რომელი?)

| კრიტერიუმი      | Flexbox                                           | CSS Grid                                                    |
| :-------------- | :------------------------------------------------ | :---------------------------------------------------------- |
| **განზომილება** | **1D** (ერთგანზომილებიანი: ან სვეტი, ან სტრიქონი) | **2D** (ორგანზომილებიანი: სვეტები და სტრიქონები ერთდროულად) |
| **ფოკუსი**      | კონტენტზე დაფუძნებული (Content-First)             | ბადეზე/სტრუქტურაზე დაფუძნებული (Layout-First)               |
| **გამოყენება**  | Navbar, ღილაკების ჯგუფი, Card-ის შიგთავსი         | მთლიანი გვერდის Layout, Card Gallery, Dashboard             |

---

## 📐 3. CSS Grid-ის საფუძვლები

Grid-ის ჩასართავად მშობელ კონტეინერს ვუწერთ `display: grid;`.

### 3.1 სვეტებისა და სტრიქონების განსაზღვრა

```css
.grid-container {
  display: grid;

  /* 3 სვეტის შექმნა: 200px, 1fr (თავისუფალი სივრცე), 2fr */
  grid-template-columns: 200px 1fr 2fr;

  /* repeat() ფუნქციის გამოყენება: 12-სვეტიანი ბადე */
  grid-template-columns: repeat(12, 1fr);

  /* მანძილი ელემენტებს შორის */
  gap: 20px; /* row-gap და column-gap */
}
```

### 3.2 ჭკვიანი ადაპტური ბადე (No Media Queries Grid)

`auto-fit`-ის, `minmax()`-ისა და `repeat()`-ის კომბინაციით შეგვიძლია შევქმნათ ბადე, რომელიც Media Query-ის გარეშეც ავტომატურად გადაეწყობა ეკრანის ზომის მიხედვით:

```css
.card-grid {
  display: grid;
  /* თითოეული ბარათი იქნება მინიმუმ 250px, მაქსიმუმ შეივსებს 1fr-ს */
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 20px;
}
```

### 3.3 Layout-ის აგება `grid-template-areas`-ით

ძალიან მოსახერხებელი გზა მთლიანი გვერდის სტრუქტურის ვიზუალურად ასაგებად:

```css
.page-layout {
  display: grid;
  grid-template-areas:
    "header  header"
    "sidebar main"
    "footer  footer";
  grid-template-columns: 250px 1fr;
}

.header {
  grid-area: header;
}
.sidebar {
  grid-area: sidebar;
}
.main {
  grid-area: main;
}
.footer {
  grid-area: footer;
}
```

---

## 📱 4. Responsive Web Design & Media Queries

Media Queries გვაძლევს საშუალებას, გავატაროთ კონკრეტული CSS წესები მხოლოდ მაშინ, როდესაც მოწყობილობის ეკრანი აკმაყოფილებს განსაზღვრულ პირობას (მაგ. სიგანეს).

### 4.1 Mobile-First მიდგომა

Mobile-First ნიშნავს, რომ ნაგულისხმევი CSS წესები დაწერილია მობილურისთვის, ხოლო Media Query-ით (`min-width`) ეტაპობრივად ვამატებთ სტილებს უფრო დიდი ეკრანებისთვის (ტაბლეტი, დესკტოპი).

```css
/* ==========================================
   1. BASE STYLES (Mobile First - Default)
   ========================================== */
body {
  font-size: 14px;
}

.container {
  display: flex;
  flex-direction: column; /* მობილურზე ელემენტები ერთმანეთის ქვეშაა */
}

/* ==========================================
   2. TABLET BREAKPOINT (>= 768px)
   ========================================== */
@media (min-width: 768px) {
  body {
    font-size: 16px;
  }

  .container {
    flex-direction: row; /* ტაბლეტზე გადადის გვერდიგვერდ */
  }
}

/* ==========================================
   3. DESKTOP BREAKPOINT (>= 1024px)
   ========================================== */
@media (min-width: 1024px) {
  .container {
    max-width: 1200px;
    margin: 0 auto;
  }
}
```

### 🛠️ სტანდარტული Breakpoint-ები:

- **მობილური:** < 768px
- **ტაბლეტი:** 768px – 1023px
- **ლეპტოპი/დესკტოპი:** >= 1024px
- **დიდი ეკრანები:** >= 1440px

> ⚠️ **შეხსენება:** არ დაგავიწყდეთ HTML-ის `<head>`-ში viewport-ის მითითება (ეს პირველ ლექციაშივე ავხსენით):
>
> ```html
> <meta name="viewport" content="width=device-width, initial-scale=1.0" />
> ```

---

## 📌 5. Quick Cheatsheet

| CSS თვისება / სინტაქსი              | განმარტება                                     |
| :---------------------------------- | :--------------------------------------------- |
| `display: grid`                     | რთავს Grid კონტექსტს                           |
| `grid-template-columns: 1fr 2fr`    | ქმნის 2 სვეტს შეფარდებით 1:2                   |
| `gap: 1.5rem`                       | მანძილი Grid სვეტებსა და სტრიქონებს შორის      |
| `minmax(200px, 1fr)`                | ელემენტის ზომის ინტერვალი (მინიმუმ – მაქსიმუმ) |
| `@media (min-width: 768px) { ... }` | CSS წესები 768px და მეტი სიგანის ეკრანებისთვის |

---

## 🎬 6. CSS Animations — `@keyframes` & Transitions

CSS Animations და Transitions არის ინსტრუმენტები ინტერფეისის გასაცოცხლებლად და უკეთესი User Experience (UX)-ის შესაქმნელად.

### ⚡ 6.1 `transition` vs `@keyframes` (როდის რომელი?)

- **`transition`:** გამოიყენება **ორ მდგომარეობას შორის** მარტივი გადასვლისთვის (მაგ. Hover-ის დროს ღილაკის ფერის შეცვლა). მას სჭირდება **ტრიგერი** (მომხმარებლის მოქმედება, მაგ. `:hover`, `:focus`).
- **`@keyframes` (Animation):** გამოიყენება **მრავალეტაპიანი, რთული** ანიმაციებისთვის. მას **არ** სჭირდება მომხმარებლის მოქმედება (შეუძლია გვერდის ჩატვირთვისთანავე ჩაირთოს) და შეუძლია იტრიალოს უსასრულოდ (`infinite`).

### 🎬 6.2 რა არის `@keyframes`?

`@keyframes` არის CSS-ის წესი, სადაც ვწერთ ანიმაციის „სცენარს" — ვგანსაზღვრავთ, რა ვიზუალური ცვლილებები უნდა მოხდეს ანიმაციის სხვადასხვა ეტაპზე (0%-დან 100%-მდე).

ანიმაციის შექმნა შედგება 2 ნაბიჯისგან: **სცენარის დაწერა** (`@keyframes`) და **ანიმაციის მიბმა** ელემენტზე (`animation` თვისება).

**ა) მარტივი (ორკადრიანი) — `from` / `to`:**

```css
@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(-20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}
```

**ბ) მრავალკადრიანი — პროცენტებით (0% – 100%):**

```css
@keyframes pulseAndRotate {
  0% {
    transform: scale(1) rotate(0deg);
    background-color: #3498db;
  }
  50% {
    transform: scale(1.2) rotate(180deg);
    background-color: #e74c3c;
  }
  100% {
    transform: scale(1) rotate(360deg);
    background-color: #3498db;
  }
}
```

### 🛠️ 6.3 ანიმაციის გამოყენება ელემენტზე

```css
.box {
  width: 100px;
  height: 100px;
  background-color: #3498db;

  animation-name: pulseAndRotate; /* keyframes-ის სახელი */
  animation-duration: 2s; /* ხანგრძლივობა */
  animation-timing-function: ease; /* სიჩქარის მრუდი */
  animation-delay: 0.5s; /* დაყოვნება დაწყებამდე */
  animation-iteration-count: infinite; /* რამდენჯერ გამეორდეს (ან რიცხვი, მაგ: 3) */
  animation-direction: alternate; /* მიმართულება: normal | reverse | alternate */
  animation-fill-mode: forwards; /* რა მდგომარეობაში დარჩეს დასრულებისას */
}
```

### 🚀 Shorthand (მოკლე ჩანაწერი)

პრაქტიკაში ცალ-ცალკე იშვიათად იწერება — გამოიყენება ერთიანი მოკლე ჩანაწერი:

```css
.box {
  /* animation: <name> <duration> <timing-function> <delay> <iteration-count> <direction> <fill-mode>; */
  animation: pulseAndRotate 2s ease-in-out 0.5s infinite alternate;
}
```

### 🔑 6.4 თვისებები, რომლებიც სტუდენტებს ხშირად ერევათ

**`animation-fill-mode`** — განსაზღვრავს, რა სტილში დარჩეს ელემენტი ანიმაციის დასრულების შემდეგ (თუ `infinite` არ უწერია):

- `none` (Default): ანიმაციის დასრულებისთანავე უბრუნდება თავდაპირველ CSS სტილს.
- `forwards`: ინარჩუნებს ანიმაციის ბოლო კადრის (`100%`/`to`) სტილს.
- `backwards`: ანიმაციის დაწყებამდე (`delay`-ის დროს) იღებს პირველი კადრის (`0%`/`from`) სტილს.
- `both`: აერთიანებს `forwards`-სა და `backwards`-ს.

**`animation-direction`:**

- `normal`: 0% → 100%
- `reverse`: 100% → 0%
- `alternate`: 0% → 100% → 0% → 100% (პირველი წრე წინ, მეორე უკან — ძალიან გლუვია პულსაციებისთვის!)

### 🛠️ 6.5 რეალური მაგალითი: Loading Spinner

```html
<div class="spinner"></div>
```

```css
.spinner {
  width: 50px;
  height: 50px;
  border: 5px solid #f3f3f3;
  border-top: 5px solid #3498db;
  border-radius: 50%;

  /* 1 წამში ბრუნავს უსასრულოდ და წრფივი სიჩქარით */
  animation: spin 1s linear infinite;
}

@keyframes spin {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}
```

### 💡 6.6 Best Practice: შესრულების ოპტიმიზაცია (Performance)

სტუდენტებს აუცილებლად უთხარი:

- ანიმაციისთვის გამოიყენონ **მხოლოდ** `transform` (`translate`, `scale`, `rotate`) და `opacity`.
- მოერიდონ ისეთი თვისებების დაანიმირებას, როგორებიცაა `width`, `height`, `margin`, `padding`, `top`/`left` — რადგან ეს იწვევს ბრაუზერის მიერ გვერდის ხელახლა დახატვას (Reflow/Repaint) და ანიმაციას ხდის „ჭედვადს" (laggy).
- `transform` მუშაობს GPU-ზე (ვიდეობარათზე) და უზრუნველყოფს 60 FPS-ს — ძალიან გლუვია.

---

## 📚 7. დამატებითი რესურსები (Practice & Tools)

### 🎮 CSS Grid — სავარჯიშო თამაშები და ვიზუალური ინსტრუმენტები:

| რესურსი                                           | აღწერა                                                                                                                                                   |
| :------------------------------------------------ | :------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **[Grid Garden](https://cssgridgarden.com/)**     | 28-დონიანი თამაში — სტუდენტი რწყავს/ალპოხავს ბაღს Grid-ის property-ების სწორად გამოყენებით. Flexbox Froggy-ის შემქმნელის ანალოგიური თამაშია Grid-ისთვის. |
| **[Layoutit Grid](https://grid.layoutit.com/)**   | ვიზუალური Grid Builder — drag & drop-ით აწყობ Layout-ს (columns, rows, gap, areas) და ცოცხლად გამოაქვს HTML+CSS კოდი.                                    |
| **[Grid by Example](https://gridbyexample.com/)** | Rachel Andrew-ს (CSS Grid-ის ერთ-ერთი ავტორის) რეფერენსი — ვიდეოებით, პატერნებით და მაგალითებით.                                                         |

### 🎬 CSS Animations — ინსტრუმენტები:

| რესურსი                                           | აღწერა                                                                                                                                                                                                |
| :------------------------------------------------ | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **[Animista](https://animista.net/)**             | On-demand `@keyframes` გენერატორი — ირჩევ მზა ანიმაციას (fade, bounce, rotate და ა.შ.), ცვლი duration/delay/timing-ს, და პირდაპირ აკოპირებ მზა CSS-ს. კარგია Live Coding-ის დროს სწრაფი დემოებისთვის. |
| **[cubic-bezier.com](https://cubic-bezier.com/)** | ვიზუალური ინსტრუმენტი `animation-timing-function`-ის საკუთარი მრუდის ასაგებად — ცოცხლად ხედავ, როგორ იცვლება მოძრაობის „ხასიათი".                                                                     |

### 📖 ზოგადი რეფერენსები (Live Coding-ის დროს გვერდით გასაშლელად):

| რესურსი                                                                                                                | აღწერა                                                                                                                       |
| :--------------------------------------------------------------------------------------------------------------------- | :--------------------------------------------------------------------------------------------------------------------------- |
| **[W3Schools — CSS Grid](https://www.w3schools.com/css/css_grid.asp)**                                                 | მარტივი, სტრუქტურირებული განმარტება "Try it Yourself" რედაქტორით — კარგია დამწყები სტუდენტისთვის დამოუკიდებელი ვარჯიშისთვის. |
| **[W3Schools — CSS Animations](https://www.w3schools.com/css/css3_animations.asp)**                                    | იგივე ფორმატი, `@keyframes`-ზე — სწრაფი სინტაქსური მაგალითებით.                                                              |
| **[MDN — Using CSS animations](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_animations/Using_CSS_animations)** | ოფიციალური, ყველაზე ზუსტი დოკუმენტაცია ღრმად ჩასაწვდომად.                                                                    |

---

## 🏋️‍♂️ 8. პრაქტიკული დავალება (Homework)

🎯 **დავალების მიზანი:** CSS Grid-ისა და Media Queries-ის გამოყენებით სრულად ადაპტური (Responsive) Landing Page Layout-ის აგება.

### 📋 დავალების ეტაპები:

**1. Mobile-First CSS სტრუქტურა:**

- დაწერეთ ბაზისური CSS მობილური ეკრანისთვის (< 768px).
- მობილურზე ნავიგაცია და ბარათები განათავსეთ ვერტიკალურად (ერთმანეთის ქვეშ).

**2. Responsive Grid Gallery (Media Queries):**

- შექმენით პროდუქტების/სერვისების სექცია CSS Grid-ით.
- მობილურზე: 1 სვეტი (`1fr`).
- ტაბლეტზე (`min-width: 768px`): 2 სვეტი (`repeat(2, 1fr)`).
- დესკტოპზე (`min-width: 1024px`): 3 ან 4 სვეტი (`repeat(auto-fit, minmax(250px, 1fr))`).

**3. Responsive Header & Footer:**

- დესკტოპზე მენიუს ბმულები გაშალეთ ჰორიზონტალურად (`display: flex`).

**4. Git Workflow:**

- გააკეთეთ Commit მესიჯით: `feat: implement responsive grid gallery and media queries`.
- ატვირთეთ ცვლილებები GitHub-ზე (`git push`) და შეამოწმეთ GitHub Pages.

### 📤 ჩაბარების ინსტრუქცია:

გამოაგზავნეთ თქვენი GitHub Repository-სა და GitHub Pages-ის Live ბმულები.
