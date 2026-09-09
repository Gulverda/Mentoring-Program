# 🧩 Web Development / Lecture 14: ფუნქციების მექანიკა & თანამედროვე კოლექციები

## 📌 ლექციის მიმოხილვა

წინა ლექციაზე ჩავიხედეთ JS Engine-ის შიგნით — Scope, Closures, Event Loop. დღეს ვამთავრებთ ფუნქციების სიღრმისეულ გაგებას (`this`-ის მართვა, "რთული" ფუნქციური პატერნები), ვსწავლობთ ტიპების არასწორად "აღრევის" თავიდან აცილებას, და ვეცნობით ორ ახალ, თანამედროვე კოლექციას — `Map`-სა და `Set`-ს — Array/Object-ის გვერდით.

- **`this`, `call`, `apply`, `bind`:** `this`-ის ხელით მართვა.
- **"რთული" ფუნქციები:** Higher-Order Functions, IIFE, Currying.
- **Pure vs Impure Functions:** კოდის პროგნოზირებადობა.
- **Type Casting/Coercion:** როცა JS თავად "გამოიცნობს" ტიპებს.
- **Loops:** `for`, `while`, `do-while`, `for...in`, `for...of`.
- **`Map`/`Set`/`WeakMap`/`WeakSet`:** თანამედროვე კოლექციები.

---

## 🧭 თემების სია

1. `this` — მოკლე გამეორება
2. `call()`, `apply()`, `bind()`
3. Higher-Order Functions (HOF)
4. IIFE (Immediately Invoked Function Expression)
5. Currying
6. Pure vs Impure Functions
7. Type Casting/Coercion
8. Loops in JS — შედარებითი მიმოხილვა
9. `Set` & `WeakSet`
10. `Map` & `WeakMap`

---

## `this` — მოკლე გამეორება

`this`-ის მნიშვნელობა დამოკიდებულია იმაზე, **როგორ** გამოიძახეთ ფუნქცია — არა სად დაწერეთ ის (განსხვავებით Lexical Scope-ისგან, რაც წინა ლექციაზე ვისწავლეთ).

```javascript
const user = {
  name: "Luka",
  greet() {
    console.log(this.name); // "Luka" — this = user (მეთოდის გამომძახებელი ობიექტი)
  },
};
user.greet();

function standalone() {
  console.log(this); // Global Object (ან undefined, strict mode-ში)
}
standalone();
```

---

## `call()`, `apply()`, `bind()`

ეს სამივე მეთოდი გვაძლევს საშუალებას, **ხელით** მივუთითოთ, რა უნდა იყოს `this` ფუნქციის შესრულებისას.

### `call()` — დაუყოვნებლივ გამოძახება, არგუმენტები ცალ-ცალკე:

```javascript
function introduce(city, country) {
  console.log(`მე ვარ ${this.name}, ${city}-დან, ${country}-დან`);
}

const person = { name: "Ana" };

introduce.call(person, "თბილისი", "საქართველო");
// "მე ვარ Ana, თბილისი-დან, საქართველო-დან"
```

### `apply()` — იგივე, რაც `call()`, მაგრამ არგუმენტები მასივის სახით:

```javascript
introduce.apply(person, ["თბილისი", "საქართველო"]);
// იგივე შედეგი
```

### `bind()` — არ იძახებს დაუყოვნებლივ, აბრუნებს **ახალ ფუნქციას** დამაგრებული `this`-ით:

```javascript
const introducePerson = introduce.bind(person);
introducePerson("თბილისი", "საქართველო"); // შეგვიძლია მოგვიანებით გამოვიძახოთ
```

> 💡 **პრაქტიკული გამოყენება:** `bind()` ხშირად გამოიყენება, როცა ფუნქციას Event Handler-ად გადასცემთ, მაგრამ გინდათ, რომ მას კონკრეტული `this` "ახსოვდეს", მიუხედავად იმისა, ვინ/რამ გამოიძახა.

---

## Higher-Order Functions (HOF)

