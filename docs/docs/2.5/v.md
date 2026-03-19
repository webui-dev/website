<div align="center">

![Logo](data/webui_120.svg)

# WebUI v2.5 - V Documentation
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
- [is_shown](#is_shown)
- [script](#script)
- [set_runtime](#set_runtime)
- [JavaScript APIs](javascript.md)


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### Download And Install

<p style="text-align: justify;">The WebUI library is written in pure C, but there is a wrapper in every popular programming language to provide easy-to-use APIs to write your backend application, while HTML5 and JavaScript are always used in the frontend inside a web browser or WebView window.</p>

```sh
v install https://github.com/webui-dev/v-webui
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### Minimal Example

Minimal example

```v
import vwebui as ui

mut win := ui.new_window()
win.show('<html><script src="webui.js"></script> Hello World from V! </html>')!
ui.wait()
```
[More V Examples](https://github.com/webui-dev/v-webui/tree/main/examples).


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### new_window

Create a new window object.

```v
mut myWindow := ui.new_window()
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### bind

Bind an HTML element event with a function. Empty element means all events.

```v
import vwebui as webui

fn my_function_string(e &webui.Event) {
    // <button id="MyID">Hello</button> gets clicked!
}

win.bind("MyID", my_function)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### event

Every event comes with information about the current event, like id, name, type (_click, connect, disconnect..._) and more.

```v
fn my_function(e &webui.Event) {
    // Get target element
    target_element := e.element
}
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

```v
myWindow.show('<html><script src="/webui.js"> ... </html>')
myWindow.show('file.html')
myWindow.show('https://mydomain.com')
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### show_browser

Show a window using a specific web browser.

> It's recommended to use `ChromiumBased` browser

> in macOS, the browser's icon may still shown in the Duck icons after exit, therefor we recommend to use `show_wv` instead, this icon issue exist only in macOS.

```v
myWindow.show_browser('<html><script src="/webui.js"> ... </html>', .chrome)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### wait

Wait until all opened windows get closed.

```v
fn main() {
    // Create windows...
    // Bind HTML elements...
    // Show the windows...
    // Show a window using the embedded HTML

    // Wait until all windows get closed
    // or when calling webui.exit()
    win.wait()
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### close

Close a specific window.

> The `win` object will still alive, and it can be reopen later.

```v
win.close()
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### exit

Close all open windows. This will make `wait()` return (_Break_).

```v
webui.exit()
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### is_shown

Check if the specified window is still running.

```v
if webui.is_shown(win) {
    println('The window is still running')
} else {
    println('The window is closed.')
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### script

Run JavaScript and get the response back.

```v
fn my_function(e &webui.Event) {
    // Run JavaScript
    response := e.window.script('return 2*2;', 0, 64)

    // Print the result
    println('JavaScript Response: ${response.int()}') // 4

    // Run JavaScript quickly with no waiting for the response
    e.window.run("alert('Fast!');")
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_runtime

Chose between `Deno`, `Bun` and `Nodejs` as runtime for `.js` and `.ts` files.

```v
// Deno
win.set_runtime(.runtime_deno)

// Bun
win.set_runtime(.runtime_bun)

// Nodejs
win.set_runtime(.runtime_nodejs)

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
