<div align="center">
  <h1>Actions on Google - Day 16</h1>
  <p>PoC - Interactive Canvas-Basics - Part 3</p>
</div>

Today we will add sound to the interactive canvas when the user first sees the page to do that `Open functions` folder in your local machine and then open `index.js` file

Once the file is opened go the `Default Welcome Intent`

Add an audio tag like below

```
const sound = `<audio src='https://actions.google.com/sounds/v1/cartoon/cowbell_ringing.ogg'/>`;
```

Now move the default welcome message to a variable like below

```
 const spokenPrompt = 'Welcome! To Interactive Aliens';
```

Now combine both sound and text like the below code

```
 conv.ask(`<speak>${sound}${spokenPrompt}</speak>`);
```

Entire welcome intent will be like below

```
app.intent('Default Welcome Intent', (conv) => {
  const sound = `<audio src='https://actions.google.com/sounds/v1/cartoon/cowbell_ringing.ogg'/>`;
  const spokenPrompt = 'Welcome! To Interactive Aliens';
  conv.ask(`<speak>${sound}${spokenPrompt}</speak>`);
  conv.ask(new HtmlResponse({
    url: 'https://interactive-alien.firebaseapp.com',
  }));
});
```

Now deploy your code using the command `firebase deploy`

Now Test your action, you could see the audio would be played with the interactive canvas

[Click here to watch the demo](https://firebasestorage.googleapis.com/v0/b/momtemplates.appspot.com/o/interactive-canvas-audio-2020-03-17_22.38.53.mp4?alt=media&token=365950ea-dd61-4afc-bd55-5aebb43ddbe1)
 

