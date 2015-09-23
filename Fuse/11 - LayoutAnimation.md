# $(LayoutAnimation) and $(MultiLayoutPanel)

AUTH: Layout properties (correct word?)
There is a certain kind of animation which deserves special attention. When an @(Element) has certain properties like @(Width), @(Height) or @(Margin) (collectively reffered to as "layout properties") changed or when its location in the visual tree changes. In Fuse we call these types of animation LayoutAnimations.

Calculating layout for a large UX-document can actually be quite costly. When animating layout properties with @(Change) animators, we run the risk of forcing a new layout to be calculated each frame. This can very easily lead to frame drops.

Fuse offers a neat alternative to this: the @(LayoutAnimation) trigger.
For example, if we instead of animating the @(Width) of an @(Element) using @(Change), we can use @(Set) and react to this change using a @(LayoutAnimation). Here is a quick example:

	<Panel>
		<Panel ux:Name="panel" Width="50" Height="50" Background="Teal">
			<LayoutAnimation>
				<Resize RelativeTo="LayoutChange" Duration="0.5"/>
				<Move RelativeTo="LayoutChange" Duration="0.5"/>
			</LayoutAnimation>
			<Clicked>
				<Set panel.Width="200"/>
				<Set panel.Height="300"/>
			</Clicked>
		</Panel>
	</Panel>

The @(Clicked) trigger has two @(Set) actions in it. They set the @(Width) and the @(Height) of the @(Panel).
The @(Panel) has a @(LayoutAnimation) on it with a @(Resize) and @(Move) animator. Both these animators have their `RelativeTo` property set to `LayoutChange`. This means that they will animate from whetever the value was before the new layout was calculated, to whatever the new values are. Because changing the @(Width), and @(Height) of the @(Panel) leads to it having its position changed, we also use a @(Move) animator to make a smooth animation.

## Animating other layout properties

@(LayoutAnimation) opens up a whole world of possibilities. We can for example use it to animate an @(Element:elements) alignment:

	<Panel>
		<Panel ux:Name="panel" Width="50" Height="50" Background="Teal" Alignment="Center">
			<LayoutAnimation>
				<Move RelativeTo="LayoutChange" Duration="0.5"/>
			</LayoutAnimation>
			<Clicked>
				<Set panel.Alignment="BottomRight"/>
			</Clicked>
		</Panel>
	</Panel>

In this example, we use a @(Set) action to change the alignment og a panel from @(Center) to @(BottomRight). We use a @(LayoutAnimation) with a @(Move) in order to make it move to its new position.


## $(MultiLayoutPanel)

`MultiLayout` allows us to move @(Element:elements) between different @(Layout:layouts). This allows us to @(Move) elements between different locations in the visual tree, and also switch between certain layouts on the fly.

## $(Placeholder)
The `Placeholder` element is used to reference other @(Element:elements) in the visual tree. A `Placeholder` is itself an @(Element) and it can have its own layout properties. Here is a quick example, showing how we can move a @(Rectangle) from one @(Panel) to another. In this example we use a @(MultilayoutPanel) with a @(DockLayout). We put three @(Panel:panels) inside it, docket do different locations. We use @(Placeholder:placeholders) to reference one @(Rectangle) and move it between the three @(Panel:panels).

	<MultiLayoutPanel ux:Name="multiLayoutPanel">
		<DockLayout/>
		<Panel ux:Name="panel1">
			<Placeholder Width="50" Height="50">
				<Rectangle ux:Name="rect" Fill="Teal">
					<LayoutAnimation>
						<Move RelativeTo="LayoutChange" Duration="1"/>
						<Resize RelativeTo="LayoutChange" Duration="1"/>
					</LayoutAnimation>
				</Rectangle>
				<Clicked>
					<Set multiLayoutPanel.LayoutElement="panel2"/>
				</Clicked>
			</Placeholder>
		</Panel>
		<Panel ux:Name="panel2" Dock="Right" Width="200">
			<Placeholder Target="rect">
				<Clicked>
					<Set multiLayoutPanel.LayoutElement="panel3"/>
				</Clicked>
			</Placeholder>
		</Panel>
		<Panel ux:Name="panel3" Dock="Bottom" Height="100">
			<Placeholder Target="rect">
				<Clicked>
					<Set multiLayoutPanel.LayoutElement="panel1"/>
				</Clicked>
			</Placeholder>
		</Panel>
	</MultiLayoutPanel>

MultiLayout can be a bit tricky to wrap ones head around, so lets go through a few more uses cases.
