CSS: Custom Properties
Accent color (which is a new attribute)
Cascade Layers

Allows create variables and that you can reference those variables. Custom properties are a way of doing that without having to use SASS.

# CSS: Custom Properties
To define a variable in CSS:
add '--' to a name you come up with.

Example:
Variable declaration
```
:root {
	--buttonBackground: #560BAD;
	--buttonColor: #00FFDD;
}
```

To use the variable:
```
button {
	background: var(--buttonBackground);
	color: var(--buttonColor);
}
```

Scoping of these variables. With CSS variables, you can reset the variables anywhere and anytime you want. You would change it when you want to. Can override the variable say, another section where you don't want the buttonBackground to be the same. And it can be called in the same place.

```
.bg-dark {
	--buttonBackground: #4361E
}
```

We can change the variable values if it is very granular, very small.

```
.btn-primary {
	--buttonBackground: var(--pink);
	--buttonColor: var(--purple);
	}
	
}
```

# @property
Default properties are strings.
In transitions it is difficult - we cannot do the transition because they recognize the variables are a string, in the original syntax.

This can still be done with pure CSS, using ""@property"

give it a name
```
@property --houdiniPoint {
	syntax: '<percentage>';
	inherits: false;
	initial-value: 0%;
}
```
Based on the syntax property - it tells CSS that this is a value and not a string. CSS will detect if you try to reassign a string or a hex or something else when it was assigned a percentage before.

# accent-color
Very difficult to design for forms and get the right color. And now they give us different states for us. If using accent-color, the browser is going to standardize our inputs and use our colors that we specify and make sure that it is accessible.

```
:root {
	accent-color: var(--dark-redox);
}
```

We can check if this person's browser prefer a dark or light color scheme and can display a different accent-color.

# cascade layers
The nature of CSS is cool. I can go to any site and have my own custom rules, so I can browse the internet the way I like to see things.

The idea here is that we want to define the CSS where we want to. So it makes sense to have two CSS files with both a bunch of stuff and import them safely as long as I dictate the rules.

Introducing @layer which defines the ruels of how styles are going to be defined.

a. Assigned a layer to each of our design sheets
b. Define the order of the layers
c. last layer wins

Example, three wins precedence
```
@layer four, three;
@import 'cascade-3.css' layer(three);
@import 'cascade-4.css' layer(four);
```
Any rule you define outside a layer, it would take precedence even if it has higher specificity.

browser gives an anonymous layer for the rules defined outside of the layers.

# container queries
Cool thing about responsive design. It should be layered out due to the browser window, perhaps you are stacking columns based on how it was but once something shifts, you have to put things side by side (if the width is smaller).

I can define something based on the user's browser.

At a certain width we want the picture stacked on top of the text. For a wider text, we want the picture next to the text.
