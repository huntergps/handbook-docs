# UI Components

UI components are the basic building blocks you construct an app from. Let's quickly look at some of them here.

> ## Videos on this topic

## Text

Displaying text is something you do in most apps. The simplest possible example to show some text is:

	<App>
		<Text>Hello, world!</Text>
	</App>

If you want to add longer passages of text, such as a _Lorem Ipsum_, you discover that you in certain cases want to enable word wrapping. In Fuse, this is done with the `TextWrapping` property on the `Text` control:

	<App>
		<Text TextWrapping="Wrap">Lorem Ipsum(...)</Text>
	</App>
	
If you find that wrapping the text still makes it hard to show all the contents you want, you probably want to look at adding the contents to a @(ScrollView), or changing @(FontSize). `TextWrapping` can be set to `Wrap` and `NoWrap` (default).

### $(Fonts)

You can import fonts from ttf files containing TrueType fonts. Because a font is typically referred to throughout an application, it is best to simply create a _global @(Resources:resource)_ for it immediately.

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

Fuse is currently unable to use _system fonts_, but that is a feature under development.

On device, unicode and other multibyte character sets work fine. However, there are currently some issues rendering multibyte character sets using desktop preview. This is a known issue, and is being worked on. 

This means, however, that emojis work fine on devices.

### Text specific layout

For further control over how your text is rendered, you can set `TextAlignment`, `TextColor` and `FontSize`:

	<App Background="#fff">
		<Font File="Roboto-Medium.ttf" ux:Global="Medium" />
		<Font File="Roboto-Light.ttf" ux:Global="Light" />
		<StackPanel>
			<Text Font="Light" TextColor="#999">Roboto Light</Text>
			<Text TextAlignment="Center" Font="Medium">Roboto Medium</Text>
			<Text Font="Light" FontSize="24">Roboto Light</Text>
		</StackPanel>
	</App>

In this example, the first text element will be left aligned as this is the default, and have its color set to a medium light grey. The second text will be center aligned. The third will have a larger font. Valid values for `TextAlignment` are `Left` (default), `Right` and `Center`.

## Image

Working with images is easy:

	<App>
		<Image File="FuseLogo.png" />
	</App>

This code assumes the file `FuseLogo.png` lives in the same directory as the UX-file. If you would rather load the contents of the image from the Internet, simply:

	<App>
		<Image Url="http://path_to_image" />
	</App>

_Note!_ If you come from a background as a web developer you might be used to assigning a URL to a `src`-attribute. While `Image` has a `Source`-property, it is used to assign a `Resource` to an image. In this context, this `Resource` is a `HttpImageSource`, but that is created behind the covers for you automatically, so stick to the `Url`-property to load contents from the web.

> ### Image contents from resources

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
			<Select Data="{pictureResource}">
				<Image Source="{DataToResource key}" />
			</Select> 
			<Select Data="{url}">
				<Image Url="{}" />
			</Select>
		</StackPanel>
	</App>

This app will show three `Image`s stacked on top of each other. The topmost picture will be fetched as a normal file. Then we create a `FileImageSource` that we bind to a picture using `DataToResource`, which looks up the resource for `pictureResource.key` and binds it to the `Image`. We also get the URL for a picture on the web and bind it to the `Url`-property of an image. If this looks complicated, don't fret: We'll look more at @(DataBinding) and @(JavaScript) shortly. 

## Shapes

Rendering basic shapes comes in handy quite often.

### Rectangle

A `Rectangle` can be drawed by adding just that:

	<App Background="#000">
		<Rectangle Fill="#f00" />
	</App>
	
In this example, even if the `Background` of the `App` is set to `#000`, the whole area of the app will be displayed as red, as the `Rectangle` will take up as much space as it can and fill it with `#f00`.

If you want to have the `Rectangle` limit itself somewhat, you can:

	<App Background="#000">
		<Rectangle Fill="#f00" Width="50" Height="50" CornerRadius="5" />
	</App>

This will render a red `Rectangle` with rounded corners over a black background.

### Circle

It is equally simple to draw `Circle`s:

	<App Background="#000">
		<Circle Fill="#f00" Width="50" Height="50">
			<Stroke Width="5">
				<SolidColor Color="#ff0" />
			</Stroke>
		</Circle>
	</App>
	
In this example, we've taken it a bit further, and we're also adding a yellow stroke.

### Fills

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

### Strokes

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
			<Stroke Width="5">
				<SolidColor Color="#ff0" />
			</Stroke>
		</Rectangle>
	</App>

## $(Button)

It is easy to make an app that has a `Button`:

	<App Theme="Basic">
		<Button Text="Click me!" ux:Name="button1">
			<Clicked>
				<Set button1.Text="Clicked!" />
			</Clicked>
		</Button>
	</App>

This small example will create a `Button` that covers the whole screen. When you click it, its label will change from "Click me!" to "Clicked!". There is one difference from our previous examples at work here, namely: `Theme="Basic"`. We have previously not needed to rely on a `Theme` because we haven't really been working with anything that has a theme that can be applied.
 
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

The `Button` can also accept `Clicked` as an _`event` trigger_ as well as an explicit trigger, as shown above:

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
- WhileTrue
- WhileFalse
- Events for when changing value
> ### Databinding switch

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
