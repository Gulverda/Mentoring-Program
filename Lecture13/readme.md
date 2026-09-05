# 💾 Web Development / Lecture 13: LocalStorage / SessionStorage & JS Mini-Project

## 📌 ლექციის მიმოხილვა

აქამდე ნასწავლი ყველაფერი — ცვლადები, DOM, Fetch — ქრება, როგორც კი გვერდს გადავტვირთავთ (refresh). დღეს ვისწავლით, როგორ **შევინახოთ** მონაცემები პირდაპირ მომხმარებლის ბრაუზერში, გვერდის დახურვის შემდეგაც კი — და ამის საფუძველზე ავაშენებთ ჩვენს პირველ **სრულყოფილ, დამოუკიდებელ Mini-Project**-ს.

- **Web Storage API:** `localStorage` და `sessionStorage` — მონაცემების შენახვა ბრაუზერში.
- **Mini-Project:** ყველა აქამდე ნასწავლი კონცეფციის (DOM, Events, Fetch, Storage) გაერთიანება ერთ, რეალურ აპლიკაციაში.

---

## 🧭 თემების სია

1. რატომ გვჭირდება Web Storage
2. `localStorage` vs `sessionStorage`
3. საბაზო API — `setItem`, `getItem`, `removeItem`, `clear`
4. რთული მონაცემების შენახვა — `JSON.stringify` / `JSON.parse`
5. ხშირი ხაფანგები
6. Mini-Project ვარიანტი A — Todo App სრული Persistence-ით
7. Mini-Project ვარიანტი B — Weather App (Fetch + Storage Caching)
8. Cheatsheet
9. საშინაო დავალება / პროექტის მოთხოვნები

---

## 1️⃣ რატომ გვჭირდება Web Storage?

მოდი, ჯერ ვნახოთ პრობლემა, რომელსაც დღეს ვხსნით:

```javascript
let taskCount = 5;
// ... გვერდს ვარეფრეშებთ (F5) ...
console.log(taskCount); // ❌ Error — taskCount საერთოდ აღარ არსებობს!
```

ყოველი გვერდის refresh, ან ბრაუზერის დახურვა, **მთლიანად შლის** JavaScript-ის მეხსიერებაში არსებულ ყველა ცვლადს. თუ გვინდა, რომ მომხმარებლის Todo List, პარამეტრები, ან "დაფავორითებული" ნივთები **გადარჩნენ** გვერდის დახურვის შემდეგაც — გვჭირდება მონაცემების შენახვა თავად ბრაუზერში, არა მხოლოდ JS-ის დროებით მეხსიერებაში.

სწორედ ამისთვის არსებობს **Web Storage API** — `localStorage` და `sessionStorage`.

---

## 2️⃣ `localStorage` vs `sessionStorage`

| თვისება                           | `localStorage`                                      | `sessionStorage`                                              |
| :-------------------------------- | :-------------------------------------------------- | :------------------------------------------------------------ |
| **რამდენ ხანს ინახება**           | სამუდამოდ, ხელით წაშლამდე                           | მხოლოდ, სანამ **tab** ღიაა                                    |
| **გამოიყენება ტაბებს შორის?**     | ✅ კი, ყველა ტაბზე იზიარება                         | ❌ არა, თითო ტაბს თავისი აქვს                                 |
| **გადარჩება ბრაუზერის დახურვას?** | ✅ კი                                               | ❌ არა                                                        |
| **ტიპური გამოყენება**             | Theme პარამეტრი, Todo List, "დამახსოვრებული" ლოგინი | ერთჯერადი ფორმის მონაცემი, Multi-step ვიზარდის დროებითი state |

> 💡 **ორივეს აქვს იდენტური API** — განსხვავება მხოლოდ იმაშია, **რამდენ ხანს** ინახება მონაცემი, არა როგორ ვამუშავებთ მას.

---

## 3️⃣ საბაზო API

```javascript
// შენახვა
localStorage.setItem("username", "Luka");

// წაკითხვა
const username = localStorage.getItem("username");
console.log(username); // "Luka"

// კონკრეტული key-ს წაშლა
localStorage.removeItem("username");

// მთლიანად, ყველაფრის წაშლა
localStorage.clear();

// key-ს არარსებობის შემოწმება
console.log(localStorage.getItem("nonExistentKey")); // null
```

`sessionStorage`-ს ზუსტად იგივე მეთოდები აქვს — უბრალოდ `localStorage`-ს ნაცვლად `sessionStorage`-ს წერთ.

---

## 4️⃣ რთული მონაცემების შენახვა — `JSON.stringify` / `JSON.parse`

> ⚠️ **კრიტიკული შეზღუდვა:** `localStorage`/`sessionStorage` ინახავს **მხოლოდ ტექსტს (String)**. თუ მასივს ან ობიექტს პირდაპირ შეინახავთ, JavaScript მას ავტომატურად, ხშირად არასწორად, ტექსტად გარდაქმნის.

