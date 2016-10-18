# Optimization

Goal of the project is to Obtain a pagespeed score of at least 90 on the Portfolio index page and increase FPS and reducing Jank on the Pizza Page.


## Getting Started
No Build tools were used during this project and accordingly no `dist` directory exists. The Site is hosted locally and [Ngrok](ngrok.com) is used to securely tunnel to the localhost.

### Temporary Localhost
Website is temporarily hosted using the python `SimpleHTTPServer` module:

`python -m SimpleHTTPServer`

### Secure Tunnel to Localhost
Download and Unzip [Ngrok](https://ngrok.com/download).

Run Ngrok and point to SimpleHTTPServer Port (default:8000)

`./ngrok http 8000`

Follow the link provided to access the Localhost.

## Index page

A pagespeed of was 97/100 achieved by making the following changes:

- CSS inlined (all of it)
- JS async attribute added to all script elements.
- `img/profilepic.jpg` and  `images/pizza.png` were resized and compressed with Imagemagick


## Pizza page
Efforts focused on reducing Jank and achieving 60 FPS.


### Background Pizzas

- The number of Background pizzas elements created is reduced to from 200 to 33 and assigned to `var backgroundPizzas= 33;`

- The background `pizza.png` resized to `70 x 100` allowing the removal of the height and width styles.

```
for (var i =  backgroundPizzas; i--;) {
  var elem = document.createElement('img');
  elem.className = 'mover';
  elem.src = "images/pizza70x100.png";
  elem.basicLeft = (i % cols) * s;
  elem.style.top = (Math.floor(i / cols) * s) + 'px';
  document.querySelector("#movingPizzas1").appendChild(elem);
}
```

- The `updatePositions()` function was optimized by moving scrollTop Calculation outside the for-loop.

- Creation of the `items` array  by `document.querySelectorAll(".mover")` change to the more efficient method `document.getElementsByClassName("mover")`

- For loop construction changed to decrement the iterator `for (var i =  backgroundPizzas; i--;)`.

```
function updatePositions() {
  frame++;  
  var items = document.getElementsByClassName("mover");
  var sTop =  document.body.scrollTop/1250;
  for (var i =  backgroundPizzas; i--;) {
    var phase = Math.sin(sTop + (i % 5));
    items[i].style.left = items[i].basicLeft + 100 * phase + 'px';
  }
  }
}
```

### Pizza Resizing Slider
- Creation of the `pizzaElements` array changed by `document.getElementsByClassName("randomPizzaContainer")` method.

- Moved `determineDx` function Call and the `newwidth` calculation outside the loop and selected only the first `randomPizzaContainer` element to be representative of all the other elements to be resized.

- For loop construction changed to decrement the iterator

```
function changePizzaSizes(size) {
  var dx = determineDx(document.querySelector(".randomPizzaContainer"), size);
  var newwidth = (document.querySelector(".randomPizzaContainer").offsetWidth + dx) + 'px';
  var pizzaElements = document.getElementsByClassName("randomPizzaContainer");

  for (var i = pizzaElements.length;i--;) {

    document.getElementsByClassName("randomPizzaContainer")[i].style.width = newwidth;
  }
}
```
