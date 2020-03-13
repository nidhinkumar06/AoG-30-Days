<div align="center">
  <h1>Actions on Google - Day 13</h1>
  <p>PoC - Date Time Conversation Helper - Part 1</p>
</div>

In this PoC i will Create an AoG action to get the date and time from user and parse it using converation helper in dialogflow

Create a new project named `datetime-conversation` like below

<div align="center">
  <img src="../../assets/day13/date-time-conv.png" alt="AoG" height="300">
</div>

Once the project is created click conversational

<div align="center">
  <img src="../../assets/day11/conversational.png" alt="AoG" height="300">
</div>

Once the conversational is selected click the invocation option at the left bar

<div align="center">
  <img src="../../assets/day13/invocation-name.png" alt="AoG" height="300">
</div>

Once the invocation name is given click the actions button and select create your action

<div align="center">
  <img src="../../assets/day11/buildactions.png" alt="AoG" height="300">
</div>

Now select the custom action which will navigate to the dialogflow console. In the dialogflow console check whether the agent name is datetime conversation or not

<div align="center">
  <img src="../../assets/day13/date-conv-dialogflow.png" alt="AoG" height="300">
</div>

If it is same click Create else give a name to the agent

Once the agent is created on the left side you will see Intent. Click `Intents`

You could see two intents select the `Default Welcome Intent`

Remove all the training phrases as well as the default response and enable webhook for the default welcome intent and click `Save`

Once it is done create a new intent named `datetime_handler` and enable the datetime helper in the event and then enable the webhook for the intent. It should be like below

<div align="center">
  <img src="../../assets/day13/datetime_handler.png" alt="AoG" height="300">
</div>