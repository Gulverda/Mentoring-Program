# 🧠 Web Development / Lecture 22: Custom Hooks - ლოგიკის რეუზაბელურობა / React Router — Client-Side Routing

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

# 🧭 React Router — Client-Side Routing

## 📌 ლექციის მიმოხილვა

აქამდე ჩვენი React აპლიკაცია ერთ "გვერდს" წარმოადგენდა — ერთი `App`, რომელიც ყველაფერს აჩვენებდა. რეალურ საიტზე კი გვაქვს `/`, `/products`, `/products/12`, `/login`... დღეს ვისწავლით, როგორ მივაბათ **URL** კონკრეტულ **კომპონენტებს** — გვერდის გადატვირთვის გარეშე:

- **Client-Side Routing:** რატომ არის SPA-ში `<a href>` პრობლემა და როგორ მუშაობს რაუტერი.
- **`BrowserRouter`, `Routes`, `Route`:** მარშრუტების აღწერა.
- **`Link` vs `NavLink`:** ნავიგაცია და აქტიური ბმულის სტილი.
- **Dynamic Routes (`:id`) & `useParams`:** ერთი კომპონენტი — უსასრულო რაოდენობის URL.
- **Nested Routes, Layout & `Outlet`:** საერთო ნავიგაცია ყველა გვერდისთვის.
- **`useNavigate`:** პროგრამული ნავიგაცია (`navigate(-1)`, რედირექტი).
- **Search Params (`useSearchParams`):** ფილტრები და ძებნა URL-ში.
- **404 გვერდი და Protected Routes:** `path="*"`, `<Navigate />`, დაცული გვერდები.

> ⚠️ **ვერსიის შენიშვნა (2026):** React Router-ის მიმდინარე ვერსია **v8**-ია. ინსტალაცია: `npm install react-router`, იმპორტი — `from "react-router"`. ძველ ტუტორიალებში ნახავთ `react-router-dom`-ს — ეს ძველი პაკეტია, რომელიც 7.x ვერსიაზე დარჩა; ახალ პროექტში მას აღარ ვიყენებთ. v8 მოითხოვს **Node.js ≥ 22.22**, ამიტომ დაიწყეთ `node -v`-ის შემოწმებით.

---

# Part 1: რატომ გვჭირდება Client-Side Routing?

ლექცია 22-ში ვისწავლეთ განსხვავება MPA და SPA-ს შორის. გავიხსენოთ:

|                     | MPA (კლასიკური საიტი)               | SPA (React)                                              |
| :------------------ | :---------------------------------- | :------------------------------------------------------- |
| ბმულზე დაჭერა       | ბრაუზერი სერვერს ახალ HTML-ს სთხოვს | JS ცვლის ეკრანს, **სერვერთან ახალი გვერდი არ იტვირთება** |
| გვერდის გადატვირთვა | ✅ ყოველ ჯერზე                      | ❌ არასდროს                                              |
| State               | იკარგება                            | ინახება                                                  |

## ❌ პრობლემა: ჩვეულებრივი `<a href>` React-ში

```jsx
// ❌ ბრაუზერი სრულად გადატვირთავს გვერდს, React-ის მთელი State იკარგება
<a href="/products">პროდუქტები</a>
```

## ✅ გადაწყვეტა: Router

რაუტერი უსმენს URL-ის ცვლილებას (**History API**-ის დახმარებით), და ამ URL-ს შესაბამის კომპონენტს აჩვენებს — ბრაუზერის მისამართიც იცვლება, Back/Forward ღილაკებიც მუშაობს, მაგრამ გვერდი **არ** იტვირთება თავიდან.

```
URL: /products/12   ➜   [ Router ]   ➜   <ProductDetailsPage />
```

---

# Part 2: ინსტალაცია და საბაზო კონფიგურაცია

```bash
npm install react-router
```

## 1️⃣ აპლიკაციის "შეფუთვა" `BrowserRouter`-ით

