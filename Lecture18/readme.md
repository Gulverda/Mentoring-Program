# 🎨 Web Development / Lecture 18: Styling in React — SCSS Modules & Component Styling

## 📌 ლექციის მიმოხილვა

წინა ლექციაზე ვისწავლეთ React-ის კომპონენტური არქიტექტურა, Props და `useState` Hook. დღეს განვიხილავთ, როგორ გავუკეთოთ სტილიზაცია React-ის კომპონენტებს პროფესიონალურ დონეზე:

- **Global CSS-ის პრობლემა:** რატომ იწვევს ჩვეულებრივი CSS/SCSS ფაილები სტილების გადაფარვას (Naming Collisions).
- **CSS / SCSS Modules (`.module.scss`):** სტილების იზოლაცია (Scoped CSS) და ავტომატური უნიკალური კლასები.
- **Dynamic Classes:** დინამიური კლასების მართვა Template Literals-ითა და `clsx` / `classnames` ბიბლიოთეკებით.
- **Styling Approaches-ის შედარება:** Inline Styles vs CSS Modules vs Tailwind CSS.

---

# Part 1: Global CSS-ის პრობლემა React-ში

Vanilla JS-იდან გამომდინარე, ხშირად კომპონენტთან ვქმნით `.css` ან `.scss` ფაილს და ვაკეთებთ იმპორტს:

```jsx
// ❌ პრობლემური მიდგომა
import "./Button.css";

export function Button() {
  return <button className="btn">Click me</button>;
}
```

## ⚠️ რა არის Naming Collision?

React-ში `import "./Button.css"` არ საზღვრავს სტილებს მხოლოდ იმ 1 კომპონენტისთვის. Build-ის დროს ყველა შემოტანილი CSS ფაილი გაერთიანდება ერთ გლობალურ CSS ფაილად!

თუ სხვა დეველოპერი `Card.css`-ში დაწერს `.btn { background: red; }`, ის გადაფარავს `Button.css`-ის `.btn` კლასს მთელ აპლიკაციაში.

```
[ Button.css ] ➔ .btn { background: blue; }  \
                                              ➔  გლობალური CSS: ბოლო იმპორტი იგებს!
[ Card.css ]   ➔ .btn { background: red; }   /  (.btn ხდება RED ყველგან)
```

---

# Part 2: CSS / SCSS Modules (`Component.module.scss`)

CSS/SCSS Modules არის გამოსავალი, რომელიც თითოეულ კომპონენტს აძლევს სრულიად იზოლირებულ (Scoped) სტილებს.

## 🛠️ როგორ მუშაობს Modules?

- ფაილის დასახელება უნდა სრულდებოდეს `.module.css` ან `.module.scss`-ით (მაგ. `Button.module.scss`).
- JS-ში სტილებს შემოვიტანთ როგორც JavaScript ობიექტს (`import styles from ...`).
- Build-ის დროს Vite/Webpack კლასების სახელებს ამატებს უნიკალურ ჰეშს (მაგ. `Button_btn__a8X1z`).

```scss
/* 📁 src/components/Button/Button.module.scss */
.btn {
  padding: 10px 20px;
  border-radius: 8px;
  border: none;
  cursor: pointer;
  background-color: #6366f1;
  color: white;

  &:hover {
    background-color: #4f46e5;
  }
}
```

```jsx
// 📁 src/components/Button/Button.jsx
import styles from "./Button.module.scss";

export function Button() {
  // styles ობიექტიდან იღებს უნიკალურ კლასს
  return <button className={styles.btn}>Click me</button>;
}
```

> 💡 **შედეგი HTML-ში:** `<button class="Button_btn__a8X1z">Click me</button>` — სხვა კომპონენტის `.btn` კლასი ამ ღილაკზე არასოდეს იმოქმედებს!

---

# Part 3: Dynamic Classes (დინამიური კლასები)

ხშირად კომპონენტის სტილი დამოკიდებულია Props-ზე ან State-ზე (მაგ. `variant="primary" | "danger"`, `isDisabled`, `isActive`).

## 1️⃣ Template Literals-ის გამოყენება

```jsx
import styles from "./Button.module.scss";

export function Button({ variant, fullWidth }) {
  // დინამიური კლასების აწყობა
  const className = `${styles.btn} ${variant === "danger" ? styles.danger : styles.primary} ${fullWidth ? styles.fullWidth : ""}`;

  return <button className={className}>Button</button>;
}
```

## 2️⃣ `clsx` / `classnames` ბიბლიოთეკის გამოყენება (Best Practice)

Template Literals რთულდება, როცა ბევრი პირობა გვექნება. ამისთვის გამოიყენება მცირე ზომის Utility ბიბლიოთეკა `clsx` (ან `classnames`).

```bash
npm install clsx
```

```jsx
import clsx from "clsx";
import styles from "./Button.module.scss";

export function Button({ variant = "primary", disabled, fullWidth, children }) {
  return (
    <button
      className={clsx(styles.btn, {
        [styles.primary]: variant === "primary",
        [styles.danger]: variant === "danger",
        [styles.disabled]: disabled,
        [styles.fullWidth]: fullWidth,
      })}
    >
      {children}
    </button>
  );
}
```

---

