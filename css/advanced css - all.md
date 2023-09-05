
[THREE PILLARS To Write Good HTML](#three-pillars-to-write-good-html)
[How CSS Works Behind the Scenes](#how-css-works-behind-the-scenes)  

Overview	3
How CSS is Parsed - Cascade and Specificity	4
Summary	6
CSS Value Processing	6
Inheritance in CSS	7
Questions to ask.	7
Example:	7
Summary:	7
Converting px to rem	8
Instead of using pixels, use percentage	8
How CSS gets Rendered	8
Box Model - how heights and widths are calculated	8
Box Types	9
Block	9
Inline-Block Type	9
Inline	9
Positioning Schemes	10
Normal Flow	10
Absolute Positioning	10
Floats	10
Stacking Contexts	10
How to structure and design CSS architecture, components and BEM	10
Think	11
Build	11
Architecting CSS	11
To Reset a Page:	12
Background-size	12
Background-Position	13
Pseudo Elements	14
Positioning of ::after pseudo element	14
Animation:	15
animation-fill-mode	15
animation	15


# THREE PILLARS To Write Good HTML
- Responsive Design
	- Write good design of one website that works beautifully across all devices Responsive design is a standard today. (Fluid layouts and media queries)
	- Also includes responsive images, correct units for font sizes or element dimensions, includes either desktop-first or mobile-first strategy.
- Write Maintainable and scalable code
	Important more for the developer than the user but this usually means
	- Clean
	- Easy-to-understand
	- Supports Growth
	- Above all is reusable for you and other developers. We need to think about our CSS architecture
		- How to Organize Files
		- How to Name Classes
		- How to Structure our Markup HTML
- Web Performance
	- Make it smaller in size and make it run more quicker and more efficiently.
	- Make as little http requests as possible, which means we need to include
	- Less code
	- Compressed code
	- Use a CSS preprocessor
	- Less images - this can most likely have the greatest impact. By only using images that are necessary for the web site or ap
	- Compressed images - when we do have to use the images, make sure it is compressed.

## How CSS Works Behind the Scenes

### Overview

User opens up page ->Browser loads the HTML -> Parses the HTML code into DOM (Document Object Model) - which describes the entire web document, like a family tree, with parents, children and sibling elements.

HTML also starts loading the CSS when it parses the HTML and then parses the CSS.

Load HTML -> Parse HTML -> Document Object Model
			|
		Load CSS -> Parse CSS

### Parsing CSS
- Resolve conflicting CSS declarations go through a process known as cascade.
- Process final CSS values (ie. converting a margin defined in percentage units to pixels). Example:  50% of a smartphone vs a screen is completely different, and must be calculated on the parsing phase on the user’s device.

**CSS is now stored in the CSS Object Model (CSSOM, similar to the DOM)**

**The DOM + CSSOM** = Render tree
After that, we have everything in order to render the page to the “Visual Formatting Model”.
Now it can display the final Rendered web site.

## How CSS is Parsed - Cascade and Specificity

**Cascade** - Process of combining different stylesheets and resolving conflicts between different CSS rules and declarations, when more than one rule applies to a certain element.

**Sources of CSS**: author (developer), User (change of default font size css in browser ie), default browser declarations (Browser user agent)

**Uses**:
Importance of CSS
Specificity
Source order
To determine which one takes precedence.

**Order**:
User !important declarations
Author !important declarations
Author declarations
User declarations
Default browser declarations

**Same Importance? **

When it comes to author declarations, those are the most common, and the order of looking at the items are as follows:
> 1. Inline styles
> 2. Ids
> 3. Classes, pseudo-classes, attribute
> 4. Elements, pseudo-elements
Example:
```csss
.button {
	font-size: 20px;
	color: white;
      background-color: blue;
}
nav #nav div.pull-right .button {
	background-color: green;
}
a {
	background-color: purple;
}
#nav a.button:hover {
	background-color: yellow;
}
```



Now, for each selector we count up how many inline, id, classes and elements there are.

**.button**:
Inline: 0
Ids: 0
Classes: 1
Elements: 0

**nav#nav div.pull-right .button**:  
Inline: 0
Ids: 1 (#nav)
Classes: 2 (.pull-right, .button)
Elements: 2 (nav, div)

**a**:
Inline: 0
Ids: 0
Classes: 0
Elements: 1 (a)

**#nav**:
Inline: 0
Ids: 1 (#nav)
Classes: 2 (.button, :hover)
Elements: 1 (a)

**How do we use the numbers to determine precedence?**
Example of going through the above example:
1. Look at numbers from left to right starting with the most specific selector of inline. If any of them has a 1, they win because they have the most specific category.
2. Inline has no selectors, so we move to ids.
3. Now there are two selectors with 1 ids. (The others have zero so we ignore them.  But both have a 1 in this category so we need to further determine which one takes precedence. 
4. The two selectors both have a 2 in their category which indicate a tie between them.
5, Moving on to the elements, finally, the `nav#nav selector` has 2 elements while the `#nav a.button:hover` selector only has one,  so the `nav#nav div.pull-right .button` is the MOST SPECIFIC element of all.

The result of a determination of specificity and precedence is called “**cascaded-value**.” because it’s the result from a cascade.

So background-color would be green.

**Same Specificity?**

In the case where both selectors had TWO elements and they had the same specificity, then the last declaration in the code will override all other declarations and will be applied.

**Summary**
- CSS declarations marked with !important have the highest priority when there are conflicted declarations
- Only use !important as a last resource. It’s better to use correct specificities - **more maintainable code!** * If something doesn’t work the way it should then look at your selector’s specificities to figure out what is going on
- Inline styles will always have priority over styles in external stylesheets
- A selector that contains **1** ID is more specific than one with 1000 classes 
- A selector that contains **1** class is more specific than one with 1000 elements
- The universal selector * has no specificity value (0,0,0,0)
- Rely more on **specificity** than on the _order_ of selectors
- But, rely on order when using 3rd- party stylesheets - always put your author stylesheet last. So in this case, order is hugely important.

## CSS Value Processing
- Each property has an initial value, used if nothing is declared (and if there is no inheritance)
- Browsers specify a root font-size for each page (usually 16px). A user-agent definition.
- Percentages and relatives are always converted to pixels
- Percentages are measured relative to their parent’s font-size, if used to specify font-size
- Percentages are measured relative to their parent’s width, if used to specify lengths
- em are measured relative to their parent font-size, if used to specify font-size.
- em are measured relative to the current font-size, if used to specify lengths
- rem are always measured relative to the document’s root font-size
- vh and vw are simply percentage measurements of the viewport’s height and width.

Reference: How CSS is Parsed, Part 2 Value Processing from Advanced CSS course in Udemy.

## Inheritance in CSS

Every CSS property must have a value.
**Questions to ask**:
_Is there a cascaded value?_
-> Yes? -> specified value = cascaded value.
-> No? -> is the property inherited? (specific to each property)
		-> Yes? Specified value = computed value of parent element

Example:
```css
.parent {
	Font-size: 20px;
	Line-height: 150%;
}
.child {
	Font-size: 25px;
}
```
Parent element -> 150% X 20px = 1.5*2 = 30px
For the child element, the line height will therefore also be 30 pixels, not 150% of the 25 pixel font size.

If it’s a property that’s not inherited, then the value will become the initial value, (ie. padding is simply 0), which is also specific to each property.

**Summary:**
- Inheritance passes the values for some specific properties from parents to children - more maintainable code
- Properties related to text are
	- inherited: font-family, font-size, color. 
	- Those that aren’t are like margins, paddings, etc.
- The computed value of a property is what gets inherited, NOT the declared value
- Inheritance of a property only works if no one declares a value for that property
- The inherit keyword forces inheritance on a certain property
- The initial keyword resets a property to its initial value

## Converting px to rem

Set root to font-size of 10px so it is easier to make calculations:
```
html {
font-size: 10px;
}
```
So with all the rest of the sizes, then we would need to divide it by 10 to calculate the rem of all the other elements, whatever the property would be.

**Instead of using pixels, use percentage**

Better to use a percentage since sometimes users will override the default font settings with their own. The default pixel size is 16px. If we set html font-size to 100%, the font size will be 16px. To get 10px, we will take 100/16 to get 0.625 which means 62.5% should be set as the font-size for html.
```css
html {
font-size: 62.5%;
}
```
:warning: Rems are not supported under Internet Explorer 9 and below.

## How CSS gets Rendered

**Visual Formatting Model:**

Algorithm that calculates boxes and determines the layout of these boxes, for each element in the render tree, in order to determine the final layout of the page.

This algorithm takes into account:
- Dimensions of the boxes (the box model)
- Box type: inline, block and inline-block
- Positioning scheme: floats and positioning
- Stacking contexts
- Other elements in the render tree
- Viewport size, dimensions of images, etc

### Box Model - how heights and widths are calculated

**Heights and Widths:**
**Total Width** = right border + right padding + specified width + left padding + left border
**Total Height** = top border + top padding + specified height + bottom padding + bottom border
**Example**:
> height = 0 + 20px + 100px + 20px + 0 = 140px
:warning: This means that whenever we define a height and width, the padding and the border get **added** to what we defined. 

But this isn’t practical.
Fix is to use the `box-sizing: border-box` parameter. Where the sizing specified **includes** the padding and the border.

### Box Types

#### Block (display: block)**

- Elements formatted visually as blocks
- 100% of parent’s width
- Vertically one after another
`display: block`
The below settings also produce block level items
```css
(display: flex)
(display: list-item)
(display: table)
```
Divs, paragraphs are set to block by default.
(Draw on this for a diagram ?)

#### Inline-Block Type (display: inline-block) ** 
- A mix of block and inline
- Occupies only content’s space
- No line-breaks
- But the box model applies as showed `display: inline-block`

#### Inline (display: inline)**
- Content is distributed in lines
- Occupies only content’s space
- No line breaks
- No heights and widths (cannot use the properties here)
- Paddings and margins only horizontal (left and right) `display:inline`
- 
### Positioning Schemes
#### Normal Flow
- Default positioning scheme;
- Not floated;
- Not absolutely positioned
- Elements laid out according to their source order
Default:
`position: relative`

#### Absolute Positioning
- Element is removed from the normal flow
- No impact on surrounding content or elements (can even overlap them)
- We use top, bottom left and right to offset the element from its relatively positioned container
`position: absolute`
`position: fixed`

#### Floats
- Element is removed from the normal flow;
- Text and inline elements will wrap around the floated element
- The container will not adjust its height to the element (solution to this is clearfixes)
`float: left`
`float: right`

#### Stacking Contexts
Forming layers to order how they are displayed.
How?
Use z-index, in relative or absolute display. Higher the number shows on top. Lower the number shows on bottom.
Transform, opacity value, a filter or other properties can also create other stacking contexts.

## How to structure and design CSS architecture, components and BEM

**Think -> Build -> Architect**
Think about the layout of your webpage or web app before writing code
Build your layout in HTML and CSS with a consistent structure for naming classes
Create a logical architecture for your CSS with files and folders.

### Think
Component-Driven Design
- Modular building blocks that make up interfaces (interface as a collection of components)
- Held together by the layout of the page
- Re-usable across a project and between different projects
- Independent allowing us to use them anywhere on the page, and should not depend on parent elements

### Build
- Name classes using **B**lock **E**lement **M**odifier
- **BLOCK**: standalone component that is meaningful on its own - components define in the the think section that are standalone and can be used anywhere in the project
- **ELEMENT**: part of a block that has no standalone meaning - This creates selectors with really low specificities because they are never nested. Great for reusability 
- **MODIFIER**: a different version of a block or an element. A flag that we can put on a block or element to make a difference from the other block and elements. (Example `.btn` is the block element, and then `.btn--round`)
Format:
```
.block {}
.block__element {}
.block__element--modifier {}
```

### Architecting CSS

Several methods, but he likes using 7-1 Pattern
7 different folders for partial Sass files
1 mailn Sass file to import all other files into a compiled CSS stylesheet.

7 folders (dependent on size and scope of project)
- base/
- components/
- layout/
- pages/
- themes/
- abstracts/
- vendors/

## To Reset a Page:

Resetting a page using the * selector

Common styling to reset a page:
```
*{
	margin: 0;
	padding:0;
	box-sizing: border-box;
}
```

:exclamation: `Border-box box-sizing` is set so that the width of the border is not added to the total height or width of the element.

## Background-size 

The background-size CSS property specifies the size of the background images. The size of the image can be fully constrained or only partially in order to preserve its intrinsic ratio.

Initial Value: auto auto
```css
/* Keywords syntax */
background-size: cover;
background-size: contain;

* One-value syntax */
/* the width of the image (height set to 'auto') */
background-size: 50%;
background-size: 3em;
background-size: 12px;
background-size: auto;

/* Two-value syntax */
/* first value: width of the image, second value: height */
background-size: 50% auto;
background-size: 3em 25%;
background-size: auto 6px;
background-size: auto auto;

/* Multiple backgrounds values by background-image */
/* Do not confuse this with background-size: auto auto */
background-size: auto, auto;
background-size: 50%, 25%, 25%;
background-size: 6px, auto, contain;

/* Global values */
background-size: inherit;
background-size: initial;
background-size: unset;
```

Resize the background image to cover the entire container, even if it has to stretch the image or cut a little bit off one of the edges

| Value |	Description |
|--------| ------------|
| auto | Default value | The background image is displayed in its original size|
|length| Sets the width and height of the background image. |The first value sets the width, the second value sets the height. If only one value is given, the second is set to "auto". Read about length units	|
|percentage| Sets the width and height of the background image in percent of the parent element. The first value sets the width, the second value sets the height. If only one value is given, the second is set to "auto"	|
|cover|	Resize the background image to cover the entire container, even if it has to stretch the image or cut a little bit off one of the edges	|
|contain| Resize the background image to make sure the image is fully visible	|
|initial| Sets this property to its default value. Read about initial	|
|inherit| Inherits this property from its parent element. Read about inherit|

## Background-Position

`background-position: top;` lines the background image to the top of the browser and always stays when minimizing the browser viewport.

`if background-position:bottom`; as a user uses the browser to navigate, the browser will only show starting from the bottom of the image and will cut off everything that doesn't fit the viewport size.

`background-position: center`; will always keep the image at the center of the viewport and cut off the edges.



## Pseudo Elements
Pseudo elements allow us to style certain parts of elements.

`::after` adds a virtual element right after the element that we’re selecting and once you master it you can make some pretty cool after effects.
Pseudo elements is basically treated like a child of the main element. They require content and display.
**Example**:
```css
.btn::after {
    content: "";
    display: inline-block;
    height: 100%;
    width: 100%;
}
```

So saying that one wants a height of 100%  and a width of 100%, then it will match the height and width of the parent element since it acts like a child.

We set the `button::after` to be behind the button. we set it to be height and width 100% to match the same height and width of the button. However, we want it to be hidden behind the button, which means we need to set the position to be absolute.

### Positioning of ::after pseudo element

When setting an element's position to be absolute, it is absolute **in relation to the first element above that is set to relative.**

The header in the example at this point is set to relative, but we actually want the ```button::after``` to be absolute in relation to button, so we want to set button position to relative.
In CSS, `::after` creates a pseudo-element that is the last child of the selected element. It is often used to add cosmetic content to an element with the content property. It is inline by default.
**Example**
```css
.btn:link, .btn:visited {
....
position: relative;
}

.btn::after {
...
position: absolute;
}
```

## Animation:
### animation-fill-mode
To start an animation at the start of 0% keyframe, use `animation-fill-mode: backwards` to start the element at 0% keyframe.

### animation
This is most useful when using an animation-delay.
Shortcut animation property can be used:
`animation: [name] [duration] [ease-in/ease-out/etc] [delay]`


Profile Ideas
https://meetsantana.netlify.app/
https://andrew.nonetoohappy.buzz/


Project Ideas
Festies for Foodies (Private)
Recipe App (Public)
TV/Movie Tracker (Public, for practice)
Skincare App (Private)

