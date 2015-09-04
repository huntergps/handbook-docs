# Hello, $(UX Markup)!

Fuse can be used in two primary ways:

* @(Creating stand-alone apps) or prototypes with cross-platform JavaScript logic
* @(Creating reusable components), UI views and designs for native apps

The main representation of both these things in Fuse is through the use of UX Markup. 

UX Markup is an XML-based format that should be immediately familiar to anyone who has 
worked with similar formats. For an in-depth look at it's quirks and detailed features, make
sure you read:

* TODO: Links to detailed UX specs


## $(Creating stand-alone apps) : `<$(App)>`

When creating stand-alone apps or prototypes in Fuse, we use the `<App>` tag. 

	<App>
		<Text>Hello, world!</Text>
	</App>

Inside your `<App>` tag you can put any `Node`, `Behavior` or `Theme` tag. The above example
simply displays text using the default font.

The `<App>` tag itself bootstraps the app and takes care of application @(lifecycle) and @(Theme).

> ### The $(Background) property

The `Background` property controls the @(color) of the root view of the app.

> ### $(Lifecycle) events

TODO: Details on lifecycle events available on App


## $(Creating reusable components) : `$(ux:Class)`

When creating reusable components, we decorate our tags with the `ux:Class`
attribute:

	<Panel ux:Class="MyClass">
		<!-- code goes here -->
	</Panel>

This can be done on any node even inside other nodes:

	<Panel>
		<Panel ux:Class="FancyClass" />
	</Panel>

You can also split your components into seprate files. The root node of a n`.ux` file is implicitly a `ux:Class`, 
so if the following code lives inside `FancyPanel.ux`:

	<Panel>
		<!-- code goes here -->
	</Panel>

Then this is equivalent to:

	<Panel ux:Class="FancyPanel">
		<!-- code goes here -->
	</Panel>

### Reusing your component within Fuse

Once you've made a reusable class, you can use it like any other tag:

	<MyClass>
		<Panel />
		<FancyClass />
	</MyClass>

> ### Using your component in a native iOS (Xcode) app

TODO: Add info on this

> ### Using your component in a native Android Studio app

TODO: Add info on that



## @(Theme)s

@unodoc(Fuse.Theme)

`App` offers a setting called `Theme`, which specifies how standard components in
the app will look and feel. You set it like this:

	<App Theme="..name of theme..">

If not specified, `App` uses a plain `GraphicsTheme` by default.

### $(`NativeTheme`)

The `NativeTheme` will render


### $(`GraphicsTheme`)

By default, app uses a `GraphicsTheme`, which will render all components using
OpenGL ES. This will give your app an identical look on all platforms, with the 
exception of:

* `TextInput` - will be rendered using the platform's native text edit controls
* Status bars will behave differently across platforms

The main benefits of working with `GraphicsTheme` are:

* You can preview your app on Mac and PC using Fuse's realtime preview window,
  which offers a much smoother experience than the iOS and Android emulators.
* Your designs and animations will look and behave identically on all platforms.

Since `GraphicsTheme` is the default theme, you don't have to specify this explicitly,
but if you did, it would look like this:

	<App Theme="Graphics">

Or like this, which means exactly the same:

	<App>
		<GraphicsTheme />
	</App>

> #### Creating your own GraphicsTheme

It is possible to *extend* `GraphicsTheme` to specify specific
look and feels for controls like `Slider` and `Button`.

You can make your own `GraphicsTheme` by using it as a base class:

	<BasicTheme ux:Class="MyGraphicsTheme">
		<Button>
			<!-- how a button looks goes here -->
		</Button>
	</BasicTheme>

And use it like this:

	<App>
		<MyGraphicsTheme />
	</App>

You can also create a global alias for it, like this:

	<MyGraphicsTheme ux:Global="MyGraphics" />

And then this works:

	<App Theme="MyGraphics">
		...
	</App>

### $(`BasicTheme`)

The `BasicTheme` is a `GraphicsTheme` that ships with fuse and gives controls a 
material design-ish look and feel. This can be useful when you want a starting
point for UIs that are supposed to look the same on all platforms.

	<App Theme="Basic">


