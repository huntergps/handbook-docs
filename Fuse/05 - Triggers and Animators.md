# $(Trigger)s and $(Animator)s

Triggers gives a declarative way of creating animations with Fuse. At its most basic, triggers represents events that are triggered in response to user and/or program input. @(Trigger:Triggers) can contain @(Animators) which are used to animate elements.

$(Trigger)s are @(behavior)s that live on a $(node) or UI $(Element), listen to events and perform animations and $(actions) in response.

For example, here is a @(Panel) with a @(WhilePressed) trigger causing the panel to rotate 90 degrees with a bouncy animation.
```
<Panel>
	<WhilePressed>
		<Rotate Degrees="90" Duration="0.5" Easing="BounceInOut"/>
	</WhilePressed>
</Panel>
```

TODO:
* Explain how triggers are a timeline, plays forwards/backwards, applies/unapplies
* Duration, easing and easing back - how the trigger takes the total length of all it's @(animators)

As well as animating properties, one can also use triggers to add and remove entire elements.

### $(Rest state) and deviation

The default layout and configuration of UX markup elements is called the rest state. Triggers define deviations from this rest state.
Each trigger knows how to "un-apply" its own animation to return the rest state, even if interrupted mid-animation. This is great, because it means
animation is completely separated from the logical state of your program, greatly reducing the complexity of dealing with combined animation on
multiple devices, screen sizes, with real data and real user input.


## Animators

Animators are used to specify which and how @(Element:elements) are to be animated when a @(Trigger:trigger) is triggered.

Animators have five pairs of properties which are important for controlling the exact result of an animation.

### $(Target)/Value
The `Target` property is used to identify the property which we intend to animate.
The `Value` property is the value of the result of an animation.

Because the task of setting a target and value, UX has a special syntax for this:
```
<Change target.Property="Value"/>
```

### $(Duration)/$(DurationBack)
Animations can have different behavior when animating forwards and backwards. When a trigger is activated, the animation is said to play forwards. When the trigger is deactivated, the animation is played backwards. Duration is used to set the duration for the animation.
One can set a different duration for the backwards animation by using the `DurationBack` property.

### $(Delay)/$(DelayBack)
Setting the `Delay` property results in the actual animation being delayed by that amount of seconds. `DelayBack` is used to set a different delay on the backwards animation. The total duation of the animation becomes the delay + the duration. The following @(Change:change) animator has a total duration of 7 seconds. It waits 5 seconds after being activated and then animates its target over 2 seconds.
```
<Change Delay="5" Duration="2" someElement.Height="100"/>
```

### $(Easing)/$(EasingBack)

Fuse comes with a standard set of predefined easing curves. Easing curves are used to control how an animation progresses over time. The default easing is set to `Linear`. With linear easing, the animation progresses at the same speed over its entire duration. This usually appears quite unnatural and fake. To gain a more natural feel, we can change the easing to QuadraticInOut, like so:
```
<Change Easing="QuadraticInOut" Duration="2" someElement.Property="SomeValue"/>
```
This animator will progress slowly in the beginning, faster in the middle, and then slow again in the end.

The following easing curves are defined:
- $(Linear)
- $(Quadratic)
- $(Cubic)
- $(Quartic)
- $(Quintic)
- $(Sinusoidal)
- $(Exponential)
- $(Circular)
- $(Elastic)
- $(Back)
- $(Bounce)

They each have three different versions: [easing]In, [easing]Out and [easing]InOut.
Assign easing to an animator like so:
```
<Move Easing="CubicIn" X="5" Y="10" Duration="0.5"/>
<Scale Easing="BounceOut" Factor="1.5" Duration="0.2"/>
<Rotate Easing="BounceOut" Degrees="10" Duration="0.4"/>
```

* Note that the easing type has to be combined with either In, Out or InOut. In means that the easing function is observed only in the beginning of the animation. Out is observed only at the end. InOut gives an effect on both the beginning and end of the animation.

