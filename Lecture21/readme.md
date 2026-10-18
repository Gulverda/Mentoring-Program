# 🚀 Web Development / Lecture 21: React Forms, Controlled Components & useRef

## 📌 ლექციის მიმოხილვა

წინა ლექციაზე ვისწავლეთ `useEffect` Hook და API-სთან ინტეგრაცია. დღეს განვიხილავთ, როგორ ვიღებთ და ვმართავთ მომხმარებლის მიერ შეყვანილ მონაცემებს ფორმებიდან:

- **Controlled Components:** ინპუტების სრული კონტროლი React State-ის საშუალებით (`value` და `onChange`).
- **Multi-Input Form Management:** მრავალი ინპუტის მართვა მხოლოდ 1 State ობიექტით (`[e.target.name]: e.target.value`).
- **`useRef` Hook:** DOM-ზე პირდაპირი წვდომა Re-render-ის გამოწვევის გარეშე (Auto-Focus, File Upload, Uncontrolled Elements).
- **Form Submit & Validation:** `e.preventDefault()`, ვალიდაციის ლოგიკა და შეცდომების გამოტანა.

---

# Part 1: Controlled Components (მართვადი კომპონენტები)

HTML-ში ფორმის ინპუტები (`<input>`, `<textarea>`, `<select>`) თავად ინახავენ საკუთარ მნიშვნელობას (Internal State). React-ში **Best Practice** არის, რომ ინპუტის ერთადერთი ჭეშმარიტი წყარო (**Single Source of Truth**) იყოს React State.

## 🛠️ Controlled Input-ის 2 აუცილებელი კომპონენტი:

- **`value`:** მიბმულია React State-ზე.
- **`onChange`:** ანახლებს State-ს ყოველ კლავიშზე დაჭერისას.

```jsx
import { useState } from "react";

export function SimpleInput() {
  const [name, setName] = useState("");

  return (
    <div>
      <input
        type="text"
        value={name}
        onChange={(e) => setName(e.target.value)}
        placeholder="ჩაწერეთ სახელი"
      />
      <p>შესვლა: {name}</p>
    </div>
  );
}
```

---

# Part 2: მრავალი ინპუტის მართვა 1 State ობიექტით 🔑

თუ ფორმაში გვექნება 5 ან 10 ინპუტი, თითოეულისთვის ცალკე `useState`-ის შექმნა (`const [email, setEmail] = useState("")`, `const [password, setPassword] = useState("")`...) კოდს გადატვირთავს.

> 💡 **პროფესიონალური მიდგომა:** ვქმნით 1 State ობიექტს და ინპუტებს ვანსხვავებთ მათი `name` ატრიბუტით — `[e.target.name]` დინამიური property key-ს გამოყენებით.

```jsx
import { useState } from "react";

export function MultiInputForm() {
  const [formData, setFormData] = useState({
    username: "",
    email: "",
    role: "user", // select-ის საწყისი მნიშვნელობა
  });

  const handleChange = (e) => {
    const { name, value, type, checked } = e.target;

    setFormData((prevData) => ({
      ...prevData, // ვინარჩუნებთ ძველ ველებს
      [name]: type === "checkbox" ? checked : value, // დინამიური გასაღები!
    }));
  };

  return (
    <form>
      <input
        type="text"
        name="username" // ⚠️ name უნდა ემთხვეოდეს State-ის ობიექტის key-ს!
        value={formData.username}
        onChange={handleChange}
        placeholder="მომხმარებლის სახელი"
      />
      <input
        type="email"
        name="email"
        value={formData.email}
        onChange={handleChange}
        placeholder="ელ.ფოსტა"
      />
      <select name="role" value={formData.role} onChange={handleChange}>
        <option value="user">User</option>
        <option value="admin">Admin</option>
      </select>
    </form>
  );
}
```

---

# Part 3: `useRef` Hook — DOM-ზე წვდომა Re-render-ის გარეშე

`useRef` არის ჰუკი, რომელიც აბრუნებს ცვლად ობიექტს `{ current: initialValue }`.

## 🔑 `useState` vs `useRef` შედარება:

