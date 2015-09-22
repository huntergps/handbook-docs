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
	
	<Rectangle Fill="#f00" Width="50" Height="50" CornerRadius="5" />

This will render a red `Rectangle` with rounded corners over a black background. Note that these units are @(Points), not pixels, and the `Rectangle` will appear to be roughly the same size on most devices, regardless of pixel density and screen size.

### $(Circle)

It is equally simple to draw `Circle`s:


	<Circle Fill="#f00" Width="50" Height="50">
		<Stroke Width="5" Brush="#ff0" />				
	</Circle>

In this example, we've taken it a bit further, and we're adding a yellow stroke.

### $(Fill:Fills)

We've seen that shapes accept simple `Fill` properties:

	<Rectangle Fill="#f00" />

It is possible to use other kinds of brushes to fill shapes. For example:


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

Here we create a `Circle` that has been filled with an `ImageFill`-brush, great for creating your typical profile picture in a social app. We then add under it a `Rectangle` that has a nice and subtle `LinearGradient` from fuchsia(?) to purple(?).

### $(Stroke:Strokes)

`Stroke`s accept a brush the same way a `Fill` does:
	
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

It can obviously just be set to be a `SolidColor`-brush:
	
	<Rectangle Fill="#f00" Width="50" Height="50">
		<Stroke Width="5" Brush="#ff0" />				
	</Rectangle>


#### $(StrokeAlignment)

The `Stroke` can be aligned:
	
	<Stroke StrokeAlignment="Center" />
	
Valid values are `Center`, `Inside`, `Outside`.

#### $(Stroke.Offset)

The `Stroke` of a @(Shape) can be `Offset`:

	<Stroke Width="10" Offset="10">
		<ImageFill File="Pictures/Picture1.jpg" />
	</Stroke>

A positive `Offset` will make the `Stroke` appear outside the `Shape` while a negative `Offset` will make it appear inside.



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

The `StartPoint` and `EndPoint` are both X and Y offsets within the @(Shape) the brush is used in, so you can specify a diagonal brush by using `StartPoint="0,0" EndPoint="1,1"`.

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
	
To make it do something, you can use the `WhileTrue`-trigger:

	<App Theme="Basic">
		<StackPanel>
			<Switch>
				<WhileTrue>
					<Change rectangle.Width="160" Duration="0.5" 
						Easing="CircularInOut" />
				</WhileTrue>
			</Switch>
			<Rectangle ux:Name="rectangle" Width="70" Height="70" Fill="#909" />
		</StackPanel>
	</App>

To make it act on the opposite state, you can use `WhileFalse`, or `WhileTrue Invert="true"`.

If you want the `Switch` to start out being activated:

	<Switch Value="true" />

The events emitted by the `Switch` can also be handled from JavaScript:

	<App Theme="Basic">		
		<JavaScript>
			module.exports = {
				switchChanged: function (args) {
					debug_log ("Switch value is: " + args.value);
				}
			};
		</JavaScript>
		<StackPanel>
			<Switch Value="true" ValueChanged="{switchChanged}" />
		</StackPanel>
	</App>

> ### Databinding switch

TODO: OBSERVE! This example crashes with invalid program when clicking on the `Switch`; using the buttons works as expected. This is true for local fuse preview and DotNetExe, works as expected on device

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
			};
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

To display a slider:

	<Slider />
	
If you want to influence something as the slider moves, you can use `ProgressAnimation`. Consider this code which allows you to `Rotate` a `Rectangle` from 0 to 90 degrees:

	<StackPanel>
		<Slider>
			<ProgressAnimation>
				<Rotate Target="rectangle" Degrees="90" />
			</ProgressAnimation>
		</Slider>
		<Rectangle ux:Name="rectangle" Width="100" Height="100" Fill="#808" />
	</StackPanel>
	
You can also listen to the `ValueChanged`-event:

	<App Theme="Basic">		
		<JavaScript>			
			module.exports = {
				sliderValueChanged: function (args) 
				{
					debug_log ("Value: " + args.value);
				}
			};
		</JavaScript>		
		<Slider ValueChanged="{sliderValueChanged}" />		
	</App>
	