```jsx
// 📁 src/main.jsx
import { StrictMode } from "react";
import { createRoot } from "react-dom/client";
import { BrowserRouter } from "react-router";
import App from "./App.jsx";

createRoot(document.getElementById("root")).render(
  <StrictMode>
    <BrowserRouter>
      <App />
    </BrowserRouter>
  </StrictMode>,
);
```

## 2️⃣ მარშრუტების აღწერა `Routes` და `Route`-ით

```jsx
// 📁 src/App.jsx
import { Routes, Route } from "react-router";
import { HomePage } from "./pages/HomePage";
import { AboutPage } from "./pages/AboutPage";

export default function App() {
  return (
    <Routes>
      <Route path="/" element={<HomePage />} />
      <Route path="/about" element={<AboutPage />} />
    </Routes>
  );
}
```

- **`path`** — URL-ის ნაწილი.
- **`element`** — JSX, რომელიც ამ URL-ზე უნდა დარენდერდეს.
- `Routes` ირჩევს **ერთადერთ** შესაბამის `Route`-ს.

---

# Part 3: `Link` vs `NavLink`

```jsx
import { Link, NavLink } from "react-router";

// Link — ჩვეულებრივი ბმული, <a> ტეგად დარენდერდება, მაგრამ გვერდს არ ტვირთავს
<Link to="/about">ჩვენ შესახებ</Link>

// NavLink — იგივეა, ოღონდ "იცის", აქტიურია თუ არა
<NavLink
  to="/products"
  className={({ isActive }) => (isActive ? "nav-link nav-link--active" : "nav-link")}
>
  პროდუქტები
</NavLink>
```

## ⚠️ `end` პროპი მთავარ გვერდზე

`/` არის **ყველა** URL-ის დასაწყისი, ამიტომ `<NavLink to="/">` ყოველთვის "აქტიური" იქნება. გამოსავალი:

```jsx
<NavLink to="/" end>
  მთავარი
</NavLink>
```

> 💡 `NavLink` ავტომატურად ამატებს `active` კლასს აქტიურ ბმულზე, ამიტომ მარტივი სტილისთვის `a.active { ... }` CSS-იც საკმარისია.

---

# Part 4: Dynamic Routes & `useParams`

ვთქვათ, გვაქვს 100 პროდუქტი. 100 `Route` ხომ არ დავწეროთ? **Dynamic Segment** (`:id`) ამ პრობლემას წყვეტს:

```jsx
<Route path="/products/:id" element={<ProductDetailsPage />} />
```

URL-იდან მნიშვნელობას `useParams` გვაძლევს:

```jsx
// 📁 src/pages/ProductDetailsPage.jsx
import { useParams } from "react-router";
import { useFetch } from "../hooks/useFetch"; // ლექცია 22-ის Custom Hook!

export function ProductDetailsPage() {
  const { id } = useParams(); // ⚠️ ყოველთვის String-ია!

  const {
    data: product,
    isLoading,
    error,
  } = useFetch(`https://dummyjson.com/products/${id}`);

  if (isLoading) return <p>იტვირთება... ⏳</p>;
  if (error) return <p>შეცდომა: {error}</p>;

  return (
    <article>
      <h1>{product.title}</h1>
      <p>{product.description}</p>
      <strong>${product.price}</strong>
    </article>
  );
}
```

და სიიდან გადასვლა დეტალურ გვერდზე:

```jsx
{
  products.map((product) => (
    <li key={product.id}>
      <Link to={`/products/${product.id}`}>{product.title}</Link>
    </li>
  ));
}
```

> ⚠️ `useParams()` ყოველთვის აბრუნებს **სტრინგს**. თუ რიცხვი გჭირდებათ: `Number(id)`.

---

# Part 5: Nested Routes, Layout & `Outlet`

ნავიგაციის მენიუ (Navbar) ყველა გვერდზე ერთნაირია. მის ყველა კომპონენტში გამეორებას ერიდებიან **Layout Route**-ით:

```jsx
// 📁 src/App.jsx
<Routes>
  <Route element={<Layout />}>
    {" "}
    {/* path-ის გარეშე — უბრალოდ "შესაფუთი" */}
    <Route index element={<HomePage />} />{" "}
    {/* index = მშობლის საკუთარი გვერდი (/) */}
    <Route path="about" element={<AboutPage />} />
    <Route path="products" element={<ProductsPage />} />
    <Route path="products/:id" element={<ProductDetailsPage />} />
  </Route>
