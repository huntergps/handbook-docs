# Data Binding

TODO: Explain, overview, example of basic use, data paths

## JavaScript module as data source

This is the main way... TODO: explain and show

> ## Other data sources

Explain how the `Fuse.Reactive` provides abstractions over data sources in Uno code, which
allows data for UX to come from anywhere and any language.

Roadmap:
* Observables for Swift
* Observables for Java

## Data Context

More detailed on how data contexts work, how bindings and `Each` change the data context

## $(Each)

TODO: explain, show examples, nested each's 

Explain that the inner tree of an `Each` is a "factory" that will be instantiated per item.

> ### Ensuring order

`Each` does not neccessarily insert the children at the exact location in the tree where the `Each` node
resides. To ensure child order, wrap the `Each` in some sort of @(Panel):

	<StackPanel>
		<Text>I go first!</Text>
		<StackPanel>
			<Each Item="{strings}">
				<Text Value="{}" />
			</Each>
		</StackPanel>
		<Text>I go last!</Text>
	</StackPanel>

> We are considering adding more tricks to allow more flexibility in this case, but for now this
  is the reccommended approach.