- MixOp
TODO: Write about mixop

### $(Change)

Temporarily changes the value of a property while its containing trigger is active. To permanently change a value, use the @(Set) animator.
The `Change` animator can be used to animate almost any property.

```
<Panel ux:Name="somePanel">
	<SolidColor ux:Name="someColor" Color="Red"/>
</Panel>
<Change somePanel.Opacity="0"/>
<Change someColor.Color="Blue" Duration="0.3"/>
<Change somePanel.Visibility="Collapsed"/>
```

One can also animate such properties as `Width`, `Height` and `Margin`, but because these properties might affect the entire layout of your UI, this can end up being quite costly in terms of performance. There are usually more effective ways to do animations that interact with layout. Check out @(LayoutAnimation) and @(MultiLayoutPanel) for some inspiration.

### $(Move)

The `Move` animator is used to move an element. `Move` does not affect layout, so the element will just get an offset from its actual location. By default, `<Move X="10"/>` will move the element by 10 points.
Some times one wants an element to move relative to its own size or some other elements size. To control this, we can use the @(RelativeTo) property.

@(RelativeTo) can be set to the following values:
- `Local`(default): Moves the set amount of points in the X and/or Y direction.
- `Size`: Moves the set amount times the size of the element. So X="1" moves the element by its entire width in the X direction.
- `ParentSize`: Same as `Size` but uses the elements parents size instead.
- `LayoutChange`: Used in response to a @(LayoutAnimation) to move the element by the amount of a layout change.
- `Keyboard`: Moves the element relative to the size of the @(Keyboard).

`<Move X="0.5" RelativeTo="Size"/>` will move the element by half of its own width in the x-direction.
Move corresponds to adding a @(Translation) on the element and using @(Change) to animate its X and Y values. The folling two examples give the same result.
```
<Panel>
	<WhilePressed>
		<Move X="100" Duration="0.2"/>
	</WhilePressed>
</Panel>
```
```
<Panel>
	<Translation ux:Name="someTranslation"/>
	<WhilePressed>
		<Change someTranslation.X="100" Duration="0.2"/>
	</WhilePressed>
</Panel>
```

### $(Scale)

`Scale` works in the same way as @(Move) except that it scales the element. Note that scale doesn't actually change the elements size. This means that the rest of the UI layout wont be affected and the animation is guaranteed to be fast.

```
<Scale Factor="2" Duration="0.4"/>
```

### $(Rotate)
`Rotate` rotates an Element and is equal to adding a @(Rotation) and animating it with a @(Change).
```
<Rotate Degrees="90" Duration="0.5"/>
```

> ### $(Cycle)
TODO: Cycle

> ### $(Spin)
TODO: Spin

> ## Transforms
All @(Element:elements) can have transforms applied to them in order to move, scale or rotate.
It is worth mentioning that the order of these transforms affects the order of when they are applied to the element, and therefore can leed to different results.

```
<Panel Width="100" Height="50">
	<Translation X="100"/>
	<Rotation Degrees="45"/>
</Panel>
```
```
<Panel Width="100" Height="50">
	<Rotation Degrees="45"/>
	<Translation X="100"/>
</Panel>
```

The two examples have quite different results. In the first case, the panel is first moved 100 points to the right and then rotated 45 degrees. In the other case, the panel is first rotated 45 degrees. The positive X-direction is now 45 degrees downwards, and so our panel ends up being moved towards the bottom right.

There are three types of transforms that can be put on elements:
### $(Translation)
`Translation` moves the element in the X and Y direction. The follwing example shows a @(Rectangle) which is moved 100 points in the X-reiction and 50 points in the Y-direction.
```
<Rectangle Width="50" Height="50">
	<Translation X="100" Y="50"/>
</Rectangle>
```

TODO: Document X, Y, Z

