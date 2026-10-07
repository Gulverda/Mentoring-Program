# ⚛️ Web Development / Lecture 20: Side Effects & useEffect Hook (API Integration)

## 📌 ლექციის მიმოხილვა

აქამდე ჩვენი React კომპონენტები მუშაობდნენ მხოლოდ შიდა State-თან და Props-თან. დღეს გადავდივართ **რეალურ სამყაროსთან (გარე სამყაროსთან) კომუნიკაციაზე**:

- **Component Lifecycle:** კომპონენტის სიცოცხლის 3 ეტაპი (Mounting, Updating, Unmounting).
- **რა არის Side Effect?** რა ოპერაციები ითვლება "გვერდით ეფექტად" React-ში.
- **`useEffect` Hook-ის ანატომია:** Dependency Array-ს 3 ვარიანტი და მათი ქცევა.
- **Cleanup Function:** რატომ და როგორ ვასუფთავებთ მეხსიერებას (Timers, Event Listeners).
- **API Integration:** მონაცემების წამოღება, Loading & Error State-ების პროფესიონალური მართვა.

---

# Part 1: Component Lifecycle & Side Effects

React-ის ყოველ კომპონენტს აქვს სიცოცხლის ციკლი (Lifecycle), რომელიც 3 ეტაპისგან შედგება:

```
[ 1. Mounting ]  ➔  [ 2. Updating ]  ➔  [ 3. Unmounting ]
  კომპონენტი          State/Props           კომპონენტი
 ეკრანზე ჩნდება        იცვლება             ეკრანიდან ქრება
```

## ❓ რა არის Side Effect (გვერდითი ეფექტი)?

React-ის კომპონენტის ძირითადი დანიშნულებაა **Pure Render** — მიიღოს Props/State და დააბრუნოს JSX.

ყველაფერი, რაც ამ სუფთა პროცესის მიღმა ხდება და გარე სამყაროს ეხება, არის **Side Effect**:

- API-დან მონაცემების წამოღება (`fetch` / `axios`).
- DOM-ზე პირდაპირი მანიპულაცია (მაგ: `document.title = "New Title"`).
- Timers & Intervals (`setTimeout`, `setInterval`).
- Event Listeners (მაგ: `window`-ზე `resize` ან `scroll` ევენთის მოსმენა).

---

# Part 2: `useEffect` Hook-ის ანატომია

`useEffect` არის ჰუკი, რომელიც საშუალებას გვაძლევს გავაშვათ Side Effect-ები კომპონენტის სიცოცხლის ციკლის კონკრეტულ მომენტში.

```jsx
import { useEffect } from "react";

useEffect(
  () => {
    // 1. Effect Code (აქ იწერება Side Effect ლოგიკა)

    return () => {
      // 2. Cleanup Function (გასუფთავების ლოგიკა)
    };
  },
  [
    /* 3. Dependency Array */
  ],
);
```

---

# Part 3: Dependency Array-ს 3 ვარიანტი 🔑 (კრიტიკული!)

`useEffect`-ის მეორე არგუმენტი — Dependency Array განსაზღვრავს, **როდის** უნდა გაეშვას ეფექტი:

## 1️⃣ უმასივოდ (No Array)

```jsx
useEffect(() => {
  console.log("გაეშვა ყოველ Re-render-ზე!");
});
```

⚠️ **ქცევა:** ეფექტი გაეშვება პირველ რენდერზე (Mount) და ყოველ შემდგომ Re-render-ზე. ხშირად იწვევს უსასრულო ციკლებს (Infinite Loops), თუ შიგნით State-ს განვაახლებთ!

## 2️⃣ ცარიელი მასივი (`[]`)

```jsx
useEffect(() => {
  console.log("გაეშვა მხოლოდ ერთხელ — Mount-ის დროს!");
}, []);
```

✅ **ქცევა:** ეფექტი გაეშვება მხოლოდ 1-ხელ, როდესაც კომპონენტი პირველად გამოჩნდება ეკრანზე (Mount). იდეალურია API-დან საწყისი მონაცემების წამოსაღებად.

## 3️⃣ მასივი ცვლადებით (`[dependency]`)

```jsx
useEffect(() => {
  console.log(`searchTerm შეიცვალა, ახალი მნიშვნელობა: ${searchTerm}`);
}, [searchTerm]);
```

🔄 **ქცევა:** გაეშვება Mount-ის დროს + ყოველ ჯერზე, როდესაც `searchTerm` ცვლადის მნიშვნელობა შეიცვლება. იდეალურია ძებნის (Search) ან ფილტრაციის დროს API მოთხოვნის ხელახლა გასაგზავნად.

---

# Part 4: Cleanup Function (მეხსიერების გასუფთავება)

თუ `useEffect`-ში ვრთავთ ტაიმერს, ინტერვალს ან `window.addEventListener`-ს, კომპონენტის წაშლისას (Unmount) ეს პროცესები მეხსიერებაში რჩება (**Memory Leak**).

ამისთვის `useEffect`-იდან ვაბრუნებთ **Cleanup ფუნქციას**:

