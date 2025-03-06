# plotters-cosmic-iced
[![Test and Build](https://github.com/D-Brox/plotters-cosmic-iced/actions/workflows/test.yml/badge.svg)](https://github.com/D-Brox/plotters-cosmic-iced/actions/workflows/test.yml/badge.svg)
[![Documentation](https://docs.rs/plotters-iced/badge.svg)](https://docs.rs/plotters-iced)
[![Crates.io](https://img.shields.io/crates/v/plotters-iced.svg)](https://crates.io/crates/plotters-iced)
[![License](https://img.shields.io/crates/l/plotters-iced.svg)](https://github.com/joylei/plotters-iced/blob/master/LICENSE)

This is a `libcosmic::iced` port of the original implementation of an Iced backend for Plotters.

This backend has been optimized as for speed. Note that some specific plotting features supported in the Bitmap backend may not be implemented there, though.

## Showcase

![CPU Monitor Example](./images/plotter_iced_demo.png)

![WASM Example](./images/split-chart-web.png)

## What is Plotters?

Plotters is an extensible Rust drawing library that can be used to plot data on nice-looking graphs, rendering them through a plotting backend (eg. to a Bitmap image raw buffer, to your GUI backend, to an SVG file, etc.).

**For more details on Plotters, please check the following links:**

- For an introduction of Plotters, see: [Plotters on Crates.io](https://crates.io/crates/plotters);
- Check the main repository on [GitHub](https://github.com/plotters-rs/plotters);
- You can also visit the Plotters [homepage](https://plotters-rs.github.io/);

## How to install?

Include `plotters-iced` in your `Cargo.toml` dependencies:

```toml

[dependencies]
plotters="0.3"
plotters-iced = { git = "https://github.com/D-Brox/plotters-cosmic-iced.git" }

[dependencies.libcosmic]
git = "https://github.com/pop-os/libcosmic.git"
default-deatures = false
features = ["tokio"] # add any required extra features if needed
```

## How to use?

First, import `Chart` and `ChartWidget`:

```rust,ignore
use plotters_iced::{Chart, ChartWidget, DrawingBackend, ChartBuilder};
```

Then, derive `Chart` trait and build your chart, and let `plotters-iced` takes care the rest:

```rust,ignore
struct MyChart;
impl Chart<Message> for MyChart {
    type State = ();
    fn build_chart<DB:DrawingBackend>(&self, state: &Self::State, builder: ChartBuilder<DB>) {
        //build your chart here, please refer to plotters for more details
    }
}
```

Finally, render your chart view:

```rust,ignore
impl MyChart {
    fn view(&mut self)->Element<Message> {
        ChartWidget::new(self)
            .width(Length::Fixed(200))
            .height(Length::Fixed(200))
            .into()
    }
}
```

## Are there any limitations?

### Limitation #1: No image rendering

No image rendering for native and wasm applications.

### Limitation #2: Limited text rendering for native applications

Only TTF font family are supported for text rendering, which is a limitation of `Iced`, please look at  [cpu-monitor.rs](./examples/cpu-monitor.rs). As well, font transforms are not supported,which is also a limitation of `Iced`.

## Credits

- [plotters-conrod](https://github.com/valeriansaliou/plotters-conrod)
- [plotters-iced](https://github.com/joylei/plotters-iced)
