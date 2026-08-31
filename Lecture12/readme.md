# ⏳ Web Development / Lecture 12: Asynchronous JavaScript — Promises, `async/await` და Fetch API

## 📌 ლექციის მიმოხილვა

აქამდე ჩვენ მიერ დაწერილი ყველა კოდი სრულდებოდა **სინქრონულად** — ერთი ხაზი მეორის მიყოლებით, თანმიმდევრობით, დაუყოვნებლივ. დღეს ვისწავლით, როგორ ვმართოთ ისეთი ოპერაციები, რომლებიც **დროში გაწელილია** — სერვერიდან მონაცემების წამოღება, ფაილის ჩატვირთვა, დროის დაყოვნება — ისე, რომ მთელი გვერდი არ "გაიყინოს" ლოდინის დროს.

- **Promises:** JavaScript-ის ინსტრუმენტი ასინქრონული ოპერაციის შედეგის "დაპირებისთვის".
- **`async`/`await`:** თანამედროვე, სუფთა სინტაქსი Promise-ებთან მუშაობისთვის.
- **Fetch API:** ბრაუზერის ჩაშენებული ინსტრუმენტი სერვერიდან (API-დან) მონაცემების წამოსაღებად.

---

## 🧭 თემების სია

1. სინქრონული vs ასინქრონული კოდი
2. Callback-ები და "Callback Hell"
3. Promises — მდგომარეობები, `.then()`, `.catch()`, `.finally()`
4. Promise Chaining
5. `async`/`await` — თანამედროვე სინტაქსი
6. Error Handling — `try`/`catch`
7. `Promise.all()` — პარალელური მოთხოვნები
8. Fetch API — GET და POST მოთხოვნები
9. რეალური მაგალითი — API-დან მონაცემების წამოღება და DOM-ზე ჩვენება
10. საშინაო დავალება

---

## 1️⃣ სინქრონული vs ასინქრონული კოდი

JavaScript **სინგლ-thread** ენაა — ერთდროულად მხოლოდ ერთ ბრძანებას ასრულებს. თუ რომელიმე ოპერაცია დროში გაწელილია (მაგ. სერვერიდან პასუხის ლოდინი, რასაც შეიძლება წამები დასჭირდეს), და ის **სინქრონულად** შესრულდებოდა, მთელი ბრაუზერი გაიყინებოდა ამ ლოდინის განმავლობაში — ღილაკებზე დაჭერაც კი არ იმუშავებდა.

```javascript
console.log("1");
console.log("2");
console.log("3");
// Output ყოველთვის: 1, 2, 3 — თანმიმდევრობით
```

ასინქრონული ოპერაციები კი "გვერდზე" გაქვს გატანილი — კოდი აგრძელებს შესრულებას, და ასინქრონული ოპერაციის შედეგი მზადდება "ფონში", მზადყოფნისთანავე კი უბრუნდება ჩვენს კოდს.

```javascript
console.log("1");

setTimeout(() => {
  console.log("2 (დაგვიანებული)");
}, 1000);

console.log("3");

// Output: 1, 3, 2 (დაგვიანებული) — თანმიმდევრობა შეიცვალა!
```

> 💡 **მთავარი აღმოჩენა:** `setTimeout`-ის შიგნით არსებული კოდი **არ** აჩერებს დანარჩენი პროგრამის შესრულებას — JavaScript გადადის შემდეგ ხაზზე დაუყოვნებლივ, და `setTimeout`-ის callback მხოლოდ მითითებული დროის შემდეგ სრულდება.

---

## 2️⃣ Callback-ები და "Callback Hell"

თავდაპირველად, ასინქრონული ოპერაციების "დასრულების შემდეგ რა უნდა მოხდეს" აღწერას **callback ფუნქციებით** აკეთებდნენ. პრობლემა ისაა, რომ თუ ერთი ასინქრონული ოპერაცია მეორეზეა დამოკიდებული, callback-ები ერთმანეთში "ჩალაგდება":

```javascript
getUser(userId, (user) => {
  getOrders(user.id, (orders) => {
    getOrderDetails(orders[0].id, (details) => {
      console.log(details);
      // და ასე შემდეგ... ეს "პირამიდა" კიდევ იზრდება
    });
  });
});
```

