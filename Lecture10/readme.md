# 📦 Web Development / Lecture 10: მონაცემთა სტრუქტურები — მასივები (Arrays) და ობიექტები (Objects)

## 📌 ლექციის მიმოხილვა

დღევანდელ გაერთიანებულ ლექციაზე ვისწავლით მონაცემთა ორი უმნიშვნელოვანესი არაპრიმიტიული სტრუქტურის მართვას:

- **მასივები (Arrays) & ციკლები:** მონაცემთა სიების შენახვა, დამუშავება და ტრანსფორმაცია ხშირად გამოყენებადი მეთოდებით (`map`, `filter`, `reduce`).
- **ობიექტები (Objects):** Key-Value წყვილების სტრუქტურირება, თანამედროვე სინტაქსი (Destructuring, Spread/Rest).

> 💡 ამ ლექციის ბოლოს დამატებით სექციად გამოტანილია რამდენიმე **დამატებითი, ხშირად ერთად გამოყენებადი** ინსტრუმენტი (`find`, `some`, `every`, `sort`, `Object.keys/values/entries`, Optional Chaining, Nullish Coalescing) — ორიგინალურ მასალას არ ცვლის, უბრალოდ ავსებს, რადგან პრაქტიკაში თითქმის ყოველთვის ერთად გამოიყენება.

---

# 🧭 თემების სრული სია

**Part 1 — Arrays:**

1. მასივის საფუძვლები (ინდექსები, `length`)
2. `for` vs `forEach`
3. `.map()` — ტრანსფორმაცია
4. `.filter()` — გაფილტვრა
5. `.reduce()` — აგრეგაცია
6. Mutating vs Non-Mutating მეთოდები (Cheatsheet)

**Part 2 — Objects:** 7. ობიექტის საფუძვლები (Dot vs Bracket, მეთოდები, `this`) 8. Destructuring (ობიექტისა და მასივის) 9. Spread & Rest ოპერატორები 10. 🎁 Bonus: `Object.keys/values/entries`, Optional Chaining (`?.`), Nullish Coalescing (`??`) 11. რეალური პრაქტიკული მაგალითი (ყველაფერი ერთად) 12. საშინაო დავალება

---

# Part 1: მასივები (Arrays) და Iteration მეთოდები

## 1️⃣ მასივის საფუძვლები

მასივი (Array) არის მონაცემთა სტრუქტურა, სადაც შეგვიძლია შევინახოთ ელემენტების სია. მასივში ელემენტები გადანომრილია **ინდექსებით**, რომლებიც **0-დან** იწყება.

```javascript
const fruits = ["Apple", "Banana", "Cherry"];

console.log(fruits[0]); // "Apple"  — პირველი ელემენტი, ინდექსი 0
console.log(fruits[2]); // "Cherry" — მესამე ელემენტი, ინდექსი 2
console.log(fruits.length); // 3        — ელემენტების რაოდენობა
```

### 📌 დამატებითი დეტალები, რომლებიც ხშირად გამოტოვება:

```javascript
// ბოლო ელემენტზე წვდომა — length - 1
console.log(fruits[fruits.length - 1]); // "Cherry"

// არარსებულ ინდექსზე წვდომა error-ს არ იძლევა, undefined-ს აბრუნებს
console.log(fruits[10]); // undefined

// მასივს შეუძლია სხვადასხვა ტიპის მონაცემი შეინახოს ერთდროულად
const mixed = ["text", 42, true, null, { key: "value" }];

// მასივის შექმნის ალტერნატიული (მაგრამ იშვიათად გამოყენებადი) გზა
const arr = new Array(3); // ცარიელი მასივი 3 slot-ით — ერიდეთ ამ სინტაქსს, დამაბნეველია
```

> 💡 **Best Practice:** მასივის შესაქმნელად თითქმის ყოველთვის გამოიყენეთ literal სინტაქსი `[]`, არა `new Array()`.

---

## 2️⃣ ტრადიციული ციკლი (`for`) vs `forEach`

```javascript
const numbers = [10, 20, 30];

// 1. ტრადიციული for ციკლი
for (let i = 0; i < numbers.length; i++) {
  console.log(`Index ${i}: ${numbers[i]}`);
}

// 2. forEach — მასივის თითოეულ ელემენტზე გადაყოლა
numbers.forEach((num, index) => {
  console.log(`Element at ${index} is ${num}`);
});
```

