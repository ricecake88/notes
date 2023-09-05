# data-attribute

Can set an html attribute to anything data-\*, and then that can be used as a selector either in CSS or in javascript via `dataset`.

For example: (from w3schools):

```html
<!DOCTYPE html>
<html>
<head>
<script>
function showDetails(animal) {
  var animalType = animal.getAttribute("data-animal-type");
  alert("The " + animal.innerHTML + " is a " + animalType + ".");
}
</script>
</head>
<body>

<h1>Species</h1>
<p>Click on a species to see what type it is:</p>

<ul>
  <li onclick="showDetails(this)" id="owl" data-animal-type="bird">Owl</li>
  <li onclick="showDetails(this)" id="salmon" data-animal-type="fish">Salmon</li>  
  <li onclick="showDetails(this)" id="tarantula" data-animal-type="spider">Tarantula</li>  
</ul>

</body>
</html>
```

## CSS

In css, we can access the value in the "data-animal-type" by entering, for example:

```css
#owl::before {
    content: attr(data-animal-type)
}
```

This will add the content of "bird" in front of Owl to the list.
If for example, we wanted to modify this:

```css
#owl:hover:before {
    display: block
}
```