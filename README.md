[![Gutter Grid logo](http://gutter-grid.net/assets/images/social-media.jpg)](http://gutter-grid.net)

``````
npm install gutter-grid --save
``````

Gutter Grid is a flexbox based grid system for building fully responsive grid layouts with highly customisable gutters. Even though it is powered by flexbox, it features `display:table`, `float:left` and `display:inline-block` backups for legacy browsers (when enabled) to prevent your site's layout from blowing up even when viewed in IE8 and 9.

To read the full documentation go to http://gutter-grid.net

## Change Log

### Version 4.0.0

This is a big update. CSS Grid is killing off almost all need for Gutter Grid. IE 11 is essentially the only reason why Gutter Grid is still at all worth using right now.
With IE 8 and 9 essentially dead, it doesn't make sense to hamper the experience of IE11 developers for the sake of these dead browsers.
I'm optimising the code to make the experience as enjoyable as possible for IE11 devs. Unfortunately it means the defaults will be different depending on if `$grid-legacy-support` is enabled or not.
If the legacy setting is turned on then Gutter Grid will behave in mostly the same way as it did in version 3.x.

#### Breaking changes:

Note: these breaking changes mostly apply only when the `$grid-legacy-support` is set to `false` (the default setting).

- New default settings
  - `$align` in the mixin now defaults to `left`.
  - `$wrap` in the mixin now defaults to `true` but only if `$cols` has been defined.
  - Grids no longer stretch by default. In order to make the grid stretch like it did in v3.x, if using the classes system, a `grid--stretch` class needs to be added to the `.grid` element. If using the mixin system, a `$stretch: true` setting needs to be called in the mixin. This is only necessary if columns have been defined.
  - Grids with a column count setting will now wrap by default unless if wrapping is explicitly disabled or `$grid-legacy-support` is enabled.
- The `grid--noGrowth` class has been renamed `grid--noStretch` to align with the new `grid--stretch` class. (A breaking change for **everyone**).
- The `$grow` setting in the mixin has been renamed to `$stretch`. (A breaking change for **everyone**).
- Multi-spanning cells now flex-grow by default. (A breaking change for **everyone**).
- `$breakpoints` parameter in the mixin has been moved to the 3rd slot. (A breaking change for **everyone**).
- `$grid-break-points` setting now has new syntax that makes it easier to tell what column count a particular set of breakpoints is for. Below is an impracticle example showing the new format:

  **Version 3.x**

  ````scss
  $grid-break-points: (
    // No media queries for 1 column grid
    (false),

    // 2 column grid breakpoints
    (
      // At 600px wide screen and below, make columns 100% wide
      100%: 600px,
    ),

    // No media queries for 3 column grid
    (false),

    // 4 column grid breakpoints
    (
      50% : 960px,
      100% : 600px,
    )

    // No media queries for a 5 column grid
    false,
  )
  ````

  **Version 4.0.0**

  ````scss
  $grid-break-points: (
    // No need to mention a 1 column grid

    // 2 column grid breakpoints
    2: (
      // At 600px wide screen and below, make it a 1 column grid
      // (using percentages here instead of column count still works)
      1: 600px,
    ),

    // No need to mention a 3 column grid

    // 4 column grid breakpoints
    4: (
      2 : 960px,
      1 : 600px,
    )

    // No media queries for 5 column grid
    // Only needed if using the class system
    5: false,
  )
  ````

#### New features:

- Instead of listing percentage values as keys in the `$breakpoints` parameter key/value pairs, you can now simply list the number of columns that  the grid should have as the keys in the key/value pairs. The old method of using percentages as the key values will still work if that method is preferred.

  **Version 3.x (Still works in version 4)**

  ````scss
  @include grid(7, $breakpoints: (
    // On a 960px wide screen or below, the columns will be 25% wide
    25% : 960px,
    // You can use mq-scss syntax here as well
    50% : (max, 600px),
    100% : 480px,
  ));
  ````

  **Version 4.x**

  ````scss
  @include grid(7, $breakpoints: (
    //On a 960px wide screen or below, there will be 4 columns
    4 : 960px,
    // You can still use mq-scss syntax here as well
    2 : (max, 600px),
    1 : 480px,
  ));
  ````

- `calc` is now used by default to determine column widths. So instead of seeing `width: 33.33%;` in your styles, you will see `width: calc(100% / 3);`. This makes it clear from the styles how many columns are being generated. For example it is much clearer that `width: calc(100% / 7);` equals 7 columns rather than `width: 14.2857%;`.

- New config variable `$grid-calc-support` has been introduced. Since `calc` [isn't supported in every browser](https://caniuse.com/#feat=calc) (thanks Opera Mini and Android 4) a new `$grid-calc-support` setting has been added. This defaults to whatever the opposite of the `$grid-legacy-support` setting is. This means that if legacy support is needed then `$grid-calc-support` will automatically be disabled. If you need to support Opera Mini (or you prefer seeing percentages in your styles), set `$grid-calc-support` to false.


## Version 3.0.0

  - Gutter Grid will now install whatever version of mq-scss is available (defaulting to the latest version)
  - **Breaking change:** You now need to set the `$grid-legacy-support` setting to `true` if you wish to support browsers that do not support flexbox natively. (Change made to drastically reduce the amount of bloat found in the output css).

## Version 2.0.0

### What's new?

  - Gutter Grid now comes in mixin flavour!
  - Gutters can now have different vertical vs horizontal gutter widths
  - Column media queries can now take [mq-scss](https://www.npmjs.com/package/mq-scss) syntax for defining completely custom breakpoints
  - Improved legacy browser support
  - Reduced specificity on class based selectors for easier overriding of styles

### v2.0.0 Breaking changes

#### Reduced specificity in class names

If upgrading, none of the class names have changed, however the reduced specificity will mean that you may need to update your styles to not conflict with the new grid system.

#### New format for assigning column break points

The new format to the column break points is a bit different to the original. before it was a list of screen-sizes then column widths with a space in between each one.

Version 1.x

`````
(
  600px 100%,
)
`````

Column width is now stated first, and screen width is stated second. By default, the screen width will be calculated as a `max-width` media query. However, you can now provide [mq-scss](https://www.npmjs.com/package/mq-scss) syntax as an alternative to stating a screen width pixel value. This means that you can define the column break points using just about any media query you like. Taking a mobile first `min-width` approach will break legacy browser support though unless you have [UnMQ](https://github.com/jonathantneal/postcss-unmq) integrated into your build process.

Version 2.x

`````
(
  100% : 600px, //state a max-width pixel value
  //or
  100% : (max, 600px), //provide a custom mq-scss media query
)
`````

#### New format for assigning gutter break-points

The new way of assigning breakpoints to gutters isn't much different different to the original method. The only incompatible difference being that now you need to write `mq` with a space after it before the list of values (the space is vital).

Version 1.x

`````scss
$grid-cell-gutters: (
    'mediaQueryGutter' : (
        50px,
        30px (max, 800px),
        10px (max, 700px)
    )
);
`````

Version 2.x

`````scss
$grid-cell-gutters: (
    'mediaQueryGutter' : mq (
        50px,
        30px (max, 800px),
        10px (max, 700px)
    )
);
`````