### 📊 `for` vs `forEach` — როდის რომელი?

| კრიტერიუმი               | `for`                                        | `forEach`                                            |
| :----------------------- | :------------------------------------------- | :--------------------------------------------------- |
| **სინტაქსი**             | გრძელი, ხელით საჭიროებს `i`-ს მართვას        | მოკლე, კითხვადი                                      |
| **`break` / `continue`** | ✅ მუშაობს                                   | ❌ **არ მუშაობს** — ციკლის ადრე გაჩერება შეუძლებელია |
| **Return Value**         | არაფერს აბრუნებს                             | არაფერს აბრუნებს (`undefined`)                       |
| **გამოყენება**           | როცა საჭიროა ადრე გაჩერება ან რთული კონტროლი | როცა უბრალოდ ყველა ელემენტზე მოქმედება გვინდა        |

> ⚠️ **კრიტიკული დეტალი:** `forEach`-ის შიგნით `break`-ის ან `continue`-ის დაწერა **სინტაქსურ Error-ს** გამოიწვევს. თუ ციკლი ადრე უნდა შეჩერდეს კონკრეტულ პირობაზე, საჭიროა ან ტრადიციული `for`, ან სხვა მეთოდი (მაგ. `.some()` — იხილეთ Bonus სექცია).

---

## 3️⃣ მასივის ტრანსფორმაციის უძლიერესი მეთოდები (`map`, `filter`, `reduce`)

ეს სამი მეთოდი არის **Functional Programming**-ის საფუძველი. ისინი **არ ცვლიან (mutate)** ორიგინალ მასივს, არამედ აბრუნებენ **ახალს**.

### ა) `.map()` — თითოეული ელემენტის გარდაქმნა

იღებს მასივის თითოეულ ელემენტს, ატარებს ფუნქციაში და აბრუნებს **იმავე სიგრძის** ახალ მასივს.

```javascript
const prices = [100, 200, 300];

// ყველა ფასს დავამატოთ 20% დღგ (VAT)
const pricesWithVAT = prices.map((price) => price * 1.2);

console.log(pricesWithVAT); // [120, 240, 360]
console.log(prices); // [100, 200, 300] — ორიგინალი უცვლელია!
```

> ⚠️ **ხშირი შეცდომა:** თუ `map`-ის callback-ს ფიგურული ფრჩხილები `{}` დაუწერეთ, აუცილებლად საჭიროა `return`, თორემ ყველა ელემენტი `undefined` გახდება:
>
> ```javascript
> const wrong = prices.map((price) => {
>   price * 1.2;
> }); // ❌ [undefined, undefined, undefined]
> const correct = prices.map((price) => {
>   return price * 1.2;
> }); // ✅ სწორია
> const alsoCorrect = prices.map((price) => price * 1.2); // ✅ Implicit Return, საუკეთესო
> ```

### ბ) `.filter()` — ელემენტების გაფილტვრა

აბრუნებს ახალ მასივს, სადაც მოხვდება **მხოლოდ** ის ელემენტები, რომლებიც აკმაყოფილებენ პირობას (`true`).

```javascript
const ages = [12, 22, 17, 30, 15, 25];

// გამოვფილტროთ მხოლოდ სრულწლოვნები
const adults = ages.filter((age) => age >= 18);

console.log(adults); // [22, 30, 25]
```

> 💡 **დამატებითი მაგალითი:** `.filter()`-ს შეგვიძლია ვერთმევდეთ `.map()`-სთანაც (Method Chaining):
>
> ```javascript
> const adultsDoubled = ages.filter((age) => age >= 18).map((age) => age * 2);
> console.log(adultsDoubled); // [44, 60, 50]
> ```

### გ) `.reduce()` — მასივის აგრეგაცია ერთ მნიშვნელობამდე

გამოიყენება მასივის ელემენტების შესაკრებად ან ერთ საბოლოო მნიშვნელობამდე (რიცხვი, ტექსტი, ობიექტი) დასაყვანად.

```javascript
const cartPrices = [15, 25, 50];

// წავიკითხოთ: accumulator (ჯამი), current (მიმდინარე)
// 0 არის accumulator-ის საწყისი მნიშვნელობა
const totalPrice = cartPrices.reduce((acc, curr) => acc + curr, 0);

console.log(totalPrice); // 90
```

