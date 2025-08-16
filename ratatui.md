---
id: ratatui
aliases:
  - Ratatui
tags: []
---

# Ratatui
Ratatui is a library for making rust based Terminal User Interfaces


### Widgets
from [https://ratatui.rs/concepts/widgets/]
Widgets are the building blocks of user interfaces in Ratatui. They are used to create and manage the layout and style of the terminal interface. Widgets can be combined and nested to create complex UIs, and can be easily customized to suit the needs of your application.

Ratatui provides a wide variety of built-in widgets that can be used to quickly create UIs. These widgets include:
    Block|Example: A basic Widget that draws a block with optional borders, titles, and styles.
    BarChart|Example: Displays multiple datasets as bars with optional grouping.
    Calendar|Example: Displays a single month.
    Canvas|Example: Draws arbitrary shapes using drawing characters.
    Chart|Example: Displays multiple datasets as a lines or scatter graph.
    Clear: Clears the area it occupies. Useful to render over previously drawn widgets.
    Gauge|Example: Displays progress percentage using block characters.
    LineGauge: Display progress as a line.
    List|Example: Displays a list of items and allows selection.
    Paragraph|Example: Displays a paragraph of optionally styled and wrapped text.
    Scrollbar|Example: Displays a scrollbar.
    Sparkline|Example: Display a single data set as a sparkline.
    Table|Example: Displays multiple rows and columns in a grid and allows selection.
    Tabs|Example: Displays a tab bar and allows selection.

Additionally, String, &str, Span, Line, and Text can be used as widgets (though it’s common to use Paragraph instead of these directly).

For more information on these widgets, you can view the Widgets API docs and the Widget showcase. Additionally, there are several third-party widgets available that can be used with Ratatui, which can be found on the third-party widgets showcase and in the Awesome Ratatui repository.


### Widget Traits

In Ratatui, widgets are implemented as Rust traits, which allow for easy implementation and extension. The two main traits for widgets are Widget and StatefulWidget, which provide the basic functionality for rendering and managing the state of a Widget.

Additionally, the WidgetRef and StatefulWidgetRef traits allow for rendering widgets by reference, which can be useful for storing and rendering collections of widgets. The latter two traits were added in Ratatui 0.26 and are at the time of writing, gated by an unstable feature flag, so there may be limited third party use of these traits. All the internal widgets have been updated to implement the ref traits and there is also a blanket implementation of Widget for &T where T: WidgetRef.
Widget

The Widget trait is the most basic trait for widgets in Ratatui. It provides the basic functionality for rendering a Widget onto a buffer.

```rust
pub trait Widget {
    fn render(self, area: Rect, buf: &mut Buffer);
}
```

WidgetRef and StatefulWidgetRef

The WidgetRef trait allows for rendering a Widget by reference instead of consuming the widget, which can be useful for storing and rendering individual or collections of widgets.

The StatefulWidgetRef trait is similar to the WidgetRef trait, but also includes a state that can be managed and updated during rendering.

These two traits were introduced in Ratatui 0.26.0 to help avoid a shortcoming that meant that widgets were always consumed on rendering while not breaking all code that has previously been built with that assumption. These two widgets are currently marked as unstable and gated behind the unstable-widget-ref feature flag.

```rust
pub trait WidgetRef {
    fn render_ref(&self, area: Rect, buf: &mut Buffer);
}

pub trait StatefulWidgetRef {
    type State;
    fn render_ref(&self, area: Rect, buf: &mut Buffer, state: &mut Self::State);
}

```
