---
tags:
  - practice
permalink: false
---


🛠 Session Storage в реальных проектах используется достаточно редко, но иногда может быть полезен. Например, если мы не хотим терять данные если пользователь случайно обновил страницу.

<p class="codepen" data-height="265" data-theme-id="light" data-default-tab="js,result" data-user="akellbl4" data-slug-hash="mdRXYgj" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="Session Storage Example">
  <span>See the Pen <a href="https://codepen.io/akellbl4/pen/mdRXYgj">
  Session Storage Example</a> by Pavel Mineev (<a href="https://codepen.io/akellbl4">@akellbl4</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

🛠 Иногда нам нужно сохранить не просто текст, а целую структуру данных, и в этом нам поможет `JSON.stringify`.

```js
const user = {
  name: "Doka Dog",
  avatarUrl: "mascot-doka.svg"
};

sessionStorage.setItem("user", JSON.stringify(user))
```

И после чтения парсим:

```js
function readUser() {
  const userJSON = sessionStorage.getItem("user")

  if (userJSON === null) {
    return undefined
  }

  // Если вдруг в хранилище оказался невалидный JSON предохраняемся от этого
  try {
    return JSON.parse(userJSON)
  } catch (e) {
    sessionStorage.removeItem("user")
    return undefined
  }
}
```

🛠 Если ваш сайт использует скрипты аналитики или другие внешние библиотеки, то они так же имеют доступ к хранилищу. Поэтому лучше именовать ключи для записи в хранилище с префиксом и в едином стиле. Например если мы записываем что-то на этом сайте я бы выбрал префикс `YD_{название ключа}`, так можно сгруппировать только те значения что нам нужны или применить фильтр в инструментах разработчика.

🛠 Используйте функции обёртки для предотвращения ошибок связанных с неудачными попытками записи, отсутствия Session Storage в браузере и дублирования кода.

```js
function getItem(key, value) {
  try {
    return window.sessionStorage.getItem(key)
  } catch (e) {
    console.log(e)
  }
}

function setItem(key, value) {
  try {
    return window.sessionStorage.getItem(key, value)
  } catch (e) {
    console.log(e)
  }
}

function setJSON(key, value) {
  try {
    const json = JSON.stringify(value)

    setItem(key, json)
  } catch (e) {
    console.error(e)
  }
}

function getJSON(key) {
  try {
    const json = getItem(key)

    return JSON.parse(json)
  } catch (e) {
    console.error(e)
  }
}
```