```javascript
const tasks = [
  { id: 1, text: "Buy milk" },
  { id: 2, text: "Walk the dog" },
];

// ❌ ცუდი — ობიექტი პირდაპირ ტექსტად გარდაიქმნება
localStorage.setItem("tasks", tasks);
console.log(localStorage.getItem("tasks")); // "[object Object],[object Object]" — მონაცემი დაკარგულია!

// ✅ სწორი — JSON.stringify გარდაქმნის ობიექტს ვალიდურ JSON ტექსტად
localStorage.setItem("tasks", JSON.stringify(tasks));

// წაკითხვისას — JSON.parse აბრუნებს ტექსტს ისევ რეალურ მასივად/ობიექტად
const savedTasks = JSON.parse(localStorage.getItem("tasks"));
console.log(savedTasks); // [{ id: 1, text: "Buy milk" }, ...] — რეალური მასივია!
```

> 💡 **ოქროს წესი:** `localStorage`-ში ჩაწერამდე — `JSON.stringify()`. `localStorage`-დან ამოღების შემდეგ — `JSON.parse()`. ეს წყვილი თითქმის ყოველთვის ერთად მოგზაურობს.

---

## 5️⃣ ხშირი ხაფანგები

```javascript
// ⚠️ ხაფანგი 1 — JSON.parse(null) Error-ს იძლევა
const data = JSON.parse(localStorage.getItem("nonExistentKey")); // ❌ SyntaxError

// ✅ გამოსწორება — default მნიშვნელობა Nullish Coalescing-ით
const data2 = JSON.parse(localStorage.getItem("nonExistentKey") ?? "[]");
console.log(data2); // [] — უსაფრთხოდ
```

- **ტევადობის ლიმიტი:** `localStorage`-ს აქვს დაახლოებით 5-10MB ლიმიტი (ბრაუზერის მიხედვით) — დიდი ფაილების ან სურათების შესანახად არ გამოდგება.
- **Same-Origin Policy:** თითოეულ დომენს თავისი, სხვებისგან იზოლირებული storage აქვს — `example.com`-ს `localStorage` ვერასდროს "დაინახავს" `other-site.com`-ის მონაცემებს.
- **სინქრონულია:** `localStorage`-ის ოპერაციები სინქრონულია (არა Promise-ზე დაფუძნებული) — დიდი მოცულობის მონაცემისთვის (რთული აპლიკაციები) სჯობს IndexedDB, რასაც ცალკე, უფრო Advanced თემად შეისწავლით.

---

## 🧪 6. Mini-Project ვარიანტი A: Todo App სრული Persistence-ით

გავაფართოვოთ Lecture 15-16-ის Todo List, `localStorage`-ით.

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

// 1. მონაცემების წაკითხვა Storage-იდან გვერდის ჩატვირთვისას
let tasks = JSON.parse(localStorage.getItem("tasks") ?? "[]");

// 2. ფუნქცია — მთელი state-ის ხელახლა დახატვა DOM-ში
function renderTasks() {
  list.innerHTML = "";

  tasks.forEach((task, index) => {
    const li = document.createElement("li");
    li.innerHTML = `
      <span>${task.text}</span>
      <button class="delete-btn" data-index="${index}">წაშლა</button>
    `;
    list.appendChild(li);
  });
}

// 3. ფუნქცია — state-ის შენახვა Storage-ში
function saveTasks() {
  localStorage.setItem("tasks", JSON.stringify(tasks));
}

// 4. დამატება
form.addEventListener("submit", (e) => {
  e.preventDefault();

  tasks.push({ text: input.value });
  saveTasks();
  renderTasks();

  input.value = "";
});

// 5. წაშლა (Event Delegation)
list.addEventListener("click", (e) => {
  if (e.target.classList.contains("delete-btn")) {
    const index = Number(e.target.dataset.index);
    tasks.splice(index, 1); // წავშალოთ სწორი ინდექსით
    saveTasks();
    renderTasks();
  }
});

// 6. საწყისი დახატვა — გვერდის პირველივე ჩატვირთვისას
renderTasks();
```

> 💡 **მთავარი პატერნი, რომელიც აქ ისწავლეთ:** `tasks` მასივი არის ჩვენი **ერთადერთი "წყარო ჭეშმარიტების"** (Single Source of Truth). ყოველი ცვლილება (`push`, `splice`) მასივში ხდება, შემდეგ ვინახავთ (`saveTasks`) და ხელახლა ვხატავთ (`renderTasks`). ეს იგივე პრინციპია, რომელსაც React-ის msგავს framework-ებში, `state`-ის სახელით, მალე შეხვდებით.

---

## 🌦️ 7. Mini-Project ვარიანტი B: Weather App (Fetch + Storage Caching)

ალტერნატიული პროექტი, რომელიც Lecture 17-ის Fetch-ს დღევანდელ Storage-თან აერთიანებს.

**რეკომენდებული API:** [Open-Meteo](https://open-meteo.com/) — **უფასო, API Key-ს არ საჭიროებს**, იდეალურია სასწავლო პროექტისთვის.

```javascript
async function getWeather(lat, lon) {
  const url = `https://api.open-meteo.com/v1/forecast?latitude=${lat}&longitude=${lon}&current_weather=true`;

  const response = await fetch(url);
  if (!response.ok) throw new Error(`HTTP Error: ${response.status}`);

  const data = await response.json();
  return data.current_weather; // { temperature, windspeed, ... }
}

