# Polyfills

FuseJS executes in a (minimum) EcmaScript 5.1 environment on all supported platforms.
There is no web browser involved, FuseJS only provides a subset of the browser's standard libraries.

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


## Polyfills

FuseJS provides polyfills for some features (typically browser features) that are mainly provided to make
$(third party libraries) work. These implementations are not neccessarily complete at this point, but check the @(FuseJS Roadmap) to see what the status is for development.

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


> ### $(File)

<!-- TODO: There used to be an asterisk here, AUTH, what is that about? -->

FuseJS has support for the `File` API:

* [`File`](https://developer.mozilla.org/en-US/docs/Web/API/File)