</Routes>
```

```jsx
// 📁 src/components/Layout.jsx
import { NavLink, Outlet } from "react-router";

export function Layout() {
  return (
    <>
      <header>
        <nav>
          <NavLink to="/" end>
            მთავარი
          </NavLink>
          <NavLink to="/products">პროდუქტები</NavLink>
          <NavLink to="/about">ჩვენ შესახებ</NavLink>
        </nav>
      </header>

      <main>
        <Outlet /> {/* აქ ჩაჯდება მიმდინარე შვილობილი გვერდი */}
      </main>

      <footer>© INF321</footer>
    </>
  );
}
```

- **`<Outlet />`** — "ხვრელი" მშობელ კომპონენტში, სადაც შვილი Route-ის `element` გამოჩნდება.
- **`index`** — შვილობილი Route, რომელიც მშობლის URL-ზე (ამ შემთხვევაში `/`) რენდერდება.
- ბავშვის `path` **ფარდობითია** მშობლის მიმართ (`"about"` და არა `"/about"`).

---

# Part 6: `useNavigate` — პროგრამული ნავიგაცია

`Link` გამოიყენეთ, როცა **მომხმარებელი** აჭერს ბმულს. `useNavigate` კი მაშინ, როცა გადასვლა უნდა მოხდეს **კოდიდან** — მაგ. ფორმის გაგზავნის შემდეგ.

```jsx
import { useNavigate } from "react-router";

function BackButton() {
  const navigate = useNavigate();

  return (
    <>
      <button onClick={() => navigate(-1)}>← უკან</button>{" "}
      {/* ბრაუზერის Back */}
      <button onClick={() => navigate("/products")}>სიაში</button>
      <button onClick={() => navigate("/login", { replace: true })}>
        გადასვლა ისტორიის ჩანაწერის ჩანაცვლებით
      </button>
    </>
  );
}
```

| გამოძახება                             | შედეგი                                                        |
| :------------------------------------- | :------------------------------------------------------------ |
| `navigate("/path")`                    | ახალ გვერდზე გადასვლა (ისტორიაში ემატება)                     |
| `navigate(-1)` / `navigate(1)`         | Back / Forward                                                |
| `navigate("/path", { replace: true })` | მიმდინარე ჩანაწერის **ჩანაცვლება** (Back-ით ვეღარ დაბრუნდება) |

---

# Part 7: Search Params — `useSearchParams`

URL-ის `?q=phone&category=laptops` ნაწილი იდეალურია **ფილტრებისთვის**: ბმული გაზიარებადია და Refresh-ს უძლებს.

```jsx
import { useSearchParams } from "react-router";

export function ProductsPage() {
  const [searchParams, setSearchParams] = useSearchParams();
  const query = searchParams.get("q") ?? ""; // URL არის "ჭეშმარიტების ერთადერთი წყარო"

  return (
    <input
      value={query}
      onChange={(e) => setSearchParams({ q: e.target.value })}
      placeholder="მოძებნე..."
    />
  );
}
```

> 💡 ლექცია 25-ში ძებნის `searchTerm` `useState`-ში ვინახავდით. აქ იგივე ამოცანა გვაქვს, მაგრამ State-ის ადგილას **URL** დგას — ამიტომ `/products?q=phone` ბმული გაგზავნის შემდეგაც იმავე შედეგს აჩვენებს.

---

# Part 8: 404 — `path="*"`

```jsx
<Routes>
  <Route element={<Layout />}>
    <Route index element={<HomePage />} />
    {/* ... */}
    <Route path="*" element={<NotFoundPage />} /> {/* ბოლოს, "catch-all" */}
  </Route>