async function loadWeatherForCity(cityName, lat, lon) {
  try {
    const weather = await getWeather(lat, lon);

    // Caching — ბოლოს ნანახი ქალაქის შენახვა
    localStorage.setItem("lastCity", JSON.stringify({ cityName, lat, lon }));

    document.querySelector("#weather-output").textContent =
      `${cityName}: ${weather.temperature}°C, ქარი: ${weather.windspeed} კმ/სთ`;
  } catch (error) {
    document.querySelector("#weather-output").textContent =
      `შეცდომა: ${error.message}`;
  }
}

// გვერდის ჩატვირთვისას — თუ ბოლო ქალაქი Storage-ში დამახსოვრებულია, ავტომატურად ჩატვირთე
const lastCity = JSON.parse(localStorage.getItem("lastCity") ?? "null");
if (lastCity) {
  loadWeatherForCity(lastCity.cityName, lastCity.lat, lastCity.lon);
}
```

> 💡 ეს პატერნი — "დაიმახსოვრე ბოლო არჩევანი" — ერთ-ერთი ყველაზე ხშირად გამოყენებადი `localStorage`-ის შემთხვევაა რეალურ პროდუქტებში (ბოლო ვიზიტისას ღია ჩანართი, ბოლო ძებნილი პროდუქტი, და ა.შ.).

---

## 📌 8. Quick Cheatsheet

| სინტაქსი                           | განმარტება                                         |
| :--------------------------------- | :------------------------------------------------- |
| `localStorage.setItem(key, value)` | მონაცემის შენახვა (value ყოველთვის String)         |
| `localStorage.getItem(key)`        | მონაცემის წაკითხვა (აბრუნებს String-ს ან `null`-ს) |
| `localStorage.removeItem(key)`     | კონკრეტული key-ს წაშლა                             |
| `localStorage.clear()`             | ყველაფრის წაშლა                                    |
| `JSON.stringify(obj)`              | ობიექტი/მასივი → ტექსტი (შენახვამდე)               |
| `JSON.parse(str)`                  | ტექსტი → ობიექტი/მასივი (წაკითხვის შემდეგ)         |
| `JSON.parse(str ?? "[]")`          | უსაფრთხო წაკითხვა, `null`-ის დაცვით                |

---

## 🏠 9. საშინაო დავალება / პროექტის მოთხოვნები

აირჩიეთ **ერთი** პროექტი და დაასრულეთ სრულად:

### ვარიანტი A — Todo App

- ✅ დავალებების დამატება/წაშლა (უკვე ვისწავლეთ)
- ✅ `localStorage`-ში სრული Persistence — refresh-ის შემდეგაც დავალებები რჩება
- 🆕 დაამატეთ "დასრულებულის" მონიშვნა (checkbox ან `classList.toggle("done")`) — ესეც შენახული უნდა იყოს Storage-ში
- 🆕 დაამატეთ ღილაკი "წაშალე ყველა დასრულებული"

### ვარიანტი B — Weather App

- ✅ ქალაქის სახელით ამინდის მონაცემების წამოღება Open-Meteo API-დან
- ✅ ბოლო ნანახი ქალაქის `localStorage`-ში დამახსოვრება და გვერდის გახსნისას ავტომატური ჩატვირთვა
- 🆕 დაამატეთ "ფავორიტი ქალაქების" სია (მასივი `localStorage`-ში), საიდანაც სწრაფად გადართვა შეიძლება
- 🆕 დაამატეთ Loading state და შესაბამისი Error message, თუ ქალაქი ვერ მოიძებნა

**ორივე ვარიანტისთვის სავალდებულოა:**

1. სუფთა, სემანტიკური HTML სტრუქტურა.
2. მინიმუმ ერთი Event Delegation-ის გამოყენების შემთხვევა.
3. `try`/`catch` Error Handling ყველა `fetch`/`localStorage` ოპერაციაზე, სადაც შესაძლებელია მარცხი.
4. Git Commit-ების ისტორია, ლოგიკურ ეტაპებად დაყოფილი (არა ერთი დიდი commit ბოლოს).
5. GitHub Pages-ზე Live დეპლოიმენტი.
