<div align="center">
  <h1>Actions on Google - Day 10</h1>
  <p>PoC - Show Current Location using Places API - Part 2</p>
</div>

We will continue for the Day 11 of adding the cloud functions to show the current location of a user

Create a new directory in your local machine and initialize the firebase cloud functions using the below command

`firebase init`

and then select the cloud functions and select the default project as the action name you have created. In my case it is Location Tracker

Once the basic things like

* `package.json`
* `index.js`
* `node_modules`

Once the above folders and files are created install the actions-on-google plugin using the below command in terminal `npm i actions-on-google`

Navigate to the `index.js` file and import the dependencies like below

```
const functions = require("firebase-functions");
const { dialogflow, Permission, SimpleResponse } = require("actions-on-google");

const app = dialogflow();
```

Now what we will do is when the user opens the actions we will get the user permission to get the current location of the user from google.

If the user says yes then we will take the location latitude and longitude

For that we need to refactor the `Default Welcome Intent` 

In the Default Welcome Intent remove the existing training phrases and add the below

```
where am i
locate me
```

Once it is added scroll down and enable the webhook

Now go to the `get_current_location` intent and click Events and select `actions_intent_permission` and remove all the training phrases


Now go to your cloud functions index.js file and create the `Default Welcome Intent`

```
app.intent("Default Welcome Intent", conv => {
  conv.data.requestedPermission = "DEVICE_PRECISE_LOCATION";
  conv.ask(new SimpleResponse('Welcome to location tracker'));
  return conv.ask(
    new Permission({
      context: "to locate you",
      permissions: conv.data.requestedPermission
    })
  );
});
```

In the above code we are getting the user permission to get the users current location from google

Once the the user says `Yes` or `No` it will trigger the `get_current_location` intent like below

```
app.intent("get_current_location", (conv, params, permissionGranted) => {
  if (permissionGranted) {
    const { requestedPermission } = conv.data;
    let address;
    if (requestedPermission === "DEVICE_PRECISE_LOCATION") {
      const { coordinates } = conv.device.location;
      console.log('coordinates are', coordinates);

      if (coordinates && address) {
        return conv.close(new SimpleResponse(`Your Location details ${address}`));
        // return conv.close(`Thank you for using Location Tracker`);
      } else {
        // Note: Currently, precise locaton only returns lat/lng coordinates on phones and lat/lng coordinates
        // and a geocoded address on voice-activated speakers.
        // Coarse location only works on voice-activated speakers.
        return conv.close("Sorry, I could not figure out where you are.");
      }
    }
  } else {
    return conv.close("Sorry, permission denied.");
  }
});
```

Now if we deploy this function using the command `firebase deploy` the function will be created in GCP Cloud functions.Once it is successfully created you will get the webhook url.

Now paste this url in Dialogflow -> Integrations -> Webhook

Now if you run the application you could see the latitude and longitude details like below

<div align="center">
  <video src="../../assets/day12/location1.mp4" width="500" height="350" controls preload></video>
</div>

Now we will use the geocoder reverse to get the address from latitude and longitude for that we will install a plugin named `node-geocoder`

Once the plugin is installed import the dependency like below

`const NodeGeocoder = require("node-geocoder");`

Once imported go to the `get_current_location` intent and add the below code

```
var options = {
  provider: 'google',
  httpAdapter: 'https', // Default
  apiKey: '', // for Mapquest, OpenCage, Google Premier
  formatter: 'json' // 'gpx', 'string', ...
};
    
var geocoder = NodeGeocoder(options);

geocoder.reverse({lat:coordinates.latitude, lon:coordinates.longitude }).then(function(res) {
  conv.ask(new SimpleResponse(res[0].formattedAddress));
}).catch(function(error) {
  console.log('error is', error);
});
```

Now if we deploy the latest changes and run the code we will get the output like below

<div align="center">
  <video src="../../assets/day12/location2.mp4" width="500" height="350" controls preload></video>
</div>


I am able to get the latitude and longitude but before getting the address the response is completed need to figure how to wait until the background process gets completed

### Resource Links

Dialogflow get currentlocation - `https://cloudops2pm.com/2018/07/26/get-location-in-google-home-assistant-app-using-dialogflow-v2-api/`
