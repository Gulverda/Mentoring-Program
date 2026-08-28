# 🌲 Web Development / Lecture11: DOM Manipulation და Event Handling

## 📌 ლექციის მიმოხილვა

დღევანდელ გაერთიანებულ ლექციაზე გადავდივართ JavaScript-ის ყველაზე ვიზუალურ ნაწილზე:

- **DOM Manipulation (DOM-ით მანიპულირება):** რა არის DOM Tree, როგორ ვიპოვოთ, შევქმნათ, წავშალოთ და შევუცვალოთ სტილები/კლასები HTML ელემენტებს.
- **Event Handling (მოვლენების მართვა):** როგორ ვაიძულოთ ვებ-გვერდი ირეაგიროს მომხმარებლის მოქმედებებზე (`click`, `submit`, `keydown`) და როგორ მუშაობს Event Bubbling & Delegation.

---

# Part 1: DOM-ის შესავალი და ელემენტების მართვა

## 🌲 რა არის DOM (Document Object Model)?

DOM არის ბრაუზერში ჩატვირთული HTML დოკუმენტის ხისებრი (Tree) სტრუქტურის წარმოდგენა. ბრაუზერი თითოეულ HTML ტეგს გარდაქმნის JavaScript ობიექტად (Node), რისი მეშვეობითაც შეგვიძლია კოდიდან გვერდის ნებისმიერი ნაწილის ცვლილება.

```
                 document
                    |
                 <html>
           +--------+--------+
           |                 |
        <head>            <body>
           |                 |
        <title>         +----+----+
                        |         |
                       <h1>      <div>
```

## 1️⃣ ელემენტების მოძებნა (DOM Selection)

თანამედროვე JavaScript-ში ელემენტების მოსაძებნად უპირატესად ვიყენებთ `querySelector` და `querySelectorAll` მეთოდებს:

```javascript
// 1. ერთი ელემენტის მოძებნა (CSS სელექტორით)
const mainTitle = document.querySelector("#main-title");
const submitBtn = document.querySelector(".btn-submit");

// 2. მრავალი ელემენტის მოძებნა (აბრუნებს NodeList-ს)
const allCards = document.querySelectorAll(".card");

// NodeList-ზე გადაყოლა
allCards.forEach((card) => {
  console.log(card);
});
```

> 💡 `querySelector` იღებს ჩვეულებრივ **CSS სელექტორს** — ანუ ზუსტად იმას, რასაც `.css` ფაილში წერდით (`#id`, `.class`, `tag`, ან კომბინაცია).

## 2️⃣ ტექსტისა და შინაარსის შეცვლა

```javascript
const heading = document.querySelector("h1");

// textContent — ცვლის მხოლოდ ტექსტს (უსაფრთხოა)
heading.textContent = "ახალი სათაური JS-დან!";

// innerHTML — საშუალებას გვაძლევს ჩავსვათ შიდა HTML ტეგებიც
const container = document.querySelector(".container");
container.innerHTML = `<p class="text-blue">ახალი აბზაცი</p>`;
```

> ⚠️ **უსაფრთხოების შენიშვნა:** `innerHTML`-ს მხოლოდ მაშინ იყენეთ, როცა HTML-ი, რომელსაც ჩასვამთ, **სანდო წყაროდან** მოდის (თქვენი კოდი, არა მომხმარებლის ინფუთი). წინააღმდეგ შემთხვევაში, XSS (Cross-Site Scripting) შეტევის რისკი იზრდება. მომხმარებლის ტექსტისთვის — ყოველთვის `textContent`.

## 3️⃣ სტილებისა და CSS კლასების მართვა

```javascript
const box = document.querySelector(".box");

// ა) Inline სტილის შეცვლა
box.style.backgroundColor = "royalblue";
box.style.fontSize = "20px";

// ბ) classList — კლასების მართვა (Best Practice!)
box.classList.add("active"); // კლასის დამატება
box.classList.remove("hidden"); // კლასის წაშლა
box.classList.toggle("highlight"); // თუ აქვს, წაშლის; თუ არ აქვს, დაამატებს
```

