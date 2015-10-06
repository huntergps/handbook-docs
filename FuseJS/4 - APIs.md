# APIs

<!-- ## EcmaScript features

TODO for the below: write some basic inline docs & examples, then point to the MDN docs for more info

> * : May have limited functionality compared to browser implementations -->

## $(fetch)

This is the main way to do HTTP requests.

<!-- TODO: (note Fetch and FetchJson will be renamed and de-emphasized) -->

### Example usage:

This is an example on how to take a JavaScript object, POST it as JSON and receive JSON data which is turned back into a JavaScript object using `fetch`:

	var status = 0;
	var response_ok = false;

	fetch('http://example.com', {
		method: 'POST',
		headers: { "Content-type": "application/json"},
		body: JSON.stringify(requestObject)
	}).then(function(response) {
		status = response.status;  // Get the HTTP status code
		response_ok = response.ok; // Is response.status in the 200-range?
		return response.json();    // This returns a promise
	}).then(function(responseObject) {
		// Do something with the result
    }).catch(function(err) {
    	// An error occured parsing Json
	});

* Complete documentation for [`fetch`](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)


> ### $(XMLHttpRequest)

FuseJS supports `XMLHttpRequest`, go here for more information:

* https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest for more info


> ### $(Promise)

FuseJS has support for promises, go here for more information:

* [`Promise`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)

<!-- TODO: Cleanup the difference between polyfill and API -->

## Polyfills

FuseJS executes in a (minimum) EcmaScript 5.1 environment on all supported platforms.
There is no web browser involved, FuseJS only provides a subset of the browser's standard libraries. FuseJS provides polyfills for some features (typically browser features) that are mainly provided to make $(third party libraries) work. These implementations are not neccessarily complete at this point, but check the @(FuseJS Roadmap) to see what the status is for development.

> ### $(setTimeout)

To call a function in 2 seconds:

	setTimeout(function() { alert("Alert"); }, 2000);

You can also use `setTimeout` to create loops:

	function poller() {
		// Do work (...)

		// And again in 1 second
		setTimeout(poller, 1000);
	}

* [`setTimeout`](https://developer.mozilla.org/en-US/docs/Web/API/WindowTimers/setTimeout)


## Storage

The storage API allows you to save text to files in the application directory.

### How to import

	var Storage = require('FuseJS/Storage');

### $(Storage.write:write)

Write a string value to specified file.

#### Syntax:

	Storage.write(filename, value)

* `filename` - Desired name of the file
* `value` - The content that will be written to the file

Returns a `Promise` with a `bool` which tells if the content was successfully written.

### Example usage:

	storage.write("filename.txt", "filecontent");

### $(Storage.read:read)

#### Syntax:

	Storage.read(filename)

* `filename` - The file that should be read

Returns a `Promise` with a `string` containing the content of the file.

### Example usage:

	storage.read("filename.txt").then(function(content) {
		console.log(content);
	}, function(error) {
		console.log(error);
	});

#### Remarks

The actual folder where the file will be saved will vary depending on platform.

### `writeSync`

Synchrounously write data to the application folder:
	
	storage.writeSync("filename.txt", "filecontent");
	
> Warning: This call will block, if you are writing large amounts of data, use @(Storage.write).

### `readSync`

Synchronously read data from a file in the application folder:

	var data = storage.readSync("filename.txt");
	
> Warning: This call will block, if you are writing large amounts of data, use @(Storage.read).

### `deleteSync`

Delete a file from the application folder:

	storage.deleteSync("filename.txt");

## $(Lifecycle)

You can react to lifecycle events using JavaScript:

	var Lifecycle = require('FuseJS/Lifecycle');
	Lifecycle.onEnteringForeground = function() {
  		initialize();	
	};

The following lifecycle events will be raised:

- $(onEnteringForeground) - the app starts or is coming back from being in the background
- $(onEnteringBackground) - the app is going into the background
- $(onEnteringInteractive) - 
- $(onExitedInteractive) - 
- $(onTerminating) - the app has been asked to terminate

## $(Phone)

You can use the Phone-API to request a $(Phone.call:call) initiation:

	var Phone = require('FuseJS/Phone');
	Phone.call("number");

## Camera

Allows you to take pictures with the device's camera.

### How to import

	var Camera = require('FuseJS/Camera');

### Example usage:

	Camera.takePicture({ targetWidth: 640, targetHeight: 360, correctOrientation:true }).then(function(file)
	{
		// file is a standard JavaScript File object
		// file.path contains the path to the saved image on the device
	}).catch(function(e) {
		console.log(e);
	});

### `takePicture` function

Takes a picture with the device's camera.

#### Syntax:

	Camera.takePicture({ targetWidth: width, targetHeight: height, correctOrientation: shouldCorrect })

The option object can contain the following properties:

* `targetWidth` - Desired width of the picture, in pixels. If omitted, the default is used.
* `targetHeight` - Desired height of the picture, in pixels. If omitted, the default is used.
* `correctOrientation` - True if you want to correct orientation of the picture

Returns a `Promise` of a `File`.

#### Remarks

The method returns a `Promise` that if successful yields a `File` object that points to the
image saved as a JPG image on disk.

Note that the `targetWith` and `targetHeight` are just hints, the returned image can be any size.

## Vibration

Allows you to use the devices vibration functionality.

### How to import

	var Vibration = require('FuseJS/Vibration');

### Example usage:

	Vibration.vibrate(200);

Vibrates for 200 milliseconds.

<!-- TODO: Document localstorage -->