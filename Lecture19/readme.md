# ⚛️ Web Development / Lecture 19: Lists, Conditional Rendering & Lifting State Up

## 📌 ლექციის მიმოხილვა

წინა ლექციებზე ვისწავლეთ კომპონენტების აგება, Props, `useState` და სტილიზაცია. დღეს განვიხილავთ React-ის **3 უმნიშვნელოვანეს არქიტექტურულ თემას**, რომელთა გარეშეც რეალური აპლიკაციის აწყობა წარმოუდგენელია:

- **List Rendering (`.map()` & `key` Prop):** მონაცემთა მასივების ეფექტურად დარენდერება და `key`-ს როლი React-ის Reconciliation ალგორითმში.
- **Conditional Rendering:** ინტერფეისის დინამიური გამოჩენა/დამალვა (`? :`, `&&` და Early Return).
- **Lifting State Up (კრიტიკული თემა):** State-ის აწევა საერთო მშობელში, კომპონენტებს შორის მონაცემთა გაზიარება და Handler ფუნქციების გადაცემა Props-ის საშუალებით.

---

# Part 1: List Rendering & The `key` Prop

React-ში მასივის ელემენტების UI-ად გარდასაქმნელად გამოიყენება JavaScript-ის `.map()` მეთოდი.

```jsx
const fruits = ["ვაშლი", "ბანანი", "ატამი"];

export function FruitList() {
  return (
    <ul>
      {fruits.map((fruit, index) => (
        <li key={index}>{fruit}</li>
      ))}
    </ul>
  );
}
```

## 🔑 რატომ არის `key` Prop აუცილებელი?

React იყენებს Virtual DOM-ს და Diffing ალგორითმს. როდესაც მასივში ელემენტი ემატება, იშლება ან იცვლის ადგილს, React-ს სჭირდება ზუსტად იცოდეს, რომელი კონკრეტული HTML ელემენტი შეიცვალა, რათა მთლიანი სია ხელახლა არ გადაახატოს (Re-render).

### ⚠️ რატომ არ შეიძლება `index`-ის გამოყენება Key-დ?

თუ Key-დ გავატანთ მასივის ინდექსს (`key={index}`), მასივში ელემენტების წაშლის ან სორტირების დროს ინდექსები იცვლება.

- ეს იწვევს არასწორ UI Re-render-ებს (მაგალითად, ინპუტებში ჩაწერილი ტექსტი რჩება ძველ ადგილას, ან არასწორი ელემენტი იშლება).
- იქმნება Performance პრობლემა.

> ✅ **ოქროს წესი:** `key` აუცილებლად უნდა იყოს **უნიკალური და სტაბილური** იდენტიფიკატორი ბაზიდან ან მონაცემებიდან (მაგ: `item.id`).

```jsx
// ✅ სწორი მიდგომა
const products = [
  { id: "p1", name: "Laptop", price: 2500 },
  { id: "p2", name: "Phone", price: 1200 },
];

{
  products.map((product) => (
    <div key={product.id}>
      <h3>{product.name}</h3>
      <p>{product.price} ₾</p>
    </div>
  ));
}
```

---

# Part 2: Conditional Rendering (პირობითი რენდერი)

ხშირად UI უნდა შეიცვალოს გარკვეული პირობის მიხედვით (მაგ: მომხმარებელი ავტორიზებულია თუ არა, მონაცემები იტვირთება თუ არა).

## 1️⃣ Ternary Operator (`condition ? true : false`)

გამოიყენება მაშინ, როდესაც გვსურს ორიდან ერთ-ერთი UI-ს გამოჩენა.

```jsx
function UserStatus({ isLoggedIn }) {
  return (
    <div>{isLoggedIn ? <button>გასვლა</button> : <button>შესვლა</button>}</div>
  );
}
```

## 2️⃣ Logical AND (`&&`) Operator და მისი ფარული საფრთხე ⚠️