**Higher-Order Function** არის ფუნქცია, რომელიც **იღებს** სხვა ფუნქციას არგუმენტად, ან **აბრუნებს** ფუნქციას. თქვენ უკვე იყენებდით მათ — `.map()`, `.filter()`, `.reduce()` ყველა HOF-ია!

```javascript
function createValidator(minLength) {
  return function (str) {
    // ფუნქცია, რომელიც ფუნქციას აბრუნებს
    return str.length >= minLength;
  };
}

const isValidPassword = createValidator(8);
console.log(isValidPassword("12345")); // false
console.log(isValidPassword("12345678")); // true

function repeat(n, action) {
  // ფუნქცია, რომელიც ფუნქციას იღებს
  for (let i = 0; i < n; i++) action(i);
}
repeat(3, (i) => console.log(`გამეორება #${i}`));
```

---

## IIFE (Immediately Invoked Function Expression)

**IIFE** არის ფუნქცია, რომელიც განისაზღვრება **და** მაშინვე, ავტომატურად სრულდება — ცალკე გამოძახების გარეშე.

```javascript
(function () {
  const secret = "ეს ცვლადი მხოლოდ აქ არსებობს";
  console.log("IIFE გაეშვა დაუყოვნებლივ!");
})();

// Arrow Function ვერსია:
(() => {
  console.log("Arrow IIFE");
})();
```

> 💡 **რატომ გამოიყენებოდა ისტორიულად:** მოდულების (`import`/`export`) გამოჩენამდე, IIFE იყო მთავარი გზა, რომ ცვლადები **არ** "დაბინძურებულიყო" გლობალურ Scope-ში — ყველაფერი IIFE-ის შიდა Scope-ში იმალებოდა. დღეს, Modules-ის დროს, ეს საჭიროება ნაკლებადაა აქტუალური, მაგრამ კოდში ხშირად შეხვდებით.

---

## Currying

**Currying** არის ტექნიკა, როცა ფუნქცია, რომელიც რამდენიმე არგუმენტს იღებს, გარდაიქმნება ფუნქციების ჯაჭვად, სადაც თითოეული **ერთ** არგუმენტს იღებს.

```javascript
// ჩვეულებრივი ფუნქცია:
function add(a, b, c) {
  return a + b + c;
}
console.log(add(1, 2, 3)); // 6

// Curried ვერსია:
function curriedAdd(a) {
  return function (b) {
    return function (c) {
      return a + b + c;
    };
  };
}
console.log(curriedAdd(1)(2)(3)); // 6

// Arrow Function-ით, გაცილებით მოკლედ:
const curriedAdd2 = (a) => (b) => (c) => a + b + c;
console.log(curriedAdd2(1)(2)(3)); // 6
```

> 💡 **პრაქტიკული გამოყენება:** Currying სასარგებლოა, როცა გინდათ "ნაწილობრივ შევსებული" ფუნქციები შექმნათ:
>
> ```javascript
> const addTen = curriedAdd2(10); // "დაპარამეტრებულია" 10-ით
> const addTenAndFive = addTen(5); // და კიდევ 5-ით
> console.log(addTenAndFive(3)); // 18
> ```

---

## Pure vs Impure Functions

**Pure Function** — ფუნქცია, რომელიც:

1. ერთი და იმავე Input-ისთვის **ყოველთვის** იგივე Output-ს აბრუნებს.
2. **არ** ცვლის (mutate) რაიმეს საკუთარი Scope-ის გარეთ (No Side Effects).

```javascript
// ✅ Pure Function
function add(a, b) {
  return a + b;
}

// ❌ Impure Function — გარეთა ცვლადს ცვლის (Side Effect)
let total = 0;
function addToTotal(amount) {
  total += amount; // გარეთა state-ს ცვლის!
  return total;
}

