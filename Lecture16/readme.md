# ⚛️ Web Development / Lecture 22: შესავალი React.js-ში — SPA, JSX, Component Architecture & Vite

## 📌 ლექციის მიმოხილვა

აქამდე ჩვენ ვწერდით **Imperative** (ბრძანებით) JavaScript-ს: ხელით ვეძებდით DOM ელემენტებს (`document.querySelector`), ხელით ვაახლებდით ტექსტს და ვმართავდით HTML სტრუქტურას.

დღევანდელ ლექციაზე გადავდივართ **Declarative** (დეკლარაციულ) მიდგომაზე React-თან ერთად.

დღეს ვისწავლით:

- **რატომ React?** — რა პრობლემას ჭრის React და რას ნიშნავს Virtual DOM.
- **SPA (Single Page Application):** როგორ მუშაობს თანამედროვე ვებ-აპლიკაციები გადატვირთვის (Reload) გარეშე.
- **JSX (JavaScript XML):** HTML-ისა და JS-ის გაერთიანების სინტაქსი.
- **Component Architecture & Props:** UI-ს დაყოფა მრავალჯერ გამოყენებად, დამოუკიდებელ ნაწილებად.
- **Vite + React:** პროექტის გამართვა და გაშვება.

---

# Part 1: რატომ React და რა არის SPA?

## 1️⃣ Traditional Web vs. Single Page Application (SPA)

**Multi-Page Application (MPA / ტრადიციული საიტები):**
მომხმარებელი ყოველ კლიკზე (მაგ. გვერდის შეცვლისას) სერვერიდან ითხოვს სრულად ახალ HTML დოკუმენტს. ბრაუზერი მთლიანად ათანაბრებს/არეფრეშებს გვერდს (თეთრი ციმციმი).

**Single Page Application (SPA / React-ის მიდგომა):**
ბრაუზერი იტვირთება მხოლოდ 1 თითო HTML ფაილს (`index.html`). გვერდზე შინაარსის შეცვლისას, React დინამიურად ცვლის DOM-ის კონკრეტულ ნაწილებს, სერვერიდან მთლიანი HTML-ის ხელახლა წამოღების გარეშე.

```
                  [ SPA არქიტექტურა ]
                     index.html
                         |
                   <div id="root">
                         |
             +-----------+-----------+
             |                       |
       Header Component       Main Content (დინამიური)
```

## 2️⃣ Imperative vs. Declarative: Real-world მაგალითი

**Imperative (Vanilla JS)** — შენ ეუბნები ბრაუზერს, **როგორ** გააკეთოს ყოველი ნაბიჯი:

```javascript
const btn = document.createElement("button");
btn.innerText = "Click me";
btn.classList.add("active");
document.body.appendChild(btn);
```

**Declarative (React)** — შენ ეუბნები React-ს, **რა** შედეგი გინდა მიიღო (State-ის მიხედვით), React კი თავად ზრუნავს DOM-ის ეფექტურ განახლებაზე:

```jsx
<button className="active">Click me</button>
```

> 💡 **კავშირი წინა ლექციებთან:** გახსოვთ Lecture 15-16-ის Todo List, სადაც ყოველ ცვლილებაზე ხელით ვწერდით `renderTasks()`-ს, DOM-ის მთლიანად ხელახლა დასახატად? React ზუსტად ამ "ხელით ხატვის" პასუხისმგებლობას იღებს საკუთარ თავზე.

## 3️⃣ Virtual DOM — როგორ მუშაობს React-ის „სწრაფი ხატვა“?

რეალურ DOM-ში ელემენტების ხშირი ცვლილება (Reflow & Repaint) ძალიან ნელია.

1. React-ს მეხსიერებაში (RAM) აქვს რეალური DOM-ის მსუბუქი კოპია — **Virtual DOM**.
2. როდესაც მონაცემი იცვლება, იქმნება ახალი Virtual DOM ხე.
3. React ადარებს ძველ და ახალ Virtual DOM-ს (**Reconciliation / Diffing Algorithm**).
4. რეალურ DOM-ში იცვლება **მხოლოდ** ის კონკრეტული ელემენტი, რომელიც რეალურად შეიცვალა (**Batch Update**).

---

# Part 2: პროექტის გაშვება Vite-ით

დღეს React პროექტების შესაქმნელად გამოიყენება **Vite** (Create React App უკვე მოძველებულია).

## 🛠️ Step-by-Step ტერმინალში:

```bash
# 1. React პროექტის შექმნა Vite-ით
npm create vite@latest my-react-app -- --template react

# 2. პროექტის საქაღალდეში შესვლა
cd my-react-app

# 3. დამოკიდებულებების დაინსტალირება
npm install

# 4. Dev Server-ის გაშვება
npm run dev
```

