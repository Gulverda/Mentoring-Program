# ⚡ Web Development / Lecture 8: JavaScript შესავალი, ცვლადები, მონაცემთა ტიპები და ოპერატორები

## 📌 ლექციის მიმოხილვა

აქამდე გავარჩიეთ, როგორ ვაგებთ ვებ-გვერდის ჩონჩხს (HTML) და როგორ ვაძლევთ მას ვიზუალურ იერსახეს (CSS). დღეიდან გადავდივართ სამშენებლო ბლოკის მესამე ნაწილზე — **JavaScript (JS)**-ზე, რომელიც ვებ-გვერდს სძენს ინტერაქტიულობასა და ლოგიკას.

---

## 🧭 1. თემების მიმოხილვა

1. **რა არის JavaScript** და როგორ ვრთავთ HTML-ში (`<script>`).
2. **ცვლადები:** `let`, `const` და მოძველებული `var`.
3. **მონაცემთა ტიპები:** Primitive vs Non-Primitive, `typeof`.
4. **ოპერატორები:** არითმეტიკული, მინიჭების, შედარების (`===` vs `==`) და ლოგიკური.

---

## 1️⃣ რა არის JavaScript და როგორ ვრთავთ HTML-ში?

JavaScript არის მაღალი დონის, ინტერპრეტირებადი და დინამიკური პროგრამირების ენა, რომელიც გაშვებულია უშუალოდ ბრაუზერში.

### 🛠️ JS-ის დაკავშირება HTML-თან (`<script>`)

HTML-ში CSS-ის მსგავსად JS-ის დაკავშირებაც 2 ძირითადი გზით შეგვიძლია:

- **შიდა (Internal):** `<script>` ტეგის გამოყენებით HTML ფაილში.
- **გარე (External — Best Practice):** ცალკე `.js` ფაილის შექმნით.

```html
<!DOCTYPE html>
<html lang="ka">
  <head>
    <meta charset="UTF-8" />
    <title>JS Introduction</title>
  </head>
  <body>
    <h1>გამარჯობა JavaScript!</h1>

    <!-- External JS - საუკეთესო პრაქტიკაა script ტეგის ჩასმა <body>-ს ბოლოში -->
    <script src="script.js"></script>
  </body>
</html>
```

> 💡 **Best Practice:** `<script src="script.js"></script>` ტეგი მოათავსეთ `<body>`-ს დახურვამდე უშუალოდ წინ. ეს უზრუნველყოფს იმას, რომ HTML ელემენტები ჯერ ჩაიტვირთოს და JS კოდმა შეძლოს მათზე წვდომა.

---

## 2️⃣ ცვლადები (Variables): `let` და `const`

ცვლადი არის მეხსიერების გამოყოფილი ადგილი (კონტეინერი), სადაც ვინახავთ მონაცემებს შემდგომი გამოყენებისთვის.

ძველ JavaScript-ში გამოიყენებოდა `var`, თუმცა თანამედროვე ES6+ სტანდარტში ვიყენებთ `let`-სა და `const`-ს.

```javascript
// 1. let — ცვლადი, რომლის მნიშვნელობის შეცვლაც (Re-assignment) შეგვიძლია
let age = 22;
age = 23; // ნებადართულია

// 2. const — კონსტანტა, რომლის მნიშვნელობის შეცვლაც შექმნის შემდეგ აკრძალულია
const birthYear = 2003;
// birthYear = 2004; ❌ Error: Assignment to constant variable.
```

### 📊 `let`, `const` და `var`-ის შედარება:

| თვისება                           | `let`                    | `const`                  | `var` (მოძველებული) |
| :-------------------------------- | :----------------------- | :----------------------- | :------------------ |
| **Scope (მოქმედების არეალი)**     | Block Scope `{}`         | Block Scope `{}`         | Function Scope      |
| **ხელახალი მინიჭება (Re-assign)** | კი                       | ❌ არა                   | კი                  |
| **Hoisting**                      | არა (Temporal Dead Zone) | არა (Temporal Dead Zone) | კი (`undefined`-ით) |

> 💡 **ოქროს წესი:** ყოველთვის გამოიყენეთ `const`, ხოლო თუ იცით, რომ ცვლადის მნიშვნელობა მომავალში აუცილებლად უნდა შეიცვალოს, გამოიყენეთ `let`. `var`-ს თანამედროვე კოდში აღარ ვიყენებთ!

---

## 3️⃣ მონაცემთა ტიპები (Data Types)

JavaScript-ში მონაცემთა ტიპები იყოფა 2 დიდ ჯგუფად: **პრიმიტიული** (Primitive) და **არაპრიმიტიული** (Non-Primitive / Objects).