// ❌ Impure Function — Input-ის მიუხედავად, სხვადასხვა Output შეიძლება დააბრუნოს
function getRandomAndAdd(a) {
  return a + Math.random(); // შედეგი ყოველთვის განსხვავებულია!
}
```

> 💡 **რატომ არის მნიშვნელოვანი:** Pure Functions ბევრად უფრო **პროგნოზირებადი** და **ტესტვადია** — თუ ფუნქცია მხოლოდ თავის Input-ზეა დამოკიდებული, მისი ტესტირება მარტივია. React-ის msგავს ჩარჩოებში, კომპონენტების "Pure" წერა ერთ-ერთი მთავარი პრინციპია, რასაც მალე შეხვდებით.

---

## Type Casting / Coercion

**Type Coercion** არის ავტომატური (ან ხელით) ტიპის გარდაქმნა ერთი ტიპიდან მეორეში.

### Implicit Coercion (JS თავად აკეთებს, ხშირად მოულოდნელად):

```javascript
console.log("5" + 3); // "53" — Number "String"-ად გარდაიქმნა
console.log("5" - 3); // 2   — String "Number"-ად გარდაიქმნა (- ოპერატორს Concatenation არ აქვს)
console.log("5" * "2"); // 10  — ორივე Number-ად
console.log(true + 1); // 2   — true → 1
console.log(false + 1); // 1   — false → 0
```

### Explicit Casting (ხელით, გაცნობიერებულად):

```javascript
console.log(String(123)); // "123"
console.log(Number("123")); // 123
console.log(Number("abc")); // NaN
console.log(Boolean(0)); // false
console.log(Boolean("")); // false
console.log(Boolean("0")); // true! ⚠️ — არაცარიელი სტრინგია, თუნდაც "0" შეიცავდეს
```

> ⚠️ **ხაფანგი:** `+` ოპერატორი String-თან ერთად ყოველთვის **Concatenation**-ს (გაერთიანებას) აკეთებს, `-`/`*`/`/` კი ცდილობს **Number**-ად გარდაქმნას. ეს ასიმეტრია ხშირი დამაბნეველი წყაროა.

> 💡 **Best Practice:** ყოველთვის უპირატესობა მიანიჭეთ **Explicit Casting**-ს (`Number(x)`, `String(x)`) Implicit-ზე — კოდი უფრო პროგნოზირებადი და წასაკითხი ხდება.

---

## Loops in JS — შედარებითი მიმოხილვა

აქამდე ვიცნობდით `for`-სა და `forEach`-ს. დღეს ვამატებთ დანარჩენებს.

```javascript
// for — კლასიკური, სამნაწილიანი
for (let i = 0; i < 3; i++) {
  console.log("for:", i);
}

// while — მანამ, სანამ პირობა true-ა
let i = 0;
while (i < 3) {
  console.log("while:", i);
  i++;
}

// do-while — მინიმუმ ერთხელ სრულდება, პირობის შემოწმებამდეც კი
let j = 0;
do {
  console.log("do-while:", j);
  j++;
} while (j < 3);

// for...in — ობიექტის KEY-ებზე გადასავლელად
const user = { name: "Luka", age: 23 };
for (const key in user) {
  console.log("for...in key:", key, "value:", user[key]);
}

// for...of — ნებისმიერი Iterable-ის (Array, String, Map, Set) ELEMENT-ებზე
const colors = ["red", "green", "blue"];
for (const color of colors) {
  console.log("for...of:", color);
}
```

### 📊 როდის რომელი:

| Loop       | გამოყენება                                                |
| :--------- | :-------------------------------------------------------- |
| `for`      | ზუსტად ცნობილი რაოდენობის იტერაცია, ან ინდექსის საჭიროება |
| `while`    | უცნობი რაოდენობის იტერაცია, პირობაზეა დამოკიდებული        |
| `do-while` | საჭიროა **მინიმუმ ერთი** გაშვება, პირობის მიუხედავად      |
| `for...in` | ობიექტის **key**-ების დათვალიერება                        |
| `for...of` | მასივის/Map-ის/Set-ის **მნიშვნელობების** დათვალიერება     |

> ⚠️ **ხშირი შეცდომა:** `for...in`-ის გამოყენება **მასივზე** — ტექნიკურად მუშაობს (index-ებს დაგიბრუნებთ, როგორც string-ებს), მაგრამ არასწორი პრაქტიკაა. მასივებისთვის ყოველთვის `for...of` ან `.forEach()`.

---

## `Set` & `WeakSet`

**`Set`** არის კოლექცია **უნიკალური** მნიშვნელობებისთვის — დუბლიკატები ავტომატურად გაუქმდება.

```javascript
const uniqueNumbers = new Set([1, 2, 2, 3, 3, 3]);
console.log(uniqueNumbers); // Set(3) {1, 2, 3}