# Part 4: Inline Styles vs SCSS Modules vs Tailwind CSS

| მიდგომა                                          | დადებითი მხარე (Pros)                                                         | უარყოფითი მხარე (Cons)                                                       | როდის ვიყენებთ?                                              |
| :----------------------------------------------- | :---------------------------------------------------------------------------- | :--------------------------------------------------------------------------- | :----------------------------------------------------------- |
| **Inline Styles** `style={{ color: 'red' }}`     | 100% იზოლირებულია, მარტივია დინამიური მნიშვნელობებისთვის (მაგ. `top: ${y}px`) | ❌ არ აქვს `:hover`, `:focus`, `@media` მხარდაჭერა. ნელია performance-ისთვის | მხოლოდ დინამიური, რიცხვითი გამოთვლებისას (x/y პოზიციონირება) |
| **SCSS Modules** `Button.module.scss`            | ✅ იზოლირებულია, აქვს SCSS-ის ყველა შესაძლებლობა (Nesting, Mixins, Variables) | საჭიროებს ცალკე `.scss` ფაილების შექმნას                                     | საშუალო და დიდ პროექტებში, კომპონენტური არქიტექტურისთვის     |
| **Tailwind CSS** `className="p-4 bg-indigo-500"` | ✅ ძალიან სწრაფი დეველოპმენტი, ფაილების გადართვა არ გიწევს                    | HTML/JSX ხდება ვიზუალურად გადატვირთული                                       | სწრაფი პროტოტიპირებისა და თანამედროვე React პროექტებისთვის   |

---

## 🧪 Live Coding / პრაქტიკული მაგალითები

### Reusable Card Component SCSS Modules-ით

```scss
/* 📁 src/components/Card/Card.module.scss */
@use "../../styles/variables" as *; // SCSS Variables-ის შემოტანა

.card {
  border-radius: 12px;
  padding: 20px;
  background-color: #ffffff;
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1);
  transition:
    transform 0.2s ease,
    box-shadow 0.2s ease;

  &:hover {
    transform: translateY(-4px);
    box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1);
  }

  &.dark {
    background-color: #1e293b;
    color: #ffffff;
  }
}

.title {
  font-size: 1.25rem;
  font-weight: 600;
  margin-bottom: 8px;
}

.body {
  font-size: 0.95rem;
  color: #64748b;

  .dark & {
    color: #94a3b8;
  }
}
```

```jsx
// 📁 src/components/Card/Card.jsx
import clsx from "clsx";
import styles from "./Card.module.scss";

export function Card({ title, description, isDark = false, children }) {
  return (
    <div className={clsx(styles.card, { [styles.dark]: isDark })}>
      {title && <h3 className={styles.title}>{title}</h3>}
      {description && <p className={styles.body}>{description}</p>}
      {children}
    </div>
  );
}
```

```jsx
// 📁 src/App.jsx
import { Card } from "./components/Card/Card";
import { Button } from "./components/Button/Button";

export default function App() {
  return (
    <div style={{ display: "flex", gap: "20px", padding: "40px" }}>
      <Card title="Standard Card" description="ეს არის ჩვეულებრივი ბარათი">
        <Button variant="primary">Learn More</Button>
      </Card>

      <Card
        title="Dark Theme Card"
        description="ეს არის მუქი თემის ბარათი"
        isDark={true}
      >
        <Button variant="danger">Delete</Button>
      </Card>
    </div>
  );
}
```

---

## 📌 Quick Cheatsheet

| სინტაქსი                                                      | განმარტება                                    |
| :------------------------------------------------------------ | :-------------------------------------------- |
| `import "./Button.css"`                                       | ❌ Global CSS — Naming Collision-ის რისკი     |
| `Component.module.scss`                                       | ✅ SCSS Module — ავტომატურად Scoped           |
| `import styles from "./X.module.scss"`                        | Modules-ის JS ობიექტად შემოტანა               |
| `className={styles.btn}`                                      | Scoped კლასის გამოყენება                      |
| `` `${styles.a} ${cond ? styles.b : ""}` ``                   | Dynamic Classes — Template Literal-ით         |
| `clsx(styles.btn, { [styles.danger]: variant === "danger" })` | Dynamic Classes — `clsx`-ით (Best Practice)   |
| `style={{ color: 'red' }}`                                    | Inline Style — მხოლოდ დინამიური რიცხვებისთვის |

---

## 🏠 საშინაო დავალება (Homework)

### დავალება 1 (UI Badge Component with Modules):

შექმენით `Badge.module.scss` და `Badge.jsx` კომპონენტი. ბეჯს ჰქონდეს `status` Prop (`"success"`, `"warning"`, `"error"`). შესაბამის სტატუსზე SCSS Modules-ით შეეცვალოს ფონი და ტექსტის ფერი.

### დავალება 2 (Interactive Product Card):

შექმენით `ProductCard.jsx` და `ProductCard.module.scss`. ბარათს ჰქონდეს პროდუქტის სურათი, სათაური, ფასი და "Add to Cart" ღილაკი. `useState`-ით დაამატეთ `isLiked` State (გულის აიკონის გადასართავად). გამოიყენეთ SCSS Modules და `clsx` დინამიური სტილებისთვის.