### $(Scaling)
`Scaling` enlarges or shrinks the element by the factor specified. The following example will make the @(Rectangle) twice as big as the original size:
```
<Rectangle Width="100" Height="100">
	<Scaling Factor="2"/>
</Rectangle>
```

TODO: Document Vector

### $(Rotation)
`Rotation` rotates the element by the amount of degrees specified. Here is an example of a rectangle which is rotated by 90 degrees.
```
<Rectangle Width="100" Height="50">
	<Rotation Degrees="90"/>
</Rectangle>
```

Document X, Y, Z

### $(Sheer)
TODO: Document sheer


## $(Attractor)

The `Attractor` is used to give a more natural movement to animations. It acts as an intermediary between an animator and its target. An `Attractor` will continuously animate its target towards its `Value` using a simple form of physics simulation. We can combine this behavior with animation by animating the attractors `Value` property.
```
<Panel ux:Name="somePanel">
	<Translation ux:Name="someTranslation"/>
	<Attractor ux:Name="someAttractor" Target="someTranslation.X"/>
	<WhilePressed>
		<Change someAttractor.Value="100"/>
	</WhilePressed>

</Panel>
```

## $(Actions)

Triggers can contain actions too, which are one-off events that fire at a particular point in the trigger's timeline.

Note that actions, contrary to @(animators) are not reversible. This means it is not neccessarily possible to return to the @(rest state) if the trigger is reversed.

(make sure examples include things like Delay)

### $(Set)

Permanently changes the value of a property. If you want to just change it temporarily, use @(Change)

TODO: (examples++)

### $(Callback)

`Callback` is used to call a JavaScript function (see @(DataBinding)) when a trigger is activated.

```
<JavaScript>
	var someJSFunction(){
		Console.Log("some function called");
	}
	module.exports = { someJSFunction: someJSFunction };
</JavaScript>
<Rectangle Width="100" Height="50" Alignment="Center" Fill="Red">
	<WhilePressed>
		<Callback Handle="{someJSFunction}"/>
	</WhilePressed>
</Rectangle>
```

> ### $(Toggle)
TODO: Not sure how toggle works exactly

> ### $(BringIntoView)
TODO: Ask edaqua

> ### $(BringToFront)
TODO: Do we need to discuss Z-ordering?


## $(State groups)

State groups allow you to define completely custom states, with custom events.

### $(State)
A `State` consists of a set of @(Animator:animators) inside a `State` object. It acts as a normal @(Trigger), but is activated by its containing @(StateGroup).
```
<State>
	<Rotate Degrees="200" Duration="0.4"/>
	<Move X="10" Duration="0.4"/>
</State>
```

### $(StateGroup)
A `StateGroup` is used to group a set of @(State:states) together and switch between them. `StateGroup` has an `Active` property, which is used to assign which @(State) is currently active in that group. One can also specify the @(TransitionType), which can be wither `Exclusive` or `Parallel`. `Exclusive` means that each state will have to be fully deactivated before the next state becomes active. `Parallel` means that as one state deactivates, the next one will active and whatever properties they animate will be interpolated between them.

Here is an exampel of how to use a `StateGroup` to switch the color of a @(Rectangle) between three states:
```
<StackPanel>
	<Panel Width="100" Height="100">
		<SolidColor ux:Name="someColor"/>
	</Panel>
	<StateGroup ux:Name="stateGroup">
		<State ux:Name="redState">
			<Change someColor.Color="#f00" Duration="0.2"/>
		</State>
		<State ux:Name="blueState">
			<Change someColor.Color="#00f" Duration="0.2"/>
		</State>
		<State ux:Name="greenState">
			<Change someColor.Color="#0f0" Duration="0.2"/>
		</State>
	</StateGroup>
	<Grid ColumnCount="3">
		<Button Text="Red">
			<Clicked>
				<Set stateGroup.Active="redState"/>
			</Clicked>
		</Button>
		<Button Text="Blue">
			<Clicked>
				<Set stateGroup.Active="blueState"/>
			</Clicked>
		</Button>
		<Button Text="Green">
			<Clicked>
				<Set stateGroup.Active="greenState"/>
			</Clicked>
		</Button>
	</Grid>
</StackPanel>
```

