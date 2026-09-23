# ⚛️ Web Development / Lecture 17: Component Architecture, Props & State (useState)

## 📌 ლექციის მიმოხილვა

წინა ლექციაზე ვისწავლეთ React-ის, SPA-ს და JSX-ის საფუძვლები. დღეს გადავდივართ React-ის ყველაზე ფუნდამენტურ კონცეფციებზე:

- **Component Architecture & PascalCase:** UI-ს დაყოფა დამოუკიდებელ ნაწილებად.
- **Props (Properties):** მონაცემების გადაცემა მშობლიდან შვილ კომპონენტზე.
- **State (`useState` Hook):** კომპონენტის შიდა დინამიური მეხსიერება და UI-ს Re-render-ის მექანიზმი.
- **State Immutability & Spread (`...`):** რატომ არ შეიძლება State-ის პირდაპირი მუტაცია (Mutate) და როგორ განვაახლოთ ობიექტები და მასივები სწორად.

---

# Part 1: Component Architecture & PascalCase

React-ში მთელი UI აგებულია კომპონენტებისგან.

> ⚠️ **ოქროს წესი:** კომპონენტის სახელი აუცილებლად **PascalCase**-ით (დიდი ასოთი) უნდა იწყებოდეს (მაგ. `UserCard.jsx`, `Button.jsx`).
>
> თუ სახელს პატარა ასოთი დაიწყებთ (`userCard`), React მას აღიქვამს სტანდარტულ HTML ტეგად (`<div>`, `<span>`) და აპლიკაცია დარეფორმდება შეცდომით!

```jsx
// 📁 src/components/Header.jsx
export function Header() {
  return (
    <header className="header">
      <h1>My React App</h1>
    </header>
  );
}
```

---

# Part 2: Props (Properties) — Read-Only Data

Props არის მექანიზმი, რომლითაც მშობელი კომპონენტი გადასცემს მონაცემებს შვილ კომპონენტს (როგორც ფუნქციის არგუმენტები).

## 🔑 Props-ის მთავარი თვისებები:

- **Unidirectional Data Flow:** მონაცემი მოძრაობს მხოლოდ ზევიდან ქვევით (მშობლიდან ➔ შვილისკენ).
- **Read-Only (Immutable):** შვილ კომპონენტს არ აქვს უფლება პირდაპირ შეცვალოს მიღებული props.

```jsx
// 📁 src/components/UserCard.jsx
// Destructuring-ის გამოყენება პარამეტრებში
export function UserCard({ name, role, age = 18 }) {
  return (
    <div className="card">
      <h3>{name}</h3>
      <p>როლი: {role}</p>
      <p>ასაკი: {age}</p>
    </div>
  );
}
```

```jsx
// 📁 src/App.jsx
import { UserCard } from "./components/UserCard";

export default function App() {
  return (
    <div>
      {/* Props-ის გადაცემა */}
      <UserCard name="ლუკა" role="Software Engineer" age={23} />
      <UserCard name="გიორგი" role="UI/UX Designer" />
    </div>
  );
}
```

---

# Part 3: State (`useState` Hook) — დინამიური მეხსიერება

თუ Props არის გარეთა მონაცემი, **State** არის კომპონენტის შიდა, დინამიური მეხსიერება, რომელიც დროთა განმავლობაში იცვლება (მაგ. ინპუტის ტექსტი, Modal-ის ღია/დახურული სტატუსი, ქაუნთერის მნიშვნელობა).

## ❓ რატომ არ მუშაობს ჩვეულებრივი `let` ცვლადი?

```jsx
// ❌ ეს არ იმუშავებს!
export function BadCounter() {
  let count = 0;

  const handleClick = () => {
    count++; // ცვლადი იცვლება, მაგრამ React-მა არ იცის, რომ UI უნდა გადაახატოს (Re-render)
    console.log(count);
  };

  return <button onClick={handleClick}>Count: {count}</button>;
}
```