გამოიყენება მაშინ, როდესაც UI უნდა გამოჩნდეს მხოლოდ მაშინ, თუ პირობა ჭეშმარიტია (`true`).

```jsx
{
  unreadMessagesCount > 0 && <p>გაქვთ ახალი შეტყობინებები!</p>;
}
```

### 🚨 ლოგიკური `&&`-ის გავრცელებული შეცდომა React-ში:

JavaScript-ში `&&` ოპერატორი აბრუნებს პირველივე **falsy** მნიშვნელობას. თუ მარცხენა მხარეს რიცხვი `0`-ია, React მას აღიქვამს არა როგორც ბულევს (`false`), არამედ როგორც **დარენდერებად მნიშვნელობას**!

```jsx
// ❌ შეცდომა: თუ messages.length არის 0, ეკრანზე დაიბეჭდება ციფრი "0"!
const messages = [];

return <div>{messages.length && <p>Messages count: {messages.length}</p>}</div>;

// ✅ გამოსწორება 1: გარდაქმენით ბულევად Boolean() ან !!-ით
{
  messages.length > 0 && <p>Messages count: {messages.length}</p>;
}

// ✅ გამოსწორება 2: Ternary operator-ის გამოყენება
{
  messages.length ? <p>Messages count: {messages.length}</p> : null;
}
```

## 3️⃣ Early Return (ადრეული დაბრუნება)

გამოიყენება კომპონენტის დასაწყისში, როდესაც გარკვეული პირობისას მთლიანი კომპონენტი არ უნდა დარენდერდეს.

```jsx
function UserProfile({ user, isLoading }) {
  if (isLoading) {
    return <h2>იტვირთება...</h2>;
  }

  if (!user) {
    return <h2>მომხმარებელი ვერ მოიძებნა</h2>;
  }

  return <div>Welcome, {user.name}!</div>;
}
```

---

# Part 3: Lifting State Up (State-ის აწევა) — 🔑 კრიტიკული თემა!

React-ში მონაცემთა ნაკადი არის **ცალმხრივი (Unidirectional Data Flow)**: მონაცემი მოძრაობს მშობლიდან შვილისკენ.

## ❓ რა ხდება, როცა ორ "ძმა" (Sibling) კომპონენტს სჭირდება ერთი და იგივე State?

მაგალითად: `SearchInput` კომპონენტში აკრეფილი ტექსტით უნდა გაიფილტროს `ProductList` კომპონენტი.

### 💡 გადაჭრა (Lifting State Up):

State გადაგვაქვს მათი უახლოესი საერთო მშობლის (Parent) დონეზე. მშობელი ფლობს State-ს, ხოლო შვილებს გადასცემს:

1. State-ის მნიშვნელობას props-ით.
2. State-ის შესაცვლელ Handler ფუნქციას props-ით.

```
               [ Parent Component (Holds State) ]
                       /              \
         (passes handler)            (passes state)
                     /                  \
      [ SearchInput (Child 1) ]    [ ProductList (Child 2) ]
```

---

## 🧪 Live Coding / პრაქტიკული მაგალითი

### პროდუქტების კატალოგის დარენდერება და დინამიური ფილტრაცია

```jsx
// 📁 src/components/SearchInput.jsx
export function SearchInput({ searchTerm, onSearchChange }) {
  return (
    <input
      type="text"
      placeholder="მოძებნე პროდუქტი..."
      value={searchTerm}
      onChange={(e) => onSearchChange(e.target.value)} // მშობლის State-ის განახლება
      style={{ padding: "8px 12px", width: "100%", marginBottom: "20px" }}
    />
  );
}
```

```jsx
// 📁 src/components/ProductList.jsx
export function ProductList({ products }) {
  if (products.length === 0) {
    return <p>პროდუქტები ვერ მოიძებნა 🔍</p>;
  }

  return (
    <div style={{ display: "grid", gap: "10px" }}>
      {products.map((product) => (
        <div
          key={product.id}
          style={{
            border: "1px solid #ddd",
            padding: "10px",
            borderRadius: "8px",
          }}
        >
          <h4>{product.name}</h4>
          <p>კატეგორია: {product.category}</p>
          <p>ფასი: {product.price} ₾</p>
        </div>
      ))}
    </div>
  );
}
```