> ⚠️ **ხშირი შეცდომა — Initial Value-ს დავიწყება:**
>
> ```javascript
> // საწყისი მნიშვნელობის (0) გარეშე, reduce პირველ ელემენტს accumulator-ად იღებს
> const risky = [].reduce((acc, curr) => acc + curr); // ❌ TypeError: Reduce of empty array with no initial value
> const safe = [].reduce((acc, curr) => acc + curr, 0); // ✅ 0 — არ იჩეხება ცარიელ მასივზეც კი
> ```
>
> **ოქროს წესი:** `.reduce()`-ს **ყოველთვის** მიეცით საწყისი მნიშვნელობა (მეორე არგუმენტი), თუნდაც აშკარად ჩანდეს, რომ საჭირო არ არის.

> 💡 **`.reduce()`-ის დამატებითი გამოყენება — ობიექტამდე დაყვანა:**
>
> ```javascript
> const words = ["apple", "banana", "apple", "cherry", "banana", "apple"];
>
> const wordCount = words.reduce((acc, word) => {
>   acc[word] = (acc[word] || 0) + 1;
>   return acc;
> }, {});
>
> console.log(wordCount); // { apple: 3, banana: 2, cherry: 1 }
> ```

---

## 4️⃣ Mutating vs Non-Mutating მეთოდები (Cheatsheet)

ეს ცხრილი მნიშვნელოვანია, რადგან ზოგიერთი მასივის მეთოდი **ცვლის** ორიგინალ მასივს (Mutating), ზოგი კი — **არა** (Non-Mutating, ახალს აბრუნებს).

| მეთოდი       | ცვლის ორიგინალს? | რას აკეთებს                                            |
| :----------- | :--------------- | :----------------------------------------------------- |
| `.push()`    | ✅ კი            | ბოლოში ამატებს ელემენტს                                |
| `.pop()`     | ✅ კი            | ბოლო ელემენტს შლის და აბრუნებს                         |
| `.shift()`   | ✅ კი            | პირველ ელემენტს შლის და აბრუნებს                       |
| `.unshift()` | ✅ კი            | თავში ამატებს ელემენტს                                 |
| `.sort()`    | ✅ კი            | ალაგებს ორიგინალ მასივს                                |
| `.splice()`  | ✅ კი            | შლის/ამატებს ელემენტებს კონკრეტულ ინდექსზე             |
| `.map()`     | ❌ არა           | ახალ, გარდაქმნილ მასივს აბრუნებს                       |
| `.filter()`  | ❌ არა           | ახალ, გაფილტრულ მასივს აბრუნებს                        |
| `.reduce()`  | ❌ არა           | ერთ საბოლოო მნიშვნელობას აბრუნებს                      |
| `.slice()`   | ❌ არა           | ნაწილის ასლს აბრუნებს (`.splice()`-სგან განსხვავებით!) |
| `.concat()`  | ❌ არა           | ორი მასივის გაერთიანებულ ასლს აბრუნებს                 |

> ⚠️ **ხშირად არეული წყვილი:** `.slice()` (Non-Mutating, ასლი) და `.splice()` (Mutating, შლის) — სახელები ძალიან ჰგავს ერთმანეთს, მაგრამ ქცევა სრულიად განსხვავებულია!

---

# Part 2: ობიექტები (Objects) და Modern JS Operations

## 5️⃣ ობიექტის საფუძვლები

ობიექტი (Object) ინახავს მონაცემებს **Key-Value** (გასაღები-მნიშვნელობა) წყვილების სახით.

```javascript
const user = {
  firstName: "Luka",
  age: 23,
  role: "Developer",
  // მეთოდი (ფუნქცია ობიექტის შიგნით)
  greet() {
    return `Hello, I'm ${this.firstName}`;
  },
};

// წვდომა მნიშვნელობებზე
console.log(user.firstName); // Dot notation: "Luka"
console.log(user["role"]); // Bracket notation: "Developer"
console.log(user.greet()); // "Hello, I'm Luka"
```

### 📌 Dot Notation vs Bracket Notation — როდის რომელი?

```javascript
// Dot notation — უმეტეს შემთხვევაში, key წინასწარ ცნობილია
console.log(user.firstName);

