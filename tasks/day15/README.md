<div align="center">
  <h1>Actions on Google - Day 15</h1>
  <p>PoC - Interactive Canvas-Basics - Part 1</p>
</div>

In this PoC i will focus on how to create an canvas using HTML, CSS and Javascript

I will try to understand how a canvas is created in HTML

Let's begin

Create a new folder named ineractive canvas in your local machine, navigate to the folder and then create three files using the below command

* `touch index.html`
* `touch style.css`
* `touch canvas.js`

Now open the folder in any text editor and open the `index.html` file

Now we will create the skeleton in index.html like below

```
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">

  <title>Canvas</title>

</head>

<body>

</body>

</html>
```

Now we will add the `style.css` file for the canvas inside the head tag like below

```
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">

  <title>Canvas</title>

  <link rel="stylesheet" type="text/css" href="style.css">
</head>
```

Now we will add the script tag `canvas.js` file inside the head tag like below

```
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">

  <title>Canvas</title>

  <link rel="stylesheet" type="text/css" href="style.css">
  <script type="text/javascript" src="canvas.js" async></script>
</head>
```

Now inside the `body` tag we will create a new tag named `canvas` like below

```
<body>
  <canvas></canvas>
</body>
```

Now we will add some styles in `styles.css` for our html file like below

```
body {
  margin: 0;
  overflow-x: hidden;
}
```

Now go to `canvas.js` 

we will create the document selector for the canvas tag we have created earlier

```
const cvs = document.querySelector('canvas');
const c = cvs.getContext('2d');
```

Now we will add the width and height for the canvas like below

```
cvs.width = window.innerWidth;
cvs.height = window.innerHeight;
```

Will add some event listeners to adapt the canvas even if we resize the window like the below command

```
window.addEventListener('resize', function () {
  cvs.width = window.innerWidth;
  cvs.height = window.innerHeight;
});
```

We will track the mouse event whenever the user moves the mouse using the mousemove event like below

```
let mouse = {
  x: undefined,
  y: undefined
};

window.addEventListener('mousemove', function (e) {
  mouse.x = event.x;
  mouse.y = event.y;
});
```

Now we will create a class named Line and create some lines

```
class Line {
  constructor(x, y, offset) {
    this.x = x;
    this.y = y;
    this.offset = offset;
    this.radians = 0;
    this.velocity = 0.01;
  }

  draw = () => {
    //here we will control the shape
  }

  update = () => {
    //this is where we will control the movement and interactivity
  }
}
```

Now we will add animation for the canvas using `requestAnimationFrame` we will clear the canvas between each render using `c.clearReact()`

```
function animate() {
  requestAnimationFrame(animate);
  c.clearRect(0, 0, window.innerWidth, window.innerHeight);
  /* this is where we call our animation methods, such as  
  Shape.draw() */
};
animate();
```

Now we will add some background for our canvas in `style.css`

```
canvas {
  background: linear-gradient(45deg, #0d1011 0% 20%, #163486);
}
```

Now in the `Line` class, `draw` method we will add the code which makes the lines to come from top of the screen

```
draw = () => {
    c.strokeStyle = 'rgba(255, 255, 255, 0.5)';
    c.fillStyle = 'rgba(255, 255, 255, 0.3)';
    c.beginPath();
    c.arc(this.x, this.y, 1, 0, Math.PI * 2, false);
    c.fill();
    c.moveTo(this.x, this.y);
    c.lineTo(this.x + 300, this.y - 1000);
    c.stroke();
    c.closePath();
    this.update();
  }
```

To check whether it works or not we will create a simple line like below

```
const line = new Line(250, 800, 0);
line.draw();
```

Now we will try to generate 100 lines of code using the below code

```
const lineArray = [];

for (let i = 0; i < 100; i++) {

  const start = { x: -250, y: 800 };
  const random = Math.random() - 0.5;
  const unit = 25;

  lineArray.push(
    new Line(
      start.x + ((unit + random) * i),
      start.y + (i + random) * -3 + Math.sin(i) * unit,
      0.1 + (1 * i)
    )
  );
```

Now inside the `animate` method we will create draw lines 

```
function animate() {
  requestAnimationFrame(animate);
  c.clearRect(0, 0, window.innerWidth, window.innerHeight);
  lineArray.forEach(line => {
    line.draw();
  })
};

animate();
```

### Complete Code

```
const cvs = document.querySelector('canvas');
const c = cvs.getContext('2d');

cvs.width = window.innerWidth;
cvs.height = window.innerHeight;

window.addEventListener('resize', function () {
  cvs.width = window.innerWidth;
  cvs.height = window.innerHeight;
});

let mouse = {
  x: undefined,
  y: undefined
};

window.addEventListener('mousemove', function (e) {
  mouse.x = event.x;
  mouse.y = event.y;
});

class Line {
  constructor(x, y, offset) {
    this.x = x;
    this.y = y;
    this.offset = offset;
    this.radians = 0;
    this.velocity = 0.01;
  }

  draw = () => {
    c.strokeStyle = 'rgba(255, 255, 255, 0.5)';
    c.fillStyle = 'rgba(255, 255, 255, 0.3)';

    const drawLinePath = (width = 0, color) => {
      c.beginPath();
      c.moveTo(this.x - (width / 2), this.y + (width / 2));
      c.lineTo(this.x - (width / 2) + 300, this.y - (width / 2) - 1000);
      c.lineTo(this.x + (width / 2) + 300, this.y - (width / 2) - 1000);
      c.lineTo(this.x + (width / 2), this.y - (width / 2));
      c.closePath();
      if (c.isPointInPath(mouse.x, mouse.y) && color) {
        c.strokeStyle = color;
      };
    };

    drawLinePath(150, '#baf2ef');
    drawLinePath(50, '#dcf3ff');

    c.beginPath();
    c.arc(this.x, this.y, 1, 0, Math.PI * 2, false);
    c.fill();
    c.moveTo(this.x, this.y);
    c.lineTo(this.x + 300, this.y - 1000);
    c.stroke();
    c.closePath();

    this.update();
  }

  update = () => {
    this.radians += this.velocity;
    this.y = this.y + Math.cos(this.radians + this.offset);
  }
}

const lineArray = [];

for (let i = 0; i < 100; i++) {

  const start = { x: -250, y: 800 };
  const random = Math.random() - 0.5;
  const unit = 25;

  lineArray.push(
    new Line(
      start.x + ((unit + random) * i),
      start.y + (i + random) * -3 + Math.sin(i) * unit,
      0.1 + (1 * i)
    )
  );
};

function animate() {
  requestAnimationFrame(animate);
  c.clearRect(0, 0, window.innerWidth, window.innerHeight);
  lineArray.forEach(line => {
    line.draw();
  })
};

animate();
```

Now if you run the index.html file in your browser you will see the output like below

<div align="center">
   <img src="../../assets/day15/canvas.gif" alt="AoG" height="300">
 </div>

[Click here to watch the demo](https://firebasestorage.googleapis.com/v0/b/momtemplates.appspot.com/o/canvas-line-2020-03-15_22.12.48.mp4?alt=media&token=d6a7bc5c-ff68-4fbe-a447-0796a9aac925)


## Reference Link

HTML5-Canvas - `https://medium.com/@bretcameron/create-interactive-visuals-with-javascript-and-html5-canvas-5f466d0b26de`