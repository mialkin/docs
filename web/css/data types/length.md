# \<length>

The `<length>` CSS data type represents a distance value. Lengths can be used in numerous CSS properties, such as `width`, `height`, `margin`, `padding`, `border-width`, `font-size`, and `text-shadow`.

## Syntax

The `<length>` data type consists of a `<number>` followed by one of the units listed below. As with all CSS dimensions, there is no space between the unit literal and the number. The length unit is optional after the number `0`.

> Some properties allow negative `<length>`s, while others do not.

## Relative length units

Relative lengths represent a measurement in terms of some other distance. Depending on the unit, this can be the size of a specific character, the line height, or the size of the viewport.

### Font-relative lengths

Font-relative lengths define the <length> value in terms of the size of a particular character or font attribute in the font currently in effect in an element or its parent.

#### **em**

`em` represents the calculated font-size of the element. If used on the font-size property itself, it represents the inherited font-size of the element.

## Absolute length units

#### **px**

One pixel. For screen displays, it traditionally represents one device pixel (dot). However, for *printers* and *high-resolution screens*, one CSS pixel implies multiple device pixels. 1px = 1/96th of 1in.

Most Web site authors have traditionally thought of a CSS pixel as a device pixel. However as we enter this new high DPI world where the entire UI may be magnified, a CSS pixel can end up being multiple pixels on screen.

For example if I set a zoom magnifcation of 2x, then 1 CSS pixel would actually be represented by a 2×2 square of device pixels.

## Links

[↑ \<length>](https://developer.mozilla.org/en-US/docs/Web/CSS/length)