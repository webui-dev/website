<div align="center">

![Logo](data/webui_120.svg)

# WebUI v2.5 - Rust Documentation

> Use any web browser or WebView as GUI, with your preferred language in the backend and modern web technologies in the frontend, all in a lightweight portable library.

![Screenshot](data/screenshot.png)

</div>


## Getting Started
- [Download And Install](#download-and-install)
- [Minimal Example](#minimal-example)

## Available APIs
- **Window Management**
  - [new_window](#new_window)
  - [show](#show)
  - [show_browser](#show_browser)
  - [show_wv](#show_wv)
  - [start_server](#start_server)
  - [close](#close)
  - [destroy](#destroy)
  - [exit](#exit)
  - [wait](#wait)
  - [wait_async](#wait_async)
  - [is_shown](#is_shown)
  - [focus](#focus)
  - [set_hide](#set_hide)
  - [set_kiosk](#set_kiosk)
  - [set_size](#set_size)
  - [set_position](#set_position)
  - [set_center](#set_center)
  - [set_minimum_size](#set_minimum_size)
  - [minimize](#minimize)
  - [maximize](#maximize)
  - [set_resizable](#set_resizable)
  - [set_frameless](#set_frameless)
  - [set_transparent](#set_transparent)
  - [set_high_contrast](#set_high_contrast)
  - [is_high_contrast](#is_high_contrast)
  - [set_icon](#set_icon)
  - [set_timeout](#set_timeout)
  - [get_best_browser](#get_best_browser)
  - [get_url](#get_url)
  - [get_port](#get_port)
  - [set_port](#set_port)
  - [get_free_port](#get_free_port)
  - [get_parent_process_id](#get_parent_process_id)
  - [get_child_process_id](#get_child_process_id)
  - [get_hwnd](#get_hwnd)
- **Event Binding**
  - [bind](#bind)
  - [event](#event)
- **Event Arguments**
  - [get_count](#get_count)
  - [get_string](#get_string)
  - [get_string_at](#get_string_at)
  - [get_int](#get_int)
  - [get_int_at](#get_int_at)
  - [get_float](#get_float)
  - [get_float_at](#get_float_at)
  - [get_bool](#get_bool)
  - [get_bool_at](#get_bool_at)
  - [get_size](#get_size)
  - [get_size_at](#get_size_at)
  - [get_bytes](#get_bytes)
  - [get_bytes_at](#get_bytes_at)
  - [return_int](#return_int)
  - [return_float](#return_float)
  - [return_string](#return_string)
  - [return_bool](#return_bool)
- **JavaScript Execution**
  - [run](#run)
  - [run_js](#run_js)
  - [send_raw](#send_raw)
- **Multi-Client (Event Methods)**
  - [show_client](#show_client)
  - [close_client](#close_client)
  - [navigate_client](#navigate_client)
  - [run_client](#run_client)
  - [script_client](#script_client)
  - [send_raw_client](#send_raw_client)
- **Browser & Profile**
  - [set_profile](#set_profile)
  - [delete_profile](#delete_profile)
  - [delete_all_profiles](#delete_all_profiles)
  - [set_proxy](#set_proxy)
  - [set_custom_parameters](#set_custom_parameters)
  - [set_browser_folder](#set_browser_folder)
  - [browser_exist](#browser_exist)
  - [open_url](#open_url)
- **Server & Files**
  - [set_root_folder](#set_root_folder)
  - [set_default_root_folder](#set_default_root_folder)
  - [set_file_handler](#set_file_handler)
  - [set_file_handler_window](#set_file_handler_window)
  - [malloc](#malloc)
  - [get_mime_type](#get_mime_type)
  - [set_public](#set_public)
  - [navigate](#navigate)
- **Configuration**
  - [set_config](#set_config)
  - [set_runtime](#set_runtime)
  - [set_event_blocking](#set_event_blocking)
  - [set_tls_certificate](#set_tls_certificate)
  - [clean](#clean)
- **Error Handling**
  - [get_last_error_number](#get_last_error_number)
  - [get_last_error_message](#get_last_error_message)

[JavaScript APIs](javascript.md)


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### Download And Install

<p style="text-align: justify;">The WebUI library is written in pure C, but there is a wrapper in every popular programming language to provide easy-to-use APIs to write your backend application, while HTML5 and JavaScript are always used in the frontend inside a web browser or WebView window.</p>

1. Add `webui` to your Cargo dependencies:

  ```sh
  webui = { git = "https://github.com/webui-dev/rust-webui/", branch = "main" }

  # Or by git tag
  webui = { git = "https://github.com/webui-dev/rust-webui/", tag = "v2.5.0" }

  # Or by git commit
  webui = { git = "https://github.com/webui-dev/rust-webui/", rev = "a1b2c3d4" }
  ```

2. Bring in the static [WebUI static release](https://github.com/webui-dev/webui/releases) or [build action](https://github.com/webui-dev/webui/actions?query=branch%3Amain) file for your platform and place it in your project's root directory.


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### Minimal Example

```rust
use webui_rs::webui;

pub fn main() {
  let win = webui::Window::new();
  win.show("<html><script src=\"webui.js\"></script> Hello World from Rust! </html>");
  webui::wait();
}
```
[More Rust Examples](https://github.com/webui-dev/rust-webui/tree/main/examples).


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### new_window

Create a new window object.

```rust
let win = webui::Window::new();
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### show

Show a window using embedded HTML, a local file, or a URL. If the window is already open, it will be refreshed.

WebUI will try this pattern:

<div class="mermaid">
flowchart TD
    A[Microsoft Windows] --> B(WebView2Loader.dll ?)
    B --> |Not Found| D[Any Chromium Based Browser ?]
    B --> |Found| C[<strong>Show WebView2 window</strong>]
    D --> |Not Found| F[Any Other Browser ?]
    D --> |Found| E[<strong>Show chromium-based browser window</strong>. <em>Most cases will be Microsoft Edge</em>]
    F --> |Not Found| J[Use default browser]
    F --> |Found| I[<strong>Use that browser</strong>. <em>e.g. Firefox</em>]
</div>

<div class="mermaid">
flowchart TD
    A[Linux] --> B(WebKit GTK v3 ?)
    B --> |Not Found| D[Any Chromium Based Browser ?]
    B --> |Found| C[<strong>Show WebView GTK window</strong>]
    D --> |Not Found| F[Any Other Browser ?]
    D --> |Found| E[<strong>Show chromium-based browser window</strong>. <em>e.g. Chromium</em>]
    F --> |Not Found| J[Use default browser]
    F --> |Found| I[<strong>Use that browser</strong>. <em>Most cases will be Firefox</em>]
</div>

<div class="mermaid">
flowchart TD
    A[macOS] --> B(WebKit ?)
    B --> |Not Found| D[Any Chromium Based Browser ?]
    B --> |Found| C[<strong>Show WebKit window</strong>. <em>Most cases</em>]
    D --> |Not Found| F[Any Other Browser ?]
    D --> |Found| E[<strong>Show chromium-based browser window</strong>. <em>e.g. Chrome</em>]
    F --> |Not Found| J[Use default browser. <em>e.g. Safari</em>]
    F --> |Found| I[<strong>Use that browser</strong>. <em>e.g. Firefox</em>]
</div>

> To use only a specific browser please use `show_browser()`

> To use only WebView please use `show_wv()`

```rust
win.show("<html><script src=\"/webui.js\"> ... </html>");
win.show("file.html");
win.show("https://mydomain.com");
```

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### show_browser

Show a window using a specific web browser.

> It's recommended to use `ChromiumBased` browser.

> On macOS, the browser's icon may still appear in the Dock after exit. We recommend using `show_wv()` on macOS to avoid this.

```rust
win.show_browser("<html><script src=\"/webui.js\"> ... </html>", webui::Browser::Chrome);
win.show_browser("index.html", webui::Browser::Firefox);
win.show_browser("index.html", webui::Browser::ChromiumBased);
```

Available browser values:

```rust
webui::Browser::NoBrowser
webui::Browser::AnyBrowser
webui::Browser::Chrome
webui::Browser::Firefox
webui::Browser::Edge
webui::Browser::Safari
webui::Browser::Chromium
webui::Browser::Opera
webui::Browser::Brave
webui::Browser::Vivaldi
webui::Browser::Epic
webui::Browser::Yandex
webui::Browser::ChromiumBased
webui::Browser::Webview
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### show_wv

Show a window using the native WebView (no browser required).

```rust
win.show_wv("<html><script src=\"/webui.js\"> ... </html>");
win.show_wv("index.html");
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### start_server

Start only the local web server without showing a window. Returns the URL of the server so you can open it in any browser manually.

```rust
let url = win.start_server("/full/path/to/folder");
println!("Server running at: {}", url);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### close

Close a specific window. The window object will still exist and can be shown again later.

```rust
win.close();
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### destroy

Close a specific window and free all memory resources. The window object becomes invalid after this call.

```rust
win.destroy();
```

> The `Window` struct also calls `destroy()` automatically when it goes out of scope via the `Drop` trait.


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### exit

Close all open windows. `wait()` will return (_break_).

```rust
webui::exit();
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### wait

Block the current thread until all opened windows get closed.

```rust
use webui_rs::webui;

fn main() {
  // ...
  webui::wait();
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### wait_async

Non-blocking check that returns `true` if at least one window is still open. Useful for integrating WebUI into an existing event loop.

```rust
while webui::wait_async() {
  // Do other work while windows are still open
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### is_shown

Check if the specified window is still running.

```rust
if win.is_shown() {
    println!("The window is still running");
} else {
    println!("The window is closed.");
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### focus

Bring a window to the front and give it focus.

```rust
win.focus();
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_hide

Set a window to be hidden (invisible) on launch. Use `show()` to make it visible again.

```rust
win.set_hide(true);
win.show("index.html");
// Window is running but not visible
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_kiosk

Set the window to kiosk mode (full screen, no title bar or window controls).

```rust
win.set_kiosk(true);
win.show("index.html");
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_size

Set the window size in pixels.

```rust
win.set_size(800, 600);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_position

Set the window position in pixels from the top-left corner of the screen.

```rust
win.set_position(100, 200);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_center

Center the window on the screen.

```rust
win.set_center();
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_minimum_size

Set the minimum allowed window size in pixels. Works only on WebView windows.

```rust
win.set_minimum_size(400, 300);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### minimize

Minimize a WebView window.

```rust
win.minimize();
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### maximize

Maximize a WebView window.

```rust
win.maximize();
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_resizable

Set whether the window frame is resizable or fixed. Works only on WebView windows.

```rust
win.set_resizable(true);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_frameless

Remove the window frame and title bar (frameless window).

```rust
win.set_frameless(true);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_transparent

Enable a transparent window background. Requires a frameless window.

```rust
win.set_frameless(true);
win.set_transparent(true);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_high_contrast

Enable high-contrast mode for a window to improve visibility for users with visual impairments.

```rust
win.set_high_contrast(true);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### is_high_contrast

Check whether the operating system is currently using a high-contrast theme.

```rust
if webui::is_high_contrast() {
    println!("OS is using high-contrast theme");
    win.set_high_contrast(true);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_icon

Set the default favicon for the embedded HTML page.

```rust
// SVG Icon
let my_icon = "<svg>...</svg>";
let my_icon_type = "image/svg+xml";

// PNG Icon
// let my_icon = "data:image/...";
// let my_icon_type = "image/png";

// When the browser requests `favicon.ico`, WebUI will
// serve `my_icon` with the given MIME type.
win.set_icon(my_icon, my_icon_type);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_timeout

Set the maximum time in seconds to wait for the browser to connect. Affects `show()` and `wait()`. Use `0` to wait forever.

```rust
// Wait up to 10 seconds for the browser to connect
webui::set_timeout(10);
webui::wait();
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_best_browser

Get the recommended browser ID for this window. If a browser is already being used, the same ID is returned.

```rust
let browser_id = win.get_best_browser();
println!("Best browser ID: {}", browser_id);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_url

Get the full URL that this window is serving.

```rust
let url = win.get_url();
println!("Window URL: {}", url);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_port

Get the port number that the WebUI internal server is using for this window.

```rust
let port = win.get_port();
println!("Server port: {}", port);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_port

Set a specific port for the WebUI server to use for this window. Returns `true` on success.

> Call this before `show()`.

```rust
if win.set_port(8080) {
    println!("Port set successfully");
}
win.show("index.html");
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_free_port

Get a free available network port that can be used with `set_port()`.

```rust
let port = webui::get_free_port();
win.set_port(port);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_parent_process_id

Get the ID of the parent process (the browser or WebView process) for this window.

```rust
let pid = win.get_parent_process_id();
println!("Parent process ID: {}", pid);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_child_process_id

Get the ID of the child process (the browser tab/renderer process) for this window.

```rust
let pid = win.get_child_process_id();
println!("Child process ID: {}", pid);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_hwnd

Get the native window handle (`HWND` on Windows). Returns a raw pointer to the underlying OS window handle.

> This is Windows-specific.

```rust
let hwnd = win.get_hwnd();
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### bind

Bind an HTML element or a JavaScript function name to a Rust callback. Use an empty string to bind all events from all elements.

```rust
// With a named function
fn my_function(e: webui::Event) {
    // <button id="MyID">Click me</button> was clicked
}

win.bind("MyID", my_function);

// With a closure (use a free function or a static fn; closures need workarounds for lifetime reasons)
fn on_click(e: webui::Event) {
    println!("Button clicked!");
}
win.bind("MyButton", on_click);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### event

Every event callback receives an `Event` struct that contains information about what happened.

```rust
fn event_handler(e: webui::Event) {
    // e.window       — window ID
    // e.event_type   — EventType variant (Connected, Disconnected, MouseClick, Navigation, Callback)
    // e.event_number — unique event number
    // e.bind_id      — bind ID returned by bind()

    println!("Event on window ID: {}", e.window);

    // Get a Window object from the event
    let win = e.get_window();

    // Read arguments passed from JavaScript
    let s   = e.get_string();          // first arg as String
    let s2  = e.get_string_at(1);      // second arg as String
    let n   = e.get_int();             // first arg as i64
    let n2  = e.get_int_at(1);         // second arg as i64
    let f   = e.get_float();           // first arg as f64
    let b   = e.get_bool();            // first arg as bool
    let sz  = e.get_size();            // byte length of first arg
    let cnt = e.get_count();           // total number of arguments
    let raw = e.get_bytes();           // first arg as Vec<u8>

    // Return a value back to JavaScript
    e.return_int(42);
    e.return_float(3.14);
    e.return_string("hello");
    e.return_bool(true);
}

// Empty element name catches all events on all elements
win.bind("", event_handler);
```

Event types:

```rust
webui::EventType::Connected     // Browser connected
webui::EventType::Disconnected  // Browser disconnected
webui::EventType::MouseClick    // Element clicked
webui::EventType::Navigation    // Navigation occurred
webui::EventType::Callback      // JavaScript called a bound function
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_count

Get the total number of arguments passed from JavaScript to this event.

```rust
fn my_func(e: webui::Event) {
    let count = e.get_count();
    println!("Received {} argument(s)", count);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_string

Get the first argument as a `String`.

```rust
fn my_func(e: webui::Event) {
    let name = e.get_string();
    println!("Name: {}", name);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_string_at

Get an argument at a specific index as a `String`. Index `0` is the first argument.

```rust
fn my_func(e: webui::Event) {
    let first  = e.get_string_at(0);
    let second = e.get_string_at(1);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_int

Get the first argument as an `i64`.

```rust
fn my_func(e: webui::Event) {
    let value = e.get_int();
    println!("Value: {}", value);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_int_at

Get an argument at a specific index as an `i64`.

```rust
fn my_func(e: webui::Event) {
    let x = e.get_int_at(0);
    let y = e.get_int_at(1);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_float

Get the first argument as an `f64`.

```rust
fn my_func(e: webui::Event) {
    let ratio = e.get_float();
    println!("Ratio: {:.2}", ratio);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_float_at

Get an argument at a specific index as an `f64`.

```rust
fn my_func(e: webui::Event) {
    let lat = e.get_float_at(0);
    let lon = e.get_float_at(1);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_bool

Get the first argument as a `bool`.

```rust
fn my_func(e: webui::Event) {
    if e.get_bool() {
        println!("Enabled");
    }
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_bool_at

Get an argument at a specific index as a `bool`.

```rust
fn my_func(e: webui::Event) {
    let a = e.get_bool_at(0);
    let b = e.get_bool_at(1);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_size

Get the byte length of the first argument.

```rust
fn my_func(e: webui::Event) {
    let len = e.get_size();
    println!("First argument is {} bytes", len);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_size_at

Get the byte length of an argument at a specific index.

```rust
fn my_func(e: webui::Event) {
    let len0 = e.get_size_at(0);
    let len1 = e.get_size_at(1);
    println!("Arg 0: {} bytes, Arg 1: {} bytes", len0, len1);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_bytes

Get the first argument as raw bytes (`Vec<u8>`). Useful for binary data sent from JavaScript.

```rust
fn my_func(e: webui::Event) {
    let data = e.get_bytes();
    println!("Received {} bytes", data.len());
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_bytes_at

Get an argument at a specific index as raw bytes (`Vec<u8>`).

```rust
fn my_func(e: webui::Event) {
    let chunk0 = e.get_bytes_at(0);
    let chunk1 = e.get_bytes_at(1);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### return_int

Return an `i64` value back to JavaScript as the result of this callback.

```rust
fn multiply(e: webui::Event) {
    let a = e.get_int_at(0);
    let b = e.get_int_at(1);
    e.return_int(a * b);
}
```

On the JavaScript side:
```js
const result = await multiply(3, 7); // result === 21
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### return_float

Return an `f64` value back to JavaScript as the result of this callback.

```rust
fn get_pi(e: webui::Event) {
    e.return_float(std::f64::consts::PI);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### return_string

Return a `&str` value back to JavaScript as the result of this callback.

```rust
fn greet(e: webui::Event) {
    let name = e.get_string();
    let msg  = format!("Hello, {}!", name);
    e.return_string(&msg);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### return_bool

Return a `bool` value back to JavaScript as the result of this callback.

```rust
fn is_valid(e: webui::Event) {
    let input = e.get_string();
    e.return_bool(!input.is_empty());
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### run

Execute JavaScript in the browser without waiting for a response. Use this for fire-and-forget scripts.

```rust
// From anywhere with a window reference
win.run("document.getElementById('status').innerText = 'Done!';");

// From inside an event callback
fn on_click(e: webui::Event) {
    e.get_window().run("alert('Hello!');");
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### run_js

Execute JavaScript and get the response back. Returns a `JavaScript` struct containing the result or an error.

```rust
// The second argument is the response buffer size in bytes (0 = default 8 KB).
let result = win.run_js("return 2 + 2;", 0);

if result.error {
    println!("JS Error: {}", result.data);
} else {
    println!("JS Result: {}", result.data); // "4"
}

// Custom buffer size when you expect a large response
let result = win.run_js("return JSON.stringify(bigObject);", 1024 * 64);
```

The `JavaScript` struct:

```rust
pub struct JavaScript {
    pub timeout: usize, // Execution timeout in seconds (0 = default)
    pub script: String, // The script that was run
    pub error: bool,    // True if an error occurred
    pub data: String,   // The return value or error message
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### send_raw

Send raw binary data to a JavaScript function in the browser.

```rust
let data: Vec<u8> = vec![0x01, 0x02, 0x03, 0x04];
win.send_raw("myJsFunction", &data);
```

On the JavaScript side, the function receives the data as a `Uint8Array`:

```js
function myJsFunction(data) {
    // data is a Uint8Array
    console.log(data);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### show_client

Show a window to a specific client only (in multi-client mode). Called from within an event callback.

```rust
fn on_connect(e: webui::Event) {
    // Show a personalized page to this specific client
    e.show_client("<html><script src=\"/webui.js\"></script><p>Welcome!</p></html>");
}

win.bind("", on_connect);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### close_client

Close the connection to a specific client only (in multi-client mode). Called from within an event callback.

```rust
fn on_logout(e: webui::Event) {
    e.close_client();
}

win.bind("LogoutButton", on_logout);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### navigate_client

Navigate a specific client to a new URL. Called from within an event callback.

```rust
fn on_click(e: webui::Event) {
    e.navigate_client("https://webui.me");
}

win.bind("MyButton", on_click);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### run_client

Execute JavaScript in a specific client without waiting for a response. Called from within an event callback.

```rust
fn on_click(e: webui::Event) {
    e.run_client("document.title = 'Updated!';");
}

win.bind("MyButton", on_click);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### script_client

Execute JavaScript in a specific client and get the response back. Called from within an event callback.

```rust
fn on_click(e: webui::Event) {
    let result = e.script_client("return document.title;");
    if !result.error {
        println!("Page title: {}", result.data);
    }
}

win.bind("MyButton", on_click);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### send_raw_client

Send raw binary data to a JavaScript function in a specific client. Called from within an event callback.

```rust
fn on_click(e: webui::Event) {
    let data: Vec<u8> = vec![0x48, 0x65, 0x6C, 0x6C, 0x6F];
    e.send_raw_client("myJsFunction", &data);
}

win.bind("MyButton", on_click);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_profile

Set a custom browser profile (name and path) for this window. Use empty strings to reset to the default profile.

```rust
win.set_profile("MyApp", "/path/to/profile");
win.show("index.html");

// Reset to default profile
win.set_profile("", "");
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### delete_profile

Delete the browser profile associated with this window.

```rust
win.close();
win.delete_profile();
win.destroy();
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### delete_all_profiles

Delete all browser profiles created by WebUI.

```rust
webui::delete_all_profiles();
webui::clean();
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_proxy

Set a proxy server for the browser to use.

```rust
win.set_proxy("http://proxy.example.com:8080");
win.show("index.html");
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_custom_parameters

Pass additional command-line arguments to the browser process.

```rust
// Enable remote debugging on port 9222
win.set_custom_parameters("--remote-debugging-port=9222");
win.show("index.html");
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_browser_folder

Set a custom path where WebUI should look for the browser executable.

```rust
webui::set_browser_folder("/opt/custom-browser/");
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### browser_exist

Check whether a specific web browser is installed on the system.

```rust
if webui::browser_exist(webui::Browser::Chrome) {
    win.show_browser("index.html", webui::Browser::Chrome);
} else {
    win.show("index.html");
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### open_url

Open a URL in the system's default browser. This opens a regular browser tab, not a WebUI-controlled window.

```rust
webui::open_url("https://webui.me");
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_root_folder

Set the web server root folder path for a specific window. All relative file paths will be resolved from this folder.

```rust
win.set_root_folder("/home/user/myapp/public");
win.show("index.html");
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_default_root_folder

Set the default web server root folder path for all windows.

```rust
webui::set_default_root_folder("/home/user/myapp/public");
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_file_handler

Set a custom handler function to intercept and serve file requests. Return `null` to let WebUI handle the file normally (serve from disk or return 404).

```rust
use webui_rs::webui;
use std::ffi::CStr;
use std::os::raw::c_void;

unsafe extern "C" fn my_file_handler(
    filename: *const i8,
    length: *mut i32,
) -> *const c_void {
    let name = CStr::from_ptr(filename).to_str().unwrap_or("");

    if name == "/hello.txt" {
        // Static response — no allocation needed, WebUI reads immediately
        static RESPONSE: &[u8] = b"HTTP/1.1 200 OK\r\nContent-Type: text/plain\r\nContent-Length: 16\r\n\r\nHello from Rust!";
        return RESPONSE.as_ptr() as *const c_void;
    }

    if name == "/dynamic.html" {
        // Dynamic response — use webui::malloc so WebUI frees the buffer after serving
        let body = format!("<html><p>Time: {}</p></html>", std::time::SystemTime::now()
            .duration_since(std::time::UNIX_EPOCH).unwrap().as_secs());
        let response = format!(
            "HTTP/1.1 200 OK\r\nContent-Type: text/html\r\nContent-Length: {}\r\n\r\n{}",
            body.len(), body
        );
        return webui::malloc(&response, length);
    }

    // Return null to fall back to normal file serving
    std::ptr::null()
}

win.set_file_handler(my_file_handler);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_file_handler_window

Same as `set_file_handler()`, but the handler also receives the window ID, allowing you to serve different content per window.

```rust
unsafe extern "C" fn my_handler(
    window: usize,
    filename: *const i8,
    len: *mut i32,
) -> *const std::os::raw::c_void {
    let filename = unsafe { std::ffi::CStr::from_ptr(filename).to_str().unwrap() };
    println!("Window {window} requested: {filename}");
    std::ptr::null()
}

win.set_file_handler_window(my_handler);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### malloc

Allocate a response buffer owned by WebUI so it automatically frees the memory after serving the response. Use this inside a `set_file_handler` callback when the content is generated dynamically at runtime.

> For static content (compile-time strings or `&[u8]` literals), you can return a raw pointer directly without allocation.

```rust
use webui_rs::webui;
use std::ffi::CStr;
use std::os::raw::c_void;

unsafe extern "C" fn my_file_handler(
    filename: *const i8,
    length: *mut i32,
) -> *const c_void {
    let name = CStr::from_ptr(filename).to_str().unwrap_or("");

    if name == "/page.html" {
        let body = format!("<html><p>Counter: {}</p></html>", get_counter());
        let response = format!(
            "HTTP/1.1 200 OK\r\nContent-Type: text/html\r\nContent-Length: {}\r\n\r\n{}",
            body.len(), body
        );
        // webui::malloc copies `response` into a WebUI-managed buffer.
        // WebUI will free the buffer automatically after serving.
        return webui::malloc(&response, length);
    }

    std::ptr::null()
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_mime_type

Get the MIME type string for a given file extension or filename.

```rust
let mime = webui::get_mime_type("file.html");
println!("{}", mime); // "text/html"

let mime = webui::get_mime_type("image.png");
println!("{}", mime); // "image/png"
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_public

Allow or deny external network access to the WebUI server. By default, the server only listens on localhost.

```rust
// Allow connections from the local network
win.set_public(true);
win.show("index.html");
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### navigate

Navigate all clients connected to this window to a new URL.

```rust
win.navigate("https://webui.me");

// Navigate to a local page
win.navigate("settings.html");
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_config

Set global WebUI configuration options.

```rust
// Block until event callbacks return (default: non-blocking)
webui::set_config(webui::Config::UiEventBlocking, true);

// Enable folder monitoring for live-reload
webui::set_config(webui::Config::FolderMonitor, true);

// Enable multi-client support (multiple browser tabs per window)
webui::set_config(webui::Config::MultiClient, true);

// Control cookie usage
webui::set_config(webui::Config::UseCookies, false);
```

Available config options:

```rust
webui::Config::ShowWaitConnection   // Show a loading indicator while waiting for connection
webui::Config::UiEventBlocking      // Make event callbacks block until they return
webui::Config::FolderMonitor        // Monitor root folder and reload on file changes
webui::Config::MultiClient          // Allow multiple browser clients per window
webui::Config::UseCookies           // Use cookies for client identification
webui::Config::AsynchronousResponse // Allow asynchronous event responses
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_runtime

Choose a JavaScript runtime (`Deno`, `Bun`, or `NodeJS`) for serving `.js` and `.ts` files.

```rust
// Use Deno
win.set_runtime(webui::Runtime::Deno);

// Use Bun
win.set_runtime(webui::Runtime::Bun);

// Use Node.js
win.set_runtime(webui::Runtime::NodeJS);

// Disable runtime (serve files as-is)
win.set_runtime(webui::Runtime::None);
```

Once set, any browser HTTP request to a `.js` or `.ts` file will be processed by the selected runtime before being served.


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_event_blocking

Control whether event callbacks for this window block the event loop until they return. This overrides the global `set_config(UiEventBlocking, ...)` for a specific window.

```rust
win.set_event_blocking(true);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_tls_certificate

Set a TLS/SSL certificate and private key to enable HTTPS. Both arguments are PEM-formatted strings.

> Requires WebUI to be built with TLS support.

```rust
let cert_pem = "-----BEGIN CERTIFICATE-----\n...\n-----END CERTIFICATE-----";
let key_pem  = "-----BEGIN PRIVATE KEY-----\n...\n-----END PRIVATE KEY-----";

if webui::set_tls_certificate(cert_pem, key_pem) {
    println!("TLS enabled");
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### clean

Free all memory resources and stop the WebUI library. Call this after `wait()` returns as part of a clean shutdown.

```rust
webui::wait();
webui::clean();
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_last_error_number

Get the error code of the last error that occurred.

```rust
let code = webui::get_last_error_number();
if code != 0 {
    println!("Error code: {}", code);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_last_error_message

Get the error message string for the last error that occurred.

```rust
let msg = webui::get_last_error_message();
if !msg.is_empty() {
    eprintln!("WebUI error: {}", msg);
}
```
