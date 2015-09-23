# Data Binding

Fuse provides first class support for creating data driven apps with UX tags through direct binding, iteration and branching. UX can also do referencing deep inside complex data structures, so you do not have to do tedious data massaging in code. 

## JavaScript module as data source

The simplest way to create a data source is through JavaScript:

	<App>		
		<JavaScript>
			var data = ["1", "2", "3"];

			module.exports = {
				data: data
			};
		</JavaScript>
		<StackPanel>
			<Each Items="{data}">
				<Text Value="{}" />
			</Each>
		</StackPanel>
	</App>

This will predictably list out the text strings 1, 2 and 3. When binding the `Text Value` `{}` means _this data context_. Typically, you will bind to more complex data sources, so each element will have something interesting to bind to:

	<App Theme="Basic">		
		<JavaScript>
			var Observable = require("FuseJS/Observable");

			var data = Observable(
				{name: "Hubert Cumberdale", age: 12}, 
				{name: "Marjory Stewart-Baxter", age: 43}, 
				{name: "Jeremy Fisher", age: 25});

			module.exports = {
				data: data
			};
		</JavaScript>
		<StackPanel>
			<Each Items="{data}">
				<DockPanel>
					<Text Dock="Right" Value="{age}" />
					<Text Value="{name}" />
				</DockPanel>
			</Each>
		</StackPanel>
	</App>

In this case, we have also made the data source @(Observable). This means that it supports reflecting changes to the data source at runtime. In this case, the collection itself is `Observable`, but the items are not. You can bind to the children, but if they were to change, these changes would not be reflected in the UI. We'll look at how to fix this shortly.

> ## Other data sources

Explain how the `Fuse.Reactive` provides abstractions over data sources in Uno code, which allows data for UX to come from anywhere and any language.  AUTH

Roadmap:
* Observables for Swift
* Observables for Java
* Expressions in paths

## Data Context

More detailed on how data contexts work, how bindings and `Each` change the data context AUTH(ority needed)

## $(Each)

The UX construct `Each` makes it easy to iterate a data source:

	<Each Items="{items}">
		<Rectangle Width="{width}" Height="{height}" Fill="#808" />
	</Each>

It is also possible to nest `Each` constructs:
	
	<JavaScript>
		var Observable = require("FuseJS/Observable");

		module.exports = {
			items: [ 
				{ 
					inner: [ 
						{ child: "John" },
						{ child: "Paul" } 
					]
				}, 
				{ 
					inner: [ 
						{ child: "Ringo" }, 
						{ child: "George" }
					]
				}
			]			
		};
	</JavaScript>
	<ScrollViewer>
		<StackPanel>
			<Each Items="{items}">
				<StackPanel Orientation="Horizontal">
					<Each Items="{inner}">
						<Text Value="{child}" Margin="10" />
					</Each>
				</StackPanel>
			</Each>
		</StackPanel>
	</ScrollViewer>

You can also just use `Each` as a simple repeater:

	<Grid ColumnCount="3">
		<Each Count="9">
			<Rectangle Margin="10" Fill="#610" />
		</Each>
	</Grid>

When using `Each`, it can be helpful to think of it as a "factory" where the inner tree will be instantiated for every element you databind to.

## $(WhileCount) and $(WhileEmpty)

You can act on the count of a collection:

	<Each Items="{friends}">
		<!-- ... List friend ... -->
	</Each>
	<WhileEmpty Items="{friends}">
		<Text>No friends. :(</Text>
	</WhileEmpty>

`WhileEmpty` is a special case of `WhileCount` where the `EqualTo`-property is set to `0`. `WhileCount` accepts:

- `EqualTo` - Active when the count of the collection is equal to the provided value
- `GreaterThan` - Active when the count of the collection is greater than the provided value
- `LessThan` - Active when the count of the collection is less than the provided value

## $(Select)

If you have a complex data context and want to narrow the data context down, you can use `Select`:

	<JavaScript>
		module.exports = {
			complex: {
				item1: {
					subitem1: { name: "Spongebob" }
				}
			}
		};	
	</JavaScript>
	<Select Data="{complex.item1.subitem1}">
		<Text Value="{name}" />
	</Select>

While this might not seem immediately useful, consider that the `Data` property of the `Select` can also be data bound. AUTH: _Bad example? Is this true and a typical usecase?_

## $(Match) and $(Case)

You can drive which subtree should be active using `Match` and `Case`:

		<JavaScript>
			module.exports = {
				active: "blue"
			};	
		</JavaScript>
		<Match Value="{active}">
			<Case String="red">
				<Rectangle Fill="#f00" Height="50" Width="50" />
			</Case>
			<Case String="blue">
				<Rectangle Fill="#00f" Height="50" Width="50" />
			</Case>
		</Match>

Valid match properties for `Case` are:



## $(DataToResource)

> ### Ensuring order

`Each` does not neccessarily insert the children at the exact location in the tree where the `Each` node
resides. To ensure child order, wrap the `Each` in some sort of @(Panel):

	<StackPanel>
		<Text>I go first!</Text>
		<StackPanel>
			<Each Items="{strings}">
				<Text Value="{}" />
			</Each>
		</StackPanel>
		<Text>I go last!</Text>
	</StackPanel>

> We are considering adding more tricks to allow more flexibility in this case, but for now this
  is the recommended approach.