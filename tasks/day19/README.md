<div align="center">
  <h1>Actions on Google - Day 19</h1>
  <p>PoC - Interactive Canvas-Stone-Paper-Scissor - Part 2</p>
</div>

Today we will create HTML,CSS and JS files needed for interactive canvas and we will try to pass the data from the HTML view to AoG using `InteractiveCanvas` `sendTextQuery`

Open the project which you have created and navigate to the `public` directory and create directory like below

* `mkdir css`
* `mkdir images`
* `mkdir js`

Navigate to `css` folder and create a new file named `index.css` and add the below code

```
  html {
    display: flex;
    height: 100%;
  }
  
  body {
    display: flex;
    flex: 1;
    margin: 0;
    background-color: white;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }
  
  div.container {
    width: 100%;
    text-align: center;
  }
  
  #welcome {
    display: block;
  }
  
  #welcome img {
    flex: 1;
    animation: rotate-anime 3s linear infinite;
  }
  
  @keyframes rotate-anime {
    0%  {transform: rotate(0);}
    100%  {transform: rotate(360deg);}
  }
  
  #vs {
    display: none;
  }
  
  #result {
    display: none;
  }
  
  .result-row {
    display: flex;
  }
  
  .result-row div {
    flex: 1;
  }
  
  #message {
    display: none;
    font-size: 48px;
  }
```

Now goto `index.html` file and add the below code

```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>Rock, Paper, Scissors!</title>
    <link rel="shortcut icon" type="image/x-icon" href="data:image/x-icon;," />
    <link rel="stylesheet" href="css/index.css" />
    <script src="https://www.gstatic.com/assistant/interactivecanvas/api/interactive_canvas.min.js"></script>
  </head>
  <body>
    <div class="container">
      <div id="welcome">
        <img src="images/rock.png" data-choice="rock">
        <img src="images/scissors.png" data-choice="scissors">
        <img src="images/paper.png" data-choice="paper">
      </div>
      <div id="vs">
        <img src="images/vs.png">
      </div>
      <div id="result">
        <div class="result-row">
          <div>
            <img id="user-choice" src="">
          </div>
          <div>
            <img id="action-choice" src="">
          </div>
        </div>
        <div id="message">
        </div>
      </div>
    </div>
    <script src="js/main.js"></script>
  </body>
</html>
```

In the `index.html` you could see 3 divs namely `welcome`, `vs`, `result`

In the `welcome` div you could there are 3 images namely rock, scissors and paper

We add a data-choice attribute for each img element. Each element has each one of rock, paper or scissors.

Now open the `js` directory and create a new file named `main.js` and inside the js file add the below code

```
'use strict';

document.querySelectorAll('#welcome img').forEach(img => {
    img.addEventListener('click', elem => {
        interactiveCanvas.sendTextQuery(elem.target.dataset.choice);
    });
});
```

What we have done in the above code is we are passing the selected image text to interactiveCanvas

Now the interactive canvas part is done now got to the `dialogflow console` and click `Intents` and select `Default Welcome Intent` and remove the `default response` and `Enable Webhook` 

Now in your local machine Navigate to the `functions` folder and navigate to `index.js` file and add the `Default Welcome Intent` like below

```
app.intent('Default Welcome Intent', conv => {
  conv.ask('Which do you want to show? Rock? Paper? Or, Scissors?');
  conv.ask(new HtmlResponse({
    url: `https://${firebaseConfig.projectId}.firebaseapp.com/`
  }));
});
```

Once the above intent is added deploy the cloud functions and HTML files to firebase hosting using the command `firebase deploy`

Once it is deployed you will see the output like below

### Demo

[Click here to watch the demo](https://firebasestorage.googleapis.com/v0/b/momtemplates.appspot.com/o/interactive-canvas-stone-paper%232-2020-03-19_22.19.43.mp4?alt=media&token=73bfd5e7-e430-4994-9a43-683d0a8903f3)


### Reference Links

Stone Paper Scissors CodeLabs - `https://yoichiro.github.io/codelabs/actions-with-interactive-canvas/?index=codelabs%2F#1`