uniqueNumbers.add(4);
uniqueNumbers.add(1); // უკვე არსებობს, არაფერი შეიცვლება

console.log(uniqueNumbers.has(2)); // true
console.log(uniqueNumbers.size); // 4 (არა .length!)

uniqueNumbers.delete(1);

// გადასვლა for...of-ით:
for (const num of uniqueNumbers) {
  console.log(num);
}

// პრაქტიკული გამოყენება — მასივიდან დუბლიკატების მოშორება:
const numbersWithDuplicates = [1, 2, 2, 3, 4, 4, 5];
const unique = [...new Set(numbersWithDuplicates)];
console.log(unique); // [1, 2, 3, 4, 5]
```

**`WeakSet`** — `Set`-ის msგავსია, მაგრამ:

- ინახავს **მხოლოდ ობიექტებს** (არა Primitives).
- "სუსტად" მიუთითებს — თუ ობიექტზე სხვაგან წვდომა აღარსად არსებობს, Garbage Collector-ს შეუძლია მისი წაშლა, თუნდაც `WeakSet`-ში "იყოს".
- არ არის Iterable (არ შეგიძლიათ `for...of`-ით გადავლა) — ეს გამიზნულია, Memory-ის ოპტიმიზაციისთვის.

---

## `Map` & `WeakMap`

**`Map`** ჰგავს Object-ს (Key-Value წყვილები), მაგრამ რამდენიმე მნიშვნელოვანი უპირატესობით.

```javascript
const userRoles = new Map();

userRoles.set("Luka", "Admin");
userRoles.set("Ana", "Editor");
userRoles.set(42, "ეს key რიცხვია!"); // Map-ში KEY შეიძლება იყოს ნებისმიერი ტიპი!

console.log(userRoles.get("Luka")); // "Admin"
console.log(userRoles.has("Ana")); // true
console.log(userRoles.size); // 3 (არა .length!)

userRoles.delete("Ana");

// გადასვლა:
for (const [key, value] of userRoles) {
  console.log(key, "→", value);
}
```

### 📊 `Map` vs `Object` — რატომ ავირჩიოთ `Map`?

| თვისება               | `Object`                              | `Map`                                         |
| :-------------------- | :------------------------------------ | :-------------------------------------------- |
| **Key-ის ტიპი**       | მხოლოდ String/Symbol                  | ნებისმიერი ტიპი (Object, Function, Number...) |
| **ზომის მიღება**      | `Object.keys(obj).length`             | `map.size` — პირდაპირ                         |
| **Iteration წესრიგი** | არ არის გარანტირებული ისტორიულად      | **ყოველთვის** insertion-წესრიგში              |
| **Performance**       | უკეთესია მცირე, სტატიკურ სტრუქტურებზე | უკეთესია ხშირი add/delete-ის დროს             |

**`WeakMap`** — `Map`-ის msგავსია, მაგრამ Key **მხოლოდ ობიექტი** შეიძლება იყოს, და "სუსტად" მიუთითებს (Garbage Collection-ს არ უშლის ხელს) — გამოსადეგია, როცა გინდათ ობიექტს "მიაბათ" დამატებითი მონაცემი, მისი სიცოცხლის ციკლზე გავლენის გარეშე.

---

## Pass by Value vs Pass by Reference

JavaScript-ში ცვლადის "გადაცემის" ქცევა დამოკიდებულია მონაცემის **ტიპზე**.

### Primitives → Pass by Value (ასლი):

```javascript
let a = 10;
let b = a; // b იღებს a-ს მნიშვნელობის ასლს
b = 20;