### 📁 პროექტის სტრუქტურა:

- **`index.html`** — ერთადერთი HTML ფაილი, სადაც წერია `<div id="root"></div>`.
- **`src/main.jsx`** — React-ის შესავალი წერტილი, რომელიც `App.jsx`-ს აბამს `#root`-ზე.
- **`src/App.jsx`** — ჩვენი მთავარი კომპონენტი.

---

# Part 3: JSX (JavaScript XML)

JSX არის JavaScript-ის სინტაქსური გაფართოება, რომელიც საშუალებას გვაძლევს JavaScript კოდში პირდაპირ დავწეროთ HTML-ის მსგავსი სტრუქტურა.

## ⚠️ JSX-ის 4 ოქროს წესი:

**1. მხოლოდ 1 Parent Element / Fragment (`<>...</>`):**

```jsx
// ❌ შეცდომა
return (
  <h1>Title</h1>
  <p>Text</p>
);

// ✅ სწორია (React Fragment)
return (
  <>
    <h1>Title</h1>
    <p>Text</p>
  </>
);
```

**2. JavaScript-ის გამოსახულებები ფიგურულ ფრჩხილებში `{ }`:**

```jsx
const userName = "Luka";
const age = 23;

return (
  <h1>
    Hello, {userName}! Next year you will be {age + 1}.
  </h1>
);
```

**3. HTML ატრიბუტები CamelCase-ში:**

- `class` ➔ `className`
- `for` ➔ `htmlFor`
- `onclick` ➔ `onClick`

**4. ყველა ტეგი აუცილებლად უნდა დაიხუროს:**

```jsx
<img src="logo.png" alt="Logo" />
<input type="text" />
```

> ⚠️ **ხშირი შეცდომა:** ჩვეულებრივ HTML-ში `<img>` და `<input>` თვითდახურვას არ საჭიროებენ — JSX-ში კი **აუცილებელია** `/>`. ეს იმიტომ ხდება, რომ JSX, სინამდვილეში, JavaScript ობიექტებად "გარდაიქმნება" (`React.createElement(...)`) — და ეს გარდაქმნა მკაცრ, XML-ის msგავს სინტაქსს მოითხოვს.

---

# Part 4: Component Architecture & Props

კომპონენტი (Component) არის დამოუკიდებელი, მრავალჯერ გამოყენებადი UI ბლოკი. React-ში კომპონენტი არის ჩვეულებრივი JS ფუნქცია, რომელიც აბრუნებს JSX-ს.

> 💡 **წესი:** კომპონენტის სახელი აუცილებლად **დიდი ასოთი** (PascalCase) უნდა იწყებოდეს! (`UserCard.jsx`, `Button.jsx`).

## 1️⃣ პირველი კომპონენტის შექმნა

```jsx
// 📁 src/components/Greeting.jsx
export function Greeting() {
  return <h2>მოგესალმებით React-ის კურსზე! 🔥</h2>;
}
```

```jsx
// 📁 src/App.jsx
import { Greeting } from "./components/Greeting";

export default function App() {
  return (
    <div>
      <h1>მთავარი აპლიკაცია</h1>
      <Greeting />
    </div>
  );
}
```

> ⚠️ **ხშირი შეცდომა:** `Greeting`-ის დაბალი ასოთი დაწერა (`greeting`) — React ამას აღიქვამს, როგორც ჩვეულებრივ HTML ტეგს (`<greeting>`), არა კომპონენტს, და "დაარენდერებს" უცნაურ, არავალიდურ HTML ელემენტს, კომპონენტის ნაცვლად.

## 2️⃣ Props (Properties) — მონაცემების გადაცემა მშობლიდან შვილ კომპონენტზე

Props არის არგუმენტები, რომლებიც გადაეცემა კომპონენტს (როგორც ფუნქციის პარამეტრები). Props არის **Read-Only** — შვილ კომპონენტს არ შეუძლია მისი პირდაპირ შეცვლა.

```jsx
// 📁 src/components/UserCard.jsx
export function UserCard({ name, role, isOnline }) {
  return (
    <div className="user-card">
      <h3>{name}</h3>
      <p>პოზიცია: {role}</p>
      <span>სტატუსი: {isOnline ? "🟢 Online" : "🔴 Offline"}</span>
    </div>
  );
}
```