// Bracket notation — აუცილებელია, როცა key ცვლადშია შენახული (Dynamic Key)
const key = "age";
console.log(user[key]); // 23
// console.log(user.key); // ❌ undefined — key-ს, როგორც ტექსტს, ეძებდა, არა ცვლადის მნიშვნელობას
```

> 💡 **`this`-ის მნიშვნელობა:** ობიექტის მეთოდის შიგნით `this` მიუთითებს იმ ობიექტზე, რომელმაც მეთოდი გამოიძახა — ამ შემთხვევაში, `user`-ზე.

---

## 6️⃣ Destructuring (დესტრუქტურიზაცია)

Destructuring საშუალებას გვაძლევს, ობიექტიდან ან მასივიდან ელემენტები მარტივად "ამოვქაჩოთ" ცალკეულ ცვლადებში.

### ობიექტის Destructuring:

```javascript
const product = {
  title: "MacBook Pro",
  price: 2500,
  currency: "USD",
};

// ძველი გზა:
// const title = product.title;
// const price = product.price;

// ES6 Destructuring:
const { title, price, currency } = product;
console.log(`${title} costs ${price} ${currency}`);
```

### 📌 დამატებითი Destructuring პატერნები:

```javascript
// 1. Default მნიშვნელობა — თუ property არ არსებობს
const { title, discount = 0 } = product;
console.log(discount); // 0, რადგან product-ს discount საერთოდ არ ჰქონდა

// 2. Renaming (Aliasing) — თუ გინდათ სხვა სახელით შენახვა
const { title: productTitle } = product;
console.log(productTitle); // "MacBook Pro"

// 3. Nested Destructuring — ჩადგმულ ობიექტში ჩასვლა
const order = {
  id: 101,
  customer: { name: "Ana", city: "Tbilisi" },
};
const {
  customer: { name, city },
} = order;
console.log(name, city); // "Ana" "Tbilisi"

// 4. Function პარამეტრებში პირდაპირ Destructuring
function printProduct({ title, price }) {
  console.log(`${title}: $${price}`);
}
printProduct(product); // "MacBook Pro: $2500"
```

### მასივის Destructuring:

```javascript
const rgb = [255, 140, 0];

const [red, green, blue] = rgb;
console.log(red); // 255

// ელემენტების გამოტოვება მძიმეებით
const [, , blueOnly] = rgb;
console.log(blueOnly); // 0

// მნიშვნელობების გაცვლა (Swap) Destructuring-ით — ელეგანტური ხრიკი
let a = 1,
  b = 2;
[a, b] = [b, a];
console.log(a, b); // 2 1
```

---

## 7️⃣ Spread & Rest ოპერატორები (`...`)

სამი წერტილი (`...`) კონტექსტის მიხედვით ასრულებს **ორ სხვადასხვა როლს**.

### ა) Spread Operator (გაშლა / კოპირება)

გამოიყენება მასივების ან ობიექტების გასაშლელად, გასაერთიანებლად და ასლების შესაქმნელად.

```javascript
// 1. მასივების გაერთიანება
const frontend = ["HTML", "CSS", "JS"];
const backend = ["Node.js", "PostgreSQL"];
const fullstack = [...frontend, ...backend, "React"];
// ["HTML", "CSS", "JS", "Node.js", "PostgreSQL", "React"]

// 2. ობიექტის კოპირება და განახლება
const originalUser = { name: "Ana", age: 20 };
const updatedUser = {
  ...originalUser,
  location: "Tbilisi",
  age: 21, // გადაფარავს ძველ age-ს
};
```

> ⚠️ **კრიტიკული დეტალი — Shallow Copy:** Spread ოპერატორი ქმნის მხოლოდ **ზედაპირულ (shallow)** ასლს. თუ ობიექტში ჩადგმული (nested) ობიექტი ან მასივია, ის **არ** კოპირდება — მისი **მისამართი** (reference) კვლავ საერთოა:
>
> ```javascript
> const original = { name: "Ana", address: { city: "Tbilisi" } };
> const copy = { ...original };
>
> copy.address.city = "Batumi"; // ვცვლით copy-ს
> console.log(original.address.city); // "Batumi" ⚠️ — ორიგინალიც შეიცვალა!
> ```
>
> ამის თავიდან ასაცილებლად საჭიროა **Deep Copy** (მაგ. `structuredClone(original)`, თანამედროვე ბრაუზერებში).

### ბ) Rest Operator (შეფუთვა / შეგროვება)

გამოიყენება ფუნქციის პარამეტრებში ან Destructuring-ის დროს **დარჩენილი** ელემენტების ერთ მასივში შესაგროვებლად.

```javascript
// ფუნქციის პარამეტრებში:
const sumAll = (...numbers) => {
  return numbers.reduce((acc, curr) => acc + curr, 0);
};