> 💡 ეს ღილაკი Console-ში სწორად დაბეჭდავს ზრდად რიცხვს, მაგრამ ეკრანზე `Count: 0` **არასდროს** შეიცვლება — React-ს არ აქვს ინფორმაცია, რომ `count`-ის ცვლილებამ UI-ც უნდა განაახლოს.

## ✅ სწორი მიდგომა: `useState` Hook

`useState` ეუბნება React-ს: "როდესაც ეს მონაცემი შეიცვლება, ხელახლა გამოიძახე კომპონენტის ფუნქცია (Re-render) და განაახლე DOM-ი!"

```jsx
import { useState } from "react";

export function Counter() {
  // useState აბრუნებს 2 ელემენტიან მასივს: [მიმდინარე State, Setter ფუნქცია]
  const [count, setCount] = useState(0);

  return (
    <div>
      <h2>Count: {count}</h2>
      <button onClick={() => setCount(count + 1)}>გაზრდა</button>
      <button onClick={() => setCount(count - 1)}>შემცირება</button>
    </div>
  );
}
```

> 💡 **სინტაქსის ახსნა:** `const [count, setCount] = useState(0);` — ეს არის Array Destructuring (Lecture 13-14-დან ნაცნობი!). `useState(0)` აბრუნებს მასივს `[0, setterFunction]`-ს, და ჩვენ ამ ორ ელემენტს ვშლით ცალკეულ ცვლადებში — პირველი არის მიმდინარე მნიშვნელობა, მეორე — ფუნქცია, რომლითაც ამ მნიშვნელობას ვცვლით.

---

# Part 4: State Immutability & Spread (`...`) ოპერატორი

React ამოწმებს State-ის ცვლილებას **Reference-ის** (მეხსიერების მისამართის) მიხედვით. თუ ობიექტს ან მასივს **პირდაპირ** შევცვლით (mutate), მეხსიერების მისამართი იგივე რჩება და React **არ** გააკეთებს Re-render-ს!

## ❌ შეცდომა (Direct Mutation):

```jsx
const [user, setUser] = useState({ name: "ლუკა", age: 22 });

// ❌ არასწორია! React ვერ დაინახავს ცვლილებას
user.age = 23;
setUser(user);
```

> 💡 **რატომ ვერ ხედავს React?** `user.age = 23` ცვლის ობიექტს **იმავე მისამართზე** — `setUser(user)`-ს გადაეცემა ზუსტად ის ობიექტი, რომელიც უკვე იყო State-ში. React შედარებას აკეთებს "ძველი მისამართი === ახალი მისამართი?" პრინციპით (ეს ჩვენი Lecture 19-ის Pass by Reference-ის პირდაპირი პრაქტიკული შედეგია!) — და, ვინაიდან მისამართი არ შეცვლილა, React ფიქრობს, "არაფერი შეცვლილა", და Re-render-ს არ აკეთებს.

## ✅ სწორი მიდგომა (Immutable Update):

ყოველთვის უნდა შევქმნათ **ახალი** ობიექტი/მასივი Spread (`...`) ოპერატორის გამოყენებით.

### 1. ობიექტის განახლება State-ში:

```jsx
const [user, setUser] = useState({ name: "ლუკა", age: 22, role: "Dev" });

const updateAge = () => {
  setUser({
    ...user, // დააკოპირე ძველი თვისებები
    age: 23, // გადააფარე მხოლოდ age
  });
};
```

### 2. მასივის განახლება State-ში:

```jsx
const [items, setItems] = useState(["React", "Node"]);

const addItem = () => {
  // ❌ items.push("Express") — კატეგორიულად აკრძალულია!
  setItems([...items, "Express"]); // ✅ ახალი მასივის შექმნა
};
```

> ⚠️ **კრიტიკული წესი:** State-ში არსებულ მასივზე **არასდროს** გამოიყენოთ Mutating მეთოდები (`push`, `pop`, `splice`, `sort` — გახსოვთ Lecture 13-14-ის Mutating vs Non-Mutating ცხრილი?) — ყოველთვის შექმენით ახალი მასივი Spread-ით, ან Non-Mutating მეთოდებით (`map`, `filter`).

