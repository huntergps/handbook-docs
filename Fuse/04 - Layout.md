# $(Layout)

Fuse has a powerful layout system that works across all platforms and devices, whether you
are building with @(NativeTheme:native) elements or @(GraphicsTheme:graphics) based elements.

<!-- * TODO: Links to jake's videos -->

## $(Panels)

Panels can contain child UI elements and lay them out according to layout rules. There are several types of panels, each with different layouting rules.

### $(Panel)
The most basic type of panel is the `Panel`. Children of a Panel will be default fill its entire space. If a panel contains several children it simply layers them on top of each other. Combining this behavior with @(Alignment:alignment), @(Margin:margin) and @(Padding:padding) can be quite useful in many situations.

```
<Panel>
	<Text>This...</Text>
	<Text>...will be on top of this</Text>
	<Rectangle Alignment="BottomLeft" Height="20" Width="20" Fill="#678" />
</Panel>
```

Note that the element order in a `Panel` is the same as the layer order in popular graphics packages such as Photoshop; the layer that appears first in the UX-file will be layered on top of elements appearing later in the file.

### $(StackPanel)
The StackPanel places its children in a stack. The default layout is a vertical stack, but one can use the $(Orientation) property to specify that the stack should be layed out horizontally.

```
<StackPanel Orientation="Horizontal">
	... elements ...
</StackPanel>
```

### $(Grid)
The Grid places its children in a grid formation. The rows and columns can be specified explicitly by the @(Rows) and @(Columns) properties, or implicitly by assigning the @(RowCount) and @(ColumnCount) properties.

#### $(RowCount) and $(ColumnCount)
If all that is needed is a grid of equally sized rows and columns one can simply state the number of rows and columns using the RowCount and ColumnCount properties.
```
<Grid RowCount="4" ColumnCount="2"/>
```
#### $(Rows) and $(Columns)
More fine grained control of how the rows and column sizes are calculated can be achieved with the Rows and Columns properties. These properties are assigned to a comma separated list of values which can take on a few different forms.
The values can either be absolute, relative or automatic.

Example of a Grid with 3 rows of size 10, 10 and 50 points.
```
<Grid Rows="10,10,50"/>
```

Example of a Grid with 3 rows where the first two each occupy 20% of the available space, and the last one occupies 60%:
```
<Grid Rows="1*,1*,3*"/>
```
The rows sizes here are calculated by first summing all the values (1+1+3 = 5).
Then we divide our value by the total (1/5 = 20%, 1/5 = 20%, 3/5 = 60%).

The following grid has 3 rows where the first two rows gets the size of its largest child and the last row takes up the remaining space:
```
<Grid Rows="auto,auto,1*"/>
```

### $(Grid.Row:Placing elements in a Grid) $(Grid.Column:)
By default, elements are placed in the grid by the order they appear in the UX, from left to right, top to bottom. One can specify per element which grid cell they should be placed in using the Row and Column like so:
```
<Grid RowCount="1" ColumnCount="2">
	<Rectangle Row="1" Column="2" />
	<Rectangle Row="1" Column="1" />
</Grid>
```

### $(WrapPanel)
The wrap panel lays out its children one after the other and wraps around whenever it reaches the end. One can specify which direction the elements are layed out in by assigning the $(FlowDirection) property. FlowDirection can either be `LeftToRight` or `RightToLeft`.

The following WrapPanel layes out its children horizontally from right ro left.
```
<WrapPanel FlowDirection="RightToLeft"/>
```

The Orientation property can be used to make a vertical `WrapPanel` like so:
```
<WrapPanel Orientation="Vertical"/>
```

<!-- TODO: Add information about `Alignment="Bottom"` to start adding at the bottom.

TODO: Illustration -->

### $(DockPanel)
The DockPanel layes out its children by docking them to the different sides, one after the other. One can specify which side per element by using the $(Dock) property like so:
```
<Rectangle Dock="Left"/>
```
The Dock property can be assigned to be either `Left`, `Right`, `Top`, `Bottom` or `Fill` (which is the default).

```
<DockPanel>
	<Rectangle Fill="Red" Dock="Left"/>
	<Rectangle Fill="Green" Dock="Top"/>
	<Rectangle Fill="Blue" Dock="Right"/>
	<Rectangle Fill="Yellow" Dock="Bottom"/>
	<Rectangle Fill="Teal" />
</DockPanel>
```

<!-- TODO: Illustration
TODO: Add information about docking multiple elements to the same dock
TODO: Order of elements added to dock affects size -->

## Element Layout

<!-- TODO: Link to video -->

Whenever a @(Panel) performs layout on its children, it has to ask each element about its layout properties.

Layout properties are assigned per element to control such things as the elements `Width`, `Height`, @(Margin) and @(Padding).
If an element doesn't specify these things, the panel performing layout on them will handle it.

Available space, @(points) (vs @(pixels)).

### $(Alignment)

When elements are positioned in a panel they may not require all of the space available to them. For example, a vertical stack panel will be as wide as its largest element, leaving extra space for the smaller elements. Elements can either be aligned within this space, or stretched to fill it.

