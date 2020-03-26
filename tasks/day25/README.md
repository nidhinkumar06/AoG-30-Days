<div align="center">
  <h1>Actions on Google - Day 25</h1>
  <p>PoC - Session Scheduler with Google Calendar - Part 2</p>
</div>

We will create an Session Scheduler action by creating an event in Google Calendar

Create an new Actions on Google project like below

<div align="center">
  <img src="../../assets/day25/session1.png" alt="AoG" height="300">
</div>

Once the project is selected click `Conversational` and then click Invocation name and give the invocation name for your action

<div align="center">
  <img src="../../assets/day25/session2.png" alt="AoG" height="300">
</div>

Once the invocation name is created click `Actions` and then select `Get Started` which will navigate to Dialogflow


<div align="center">
  <img src="../../assets/day25/session3.png" alt="AoG" height="300">
</div>

<div align="center">
  <img src="../../assets/day25/session4.png" alt="AoG" height="300">
</div>

Once the screen in navigated to dialogflow check whether the agent name is same as your action name


<div align="center">
  <img src="../../assets/day25/session-scheduler.png" alt="AoG" height="300">
</div>

Click `Create` and Click `Intents` and select a new intent named `Schedule event` and in the training phrase add like the below image

<div align="center">
  <img src="../../assets/day25/schedule-event.png" alt="AoG" height="300">
</div>


Once you add the `Training phrase` you could see some highlighters for the training data which are the `system entities`

Once it is done scroll down click `Action and Parameters` and check all the `system entities` and add the prompts for it

<div align="center">
  <img src="../../assets/day25/date-params.png" alt="AoG" height="300">
</div>

<div align="center">
  <img src="../../assets/day25/time-params.png" alt="AoG" height="300">
</div>


Prompt is used to check whether the user has given the correct phrase or not

Once the `Actions and parameters` are created scroll down to the Default Response and add the response like the below image

<div align="center">
  <img src="../../assets/day25/response.png" alt="AoG" height="300">
</div>

Click `Save` for the intent and goto the Dialogflow settings you can see a option called `Google Cloud` click on it which will navigate to GCP console

<div align="center">
  <img src="../../assets/day25/google-project-id.png" alt="AoG" height="300">
</div>

Now click on APIS and Services and select `Calendar API`

<div align="center">
  <img src="../../assets/day25/api&services.png" alt="AoG" height="300">
</div>

Click the `Calendar API` and enable the API

<div align="center">
  <img src="../../assets/day25/calendarapi.png" alt="AoG" height="300">
</div>

Once the API is created click on the `Create new Credential`

<div align="center">
  <img src="../../assets/day25/credentials.png" alt="AoG" height="300">
</div>

Now give the appname and description and select the service account type as `JSON` and select `Create a new key`

<div align="center">
  <img src="../../assets/day25/service-account1.png" alt="AoG" height="300">
</div>

<div align="center">
  <img src="../../assets/day25/sevice-json.png" alt="AoG" height="300">
</div>

Once the JSON file is created  go to  `Google Calendar` and create a new calendar named `Session Scheduler`

<div align="center">
  <img src="../../assets/day25/create-new-calendar.png" alt="AoG" height="300">
</div>

Fill the name and description as well as select the timezone and click save

<div align="center">
  <img src="../../assets/day25/create-calendar.png" alt="AoG" height="300">
</div>

Once the Calendar is created click the `Settings option` and in `Add people section` paste the `client_email` from the `service-account.json` which you have downloaded earlier

<div align="center">
  <img src="../../assets/day25/add-people.png" alt="AoG" height="300">
</div>

Now we have completed the setup of Google Calendar and Dialogflow.We will see how to create cloud functions for the Session scheduler action on `Day 26`