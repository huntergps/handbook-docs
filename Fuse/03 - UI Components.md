# UI Components

Fuse comes with a number of UI components that can be used to construct a user interface. In UX you can add UI components by enclosing them in tags:

	<Image File="MyImage.png" />
	<Rectangle Width="50" Height="50" Fill="#888" />
	<Text>Hello world!</Text>
	
Note that just because something is enclosed in a tag doesn't necessarily mean it has to be a UI component. UX uses tags for other concepts also, such as triggers and actions.

> ## Videos on this topic

## Text

To render text in a minimal app:

	<App>
		<Text>Hello, world!</Text>
	</App>

If you want to add longer passages of text, such as a _Lorem Ipsum_, you discover that you in certain cases want to enable word wrapping. In Fuse, this is done with the `$(TextWrapping)` property on the `Text` control:

	<Text TextWrapping="Wrap">Lorem Ipsum(...)</Text>

If you find that wrapping the text still makes it hard to show all the contents you want, you probably want to look at adding the contents to a @(ScrollView), or changing @(FontSize). `TextWrapping` can be set to `Wrap` and `NoWrap` (default).

### $(Fonts)

You can import fonts from ttf files containing TrueType fonts. Because a font is typically referred to throughout an application, it is best to simply create a _global @(Resources:resource)_ for it.

	<App>
		<Font File="Roboto-Medium.ttf" ux:Global="Medium" />
		<Font File="Roboto-Light.ttf" ux:Global="Light" />
		<StackPanel>
			<Text Font="Light">Roboto Light</Text>
			<Text Font="Medium">Roboto Medium</Text>
			<Text Font="Light">Roboto Light again</Text>
		</StackPanel>
	</App>

In this example, the fonts are located in the same directory as the relevant UX file. This way of importing the font ensures that the font is available through the whole project, and is only loaded once.

Both iOS and Android support text rendering with multibyte character sets. This means that emojis work fine rendering on device. 

> Note! There are currently some issues rendering multibyte character sets using desktop preview. Do not be surprised if the desktop rendering doesn't match device rendering 100%. This is an issue that is being worked on.

### Text properties

For further control over how your text is rendered, you can set `$(TextAlignment)`, `$(TextColor)` and `$(FontSize)`:

	<Text TextColor="#999">Left</Text>
	<Text TextAlignment="Center">Center</Text>
	<Text FontSize="24" TextAlignment="Right">Right</Text>		

In this example, the first text element will be left aligned as this is the default, and have its color set to a medium light grey. The second text will be center aligned. The third will be right aligned and have a larger font. Valid values for `TextAlignment` are `Left` (default), `Right` and `Center`.

## $(Image)

To display an image:

	<App>
		<Image File="FuseLogo.png" />
	</App>

This code assumes the file `FuseLogo.png` lives in the same directory as the UX-file. If you would rather load the contents of the image from the Internet, simply:

	<Image Url="http://path_to_image" />

> _Note!_ If you come from a background as a web developer you might be used to assigning a URL to a `src`-attribute. While `Image` has a `Source`-property, it is used to assign a `Resource` to an image. In this context, this `Resource` is a `HttpImageSource`, but that is created behind the covers for you automatically, so stick to the `Url`-property to load contents from the web.

> ### Image contents from resources

TODO: Link/Move parts to DataBinding chapter(?) I think there was the idea to keep it here for now. Especially after the code was simplified

For a small example of other ways to load image data, here is a small example that also uses databinding from JavaScript:

	<App>
		<FileImageSource ux:Key="pic2" File="Pictures/Picture2.jpg" />
		<StackPanel>
			<JavaScript>
				module.exports = {
					pictureResource: {key: "pic2"},
					url: "http://somewhereontheinternet/Cute-Cat.jpg"
				}
			</JavaScript>
			<Image File="Pictures/Picture1.jpg" />
			<Image Source="{DataToResource pictureResource.key}" />
			<Image Url="{url}" />
		</StackPanel>
	</App>

This app will show three `Image`s stacked on top of each other. The topmost picture will be fetched as a normal file. Then we create a `FileImageSource` that we bind to a picture using `DataToResource`, which looks up the resource for `pictureResource.key` and binds it to the `Image`. We also get the URL for a picture on the web and bind it to the `Url`-property of an image. If this looks complicated, don't fret: We'll look more at @(DataBinding) and @(JavaScript) shortly.

> ### Image Color

It is possible to tint an `Image` by using the `Color`-property. Note, that this will affect the parts of the image that is closest to white in the most predictable way. For example:

	<Image File="WhiteIcon.png" Color="#f00" />
	
This will turn a white icon red.

### $(StretchMode)

When added to a container, an `Image` will by default try to show as much as itself as possible. If the image isn't the same aspect as its container, there will be parts of the container that will not be covered.

There are a number of ways to address this issue. You can set the `StretchMode`-property on your `Image` to make it behave differently. Here are the different `StretchMode`s:

- `Fill` - Fill the available area in the container without necessarily preserving aspect ratio.
- `PixelPrecise` - TODO: Explain @mortoray?
- `PointPrecise` - TODO: Explain
- `PointPrefer` - TODO: Explain
- `Scale9` - Link to external documentation for this?
- `Uniform` - This will make the picture as large as possible while preserving aspect ratio. This will often make the `Image` not cover the whole parent.
- `UniformToFill` - Fill the parent container while preserving aspect ratio. This will often mean that parts of the picture are left out.

> ### Multi density images

Because devices have widely different resolutions, Fuse allows you to specify multiple image resources for the same logical `Image`:

	<Image>
		<MultiDensityImageSource>
			<FileImageSource File="Icon.png" Density="1"/>
			<FileImageSource File="Icon.png@2x.png" Density="2"/>
		</MultiDensityImageSource>
	</Image>
	
Fuse will then pick the resource best suited for the device in question.

TODO: Explain how Fuse decides which png to choose? 

> ### Memory policy

TODO: Explain @mortoray?

> ### HttpImageSource

Q: Is this different from just using `Url`?

## $(Shapes)

TODO: Talk about the shape API?

Fuse comes with functionality for doing shape rendering. All shapes can have multiple `Fill`s and `Stroke`s, these will be layered.

### $(Rectangle)

To draw a `Rectangle`:

	<Rectangle Fill="#f00" />

In this example, the `Rectangle` will take up as much space as it is allowed by its parent and fill it with `#f00`.

If you want to have the `Rectangle` limit itself, you can add `Width` and `Height`:

	<App Background="#000">
		<Rectangle Fill="#f00" Width="50" Height="50" CornerRadius="5" />
	</App>

This will render a red `Rectangle` with rounded corners over a black background. Note that these units are @(Points), not pixels, and the `Rectangle` will appear to be roughly the same size on most devices, regardless of pixel density and screen size.

### $(Circle)

It is equally simple to draw `Circle`s:

	<App Background="#000">
		<Circle Fill="#f00" Width="50" Height="50">
			<Stroke Width="5" Brush="#ff0" />				
		</Circle>
	</App>

In this example, we've taken it a bit further, and we're adding a yellow stroke.

### $(Fill:Fills)

We've seen that shapes accept simple `Fill` properties:

	<Rectangle Fill="#f00" />

It is possible to use other kinds of brushes to fill shapes. For example:

	<App>
		<StackPanel>
			<Circle Width="150" Height="150">
				<ImageFill File="Pictures/Picture1.jpg" />
			</Circle>
			<Rectangle Height="150">
				<LinearGradient StartPoint="0,0" EndPoint="1,0.75">
					<GradientStop Offset="0" Color="#FC3C47" />
					<GradientStop Offset="1" Color="#B73070" />
				</LinearGradient>
			</Rectangle>
		</StackPanel>
	</App>

Here we create a `Circle` that has been filled with an `ImageFill`-brush, great for creating your typical profile picture in a social app. We then add under it a `Rectangle` that has a nice and subtle `LinearGradient` from fuchsia(?) to purple(?).

### $(Stroke:Strokes)

TODO: StrokeAlignment
TODO: Stroke has Brush property (Brush="#f00")
Todo: StrokeOffset

`Stroke`s accept a brush the same way a `Fill` does:

	<App>
		<StackPanel>
			<Circle Width="150" Height="150">
				<Stroke Width="10">
					<ImageFill File="Pictures/Picture1.jpg" />
				</Stroke>
			</Circle>
			<Rectangle Height="150">
				<Stroke Width="15">
					<LinearGradient StartPoint="0,0" EndPoint="1,0.75">
						<GradientStop Offset="0" Color="#FC3C47" />
						<GradientStop Offset="1" Color="#B73070" />
					</LinearGradient>
				</Stroke>
			</Rectangle>
		</StackPanel>
	</App>

It can obviously just be set to be a `SolidColor`-brush:

	<App Background="#000">
		<Rectangle Fill="#f00" Width="50" Height="50">
			<Stroke Width="5" Brush="#ff0" />				
		</Rectangle>
	</App>

### Brushes

Fuse comes with different brush types that can be used as @(Stroke) and @(Fill) in @(Shapes).

#### $(SolidColor)

If you want to make a simple continuous color, you can use a `SolidColor`:

	<SolidColor Color="#00f" />
	
This will create a brush that can be assigned to any place that accepts a brush:

	<Rectangle>
		<SolidColor Color="#00f" />
	</Rectangle>
	
Note that this is equivalent of writing:

	<Rectangle Fill="#00f" />

#### $(ImageFill)

You can fill a @(Shape) with an image using `ImageFill`:

	<Circle Width="160" Height="160">
		<ImageFill File="Portrait.png" />
	</Circle>

The same way @(Image) allows you to, `ImageFill` lets you set @(StretchMode).

#### $(LinearGradient)