ამას **Callback Hell** ჰქვია — კოდი მარჯვნივ "მიცოცავს", ძნელად წასაკითხავია, და error-ების მართვა კოშმარად იქცევა. სწორედ ამ პრობლემის მოსაგვარებლად შემოვიდა Promises.

---

## 3️⃣ Promises — რა არის Promise?

**Promise** არის ობიექტი, რომელიც წარმოადგენს ასინქრონული ოპერაციის **მომავალ შედეგს**. მას აქვს 3 შესაძლო მდგომარეობა:

| მდგომარეობა   | მნიშვნელობა                                       |
| :------------ | :------------------------------------------------ |
| **Pending**   | ოპერაცია ჯერ არ დასრულებულა (საწყისი მდგომარეობა) |
| **Fulfilled** | ოპერაცია წარმატებით დასრულდა — შედეგი მზადაა      |
| **Rejected**  | ოპერაცია წარუმატებლად დასრულდა — მოხდა error      |

```javascript
const myPromise = new Promise((resolve, reject) => {
  const success = true;

  setTimeout(() => {
    if (success) {
      resolve("მონაცემები წარმატებით მოვიდა!");
    } else {
      reject("დაფიქსირდა შეცდომა!");
    }
  }, 1000);
});
```

### `.then()`, `.catch()`, `.finally()`

Promise-ის შედეგის "მოსაცდელად" ვიყენებთ ამ სამ მეთოდს:

```javascript
myPromise
  .then((result) => {
    console.log("წარმატება:", result); // თუ resolve-მა გაითამაშა
  })
  .catch((error) => {
    console.log("შეცდომა:", error); // თუ reject-მა გაითამაშა
  })
  .finally(() => {
    console.log("ეს ყოველთვის სრულდება, შედეგის მიუხედავად");
  });
```

---

## 4️⃣ Promise Chaining

`.then()` თავადაც აბრუნებს ახალ Promise-ს — რაც საშუალებას გვაძლევს, ერთმანეთზე "ჯაჭვად" მივაბათ, Callback Hell-ის ღრმა ჩალაგების ნაცვლად:

```javascript
getUser(userId)
  .then((user) => getOrders(user.id))
  .then((orders) => getOrderDetails(orders[0].id))
  .then((details) => console.log(details))
  .catch((error) => console.log("სადმე დაფიქსირდა შეცდომა:", error));
```

> 💡 ერთი `.catch()` საკმარისია მთელი ჯაჭვისთვის — თუ ჯაჭვში ნებისმიერ ეტაპზე რამე "ჩავარდება" (reject-დება), კონტროლი პირდაპირ უახლოეს `.catch()`-ს გადაეცემა, შუალედური `.then()`-ების გამოტოვებით.

---

## 5️⃣ `async`/`await` — თანამედროვე სინტაქსი

`async`/`await` არის Promise-ების **"სინტაქსური შაქარი"** (Syntactic Sugar) — იგივე ფუნქციონალობა, მაგრამ ისეთი კოდი, რომელიც სინქრონულს ჰგავს და ბევრად უფრო კითხვადია.

```javascript
// Promise Chaining ვერსია:
function loadUserData(userId) {
  return getUser(userId)
    .then((user) => getOrders(user.id))
    .then((orders) => console.log(orders));
}

// async/await ვერსია — იგივე ლოგიკა:
async function loadUserData(userId) {
  const user = await getUser(userId);
  const orders = await getOrders(user.id);
  console.log(orders);
}
```

### წესები, რომლებიც უნდა დაიმახსოვროთ:

- `await` **მხოლოდ** `async` ფუნქციის შიგნით შეიძლება გამოვიყენოთ.
- `async` ფუნქცია **ყოველთვის** Promise-ს აბრუნებს, თუნდაც ჩვეულებრივი მნიშვნელობა დააბრუნოთ `return`-ით.
- `await` "აჩერებს" **მხოლოდ** იმ კონკრეტული `async` ფუნქციის შესრულებას — არა მთელ პროგრამას.