We can align elements to the sides of its container by the @(Alignment) property.
In the following example, we have a @(Panel) which is 500x500 in size, which contains a @(Rectangle) which is only 50x50 points in size. By setting alignment to BottomRight, this rectangle will be positioned by the bottom right corner of the panel.

```
<Panel>
	<Rectangle Width="50" Height="50" Alignment="BottomRight"/>
</Panel>
```

Alignment can be assigned to any one of the following values:
- $(Left)
- $(HorizontalCenter)
- $(Right)
- $(Top)
- $(VerticalCenter)
- $(Bottom)
- $(TopLeft)
- $(TopCenter)
- $(TopRight)
- $(CenterLeft)
- $(Center)
- $(CenterRight)
- $(BottomLeft)
- $(BottomCenterter)
- $(BottomRight)

If you don't assign an `Alignment` explicitly, the default alignment will be to stretch the control as much as the parent control requests. For a normal `Panel`, this means that the child control will try to fill the parent `Panel`, but other containing controls might ask the children to behave differently.

### $(Width) and $(Height)

You can combine @(Alignment) and other layout properties with `Width` and `Height`, which will set the size of the @(Element) in the desired @(Units:Unit).

### $(Margin) and $(Padding)
Each element can specify the amount of space between it and its parent or surrounding elements by using its @(Margin) property.

Each element can also specify how much space should be between its borders and any element inside it by using the @(Padding) property.

Both @(Margin) and @(Padding) are specified using a comma separated list of 4 values which defines the left, top, right and bottom values in that order.
Here is an example of a @(Rectangle) with a margin of 20 to the left, 30 to the top, 50 to the right and 10 to the bottom:
```
<Rectangle Margin="20,30,50,10"/>
```
Here is a rectangle where all sides have the same margin:
```
<Rectangle Margin="50"/>
```
This rectangle has a margin of 50 for its left and right, and 20 for its top and bottom:
```
<Rectangle Margin="50,20"/>
```

### $(Units)
There are multiple ways of specifying values on layout properties, points, percent and pixels.
The following properties support units
- Width
- Height
- MinWidth
- MinHeight
- MaxWidth
- MaxHeight
- @Offset
- @Anchor

* Note that the column and row properties of @(Grid) use their own system, proportion, auto. <!-- (TODO: Finish sentence?). -->

### Specifying units in $(points)
When setting an elements Width to a number like so `<Element Width="50"/>`, the element will become 50 points wide.

Points are different from pixels in that they will represent multiple pixels on high density displays. This way, using points will give a you a consistent look on all different screen densities.

### Specifying units in $(Percentage:percent) (%)
Specifying values using the percent sign means that this value should be a certain percentage of available space.
For example, if we place an @(Element) inside a @(Panel) which is 500 points wide, and set the elements Width property to be 50%, we get an element which is 250 points wide.

### Specifying units in $(Pixels:pixels)
We can specify values in pixels using the px suffix like so:
```
<Rectangle Width="200px"/>
```
This rectangle will be exactly 200 pixels wide, which means it will be smaller when the screen density is high.

### $(Anchor) and $(Offset)
When a @(Panel) places its children, it assumes that the "center" of that element is in the middle. However, if we want the element to be placed as if its "center" was along its left side, we could use the @(Anchor) property like so:
```
<Rectangle Anchor="0,50%"/>
```
This puts the elements anchor in the middle of its left edge.

### $(StatusBarBackground)
iOS and Android devices usually has a status bar aligned to the top of the screen which shows status information from the operating system like battery and network information. This status bar might or might not be visible while our app is running. We also might or might not be able to draw behind it. The @(StatusBarBackground) element is used to compensate for the status bar. It will always have the same size as the status bar across all platforms and devices. We can use a @(DockPanel) to dock this element on the top of our app and "offset" the rest of our content so it fits within the visible parts of our screen.
```
<App>
	<DockPanel>
		<StatusBarBackground Dock="Top"/>
		<Panel>
			... our apps content ...
		</Panel>
	</DockPanel>
</App>
```
### $(BottomBarBackground)
The BottomBarBackground element is quite similar to the @(StatusBarBackground) in that it takes on the same size as certain OS specific elements. The BottomBarBackground will take on the same size as the keyboard (whenever it is visible). Certain Android devices have their home button on the screen instead of as a physical button. The BottomBarBackground will also take this into account when sizing itself.
Here is how we can make sure our content is never covered by the keyboard or home button:
```
<App>
	<DockPanel>
		<Panel>
			... our apps content ...
		</Panel>
		<BottomBarBackground Dock="Bottom"/>
	</DockPanel>
</App>
```

### $(Absolute positioning) $(X:) $(Y:)

If we want to give our elements an explicit position, we can assign their `X` and `Y` properties. The `X` property will move the element relative to the left side of its container, while the `Y` property moves it relative to the top.

Be aware that absolute positioning elements should generally be avoided in favor of using layout rules. This is because when real data is used, the absolute values used might no longer be meaningfull.