</Routes>
```

`*` ერგება ნებისმიერ URL-ს, რომელიც ზემოთ არცერთ Route-ს არ დაემთხვა. React Router ირჩევს **ყველაზე სპეციფიკურ** დამთხვევას, ამიტომ `*` არასდროს "გადაფარავს" რეალურ გვერდებს.

---

# Part 9: Protected Routes — `<Navigate />`

რეალურ აპში `/dashboard` მხოლოდ ავტორიზებულებისთვისაა. გამოსავალი — **Layout Route-ი, რომელიც ამოწმებს ავტორიზაციას**:

```jsx
// 📁 src/components/ProtectedRoute.jsx
import { Navigate, Outlet, useLocation } from "react-router";

export function ProtectedRoute({ isAuthenticated }) {
  const location = useLocation();

  if (!isAuthenticated) {
    // replace — რომ Back-ზე დაჭერით "დაცულ" გვერდზე უკან არ დაბრუნდეს
    // state — დავიმახსოვროთ, საიდან მოვიდა, რომ Login-ის შემდეგ იქ დავაბრუნოთ
    return <Navigate to="/login" replace state={{ from: location }} />;
  }

  return <Outlet />;
}
```

```jsx
<Route element={<ProtectedRoute isAuthenticated={Boolean(user)} />}>
  <Route path="dashboard" element={<DashboardPage />} />
</Route>
```

და Login გვერდზე, წარმატებული შესვლის შემდეგ:

```jsx
const navigate = useNavigate();
const location = useLocation();
const from = location.state?.from?.pathname ?? "/dashboard";

function handleSubmit(e) {
  e.preventDefault();
  // ... ვალიდაცია, onLogin(username) ...
  navigate(from, { replace: true }); // ❗ ორიგინალ მისამართზე დაბრუნება
}
```

|       | `<Navigate />`         | `useNavigate()`            |
| :---- | :--------------------- | :------------------------- |
| ტიპი  | კომპონენტი (JSX-ში)    | ჰუკი (ფუნქცია)             |
| როდის | რენდერის დროს, პირობით | Event Handler-ში / ეფექტში |

---

# Part 10: შემდეგი ნაბიჯი — Data Mode (მიმოხილვა)

დღეს ვისწავლეთ **Declarative Mode** (`BrowserRouter` + `<Routes>`) — ყველაზე მარტივი და სასწავლო. React Router-ს აქვს ასევე **Data Mode** (`createBrowserRouter` + `RouterProvider`), სადაც Route-ს შეუძლია `loader` (მონაცემების წინასწარ წამოღება) და `action` (ფორმების დამუშავება):

```jsx
import { createBrowserRouter } from "react-router";
import { RouterProvider } from "react-router/dom";

const router = createBrowserRouter([
  {
    path: "/",
    element: <Layout />,
    children: [{ index: true, element: <HomePage /> }],
  },
]);

<RouterProvider router={router} />;
```

ამას ცალკე ლექციაზე განვიხილავთ — ახლა მთავარია Declarative Mode-ის პრინციპების გაგება: **Route-ებს იგივე კონცეფციები აქვთ ორივე რეჟიმში**.

---

# ⚠️ Deploy-ის ნიუანსი: Refresh `/products/12`-ზე → 404?

`localhost`-ზე `/products/12`-ზე Refresh მუშაობს, რადგან Vite Dev Server ნებისმიერ URL-ზე `index.html`-ს აბრუნებს. **GitHub Pages** კი ფიზიკურ ფაილს `products/12/index.html` ეძებს და ვერ პოულობს → 404.

| გამოსავალი       | აღწერა                                                                                           |
| :--------------- | :----------------------------------------------------------------------------------------------- |
| `HashRouter`     | URL ხდება `/#/products/12` — სერვერს `#`-ის შემდეგი ნაწილი არ ეგზავნება. უმარტივესი სასწავლო გზა |
| `404.html` ხრიკი | `404.html`-ში გადამისამართება `index.html`-ზე — GH Pages-ზე გავრცელებული workaround              |
| Vercel/Netlify   | უბრალოდ ერთი "rewrite to index.html" წესი                                                        |

