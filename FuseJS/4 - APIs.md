# APIs

## Storage

The storage API allows you to save text to files in the application directory.

### How to import

	var Storage = require('FuseJS/Storage');

### `write` function

Write a string value to specified file.

#### Syntax:

	Storage.write(filename, value)

* `filename` - Desired name of the file
* `value` - The content that will be written to the file

Returns a `Promise` with a `bool` which tells if the content was successfully written.

### Example usage:

	storage.write("filename.txt", "filecontent");

### `read` function

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
### `readSync`
### `deleteSync`


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