When moving the slider from the far left to the far right, your console will output floating point numbers from 0 to 100, which are the default `Minimum` and `Maximum` values. These properties can also be set:

	<Slider Minimum="4" Maximum="42" />
	
- WhileInRange? TODO: I could not find WhileInRange. Has it been renamed?

> ### Databinding slider

It is also possible to databind the slider position:

	<App Theme="Basic">		
		<JavaScript>
			var Observable = require("FuseJS/Observable");

			var sliderValue = Observable(42);
			var sliderLabel = sliderValue.map(function (val) {
				return "Current position: " + val;
			});

			module.exports = {
				sliderValue: sliderValue,
				sliderLabel: sliderLabel
			};
		</JavaScript>
		<StackPanel>
			<Slider Value="{sliderValue}" />
			<Text TextAlignment="Center" Value="{sliderLabel}" />
		</StackPanel>
	</App>

## $(TextInput)

Fuse provides a `TextInput`-control to allow for user input of text:

		
	<JavaScript>
		var valueChanged = function (args) {
			debug_log (args.value);
		}

		module.exports = {			
			valueChanged: valueChanged
		};
	</JavaScript>

	<TextInput ValueChanged="{valueChanged}" />
	
You can also easily do two-way databinding:	

	<App Theme="Basic">		
		<JavaScript>
			var Observable = require("FuseJS/Observable");

			var name = Observable("");
		
			var greeting = name.map(function (name) {
				if (name == "") {
					return "Type your name above";
				} else {
					return "Hello there, " + name + "!";
				}
			});

			var clearName = function () {
				name.value = "";
			}

			module.exports = {
				name: name,
				greeting: greeting,				
				clearName: clearName
			};
		</JavaScript>

		<StackPanel>
			<StatusBarBackground />
			<DockPanel>
				<Text Dock="Left" Alignment="VerticalCenter">Name:</Text>
				<TextInput Value="{name}" Alignment="VerticalCenter" />
			</DockPanel>	
			<Text TextAlignment="Center" Value="{greeting}" />
			<Button Clicked="{clearName}" Text="Clear" />
		</StackPanel>
	</App>
	
If you want to accept a password, you might want to mask the user input:

	<TextInput IsPassword="true" />

If you want to accept numeric values mainly, you can set an `InputHint`:

	<TextInput InputHint="Number" />
	
Valid values for `InputHint` are `Default`, `Email`, `Number`, `Phone`, `Url`. These are called "hints" because they might not do anything, depending on which platform you are on. For example, when on the desktop, the keyboard will be the same no matter which hint is added to the `TextInput`.

`TextInput` also allows you to input contents over multiple lines instead of scrolling off to the right, which it does by default:

	<TextInput IsMultiline="true" />

When a `TextInput` gets focus, it will often summon the device's on-screen keyboard. Fuse provides a number of mechanisms to react to this event, one of which is `WhileKeyboardVisible`:

	<Text Value="Instructions of some kind">
		<WhileKeyboardVisible>
			<Move Y="-1" RelativeTo="Keyboard" />
		</WhileKeyboardVisible>
	</Text>
	<TextInput />
	
As you can see, `WhileKeyboardVisible` can be attached to an arbitrary element, and you can do pretty much anything you want as a response to the on-screen keyboard taking up space on the screen.

- WhileFocused TODO: I am not sure what exactly this is supposed to demonstrate
- WhileEmpty TODO: This doesn't exist, should it? It is good for implementing placeholder data
- link to styling? 
- text edit TODO: What is this?


## $(PageControl)

Fuse has a lot of ways to deal with @(Navigation:navigation). If you need to get started quickly, you can use `PageControl`:

	<PageControl />

### $(Page)

A `PageControl` is typically used with `Page`s:

	<PageControl>
		<Page>
			<Rectangle Fill="#808" />
		</Page>
		<Page>
			<Rectangle Fill="#990" />
		</Page>
	</PageControl>

This will create two swipable pages. You can exchange the `Rectangle`s in this example for any UX you want, of course.

### $(PageIndicator)

