Json-Pollock
============

The **Joson-Pollock** package renders live DOM elements out of JSON accrding to the [Structured Messaging Templates RFC](https://lpgithub.dev.lprnd.net/lp-mobile/Structured-Messaging-Templates) (work in progress)

Installation
-------

    npm i json-pollock --save


In order to consume it do one of the following:

A script tag:

    <script src="path-to-node-modules/dist/json-pollock.min.js"></script>

Using [RequireJS](http://requirejs.org/):

Map the JsonPollock path in the RequireJs config, and then:
	

    require(["JsonPollock"],(jsonPollock) => {
    ...
    })
Using [CommonJS](http://requirejs.org/docs/commonjs.html):

    const jsonPollock = require("JsonPollock");

Usage
-------

**init**

You can call the *init* function if you want to configure JsonPollock - it is not mandatory, if you won't call it JsonPollock will be initialized with defaults.

    JsonPollock.init({
	     maxAllowedElements: 50    // max DOM elements that will be rendered
			               // other elements will be ignored, default is 50.
     });

**render**

The *render* function renders json into a DOM element.

 

    const json = {
     "type": "vertical",
      "elements": [{
        "type": "image",
        "url": "http://assets/phone.jpg",
        "tooltip": "Great Phone!",
        "action": {
          "type": "navigate",
          "name": "Navigate to store via image",
          "lo": 23423423,
          "la": 2423423423
        }
      }]
    }
    const rooEl = JsonPollock.render(json);
    document.getElementById('container').appendChild(rooEl);

**registerAction**

The *registerAction* function allow to register a callback to a certain action type, as defined in the [RFC](https://lpgithub.dev.lprnd.net/lp-mobile/Structured-Messaging-Templates#actions).

    const linkCallback = (data) => {
	    window.open(data.uri,"_blank")
    };
    JsonPollock.registerAction('link', linkCallback);

Error Handling
-------
*JsonPollock.render()* will throw an Error if it fails from any reason, the error object will have a *message* property that will give the error description.

Perior to the rendering the JSON object is validated against the JSON [schema](js/schema)  , if it fails to validate the error object will also include an *errors* property that will hold the validation errors.

    ...
    try {
	    const rooEl = JsonPollock.render(json);
	    ...
	} catch(e) {
		console.log(e.message);    // error message
		console.log(e.errors);     // validation errors
	}