თუ საიტი ქვე-გზაზე ცხოვრობს (`username.github.io/repo-name/`), დაგჭირდებათ `basename`: `<BrowserRouter basename="/repo-name">` და Vite-ში `base: "/repo-name/"`.

---

## 🧪 Live Coding: მინი-მაღაზია

ლექციაზე ვაგებთ აპლიკაციას შემდეგი მარშრუტებით:

| URL                 | კომპონენტი           | თემა                                      |
| :------------------ | :------------------- | :---------------------------------------- |
| `/`                 | `HomePage`           | `index` Route                             |
| `/about`            | `AboutPage`          | უბრალო Route                              |
| `/products?q=phone` | `ProductsPage`       | Search Params                             |
| `/products/:id`     | `ProductDetailsPage` | `useParams` + `useFetch` + `navigate(-1)` |
| `/login`            | `LoginPage`          | `useNavigate` + `location.state`          |
| `/dashboard`        | `DashboardPage`      | Protected Route                           |
| `*`                 | `NotFoundPage`       | 404                                       |

---

## 📌 Quick Cheatsheet

| სინტაქსი                                           | განმარტება                                 |
| :------------------------------------------------- | :----------------------------------------- |
| `npm install react-router`                         | ინსტალაცია (v8)                            |
| `<BrowserRouter>`                                  | აპის შეფუთვა (`main.jsx`-ში)               |
| `<Routes>` + `<Route path element />`              | მარშრუტების აღწერა                         |
| `<Route index element />`                          | მშობლის საკუთარი (ნაგულისხმევი) გვერდი     |
| `<Route path="*" />`                               | 404 / catch-all                            |
| `<Link to>` / `<NavLink to end>`                   | ნავიგაცია / ნავიგაცია აქტიური სტილით       |
| `path="products/:id"` + `useParams()`              | Dynamic segment (მნიშვნელობა — **String**) |
| `<Outlet />`                                       | შვილობილი Route-ის ადგილი Layout-ში        |
| `useNavigate()` → `navigate("/x")`, `navigate(-1)` | პროგრამული ნავიგაცია                       |
| `useSearchParams()`                                | `?key=value` წაკითხვა/ჩაწერა               |
| `useLocation()`                                    | მიმდინარე URL + `state`                    |
| `<Navigate to replace state />`                    | რედირექტი რენდერისას                       |

---

## 🏠 საშინაო დავალება (Homework)

### დავალება 1 (Blog App — List & Details):

- შექმენით `/posts` გვერდი, რომელიც `useFetch`-ით წამოიღებს პოსტებს `https://dummyjson.com/posts?limit=10`-დან და ყოველ პოსტს `Link`-ად აჩვენებს.
- შექმენით `/posts/:id` გვერდი (`useParams` + `useFetch`), რომელიც ერთი პოსტის სათაურსა და ტექსტს აჩვენებს, და "← უკან" ღილაკს (`navigate(-1)`).
- დაამატეთ `Layout` `NavLink`-ებით (აქტიური ბმულის სტილით) და 404 გვერდი.

### დავალება 2 (Protected Dashboard + Redirect Back):

- შექმენით `LoginPage` და `DashboardPage`. `DashboardPage` უნდა იყოს დაცული `ProtectedRoute`-ით.
- ავტორიზაციის გარეშე `/dashboard`-ზე შესვლისას მომხმარებელი გადაიყვანეთ `/login`-ზე.
- წარმატებული Login-ის შემდეგ დააბრუნეთ **იქ, საიდანაც მოვიდა** (`location.state.from`).
- ავტორიზაციის მდგომარეობა შეინახეთ `useLocalStorage` ჰუკით (ლექცია 22), რომ Refresh-ზე არ დაიკარგოს. დაამატეთ "გასვლა" ღილაკი.
