# 🧠 Web Development / Lecture 22: Custom Hooks - ლოგიკის რეუზაბელურობა

## 📌 ლექციის მიმოხილვა

წინა ლექციებზე გავიარეთ React-ის ძირითადი ჰუკები (`useState`, `useEffect`, `useRef`). დღეს ვეცნობიანთ React-ის ერთ-ერთ ყველაზე მძლავრ და პროფესიონალურ კონცეფციას — **Custom Hooks** (საკუთარი ჰუკები):

- **რა არის Custom Hook?** — წესები, კონვენცია და რატომ იწყება მისი სახელი `use`-ით.
- **Separation of Concerns (ლოგიკისა და UI-ს განცალკევება):** როგორ გავხადოთ კომპონენტები სუფთა და ადვილად ტესტებადი.
- **პრაქტიკული Custom Hooks-ების აწყობა:**
  - `useFetch(url)` — API მოთხოვნის, Loading-ისა და Error-ის რეუზაბელური ლოგიკა.
  - `useLocalStorage(key, initialValue)` — State-ის ავტომატური სინქრონიზაცია ბრაუზერის LocalStorage-თან.

---

# Part 1: რა არის Custom Hook და რა წესები აქვს?

Custom Hook არის ჩვეულებრივი JavaScript ფუნქცია, რომლის შიგნითაც ვიყენებთ React-ის სხვა ჰუკებს (`useState`, `useEffect`, `useRef` და ა.შ.).

## 🔑 3 მთავარი წესი Custom Hooks-ისთვის:

1. **სახელის პრეფიქსი (`use`):** ფუნქციის სახელი აუცილებლად უნდა იწყებოდეს სიტყვით `use` (მაგ: `useFetch`, `useLocalStorage`, `useWindowSize`).
   > რატომ? React-ის Linter-მა რომ იცოდეს, რომ ეს არის ჰუკი და გააკონტროლოს ჰუკების წესები (Rules of Hooks).
2. **მხოლოდ React ფუნქციებში:** Custom Hook უნდა გამოიძახოთ მხოლოდ Functional Component-ის შიგნით ან სხვა Custom Hook-იდან.
3. **State Isolation:** თითოეული კომპონენტი, რომელიც იყენებს Custom Hook-ს, იღებს ჰუკის შიგნით არსებული State-ის სრულად დამოუკიდებელ (Isolated) ასლს.

---

# Part 2: Separating Concerns (ლოგიკისა და UI-ს განცალკევება)

