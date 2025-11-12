
---

# 🧮 פרויקט: מחשבון במעבר ל־OOP

### חלק מקורס JavaScript – לימוד הבדל בין גישה פונקציונלית לגישת OOP

---

## 🎯 מטרת השיעור

בפרויקט זה נבנה **מחשבון (Counter)** בשתי דרכים שונות:

1. 🧱 **גישה רגילה (DOM + פונקציות רגילות)** – שימוש במשתנים, מאזיני אירועים ופונקציות פשוטות.
2. 🧩 **גישה מונחית עצמים (OOP)** – שימוש באובייקטים, קונסטרקטור, מתודות ו־class.

המטרה היא להבין **איך אותו קוד מתורגם לגישת OOP בצורה נקייה, מסודרת וגמישה**.

---

## 🧠 מה נלמד

* להבין מהו **אובייקט** ב־JavaScript.
* ליצור **מחלקה (class)** ולבנות ממנה **אובייקטים שונים**.
* להשתמש במאפיין `this` כדי להפנות אל הנתונים הפנימיים של כל אובייקט.
* להבין את ההבדל בין **Prototype functions** ל־**Class methods**.
* לנהל **אירועים (Event Listeners)** בצורה חכמה ומודולרית.

---

## 📂 מבנה הפרויקט

```
📁 counter-app
├── index.html        ← מבנה הדף (שני מחשבונים)
├── styles.css        ← עיצוב כללי
├── app-function.js   ← גישה רגילה עם DOM
└── app-class.js      ← גישת OOP עם class
```

---

## 🧱 שלב ראשון – HTML

שני מחשבונים זהים במבנה, אבל עם ערכים התחלתיים שונים:

```html
<div class="container first-counter">
  <h2>מחשבון 1</h2>
  <span class="value">0</span>
  <div class="button-container">
    <button class="btn decrease">להחסיר</button>
    <button class="btn reset">התחלה מחדש</button>
    <button class="btn increase">להוסיף</button>
  </div>
</div>

<div class="container second-counter">
  <h2>מחשבון 2</h2>
  <span class="value">0</span>
  <div class="button-container">
    <button class="btn decrease">להחסיר</button>
    <button class="btn reset">התחלה מחדש</button>
    <button class="btn increase">להוסיף</button>
  </div>
</div>
```

---

## 🎨 שלב שני – עיצוב בסיסי (CSS)

```css
body {
  font-family: Arial, sans-serif;
  background: #f2f2f2;
  display: flex;
  justify-content: center;
  align-items: flex-start;
  gap: 2rem;
  padding: 2rem;
}

.container {
  background: white;
  border-radius: 10px;
  box-shadow: 0 0 10px #ccc;
  padding: 1.5rem;
  text-align: center;
}

.value {
  display: block;
  font-size: 2rem;
  margin: 1rem 0;
}

button {
  margin: 0.3rem;
  padding: 0.6rem 1rem;
  border: none;
  border-radius: 5px;
  background: steelblue;
  color: white;
  cursor: pointer;
}
button:hover {
  background: #4682b4;
}
```

---

## ⚙️ שלב שלישי – גישה פונקציונלית (DOM רגיל)

```js
const counters = document.querySelectorAll(".container");

counters.forEach(counter => {
  let value = 0;
  const valueDOM = counter.querySelector(".value");
  const increaseBtn = counter.querySelector(".increase");
  const decreaseBtn = counter.querySelector(".decrease");
  const resetBtn = counter.querySelector(".reset");

  increaseBtn.addEventListener("click", () => {
    value++;
    valueDOM.textContent = value;
  });

  decreaseBtn.addEventListener("click", () => {
    value--;
    valueDOM.textContent = value;
  });

  resetBtn.addEventListener("click", () => {
    value = 0;
    valueDOM.textContent = value;
  });
});
```

🧩 כאן כל מחשבון מטפל בעצמו, אבל כל הקוד **חוזר על עצמו**.
בגישה הבאה – **נפשט ונייעל את הקוד** עם OOP.

---

## 🧠 שלב רביעי – מעבר לגישת OOP

### 📘 רעיון בסיסי

ב־OOP אנחנו כותבים **תוכנית אחת כללית (class)** שיכולה לייצר כמה מחשבונים שונים.
כל מחשבון הוא **אובייקט נפרד** עם ערכים משלו.

