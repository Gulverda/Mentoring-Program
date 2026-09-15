# 🛠️ Web Development / Lecture 15: Modern Tooling — SCSS, CSS Variables, ES6 Modules, NPM & Vite

## 📌 ლექციის მიმოხილვა

დღევანდელ გაერთიანებულ ლექციაზე გავდივართ ყველაფერს, რაც სტილებისა და JS არქიტექტურის თანამედროვე სტანდარტად ითვლება:

- **CSS Custom Properties (`var()`):** დინამიური CSS ცვლადები და JS-იდან მათი მართვა.
- **SCSS / Sass:** Nesting, Variables, Mixins, Partials (`_filename.scss`) და `@use`.
- **ES6 Modules (`import`/`export`):** JS კოდის მოდულური დაყოფა.
- **JQuery/Bootstrap:** JQuery, Bootstrap 5-ის SCSS-ით მორგება (Customization) და build tools.

---

## 🧭 თემების სია

1. CSS Custom Properties — გამოცხადება და გამოყენება
2. CSS Custom Properties — დინამიური მართვა JS-იდან
3. SCSS — Nesting
4. SCSS Variables vs CSS Custom Properties
5. SCSS — Mixins
6. SCSS — Partials & `@use`
7. ES6 Modules — საფუძვლები (`import`/`export`)
8. JQuery
9. Bootstrap 5-ის SCSS Customization
10. Live Coding პროექტი — Vite + SCSS Dynamic Theme Dashboard
11. საშინაო დავალება

---

# Part 1: CSS Custom Properties (`--variable`)

CSS-ის Native ცვლადები დინამიურია — მათზე წვდომა აქვს როგორც CSS-ის Cascade/Inheritance მექანიზმს, ასევე უშუალოდ JavaScript-ს!

## 1️⃣ ცვლადების გამოცხადება და გამოყენება

```css
/* :root ნიშნავს გლობალურ ხელმისაწვდომობას (მთელი HTML დოკუმენტისთვის) */
:root {
  --primary-color: #6366f1;
  --bg-color: #ffffff;
  --text-color: #0f172a;
  --padding-std: 16px;
}

body {
  background-color: var(--bg-color);
  color: var(--text-color);
  padding: var(--padding-std);
}

.btn-primary {
  background-color: var(--primary-color);
}
```

> 💡 **Fallback მნიშვნელობა:** `var()`-ს შეუძლია მეორე არგუმენტიც მიიღოს — მნიშვნელობა, რომელიც გამოიყენება, თუ ცვლადი განუსაზღვრელია: `var(--accent-color, #333)`.

## 2️⃣ JS-იდან CSS ცვლადის დინამიური შეცვლა (მაგ: Theme Switcher)

```javascript
// JS-ით CSS ცვლადის მნიშვნელობის შეცვლა runtime-ში
function setDarkMode() {
  document.documentElement.style.setProperty("--bg-color", "#0f172a");
  document.documentElement.style.setProperty("--text-color", "#ffffff");
}
```

> 💡 **`document.documentElement`** არის `<html>` ტეგი — CSS ცვლადის იქ შეცვლა მთელ დოკუმენტზე მოქმედებს, `:root`-ის Cascade-ის წყალობით.

---

# Part 2: SCSS (Sass — Syntactically Awesome Style Sheets)

SCSS არის CSS-ის **Preprocessor** — ენა, რომელიც კომპილაციისას ჩვეულებრივ CSS-ად გარდაიქმნება, მაგრამ თავად წერის პროცესში გვაძლევს ბევრად უფრო სუფთა, სტრუქტურირებული და ორგანიზებული სინტაქსის საშუალებას.

## 3️⃣ Nesting (ჩაბუდებული სტილები)

მშობელი და შვილობილი სტილების იერარქიული ჩაწერა, ხელით სელექტორების დუბლირების გარეშე.

```scss
.navbar {
  background-color: #1e293b;
  padding: 1rem;

  .nav-list {
    list-style: none;
    display: flex;

    .nav-item {
      margin-right: 15px;

      a {
        color: white;
        text-decoration: none;

        &:hover {
          // & უდრის მშობელ სელექტორს (a:hover)
          color: #6366f1;
        }
      }
    }
  }
}
```

