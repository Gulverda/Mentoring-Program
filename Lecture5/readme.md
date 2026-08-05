# 🎨 Web Development / Lecture 5: Introduction to CSS, Selectors & The Box Model

მოგესალმებით **მე-5 ლექციაში**! ამ შეხვედრიდან ჩვენ ვნერგავთ **CSS (Cascading Style Sheets)**-ს — ენას, რომელიც პასუხისმგებელია ვებ-გვერდის ვიზუალურ მხარეზე, დიზაინსა და განლაგებაზე. ვისწავლით CSS-ის დაკავშირების გზებს, სელექტორების ტიპებს და HTML-ის ერთ-ერთ ყველაზე ფუნდამენტურ კონცეფციას — **CSS Box Model**-ს.

---

## 🧭 1. ლექციის მიმოხილვა (Lecture Overview)

1. **CSS-ის დანიშნულება:** რა არის CSS და როგორ მუშაობს "Cascading" (კასკადურობა).
2. **CSS-ის დაკავშირების 3 გზა:** Inline, Internal, External (`<link>`).
3. **CSS სელექტორები:** Element, Class, ID, Grouping და Combinators.
4. **CSS Box Model:** `content`, `padding`, `border`, `margin`.
5. **`box-sizing` ატრიბუტი:** `content-box` vs `border-box` (ინდუსტრიული სტანდარტი).

---

## 🔗 2. CSS-ის დაკავშირების გზები (Connecting CSS)

| მეთოდი       | სინტაქსი / მაგალითი                        | გამოყენება / შეფასება                                |
| :----------- | :----------------------------------------- | :--------------------------------------------------- |
| **Inline**   | `<h1 style="color: blue;">...</h1>`        | ❌ **არ გამოიყენება!** არღვევს Clean Code-ს.         |
| **Internal** | `<style> h1 { color: blue; } </style>`     | ⚠️ გამოიყენება მხოლოდ მცირე ტესტირებისთვის.          |
| **External** | `<link rel="stylesheet" href="style.css">` | ✅ **ინდუსტრიული სტანდარტი!** სუფთა და ორგანიზებული. |

> **💡 ოქროს წესი:** ყოველთვის გამოიყენეთ **External CSS** (`style.css` ფაილი `<head>` სექციაში)!

---

## 🎯 3. CSS სელექტორები (CSS Selectors)

სელექტორი ეუბნება ბრაუზერს, **რომელ** HTML ელემენტზე გავრცელდეს სტილი.

### 3.1 საბაზო სელექტორები:

```css
/* 1. Element Selector (ეხება ყველა h1-ს) */
h1 {
  color: #2c3e50;
}

/* 2. Class Selector (ეხება ყველას, ვისაც აქვს class="card") */
.card {
  background-color: #f8f9fa;
}

/* 3. ID Selector (უნიკალურია, ეხება მხოლოდ id="main-header"-ს) */
#main-header {
  font-size: 24px;
}

/* 4. Universal Selector (ეხება აბსოლუტურად ყველა ელემენტს) */
* {
  margin: 0;
  padding: 0;
}
```

### 3.2 დაჯგუფება და კომბინატორები (Grouping & Combinators):

```css
/* Grouping — ერთი და იგივე სტილი რამდენიმე ელემენტს */
h1,
h2,
p {
  font-family: Arial, sans-serif;
}

/* Descendant Selector — .card-ის შიგნით არსებული p ტეგი */
.card p {
  color: #555555;
}

/* Child Selector (პირდაპირი შვილი) */
ul > li {
  list-style: none;
}
```

---

## 📦 4. CSS Box Model (ყუთის მოდელი)

HTML-ის ყველა ელემენტი ბრაუზერისთვის წარმოადგენს ოთხკუთხედ ყუთს, რომელიც 4 შრისგან შედგება:

```
+-----------------------------------+
|               MARGIN              |  <-- გარე დაშორება (სხვა ელემენტებთან)
|  +-----------------------------+  |
|  |           BORDER            |  |  <-- ჩარჩო
|  |  +-----------------------+  |  |
|  |  |        PADDING        |  |  |  <-- შიდა დაშორება (კონტენტსა და Border-ს შორის)
|  |  |  +-----------------+  |  |  |
|  |  |  |     CONTENT     |  |  |  |  <-- უშუალოდ ტექსტი / სურათი
|  |  |  +-----------------+  |  |  |
|  |  +-----------------------+  |  |
|  +-----------------------------+  |
+-----------------------------------+
```

### Box Model-ის თვისებები:

- **Content:** ელემენტის შიგთავსი (სიგანე `width` და სიმაღლე `height`).
- **Padding:** შიდა სივრცე კონტენტსა და ჩარჩოს (Border) შორის (ფონი ფარავს Padding-საც).
- **Border:** ჩარჩო, რომელიც გარს ერტყმის Padding-ს.
- **Margin:** გარე სივრცე ელემენტსა და მის მეზობელ ელემენტებს შორის (გამჭვირვალეა).

```css
.box {
  width: 300px;
  padding: 20px; /* შიდა დაშორება ოთხივე მხარეს */
  border: 2px solid red; /* 2px სისქის წითელი ჩარჩო */
  margin: 15px auto; /* ზევით/ქვევით 15px, მარცხნივ/მარჯვნივ ცენტრირება */
}
```

---

## 📐 5. Box Model-ის საიდუმლო: `box-sizing`

ნაგულისხმევად ბრაუზერი იყენებს `box-sizing: content-box;`-ს, სადაც `width` ითვლება **მხოლოდ** კონტენტისთვის. შედეგად:

**რეალური სიგანე = width + padding + border.**

### 🛠️ ინდუსტრიული Reset (Global Rule):

იმისთვის, რომ ელემენტმა ზუსტად შეინარჩუნოს ჩვენ მიერ მითითებული `width` და Padding/Border-მა ის არ გაზარდოს, ყოველი CSS პროექტის დასაწყისში ვწერთ:

```css
*,
*::before,
*::after {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
```

---

## 📌 6. Quick Cheatsheet (მოკლე ცნობარი)

| თვისება (Property)   | მაგალითი                       | განმარტება                                           |
| :------------------- | :----------------------------- | :--------------------------------------------------- |
| `color`              | `color: #333;`                 | ტექსტის ფერი                                         |
| `background-color`   | `background-color: #f4f4f4;`   | ფონის ფერი                                           |
| `margin` (Shorthand) | `margin: 10px 20px 10px 20px;` | Top, Right, Bottom, Left (საათის ისრის მიმართულებით) |
| `margin` (2 value)   | `margin: 20px auto;`           | Top/Bottom: 20px, Left/Right: auto (ცენტრირება)      |
| `border` (Shorthand) | `border: 1px solid #ccc;`      | Width, Style, Color                                  |
| `border-radius`      | `border-radius: 8px;`          | ჩარჩოს კუთხეების დაგლუვება                           |

---

## 🏋️‍♂️ 7. პრაქტიკული დავალება (Homework)

🎯 **დავალების მიზანი:** წინა ლექციებზე აგებული პროექტის (ან ახალი HTML გვერდის) ვიზუალური დადიზაინება CSS-ის, სელექტორებისა და Box Model-ის სწორი გამოყენებით.

### 📋 დავალების ეტაპები:

**დიზაინი**
- ლინკი: https://www.figma.com/design/qp3UUDDopn1FjfpwQG5UfC/Profile-Card--Community-?node-id=10-287&t=tAUlWjdEPI0Jt5lk-0

**1. პროექტის სტრუქტურა:**

- შექმენით `style.css` ფაილი და დააკავშირეთ ის HTML-თან `<link>` ტეგით.
- დაამატეთ CSS Global Reset (`box-sizing: border-box;`, `margin: 0;`, `padding: 0;`).

**2. ბარათების (Cards) სექციის აგება:**

- HTML-ში შექმენით მინიმუმ 2 სემანტიკური ბარათი (მაგ. `class="card"`).
- CSS-ით მიეცით ბარათებს:
  - `width` და `background-color`.
  - `padding` (შიდა დაშორება ტექსტისთვის).
  - `border` და `border-radius` (კუთხეების დაგლუვება).
  - `margin` (ბარათებს შორის დაშორებისთვის).

**3. ტექსტისა და ღილაკების სტილიზაცია:**

- გამოიყენეთ Class და Element სელექტორები სათაურებისა და პარაგრაფების გასაფერადებლად.
- ღილაკებს (`<button>`) ჩამოაშორეთ ნაგულისხმევი ჩარჩო (`border: none;`), დაამატეთ `padding` და ფონი.

**4. Git Integration & GitHub Pages:**

- გააკეთეთ Commit მესიჯით: `feat: add initial CSS styles and box-model layouts`.
- ატვირთეთ ცვლილებები GitHub-ზე (`git push`) და შეამოწმეთ განახლებული Live ბმული.

### 📤 ჩაბარების ინსტრუქცია:

გამოაგზავნეთ თქვენი GitHub Repository-სა და GitHub Pages-ის Live ბმულები.