> 💡 **Best Practice:** სტილების მართვისას ყოველთვის ამჯობინეთ `classList`-ს პირდაპირ `.style`-ზე — CSS ლოგიკა `.css` ფაილში რჩება, JS მხოლოდ კლასებს ურთავს/თიშავს. ეს კოდს ბევრად უფრო სუფთად და მართვადს ხდის დიდ პროექტებში.

## 4️⃣ ელემენტების დინამიურად შექმნა და წაშლა

```javascript
// 1. შექმნა
const newCard = document.createElement("div");
newCard.classList.add("card");
newCard.textContent = "ახალი ბარათი";

// 2. ჩასმა DOM-ში (მშობელ ელემენტში)
const parentDiv = document.querySelector(".wrapper");
parentDiv.appendChild(newCard); // ჩასვამს ბოლოში
// ან parentDiv.prepend(newCard); // ჩასვამს დასაწყისში

// 3. წაშლა
// newCard.remove();
```

---

# Part 2: Event Handling (მოვლენების მართვა)

Event (მოვლენა) არის სიგნალი იმისა, რომ ბრაუზერში რაღაც მოხდა (მომხმარებელმა დააკლიკა, აკრიფა ტექსტი, დაასაბმიტა ფორმა).

## 1️⃣ `addEventListener` — მოვლენის მოსმენა

```javascript
const button = document.querySelector("#myBtn");

button.addEventListener("click", (event) => {
  console.log("ღილაკს დააჭირეს!");
  console.log(event.target); // აბრუნებს ელემენტს, რომელსაც დააჭირეს
});
```

## 2️⃣ ხშირად გამოყენებადი Event-ები

### ა) `submit` (ფორმის გაგზავნა) & `preventDefault()`

ფორმის დასაბმიტებისას ბრაუზერი ნაგულისხმევად ახდენს გვერდის დარეფრეშებას. ამის შესაჩერებლად ვიყენებთ `event.preventDefault()`-ს.

```javascript
const loginForm = document.querySelector("#login-form");

loginForm.addEventListener("submit", (e) => {
  e.preventDefault(); // არეგულირებს გვერდის გადატვირთვის აღკვეთას

  const emailInput = document.querySelector("#email").value;
  console.log("გაგზავნილი ემაილი:", emailInput);
});
```

### ბ) `keydown` / `keyup` (კლავიატურის მოვლენები)

```javascript
const searchInput = document.querySelector("#search");

searchInput.addEventListener("keydown", (e) => {
  if (e.key === "Enter") {
    console.log("ძებნა შესრულდა:", e.target.value);
  }
});
```

## 3️⃣ Event Bubbling და Event Delegation

### 🫧 Event Bubbling (მოვლენის ამოტივტივება)

როდესაც შვილობილ ელემენტზე ხდება event (მაგ. `click`), ის იწყება ამ ელემენტიდან და "ამოტივტივდება" ზევით, მისი ყველა მშობელი ელემენტის მიმართულებით.

```
[ document ]  ^ 3. ბოლოს აღწევს document-მდე
   [ div ]     | 2. გადაეცემა მშობელ div-ს
  [ button ]   * 1. Click ხდება უშუალოდ ღილაკზე
```

### 🎯 Event Delegation (მოვლენის დელეგირება)

Bubbling-ის თვისების გამოყენებით, თითოეულ შვილობილ ელემენტზე ცალ-ცალკე `addEventListener`-ით მოსმენის ნაცვლად, ერთ მოსმენას ვადებთ საერთო მშობელს.

ეს განსაკუთრებით გამოსადეგია **დინამიურად შექმნილი** ელემენტებისთვის!

