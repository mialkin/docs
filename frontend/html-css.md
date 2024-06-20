# HTML/CSS

## Table of contents

- [HTML/CSS](#htmlcss)
  - [Table of contents](#table-of-contents)
  - [CSS](#css)
    - [`px`, `em`, `rem`](#px-em-rem)
      - [`px`](#px)
      - [`em`](#em)
      - [`rem`](#rem)
  - [Shortcuts](#shortcuts)
    - [HTML](#html)
      - [`.item`](#item)
      - [`.item*2`](#item2)
      - [`.box>.text*4`](#boxtext4)
      - [`main.main`](#mainmain)
      - [`.grid-item.footer`](#grid-itemfooter)
      - [`lorem3`](#lorem3)
    - [CSS](#css-1)
      - [`p`](#p)
      - [`p50-1`](#p50-1)
      - [`p1-2-3-4`](#p1-2-3-4)
      - [`ff`](#ff)
      - [`fz20`](#fz20)
      - [`fw:b`](#fwb)
  - [Figma](#figma)
    - [Canvas](#canvas)
    - [Frame](#frame)
      - [Layout grid](#layout-grid)
    - [Glossary](#glossary)
    - [Links](#links)

## CSS

### `px`, `em`, `rem`

[↑ The Battle of the Units: PX vs REM vs EM](https://dev.to/ochukodotspace/the-battle-of-the-units-px-vs-rem-vs-em-3ka8).

#### `px`

One pixel, `1px`, is defined as 1/96th of an inch in print media. On computer screens however, they are not usually related to actual measurements like centimeters and inches like you may think, they are just defined to be small but visible. What is considered visible is dependent on the device.

#### `em`

The `em` unit uses the current `font-size` of the parent element as its base. It can essentially be used to scale up or scale down the `font-size` of an element based on the `font-size` inherited from the parent.

#### `rem`

The `rem` works almost the same as `em`, but the main difference is that `rem` only references the `font-size` of the root element on the page rather than the parent's font-size.

The root `font-size` is the default `font-size` specified either by the user in their browser settings or by you, the developer.

The default `font-size` of a web browser is usually `16px` so therefore `1rem` will be `16px` and `2rem` will be `32px`. However, in a case where the root `font-size` is changed to `10px` for example; `1rem` will be `10px` and `2rem` will be `20px`.

## Shortcuts

### HTML

#### `.item`

```html
<div class="item"></div>
```

#### `.item*2`

```html
<div class="item"></div>
<div class="item"></div>
```

#### `.box>.text*4`

```html
<div class="box">
  <div class="text"></div>
  <div class="text"></div>
  <div class="text"></div>
  <div class="text"></div>
</div>
```

#### `main.main`

```html
<main class="main"></main>
```

#### `.grid-item.footer`

```html
<div className="grid-item footer"></div>
```

#### `lorem3`

```html
Lorem ipsum dolor.
```

### CSS

#### `p`

```css
padding: ;
```

#### `p50-1`

```css
padding: 50px 1px;
```

#### `p1-2-3-4`

```css
padding: 1px 2px 3px 4px;
```

#### `ff`

```css
font-family: ;
```

#### `fz20`

```css
font-size: 20px;
```

#### `fw:b`

```css
font-weight: bold;
```

## Figma

[↑ Figma](https://www.figma.com) is a collaborative web application for interface design, with additional offline features enabled by desktop applications for macOS and Windows.

### Canvas

The [↑ canvas](https://help.figma.com/hc/en-us/articles/360041064814-Explore-the-canvas) in Figma is the backdrop on which all of your frames, groups, and other layers live.

### Frame

A [↑ frame](https://help.figma.com/hc/en-us/articles/360041539473-Frames-in-Figma) is a foundational element for your designs that can act as a top-level container, like a device viewport, and/or represent areas or components within your design.

Frames allow you to choose an area of the canvas to create your designs in.

You can nest frames within other frames.

#### Layout grid

A [↑ layout grid](https://help.figma.com/hc/en-us/articles/360040450513-Create-layout-grids-with-grids-columns-and-rows) is a thing that helps to align objects within a frame.

### Glossary

- [↑ Alignment](https://en.wikipedia.org/wiki/Typographic_alignment). [↑ Выключка](https://ru.wikipedia.org/wiki/Выключка)
- [↑ Kerning](https://en.wikipedia.org/wiki/Kerning). [↑ Кернинг](https://ru.wikipedia.org/wiki/Кернинг)
- [↑ Leading](https://en.wikipedia.org/wiki/Leading) or line spacing. [↑ Интерлиньяж](https://ru.wikipedia.org/wiki/Интерлиньяж)
- [↑ Letter spacing](https://en.wikipedia.org/wiki/Letter_spacing) or tracking. [↑ Трекинг](https://ru.wikipedia.org/wiki/Трекинг_(типографика))

### Links

- [↑ Behance](https://www.behance.net), the world's largest creative network for showcasing and discovering creative work
- [↑ Pinterest](https://www.pinterest.com) – discover recipes, home ideas, style inspiration and other ideas to try
- [↑ Dribbble](https://dribbble.com) is a self-promotion and social networking platform for digital designers. It serves as a design portfolio platform, jobs and recruiting site, and is one of the largest platforms for designers to share their work online
