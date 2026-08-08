# 📐 Web Development / Lecture 6: Advanced Layouts — CSS Positioning & Flexbox Deep Dive

მოგესალმებით **მე-6 ლექციაში**! ამ შეხვედრაზე ჩვენ გადავდივართ თანამედროვე ვებ-გვერდების სტრუქტურისა და განლაგების (Layout) აგების ყველაზე მნიშვნელოვან ინსტრუმენტებზე. ვისწავლით, როგორ ვმართოთ ელემენტების პოზიცია სივრცეში **CSS Positioning**-ის საშუალებით და როგორ ვაწყოთ დინამიური, მოქნილი ინტერფეისები **CSS Flexbox**-ით.

---

## 🧭 1. ლექციის მიმოხილვა (Lecture Overview)

1. **CSS Position თვისება:** `static`, `relative`, `absolute`, `fixed`, `sticky`.
2. **`z-index` კონცეფცია:** შრეების (Layers) მართვა Z-ღერძზე.
3. **Flexbox-ის არქიტექტურა:** Flex Container vs Flex Items, Main Axis & Cross Axis.
4. **Flex Container თვისებები:** `display: flex`, `flex-direction`, `justify-content`, `align-items`, `flex-wrap`, `gap`.
5. **Flex Item თვისებები:** `flex-grow`, `flex-shrink`, `flex-basis`, `align-self`, `order`.
6. **პრაქტიკა:** Header/Navbar-ის, Hero სექციისა და Card Grid-ის აწყობა Flexbox-ით.

---

## 📍 2. CSS Position (პოზიციონირება)

`position` თვისება განსაზღვრავს, თუ როგორ განთავსდება ელემენტი დოკუმენტის ნაკადში (Document Flow) `top`, `right`, `bottom`, `left` და `z-index` თვისებებთან კომბინაციით.

| Position ტიპი  | დოკუმენტის ნაკადი    | პოზიციონირების ათვლის წერტილი              | ხშირი გამოყენება                     |
| :------------- | :------------------- | :----------------------------------------- | :----------------------------------- |
| **`static`**   | ინარჩუნებს (Default) | ნორმალური ნაკადი (`top`/`left` არ მუშაობს) | სტანდარტული ელემენტები               |
| **`relative`** | ინარჩუნებს           | **საკუთარი თავდაპირველი ადგილი**           | `absolute` შვილის მშობლად გამოყენება |
| **`absolute`** | **ამოვარდნილია**     | უახლოესი **`position: relative`** მშობელი  | Badge-ები, Icon-ები, Modal-ები       |
| **`fixed`**    | **ამოვარდნილია**     | **ბრაუზერის ფანჯარა (Viewport)**           | ეკრანზე მიჭედილი Header ან Chat Icon |
| **`sticky`**   | ნაწილობრივ           | მშობელი კონტეინერი / Viewport scroll       | Scroll-ისას ეკრანზე გაჩერებული მენიუ |

### 🧱 Position Absolute-ისა და Relative-ის ოქროს წყვილი:

`absolute` ელემენტი იწყებს მშობლების შემოწმებას იერარქიაში ზევით. პირველი მშობელი, რომელსაც აქვს `position: relative` (ან სხვა არასტატიკური პოზიცია), ხდება მისი ათვლის სისტემა.

```css
/* მშობელი კონტეინერი */
.card {
  position: relative; /* ხდება ათვლის წერტილი absolute შვილისთვის */
  width: 300px;
}

/* შვილი - Badge ბარათის ზედა მარჯვენა კუთხეში */
.badge {
  position: absolute;
  top: 10px;
  right: 10px;
  background-color: red;
}
```

---

## 🥞 `z-index` (შრეების მართვა)

როდესაც ელემენტები გადაფარავენ ერთმანეთს (`absolute` ან `fixed` დროს), `z-index` განსაზღვრავს, რომელი გამოჩნდება წინა პლანზე.

- მუშაობს **მხოლოდ** პოზიციონირებულ ელემენტებზე (`relative`, `absolute`, `fixed`, `sticky`).
- რაც უფრო მაღალია რიცხვი (მაგ. `z-index: 100;`), მით უფრო წინა პლანზეა ელემენტი.

---

## 🎯 3. CSS Flexbox (Flexible Box Layout)

