#Kelp css guide

## Style guide
### CSS Selectors
Kelp and css should use the following syntax for selectors:
```css
namespace-itemName[__descendantName|--modifierName]
```

At first glance, it looks ugly. This format makes it easy to scan css selectors and makes it easy to keep specificity low. Why use double underscore/hyphen when one could work? Scannability.

#### itemName
This consists of the parent item.
#### itemName--modifierName
This does not replace the use of itemName. If you use a modifier, you must also include the non-modified one too:
```
<div class="itemName itemName--modifierName"></div>
```
#### itemName__descendantName
#### itemName.is-stateOfComponent
States should not be written as modifier names. Also, states should never be independently styled. This is a case where it's good to have specificity `0,2,0`.

### Don't use ID's
Nope. (except for hash anchor points or css `:target`).

### Don't nest selectors
Media queries and pseudoelements excepted.

### Kelp Namespaces
You can define your own namespace but make sure that it doesn't clash with these predefined kelp namespaces.

General namespace:
- **`is-`**: States of modules
- **`js-`**: Kelp modules (classes for js to use)

Kelp framework namespaces:
- **`K-`**: Kelp components
- **`k-`**: Kelp modules
- **`U-`**: General kelp utilities

User defined namespace:
- **`u-`**: User defined utilities
- **`r-`**: Responsive utilities

## Format
* Use one discrete selector per line in multi-selector rulesets.
* Include a single space before the opening brace of a ruleset.
* Include one declaration per line in a declaration block.
* Use one level of indentation for each declaration.
* Include a single space after the colon of a declaration.
* Use lowercase and shorthand hex values, e.g., `#aaa`.
* Use single or double quotes consistently. Preference is for double quotes,
  e.g., `content: ""`.
* Quote attribute values in selectors, e.g., `input[type="checkbox"]`.
* _Where allowed_, avoid specifying units for zero-values, e.g., `margin: 0`.
* Include a space after each comma in comma-separated property or function
  values.
* Include a semi-colon at the end of the last declaration in a declaration
  block.
* Place the closing brace of a ruleset in the same column as the first
  character of the ruleset.
* Separate each ruleset by a blank line.

```css
.selector-1,
.selector-2,
.selector-3[type="text"] {
  -webkit-box-sizing: border-box;
  -moz-box-sizing: border-box;
  box-sizing: border-box;
  display: block;
  font-family: helvetica, arial, sans-serif;
  color: #333;
  background: #fff;
  background: linear-gradient(#fff, rgba(0, 0, 0, 0.8));
}

.selector-a,
.selector-b {
  padding: 10px;
}
```

### Whitespace
Use two spaces for indentation and don't use tabs.

### Declaration Order
When declaring selectors, do so in this order:
```css
.selector {
  // Positioning
  position: absolute;
  z-index: 10;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;

  // Display & Box Model
  display: inline-block;
  overflow: hidden;
  box-sizing: border-box;
  width: 100px;
  height: 100px;
  padding: 10px;
  border: 10px solid #333;
  margin: 10px;

  // Other
  background: #000;
  color: #fff;
  font-family: sans-serif;
  font-size: 16px;
  text-align: right;
}
```
Do not include the comments. Newlines are necessary when there are 5 or more items; optional if less than 5.

### Exceptions and slight deviations

Large blocks of single declarations can use a slightly different, single-line
format. In this case, a space should be included after the opening brace and
before the closing brace.

```css
.selector-1 { width: 10%; }
.selector-2 { width: 20%; }
.selector-3 { width: 30%; }
```

Long, comma-separated property values - such as collections of gradients or
shadows - can be arranged across multiple lines in an effort to improve
readability and produce more useful diffs. There are various formats that could
be used; one example is shown below.

```css
.selector {
    background-image:
        linear-gradient(#fff, #ccc),
        linear-gradient(#f3c, #4ec);
    box-shadow:
        1px 1px 1px #000,
        2px 2px 1px 1px #ccc inset;
}
```

### Removing borders
When removing borders, set the border with a value of `0` instead of `none`. Setting it to `0` preserves the color and stroke type while `none` completely resets it to the initial style. With removing it using a `0`, the border (same stroke/color) can be added back on later by simply setting a border-width.

### Comments
Do not use block comments (`/* */`). Instead, use the double slash to write comments `//`.

### Specificity
The css selector syntax is designed to keep css specificity low (0,1,0). Use as few selectors as by adding classes to things when necessary.

### Embedding colors inside svg inline images
SVG images sometimes use as inline background values. You can use the sass variable interpolation to put a variable inside a svg fill. When doing so, package them into reusable units:
```css
@function kInlineImage-caret($fillColor) {
  @return url('data:image/svg+xml;utf8,<svg width="14" height="8" viewBox="0 0 14 8" xmlns="http://www.w3.org/2000/svg" xmlns:sketch="http://www.bohemiancoding.com/sketch/ns"><path d="M0 0l7 8 7-8h-14z" sketch:type="MSShapeGroup" fill="#{$fillColor}"/></svg>');
}
```
<!-- '}
```
 wah my syntax highlighter is messed up
-->

### Sass variables and functions
Sass variables and function should be prefixed with a `k` and have no dashes. Then, it should have the type of variable, a dash, then the variable name. For example:
```
$kColor-accent
```

Kelp already has a definition for a few types of variables/functions in sass:
- kColor
- kInlineImage

### Readability/clarity/usefulness over perfection
It is important to follow this styleguide as much as possible. Exceptions can and will occur though, and in those cases, use your best judgement and lean towards the side of making things easy for others to understand.


## em vs px vs rem
Whether to use em, px, or rem is a topic that there is no global consensus. px offers simplicity; em offers inheritance; and rem is good for zoom

That said, there are better ways to work. kelp will follow these conventions

### em
The `em` unit inherits its size from the font size of the current element. If not defined, it will be the inherited font sizes. Keep in mind that em will inherit from its parent and thus you must be careful to not have a cascading effect

`em` are good for used for:
- media queries
- buttons: padding should grow/shrink with the font size
- anything that you want to scale with font size

### px
The `px` unit is an absolute measurement of a css "pixel". 1px will always be 1px.

`px` is good for:
- zoom sensitive items:
  - borders -- so that when zoomed out they don't get rounded to 0
  - border radius
- gutters (for grids)
- absolutes that don't change

### rem
The `rem` unit inherits its size from the html base font size.

`rem` is good for browsers that give the option to zoom text only. However, this is mostly relevant to a previous era of web development (IE6,7) where text would only be zoomable if em or rem units inherited from the browser stylesheet. Now, most browsers zoom the reference pixel instead of the base font size. In mobile devices, zooming is often in the form of pinch to zoom. Due to these changes, rem provides very little benefit over px. Additionally, using rem requires conversion from px to rem, adding cognitive load for something basely affects users. Lastly, rem support is still a bit weird in some browsers.

### Rounding and subpixels
We should treat font sizes as if they were exact to the sub pixel. Browsers are moving towards that direction and even in ones that don't support subpixel rendering, font sizes still look and work mostly fine.

Borders should not be subpixels since many browsers currently can't handle these cases well.

## Typography

### line height
line height should be expressed as unitless numbers

## Credits & attribution
Shoutouts to a lot of the people who have written and taught about the practices that kelp uses. Specifically but not limited to: @necolas, @mdo, @fat.

- Style guide > CSS selector syntax: @necolas (suit), Yandex (BEM)
- Style guide > format (modified) (CC BY 3.0): @necolas (https://github.com/necolas/idiomatic-css)