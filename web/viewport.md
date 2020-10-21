# Viewport

A **viewport** represents a polygonal (normally rectangular) area in computer graphics that is currently being viewed. In web browser terms, it refers to the part of the document you're viewing which is currently visible in its window (or the screen, if the document is being viewed in full screen mode). Content outside the viewport is not visible onscreen until scrolled into view.

## Visual viewport

The portion of the **viewport** that is currently visible is called the **visual viewport**. This can be smaller than the layout viewport, such as when the user has pinched-zoomed. The layout viewport remains the same, but the visual viewport became smaller. The visual viewport is the visual portion of a screen excluding on-screen keyboards, areas outside of a pinch-zoom area, or any other on-screen artifact that doesn't scale with the dimensions of a page.

## Layout viewport

The **layout viewport** is the viewport into which the browser draws a web page. Essentially, it represents what is available to be seen, while the **visual viewport** represents what is currently visible on the user's display device.

This becomes important, for example, on mobile devices, where a pinching gesture can usually be used to zoom in and out on a site's contents. The rendered document doesn't change in any way, so the layout viewport remains the same as the user adjusts the zoom level. Instead, the visual viewport is updated to indicate the area of the page that they can see.

**viewport** — видимая часть страницы.

По-умолчанию на мобильных устройствах ширина vieport равняется от 900 до 1000 пикселей. Зависит от устройства.

viweport изменяется при масштабировании.

```html
<meta name="viewport" content="width=device-width,maximum-scale=1, initial-scale=1">
```

* **width** — ширина viewport

* Значение **device-width** — ширина, оптимальная для этого устройства

* **initial-scale** — первоначальное приближение

* minimum-scale и maximum-scale

* user-scalable=no (можно ли вообще масштабировать, не рекомендуемый)

Не задавать элементам большую фиксированную ширину (больше минимально возможной ширины экрана, например 300 пикселей на экране мобильного телефона)

Использовать CSS Media Queries для определения различных стилей под большие и маленькие экраны.