## Data triggers

Reacts to data changes, either from data binding or from the control context.

### $(WhileTrue)
`WhileTrue` is active while its @(Value) property is `True` and inactive while it's false.

### $(WhileFalse)
`WhileFalse` is active while its @(Value) property is `False` and inactive while it's true.

### WhileFailed
TODO: I dont know what it does

## $Gestures

Reacts to pointer inputs.

### $(WhilePressed)
`WhilePressed` is active while its containing element is being pressed and the pointer is inside its bounds.
```
<Panel Width="50" Height="50">
	<WhilePressed>
		<Scale Factor="2" Duration="0.2"/>
	</WhilePressed>
</Panel>
```

### $(Clicked)
`Clicked` is activated in response to a click @(Gestures:gesture). What constitutes a click-event can be platform specific, but usually means that the pointer was pressed and released within the bounds of the containing element.

* Note: `Clicked`-triggers can be placed on any @(Element), not just @(Button:buttons).

```
<Panel Width="50" Height="50">
	<WhilePressed>
		<Scale Factor="2" Duration="0.2"/>
	</WhilePressed>
</Panel>
```


### $(Tapped)
The `Tapped`-trigger is quite similar to the @(Clicked)-trigger. Where a click just means that the pointer has to be pressed and released on the element, a tap means that the pointer has to be released within a certain time after the pointer is pressed.

TODO: Can we configure this time? And what is the default?

### $(WhileHovering)
`WhileHovering` is active while the pointer is within the bounds if its containing @(Element).

* Note: `WhileHovering` only has value when the device supports a hovering pointer, like the mouse pointer on desktop machines. For most smart phones this trigger won't have much value.

## $(Control triggers)

### $(WhileEnabled)
The `WhileEnabled` trigger is active whenever its containing @(Element:elements) @(IsEnabled) property is set to `True`.

```
<Panel  Width="50" Height="50" Background="Red" >
	<WhileEnabled>
		<Rotate Degrees="45" Duration="0.5"/>
	</WhileEnabled>
</Panel>
```

### $(WhileDisabled)
The `WhileDisabled` trigger is active whenever its containing @(Element:elements) @(IsEnabled) property is set to `False`.


## Platform triggers

TODO: Do android and ios work?

### Android

### iOS

### $(WhileKeyboardVisible)
`WhileKeyboardVisible` is active whenever the on-screen keyboard is visible.

TODO: Example

### WhileWindowAspect
TODO: Did we change the name of this?

## Special $(Animation)s

In Fuse terminology, *animations* are a certain category of triggers that animate in response to a higher level interpretation
of input, events and logical state changes.

Animation triggers are designed to solve quite specific animation problems that would otherwise be hard to pull off using the simpler triggers.
The flipside is that these triggers can be somewhat hard to understand and use, so make sure you read the docs and study examples before spending
too much time scratching your head.

### $(LayoutAnimation)

The `LayoutAnimation` is triggered in response to a layout change. A layout change happens whenever the element gets a new layout by the layout system. `LayoutAnimation` is commonly used together with @(MultiLayoutPanel) for some pretty interresting animations. See @(LayoutAnimation) for more in depth documentation.

TODO: consider linking to external article here

### $(AddingAnimation)

TODO: Link to examples

`AddingAnimation` is triggered whenever the element is added to the visual tree. Adding animation is by default a backwards animation, meaning it will animate from progress 1 back to 0.



### $(RemovingAnimation)

TODO: Link to examples

`RemoveAnimation` is similar to @(AddingAnimation) but is triggered whenever the element is removed from its parent. `RemoveAnimation` progresses normally from 0 to 1 over the specified @(Duration).

