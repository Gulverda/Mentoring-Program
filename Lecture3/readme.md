# 📝 Web Development / Lecture 3: HTML Forms & Validation — Lecture Summary & Homework

მოგესალმებით! ამ დოკუმენტში გაერთიანებულია მე-3 ლექციის ძირითადი თეორიული მასალა, HTML Form ელემენტებისა და **ყველა არსებული Input ტიპის** სრული ცნობარი (ქართულ-ინგლისური განმარტებებით), პრაქტიკული დავალება და სასარგებლო რესურსები.

---

## 📚 1. HTML Form ელემენტების სრული ცნობარი (Form Elements Guide)

### 🧱 1.1 ძირითადი ტეგები (Core Form Tags)

| ტეგი (Tag)       | აღწერა (Georgian Description)                                                 | Description (English)                                          |
| :--------------- | :---------------------------------------------------------------------------- | :------------------------------------------------------------- |
| **`<form>`**     | ფორმის მთავარი კონტეინერი, რომელიც აერთიანებს ინფუთებსა და გაგზავნის ლოგიკას. | Container for all form elements and controls.                  |
| **`<label>`**    | ინფუთის ტექსტური დასახელება. აუმჯობესებს Accessibility-ს და UX-ს.             | Caption/label for a form control. Improves UX & accessibility. |
| **`<input>`**    | ყველაზე მრავალფუნქციური ველი მონაცემების შესაყვანად.                          | Interactive control to accept data from the user.              |
| **`<select>`**   | ჩამოსაშლელი სია (Dropdown menu).                                              | Dropdown list for selecting one or multiple options.           |
| **`<option>`**   | ჩამოსაშლელი სიის (`<select>`) თითოეული ელემენტი/არჩევანი.                     | Individual option within a `<select>` dropdown menu.           |
| **`<textarea>`** | მრავალხაზიანი ტექსტური ველი (მაგ. კომენტარებისთვის).                          | Multi-line text input field (e.g., for comments/feedback).     |
| **`<fieldset>`** | ფორმის ლოგიკურად დაკავშირებული ველების ჯგუფი ჩარჩოთი.                         | Groups related elements within a form with a visible border.   |
| **`<legend>`**   | `<fieldset>` ჯგუფის სათაური.                                                  | Caption/title for the content of a `<fieldset>`.               |
| **`<button>`**   | ინტერაქტიული ღილაკი (Submit, Reset ან Custom JS action).                      | Clickable button used to submit form data or perform actions.  |

---

### 📥 1.2 Input ტიპების სრული ჩამონათვალი (`type="..."`)

| Input Type                  | აღწერა (Georgian Description)                                      | Description (English)                                       |
| :-------------------------- | :----------------------------------------------------------------- | :---------------------------------------------------------- |
| **`type="text"`**           | სტანდარტული ერთხაზიანი ტექსტური ველი.                              | Single-line plain text field.                               |
| **`type="password"`**       | დაფარული ტექსტური ველი პაროლისთვის (წერტილები/ვარსკვლავები).       | Masked text field for sensitive password input.             |
| **`type="email"`**          | ელფოსტის ველი (`@` ფორმატის ავტომატური შემოწმებით).                | Input field that validates email address format.            |
| **`type="number"`**         | რიცხვითი ველი (იღებს მხოლოდ ციფრებს, `min`/`max` ზღვრებით).        | Field for numeric input with spin-buttons.                  |
| **`type="tel"`**            | ტელეფონის ნომრის ველი (მობილურზე ხსნის Numpad-ს).                  | Field for telephone numbers (opens numpad on mobile).       |
| **`type="url"`**            | ვებ-მისამართის ველი (`http://` ან `https://` ფორმატის შემოწმებით). | Input field that validates web URL syntax.                  |
| **`type="search"`**         | საძიებო ველი (ბრაუზერში ემატება ტექსტის სწრაფი წაშლის `x` ღილაკი). | Search field optimized for queries (includes clear button). |
| **`type="date"`**           | თარიღის არჩევა კალენდარული ინტერფეისით (წელი, თვე, დღე).           | Date picker control (year, month, day).                     |
| **`type="time"`**           | დროის არჩევა (საათი და წუთი).                                      | Time picker control (hours and minutes).                    |
| **`type="datetime-local"`** | თარიღისა და დროის არჩევა ერთად (ადგილობრივი დროის ზონით).          | Date and time picker (year, month, day, hours, minutes).    |
| **`type="month"`**          | თვისა და წლის არჩევა (მაგ. `2026-07`).                             | Month and year picker control.                              |
| **`type="week"`**           | წლის კონკრეტული კვირის არჩევა (მაგ. `Week 30, 2026`).              | Week number and year picker control.                        |
| **`type="range"`**          | სლაიდერი (Slider) რიცხვითი მნიშვნელობის ინტერვალში ასარჩევად.      | Slider control for selecting a value within a given range.  |
| **`type="radio"`**          | რადიო ღილაკი ჯგუფიდან მხოლოდ ერთი ოპციის ასარჩევად.                | Radio button allowing only one selection from a group.      |
| **`type="checkbox"`**       | ჩექბოქსი მრავალჯერადი არჩევანისთვის ან Toggle-სთვის.               | Checkbox allowing multiple selections or single toggle.     |
| **`type="color"`**          | ფერის არჩევა Color Picker-ის პალიტრით (აბრუნებს Hex კოდს).         | Color picker control returning Hex color code.              |
| **`type="file"`**           | ფაილების ატვირთვის ველი (შესაძლებელია `accept` და `multiple`).     | File-select control for uploading local files.              |
| **`type="hidden"`**         | დამალული ველი (სერვერზე იგზავნება, მომხმარებელი ვერ ხედავს).       | Hidden input field sent to server without rendering on UI.  |
| **`type="submit"`**         | ფორმის გაგზავნის ინფუთ-ღილაკი.                                     | Input rendered as a form submit button.                     |
| **`type="reset"`**          | ფორმის გასუფთავების ინფუთ-ღილაკი საწყის მდგომარეობამდე.            | Input rendered as a form reset button.                      |
| **`type="button"`**         | სტანდარტული ღილაკი (ჩვეულებრივ გამოიყენება JS-ისთვის).             | Generic input button without default behavior.              |
| **`type="image"`**          | სურათის ფორმის Submit ღილაკად გამოყენება (`src` ატრიბუტით).        | Graphical submit button using an image URL.                 |