Flexbox არის ერთგანზომილებიანი (1D) Layout მოდელი, რომელიც ამარტივებს ელემენტების გასწორებას, გადანაწილებასა და ცენტრირებას სვეტად ან სტრიქონად.

```
                 Main Axis (მთავარი ღერძი)
          ------------------------------------->
         +--------------------------------------+
    C    |  Flex Item  |  Flex Item  | Flex Item |   Cross Axis
    r    |             |             |           |   (ჯვარედინი
    o    +--------------------------------------+    ღერძი)
    s
```

### 📦 3.1 Flex Container-ის თვისებები (მშობელი)

```css
.container {
  display: flex; /* რთავს Flexbox რეჟიმს */

  /* ღერძის მიმართულება */
  flex-direction: row; /* row (default) | column | row-reverse | column-reverse */

  /* მთავარი ღერძის (Main Axis) გასწორება */
  justify-content: flex-start; /* flex-start | flex-end | center | space-between | space-around | space-evenly */

  /* ჯვარედინი ღერძის (Cross Axis) გასწორება */
  align-items: stretch; /* stretch | center | flex-start | flex-end | baseline */

  /* ელემენტების გადატანა ახალ ხაზზე */
  flex-wrap: nowrap; /* nowrap | wrap | wrap-reverse */

  /* ელემენტებს შორის დაშორება (Margin-ის თანამედროვე ალტერნატივა) */
  gap: 20px; /* row-gap და column-gap */
}
```

### 🧩 3.2 Flex Item-ის თვისებები (შვილი)

```css
.item {
  /* თავისუფალი სივრცის ათვისება (გაზრდის პროპორცია) */
  flex-grow: 1; /* Default: 0 */

  /* შეკუმშვის პროპორცია ადგილის ნაკლებობისას */
  flex-shrink: 1; /* Default: 1 */

  /* საწყისი ზომა გაზრდა/შეკუმშვამდე */
  flex-basis: 200px; /* Default: auto */

  /* Shorthand: flex-grow | flex-shrink | flex-basis */
  flex: 1 1 200px;

  /* კონკრეტული ელემენტის ამოვარდნა align-items წესიდან */
  align-self: center;

  /* ელემენტის ვიზუალური მიმდევრობის შეცვლა HTML-ის შეცვლის გარეშე */
  order: 2; /* Default: 0 */
}
```

---

## 🛠️ 4. ხშირი გამოყენების შაბლონები (Common Patterns)

**1. ელემენტის იდეალური ცენტრირება (Perfect Centering):**

```css
.hero {
  display: flex;
  justify-content: center; /* ჰორიზონტალური ცენტრირება */
  align-items: center; /* ვერტიკალური ცენტრირება */
  height: 100vh;
}
```

**2. Header / Navigation Bar:**

```css
.navbar {
  display: flex;
  justify-content: space-between; /* ლოგო მარცხნივ, მენიუ მარჯვნივ */
  align-items: center;
  padding: 1rem 2rem;
}
```

---

## 📌 5. Quick Cheatsheet

| თვისება           | მნიშვნელობები                                                                       | ეხება                         |
| :---------------- | :---------------------------------------------------------------------------------- | :---------------------------- |
| `display: flex`   | —                                                                                   | Flex Container-ს              |
| `justify-content` | `flex-start`, `flex-end`, `center`, `space-between`, `space-around`, `space-evenly` | Main Axis-ს                   |
| `align-items`     | `stretch`, `center`, `flex-start`, `flex-end`                                       | Cross Axis-ს                  |
| `gap`             | `10px`, `1rem` და ა.შ.                                                              | Flex-ის შვილებს შორის მანძილს |
| `position`        | `static`, `relative`, `absolute`, `fixed`, `sticky`                                 | ნებისმიერ ელემენტს            |

---

## 📚 6. დამატებითი რესურსები (Practice & Tools)

### 🎮 სავარჯიშო თამაშები (რეკომენდებული თანმიმდევრობით):