You can describe a `LinearGradient`-brush using `LinearGradient` and `GradientStop`. For example, to create a grey ramp that is white in the top and black in the bottom:

	<LinearGradient StartPoint="0,0" EndPoint="0,1">
		<GradientStop Offset="0" Color="#fff" />
		<GradientStop Offset="1" Color="#000" />
	</LinearGradient>

## $(Button)

TODO: Remove DebugAction and or rename to <Debug Message=

It is easy to make an app that has a `Button`:

	<App Theme="Basic">
		<Button Text="Click me!" ux:Name="button1">
			<Clicked>
				<Set button1.Text="Clicked!" />
			</Clicked>
		</Button>
	</App>

This small example will create a `Button` that covers the whole screen. When you click it, its label will change from "Click me!" to "Clicked!". Because we're working with a control, we add `Theme="Basic"`. We have previously not needed to rely on a `Theme` because we haven't really been working with anything that has a theme that can be applied.

In Fuse, pretty much anything can easily be made clickable (and tappable, etc):

	<App>
		<Rectangle Fill="#309">
			<Clicked>
				<DebugAction Message="Rectangle got clicked" />
			</Clicked>
		</Rectangle>
	</App>

Depending on where you started the preview process from, you'll see the `Message` output when you click the `Rectangle`.

Why, then is there a need to a separate `Button` concept?

Because when you switch the `Theme` to `Native`, Fuse will render the `Button` as a native iOS or Android `Button`, depending on which platform the app is run on. Also, creating a concept called "Button" makes it easier to make meaningful themes.

### $(Event triggers)

The `Button` can also accept `Clicked` as an _`event` trigger_:

	<App Theme="Basic">
		<JavaScript>
			module.exports = {
				buttonClick: function (args) { debug_log ("Button was clicked"); }
			}
		</JavaScript>
		<Button Text="Click me!" Clicked="{buttonClick}" />
	</App>

This is handy when you want to drive business logic from events.

## $(Switch)

To accept on/off-style input, Fuse has a `Switch`-control:

	<Switch />
	
To make it do anything, you can use the `WhileTrue`-trigger:

	<App Theme="Basic">
		<StackPanel>
			<Switch>
				<WhileTrue>
					<Change rectangle.Width="160" Duration="0.5" Easing="CircularInOut" />
				</WhileTrue>
			</Switch>
			<Rectangle ux:Name="rectangle" Width="70" Height="70" Fill="#909" />
		</StackPanel>
	</App>

To make it act on the opposite state, you can use `WhileFalse`, or `WhileTrue Invert="true"`.

If you want the `Switch` to start out being set:

	<Switch Value="true" />

The events emitted by the `Switch` can also be handled from JavaScript:

	<App Theme="Basic">		
		<JavaScript>
			module.exports = {
				switchChanged: function (args) {
					debug_log ("Switch value is: " + args.value);
				}
			}
		</JavaScript>
		<StackPanel>
			<Switch Value="true" ValueChanged="{switchChanged}" />
		</StackPanel>
	</App>

> ### Databinding switch

TODO: OBSERVE! This example crashes with invalid program when clicking on the `Switch`; using the buttons works as expected.

You can also databind the switch:

	<App Theme="Basic">		
		<JavaScript>
			var Observable = require("FuseJS/Observable");

			var switchEnabled = Observable(false);

			module.exports = {
				switchEnabled: switchEnabled,
				enableSwitch: function () { switchEnabled.value = true; },
				disableSwitch: function () { switchEnabled.value = false; },
				switchChanged: function (args) {
					debug_log ("Switch value is: " + args.value);
				}
			}
		</JavaScript>
		<StackPanel>
			<Switch Value="{switchEnabled}" ValueChanged="{switchChanged}" />
			<Grid ColumnCount="2">
				<Button Text="Disable" Clicked="{disableSwitch}" />
				<Button Text="Enable" Clicked="{enableSwitch}" />
			</Grid>
		</StackPanel>
	</App>

## $(Slider)
- ProgressAnimation
- Events for when changing value
- WhileInRange?
> ### Databinding slider

## $(TextInput)
- Value (2way databinding)
- Events for when changing value
- Password mode
- Numeric mode
- WhileKeyboardVisible
- WhileFocused
- WhileEmpty
- Multiline
- Word wrap
- link to styling?
- text edit


## $(PageControl)
- Forward link to navigation chapter
- Pagecontrol wraps navigation with swipenavigate

### $(Page)

### $(PageIndicator)

## $(ScrollView) (link elsewhere?)
- ScrollDirection
- ScrollingAnimation
- WhileScrollable


## $(MapView) (link elsewhere?)

## $(WebView) (link elsewhere?)

## $(GraphicsView) (link elsewhere?)

## $(Picker) (link elsewhere?)

## $(DatePicker) (link elsewhere?)

> ## Hit test
- HitTestMode
- How hit testing works
- Common pitfalls, opacity="0" == HitTestMode="None"

> ## Clip to bounds

> ## Opacity

> ## Layers

> ## Effects
- Only in graphics mode


> ## About Controls

TODO: Generally about controls and how they are just semantic containers

How themes interact with controls.

How controls are panels (usually)
