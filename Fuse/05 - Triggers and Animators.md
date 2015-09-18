# Triggers and Animators

Triggers is the 

$(Trigger)s are @(behavior)s that live on a $(node) or UI $(Element), listen to events and perform animations and $(actions) in response.

For example, here is a @(Panel) with a @(WhilePressed) trigger doing some animation in resonse:

	/// TODO

TODO:
* Explain how triggers are a timeline, plays forwards/backwards, applies/unapplies
* Duration, easing and easing back - how the trigger takes the total length of all it's @(animators)


> ### $(Rest state) and deviation

The default layout and configuration of UX markup elements is called the rest state. Triggers define deviations from this rest state.
Each trigger knows how "un-apply" its own animation to return the rest state, even if interrupted mid-animation. This is great, because it means 
animation is completely separated from the logical state of your program, greatly reducing the complexity of dealing with combined animation on 
multiple devices, screen sizes, with real data and real user input.



## Animators

TODO: explain overall how animators work

> ### $(Change

Temporarily changes the value of a property. If you want to permanently change a value, use @(Set)

(examples++)

> ### $(Move)

> ### $(Scale)

> ### $(Rotate)

> ### $(Cycle)

> ### $(Sping)

## Trigger $(Actions)

Triggers can contain actions too, which are one-off events that fire at a particular point in the trigger's timeline.

Note that actions, contrary to @(animators) are not reversible. This means it is not neccessarily possible to return to
the @(rest state) if the trigger is reversed.

(make sure examples include things like Delay)

> ### $(Set)

Permanently changes the value of a property. If you want to just change it temporarily, use @(Change)

(examples++)

> ### $(Callback)

> ### $(BringIntoView)

> ### $(BringToFront)




## $(State groups)

State groups allow you to define completely custom states, with custom event

## Data triggers

Reacts to data changes, either from data binding or from the control context.

> ### WhileTrue

> ### WhileFalse

> ### WhileFailed



## Gestures

Reacts to pointer inputs.

> ### WhilePressed

> ### WhilePressed

> ### Clicked


## Control triggers

> ### Disabled

> ### Enabled

> ### WhileScrollable

> #

## Platform triggers

> ### Android

> ### iOS

> ### KeyboardVisible

> ### WhileWindowAspect

## Special $(Animation)s

In Fuse terminology, *animations* are a certain category of triggers that animate in response to a higher level interpretation
of input, events and logical state changes. 

Animation triggers are designed to solve quite specific animation problems that would otherwise be hard to pull off using the simpler triggers.
The flipside is that these triggers can be somewhat hard to understand and use, so make sure you read the docs and study examples before spending
too much time scratching your head.

> ### $(LayoutAnimation)

(consider linking to external article here)

> ### $(AddingAnimation)

(consider linking to examples)

> ### $(RemovingAnimation)

> ### $(EnteringAnimation)

> ### $(ExitingAnimation)

> ### $(ActivatingAnimation)

> ### $(ScrollingAnimation)

> ### $(ProgressAnimation)

## $(Timeline)

Timelines are used to create custom animations that can be played, paused, seeked etc. in response to events.


> ## Advanced trigger usage

TODO: Show examples of
* Triggers within triggers
* Elements/nodes within triggers
* Styles within triggers (? not sure if this is currently possible)