```jsx
// 📁 src/App.jsx — მშობელი კომპონენტი
import { UserCard } from "./components/UserCard";

export default function App() {
  return (
    <div className="app-container">
      <h2>გუნდის წევრები</h2>

      {/* Props-ების გადაცემა */}
      <UserCard name="ლუკა" role="Full-Stack Dev" isOnline={true} />
      <UserCard name="ანა" role="UI/UX Designer" isOnline={false} />
      <UserCard name="გიორგი" role="QA Engineer" isOnline={true} />
    </div>
  );
}
```

> 💡 **კავშირი Lecture 11-12-თან:** `{ name, role, isOnline }` პარამეტრებში — ეს ხომ Object Destructuring, ზუსტად ის, რაც Lecture 13-14-ში ვისწავლეთ! Props, სინამდვილეში, უბრალოდ ერთი ობიექტია, რომელიც კომპონენტს არგუმენტად გადაეცემა — `UserCard({ name: "ლუკა", role: "...", isOnline: true })`.

---

## 🧪 Live Coding: Profile Card List React-ში

ლექციაზე სტუდენტებთან ერთად იწყებთ ნულიდან:

1. Vite-ით React პროექტის დაგენერირება.
2. `src/components/` საქაღალდის შექმნა.
3. `Header.jsx`, `ProductCard.jsx` და `Footer.jsx` კომპონენტების აწყობა.
4. `App.jsx`-ში მასივიდან დინამიურად პროდუქტების დარენდერება `.map()` მეთოდით:

```jsx
const products = [
  { id: 1, title: "MacBook Pro", price: 4500 },
  { id: 2, title: "iPhone 15 Pro", price: 3200 },
];

return (
  <div>
    {products.map((item) => (
      <ProductCard key={item.id} title={item.title} price={item.price} />
    ))}
  </div>
);
```

> ⚠️ **კრიტიკული დეტალი — `key` პროპი:** `.map()`-ით სიის დარენდერებისას, React-ს **ყოველთვის** სჭირდება უნიკალური `key` პროპი თითოეულ ელემენტზე — ეს ეხმარება Virtual DOM-ის Diffing ალგორითმს, ზუსტად იცოდეს, რომელი ელემენტი შეიცვალა, დაემატა, ან წაიშალა სიაში. `key`-ს გარეშე, React Console-ში გააფრთხილებთ, და შესაძლოა არასწორად "დაარენდეროს" სია ცვლილებების დროს. **არასდროს** გამოიყენოთ მასივის ინდექსი (`index`) `key`-ად, თუ სია დინამიურად იცვლება (ემატება/იშლება ელემენტები) — გამოიყენეთ მონაცემის საკუთარი, უნიკალური `id`.

---

## 📌 Quick Cheatsheet

| სინტაქსი                          | განმარტება                                                 |
| :-------------------------------- | :--------------------------------------------------------- |
| MPA                               | ყოველ ნავიგაციაზე სრული გვერდის reload                     |
| SPA                               | ერთი `index.html`, დინამიური DOM განახლება                 |
| Virtual DOM                       | React-ის მეხსიერებაში არსებული DOM-ის ასლი                 |
| Reconciliation/Diffing            | ძველი vs ახალი Virtual DOM-ის შედარება                     |
| `<>...</>`                        | React Fragment — Multiple Root Elements-ის თავიდან არიდება |
| `{expression}`                    | JS გამოსახულების ჩასმა JSX-ში                              |
| `className`, `htmlFor`, `onClick` | HTML ატრიბუტების CamelCase JSX ვერსია                      |
| `function ComponentName() {}`     | კომპონენტი — აუცილებლად PascalCase                         |
| `export function X({ a, b })`     | Named Export + Props Destructuring                         |
| `<Component prop={value} />`      | Props-ის გადაცემა                                          |
| `key={item.id}`                   | უნიკალური იდენტიფიკატორი `.map()`-ით რენდერისას            |

---

## 🏠 საშინაო დავალება (Homework)

### დავალება 1 (First React App with Vite):

შექმენით ახალი React პროექტი Vite-ით. წაშალეთ დეფოლტური სტილები და `App.jsx`-ში შექმენით პერსონალური Portfolio Header კომპონენტი (`Header.jsx`), სადაც გამოიტანთ თქვენს სახელს, გვარს და პროფესიას JSX ცვლადების გამოყენებით.

### დავალება 2 (Reusable Component with Props):

შექმენით `CourseCard.jsx` კომპონენტი, რომელიც Props-ის სახით მიიღებს: `title`, `instructor`, `duration`, `isCompleted`. `App.jsx`-ში გამოიყენეთ ეს კომპონენტი სულ მცირე 3-ჯერ, სხვადასხვა მონაცემებით.