| რესურსი                                                | აღწერა                                                                                                                                                            |
| :----------------------------------------------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **[Flexbox Froggy](https://flexboxfroggy.com/)**       | 24-დონიანი თამაში — ბაყაყს გადაატარებ ფოთოლზე `justify-content`, `align-items`, `flex-direction`-ის სწორად გამოყენებით. საუკეთესო საწყისი წერტილია დამწყებთათვის. |
| **[Flexbox Zombies](https://flexboxzombies.com/)**     | 12 თავიანი, უფრო ღრმა კურსი — `flex-grow`, `flex-shrink`, `flex-basis` და `order`-საც სერიოზულად მოიცავს, არა მხოლოდ საბაზო align/justify-ს.                      |
| **[Flexbox Defense](https://www.flexboxdefense.com/)** | Tower Defense თამაში, სადაც პოზიციები Flexbox-ის თვისებებით მართავ — კარგია მას შემდეგ, რაც საბაზო კონცეფციები უკვე გამყარებულია.                                 |

### 📖 ვიზუალური რეფერენსები (Live Coding-ის დროს გვერდით გასაშლელად):

| რესურსი                                                                                                       | აღწერა                                                                                               |
| :------------------------------------------------------------------------------------------------------------ | :--------------------------------------------------------------------------------------------------- |
| **[A Complete Guide to Flexbox — CSS-Tricks](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)**       | ინდუსტრიის სტანდარტული რეფერენსი — ყველა property ვიზუალურ დიაგრამებთან ერთად.                       |
| **[Flexbox Cheatsheet — Yoksel](https://yoksel.github.io/flex-cheatsheet/)**                                  | ერთგვერდიანი ვიზუალური ცნობარი ყველა flex თვისებაზე.                                                 |
| **[CSS Flexible Box Layout — MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_flexible_box_layout)** | ოფიციალური, ყველაზე ზუსტი დოკუმენტაცია — დამატებით წყაროდ მათთვის, ვისაც სიღრმისეულად სურს ჩაწვდომა. |

### 🛠️ Visual Builder (self-check ინსტრუმენტი):

| რესურსი                                                                        | აღწერა                                                                                                                                                                                                                            |
| :----------------------------------------------------------------------------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **[CSS Flexbox Generator — FWD Tools](https://fwdtools.com/flexbox-builder/)** | Container-ისა და item-ის ყველა თვისება chip-ღილაკებით იმართება, Live Preview-ითვე გამოაქვს CSS, SCSS, Tailwind და React კოდი. კარგია დავალების შემდეგ — სტუდენტმა შეადაროს საკუთარი ხელნაწერი CSS ტულის ავტომატურად გენერირებულს. |

---

## 🏋️‍♂️ 7. პრაქტიკული დავალება (Homework)

🎯 **დავალების მიზანი:** Flexbox-ისა და CSS Position-ის გამოყენებით სემანტიკური, რეალური ვებ-გვერდის Layout-ის აგება.

### 📋 დავალების ეტაპები:

**1. Header & Navigation (Flexbox + Position Sticky):**

- შექმენით Header ლოგოთი და ნავიგაციის ბმულებით.
- გამოიყენეთ `display: flex` და `justify-content: space-between`.
- გახადეთ Header ეკრანის ზედა ნაწილში გაჩერებული (`position: sticky; top: 0; z-index: 100;`).

**2. Hero Section (Perfect Centering):**

- ააგეთ Hero სექცია `min-height: 80vh;`-ით.
- Flexbox-ით გააცენტრეთ სათაური, ტექსტი და CTA ღილაკი ცენტრში (`flex-direction: column; justify-content: center; align-items: center;`).

**3. Product / Service Cards Grid (Flexbox Wrap & Position Absolute):**

- შექმენით მინიმუმ 3 ბარათი კონტეინერში (`display: flex; flex-wrap: wrap; gap: 20px;`).
- თითოეულ ბარათს ჰქონდეს `position: relative;`.
- დაამატეთ ბარათის ზედა კუთხეში სააქციო Badge (მაგ. „NEW" ან „-20%") `position: absolute; top: 10px; right: 10px;`-ით.

**4. Git Workflow:**

- გააკეთეთ Commit: `feat: implement flexbox layout and absolute elements`.
- ატვირთეთ ცვლილებები GitHub-ზე (`git push`) და შეამოწმეთ GitHub Pages.

### 📤 ჩაბარების ინსტრუქცია:

გამოაგზავნეთ თქვენი GitHub Repository