If you want to add a page indicator to the mix:

	<App Theme="Basic" Background="#000">		
		<DockPanel>
			<PageControl ux:Name="pageControl">
				<Page>
					<!-- Page 1 -->
				</Page>
				<Page>
					<!-- Page 2 -->
				</Page>
			</PageControl>
			<PageIndicator Dock="Bottom" Alignment="Center" Margin="5" Navigation="pageControl">
				<Circle ux:Generate="Factory" ux:Binding="DotFactory" Width="10" Height="10"  Margin="4" Padding="10">
					<SolidColor ux:Name="dot" Color="#444" />
					<ActivatingAnimation>
						<Change dot.Color="#fff" />
					</ActivatingAnimation>
				</Circle>
			</PageIndicator>
		</DockPanel>
	</App>

`PageControl` is really an abstraction on @(SwipeNavigate) and @(LinearNavigation), so if you want to make more custom navigation scenarios, look at @(Navigation).

TODO: Link to https://www.fusetools.com/developers/examples/pagecontrol

## $(ScrollView) (link elsewhere?)

Fuse has a `ScrollView` that can be used to navigate contents that are larger than the available size.

	<ScrollView>
		<Panel Width="2000" Height="2000" />
	</ScrollView>

To limit the behavior of a `ScrollView`, you can set the ScrollDirection:

	<ScrollView AllowedScrollDirections="Horizontal">
		<!-- Contents -->
	</ScrollView>

Valid settings for `AllowedScrollDirections` include `Horizontal`, `Both` and `Vertical` (default). TODO: Check that this is true. There are also a bunch of AllowedScrollDirections in the enum I believe are not used

> ### $(ScrollingAnimation)

It is possible to animate properties based on absolute `ScrollView` position. For example, let's remove a top ledge as a `ScrollView` scrolls down: 

	<App Theme="Basic" Background="#fff">		
		<Panel>
			<Panel Alignment="Top" Height="50" ux:Name="ledge">		
				<Text Alignment="Center" TextAlignment="Center" TextColor="#fff" Value="TopLedge" />
				<Rectangle  Fill="#000" />
			</Panel>
			<ScrollView>
				<ScrollingAnimation From="0" To="50">
					<Change ledge.Opacity="0" />
				</ScrollingAnimation>
				<StackPanel>
					<!-- Block out the top ledge in the scrollview -->
					<Panel Height="50" />
					<!-- ... Content ... -->
				</StackPanel>
			</ScrollView>
		</Panel>
	</App>

> ### $(WhileScrollable)


## $(MapView) (link elsewhere?)

## $(WebView) (link elsewhere?)

## $(GraphicsView) (link elsewhere?)

## $(Picker) (link elsewhere?)

## $(DatePicker) (link elsewhere?)

> ## Hit test

When interacting with an element, it is sometimes desirable to be able to differenciate which elements can be interacted with and how. This is typically referred to as "hit testing". In Fuse, how elements interact with user input can be set using $(HitTestMode).

Consider this code:

	<Grid ColumnCount="2">
		<Rectangle Width="100" Height="100" Fill="#808" >
			<Clicked>
				<DebugAction Message="Clicked left" />
			</Clicked>
		</Rectangle>
		<Rectangle Width="100" Height="100" Fill="#808" HitTestMode="None" >
			<Clicked>
				<DebugAction Message="Clicked right" />
			</Clicked>
		</Rectangle>
	</Grid>
	
It will layout two `Rectangle`s and add `Clicked`-triggers to both of them. However, only the left one will output anything when clicked, as the hit testing has been explicitly disabled on the right rectangle. This can be very helpful if you have visual elements obscuring elements below it, where you want the lower elements respond to input.

Valid values for `HitTestMode` are:

