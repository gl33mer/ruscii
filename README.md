[![lancat](https://img.shields.io/crates/v/ruscii)](https://crates.io/crates/ruscii)
[![license](https://img.shields.io/crates/l/ruscii)](https://www.apache.org/licenses/LICENSE-2.0.txt)
[![downloads](https://img.shields.io/crates/d/ruscii)](https://crates.io/crates/ruscii)

# `ruscii`
An easy library to make terminal applications and games in *rust*.

The aim of this project is to make easier the game development in terminals.
Any contribution, issue, or pull request would be welcome!

## Features
- Optimized to render fast in terminals.
- Multi-platform (Linux, Windows and MacOS)
- Multi-terminal (See [crossterm terminal support](https://github.com/crossterm-rs/crossterm#tested-terminals))
- Enable **key press** and **release** events in terminal (essential for games!)
- Easy to use. Make your terminal game in a few lines!
- Easy way to recover the terminal state at error.

## Examples
You can found several examples into the [example folder](examples).

To test an example, install `ruscii` with the examples flag and run it.
```
cargo install ruscii --examples
~/.cargo/bin/<example_name>
```

Or clone the repo and execute inside of it:
```
cargo run --example <example_name> --release
```

### Some of these examples:
#### Space invaders ([200 lines](examples/space_invaders.rs)):
  [![asciicast](https://asciinema.org/a/291004.svg)](https://asciinema.org/a/291004)

#### Pong ([150 lines](examples/pong.rs)):
  [![asciicast](https://asciinema.org/a/291007.svg)](https://asciinema.org/a/291007)

Note: the first `asciimedia` playback could be shown laggy, a second playback fix this issue.

## Getting started

### Test it in your own terminal!
Add the following line to your dependencies section in `Cargo.toml` file:
```
ruscii = "0.2"
```

Copy the following code in your `main.rs` to run the base `ruscii` application:
```rust
use ruscii::app::{App, State};
use ruscii::terminal::{Window};
use ruscii::drawing::{Pencil};
use ruscii::keyboard::{KeyEvent, Key};
use ruscii::spatial::{Vec2};
use ruscii::gui::{FPSCounter};

fn main() {
    let mut fps_counter = FPSCounter::new();
    let mut app = App::new();

    app.run(|app_state: &mut State, window: &mut Window| {
        for key_event in app_state.keyboard().last_key_events() {
            match key_event {
                KeyEvent::Pressed(Key::Esc) => app_state.stop(),
                KeyEvent::Pressed(Key::Q) => app_state.stop(),
                _ => (),
            }
        }

        fps_counter.update();

        let mut pencil = Pencil::new(window.canvas_mut());
        pencil.draw_text(&format!("FPS: {}", fps_counter.count()), Vec2::xy(1, 1));
    });
}
```