console.log(sumAll(10, 20, 30, 40)); // 100

// Destructuring-ის დროს:
const [first, second, ...restOfNumbers] = [1, 2, 3, 4, 5];
console.log(restOfNumbers); // [3, 4, 5]

// ობიექტის Destructuring-ში:
const { title, ...restOfProduct } = product;
console.log(restOfProduct); // { price: 2500, currency: "USD" }
```

> 💡 **როგორ გავარჩიოთ Spread და Rest ერთმანეთისგან, თუ სინტაქსი იდენტურია?**
>
> - თუ `...` ჩნდება **მნიშვნელობის შექმნისას** (მასივის/ობიექტის literal-ში, ან ფუნქციის გამოძახებისას) — ეს **Spread**-ია (შლის/შლიდება).
> - თუ `...` ჩნდება **მნიშვნელობის მიღებისას** (ფუნქციის პარამეტრებში, ან Destructuring-ის მარცხენა მხარეს) — ეს **Rest**-ია (აგროვებს).

---

## 🎁 8. Bonus: დამატებითი, ხშირად ერთად საჭირო ინსტრუმენტები

### `Object.keys()`, `Object.values()`, `Object.entries()`

ხშირად გვჭირდება ობიექტზე loop-ის გავლა — ეს სამი მეთოდი ობიექტს მასივად აქცევს, რომ `map`/`filter`/`forEach` შევძლოთ გამოვიყენოთ:

```javascript
const user = { name: "Luka", age: 23, role: "Developer" };

console.log(Object.keys(user)); // ["name", "age", "role"]
console.log(Object.values(user)); // ["Luka", 23, "Developer"]
console.log(Object.entries(user));
// [["name", "Luka"], ["age", 23], ["role", "Developer"]]

// პრაქტიკული გამოყენება — ობიექტზე loop
Object.entries(user).forEach(([key, value]) => {
  console.log(`${key}: ${value}`);
});
```

### Optional Chaining (`?.`)

უსაფრთხოდ ვწვდებით ღრმად ჩადგმულ property-ს, error-ის გარეშე, თუ შუალედური ობიექტი არ არსებობს:

```javascript
const order = { customer: { name: "Ana" } };

console.log(order.customer.address.city); // ❌ TypeError: Cannot read properties of undefined
console.log(order.customer?.address?.city); // ✅ undefined — Error-ის მაგივრად, "მშვიდად" ჩერდება
```

### Nullish Coalescing (`??`)

აბრუნებს მარჯვენა მხარეს **მხოლოდ** მაშინ, თუ მარცხენა მხარე ზუსტად `null` ან `undefined` არის (და არა უბრალოდ "falsy", როგორც `||`):

```javascript
const discount = 0;

console.log(discount || 10); // 10 ⚠️ — 0 არის "falsy", ამიტომ || მას "ცარიელად" თვლის
console.log(discount ?? 10); // 0  ✅ — ?? მხოლოდ null/undefined-ზე რეაგირებს, 0 ვალიდურ მნიშვნელობად ითვლება
```

> 💡 **პრაქტიკული მნიშვნელობა:** `discount || 10` პატერნი ცნობილი ბაგის წყაროა — თუ ლეგიტიმური მნიშვნელობა (`0`, `""`, `false`) "ცარიელ" მნიშვნელობად აღიქმება, არასწორ default-ს მიიღებთ. `??` ამ პრობლემას წყვეტს.

### Bonus Array მეთოდები: `.find()`, `.some()`, `.every()`, `.includes()`, `.sort()`

```javascript
const numbers = [5, 12, 8, 130, 44];

// .find() — პირველი ელემენტი, რომელიც პირობას აკმაყოფილებს
console.log(numbers.find((n) => n > 10)); // 12

// .some() — არსებობს თუ არა მინიმუმ ერთი ელემენტი, რომელიც პირობას აკმაყოფილებს
console.log(numbers.some((n) => n > 100)); // true

// .every() — ყველა ელემენტი აკმაყოფილებს თუ არა პირობას
console.log(numbers.every((n) => n > 0)); // true

// .includes() — შეიცავს თუ არა მასივი კონკრეტულ მნიშვნელობას
console.log(numbers.includes(8)); // true

