<div align="center">
  <h1>Actions on Google - Day 10</h1>
  <p>PoC - Show Current Location using Places API - Part 1</p>
</div>

In this PoC i will create an AoG action to show the current location of the user using Places API

In the Part 1 i will create the dialogflow part

Create a new project named `Location Tracker` like below

<div align="center">
   <img src="../../assets/day11/locationtracker.png" alt="AoG" height="300">
</div>

Once the project is created click conversational

<div align="center">
   <img src="../../assets/day11/conversational.png" alt="AoG" height="300">
</div>

Once the conversational is selected click the invocation option at the left bar

<div align="center">
   <img src="../../assets/day11/invocation-name.png" alt="AoG" height="300">
</div>

Once the invocation name is given click the actions button and select create your action

<div align="center">
   <img src="../../assets/day11/buildactions.png" alt="AoG" height="300">
</div>

Now select the custom action which will navigate to the dialogflow console. In the dialogflow console check whether the agent name is Location Tracker or not

<div align="center">
   <img src="../../assets/day11/locationtracker-dialogflow.png" alt="AoG" height="300">
</div>

If it is same click Create else give a name to the agent

Once the agent is created on the left side you will see Intent.Click Intents

You could see two intents select the `Default Welcome Intent`

<div align="center">
   <img src="../../assets/day11/open-default-welcome-intent.png" alt="AoG" height="300">
</div>

Now scroll down you could see some default responses like 

<div align="center">
   <img src="../../assets/day11/default-welcome-response.png" alt="AoG" height="300">
</div>

Delete all the default response and now add a response like below

`Hi! Welcome to Location Tracker`

Now click the `+` button in the intent and give a new intent name `get_currentlocation`

<div align="center">
   <img src="../../assets/day11/currentlocation-intent.png" alt="AoG" height="300">
</div>

Now in the training phase add some locations by default the `@sys.location` entity should match up.

Scroll down and enable the webhook for the intent

Once it is done save the intent.

Now test your action you will get the default response like below

<div align="center">
   <img src="../../assets/day11/intial-response.png" alt="AoG" height="300">
</div>

Now we have done with the dialogflow setup next we will set up the cloud function for this actions.

Catch it up on Day 12 post