<div align="center">

![Logo](data/webui_120.svg)

# WebUI v2.5 - V Documentation

> Use any web browser or WebView as GUI, with your preferred language in the backend and modern web technologies in the frontend, all in a lightweight portable library.

![Screenshot](data/screenshot.png)

</div>


## Getting Started
- [Download And Install](#download-and-install)
- [Minimal Example](#minimal-example)

## Available APIs
- **Windows**
  - [new_window](#new_window)
  - [new_window_id](#new_window_id)
  - [show](#show)
  - [show_browser](#show_browser)
  - [show_wv](#show_wv)
  - [show_client](#show_client)
  - [start_server](#start_server)
  - [wait](#wait)
  - [wait_async](#wait_async)
  - [close](#close)
  - [close_client](#close_client)
  - [destroy](#destroy)
  - [exit](#exit)
  - [is_shown](#is_shown)
  - [focus](#focus)
  - [minimize](#minimize)
  - [maximize](#maximize)
  - [navigate](#navigate)
  - [navigate_client](#navigate_client)
- **Binding & Events**
  - [bind](#bind)
  - [event](#event)
- **JavaScript**
  - [run](#run)
  - [run_client](#run_client)
  - [script](#script)
  - [script_client](#script_client)
  - [get_arg](#get_arg)
  - [set_runtime](#set_runtime)
- **Window Appearance**
  - [set_size](#set_size)
  - [set_minimum_size](#set_minimum_size)
  - [set_position](#set_position)
  - [set_center](#set_center)
  - [set_hide](#set_hide)
  - [set_kiosk](#set_kiosk)
  - [set_icon](#set_icon)
  - [set_frameless](#set_frameless)
  - [set_transparent](#set_transparent)
  - [set_resizable](#set_resizable)
  - [set_high_contrast](#set_high_contrast)
  - [is_high_contrast](#is_high_contrast)
- **Browser & Profiles**
  - [get_best_browser](#get_best_browser)
  - [browser_exist](#browser_exist)
  - [set_profile](#set_profile)
  - [set_proxy](#set_proxy)
  - [set_public](#set_public)
  - [set_custom_parameters](#set_custom_parameters)
  - [set_browser_folder](#set_browser_folder)
  - [delete_profile](#delete_profile)
  - [delete_all_profiles](#delete_all_profiles)
- **Server & Network**
  - [set_root_folder](#set_root_folder)
  - [get_url](#get_url)
  - [open_url](#open_url)
  - [get_port](#get_port)
  - [set_port](#set_port)
  - [get_free_port](#get_free_port)
  - [get_mime_type](#get_mime_type)
- **Configuration**
  - [set_config](#set_config)
  - [set_event_blocking](#set_event_blocking)
  - [set_timeout](#set_timeout)
- **Utilities**
  - [encode](#encode)
  - [decode](#decode)
  - [clean](#clean)
  - [get_last_error](#get_last_error)
- **Process Info**
  - [get_parent_process_id](#get_parent_process_id)
  - [get_child_process_id](#get_child_process_id)
- **SSL/TLS**
  - [set_tls_certificate](#set_tls_certificate)

[JavaScript APIs](javascript.md)


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### Download And Install

<p style="text-align: justify;">The WebUI library is written in pure C, but there is a wrapper in every popular programming language to provide easy-to-use APIs to write your backend application, while HTML5 and JavaScript are always used in the frontend inside a web browser or WebView window.</p>

```sh
v install https://github.com/webui-dev/v-webui
```

> **Note:** TCC is not supported. Use GCC (`v -cc gcc`) or Clang (`v -cc clang`).


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### Minimal Example

```v
import vwebui as ui

fn main() {
    mut win := ui.new_window()
    win.show('<html><script src="webui.js"></script> Hello World from V! </html>')!
    ui.wait()
}
```

[More V Examples](https://github.com/webui-dev/v-webui/tree/main/examples).


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### new_window

Create a new window object.

```v
win := ui.new_window()
```

To create a window using a specific ID (the ID must be between 1 and `WEBUI_MAX_IDS`):

```v
// Reserve window ID 5 first
win_id := ui.new_window_id()

// Later, create the window using that ID
win_id.new_window()
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### new_window_id

Get a free window ID that can be used with the `new_window` method.

```v
win_id := ui.new_window_id()
win_id.new_window()
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### bind

Bind an HTML element or JavaScript object with a backend V function. Empty element name means all events.

The function receives an `Event` and its return value is automatically sent back to JavaScript.

```v
import vwebui as ui

fn my_handler(e &ui.Event) string {
    // <button id="MyBtn">Click me</button> was clicked
    name := e.get_arg[string]() or { 'World' }
    return 'Hello, ${name}!'
}

win.bind('MyBtn', my_handler)

// Bind all events
win.bind('', my_handler)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### event

Every callback receives an `Event` containing information about the current event.

```v
pub struct Event {
pub:
    window        ui.Window   // The window object number
    event_type    ui.EventType // Event type (click, connect, disconnect...)
    element       string      // HTML element ID that triggered the event
    event_number  usize       // Internal WebUI event number
    bind_id       usize       // Bind ID
    client_id     usize       // Client's unique ID (useful in multi-client mode)
    connection_id usize       // Client's connection ID
    cookies       string      // Client's full cookies
}
```

```v
fn my_handler(e &ui.Event) {
    println('Element: ${e.element}')
    println('Client ID: ${e.client_id}')
    println('Event type: ${e.event_type}')
    println('Cookies: ${e.cookies}')
}
```

**EventType values:**

| Value | Description |
|---|---|
| `disconnected` | Window disconnection event |
| `connected` | Window connection event |
| `mouse_click` | Mouse click event |
| `navigation` | Window navigation event |
| `callback` | Function call event |


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### show

Show a window using embedded HTML, a local file, or a URL. If the window is already open, it will be refreshed. This refreshes all clients in multi-client mode.

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

```v
win.show('<html><script src="webui.js"></script> Hello! </html>')!
win.show('index.html')!
win.show('https://mydomain.com')!

// Wait for the window to be shown before continuing
win.show('index.html', await: true)!

// Wait with a custom timeout (default is 10 seconds)
win.show('index.html', await: true, timeout: 5)!
```

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### show_browser

Show a window using a specific web browser.

> It's recommended to use `.chromium_based` for the best compatibility.

> On macOS, the browser icon may remain in the Dock after exit. Use `show_wv()` to avoid this.

**Browser values:**

| Value | Browser |
|---|---|
| `no_browser` | No browser (server only) |
| `any` | Default recommended browser |
| `chrome` | Google Chrome |
| `firefox` | Mozilla Firefox |
| `edge` | Microsoft Edge |
| `safari` | Apple Safari |
| `chromium` | The Chromium Project |
| `opera` | Opera Browser |
| `brave` | Brave Browser |
| `vivaldi` | Vivaldi Browser |
| `epic` | Epic Browser |
| `yandex` | Yandex Browser |
| `chromium_based` | Any Chromium-based browser |
| `webview` | WebView (not a web browser) |

```v
win.show_browser('<html><script src="webui.js"></script> Hello! </html>', .chrome)!
win.show_browser('index.html', .edge)!
win.show_browser('index.html', .chromium_based)!
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### show_wv

Show a WebView window using embedded HTML, a local file, or a URL.

> On Windows, `WebView2Loader.dll` is required.

```v
win.show_wv('<html><script src="webui.js"></script> Hello! </html>')!
win.show_wv('index.html')!
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### show_client

Show a window for a single connected client. Useful in multi-client mode to send different content to different users.

```v
fn my_handler(e &ui.Event) {
    // Show different content to this specific client
    e.show_client('<html><script src="webui.js"></script> Welcome, client ${e.client_id}! </html>')!
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### start_server

Start only the local web server and return its URL. No window will be shown. Useful for integrating WebUI with an external web server.

```v
url := win.start_server('/path/to/root/folder')
println('Server running at: ${url}')
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### wait

Wait until all opened windows get closed. This is a blocking call and should typically be the last statement in `main()`.

```v
fn main() {
    win := ui.new_window()
    win.bind('MyBtn', my_handler)
    win.show('index.html')!

    ui.wait() // Blocks here until all windows are closed
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### wait_async

A non-blocking alternative to `wait()`. Returns `true` while windows are still open, `false` when all windows have been closed. Use this when you need to run code on the main thread while windows are open (required for WebView mode).

```v
for ui.wait_async() {
    // Your main thread work here
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### close

Close a specific window. The window object still exists and the window can be shown again later.

```v
win.close()
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### close_client

Close the connection of a single client without closing the window.

```v
fn my_handler(e &ui.Event) {
    e.close_client()
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### destroy

Close the window and free all its memory resources. Use this when you no longer need the window object.

```v
win.destroy()
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### exit

Close all open windows. `wait()` will return.

```v
ui.exit()
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### is_shown

Check if the specified window is still running.

```v
if win.is_shown() {
    println('The window is still running')
} else {
    println('The window is closed.')
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### focus

Bring a window to the front and give it focus.

```v
win.focus()
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### minimize

Minimize a WebView window.

```v
win.minimize()
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### maximize

Maximize a WebView window.

```v
win.maximize()
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### navigate

Navigate all connected clients to a specific URL.

```v
win.navigate('https://mydomain.com')
win.navigate('other_page.html')
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### navigate_client

Navigate a single client to a specific URL.

```v
fn my_handler(e &ui.Event) {
    e.navigate_client('https://mydomain.com')
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### run

Execute JavaScript in the browser without waiting for a response. Sends to all connected clients.

```v
win.run("alert('Hello!');")
win.run("document.getElementById('MyDiv').innerHTML = 'Updated!';")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### run_client

Execute JavaScript for a single connected client without waiting for a response.

```v
fn my_handler(e &ui.Event) {
    e.run_client("alert('Hello, client ${e.client_id}!');")
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### script

Execute JavaScript and get the response back. Works in single-client mode.

The default response buffer is 8 KiB. Use `max_response_size` to increase it.

```v
fn my_handler(e &ui.Event) {
    // Run JavaScript and get the result
    result := e.window.script('return 2 * 2;') or {
        eprintln('Script error: ${err}')
        return
    }
    println('JavaScript result: ${result}') // "4"

    // With a custom buffer size and timeout
    long_result := e.window.script('return getLargeData();',
        max_response_size: 64 * 1024,
        timeout: 10
    ) or { '' }
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### script_client

Execute JavaScript for a single connected client and get the response back.

```v
fn my_handler(e &ui.Event) {
    result := e.script_client('return document.title;') or {
        eprintln('Script error: ${err}')
        return
    }
    println('Page title: ${result}')
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_arg

Get a JavaScript argument passed to the bound V function. Arguments are zero-indexed.

Supported types: `int`, `i64`, `f64`, `string`, `bool`, and any struct or array (decoded from JSON).

```v
fn my_handler(e &ui.Event) {
    // Get the first argument (index 0 is the default)
    name := e.get_arg[string]() or { '' }

    // Get argument at a specific index
    count := e.get_arg[int](idx: 1) or { 0 }
    ratio := e.get_arg[f64](idx: 2) or { 0.0 }
    flag  := e.get_arg[bool](idx: 3) or { false }

    println('name=${name}, count=${count}, ratio=${ratio}, flag=${flag}')
}
```

JavaScript side:
```js
// Call V function with multiple arguments
webui.MyBtn('Alice', 42, 3.14, true);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_runtime

Set the runtime for `.js` and `.ts` files served by WebUI.

**Runtime values:**

| Value | Description |
|---|---|
| `.none` | No runtime (default) |
| `.deno` | Use Deno |
| `.nodejs` | Use Node.js |
| `.bun` | Use Bun |

```v
win.set_runtime(.deno)
win.set_runtime(.nodejs)
win.set_runtime(.bun)
```

Once set, any HTTP request to a `.js` or `.ts` file will be executed by the chosen runtime:

```js
// JavaScript:
var xhr = new XMLHttpRequest();
xhr.open('GET', 'script.ts?foo=123', false);
xhr.send(null);
console.log(xhr.responseText);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_size

Set the window size in pixels.

```v
win.set_size(800, 600)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_minimum_size

Set the minimum window size in pixels. The user cannot resize the window below this size.

```v
win.set_minimum_size(400, 300)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_position

Set the window position on the screen.

```v
win.set_position(100, 100)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_center

Center the window on the screen. Call before `show()` for best results.

```v
win.set_center()
win.show('index.html')!
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_hide

Show the window in hidden mode. The process runs but the window is not visible. Must be called before `show()`.

```v
win.set_hide(true)
win.show('index.html')!
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_kiosk

Set the window to Kiosk mode (full screen, no browser UI). Must be called before `show()`.

```v
win.set_kiosk(true)
win.show('index.html')!
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_icon

Set the default embedded HTML favicon. Overrides the default WebUI icon.

```v
win.set_icon('<svg>...</svg>', 'image/svg+xml')
win.set_icon(my_png_bytes, 'image/png')
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_frameless

Make a WebView window frameless (no title bar or borders).

```v
win.set_frameless(true)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_transparent

Make a WebView window background transparent.

```v
win.set_transparent(true)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_resizable

Set whether a WebView window is resizable. Works only on WebView windows.

```v
win.set_resizable(false) // Fixed size
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_high_contrast

Enable high-contrast support for the window. Use this alongside high-contrast CSS to improve accessibility.

```v
win.set_high_contrast(true)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### is_high_contrast

Check if the operating system is currently using a high-contrast theme.

```v
if ui.is_high_contrast() {
    win.show('high-contrast.html')!
} else {
    win.show('index.html')!
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_best_browser

Get the recommended web browser ID to use for this window. If a browser is already in use, returns the same ID.

```v
best := win.get_best_browser()
println('Best browser: ${best}')
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### browser_exist

Check if a specific web browser is installed on the system.

```v
if ui.browser_exist(.chrome) {
    win.show_browser('index.html', .chrome)!
} else {
    win.show('index.html')!
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_profile

Set the web browser profile to use. An empty `name` and `path` uses the default user profile. Must be called before `show()`.

```v
// Use a custom profile
win.set_profile('MyAppProfile', '/path/to/profiles/MyAppProfile')

// Use the default browser profile
win.set_profile('', '')
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_proxy

Set the web browser proxy server. Must be called before `show()`.

```v
win.set_proxy('http://127.0.0.1:8888')
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_public

Allow the window's address to be accessible from a public network. By default, windows are only accessible locally.

```v
win.set_public(true)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_custom_parameters

Add custom command-line parameters to the web browser launch command.

```v
win.set_custom_parameters('--remote-debugging-port=9222')
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_browser_folder

Set a custom folder path where WebUI should look for the web browser executable.

```v
ui.set_browser_folder('/opt/my-chrome/bin')
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### delete_profile

Delete the local browser profile folder for a specific window. Call after `wait()`.

> This may break other windows using the same browser profile.

```v
ui.wait()
win.delete_profile()
ui.clean()
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### delete_all_profiles

Delete all local browser profile folders. Call after `wait()`.

```v
ui.wait()
ui.delete_all_profiles()
ui.clean()
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_root_folder

Set the web server root folder path. Can be set per-window or globally for all windows. Must be called before `show()`.

```v
// Set for a specific window
win.set_root_folder('/path/to/ui/folder')!

// Set for all windows
ui.set_root_folder('/path/to/shared/folder')!
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_url

Get the full URL of a running window's web server.

```v
url := win.get_url()
println('Window URL: ${url}') // e.g. "http://localhost:12345"
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### open_url

Open a URL in the system's native default web browser (outside of WebUI).

```v
ui.open_url('https://webui.me')
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_port

Get the network port being used by a running window's web server.

```v
port := win.get_port()
println('WebUI JS URL: http://localhost:${port}/webui.js')
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_port

Set a specific network port for the window's web server. Useful when integrating with an external web server like Nginx.

```v
win.set_port(8080)!
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_free_port

Get an available free network port on the system.

```v
port := ui.get_free_port()
println('Free port: ${port}')
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_mime_type

Get the HTTP MIME type string for a given filename or extension.

```v
mime := ui.get_mime_type('photo.png')
println(mime) // "image/png"
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_config

Control WebUI global behaviour. Recommended to call at the start of the application.

**Config options:**

| Option | Default | Description |
|---|---|---|
| `show_wait_connection` | `true` | Wait for the browser to connect before `show()` returns |
| `ui_event_blocking` | `false` | Process all UI events in a single blocking thread |
| `folder_monitor` | `false` | Auto-refresh the window when files in the root folder change |
| `multi_client` | `false` | Allow multiple clients to connect to the same window |
| `use_cookies` | `true` | Use `webui_auth` cookies to block unauthorized access |
| `asynchronous_response` | `false` | Wait for `return_x()` to be called before responding to JS |

```v
// Allow multiple browser tabs to connect to the same window
ui.set_config(.multi_client, true)

// Auto-reload the UI when source files change (great for development)
ui.set_config(.folder_monitor, true)

// Process all events one at a time (useful for non-thread-safe backends)
ui.set_config(.ui_event_blocking, true)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_event_blocking

Control event processing for a single window: either one at a time in a blocking thread (`true`) or each event in a new non-blocking thread (`false`). Use `set_config(.ui_event_blocking, ...)` to apply this to all windows.

```v
win.set_event_blocking(true)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_timeout

Set the maximum time in seconds to wait for the browser to start and connect. A value of `0` means wait forever.

```v
ui.set_timeout(30)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### encode

Encode a string to Base64. Use this to safely pass binary or special-character data to the JavaScript frontend.

```v
encoded := ui.encode('Hello, World!')
println(encoded) // "SGVsbG8sIFdvcmxkIQ=="
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### decode

Decode a Base64-encoded string received from the JavaScript frontend.

```v
decoded := ui.decode('SGVsbG8sIFdvcmxkIQ==')
println(decoded) // "Hello, World!"
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### clean

Free all WebUI memory resources. Should be called only at the very end of the application, after `wait()`.

```v
ui.wait()
ui.clean()
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_last_error

Get the last error code and message from WebUI. Useful for diagnosing failures when an API call does not return an error directly.

```v
code := ui.get_last_error_number()
msg  := ui.get_last_error_message()
println('Error ${code}: ${msg}')
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_parent_process_id

Get the process ID of the backend application (the parent process).

```v
pid := win.get_parent_process_id()
println('Backend PID: ${pid}')
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_child_process_id

Get the process ID of the web browser window (the child process). In WebView mode this returns the same PID as `get_parent_process_id()` since both run in the same process.

```v
pid := win.get_child_process_id()
println('Browser PID: ${pid}')
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_tls_certificate

Set the SSL/TLS certificate and private key (both in PEM format) to enable HTTPS. Requires compiling with `-d tls`. If called with empty strings, WebUI generates a self-signed certificate.

```v
cert := '-----BEGIN CERTIFICATE-----\n...'
key  := '-----BEGIN PRIVATE KEY-----\n...'
ui.set_tls_certificate(cert, key)!
```

Compile with TLS support:
```sh
v -d tls -cc gcc run main.v
```
