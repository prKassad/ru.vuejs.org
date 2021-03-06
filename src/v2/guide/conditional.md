---
title: Условный рендеринг
type: guide
order: 7
---

## v-if

В строковых шаблонизаторах, например в Handlebars, мы бы определили условно отображаемый блок так:

``` html
<!-- шаблон Handlebars -->
{{#if ok}}
  <h1>Yes</h1>
{{/if}}
```

Во Vue для тех же целей применяется директива `v-if`:

``` html
<h1 v-if="ok">Да</h1>
```

Также можно добавить блок "иначе", используя `v-else`:

``` html
<h1 v-if="ok">Да</h1>
<h1 v-else>Нет</h1>
```

### Использование v-if с псевдоэлементом template

Поскольку `v-if` — это директива, она должна быть указана для одного конкретного тега. Но что если мы хотим управлять отображением сразу нескольких элементов? В этом случае мы можем применить `v-if` к псевдоэлементу `<template>`, который служит невидимой обёрткой, и сам в результатах рендеринга не появляется.

``` html
<template v-if="ok">
  <h1>Заголовок</h1>
  <p>Абзац 1</p>
  <p>Абзац 2</p>
</template>
```

### v-else

Для указания блока "иначе" для `v-if` можно использовать директиву `v-else`:

``` html
<div v-if="Math.random() > 0.5">
  Сейчас меня видно
</div>
<div v-else>
  А теперь — нет
</div>
```

Элемент с директивой `v-else` должен непосредственно следовать за элементом с директивой `v-if`, в противном случае он не будет опознан.

## v-show

Ещё одну возможность условного отображения даёт директива `v-show`. Используется она так же:

``` html
<h1 v-show="ok">Привет!</h1>
```

Разница состоит в том, что элемент с `v-show` будет всегда оставаться в DOM, а изменяться будет лишь свойство `display` в его параметрах CSS.

<p class="tip">Обратите внимание, что `v-show` не поддерживает использование `<template>` и не работает с `v-else`.</p>

``` html
<div v-show="Math.random() > 0.5">
  Видишь суслика? А он есть...
</div>
```

## v-if vs. v-show

`v-if` производит "настоящий" условный рендеринг, удостоверяясь что слушатели и дочерние компоненты внутри условного блока должным образом уничтожаются и воссоздаются при изменении истинности управляющего условия.

`v-if` также **ленив**: если условие ложно на момент первоначального рендеринга, он не произведёт никаких действий - условный блок не будет отображён, пока условие впервые не станет истинным.

`v-show`, напротив, куда проще — элемент всегда присутствует в DOM, и только CSS-свойство переключается в зависимости от значения выражения.

Говоря обобщённо, стоимость переключения `v-if` выше, в то время как для `v-show` выше стоимость первичного рендеринга. Так что если переключения предполагаются частыми — то используйте `v-show`, если же редкими или вовсе маловероятными — то `v-if`.