---

## 🧪 Live Coding / პრაქტიკული მაგალითები

### 1. Toggle / Accordion Component (`isOpen`, `setIsOpen`)

```jsx
// 📁 src/components/Accordion.jsx
import { useState } from "react";

export function Accordion({ title, content }) {
  const [isOpen, setIsOpen] = useState(false);

  const toggleOpen = () => {
    setIsOpen(!isOpen); // ან setIsOpen((prev) => !prev);
  };

  return (
    <div
      className="accordion-item"
      style={{ border: "1px solid #ccc", margin: "10px", padding: "10px" }}
    >
      <div
        onClick={toggleOpen}
        style={{
          cursor: "pointer",
          fontWeight: "bold",
          display: "flex",
          justifyContent: "space-between",
        }}
      >
        <span>{title}</span>
        <span>{isOpen ? "➖" : "➕"}</span>
      </div>

      {/* Conditional Rendering */}
      {isOpen && <p style={{ marginTop: "10px" }}>{content}</p>}
    </div>
  );
}
```

> 💡 **`{isOpen && <p>...}`-ის ახსნა:** ეს არის **Conditional Rendering** ლოგიკური `&&`-ით — თუ `isOpen` არის `true`, JSX მარჯვნივ დაირენდერება; თუ `false`-ია, JavaScript-ის `&&` ოპერატორი მთელ გამოსახულებას "მოკლედ" წყვეტს (Short-Circuit), და **არაფერი** არ დაირენდერება (`false`-ის დარენდერება React-ში "არაფერს" უდრის).

---

## 📌 Quick Cheatsheet

| სინტაქსი                                      | განმარტება                                         |
| :-------------------------------------------- | :------------------------------------------------- |
| `PascalCase`                                  | კომპონენტის სახელის სავალდებულო ფორმატი            |
| `{ name, role, age = 18 }`                    | Props Destructuring + Default Value                |
| `const [state, setState] = useState(initial)` | State-ის დეკლარაცია                                |
| `setState(newValue)`                          | State-ის განახლება → Re-render                     |
| `setState(prev => !prev)`                     | Functional Update — წინა მნიშვნელობაზე დაფუძნებული |
| `{...obj, key: newValue}`                     | Immutable ობიექტის განახლება                       |
| `[...arr, newItem]`                           | Immutable მასივზე დამატება                         |
| `{condition && <JSX />}`                      | Conditional Rendering                              |

---

## React DevTools

ექსთენშენის ლინკი: [React DevTools]((https://react.dev/learn/react-developer-tools)) 

## React Documentation

დოკუმენტაცია: [React Documentation]((https://react.dev/learn))

## 🏠 საშინაო დავალება (Homework)

### დავალება 1 (Counter with Step):

შექმენით `AdvancedCounter.jsx` კომპონენტი. ქაუნთერს ჰქონდეს `+1`, `-1` და `Reset` ღილაკები. დაამატეთ `step` State (დეფოლტად `1`), რომლითაც მომხმარებელი შეძლებს აირჩიოს, რამდენით გაიზარდოს/შემცირდეს თვლა (`+5`, `+10` და ა.შ.).

### დავალება 2 (Profile Card with Dark Mode Toggle):

შექმენით `ProfileCard.jsx` კომპონენტი, რომელიც Props-ით იღებს მომხმარებლის მონაცემებს (`name`, `bio`, `avatarUrl`). კომპონენტის შიგნით ჰქონდეს ღილაკი "Toggle Theme", რომელიც `useState`-ით შეცვლის მხოლოდ ამ კონკრეტული ბარათის ფონს (Light / Dark), `isDark`-ის State-ზე დაყრდნობით.

`[დიზაინის ლინკი:](https://www.figma.com/design/sSomtkMEXxPZCHYZ4Bvs5d/Profile-Card-UI--Community-?node-id=1-6&t=CpTK44yRrqI242z6-0)`
