# 📚 HTML-ის გაღრმავება & სემანტიკა / HTML In-Depth & Semantics

მოგესალმებით **მე-2 ლექციაში**! ამ დოკუმენტაციაში განხილულია ტექსტის ფორმატირების, სემანტიკის, სიების, ბმულებისა და მედიის ძირითადი HTML თეგები.

Welcome to **Lecture 2**! This documentation covers text formatting, semantics, lists, links, and media HTML tags.

---

## 📑 თეგების ცნობარი / Tag Reference

### 1. ტექსტი და სემანტიკა / Text & Semantics

| თეგი (Tag) | ქართული განმარტება                                                                                     | English Description                                                         | მაგალითი (Example)            |
| :--------- | :----------------------------------------------------------------------------------------------------- | :-------------------------------------------------------------------------- | :---------------------------- |
| `<strong>` | **მნიშვნელოვანი ტექსტი.** გამოიყენება აზრობრივად კრიტიკული სიტყვების დასაშტრიხად (ვიზუალურად ამუქებს). | **Important text.** Used for semantically strong importance (renders bold). | `<strong>ყურადღება!</strong>` |
| `<b>`      | **გამუქებული ტექსტი.** უბრალოდ ვიზუალური გამუქება აზრობრივი დატვირთვის გარეშე.                         | **Bold text.** Purely visual bolding without semantic weight.               | `<b>ტექსტი</b>`               |
| `<em>`     | **ხაზგასმული ტექსტი.** გამოიყენება ინტონაციური ხაზგასმისთვის (ვიზუალურად ახრის).                       | **Emphasized text.** Used to stress/emphasize words (renders italic).       | `<em>მნიშვნელოვანია</em>`     |
| `<i>`      | **დახრილი ტექსტი.** გამოიყენება უცხო სიტყვების, ტერმინების ან აზრებისთვის.                             | **Italic text.** Used for alternate voice/technical terms.                  | `<i>HTML5</i>`                |
| `<mark>`   | **გამოყოფილი/გამონათებული ტექსტი.** ტექსტის მარკერით დაფარვის ეფექტი.                                  | **Highlighted text.** Represents text highlighted for reference.            | `<mark>სიახლე</mark>`         |
| `<small>`  | **მცირე ტექსტი.** გამოიყენება შენიშვნებისთვის, ავტორების ან ლიცენზიის ტექსტისთვის.                     | **Small text.** Used for side-comments, copyrights, and fine print.         | `<small>© 2026</small>`       |
| `<del>`    | **წაშლილი ტექსტი.** ვიზუალურად გადის ხაზს (მაგ. ფასდაკლებისას).                                        | **Deleted text.** Represents text that has been removed (strikethrough).    | `<del>100$</del>`             |
| `<ins>`    | **დამატებული ტექსტი.** ხაზგასმულია ქვემოდან (ხშირად იწერება `<del>`-ის გვერდით).                       | **Inserted text.** Represents text that has been added (underlined).        | `<ins>80$</ins>`              |
| `<br>`     | **ხაზის გადატანა.** გადაჰყავს ტექსტი ახალ სტრიქონზე (Self-closing).                                    | **Line break.** Forces a text break to a new line (Self-closing).           | `პირველი ხაზი<br>მეორე ხაზი`  |
| `<hr>`     | **ჰორიზონტალური გამყოფი.** შინაარსობრივად ყოფს სექციებს ხაზით (Self-closing).                          | **Horizontal rule.** Semantic break between paragraph-level elements.       | `<hr>`                        |

---

### 2. სიები / Lists

| თეგი (Tag) | ქართული განმარტება                                             | English Description                                         | მაგალითი (Example)  |
| :--------- | :------------------------------------------------------------- | :---------------------------------------------------------- | :------------------ |
| `<ul>`     | **დაუნომრავი სია.** ქმნის სტრუქტურას Bullet point-ებით.        | **Unordered list.** Creates a bulleted list.                | `<ul>...</ul>`      |
| `<ol>`     | **დანომრილი სია.** ქმნის დანომრილ სტრუქტურას (1, 2, 3...).     | **Ordered list.** Creates a numbered list.                  | `<ol>...</ol>`      |
| `<li>`     | **სიის ელემენტი.** თითოეული პუნქტი `<ul>` ან `<ol>`-ის შიგნით. | **List item.** Represents an individual item inside a list. | `<li>პუნქტი 1</li>` |

---

### 3. ბმულები და მედია / Links & Media