```jsx
import { useState, useEffect } from "react";

export function Timer() {
  const [seconds, setSeconds] = useState(0);

  useEffect(() => {
    // 1. ვრთავთ ინტერვალს
    const intervalId = setInterval(() => {
      setSeconds((prev) => prev + 1);
    }, 1000);

    // 2. Cleanup Function (გაეშვება Unmount-ის დროს ან ეფექტის ხელახლა გაშვებამდე)
    return () => {
      clearInterval(intervalId); // ინტერვალის გასუფთავება
    };
  }, []); // [] ნიშნავს, რომ მხოლოდ 1-ხელ ჩაირთვება

  return <h2>ტაიმერი: {seconds} წამი</h2>;
}
```

---

# Part 5: Data Fetching — API Integration, Loading & Error States

რეალურ აპლიკაციაში API მოთხოვნისას ყოველთვის გვჭირდება 3 State-ის მართვა:

- **`data`:** წამოღებული მონაცემები (`null` ან `[]`).
- **`isLoading`:** იტვირთება თუ არა მონაცემები (`boolean`).
- **`error`:** მოხდა თუ არა შეცდომა (`null` ან ტექსტური შეტყობინება).

---

## 🧪 Live Coding / პრაქტიკული მაგალითი

### მონაცემების წამოღება DummyJSON API-დან

```jsx
// 📁 src/components/UserList.jsx
import { useState, useEffect } from "react";

export function UserList() {
  const [users, setUsers] = useState([]);
  const [isLoading, setIsLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    // async ფუნქციის შექმნა useEffect-ის შიგნით
    const fetchUsers = async () => {
      try {
        setIsLoading(true);
        setError(null);

        const response = await fetch("https://dummyjson.com/users?limit=5");

        if (!response.ok) {
          throw new Error("მონაცემების წამოღება ვერ მოხერხდა!");
        }

        const data = await response.json();
        setUsers(data.users);
      } catch (err) {
        setError(err.message);
      } finally {
        setIsLoading(false); // იტვირთება დასრულდა (წარმატებით თუ შეცდომით)
      }
    };

    fetchUsers();
  }, []); // [] — წამოიღე მხოლოდ კომპონენტის ჩატვირთვისას

  // 1. Conditional Rendering: Loading State
  if (isLoading) {
    return <h3 style={{ textAlign: "center" }}>მონაცემები იტვირთება... ⏳</h3>;
  }

  // 2. Conditional Rendering: Error State
  if (error) {
    return (
      <h3 style={{ color: "red", textAlign: "center" }}>შეცდომა: {error} ❌</h3>
    );
  }

  // 3. Main Data Render
  return (
    <div style={{ maxWidth: "600px", margin: "20px auto" }}>
      <h2>მომხმარებლების სია</h2>
      <ul style={{ listStyle: "none", padding: 0 }}>
        {users.map((user) => (
          <li
            key={user.id}
            style={{
              padding: "12px",
              border: "1px solid #ddd",
              borderRadius: "8px",
              marginBottom: "8px",
              display: "flex",
              justify: "space-between",
            }}
          >
            <span>
              <strong>
                {user.firstName} {user.lastName}
              </strong>
            </span>
            <span>{user.email}</span>
          </li>
        ))}
      </ul>
    </div>
  );
}
```

> 💡 **რატომ `async` ფუნქცია შიგნით, არა თვითონ `useEffect`-ის callback?** `useEffect`-ის callback-ს უნდა დააბრუნოს ან `undefined`, ან Cleanup ფუნქცია — არასდროს `Promise`. ამიტომ `async` ფუნქციას ვაცხადებთ `useEffect`-ის **შიგნით** და ვუძახებთ იქვე, callback-ის დასაბრუნებელი მნიშვნელობის გარეშე.

---

## 📌 Quick Cheatsheet

| სინტაქსი                                 | განმარტება                                            |
| :--------------------------------------- | :---------------------------------------------------- |
| `useEffect(() => {...})`                 | ❌ უმასივოდ — გაეშვება ყოველ Re-render-ზე             |
| `useEffect(() => {...}, [])`             | ✅ ცარიელი მასივი — მხოლოდ Mount-ზე                   |
| `useEffect(() => {...}, [dep])`          | 🔄 მასივი ცვლადით — Mount + `dep`-ის ცვლილებაზე       |
| `return () => { cleanup(); }`            | Cleanup Function — Unmount-ზე ან ეფექტის გამეორებამდე |
| `clearInterval(id)` / `clearTimeout(id)` | Timer-ების გასუფთავება                                |
| `window.removeEventListener(...)`        | Event Listener-ების გასუფთავება                       |
| `isLoading` / `error` / `data`           | API State Management-ის 3 საყრდენი State              |

---

## 🏠 საშინაო დავალება (Homework)

### დავალება 1 (Product Search with Debounce Effect):

- შექმენით ინპუტი `searchTerm` State-ით.
- `useEffect`-ში `searchTerm`-ის ცვლილებაზე გააგზავნეთ API მოთხოვნა `https://dummyjson.com/products/search?q={searchTerm}`.
- მართეთ `isLoading` და `error` State-ები.
- Dependency Array-ში ჩასვით `[searchTerm]`.

### დავალება 2 (Window Width Tracker with Cleanup):

- შექმენით `WindowTracker.jsx` კომპონენტი, რომელიც ეკრანზე გამოაჩენს `window.innerWidth`-ს.
- `useEffect`-ში დაამატეთ `window.addEventListener('resize', ...)` ევენთი.
- აუცილებლად დაწერეთ Cleanup Function, რომელიც წაშლის ევენთს (`window.removeEventListener`).
