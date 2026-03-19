<div align="center">

![Logo](data/webui_120.svg)

# WebUI v2.5 - Rust Documentation
***[ ! ] Beta 3 - Not Complete Yet***

> Use any web browser or WebView as GUI, with your preferred language in the backend and modern web technologies in the frontend, all in a lightweight portable library.

![Screenshot](data/screenshot.png)

</div>


## Available APIs

- [Download And Install](#download-and-install)
- [Minimal Example](#minimal-example)
- [new_window](#new_window)
- [bind](#bind)
- [event](#event)
- [show](#show)
- [show_browser](#show_browser)
- [wait](#wait)
- [close](#close)
- [exit](#exit)
- [set_file_handler](#set_file_handler)
- [is_shown](#is_shown)
- [set_timeout](#set_timeout)
- [set_icon](#set_icon)
- [script](#script)
- [set_runtime](#set_runtime)
- [JavaScript APIs](javascript.md)


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

Minimal example

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
### bind

Bind an HTML element event with a function. Empty element means all events.

```rust
// With external function
fn my_function(e: webui::Event) -> String {
    // <button id="MyID">Hello</button> gets clicked!
    "".to_string()
}

win.bind("MyID", my_function);

// With closure
win.bind("MyID2", |_: webui::Event| -> String {
    // <button id="MyID2">Hello</button> gets clicked!
    "".to_string()
});
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### event

Every event comes with information about the current event, like id, name, type (_click, connect, disconnect..._) and more.

```rust
fn event_handler(e: webui::Event) {
  println!("Hi!, You clicked on {} element", e.element);
}

// Empty ID means all events on all elements
win.bind("", event_handler);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### show

Show a window using embedded HTML, or a file. If the window is already open, it will be refreshed.

WebUI will try this patern:

- Windows
  - `WebView2Loader.dll` exists?
    - Use the WebView2 window.
  - Any Chromium Based Browser exists?
    - Use that chromium-based browser (_Most cases will be Microsoft Edge_).
  - Any Other Browser exists?
    - Use that browser (_Like Firefox_).
  - All failed?
    - Show the UI in the default browser (_Like a normal web site_)
- Linux
  - `WebKit GTK v3` exist?
    - Use the WebView GTK window.
  - Any Chromium Based Browser exists?
    - Use that chromium-based browser (_Like Chromium_).
  - Any Other Browser exists?
    - Use that browser (_Most cases will be Firefox_).
  - All failed?
    - Show the UI in the default browser (_Like a normal web site_)
- macOS
  - `WebKit` exist?
    - Use the WebView window (_Most cases_).
  - Any Chromium Based Browser exists?
    - Use that chromium-based browser (_Like Chrome_).
  - Any Other Browser exists?
    - Use that browser (_Like Firefox_).
  - All failed?
    - Show the UI in the default browser (_Safari, like a normal web site_)

> To use only a specific browser please use `show_browser()`, And to use only WebView please use `show_wv()`

```rust
win.show("<html><script src=\"/webui.js\"> ... </html>");
win.show("file.html");
win.show("https://mydomain.com");
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### show_browser

Show a window using a specific web browser.

> It's recommended to use `ChromiumBased` browser

> in macOS, the browser's icon may still shown in the Duck icons after exit, therefor we recommend to use `show_wv` instead, this icon issue exist only in macOS.

```rust
win.show_browser("<html><script src=\"/webui.js\"> ... </html>", webui::WebUIBrowser::Chrome);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### wait

Wait until all opened windows get closed.

```rust
use webui_rs::webui;

fn main() {
  // ...
  webui::wait();
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### close

Close a specific window.

> The `win` object will still alive, and it can be reopen later.

```rust
win.close();
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### exit

Close all open windows. This will make `wait()` return (_Break_).

```rust
webui::exit();
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_file_handler

Set a custom handler to serve files.

```rust
static mut COUNT: i32 = 0;

fn my_file_handler(filename: *const i8, len: *mut i32) -> *const std::os::raw::c_void {
    let filename = unsafe { std::ffi::CStr::from_ptr(filename).to_str().unwrap() };

    println!("File: {}", filename);

    if filename == "/test.txt" {
        // Const static file example
        // Note: The connection will drop if the content
        // does not have `<script src="/webui.js"></script>`
        let content = "This is a embedded file content example.";
        unsafe { *len = content.len() as i32 };
        return content.as_ptr() as *const std::os::raw::c_void;
    } else if filename == "/dynamic.html" {
        // Dynamic file example
        unsafe { COUNT += 1 };
        let content = format!(
            "<html>This is a dynamic file content example.<br>Count: {} <a href=\"dynamic.html\">[Refresh]</a><br><script src=\"/webui.js\"></script></html>",
            unsafe { COUNT }
        );
        unsafe { *len = content.len() as i32 };
        return content.as_ptr() as *const std::os::raw::c_void;
    }

    // A NULL return will make WebUI
    // looks for the file locally. if
    // not, WebUI will generate `404`
    std::ptr::null()
}

// Set a custom files handler
win.set_file_handler(my_file_handler);
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
### set_timeout

Set the maximum time in seconds to wait for the window to connect. This effect `show()` and `wait()`.

```rust
// Set timeout for 10 seconds
webui::set_timeout(10);

// Now, After 10 seconds, if the browser did
// not get started, wait() will break
webui::wait();
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_icon

Set the default embedded HTML favicon.

```rust
// SVG Icon
let my_icon = "<svg>...</svg>";
let my_icon_type = "image/svg+xml";

// PNG Icon
// let my_icon = "data:image/...";
// let my_icon_type = "image/png";

// When the web browser ask for `favicon.ico`, WebUI will
// send a redirection to `favicon.svg`, the body will be `my_icon`
// and the mime-type will be `my_icon_type`
win.set_icon(my_icon, my_icon_type);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### script

Run JavaScript and get the response back.

```rust
fn main() {
  // ...
  win.run_js("console.log('Hello from the backend!')");
}

fn event_handler(e: webui::Event) {
  e.get_window().run_js("console.log('Hello from the event handler!')");
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_runtime

Chose between `Deno`, `Bun` and `Nodejs` as runtime for `.js` and `.ts` files.

```rust
// Deno
win.set_runtime(webui::WebUIRuntime::Deno);

// Bun
win.set_runtime(webui::WebUIRuntime::Bun);

// Nodejs
win.set_runtime(webui::WebUIRuntime::Nodejs);

// Now, any HTTP request to any `.js` or `.ts` file
// will be interpreted by Deno.
//
// JavaScript:
//
// var xmlHttp = new XMLHttpRequest();
// xmlHttp.open('GET', 'test.ts?foo=123&bar=456', false);
// xmlHttp.send(null);
//
// console.log(xmlHttp.responseText);
```