console.log(a); // 10 — a უცვლელია!
console.log(b); // 20
```

### Objects/Arrays → Pass by Reference (მისამართი):

```javascript
let obj1 = { value: 10 };
let obj2 = obj1; // obj2 იღებს obj1-ის მისამართს მეხსიერებაში, არა ასლს!
obj2.value = 20;

console.log(obj1.value); // 20 — obj1-იც შეიცვალა!
console.log(obj2.value); // 20
```

ეს იგივე ლოგიკაა, რაც ფუნქციის პარამეტრებზეც ვრცელდება:

```javascript
function updateValue(num) {
  num = 100;
}
function updateObject(obj) {
  obj.value = 100;
}

let myNum = 5;
let myObj = { value: 5 };

updateValue(myNum);
updateObject(myObj);

console.log(myNum); // 5 — უცვლელია (Value)
console.log(myObj.value); // 100 — შეიცვალა! (Reference)
```

> 💡 **კავშირი Lecture 13-14-თან:** გახსოვთ Spread Operator-ის "Shallow Copy" ხაფანგი? ეს ზუსტად ამ Pass by Reference ლოგიკის შედეგია — Spread ქმნის ახალ ობიექტს ერთ დონეზე, მაგრამ ჩადგმული ობიექტები კვლავ **იმავე მისამართზე** მიუთითებენ.

---

## 📌 Quick Cheatsheet

| სინტაქსი                | განმარტება                                        |
| :---------------------- | :------------------------------------------------ |
| `fn.call(obj, a, b)`    | `this = obj`-ით გამოძახება, არგუმენტები ცალ-ცალკე |
| `fn.apply(obj, [a, b])` | იგივე, არგუმენტები მასივად                        |
| `fn.bind(obj)`          | აბრუნებს ახალ ფუნქციას, დამაგრებული `this`-ით     |
| HOF                     | ფუნქცია, რომელიც ფუნქციას იღებს/აბრუნებს          |
| IIFE                    | `(function(){...})()` — დაუყოვნებელი გამოძახება   |
| Currying                | `a => b => c => ...`                              |
| Pure Function           | იგივე Input → იგივე Output, არანაირი Side Effect  |
| `for...in`              | ობიექტის **key**-ები                              |
| `for...of`              | Iterable-ის **მნიშვნელობები**                     |
| `new Set([...])`        | უნიკალური მნიშვნელობები                           |
| `new Map()`             | Key-Value, ნებისმიერი Key ტიპით                   |

---

## 🏠 საშინაო დავალება (Homework)

### დავალება 1 (`bind`):

შექმენით ობიექტი `car = { brand: "Audi", start() { console.log(`${this.brand} სტარტავს`); } }`. ამოიღეთ `start` მეთოდი ცალკე ცვლადში (`const startFn = car.start;`) და გამოიძახეთ — შეამჩნევთ, რომ `this` "იკარგება". გამოასწორეთ `bind()`-ის გამოყენებით.

### დავალება 2 (Currying):

დაწერეთ `curriedMultiply` ფუნქცია (`a => b => c => a * b * c`) და გამოიყენეთ ის, რომ შექმნათ `double = curriedMultiply(2)` და `doubleAndTriple = double(3)`.

### დავალება 3 (`Set`/`Map`):

გაქვთ მასივი ელფოსტების სიით, სადაც დუბლიკატებია: `["a@mail.com", "b@mail.com", "a@mail.com"]`. `Set`-ის გამოყენებით, გამოასუფთავეთ დუბლიკატები. შემდეგ, `Map`-ის გამოყენებით, დაითვალეთ, რამდენჯერ გვხვდება თითოეული ელფოსტა ორიგინალ (დუბლიკატებიან) მასივში.