> ⚠️ **გაფრთხილება:** ჩაბუდების ღრმად წაღება (4-5+ დონე) ბადებს ზედმეტად სპეციფიკურ, რთულად საკონტროლებელ CSS Selector-ებს კომპილაციის შემდეგ. საერთო წესი — **არაუმეტეს 3 დონისა**.

## 4️⃣ SCSS Variables (`$`) vs CSS Custom Properties (`var()`)

| თვისება               | SCSS Variables (`$color`)                                    | CSS Custom Properties (`var(--color)`) |
| :-------------------- | :----------------------------------------------------------- | :------------------------------------- |
| **სად კომპილირდება?** | Build-ის დროს (კომპილაციისას ქრება და ხდება ჩვეულებრივი CSS) | ბრაუზერში (DOM-ში ცოცხლად არსებობს)    |
| **JS-ით მართვა?**     | ❌ შეუძლებელია                                               | ✅ `style.setProperty()`-ით მარტივად   |
| **გამოყენება**        | SCSS-ის შიდა ლოგიკაში, Mixin-ებში, კომპილაციის დონეზე        | Theme Switcher, დინამიური UI სტილები   |

```scss
$primary: #4f46e5;
$radius: 8px;

.card {
  border-radius: $radius;
  background: $primary;
}
```

> 💡 **პრაქტიკული წესი:** თუ მნიშვნელობა **არასდროს** იცვლება runtime-ში (მაგ. spacing scale, breakpoint-ები) — გამოიყენეთ SCSS Variable. თუ მნიშვნელობა **უნდა** შეიცვალოს JS-ით (თემა, მომხმარებლის პრეფერენციები) — გამოიყენეთ CSS Custom Property. ორივეს ერთდროულად, ერთმანეთის გვერდით გამოყენებაც სავსებით ნორმალურია.

## 5️⃣ Mixins (`@mixin` & `@include`) — მრავალჯერადი CSS ბლოკები

როდესაც სტილების ჯგუფი (მაგ. Flexbox Centering ან Media Queries) ხშირად მეორდება:

```scss
// 1. Mixin-ის განსაზღვრა
@mixin flex-center($direction: row) {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: $direction;
}

// 2. Mixin-ის გამოყენება
.hero-section {
  @include flex-center(column);
  height: 100vh;
}
```

> 💡 Mixin-ს შეუძლია პარამეტრებიც მიიღოს (`$direction`-ივით), default მნიშვნელობით — ზუსტად ისე, როგორც JS ფუნქციებში.

## 6️⃣ Partials (`_filename.scss`) & თანამედროვე `@use`

კოდის დაყოფა მცირე ფაილებად (არქიტექტურა) — ფაილის სახელის წინ `_` (Underscore) ნიშნავს, რომ ეს არის **Partial**, დამოუკიდებლად არასდროს კომპილირდება, მხოლოდ სხვაგან "შემოტანისთვისაა":

```
styles/
├── _variables.scss
├── _mixins.scss
├── _buttons.scss
└── main.scss
```

```scss
// _variables.scss
$brand-color: #ff3e3e;

// main.scss
@use "variables" as *;

.badge {
  background-color: $brand-color;
}
```

> 💡 **`@use` vs ძველი `@import`:** თანამედროვე Sass-ში `@use` ანაცვლებს ძველ `@import`-ს — `@use` ცვლადებს/მიქსინებს **namespace**-ავს (`as *` შლის ამ namespace-ს, პირდაპირ წვდომისთვის), თავიდან აცილებს სახელების კონფლიქტს, და თითოეულ ფაილს მხოლოდ **ერთხელ** ტვირთავს, მიუხედავად რამდენჯერაც არის "შემოტანილი" სხვადასხვა ფაილში.

---

# Part 3: ES6 Modules, NPM, Bootstrap & Vite Integration

## 7️⃣ ES6 Modules — საფუძვლები (`import` / `export`)