---

## 6️⃣ Error Handling — `try`/`catch`

`async`/`await`-თან ერთად, `.catch()`-ის ნაცვლად, ჩვეულებრივ `try`/`catch` ბლოკს ვიყენებთ:

```javascript
async function loadUserData(userId) {
  try {
    const user = await getUser(userId);
    const orders = await getOrders(user.id);
    console.log(orders);
  } catch (error) {
    console.log("დაფიქსირდა შეცდომა:", error);
  } finally {
    console.log("ოპერაცია დასრულდა");
  }
}
```

> ⚠️ **კრიტიკული დეტალი:** თუ `try`/`catch`-ს დაგავიწყდებათ `async` ფუნქციაში, და `await`-ის ოპერაცია "ჩავარდება" — მიიღებთ **Unhandled Promise Rejection**-ს, რაც production-ში სერიოზული ბაგია.

---

## 7️⃣ `Promise.all()` — პარალელური მოთხოვნები

თუ რამდენიმე ასინქრონული ოპერაცია ერთმანეთზე **არ** არის დამოკიდებული, `await`-ის თანმიმდევრობითი გამოყენება დროის ფლანგვაა — უმჯობესია, ერთდროულად, პარალელურად გავუშვათ:

```javascript
// ❌ ნელი — თანმიმდევრობითი (თუ თითოეული 1 წამს იღებს, ჯამში 3 წამი)
const users = await getUsers();
const products = await getProducts();
const orders = await getOrders();

// ✅ სწრაფი — პარალელური (ჯამში ~1 წამი, ყველაზე ნელი მოთხოვნის ტოლი)
const [users, products, orders] = await Promise.all([
  getUsers(),
  getProducts(),
  getOrders(),
]);
```

> ⚠️ `Promise.all()` "ჩავარდება" (reject), თუ **თუნდაც ერთი** მოცემული Promise ჩავარდება — მთელი ჯგუფის შედეგი მაშინვე უარყოფილია.

---

## 8️⃣ Fetch API — მონაცემების წამოღება

`fetch()` არის ბრაუზერის ჩაშენებული ფუნქცია HTTP მოთხოვნების გასაგზავნად. ის **Promise-ს აბრუნებს**.

### GET მოთხოვნა:

```javascript
fetch("https://jsonplaceholder.typicode.com/users")
  .then((response) => response.json()) // Response ობიექტიდან რეალურ მონაცემებზე გადასვლა
  .then((data) => console.log(data))
  .catch((error) => console.log("Fetch-ის შეცდომა:", error));
```

> 💡 **რატომ ორი `.then()`?** `fetch()`-ის პირველი Promise დასრულდება, როგორც კი სერვერიდან **პასუხის სათაურები** მოვა — არა აუცილებლად სრული მონაცემი. `response.json()` **თავადაც** Promise-ს აბრუნებს, რომელიც სრულდება, როცა მთელი body წაკითხული და JSON-ად დაპარსილია.

### იგივე, `async`/`await`-ით (გაცილებით სუფთა):

```javascript
async function loadUsers() {
  try {
    const response = await fetch("https://jsonplaceholder.typicode.com/users");
    const data = await response.json();
    console.log(data);
  } catch (error) {
    console.log("Fetch-ის შეცდომა:", error);
  }
}
```

### ⚠️ Fetch-ის "ხაფანგი" — HTTP Error-ები `catch`-ს არ ააქტიურებს!

```javascript
async function loadData() {
  const response = await fetch("https://api.example.com/not-found");

  // response.ok არის false, თუ სტატუსი 400+ ან 500+ არის
  if (!response.ok) {
    throw new Error(`HTTP Error: ${response.status}`);
  }

  const data = await response.json();
  return data;
}
```

> ⚠️ **კრიტიკული დეტალი:** `fetch()` **მხოლოდ** ქსელური პრობლემის დროს (ინტერნეტი გაწყდა, DNS ვერ იპოვა) ააქტიურებს `catch`-ს. 404 ან 500 სტატუსიც კი **წარმატებულ** Promise-დ ითვლება Fetch-ისთვის! ამიტომ `response.ok`-ის ხელით შემოწმება აუცილებელია.