// .sort() — ⚠️ ნაგულისხმევად STRING-ებად ალაგებს, არა რიცხვებად!
console.log([10, 1, 2].sort()); // [1, 10, 2] ❌ — მოულოდნელი შედეგი!
console.log([10, 1, 2].sort((a, b) => a - b)); // [1, 2, 10] ✅ — სწორი, რიცხვითი დალაგება
```

> ⚠️ **ძალიან ხშირი შეცდომა:** `.sort()` ნაგულისხმევად მასივის ელემენტებს **ტექსტად** გარდაქმნის და ანბანურად ალაგებს — რიცხვების დასალაგებლად **ყოველთვის** გადაეცით comparator ფუნქცია: `(a, b) => a - b`.

---

## 🧪 9. რეალური პრაქტიკული მაგალითი (ყველაფერი ერთად)

დავამუშაოთ ელექტრონული მაღაზიის შეკვეთის მონაცემები:

```javascript
const products = [
  { id: 1, name: "Laptop", price: 1200, inStock: true },
  { id: 2, name: "Mouse", price: 25, inStock: false },
  { id: 3, name: "Keyboard", price: 75, inStock: true },
  { id: 4, name: "Monitor", price: 300, inStock: true },
];

// 1. გამოვფილტროთ მხოლოდ მარაგში არსებული პროდუქტები (.filter)
const availableProducts = products.filter((item) => item.inStock);

// 2. ამოვიღოთ მხოლოდ ფასები და გამოვთვალოთ ჯამური ღირებულება (.reduce)
const totalPrice = availableProducts.reduce((sum, item) => sum + item.price, 0);

// 3. შევქმნათ პროდუქტების სახელების სია (.map) და Destructuring
const productNames = availableProducts.map(({ name }) => name);

console.log("მარაგშია:", productNames); // ["Laptop", "Keyboard", "Monitor"]
console.log("ჯამური ღირებულება:", totalPrice); // 1575
```

> 💡 **გაფართოებული ვერსია — Bonus მეთოდების ჩართვით:**
>
> ```javascript
> // არსებობს თუ არა ძვირი (>1000) პროდუქტი მარაგში?
> const hasExpensiveItem = availableProducts.some((item) => item.price > 1000);
> console.log(hasExpensiveItem); // true ("Laptop")
>
> // ვიპოვოთ კონკრეტულად "Keyboard"
> const keyboard = availableProducts.find((item) => item.name === "Keyboard");
> console.log(keyboard); // { id: 3, name: "Keyboard", price: 75, inStock: true }
>
> // დავალაგოთ ფასის მიხედვით, ზრდადობით
> const sortedByPrice = [...availableProducts].sort(
>   (a, b) => a.price - b.price,
> );
> console.log(sortedByPrice.map((p) => p.name)); // ["Keyboard", "Monitor", "Laptop"]
> ```
>
> ⚠️ ხედავთ `[...availableProducts]`-ს? განზრახ დავაკოპირეთ Spread-ით, სანამ `.sort()`-ს გამოვიძახებდით — რადგან `.sort()` **Mutating**-ია და ორიგინალ `availableProducts`-ს შეცვლიდა.

---

## 🏠 10. საშინაო დავალება (Homework)

### დავალება 1 (`map` & `filter`):

გაქვთ სტუდენტების მასივი:

```javascript
const students = [
  { name: "Luka", score: 85 },
  { name: "Gio", score: 40 },
  { name: "Nino", score: 92 },
];
```

- `.filter()`-ით გამოყავით მხოლოდ ის სტუდენტები, რომელთა `score >= 51`.
- `.map()`-ით შექმენით ახალი მასივი, სადაც თითოეულ წარმატებულ სტუდენტს დაემატება თვისება `status: "Passed"`.

### დავალება 2 (`reduce`):

გაქვთ რიცხვების მასივი `[12, 5, 8, 130, 44]`. `.reduce()` მეთოდის გამოყენებით იპოვეთ ამ მასივში არსებული **მაქსიმალური** რიცხვი.

### დავალება 3 (Destructuring & Spread):

- შექმენით ობიექტი `car = { brand: "Audi", model: "A4", year: 2019, color: "Black" }`.
- Destructuring-ით ამოიღეთ `brand` და `model`.
- Spread ოპერატორით შექმენით ახალი ობიექტი `updatedCar`, სადაც შეცვლით `year`-ს 2024-ით და დაამატებთ ახალ თვისებას `owner: "Luka"`.
