# 🔀 Web Development / Lecture 11-12: საკონტროლო ნაკადები (Conditionals) და ფუნქციები (Functions)

## 📌 ლექციის მიმოხილვა

წინა ლექციაზე ვისწავლეთ ცვლადები, მონაცემთა ტიპები და ოპერატორები. დღევანდელ გაერთიანებულ ლექციაზე გავარჩევთ ორ უმნიშვნელოვანეს კონცეფციას:

- **პირობით ოპერატორებს (Conditionals):** როგორ მივაღებინოთ კოდს გადაწყვეტილებები სხვადასხვა პირობის მიხედვით.
- **ფუნქციებს (Functions):** როგორ გავხადოთ კოდი მრავალჯერ გამოყენებადი (DRY — Don't Repeat Yourself) და სტრუქტურირებული.

---

# Part 1: პირობითი ოპერატორები (Conditionals)

პირობითი კონსტრუქციები საშუალებას გვაძლევს, კოდის კონკრეტული ბლოკი გავაშვათ მხოლოდ მაშინ, როდესაც გარკვეული ლოგიკური პირობა არის `true`.

## 1️⃣ `if`, `else if` და `else`

ყველაზე ხშირად გამოყენებადი კონსტრუქციაა.

```javascript
const userAge = 20;

if (userAge >= 18) {
  console.log("წვდომა ნებადართულია: თქვენ სრულწლოვანი ხართ.");
} else if (userAge >= 16) {
  console.log("წვდომა შეზღუდულია: საჭიროა მშობლის თანხმობა.");
} else {
  console.log("წვდომა უარყოფილია: თქვენ არასრულწლოვანი ხართ.");
}
```

> 💡 **როგორ მუშაობს?** ბრაუზერი ამოწმებს პირობებს ზევიდან ქვემოთ. როგორც კი პირველი `true` პირობა შეხვდება, იმ ბლოკს ასრულებს და დანარჩენს ტოვებს.

## 2️⃣ სამობითი ოპერატორი (Ternary Operator)

Ternary Operator არის `if`/`else`-ის მოკლე, ერთხაზიანი ალტერნატივა. ის განსაკუთრებით მოსახერხებელია, როდესაც ცვლადს პირობის მიხედვით ვანიჭებთ მნიშვნელობას.

**სინტაქსი:** `პირობა ? მნიშვნელობა_თუ_true : მნიშვნელობა_თუ_false`

```javascript
const score = 85;

// if/else-ის გარეშე:
const result = score >= 51 ? "გადალახა" : "ვერ გადალახა";
console.log(result); // "გადალახა"

// გამოიყენება Template Literals-ის შიგნითაც:
console.log(`სტუდენტმა გამოცდა ${score >= 51 ? "ჩააბარა" : "ჩაჭრა"}.`);
```

## 3️⃣ `switch` კონსტრუქცია

`switch` გამოიყენება მაშინ, როდესაც ერთ კონკრეტულ ცვლადს ვუდარებთ ბევრ შესაძლო კონკრეტულ მნიშვნელობას. ის ხშირად `if`/`else if`-ზე უფრო სუფთა და კითხვადია.

```javascript
const dayOfWeek = 3;
let dayName;

switch (dayOfWeek) {
  case 1:
    dayName = "ორშაბათი";
    break;
  case 2:
    dayName = "სამშაბათი";
    break;
  case 3:
    dayName = "ოთხშაბათი";
    break;
  case 4:
    dayName = "ხუთშაბათი";
    break;
  case 5:
    dayName = "პარასკევი";
    break;
  default:
    dayName = "შაბათ-კვირა";
}

console.log(dayName); // "ოთხშაბათი"
```

> ⚠️ **კრიტიკული დეტალი:** არ დაგავიწყდეთ `break`! `break`-ის გარეშე კოდი გაგრძელდება და შეასრულებს ყველა მომდევნო `case`-ს, მიუხედავად იმისა, ემთხვევა თუ არა პირობა.

---

# Part 2: ფუნქციები (Functions)

ფუნქცია არის კოდის ბლოკი, რომელიც ასრულებს კონკრეტულ დავალებას. ის იწერება ერთხელ და მისი გამოძახება (Call / Invoke) შეგვიძლია მრავალჯერ, სხვადასხვა მონაცემით.

```
Inputs (Parameters) --->  [ FUNCTION LOGIC ]  ---> Output (Return Value)
```

## 1️⃣ Function Declaration (ფუნქციის გამოცხადება)

ეს არის ფუნქციის წერის ტრადიციული გზა.

```javascript
// ფუნქციის განსაზღვრა (Declaration)
function greetUser(name, role = "მომხმარებელი") {
  // role-ს აქვს default პარამეტრი
  return `გამარჯობა ${name}, თქვენი როლია: ${role}`;
}

// ფუნქციის გამოძახება (Execution)
const message1 = greetUser("ლუკა", "Admin");
const message2 = greetUser("ანა"); // role გახდება "მომხმარებელი"

console.log(message1); // "გამარჯობა ლუკა, თქვენი როლია: Admin"
console.log(message2); // "გამარჯობა ანა, თქვენი როლია: მომხმარებელი"
```

> 💡 **Hoisting (ამაღლება):** Function Declaration-ით შექმნილი ფუნქციის გამოძახება შეგიძლიათ კოდში იმაზე ზევითაც, ვიდრე თავად ფუნქციაა დაწერილი.

## 2️⃣ Function Expression (ფუნქციის გამოსახულება)

აქ ფუნქციას ვქმნით ანონიმურად და ვინახავთ ცვლადში (`const`).

```javascript
const calculateArea = function (width, height) {
  return width * height;
};

console.log(calculateArea(10, 5)); // 50
```

> ⚠️ **განსხვავება:** Function Expression არ ექვემდებარება Hoisting-ს. მისი გამოძახება ცვლადის გამოცხადებამდე შეუძლებელია.

## 3️⃣ Arrow Functions (ისრიანი ფუნქციები — ES6)

თანამედროვე JavaScript-ის სტანდარტი. ის უზრუნველყოფს მოკლე, სუფთა სინტაქსს.

```javascript
// სტანდარტული Arrow Function
const multiply = (a, b) => {
  return a * b;
};

// შემოკლებული (Implicit Return) — თუ ფუნქცია 1 ხაზიანია:
const square = (x) => x * x; // 1 პარამეტრისას ფრჩხილებიც არასავალდებულოა
const add = (a, b) => a + b;

console.log(square(4)); // 16
console.log(add(12, 8)); // 20
```

### 📊 ფუნქციების სინტაქსების შედარება:

| თვისება             | Function Declaration | Function Expression          | Arrow Function                   |
| :------------------ | :------------------- | :--------------------------- | :------------------------------- |
| **სინტაქსი**        | `function name() {}` | `const name = function() {}` | `const name = () => {}`          |
| **Hoisting**        | კი                   | ❌ არა                       | ❌ არა                           |
| **`this` Bounding** | აქვს საკუთარი `this` | აქვს საკუთარი `this`         | იღებს გარემოდან (Lexical `this`) |
| **მოკლე ჩანაწერი**  | ❌ არა               | ❌ არა                       | კი (Implicit return)             |

---

## 🧪 პრაქტიკული მაგალითი: პირობებისა და ფუნქციების გაერთიანება

დავწეროთ Arrow Function, რომელიც იღებს პროდუქტის ფასსა და მომხმარებლის ტიპს, და ითვლის საბოლოო ფასს ფასდაკლებით:

```javascript
const calculateFinalPrice = (price, userType) => {
  let discount = 0;

  switch (userType) {
    case "VIP":
      discount = 0.2; // 20%
      break;
    case "STUDENT":
      discount = 0.15; // 15%
      break;
    case "REGULAR":
      discount = 0.05; // 5%
      break;
    default:
      discount = 0;
  }

  const finalPrice = price - price * discount;
  return finalPrice;
};

console.log(calculateFinalPrice(100, "VIP")); // 80
console.log(calculateFinalPrice(100, "STUDENT")); // 85
console.log(calculateFinalPrice(100, "GUEST")); // 100
```

---

## 🏠 საშინაო დავალება (Homework)

სტუდენტებმა უნდა შეასრულონ 3 სავარჯიშო ცალკე `.js` ფაილში:

### დავალება 1 (Ternary Operator):

დაწერეთ ფუნქცია `checkPassOrFail(score)`, რომელიც იღებს ქულას (0-100) და Ternary Operator-ით აბრუნებს `"Passed"`, თუ ქულა 51 ან მეტია, ხოლო წინააღმდეგ შემთხვევაში — `"Failed"`.

### დავალება 2 (Switch + Functions):

შექმენით კალკულატორის ფუნქცია `calculator(a, b, operation)`. `operation` პარამეტრი შეიძლება იყოს `"+"`, `"-"`, `"*"` ან `"/"`. `switch`-ის გამოყენებით შეასრულეთ შესაბამისი მოქმედება და დააბრუნეთ შედეგი. (გაითვალისწინეთ 0-ზე გაყოფის შემთხვევა!)

### დავალება 3 (Arrow Functions + Conditions):

დაწერეთ Arrow Function `getTaxAmount(salary)`, რომელიც ითვლის საშემოსავლო გადასახადს:

- თუ ხელფასი 1000 ლარზე ნაკლებია — გადასახადი 0%.
- 1000-დან 3000 ლარამდე — 10%.
- 3000 ლარზე მეტი — 20%.

ფუნქციამ უნდა დააბრუნოს გადასახადის ზუსტი თანხა.