---

### 🔹 שלב 1: פונקציה לעזרה בבחירת אלמנטים

```js
function getElement(selection) {
  const element = document.querySelector(selection);
  if (element) return element;
  throw new Error(`לא נמצא אלמנט בשם ${selection}`);
}
```

---

### 🔹 שלב 2: יצירת מחלקה (Class)

```js
class Counter {
  constructor(element, value) {
    this.counter = element;
    this.value = value;

    this.valueDOM = element.querySelector('.value');
    this.increaseBtn = element.querySelector('.increase');
    this.decreaseBtn = element.querySelector('.decrease');
    this.resetBtn = element.querySelector('.reset');

    this.valueDOM.textContent = this.value;

    // חיבור הפונקציות לאירועים
    this.increase = this.increase.bind(this);
    this.decrease = this.decrease.bind(this);
    this.reset = this.reset.bind(this);

    this.increaseBtn.addEventListener('click', this.increase);
    this.decreaseBtn.addEventListener('click', this.decrease);
    this.resetBtn.addEventListener('click', this.reset);
  }

  increase() {
    this.value++;
    this.valueDOM.textContent = this.value;
  }

  decrease() {
    this.value--;
    this.valueDOM.textContent = this.value;
  }

  reset() {
    this.value = 0;
    this.valueDOM.textContent = this.value;
  }
}
```

---

### 🔹 שלב 3: יצירת שני אובייקטים שונים

```js
const firstCounter = new Counter(getElement('.first-counter'), 100);
const secondCounter = new Counter(getElement('.second-counter'), 200);
```

* שני המחשבים משתמשים באותה **מחלקה**,
  אבל לכל אחד ערך התחלתי שונה (`100`, `200`).

---

## 📊 השוואה בין שתי הגישות

| נושא          | DOM רגיל                 | OOP                       |
| ------------- | ------------------------ | ------------------------- |
| שורות קוד     | ארוכות וחוזרות על עצמן   | קצרות ונקיות              |
| שינוי קוד     | צריך לשנות בכל מקום      | שינוי פעם אחת במחלקה      |
| קריאות        | בסיסית, אך פחות מודולרית | גבוהה וברורה              |
| שימוש חוזר    | מוגבל                    | קל ליצירת אובייקטים חדשים |
| למידה על this | לא נדרש                  | חובה להבין איך this עובד  |

---

## 💡 מושגים חשובים 

* **אובייקט (Object)** – “קופסה” עם נתונים (value) ופעולות (methods).
* **class** – תבנית ליצירת אובייקטים מאותו סוג.
* **constructor()** – קוד שרץ ברגע ש"אובייקט" חדש נוצר.
* **this** – מתייחס לאובייקט הנוכחי.
* **bind()** – קושר את הפונקציה לאובייקט הנכון כדי שלא תאבד את ההקשר שלה.
<img width="949" height="834" alt="image" src="https://github.com/user-attachments/assets/9d249158-a219-4207-912d-3a816307c2d8" />

---

## 🧩 תרגילים להרחבה

1. הוסיפו כפתור “כפול” שמכפיל את הערך.
2. הוסיפו מגבלה – הערך לא יכול להיות קטן מ־0.
3. הוסיפו צבעים דינמיים לפי הערך (ירוק = חיובי, אדום = שלילי).
4. צרו מחשבון שלישי עם ערך פתיחה שונה.
5. המירו את הקוד לגרסה של Class ES6 עם `export/import` (מתאים ל־Next.js או React).

---

## ✅ סיכום

בעזרת הפרויקט הזה  אנחנו לומדים:

* חיזקו את שליטתם ב־DOM ו־Events.
* למדו איך לכתוב קוד **נוח, נקי וחוזר על עצמו פחות**.
* הבינו מה ההבדל בין קוד “רגיל” ל־**OOP אמיתי**.
*  בסיס מצוין להמשך — בניית אפליקציות מורכבות יותר עם מחלקות, מורשת ו־Modules.

---

רוצה שאכין גם **גרסת המשך** לשיעור הזה – “מחשבון עם OOP + שמירה ב־localStorage”?
זה מצוין לשלב הבא של הקורס.



