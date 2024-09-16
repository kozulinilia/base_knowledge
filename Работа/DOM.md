- [[#closest|closest]]
- [[#querySelector|querySelector]]
- [[#Доступ к ДОМ элементу по ref|Доступ к ДОМ элементу по ref]]

# DOM

- closest когда нужно найти элемент, ребенком которого является target элемент
- querySelector когда нужно найти в родительском элементе ребенка
- Доступ к ДОМ элементу по ref

## closest

# [closest](https://learn.javascript.ru/searching-elements-dom#closest)

*Предки* элемента – родитель, родитель родителя, его родитель и так далее. Вместе они образуют цепочку иерархии от элемента до вершины.

Метод `elem.closest(css)` ищет ближайшего предка, который соответствует CSS-селектору. Сам элемент также включается в поиск.

Другими словами, метод `closest` поднимается вверх от элемента и проверяет каждого из родителей. Если он соответствует селектору, поиск прекращается. Метод возвращает либо предка, либо `null`, если такой элемент не найден.

Например:

```html
<h1>Содержание</h1>
<div class="contents">
  <ul class="book">
    <li class="chapter">Глава 1</li>
    <li class="chapter">Глава 2</li>
  </ul>
</div>
<script>
  let chapter = document.querySelector('.chapter'); // LIalert(chapter.closest('.book')); // ULalert(chapter.closest('.contents')); // DIValert(chapter.closest('h1')); // null (потому что h1 - не предок)
</script>
```

## querySelector

# [querySelector](https://learn.javascript.ru/searching-elements-dom#querySelector)

Метод `elem.querySelector(css)` возвращает первый элемент, соответствующий данному CSS-селектору.

Иначе говоря, результат такой же, как при вызове `elem.querySelectorAll(css)[0]`, но он сначала найдёт *все* элементы, а потом возьмёт первый, в то время как `elem.querySelector` найдёт только первый и остановится. Это быстрее, кроме того, его короче писать.

# [querySelectorAll](https://learn.javascript.ru/searching-elements-dom#querySelectorAll)

Самый универсальный метод поиска – это `elem.querySelectorAll(css)`, он возвращает все элементы внутри `elem`, удовлетворяющие данному CSS-селектору.

Следующий запрос получает все элементы `<li>`, которые являются последними потомками в `<ul>`:

```html
<ul>
  <li>Этот</li>
  <li>тест</li>
</ul>
<ul>
  <li>полностью</li>
  <li>пройден</li>
</ul>
<script>
  let elements = document.querySelectorAll('ul > li:last-child');for (let elem of elements) {alert(elem.innerHTML); // "тест", "пройден"}
</script>
```

## Доступ к ДОМ элементу по ref

доступ через $el

пример

```html
this.$refs["cheat-report-table"].$el
```