```jsx
// 📁 src/components/ProductCatalog.jsx (საერთო მშობელი)
import { useState } from "react";
import { SearchInput } from "./SearchInput";
import { ProductList } from "./ProductList";

const INITIAL_PRODUCTS = [
  { id: "1", name: "iPhone 15 Pro", category: "Phones", price: 3200 },
  { id: "2", name: "MacBook Pro M3", category: "Laptops", price: 5400 },
  { id: "3", name: "AirPods Pro 2", category: "Audio", price: 750 },
  { id: "4", name: "Samsung S24 Ultra", category: "Phones", price: 3100 },
];

export function ProductCatalog() {
  // 1. State აწეულია საერთო მშობელში
  const [searchTerm, setSearchTerm] = useState("");

  // 2. ფილტრაციის ლოგიკა
  const filteredProducts = INITIAL_PRODUCTS.filter((product) =>
    product.name.toLowerCase().includes(searchTerm.toLowerCase()),
  );

  return (
    <div style={{ maxWidth: "500px", margin: "40px auto" }}>
      <h2>პროდუქტების კატალოგი</h2>

      {/* 3. Handler-ის გადაცემა Child 1-ისთვის */}
      <SearchInput searchTerm={searchTerm} onSearchChange={setSearchTerm} />

      {/* 4. გაფილტრული მონაცემების გადაცემა Child 2-ისთვის */}
      <ProductList products={filteredProducts} />
    </div>
  );
}
```

---

## 📌 Quick Cheatsheet

| სინტაქსი                                     | განმარტება                                           |
| :------------------------------------------- | :--------------------------------------------------- |
| `array.map((item) => <li key={item.id}>...)` | List Rendering — მასივის UI-ად გარდაქმნა             |
| `key={item.id}`                              | ✅ სტაბილური, უნიკალური key                          |
| `key={index}`                                | ⚠️ თავიდან ასაცილებელია — არასტაბილურია              |
| `cond ? <A /> : <B />`                       | Ternary — ორიდან ერთ-ერთის არჩევა                    |
| `cond && <A />`                              | Logical AND — მხოლოდ true-ს დროს გამოჩენა            |
| `count > 0 && ...`                           | ✅ ყოველთვის ბულევში გარდაქმენი რიცხვი &&-ის წინ     |
| `if (x) return <A />;`                       | Early Return — მთლიანი კომპონენტის ადრეული დაბრუნება |
| `useState` მშობელში + `props`-ით გადაცემა    | Lifting State Up                                     |

---

## 🏠 საშინაო დავალება (Homework)

### დავალება 1 (Todo App with Lifting State Up):

- შექმენით `TodoInput.jsx` (ინპუტი და Add ღილაკი).
- შექმენით `TodoList.jsx` (სიის დარენდერება `.map()`-ით და წაშლის ღილაკით).
- შექმენით `TodoApp.jsx` (საერთო მშობელი, სადაც ინახება `todos` State).
- შვილმა კომპონენტმა უნდა შეძლოს ახალი დავალების დამატება და არსებულის წაშლა მშობლიდან გადაწოდებული Handler ფუნქციებით.

### დავალება 2 (Safe Conditional Rendering Practice):

- შექმენით `CartBadge.jsx` კომპონენტი, რომელიც Props-ით იღებს `items` მასივს.
- თუ მასივი ცარიელია, არაფერი არ უნდა გამოჩნდეს (გამოიყენეთ უსაფრთხო პირობითი რენდერი `items.length > 0`).
- თუ მასივში არის ელემენტები, გამოაჩინეთ ბეჯი რაოდენობით.