სანამ Vite-ის ეკოსისტემაში გადავალთ, გავიხსენოთ თავად `import`/`export`-ის სინტაქსი — ეს არის ის მოდულური სისტემა, რომელზეც მთელი თანამედროვე JS Tooling დგას.

```javascript
// math.js — Named Exports (შეიძლება რამდენიმე, ერთ ფაილში)
export function add(a, b) {
  return a + b;
}
export const PI = 3.14159;

// math.js — Default Export (მხოლოდ ერთი, თითო ფაილში)
export default function multiply(a, b) {
  return a * b;
}
```

```javascript
// main.js — იმპორტი
import multiply, { add, PI } from "./math.js"; // Default + Named ერთად

console.log(add(2, 3)); // 5
console.log(PI); // 3.14159
console.log(multiply(2, 3)); // 6

// ალტერნატიული სინტაქსი — ყველაფრის ერთად, namespace-ის სახით:
import * as MathUtils from "./math.js";
console.log(MathUtils.add(1, 1));
```

> 💡 **`export`-ის ორი ტიპი:** **Named Export** — შეგიძლიათ რამდენიც გინდათ, ერთ ფაილში, და იმპორტისას ზუსტი სახელი უნდა დაწეროთ ფიგურულ ფრჩხილებში (`{ add, PI }`). **Default Export** — მხოლოდ ერთი თითო ფაილში, და იმპორტისას **ნებისმიერ** სახელს შეგიძლიათ დაარქვათ (`import multiply` ან `import calc` — ორივე იმუშავებდა, რადგან default-ს სახელი "თან არ ახლავს").

> ⚠️ **მნიშვნელოვანი დეტალი:** სუფთა ბრაუზერში, `import`/`export` მუშაობს **მხოლოდ** მაშინ, თუ `<script>` ტეგს აქვს `type="module"` ატრიბუტი (`<script type="module" src="main.js">`). Vite-ისა და მსგავსი Build Tool-ების გამოყენებისას, ეს კონფიგურაცია ავტომატურად, "ხედვის მიღმა" ხდება.

---

## 8️⃣ NPM & Vite — გარემოს აწყობა

**Vite**-ს აქვს Native SCSS-ის მხარდაჭერა! ტერმინალში `sass`-ის დამატებით შეგვიძლია პირდაპირ SCSS-ში გადავწეროთ Bootstrap-ის ცვლადები.

### 🛠️ Step-by-Step Vite + SCSS Setup:

**1. პროექტისა და პაკეტების დაინსტალირება:**

```bash
npm create vite@latest my-app -- --template vanilla
cd my-app
npm install
npm install bootstrap
npm install -D sass
```

> 💡 **`-D` ფლაგის მნიშვნელობა:** `npm install -D sass` ინსტალაციისას, `sass` ემატება `devDependencies`-ში (არა `dependencies`) — ეს ნიშნავს, რომ ის საჭიროა მხოლოდ **დეველოპმენტის/build-ის** დროს, არა თავად საბოლოო, საიტზე გამოქვეყნებულ კოდში.

**2. `src/styles/main.scss` ფაილის შექმნა:**

```scss
// 1. გადავაწეროთ Bootstrap-ის შიდა ცვლადები ჩვენი SCSS ცვლადებით!
$primary: #6366f1;
$danger: #ef4444;
$body-bg: #f8fafc;

// 2. შემოვიტანოთ Bootstrap-ის SCSS
@import "bootstrap/scss/bootstrap";

// 3. ჩვენი Custom SCSS სტილები
.custom-card {
  @include border-radius(12px);
  box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1);
}
```

> 💡 **რატომ მუშაობს ცვლადების "გადაწერა"?** Sass ცვლადებს ფაილში ზემოდან ქვემოთ კითხულობს — თუ `$primary`-ს **გავაცხადებთ Bootstrap-ის იმპორტამდე**, Bootstrap-ის საკუთარი, შიდა SCSS ფაილები ამ ჩვენს მნიშვნელობას გამოიყენებენ საკუთარი default-ის ნაცვლად. ეს არის მთელი Bootstrap Customization-ის საიდუმლო.