---

### ⚙️ 1.3 ატრიბუტები და Native ვალიდაცია (Attributes & Validation)

| ატრიბუტი                      | აღწერა (Georgian Description)                                              | Description (English)                                               |
| :---------------------------- | :------------------------------------------------------------------------- | :------------------------------------------------------------------ |
| **`action`**                  | სერვერის URL, სადაც იგზავნება ფორმის მონაცემები.                           | URL where the form data will be submitted.                          |
| **`method`**                  | HTTP მეთოდი (`GET` — URL-ში გამოჩენა, `POST` — Request Body-ში).           | HTTP method used (`GET` or `POST`).                                 |
| **`name`**                    | **სავალდებულო!** მონაცემის გასაღები (Key) სერვერისთვის.                    | Name of the control; used as the key in form submission.            |
| **`for` / `id`**              | აკავშირებს `<label>`-ს შესაბამის `<input>`-თან.                            | Connects a `<label>` element to its corresponding `<input>`.        |
| **`placeholder`**             | დროებითი დამხმარე ტექსტი ველის შიგნით.                                     | Short hint displayed inside the input field before typing.          |
| **`value`**                   | ველის საწყისი ან არჩეული მნიშვნელობა.                                      | Initial value or current submitted value of the element.            |
| **`required`**                | ველის შევსებას ხდის სავალდებულოს.                                          | Specifies that an input field must be filled out before submitting. |
| **`minlength` / `maxlength`** | ტექსტის მინიმალური / მაქსიმალური სიმბოლოების რაოდენობა.                    | Minimum/maximum character length for text input.                    |
| **`min` / `max` / `step`**    | რიცხვის ან თარიღის მინიმალური/მაქსიმალური ზღვარი და ბიჯი.                  | Minimum/maximum value and increment step for numbers/dates.         |
| **`pattern`**                 | ამოწმებს ტექსტის შესაბამისობას Regex ფორმატთან.                            | Regular expression defining expected input pattern.                 |
| **`multiple`**                | იძლევა რამდენიმე ფაილის (`file`) ან ელფოსტის (`email`) არჩევის საშუალებას. | Allows multiple values (for file uploads or emails).                |
| **`autofocus`**               | გვერდის ჩატვირთვისას ავტომატურად სვამს ფოკუსს/კურსორს ამ ველზე.            | Automatically focuses the input when page loads.                    |
| **`autocomplete`**            | რთავს/თიშავს ბრაუზერის ავტოშევსებას (`on` / `off`).                        | Enables or disables browser auto-fill suggestions.                  |
| **`disabled`**                | თიშავს ველს (მონაცემი **არ იგზავნება** სერვერზე).                          | Disables input (user cannot interact & data is not submitted).      |
| **`readonly`**                | ველი მხოლოდ წაკითხვადია (მონაცემი **იგზავნება** სერვერზე).                 | Input is read-only (value cannot be changed but is submitted).      |

---

## 🏋️‍♂️ 2. პრაქტიკული დავალება (Homework Assignment)

### 🎯 დავალების მიზანი:

ააგეთ სასტუმროს ნომრის დაჯავშნის სემანტიკური და ვალიდური HTML ფორმა სერვერული იმიტაციით.