```html
<ul id="todo-list">
  <li>დავალება 1 <button class="delete-btn">X</button></li>
  <li>დავალება 2 <button class="delete-btn">X</button></li>
</ul>
```

```javascript
const todoList = document.querySelector("#todo-list");

// მოსმენას ვადებთ მშობელ <ul>-ს
todoList.addEventListener("click", (e) => {
  // შევამოწმოთ, დააჭირეს თუ არა წაშლის ღილაკს
  if (e.target.classList.contains("delete-btn")) {
    const li = e.target.parentElement;
    li.remove(); // წაშლის შესაბამის <li> ელემენტს
  }
});
```

> 💡 **რატომ არის ეს ასეთი მნიშვნელოვანი?** თუ ღილაკებზე ცალ-ცალკე `addEventListener`-ს დავდებდით, მაშინ **ახლად, დინამიურად შექმნილ** `<li>`-ებზე listener საერთოდ არ იმუშავებდა — ისინი ხომ იმ დროს, როცა `addEventListener`-ს ვწერდით, ჯერ არც არსებობდნენ! Event Delegation ამ პრობლემას მთლიანად აგვარებს, რადგან მშობელი ელემენტი **ყოველთვის** ისმენს, მიუხედავად იმისა, როდის შეიქმნა შვილი.

---

## 🧪 რეალური პრაქტიკული პროექტი: Todo List (ინტერაქტიული აპლიკაცია)

გავაერთიანოთ DOM Manipulation და Event-ები პატარა ფუნქციონალურ აპლიკაციაში:

```html
<form id="todo-form">
  <input
    type="text"
    id="todo-input"
    placeholder="შეიყვანეთ დავალება..."
    required
  />
  <button type="submit">დამატება</button>
</form>

<ul id="todo-list"></ul>
```

```javascript
const form = document.querySelector("#todo-form");
const input = document.querySelector("#todo-input");
const list = document.querySelector("#todo-list");

// 1. ახალი დავალების დამატება
form.addEventListener("submit", (e) => {
  e.preventDefault();

  const taskText = input.value;

  // შევქმნათ ახალი li ელემენტი
  const li = document.createElement("li");
  li.innerHTML = `
    <span>${taskText}</span>
    <button class="delete-btn">წაშლა</button>
  `;

  list.appendChild(li);
  input.value = ""; // გავასუფთაოთ ინპუტი
});

// 2. დავალების წაშლა (Event Delegation-ით)
list.addEventListener("click", (e) => {
  if (e.target.classList.contains("delete-btn")) {
    e.target.parentElement.remove();
  }
});
```

---

## 🏠 საშინაო დავალება (Homework)

### დავალება 1 (Theme Toggle / Modal):

შექმენით ღილაკი `<button id="theme-btn">`. მასზე კლიკისას `document.body`-ს `classList.toggle()`-ით დაემატოს ან მოეხსნას კლასი `.dark-mode` (რომელიც CSS-ში შეცვლის ფონისა და ტექსტის ფერს).

### დავალება 2 (Interactive Counter):

შექმენით 3 ელემენტი: ეკრანზე ციფრი `0` და ორი ღილაკი `+` და `-`.

- `+`-ზე კლიკისას რიცხვი გაიზარდოს 1-ით.
- `-`-ზე კლიკისას შემცირდეს 1-ით (არ უნდა ჩამოვიდეს 0-ის ქვემოთ).
- თუ რიცხვი 10-ზე მეტია, ტექსტის ფერი გახდეს მწვანე!

### დავალება 3 (Dynamic Card Filter — Event Delegation):

შექმენით პროდუქტების სიის ბარათები. თითოეულ ბარათს ჰქონდეს წაშლისა და "Favorite"-ის ღილაკები. Event Delegation-ის გამოყენებით უზრუნველყავით, რომ მშობელ კონტეინერზე დაჭერისას წაშლის ღილაკმა წაშალოს ბარათი, ხოლო Favorite ღილაკმა შეცვალოს ბარათის ფონი.
