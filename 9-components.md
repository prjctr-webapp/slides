---
title: "Вебкомпоненти ІI"
customTheme: "theme"
width: 1280
height: 720
---

# Вебкомпоненти

## Детальний огляд технологій

---

## HTML Templates: основи

Розгляд синтаксису та методів роботи з шаблонами HTML для створення перевикористовуваної розмітки.

--

### Синтаксис шаблонів

Шаблони HTML (`<template>`) — це механізм, що дозволяє розробникам зберігати клієнтську розмітку на сторінці без відображення її до активації за допомогою JavaScript. Шаблони не обробляються браузером до того моменту, поки ви не скажете це зробити, що робить їх ідеальними для перевикористання коду.

```html
<template id="my-template">
  <div>
    <p>Це приклад шаблону.</p>
  </div>
</template>
```

--

### Ініціалізація та маніпуляція шаблоном

Щоб використовувати шаблон, ви маєте створити його копію та вставити в документ.

```javascript
// Знаходження шаблону в DOM
const template = document.getElementById("my-template");

// Створення «глибокої» копії вмісту шаблону
const clone = document.importNode(template.content, true);

// Додавання копії в документ
document.body.appendChild(clone);
```

--

### Динамічне відображення даних

Шаблони можуть динамічно відображати дані, отримуючи їх через JavaScript. Це робить шаблони дуже гнучкими для використання в різних сценаріях.

```html
<template id="user-template">
  <div class="user">
    <p>Name: <span class="user-name"></span></p>
  </div>
</template>
```

```javascript
const userTemplate = document.getElementById("user-template").content;

// Створення нового екземпляру шаблону для кожного користувача
users.forEach((user) => {
  const userClone = document.importNode(userTemplate, true);
  userClone.querySelector(".user-name").textContent = user.name;
  document.body.appendChild(userClone);
});
```

--

### Динамічне додавання даних

Дані можуть бути динамічно вставлені в клонований шаблон перед його додаванням у DOM, що дозволяє використовувати шаблон для різних користувачів або сценаріїв.

```javascript
const users = [
  {
    name: "Іван Іванов",
    email: "ivan@example.com",
    photoUrl: "path/to/photo1.jpg",
  },
  {
    name: "Марія Петренко",
    email: "maria@example.com",
    photoUrl: "path/to/photo2.jpg",
  },
];

users.forEach((user) => {
  const template = document
    .getElementById("user-card-template")
    .content.cloneNode(true);
  template.querySelector(".user-photo").src = user.photoUrl;
  template.querySelector(".user-name").textContent = user.name;
  template.querySelector(".user-email").textContent = user.email;
  document.body.appendChild(template);
});
```

--

### Умовний рендеринг та цикли

HTML шаблони можуть бути динамічно модифіковані для відображення вмісту на основі умов або циклічних структур.

```javascript
users.forEach((user) => {
  const template = document
    .getElementById("user-card-template")
    .content.cloneNode(true);

  // Умовний рендеринг фотографії користувача
  if (user.photoUrl) {
    template.querySelector(".user-photo").src = user.photoUrl;
  } else {
    template.querySelector(".user-photo").remove(); // Видалення елемента, якщо фото відсутнє
  }

  // Заповнення даними
  template.querySelector(".user-name").textContent = user.name;
  template.querySelector(".user-email").textContent = user.email;
  document.body.appendChild(template);
});
```

---

## Shadow DOM: створення та монтування

Shadow DOM дозволяє інкапсулювати структуру, стилі та поведінку компонентів, ізолюючи їх від основного документа. Це забезпечує більшу модульність та контроль над стилем.

--

### Синтаксис

```html
<div id="hostElement"></div>
```

```javascript
// Знаходження елемента, для якого буде створений Shadow DOM
const hostElement = document.getElementById("hostElement");

// Створення Shadow DOM для елемента
const shadowRoot = hostElement.attachShadow({ mode: "open" });

// Додавання HTML та CSS у Shadow DOM
shadowRoot.innerHTML = `
  <style>
    p {
      color: blue;
    }
  </style>
  <p>Це контент у Shadow DOM!</p>
`;
```

--

### Використання слотів для вмісту

Слоти у Shadow DOM дозволяють визначити місця, куди можна вставити зовнішній контент. Це робить компоненти більш гнучкими та перевикористовуваними.

```html
<my-component>
  <span slot="user-name">Ім'я користувача</span>
</my-component>
```

```javascript
class MyComponent extends HTMLElement {
  constructor() {
    super();
    const shadowRoot = this.attachShadow({ mode: "open" });
    shadowRoot.innerHTML = `
      <style>
        ::slotted(span) { color: green; }
      </style>
      <slot name="user-name"></slot>
    `;
  }
}

customElements.define("my-component", MyComponent);
```

--

### Керування енкапсуляцією стилів

Shadow DOM дозволяє ізолювати стилі від основного документа, запобігаючи несподіваним перекриттям та конфліктам.

```javascript
// Створення Shadow DOM з ізольованими стилями
const shadowRoot = hostElement.attachShadow({ mode: "open" });
shadowRoot.innerHTML = `
  <style>
    p {
      color: red;
      font-size: 16px;
    }
  </style>
  <p>Цей текст буде червоним та 16px, і стилі не впливатимуть на основний документ.</p>
`;
```

---

## Custom Elements: основи створення

Створення користувацького елемента починається з визначення нового класу, що розширює `HTMLElement`. Цей клас потім реєструється з використанням `customElements.define`, що дозволяє використовувати його у HTML як будь-який інший тег.

--

### Синтаксис