წინა ლექციებზე `useEffect` და `fetch` პირდაპირ კომპონენტებში ვწერდით. წარმოიდგინეთ, თუ 5 სხვადასხვა კომპონენტს სჭირდება API მოთხოვნა — გავიმეორებთ ერთსა და იმავე `useState` (`data`, `loading`, `error`) და `useEffect` კოდს 5-ჯერ? (**DRY Principle** — _Don't Repeat Yourself_).

Custom Hook საშუალებას გვაძლევს ეს მთელი ლოგიკა გავიტანოთ ცალკე ფაილში.

---

# Part 3: პრაქტიკული Custom Hook-ების აწყობა

## 1️⃣ `useFetch(url)` — API მოთხოვნის რეუზაბელური ლოგიკა

შევქმნათ ჰუკი, რომელიც მიიღებს URL-ს და დააბრუნებს `{ data, isLoading, error }` ობიექტს.

```jsx
// 📁 src/hooks/useFetch.js
import { useState, useEffect } from "react";

export function useFetch(url) {
  const [data, setData] = useState(null);
  const [isLoading, setIsLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const fetchData = async () => {
      try {
        setIsLoading(true);
        setError(null);

        const response = await fetch(url);

        if (!response.ok) {
          throw new Error(`HTTP Error! Status: ${response.status}`);
        }

        const result = await response.json();
        setData(result);
      } catch (err) {
        setError(err.message);
      } finally {
        setIsLoading(false);
      }
    };

    fetchData();
  }, [url]); // თუ url შეიცვლება, ჰუკი ავტომატურად გააგზავნის ახალ მოთხოვნას

  return { data, isLoading, error };
}
```

### 🎯 როგორ ვიყენებთ მას კომპონენტში?

```jsx
// 📁 src/components/ProductCatalog.jsx
import { useFetch } from "../hooks/useFetch";

export function ProductCatalog() {
  // ერთი ხაზით ვიღებთ ყველაფერს! აღარ გვჭირდება useEffect და useState-ების წერა კომპონენტში.
  const { data, isLoading, error } = useFetch(
    "https://dummyjson.com/products?limit=5",
  );

  if (isLoading) return <p>იტვირთება პროდუქტები... ⏳</p>;
  if (error) return <p style={{ color: "red" }}>შეცდომა: {error}</p>;

  return (
    <div>
      <h2>პროდუქტების კატალოგი</h2>
      <ul>
        {data.products.map((product) => (
          <li key={product.id}>
            {product.title} - ${product.price}
          </li>
        ))}
      </ul>
    </div>
  );
}
```

## 2️⃣ `useLocalStorage(key, initialValue)` — State სინქრონიზაცია LocalStorage-თან

ხშირად გვჭირდება, რომ State-ის მნიშვნელობა (მაგ: Dark/Light Theme ან User Settings) შეინახოს `localStorage`-ში, რათა გვერდის გადატვირთვისას არ დაიკარგოს.

```jsx
// 📁 src/hooks/useLocalStorage.js
import { useState, useEffect } from "react";

export function useLocalStorage(key, initialValue) {
  // 1. ვცდილობთ მნიშვნელობის წაკითხვას LocalStorage-დან
  const [value, setValue] = useState(() => {
    try {
      const item = window.localStorage.getItem(key);
      return item ? JSON.parse(item) : initialValue;
    } catch (error) {
      console.error(`Error reading localStorage key "${key}":`, error);
      return initialValue;
    }
  });

  // 2. როცა value იცვლება, ვანახლებთ LocalStorage-ს
  useEffect(() => {
    try {
      window.localStorage.setItem(key, JSON.stringify(value));
    } catch (error) {
      console.error(`Error setting localStorage key "${key}":`, error);
    }
  }, [key, value]);

  return [value, setValue];
}
```

### 🎯 გამოყენება კომპონენტში:

```jsx
// 📁 src/components/ThemeToggle.jsx
import { useLocalStorage } from "../hooks/useLocalStorage";

export function ThemeToggle() {
  // მუშაობს ზუსტად ისე, როგორც ჩვეულებრივი useState, მაგრამ ავტომატურად ინახავს LocalStorage-ში!
  const [theme, setTheme] = useLocalStorage("app_theme", "light");

  const toggleTheme = () => {
    setTheme((prev) => (prev === "light" ? "dark" : "light"));
  };

  return (
    <div
      style={{
        padding: "20px",
        background: theme === "dark" ? "#222" : "#fff",
        color: theme === "dark" ? "#fff" : "#000",
      }}
    >
      <p>მიმდინარე თემა: {theme}</p>
      <button onClick={toggleTheme}>თემის შეცვლა</button>
    </div>
  );
}
```

---

## 🧪 Live Coding: `useFetch`-ის გამოყენება სხვადასხვა კომპონენტში

ლექციაზე სტუდენტებთან ერთად ვინახავთ `useFetch` ჰუკს `src/hooks/useFetch.js` ფაილში და ვამოწმებთ, რამდენად მარტივია მისი მიბმა ორ სხვადასხვა კომპონენტზე:

- `UserList.jsx` (`https://dummyjson.com/users`)
- `PostList.jsx` (`https://dummyjson.com/posts`)

ორთავე კომპონენტი თავისუფლდება ბოილერპლეიტისგან (Boilerplate code) და კოდი ხდება ელეგანტური და სუფთა.

---

## 📌 Quick Cheatsheet

| სინტაქსი                                            | განმარტება                                         |
| :-------------------------------------------------- | :------------------------------------------------- |
| `function useSomething() {...}`                     | Custom Hook — ყოველთვის `use`-ით იწყება            |
| `useFetch(url) → { data, isLoading, error }`        | API ლოგიკის რეუზაბელური hook                       |
| `useLocalStorage(key, initial) → [value, setValue]` | State + LocalStorage სინქრონიზაცია                 |
| ჰუკის გამოძახება მხოლოდ კომპონენტში/სხვა ჰუკში      | Rules of Hooks                                     |
| 2 კომპონენტი, 1 Custom Hook                         | თითოეულს თავისი, დამოუკიდებელი State               |
| DRY Principle                                       | Don't Repeat Yourself — ლოგიკის გატანა ცალკე ჰუკში |

---

## 🏠 საშინაო დავალება (Homework)

### დავალება 1 (Custom Hook `useCounter`):

- შექმენით `useCounter.jsx` ჰუკი, რომელიც მართავს რიცხვით State-ს. ჰუკმა უნდა დააბრუნოს: `count`, `increment`, `decrement`, `reset` და `setCount` ფუნქციები.
- გამოიყენეთ ეს ჰუკი `CounterComponent`-ში.

### დავალება 2 (Custom Hook `useDebounce`):

- შექმენით `useDebounce(value, delay)` ჰუკი, რომელიც გამოიყენება Search ინპუტებში (რომ არ გაიგზავნოს API მოთხოვნა ყოველ კლავიშზე აკრეფისას, არამედ დაელოდოს მომხმარებლის შეჩერებას, მაგ: 500 მილიწამი).
- გამოიყენეთ ის რეალურ Search აპლიკაციაში.