| თვისება             | `useState`                            | `useRef`                                      |
| :------------------ | :------------------------------------ | :-------------------------------------------- |
| მნიშვნელობის შეცვლა | იწვევს Re-render-ს 🔄                 | არ იწვევს Re-render-ს 🚫                      |
| მეხსიერება          | ინახავს მონაცემს Re-render-ებს შორის  | ინახავს მონაცემს Re-render-ებს შორის          |
| გამოყენება          | UI-ში გამოსაჩენი დინამიური მონაცემები | DOM ელემენტებზე წვდომა, ტაიმერების ID, ფოკუსი |

## 🛠️ `useRef`-ის ძირითადი გამოყენებები:

### 1. DOM ელემენტზე პირდაპირი წვდომა (Auto-Focus):

```jsx
import { useRef, useEffect } from "react";

export function AutoFocusInput() {
  const inputRef = useRef(null);

  useEffect(() => {
    // კომპონენტის ჩატვირთვისთანავე ფოკუსდება ინპუტზე
    inputRef.current.focus();
  }, []);

  return <input ref={inputRef} type="text" placeholder="ავტომატური ფოკუსი" />;
}
```

### 2. File Inputs (Uncontrolled Input):

ფაილის ინპუტი (`<input type="file" />`) React-ში არის **Uncontrolled**, რადგან მისი მნიშვნელობა Read-Only-ა. მასზე წვდომას ვახდენთ `useRef`-ით.

---

# Part 4: Form Submit, `e.preventDefault()` & Validation

ფორმის გაგზავნისას (`onSubmit`):

- **`e.preventDefault()`:** აუცილებელია ბრაუზერის სტანდარტული ქცევის (გვერდის გადატვირთვის/Reload) შესაჩერებლად.
- **Form Validation:** მონაცემების შემოწმება გაგზავნამდე.

---

## 🧪 Live Coding / პრაქტიკული მაგალითი

### სარეგისტრაციო ფორმა ავტომატური ფოკუსით, ვალიდაციითა და `useRef`-ით

```jsx
// 📁 src/components/RegisterForm.jsx
import { useState, useRef, useEffect } from "react";

export function RegisterForm() {
  // 1. State ფორმის მონაცემებისთვის
  const [formData, setFormData] = useState({
    username: "",
    email: "",
    password: "",
  });

  // 2. State შეცდომების (Validation Errors) შეტყობინებებისთვის
  const [errors, setErrors] = useState({});

  // 3. useRef ავტომატური ფოკუსისთვის პირველ ინპუტზე
  const usernameInputRef = useRef(null);

  useEffect(() => {
    // გვერდის ჩატვირთვისას ავტომატურად დააფოკუსე Username ინპუტზე
    usernameInputRef.current?.focus();
  }, []);

  // 4. Input Change Handler
  const handleChange = (e) => {
    const { name, value } = e.target;
    setFormData((prev) => ({
      ...prev,
      [name]: value,
    }));

    // ტიპინგისას შეცდომის გასუფთავება კონკრეტულ ველზე
    if (errors[name]) {
      setErrors((prevErrors) => ({
        ...prevErrors,
        [name]: null,
      }));
    }
  };

  // 5. Validation Logic
  const validateForm = () => {
    const newErrors = {};

    if (!formData.username.trim()) {
      newErrors.username = "მომხმარებლის სახელი აუცილებელია!";
    }

    if (!formData.email.includes("@")) {
      newErrors.email = "გთხოვთ შეიყვანოთ ვალიდური ელ.ფოსტა!";
    }

    if (formData.password.length < 6) {
      newErrors.password = "პაროლი უნდა შედგებოდეს მინიმუმ 6 სიმბოლოსგან!";
    }

    setErrors(newErrors);
    // თუ ობიექტი ცარიელია (keys.length === 0), ესე იგი ვალიდაცია გაიარა
    return Object.keys(newErrors).length === 0;
  };

  // 6. Form Submit Handler
  const handleSubmit = (e) => {
    e.preventDefault(); // ბრაუზერის გადატვირთვის შეჩერება

    if (validateForm()) {
      console.log("ფორმის მონაცემები წარმატებით გაიგზავნა:", formData);
      alert("რეგისტრაცია წარმატებით დასრულდა! 🎉");

      // ფორმის გასუფთავება
      setFormData({ username: "", email: "", password: "" });
      usernameInputRef.current?.focus(); // ხელახალი ფოკუსი
    }
  };

  return (
    <div
      style={{
        maxWidth: "400px",
        margin: "40px auto",
        padding: "20px",
        border: "1px solid #ccc",
        borderRadius: "10px",
      }}
    >
      <h2>სარეგისტრაციო ფორმა</h2>

      <form onSubmit={handleSubmit}>
        {/* Username Input */}
        <div style={{ marginBottom: "15px" }}>
          <label style={{ display: "block", marginBottom: "5px" }}>
            მომხმარებლის სახელი:
          </label>
          <input
            ref={usernameInputRef} // useRef-ის მიბმა
            type="text"
            name="username"
            value={formData.username}
            onChange={handleChange}
            style={{ width: "100%", padding: "8px", boxSizing: "border-box" }}
          />
          {errors.username && (
            <span style={{ color: "red", fontSize: "0.85rem" }}>
              {errors.username}
            </span>
          )}
        </div>

        {/* Email Input */}
        <div style={{ marginBottom: "15px" }}>
          <label style={{ display: "block", marginBottom: "5px" }}>
            ელ.ფოსტა:
          </label>
          <input
            type="email"
            name="email"
            value={formData.email}
            onChange={handleChange}
            style={{ width: "100%", padding: "8px", boxSizing: "border-box" }}
          />
          {errors.email && (
            <span style={{ color: "red", fontSize: "0.85rem" }}>
              {errors.email}
            </span>
          )}
        </div>

        {/* Password Input */}
        <div style={{ marginBottom: "15px" }}>
          <label style={{ display: "block", marginBottom: "5px" }}>
            პაროლი:
          </label>
          <input
            type="password"
            name="password"
            value={formData.password}
            onChange={handleChange}
            style={{ width: "100%", padding: "8px", boxSizing: "border-box" }}
          />
          {errors.password && (
            <span style={{ color: "red", fontSize: "0.85rem" }}>
              {errors.password}
            </span>
          )}
        </div>

        <button
          type="submit"
          style={{
            width: "100%",
            padding: "10px",
            backgroundColor: "#4f46e5",
            color: "white",
            border: "none",
            borderRadius: "5px",
            cursor: "pointer",
          }}
        >
          რეგისტრაცია
        </button>
      </form>
    </div>
  );
}
```