```
                  JS Data Types
                     |
       +-------------+-------------+
       |                           |
   Primitive                  Non-Primitive
   - String                   - Objects
   - Number                   - Arrays
   - Boolean                  - Functions
   - Null
   - Undefined
   - Symbol / BigInt
```

### 🔹 პრიმიტიული ტიპები:

**String (ტექსტური):** ბრჭყალებში მოქცეული ტექსტი.

```javascript
const firstName = "Luka";
const greeting = "Hello World";
const templateLiteral = `My name is ${firstName}`; // Template Literal (ES6)
```

**Number (რიცხვითი):** მთელი ან ათწილადი რიცხვები.

```javascript
const score = 100;
const price = 19.99;
```

**Boolean (ლოგიკური):** იღებს მხოლოდ ორ მნიშვნელობას: `true` ან `false`.

```javascript
const isLoggedIn = true;
const hasDiscount = false;
```

**Undefined:** ცვლადი გამოცხადებულია, მაგრამ მნიშვნელობა ჯერ არ მიუნიჭებია.

```javascript
let userRole;
console.log(userRole); // undefined
```

**Null:** განზრახ მინიჭებული „ცარიელი" ან „არარსებული" მნიშვნელობა.

```javascript
let selectedProduct = null; // პროდუქტი ჯერ არ არის არჩეული
```

### 🔍 `typeof` ოპერატორი

ტიპის შესამოწმებლად ვიყენებთ `typeof` ბრძანებას:

```javascript
console.log(typeof "Text"); // "string"
console.log(typeof 42); // "number"
console.log(typeof true); // "boolean"
console.log(typeof null); // "object" ⚠️ (JS-ის ცნობილი ისტორიული ბაგი)
```

---

## 4️⃣ ოპერატორები (Operators)

### 1. არითმეტიკული ოპერატორები:

```javascript
let a = 10;
let b = 3;

console.log(a + b); // 13 (მიმატება)
console.log(a - b); // 7  (გამოკლება)
console.log(a * b); // 30 (გამრავლება)
console.log(a / b); // 3.333... (გაყოფა)
console.log(a % b); // 1  (ნაშთი გაყოფიდან - Modulo)
console.log(a ** b); // 1000 (ხარისხში აყვანა)
```

### 2. მინიჭების (Assignment) ოპერატორები:

```javascript
let x = 5;
x += 3; // იგივეა რაც: x = x + 3; (შედეგი: 8)
x -= 2; // იგივეა რაც: x = x - 2; (შედეგი: 6)
x *= 2; // იგივეა რაც: x = x * 2; (შედეგი: 12)
```

### 3. შედარების (Comparison) ოპერატორები:

```javascript
const x = 5;
const y = "5";

// მკაცრი შედარება (Strict Comparison) - ამატჩებს ტიპსაც და მნიშვნელობასაც
console.log(x === y); // false (Number !== String)
console.log(x !== y); // true

// არამკაცრი შედარება (Loose Comparison) - ერიდეთ გამოყენებას!
console.log(x == y); // true (ავტომატურად გარდაქმნის ტიპებს)
```

> 💡 **Best Practice:** ყოველთვის გამოიყენეთ მკაცრი შედარება (`===` და `!==`), რათა თავიდან აიცილოთ ტიპების ავტომატური გარდაქმნით (Type Coercion) გამოწვეული ბაგები.

### 4. ლოგიკური ოპერატორები:

- `&&` (AND – და): ჭეშმარიტია, თუ ორივე პირობა ჭეშმარიტია.
- `||` (OR – ან): ჭეშმარიტია, თუ ერთ-ერთი მაინც პირობა ჭეშმარიტია.
- `!` (NOT – უარყოფა): აბრუნებს საპირისპირო ბულეანურ მნიშვნელობას.

```javascript
const hasAge = true;
const hasId = false;

console.log(hasAge && hasId); // false
console.log(hasAge || hasId); // true
console.log(!hasAge); // false
```

---

## 🧪 5. პრაქტიკული დავალება სტუდენტებისთვის (Classwork)

შექმენით `script.js` ფაილი და შეასრულეთ შემდეგი ნაბიჯები:

```javascript
// 1. შექმენით კონსტანტა თქვენი სახელისთვის და ცვლადი ასაკისთვის
const studentName = "გიორგი";
let studentAge = 20;

// 2. გამოთვალეთ დაბადების წელი
const currentYear = 2026;
const estimatedBirthYear = currentYear - studentAge;

// 3. დაბეჭდეთ შედეგი Template Literal-ის გამოყენებით
console.log(`${studentName} დაიბადა დაახლოებით ${estimatedBirthYear} წელს.`);

// 4. შეამოწმეთ შედარების ოპერატორები
const isAdult = studentAge >= 18;
console.log(`არის თუ არა სრულწლოვანი? ${isAdult}`);
```