- `None` - This element will not do hit testing
- `LocalBounds` - This element will be hit tested based on its size
- `LocalVisual` - This element will be hit tested based on its appearance
- `LocalBoundsAndChildren` - Hit testing will include the bounds of the element and its ch`ildren
- `LocalVisualAndChildren` - Hit testing will include the appearance of the element and its children

Note that if you set the @(Opacity) of an element below or equal its `HitTestOpacityThreshold` (which defaults to being 0), hit testing will be disabled for that object. This means that you can click an element as you fade it out, but it will stop accepting clicks at a certain point.

> ## $(ClipToBounds)

Normally, when laying out an element inside the other, the inner element can freely live outside the parent element:

	<Panel Width="100" Height="100">
			<Image Margin="-100" File="Pictures/Picture1.jpg" 	
				StretchMode="UniformToFill" />
	</Panel>

This `Image` will appear to be 300pt wide and tall, as the `Panel` doesn't clip children to its bounds. TODO: This really should have a screenshot.

If you intent to have the `Image` clip to its parent size, simply add $(ClipToBounds) to the `Panel`:

		<Panel Width="100" Height="100" ClipToBounds="true">
			<Image Margin="-100" File="Pictures/Picture1.jpg" 
				StretchMode="UniformToFill" />
		</Panel>

Now, the `Image` will not overflow the bounds of the `Panel`.

> ## $(Opacity)

You can set the transparency of objects using the `Opacity`-property. 

	<Panel>
		<Opacity Value="0.5" />
	</Panel>
	
When the `Opacity` is set to 0.0, the element is fully transparent and will no longer respond to @(HitTest:HitTests). When the `Opacity` is set to 1.0, the element will be at its default state. 

> ## Layers

> ## $(Effects)

Fuse has the ability to render a set of visual effects that can be added to most controls. It is important to understand that in order for these to work, you need to be in graphics mode; native themes are limited in their ability to render these effects.

### $(DropShadow)

To add a `DropShadow` to an element:

	<Rectangle Width="50" Height="50" Fill="#808">
		<DropShadow />
	</Rectangle>

To make a soft `DropShadow` from the top down:

	<Rectangle Width="50" Height="50" Fill="#808">
		<DropShadow Angle="90" Distance="12" Size="20" Spread="0.1"  />
	</Rectangle>

It can also be used to create artistic effects like outer glow:

	<Panel Background="#000">
		<Circle Width="50" Height="50" Fill="#808">
			<DropShadow Distance="0" Size="50" Spread="0.2" Color="#ff06" />
		</Circle>
	</Panel>

`DropShadow` has these properties:

- `Angle` - Which direction does the light come from: 
	- 0 - right
	- 90 - top
	- 180 - left
	- 270 - bottom
- `Distance` - The distance in points from the source of the shadow
- `Size` - The size of the dropshadow
- `Spread` - How the shadow drops off. The closer to 0, the more linear. Keep this value low (experiment below 1.0), or you will get artifacting
- `Color` - Which color the dropshadow should have. Note that this also supports alpha channel, so you can make the shadow more or less transparent 

### $(Blur)

To blur an element:

	<Text Value="Hello there!">
		<Blur Radius="0.9" />
	</Text>
	
Note that while the `Radius` of the `Blur` can be animated like most other properties, this is potentially an expensive operation, and should be tested on devices to make sure it behaves properly.

### $(Desaturate)

To `Desaturate` an element, fully or partially:

	<Image File="Pictures/Picture1.jpg">
		<Desaturate Amount="0.4" />
	</Image>
	
An amount of 1.0 will fully `Desaturate` the element.

### $(Halftone)

Add a classic halftone effect:

	<Image File="Pictures/Picture1.jpg">
		<Halftone />
	</Image>

`Halftone` accepts:

- `Spacing` - The distance between the dots (`float`, default 0.5)
- `Smoothness` - How hard or soft the dot edges are (`float`, default 2)
- `Intensity` - How much of the effect is applied (`float`, default 1)
- `DotTint` - Tint amount of the dots (`float`, default 0.5)
- `PaperTint` - Tint amount of the paper (`float`, default 0.2)

### $(Mask)

Fuse allows you to mask an element with an image or `ImageSource`.

	<Rectangle Width="50" Height="50">
		<Mask File="Masks/Flower.png" />
	</Rectangle>

The `Mask` effects accepts the following properties:

- `Mode` - How to interpret the mask source
	- `RGBA` (default) - Use the alpha channel of the source image as the mask and multiply the RGB values from the mask with the RGB values from the element to be masked
	- `Alpha` - Use the alpha channel of the source image without touching the RGB values from the masked element
	- `Greyscale` - Use the color component of the greyscale mask as a multiplication factor with the original alpha value
	
If you use a white image with alpha channel, `RGBA` and `Alpha` will have the same result.

The mask will always stretch itself to match the size of the element to be masked.

> ## About Controls

TODO: Generally about controls and how they are just semantic containers

How themes interact with controls.

How controls are panels (usually)