---

### 📋 მოთხოვნები და დავალების სტრუქტურა:

1. **ფორმის ძირითადი პარამეტრები:**
   - ფორმას უნდა ჰქონდეს `method="POST"` და `action="#"`.

2. **სექცია 1: სტუმრის პირადი ინფორმაცია (`<fieldset>` & `<legend>`)**
   - **სრული სახელი:** `type="text"`, სავალდებულო (`required`), მინიმუმ 3 სიმბოლო.
   - **ელფოსტა:** `type="email"`, სავალდებულო.
   - **ტელეფონის ნომერი:** `type="tel"`, სავალდებულო.

3. **სექცია 2: დაჯავშნის დეტალები (`<fieldset>` & `<legend>`)**
   - **შესვლის თარიღი (Check-in):** `type="date"`, სავალდებულო.
   - **გამოსვლის თარიღი (Check-out):** `type="date"`, სავალდებულო.
   - **სტუმრების რაოდენობა:** `type="number"`, მინიმუმ 1, მაქსიმუმ 5, საწყისი მნიშვნელობა (`value="1"`).
   - **ოთახის ტიპი (`<select>`):** Dropdown სია ოპციებით:
     - _სტანდარტული (Standard)_
     - _ლუქსი (Deluxe)_
     - _აპარტამენტი (Suite)_
     - პირველი ოპცია უნდა იყოს გათიშული ინსტრუქცია: `<option value="" disabled selected>აირჩიეთ ოთახი...</option>`.

4. **სექცია 3: კვება და დამატებითი სერვისები**
   - **კვების ტიპი (Radio Buttons - მხოლოდ 1-ის არჩევა):**
     - მხოლოდ საუზმე (BB)
     - ორჯერადი კვება (HB)
     - სრული პანსიონი (FB)
       _(ყველა radio-ს უნდა ჰქონდეს იდენტური `name` ატრიბუტი!)_
   - **დამატებითი სერვისები (Checkboxes - მრავალჯერადი არჩევანი):**
     - ტრანსფერი აეროპორტიდან
     - SPA & Wellness
     - პარკინგი

5. **სექცია 4: დამატებითი კომენტარი და პირობები**
   - **სპეციალური მოთხოვნები:** `<textarea>`, მაქსიმუმ 200 სიმბოლო, 4 სტრიქონის სიმაღლით (`rows="4"`).
   - **წესებზე თანხმობა:** `type="checkbox"`, სავალდებულო (`required`).

6. **მოქმედების ღილაკები:**
   - ფორმის გაგზავნის ღილაკი (`type="submit"`) ტექსტით: **„დაჯავშნა“**.
   - ფორმის გასუფთავების ღილაკი (`type="reset"`) ტექსტით: **„გასუფთავება“**.

---

## 💡 3. ხშირად დაშვებული შეცდომები (Common Pitfalls)

- ❌ **Input-ისთვის `name` ატრიბუტის დავიწყება:** ამ დროს ფორმა იგზავნება, მაგრამ ამ კონკრეტული ველის მონაცემი იკარგება.
- ❌ **`<label>`-ის `for` ატრიბუტის აცდენა input-ის `id`-სთან:** `for="email_input"` უნდა ემთხვეოდეს `id="email_input"`-ს, თორემ ლეიბლზე დაჭერით კურსორი input-ში არ ჩადგება.
- ❌ **Radio button-ებისთვის განსხვავებული `name`-ების დარქმევა:** თუ Radio-ებს სხვადასხვა `name` აქვთ, მომხმარებელი შეძლებს ყველა ოპციის ერთდროულად არჩევას.
- ❌ **Placeholder-ის გამოყენება Label-ის ნაცვლად:** Placeholder ქრება ტექსტის აკრეფისას და აუარესებს მომხმარებლის გამოცდილებას (UX).

---

## 🔗 4. სასარგებლო რესურსები & ბმულები

- 📖 [MDN Web Docs — HTML Forms Guide](https://developer.mozilla.org/en-US/docs/Learn/Forms)
- 📖 [MDN Web Docs — The Native Form Widgets](https://developer.mozilla.org/en-US/docs/Learn/Forms/The_native_form_widgets)
- 🎨 [W3Schools — HTML Form Attributes](https://www.w3schools.com/html/html_forms_attributes.asp)
- 🧪 [Regex101 (Pattern ატრიბუტის ტესტირებისთვის)](https://regex101.com/)

---

### 📤 დავალების ჩაბარების ინსტრუქცია:

1. შექმენით ახალი ფაილი სახელით `index.html`.
2. ააგეთ ფორმა ზემოთ მოცემული ინსტრუქციების მიხედვით.
3. ატვირთეთ კოდი თქვენს GitHub რეპოზიტორიაში და გამოაგზავნეთ ბმული.