**3. `src/main.js`-ში SCSS-ის იმპორტი:**

```javascript
import "./styles/main.scss";
import { Modal } from "bootstrap";

console.log("Vite + SCSS + Bootstrap 5 Is Running!");
```

> 💡 ეს არის ES6 Modules-ის რეალური, პრაქტიკული გამოყენება — SCSS ფაილის "იმპორტიც" კი შესაძლებელია, რადგან Vite ავტომატურად ამუშავებს (compile-ავს) მას build-ის დროს.

---

## 🧪 9. Live Coding: Vite + SCSS Dynamic Theme Dashboard

ლექციაზე ავაწყობთ მცირე Dashboard UI-ს, სადაც:

- **SCSS Nesting & Mixins** გამოიყენება კომპონენტების სტილიზაციისთვის.
- **CSS Custom Properties (`var()`)** გამოიყენება Dark/Light mode-ის გადასართავად JS-იდან.
- **ES6 Import/Export** ყოფს JS ლოგიკას დოკუმენტებად (`theme.js`, `main.js`).

```javascript
// theme.js
export function toggleTheme() {
  const root = document.documentElement;
  const isDark = root.style.getPropertyValue("--bg-color") === "#0f172a";

  if (isDark) {
    root.style.setProperty("--bg-color", "#ffffff");
    root.style.setProperty("--text-color", "#0f172a");
  } else {
    root.style.setProperty("--bg-color", "#0f172a");
    root.style.setProperty("--text-color", "#ffffff");
  }
}
```

```javascript
// main.js
import "./styles/main.scss";
import { toggleTheme } from "./theme.js";

document
  .querySelector("#theme-toggle-btn")
  .addEventListener("click", toggleTheme);
```

---

## 📌 10. Quick Cheatsheet

| სინტაქსი                                | განმარტება                           |
| :-------------------------------------- | :----------------------------------- |
| `:root { --x: value; }`                 | გლობალური CSS ცვლადის გამოცხადება    |
| `var(--x, fallback)`                    | ცვლადის გამოყენება, fallback-ითურთ   |
| `style.setProperty('--x', v)`           | CSS ცვლადის შეცვლა JS-იდან           |
| `$x: value;`                            | SCSS ცვლადი (კომპილაციის დროს ქრება) |
| `&:hover`                               | SCSS-ში მშობელი სელექტორის მითითება  |
| `@mixin name() { }` / `@include name()` | SCSS-ის მრავალჯერადი ბლოკი           |
| `_file.scss` + `@use 'file'`            | SCSS Partial-ის შემოტანა             |
| `export` / `export default`             | Named vs Default გატანა მოდულიდან    |
| `import { x } from './f.js'`            | Named იმპორტი                        |
| `import x from './f.js'`                | Default იმპორტი                      |
| `npm install -D package`                | Dev-dependency-ის ინსტალაცია         |

---

## 🏠 11. საშინაო დავალება (Homework)

### დავალება 1 (SCSS Component Architecture):

შექმენით პროექტი `sass`(scss) მხარდაჭერით. შექმენით `_variables.scss`, `_mixins.scss` და `_card.scss` ფაილები. `@use`-ით შემოიტანეთ `main.scss`-ში და ააგეთ 3 პროდუქტის ბარათი SCSS Nesting-ისა და Mixin-ის გამოყენებით.

### დავალება 2 (Dynamic CSS Variables Theme Switcher):

`:root`-ში განსაზღვრეთ CSS ცვლადები (`--bg`, `--text`, `--accent`). JS-ით შექმენით Toggle Button, რომელიც `document.documentElement.style.setProperty()`-ით შეცვლის ამ ცვლადების მნიშვნელობებს Dark/Light რეჟიმებს შორის და შეინახავს არჩევანს `localStorage`-ში.

### დავალება 3 (JQuery/Bootsrap):

შექმენი დავალება `JQuery`-სა და `Bootstrap`-ის გამოყენებით.
