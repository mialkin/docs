# `px`, `em`, `rem`

[↑ The Battle of the Units: PX vs REM vs EM](https://dev.to/ochukodotspace/the-battle-of-the-units-px-vs-rem-vs-em-3ka8).

## `px`

One pixel, `1px`, is defined as 1/96th of an inch in print media. On computer screens however, they are not usually related to actual measurements like centimeters and inches like you may think, they are just defined to be small but visible. What is considered visible is dependent on the device.

## `em`

The `em` unit uses the current `font-size` of the parent element as its base. It can essentially be used to scale up or scale down the `font-size` of an element based on the `font-size` inherited from the parent.

## `rem`

The `rem` works almost the same as `em`, but the main difference is that `rem` only references the `font-size` of the root element on the page rather than the parent's font-size.

The root `font-size` is the default `font-size` specified either by the user in their browser settings or by you, the developer.

The default `font-size` of a web browser is usually `16px` so therefore `1rem` will be `16px` and `2rem` will be `32px`. However, in a case where the root `font-size` is changed to `10px` for example; `1rem` will be `10px` and `2rem` will be `20px`.
