# CSS Grid

Когда есть CSS Grid, больше не нужны:

1. Layout из CSS-фреймворков типа Bootstrap, Semantic UI и др.
2. В большинстве случаев – библиотеки Grid Layout типа Mansonry
3. 100500 гайдов про то, как центрировать элементы, типа https://howtocenterincss.com

4. Свыше 9000 руководств про верстку разметки равными по высоте колонками и "прибитыми" к низу подвалу (т.н. The Holy Grail Layout).

**CSS Grid** — управление блоками по двум осям:

1. Inline or row axis (justify-*)
2. Block or column axis (align-*)

## Терминология

* Track
  * Row
  * Column
* Grid line
* Grid area
* Gutter (gaps)

**Track** — кумулятивное понятие грида и колонок

**Grid line** — линия, которая ограничивает колонки и ряды

**Grid area** — прямоугольное множество ячеек

**Gaps** или **gutters** — расстояния между ячеек (как вертикальное, так и горизонтальное)

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <title>Document</title>
        <link href="css/layout.css" rel="stylesheet" />
        <link href="css/resets/normalize.css" rel="stylesheet" />
        <link href="css/resets/typography.css" rel="stylesheet" />
        <link href="css/resets/reset.local.css" rel="stylesheet" />
    </head>
    <body>
        <div class="layout">
            <header>HEADER</header>
            <main>CONTENT</main>
            <section>
                <header>LINKS</header>
                <h3>TAGS</h3>
                <h3>CATEGORIES</h3>
                <h3>SOCIAL LINKS</h3>
            </section>
            <footer>FOOTER</footer>
        </div>
        <div class="layout2">
            <header>HEADER</header>
            <main>CONTENT</main>
            <section>
                <header>LINKS</header>
                <h3>TAGS</h3>
                <h3>CATEGORIES</h3>
                <h3>SOCIAL LINKS</h3>
            </section>
            <footer>FOOTER</footer>
        </div>
    </body>
</html>
```

```css
.layout {
    display: grid;
    grid-template-columns: repeat(12, 1fr);
    grid-template-rows: 120px 1fr 120px;
}

.layout > header {
    grid-column: 1/13;
    background-color: yellow;
}

.layout > main {
    grid-column: 1/10;
    background-color: red;
}

.layout > section {
    grid-column: 10/13;
    background-color: aqua;
}

.layout > footer {
    grid-column: 1/13;
    background-color: thistle;
}

.layout2 {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    grid-template-rows: 120px 1fr 120px;
    grid-template-areas: "h h h h"
                         "c c c s"
                         "f f f f";    
}

.layout2 > header {
    grid-area: h;
    background-color: gray;
}

.layout2 > main {
    grid-area: c;
    background-color: lightblue;
}

.layout2 > section {
    grid-area: s;
    background-color: lightcyan;
}

.layout2 > footer {
    grid-area: f;
    background-color: lightpink
}
```