### POST მოთხოვნა (მონაცემების გაგზავნა):

```javascript
async function createPost(title, body) {
  const response = await fetch("https://jsonplaceholder.typicode.com/posts", {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify({ title, body, userId: 1 }),
  });

  const data = await response.json();
  return data;
}
```

---

## 🧪 9. რეალური მაგალითი — API-დან მონაცემების წამოღება და DOM-ზე ჩვენება

გავაერთიანოთ დღეს ნასწავლი წინა ლექციის (DOM Manipulation) ცოდნასთან:

```javascript
const userList = document.querySelector("#user-list");
const loadingText = document.querySelector("#loading");

async function loadAndDisplayUsers() {
  loadingText.textContent = "იტვირთება...";

  try {
    const response = await fetch("https://jsonplaceholder.typicode.com/users");

    if (!response.ok) {
      throw new Error(`HTTP Error: ${response.status}`);
    }

    const users = await response.json();

    loadingText.textContent = "";
    userList.innerHTML = ""; // წავშალოთ ძველი შიგთავსი

    users.forEach((user) => {
      const li = document.createElement("li");
      li.innerHTML = `<strong>${user.name}</strong> — ${user.email}`;
      userList.appendChild(li);
    });
  } catch (error) {
    loadingText.textContent = `შეცდომა: ${error.message}`;
  }
}

loadAndDisplayUsers();
```

---

## 📌 10. Quick Cheatsheet

| სინტაქსი                                  | განმარტება                                      |
| :---------------------------------------- | :---------------------------------------------- |
| `new Promise((resolve, reject) => {...})` | ახალი Promise-ის შექმნა                         |
| `.then(result => {...})`                  | წარმატებული შედეგის დამუშავება                  |
| `.catch(error => {...})`                  | შეცდომის დამუშავება                             |
| `async function name() {...}`             | ფუნქცია, რომელიც Promise-ს აბრუნებს             |
| `await promiseValue`                      | Promise-ის შედეგის მოლოდინი (`async` ფუნქციაში) |
| `try { } catch (e) { }`                   | Error Handling `async/await`-თან                |
| `Promise.all([p1, p2])`                   | პარალელური მოთხოვნები                           |
| `fetch(url)`                              | HTTP მოთხოვნა, Promise-ს აბრუნებს               |
| `response.json()`                         | Response body-ის JSON-ად დაპარსვა               |
| `response.ok`                             | `true`, თუ სტატუსი 200-299-ის შუალედშია         |

---

## 🏠 საშინაო დავალება (Homework)

### დავალება 1 (Fetch + DOM):

გამოიყენეთ [JSONPlaceholder API](https://jsonplaceholder.typicode.com/posts) და წამოიღეთ პირველი 10 პოსტი. თითოეული პოსტის სათაური და ტექსტი აჩვენეთ ცალკეულ ბარათებად გვერდზე (`document.createElement` + `appendChild`-ის გამოყენებით).

### დავალება 2 (Error Handling):

დაწერეთ `async` ფუნქცია `fetchUserSafely(id)`, რომელიც:

- fetch-ავს კონკრეტულ მომხმარებელს `https://jsonplaceholder.typicode.com/users/{id}`-დან.
- თუ `id` არასწორია (მაგ. `999`), API 404-ს დააბრუნებს — დაიჭირეთ ეს `response.ok`-ის შემოწმებით და ეკრანზე აჩვენეთ მკაფიო შეცდომის შეტყობინება, "ჩამოშლის" ნაცვლად.

### დავალება 3 (`Promise.all` + POST):

შექმენით ღილაკი "შექმენი და წამოიღე", რომელიც ერთდროულად (`Promise.all()`-ის გამოყენებით):

- POST მოთხოვნით ქმნის ახალ პოსტს (`https://jsonplaceholder.typicode.com/posts`-ზე).
- GET მოთხოვნით წამოიღებს არსებული პოსტების სიას.

ორივე შედეგი ერთდროულად აჩვენეთ Console-ში, როცა ორივე დასრულდება.
