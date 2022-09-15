# CSS units

## Fr

With [CSS Grid](grid.md) layout, we get a new flexible unit: the Fr unit. Fr is a fractional unit and `1fr` is for 1 part of the available space.

```css
.container {
  grid-template-columns: 1fr 1fr 40px 2fr;
  grid-template-rows: 100px 200px 100px;
}
```

As you saw in the previous examples, you can mix `fr` values with fixed and percentage values. The `fr` values will be divided between the space that's left after what's taken by the other values.

For example, if you have a grid with 4 columns as in the following snippet, the 1st column will be 300px, the second 80px (10% of 800px), the 3rd and 4th columns will be 210px (each occupying half of the remaining space):

```css
main {
  width: 800px;
  display: grid;
  grid-template-columns: 300px 10% 1fr 1fr;
  /* 300px 80px 210px 210px */

  grid-template-rows: auto;
}
```