In the following example, a rectangle will move in from the right side by the width of its parent container over one second when it is added to the visual tree by the @(Switch). It will move out to the left by the same length when it is removed.
```
<App Theme="Basic" ClearColor="#eeeeeeff">
	<StackPanel Background="#ddd" Margin="10">
		<Switch>
			<WhileTrue>
				<Change whileTrue.Value="true"/>
			</WhileTrue>
		</Switch>
		<WhileTrue ux:Name="whileTrue">
			<Panel Height="50" Width="50" Background="Red">
				<AddingAnimation>
					<Move RelativeTo="ParentSize" X="1" Duration="1" Easing="BounceIn"/>
				</AddingAnimation>
				<RemovingAnimation>
					<Move RelativeTo="ParentSize" X="-1" Duration="0.5"/>
				</RemovingAnimation>
			</Panel>
		</WhileTrue>
	</StackPanel>
</App>
```

### $(EnteringAnimation)

TODO: Link to navigation
`EnteringAnimation` is used to control how pages behave when they are navigated to and from. Check out the @(Navigation) chapter for more in depth documentation.

### $(ExitingAnimation)

TODO: Same as above

### $(ActivatingAnimation)

`ActivatingAnimation` allows for animating based on which @(Page) is active. `ActivatingAnimation` will progress from 0 to 1 as a @(Page) is being navigated to. If @(SwipeNavigate) is used, one can observe that `ActivatingAnimation` progressed from 0 as soon as the @(Page) is entering, stays at 1 as long as the @(Page) is active, and then progresses towards 0 again as the @(Page) is exiting.

### $(ScrollingAnimation)

`ScrollingAnimation` lets us create animations in response to a @(ScrollView) being scrolled. By using the `From` and `To` properties one can define an interval on the @(ScrollView) where the trigger gets activated.

In the following example, we use `ScrollingAnimation` to @(Scale) the @(ScrollView) as it is being scrolled.
```
<ScrollView>
	<StackPanel Background="#ddd" Margin="10">
		<Panel Height="200" Background="Red"/>
		<Panel Height="200" Background="Blue"/>
		<Panel Height="200" Background="Green"/>
		<Panel Height="200" Background="Purple"/>
		<Panel Height="200" Background="Teal"/>
	</StackPanel>
	<ScrollingAnimation From="0" To="400">
		<Scale Factor="0.3" />
	</ScrollingAnimation>
</ScrollView>
```

### $(WhileScrollable)

`WhileScrollable` is used to animate based on whether a @(ScrollView) can be scrolled or not. Use the `ScrollDirections` property to filter the activation based which directions we care about.

`ScrollDirections` can take on any one of the following values:
- All
- Both
- Down
- Horizontal
- Left
- Right
- Up
- Vertical

In the following example, our background changes color when we reach the bottom of our @(ScrollView):
```
<ScrollViewer>
	<SolidColor ux:Name="color" Color="#000"/>
	<StackPanel Background="#ddd" Margin="10">
		<Panel Height="200" Background="Red"/>
		<Panel Height="200" Background="Blue"/>
		<Panel Height="200" Background="Green"/>
		<Panel Height="200" Background="Purple"/>
		<Panel Height="200" Background="Teal"/>
	</StackPanel>
	<WhileScrollable ScrollDirections="Down">
		<Change color.Color="#ddd" Duration="0.4"/>
	</WhileScrollable>
</ScrollViewer>
```

### $(ProgressAnimation)

TODO: What does it do?

## $(Timeline)

TODO: Might need edaqua for this one.

Timelines are used to create custom animations that can be played, paused, seeked etc. in response to events.

> ## Advanced trigger usage

TODO: Show examples of
* Document Bypass
* Triggers within triggers
* Elements/nodes within triggers
* Styles within triggers (? not sure if this is currently possible)
