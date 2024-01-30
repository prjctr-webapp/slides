---
title: "SASS"
customTheme: "theme"
width: 1280
height: 720
---

# Поглиблений огляд SASS

---

## Оператори та обчислення в SASS

--

### Використання операторів у SASS

Оператори в SASS дозволяють виконувати математичні та логічні операції прямо в стилях, що спрощує динамічне управління ними.

```scss
Математичні оператори:

$padding: 10px;
.container {
  padding: $padding / 2; // Результат: 5px
}

Логічні оператори:

$primary-color: red;
$use-red: true;
.button {
  color: if(
    $use-red,
    $primary-color,
    blue
  ); // Використовує червоний колір, якщо $use-red є true
}
```

--

### Обчислення та конкатенація значень в SASS

SASS дозволяє не тільки виконувати обчислення, але й з'єднувати різні значення та вирази для створення складних стилів.

```scss
Обчислення значень:

$width: 100px;
$margin: 20px;
.box {
  width: $width - $margin; // Результат: 80px
}

Конкатенація рядків

$base-font: "Arial";
$font-size: 16px;
.text {
  font: #{$font-size} #{$base-font}; // Результат: 16px Arial
}
```

---

### Умови та цикли

Умови та цикли в SASS дозволяють створювати складні та динамічні стилі, які автоматично адаптуються до різних умов.

```scss
// Умовні оператори:
$is-dark-theme: true;
.theme {
  background-color: if($is-dark-theme, black, white);
}

// Цикли для створення стилів:
@for $i from 1 through 3 {
  .margin-#{$i} {
    margin: $i * 10px;
  }
}
```

Генерує класи з різними значеннями відступів.

--

### Застосування для динамічного створення стилів

```scss
// Динамічна генерація стилів:
@each $color, $code in (red: #ff0000, green: #00ff00, blue: #0000ff) {
  .text-#{$color} {
    color: $code;
  }
}

// Адаптація стилів під умови:
$breakpoints: (
  small: 600px,
  medium: 900px,
  large: 1200px,
);
@each $size, $width in $breakpoints {
  @media (min-width: $width) {
    .container-#{$size} {
      max-width: $width;
    }
  }
}
```

---

### Змінні

Змінні в SASS надають можливість централізовано управляти значеннями, що спрощує зміну та підтримку стилів.

```scss
$primary-color: #3498db;
$secondary-color: #2ecc71;

.button {
  background-color: $primary-color;
  &:hover {
    background-color: darken($primary-color, 10%);
  }
}

.alert {
  color: $secondary-color;
}
```

--

### Міксини

Міксини в SASS дозволяють створювати перевикористовувані блоки коду з параметрами, що значно збільшує ефективність та модульність коду.

```scss
@mixin border-radius($radius) {
  -webkit-border-radius: $radius;
  -moz-border-radius: $radius;
  -ms-border-radius: $radius;
  border-radius: $radius;
}

.button {
  @include border-radius(3px);
}

.modal {
  @include border-radius(5px);
}
```

---

## Вкладеність стилів

--

### Використання батьківського селектора & в SASS

Батьківський селектор `&` у SASS дає змогу ефективно вкладати селектори, підвищуючи читабельність та зберігаючи ієрархічну структуру стилів.

```scss
.button {
  background-color: blue;
  &:hover {
    background-color: darken(blue, 10%);
  }
  &.active {
    background-color: red;
  }
}
```

--

### Стилізація псевдокласів та псевдоелементів у SASS

SASS дозволяє легко стилізувати псевдокласи та псевдоелементи, використовуючи вкладеність для збереження чистоти та організованості коду.

```scss
.link {
  color: blue;
  &:hover {
    color: darken(blue, 20%);
  }
  &:visited {
    color: purple;
  }
}

.button::before {
  content: "►";
  margin-right: 10px;
}
```
