# CSS data types

**CSS data types** define typical values (including keywords and units) accepted by CSS properties and functions. They are a special kind of component value type.

In formal syntax, data types are denoted by a keyword placed between the inequality signs "<" and ">".

## \<length>

The `<length>` CSS data type represents a distance value. Lengths can be used in numerous CSS properties, such as `width`, `height`, `margin`, `padding`, `border-width`, `font-size`, and `text-shadow`.

### Syntax

The `<length>` data type consists of a `<number>` followed by one of the units listed below. As with all CSS dimensions, there is no space between the unit literal and the number. The length unit is optional after the number `0`.

> Some properties allow negative `<length>`s, while others do not.

### Relative length units

Relative lengths represent a measurement in terms of some other distance. Depending on the unit, this can be the size of a specific character, the line height, or the size of the viewport.

#### Font-relative lengths

Font-relative lengths define the <length> value in terms of the size of a particular character or font attribute in the font currently in effect in an element or its parent.

## Links

[↑ \<length>](https://developer.mozilla.org/en-US/docs/Web/CSS/length)