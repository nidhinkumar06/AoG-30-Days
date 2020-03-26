<div align="center">
  <h1>Actions on Google - Day 26</h1>
  <p>PoC - Session Scheduler with Google Calendar - Part 2</p>
</div>

Now we will create the cloud functions for the session scheduler.

In your local terminal create a folder named `session scheduler` and navigate to the folder

Now initialize firebase using `firebase init` and select `functions` and select the `project`. Once the project is selected firebase will create the following files and nodes_modules folder

* index.js
* package.json


Now install the following dependencies using the command 

* `npm i actions-on-google`
* `npm i googleapis`
* `npm i dialogflow-fulfillment`

Once the plugins are installed navigate to `index.js` file and import the dependencies like below

```
 const functions = require('firebase-functions');
 const {google} = require('googleapis');
 const {WebhookClient} = require('dialogflow-fulfillment');

```

Once the files are imported we will setup the calendar service account credentials like below

```
// Set up Google Calendar Service account credentials
 const serviceAccountAuth = new google.auth.JWT({
   email: serviceAccount.client_email,
   key: serviceAccount.private_key,
   scopes: 'https://www.googleapis.com/auth/calendar'
 });

```

Once it is done we will set the timezone and timezoneoffset like below

```
 const calendar = google.calendar('v3');
 process.env.DEBUG = 'dialogflow:*'; // enables lib debugging statements
 
 const timeZone = 'America/Los_Angeles';
 const timeZoneOffset = '-07:00';

```

Once the timezone is added we will add the logic to parse the session name, date and timings and create an event in google calendar

```
exports.dialogflowFirebaseFulfillment = functions.https.onRequest((request, response) => {
   const agent = new WebhookClient({ request, response });
   console.log("Parameters", agent.parameters);
   const session_name = agent.parameters.SessionName;
   function makeAppointment (agent) {
     // Calculate appointment start and end datetimes (end = +1hr from start)
     //console.log("Parameters", agent.parameters.date);
     const dateTimeStart = new Date(Date.parse(agent.parameters.date.split('T')[0] + 'T' + agent.parameters.time.split('T')[1].split('-')[0] + timeZoneOffset));
     const dateTimeEnd = new Date(new Date(dateTimeStart).setHours(dateTimeStart.getHours() + 1));
     const appointmentTimeString = dateTimeStart.toLocaleString(
       'en-IN',
       { month: 'long', day: 'numeric', hour: 'numeric', timeZone: timeZone }
     );
 
     // Check the availibility of the time, and make an appointment if there is time on the calendar
     return createCalendarEvent(dateTimeStart, dateTimeEnd, session_name).then(() => {
       agent.add(`Ok, let me see if we can fit you in. ${appointmentTimeString} is fine!.`);
     }).catch(() => {
       agent.add(`I'm sorry, there are no slots available for ${appointmentTimeString}.`);
     });
   }
 
   let intentMap = new Map();
   intentMap.set('schedule event', makeAppointment);
   agent.handleRequest(intentMap);
});
```
In the above code we are getting the SessionName from the `SessionName` entity as well as the date and time of the event

once those details are received we are passing those details to the `createCalendarEvent()`

```
function createCalendarEvent (dateTimeStart, dateTimeEnd, session_name) {
   return new Promise((resolve, reject) => {
     calendar.events.list({
       auth: serviceAccountAuth, // List events for time period
       calendarId: calendarId,
       timeMin: dateTimeStart.toISOString(),
       timeMax: dateTimeEnd.toISOString()
     }, (err, calendarResponse) => {
       // Check if there is a event already on the Calendar
       if (err || calendarResponse.data.items.length > 0) {
         reject(err || new Error('Requested time conflicts with another appointment'));
       } else {
         // Create event for the requested time period
         calendar.events.insert({ auth: serviceAccountAuth,
           calendarId: calendarId,
           resource: {summary: session_name +' Session', description: session_name,
             start: {dateTime: dateTimeStart},
             end: {dateTime: dateTimeEnd}}
         }, (err, event) => {
           err ? reject(err) : resolve(event);
         }
         );
       }
     });
   });
 }
```

In the above code we are creating the event in `Google Calendar` with the serviceAccountAuth, calendarId, dataTime with session name and description about the session.

Once the above code is completed we need to add the service account details as well as the CaledarId which we got on `Day 25`

```
// Enter your calendar ID below and service account JSON below
 const calendarId = "// add your calendar id";
 const serviceAccount = { // add your service.json file details here};
```


Once it is done deploy your code to firebase using the command `firebase deploy`. Once your code is deployed copy the `webhook url` and paste it in Dialogflow webhook url.

Once it is done test your action 