| თეგი / ატრიბუტი   | ქართული განმარტება                                                      | English Description                                                                 | მაგალითი (Example)                        |
| :---------------- | :---------------------------------------------------------------------- | :---------------------------------------------------------------------------------- | :---------------------------------------- |
| `<a>`             | **ჰიპერბმული.** გადაჰყავს მომხმარებელი სხვა გვერდზე ან რესურსზე.        | **Anchor / Link.** Defines a hyperlink to another page/resource.                    | `<a href="https://google.com">Google</a>` |
| `href=""`         | **ბმულის მისამართი.** `<a>` თეგის ატრიბუტი, სადაც იწერება URL.          | **Hyperlink reference.** Specifies the target URL for the link.                     | `href="about.html"`                       |
| `target="_blank"` | **ახალ ჩანართში გახსნა.** ბმულს ხსნის ბრაუზერის ახალ Tab-ში.            | **Open in new tab.** Opens the linked document in a new tab.                        | `target="_blank"`                         |
| `<img>`           | **სურათი.** სურათის გამოჩენა გვერდზე (Self-closing).                    | **Image.** Embeds an image in the document (Self-closing).                          | `<img src="pic.jpg" alt="აღწერა">`        |
| `src=""`          | **სურათის წყარო.** `<img>` თეგის ატრიბუტი — გზა ფაილამდე.               | **Source.** Specifies the path to the image file.                                   | `src="assets/logo.png"`                   |
| `alt=""`          | **ალტერნატიული ტექსტი.** აუცილებელია SEO-სთვის და Screen Reader-ისთვის. | **Alternative text.** Describes the image if it fails to load / for screen readers. | `alt="კომპანიის ლოგო"`                    |

---

## 🏗️ სემანტიკური HTML5 სტრუქტურა / Semantic Structure

- **`<header>`** — გვერდის ან სექციის თავსართი (ლოგო, სათაური).
- **`<nav>`** — ნავიგაციის ბლოკი (მენიუ, ბმულები).
- **`<main>`** — გვერდის ძირითადი, უნიკალური კონტენტი.
- **`<article>`** — დამოუკიდებელი, სრული შინაარსის ბლოკი (მაგ. ბლოგ პოსტი).
- **`<section>`** — გვერდის თემატური სექცია/ჯგუფი.
- **`<footer>`** — გვერდის ან სექციის ბოლოსართი (საავტორო უფლებები, კონტაქტი).

---

## 🏠 საშინაო დავალება / Homework Assignment

### 📌 დავალების დასახელება: „პირადი ბლოგის / პორტფოლიოს გვერდი (About Me)“

შექმენით HTML ფაილი სახელად `index.html` და ააგეთ თქვენი სავიზიტო/ბლოგის გვერდი შემდეგი მოთხოვნების დაცვით:

#### 1. სტრუქტურა და სემანტიკა (Structure & Semantics)

- გამოიყენეთ სწორი HTML5 სტრუქტურა (`<!DOCTYPE html>`, `<html>`, `<head>`, `<body>`).
- გამოიყენეთ სემანტიკური თეგები: `<header>`, `<nav>`, `<main>`, `<footer>`.

#### 2. შინაარსი და ტექსტის ფორმატირება (Content & Text Formatting)

- **`<header>` სექციაში:**
  - მთავარი სათაური (`<h1>`): თქვენი სახელი და გვარი / პროფესია.
  - ნავიგაცია (`<nav>`): `<ul>` სია, სადაც იქნება ბმული თქვენს მეორე HTML გვერდზე (მაგ: `contact.html`).
- **`<main>` სექციაში:**
  - **პროფილის ფოტო:** `<img>` თეგი სწორი `src`, `alt` ატრიბუტებით და მითითებული `width`-ით.
  - **მოკლე ბიოგრაფია:** მინიმუმ 2 აბზაცი (`<p>`).
  - **ფორformatირება:** აუცილებლად გამოიყენეთ თეგები: `<strong>`, `<em>`, `<mark>`, `<small>` და `<hr>`.
  - **ფასდაკლების / აქციის მაგალითი (სავარჯიშოდ):** გამოიყენეთ `<del>` და `<ins>` (მაგალითად: "ჩემი კონსულტაციის ფასი: <del>100$</del> <ins>50$</ins>").

#### 3. სიები (Lists)

- **დაუნომრავი სია (`<ul>`):** „ჩემი ჰობი / ინტერესები“ (მინიმუმ 4 პუნქტი).
- **დანომრილი სია (`<ol>`):** „3 მთავარი მიზანი ამ კურსზე“ (1, 2, 3).

#### 4. ბმულები (Links)

- მინიმუმ 2 გარე ბმული (`<a>`) თქვენს LinkedIn-ზე, GitHub-ზე ან საყვარელ საიტზე, რომლებიც გაიხსნება **ახალ ჩანართში** (`target="_blank"`).
# Mentoring-Program