```javascript
class MyCustomElement extends HTMLElement {
  constructor() {
    super(); // завжди викликайте super() спочатку в конструкторі
    // Додавання функціональності або ініціалізація стану
  }
}

// Реєстрація нового елемента
customElements.define("my-custom-element", MyCustomElement);
```

--

### Життєвий цикл користувацьких елементів (1/2)

Користувацькі елементи мають кілька життєвих циклів для керування їх станом:

- **connectedCallback**: Викликається, коли елемент вставлений у DOM.
- **disconnectedCallback**: Викликається, коли елемент видалений з DOM.
- **attributeChangedCallback**: Викликається, коли атрибути елемента змінюються.

--

### Життєвий цикл користувацьких елементів (2/2)

```javascript
class LifecycleElement extends HTMLElement {
  connectedCallback() {
    console.log("Елемент додано до DOM");
  }

  disconnectedCallback() {
    console.log("Елемент видалено з DOM");
  }

  attributeChangedCallback(name, oldValue, newValue) {
    console.log(`Атрибут ${name} змінено з ${oldValue} на ${newValue}`);
  }
}

customElements.define("lifecycle-element", LifecycleElement);
```

--

### Керування подіями та атрибутами

Користувацькі елементи можуть обробляти події та реагувати на зміни атрибутів, що надає їм інтерактивності та динамічності.

```javascript
class InteractiveElement extends HTMLElement {
  constructor() {
    super();
    this.addEventListener("click", this.onClick);
  }

  onClick(event) {
    console.log("Елемент натиснуто");
  }

  static get observedAttributes() {
    return ["custom-attr"];
  }

  attributeChangedCallback(name, oldValue, newValue) {
    name === "custom-attr" && this.updateAttribute(newValue);
  }

  updateAttribute(value) {
    // Оновити стан або стиль елемента відповідно до атрибута
  }
}

customElements.define("interactive-element", InteractiveElement);
```

---

## Приклад створення повноцінного компонента

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Інтерактивна користувацька картка</title>
  </head>
  <body>
    <!-- Використання компоненту користувацької картки -->
    <user-card
      name="Іван Іванов"
      email="ivan@example.com"
      image="path/to/image.jpg"
    ></user-card>

    <template id="user-card-template">
      <style>
        .card {
          font-family: "Arial", sans-serif;
          background: #f4f4f4;
          width: 300px;
          display: block;
          margin: 10px;
          box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
          border-radius: 3px;
          position: relative;
        }

        .card img {
          width: 100%;
          height: 200px;
          object-fit: cover;
          border-top-left-radius: 3px;
          border-top-right-radius: 3px;
        }

        .card .container {
          padding: 20px;
        }

        .card .container .name {
          font-size: 20px;
          color: #333;
        }

        .card .container .email {
          color: #555;
          font-size: 14px;
        }

        .delete-btn {
          position: absolute;
          top: 10px;
          right: 10px;
          border: none;
          background: red;
          color: white;
          border-radius: 50%;
          width: 30px;
          height: 30px;
          font-weight: bold;
          cursor: pointer;
        }
      </style>
      <div class="card">
        <button class="delete-btn">X</button>
        <img src="" alt="Фото користувача" />
        <div class="container">
          <p class="name"></p>
          <p class="email"></p>
        </div>
      </div>
    </template>

    <script>
      class UserCard extends HTMLElement {
        constructor() {
          super();
          const templateContent =
            document.getElementById("user-card-template").content;

          // Створення Shadow root
          const shadowRoot = this.attachShadow({ mode: "open" });
          shadowRoot.appendChild(templateContent.cloneNode(true));

          // Ініціалізація властивостей
          this.name = this.getAttribute("name");
          this.email = this.getAttribute("email");
          this.image = this.getAttribute("image");
        }

        connectedCallback() {
          this.updateCard();
          this.shadowRoot
            .querySelector(".delete-btn")
            .addEventListener("click", () => this.remove());
        }

        updateCard() {
          this.shadowRoot.querySelector(".name").innerText = this.name;
          this.shadowRoot.querySelector(".email").innerText = this.email;
          this.shadowRoot.querySelector("img").src = this.image;
        }
      }

      customElements.define("user-card", UserCard);
    </script>
  </body>
</html>
```

---

## Заключення та перспективи вебкомпонентів

--

### Загальні переваги використання вебкомпонентів

Вебкомпоненти надають потужний інструментарій для створення структурованих, інтерактивних та легко підтримуваних вебзастосунків. Їх основні переваги включають:

- **Інкапсуляція та модульність**: Вебкомпоненти забезпечують чітке розділення логіки, стилів та розмітки, що сприяє легкій підтримці та розширенню коду.
- **Перевикористання та стандартизація**: Можливість перевикористовувати компоненти в різних проєктах та контекстах забезпечує консистентність та знижує витрати на розробку.
- **Сумісність із майбутніми стандартами**: Вебкомпоненти є частиною вебплатформи, що забезпечує їхню стабільність та сумісність із майбутніми розширеннями та поліпшеннями.

--

### Вклад вебкомпонентів у стандартизацію та інновації

Вебкомпоненти активно сприяють стандартизації веброзробки, і роблять важливий вклад в єдність та інноваційність:

- **Єдність інтерфейсів**: Сприяють створенню консистентних та легко впізнаваних інтерфейсів користувача.
- **Інноваційність**: Забезпечують платформу для експериментів та впровадження нових ідей у вебдизайні та функціональності.
- **Зростання спільноти**: Сприяють обміну знаннями та ресурсами між розробниками, посилюючи розвиток вебтехнологій.