---

## 📌 Quick Cheatsheet

| სინტაქსი                           | განმარტება                                              |
| :--------------------------------- | :------------------------------------------------------ |
| `value={state}` + `onChange={...}` | Controlled Component — React State ერთადერთი წყაროა     |
| `[e.target.name]: e.target.value`  | დინამიური key — მრავალი ინპუტი 1 State ობიექტით         |
| `{ ...prevData, [name]: value }`   | Spread + Computed Property — ძველი ველების შენარჩუნება  |
| `useRef(null)`                     | ცვლადი ობიექტი `{ current: null }`, Re-render-ის გარეშე |
| `<input ref={myRef} />`            | DOM ელემენტზე მიბმა `ref` ატრიბუტით                     |
| `myRef.current.focus()`            | პირდაპირი DOM მეთოდის გამოძახება                        |
| `e.preventDefault()`               | გვერდის რელოადის შეჩერება ფორმის გაგზავნისას            |
| `Object.keys(errors).length === 0` | ვალიდაციის შედეგის შემოწმება                            |

---

## 🏠 საშინაო დავალება (Homework)

### დავალება 1 (Login Form with Show/Hide Password & useRef Focus):

- შექმენით ავტორიზაციის ფორმა (`email`, `password`).
- დაამატეთ Show/Hide Password ღილაკი, რომელიც შეცვლის ინპუტის ტიპს (`password` ➔ `text`).
- გამოიყენეთ `useRef`, რათა არასწორი პაროლის შემთხვევაში ფოკუსი ავტომატურად გადავიდეს პაროლის ინპუტზე და ტექსტი იყოს არჩეული (`inputRef.current.select()`).

### დავალება 2 (Uncontrolled File Upload Preview with useRef):

- შექმენით `<input type="file" accept="image/*" />` კომპონენტი `useRef`-ის გამოყენებით.
- სურათის არჩევისას (`onChange`), წაიკითხეთ ფაილი (`FileReader`) და აჩვენეთ სურათის პრევიუ UI-ში (`<img src="..." />`).
