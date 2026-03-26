<div align="center">

![Logo](data/webui_120.svg)

# WebUI v2.5 - Odin Documentation

> Use any web browser or WebView as GUI, with your preferred language in the backend and modern web technologies in the frontend, all in a lightweight portable library.

![Screenshot](data/screenshot.png)

</div>


## Getting Started
- [Download And Install](#download-and-install)
- [Minimal Example](#minimal-example)

## Available APIs
- **Window**
  - [new_window](#new_window)
  - [new_window_id](#new_window_id)
  - [get_new_window_id](#get_new_window_id)
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
  - [set_timeout](#set_timeout)
- **Window Appearance**
  - [set_kiosk](#set_kiosk)
  - [set_hide](#set_hide)
  - [set_size](#set_size)
  - [set_minimum_size](#set_minimum_size)
  - [set_position](#set_position)
  - [set_center](#set_center)
  - [set_icon](#set_icon)
  - [set_high_contrast](#set_high_contrast)
  - [is_high_contrast](#is_high_contrast)
  - [set_resizable](#set_resizable)
  - [set_frameless](#set_frameless)
  - [set_transparent](#set_transparent)
  - [set_custom_parameters](#set_custom_parameters)
- **Binding & Events**
  - [bind](#bind)
  - [set_context](#set_context)
  - [get_context](#get_context)
  - [event](#event)
  - [get_count](#get_count)
  - [get_int_at](#get_int_at)
  - [get_int](#get_int)
  - [get_float_at](#get_float_at)
  - [get_float](#get_float)
  - [get_string_at](#get_string_at)
  - [get_string](#get_string)
  - [get_bool_at](#get_bool_at)
  - [get_bool](#get_bool)
  - [get_size_at](#get_size_at)
  - [get_size](#get_size)
  - [get_arg](#get_arg)
  - [return_int](#return_int)
  - [return_float](#return_float)
  - [return_string](#return_string)
  - [return_bool](#return_bool)
  - [result](#result)
- **JavaScript**
  - [run](#run)
  - [run_client](#run_client)
  - [script](#script)
  - [script_client](#script_client)
  - [set_runtime](#set_runtime)
  - [send_raw](#send_raw)
  - [send_raw_client](#send_raw_client)
- **Navigation**
  - [navigate](#navigate)
  - [navigate_client](#navigate_client)
  - [get_url](#get_url)
  - [open_url](#open_url)
  - [set_public](#set_public)
- **File Serving**
  - [set_root_folder](#set_root_folder)
  - [set_default_root_folder](#set_default_root_folder)
  - [set_browser_folder](#set_browser_folder)
  - [set_file_handler](#set_file_handler)
  - [set_file_handler_window](#set_file_handler_window)
  - [interface_set_response_file_handler](#interface_set_response_file_handler)
  - [set_close_handler_wv](#set_close_handler_wv)
- **Browser & Profile**
  - [get_best_browser](#get_best_browser)
  - [browser_exist](#browser_exist)
  - [set_profile](#set_profile)
  - [set_proxy](#set_proxy)
  - [delete_profile](#delete_profile)
  - [delete_all_profiles](#delete_all_profiles)
- **Network & Ports**
  - [get_port](#get_port)
  - [get_free_port](#get_free_port)
  - [set_port](#set_port)
- **Configuration**
  - [set_config](#set_config)
  - [set_event_blocking](#set_event_blocking)
  - [set_tls_certificate](#set_tls_certificate)
  - [set_logger](#set_logger)
- **Process & System**
  - [get_parent_process_id](#get_parent_process_id)
  - [get_child_process_id](#get_child_process_id)
  - [win32_get_hwnd](#win32_get_hwnd)
  - [get_hwnd](#get_hwnd)
- **Memory & Utilities**
  - [encode](#encode)
  - [decode](#decode)
  - [malloc](#malloc)
  - [memcpy](#memcpy)
  - [free](#free)
  - [get_mime_type](#get_mime_type)
  - [get_last_error_number](#get_last_error_number)
  - [get_last_error_message](#get_last_error_message)
  - [clean](#clean)

[JavaScript APIs](javascript.md)


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### Download And Install

<p style="text-align: justify;">The WebUI library is written in pure C, but there is a wrapper in every popular programming language to provide easy-to-use APIs to write your backend application, while HTML5 and JavaScript are always used in the frontend inside a web browser or WebView window.</p>

```sh
# Add odin-webui as a submodule to your project
git submodule add https://github.com/webui-dev/odin-webui.git webui

# Linux/MacOS
webui/setup.sh

# Windows
webui/setup.ps1
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### Minimal Example

```odin
// main.odin
package main

import ui "webui"

main :: proc() {
    my_window: uint = ui.new_window()
    ui.show(my_window, "<html> <script src=\"webui.js\"></script> Hello World from Odin! </html>")
    ui.wait()
}
```
[More Odin Examples](https://github.com/webui-dev/odin-webui/tree/main/examples).


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### new_window

Create a new window object.

```odin
package main

import ui "webui"

main :: proc() {
    my_window: uint = ui.new_window()

    // Later
    ui.show(my_window, "index.html")
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### new_window_id

Create a new window object using a specified ID.

> The `id` should be between 1 and 255

```odin
package main

import ui "webui"

main :: proc() {
    /*
    * @param window_number The window number (should be > 0, and < 255)
    */

    win: uint = 1
    ui.new_window_id(win)

    // Later
    ui.show(win, "index.html")
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_new_window_id

Get a free window ID that can be used later with `new_window_id` to create a new window object.

```odin
package main

import ui "webui"

main :: proc() {

    win: uint = ui.get_new_window_id()

    // Later
    ui.new_window_id(win)
    ui.show(win, "index.html")
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### show

Show a window using embedded HTML, or a file. If the window is already open, it will be refreshed.

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

```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param window The window number
    * @param content The HTML, URL, Or a local file
    */

    ui.show(my_window, "<html><script src=\"/webui.js\"></script> ... </html>")
    ui.show(my_window, "file.html")
    ui.show(my_window, "https://mydomain.com")
}
```

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### show_browser

Show a window using a specific web browser.

> It's recommended to use `ChromiumBased` browser

> in macOS, the browser's icon may still shown in the Duck icons after exit, therefor we recommend to use `show_wv` instead, this icon issue exist only in macOS.

```odin
package main

import ui "webui"

main :: proc() {

    /*
        Browser :: enum {
            NoBrowser,      // 0. No web browser
            AnyBrowser,     // 1. Default recommended web browser
            Chrome,         // 2. Google Chrome
            Firefox,        // 3. Mozilla Firefox
            Edge,           // 4. Microsoft Edge
            Safari,         // 5. Apple Safari
            Chromium,       // 6. The Chromium Project
            Opera,          // 7. Opera Browser
            Brave,          // 8. The Brave Browser
            Vivaldi,        // 9. The Vivaldi Browser
            Epic,           // 10. The Epic Browser
            Yandex,         // 11. The Yandex Browser
            ChromiumBased,  // 12. Any Chromium based browser
            Webview,        // 13. WebView (Non-web-browser)
        }
    */

    /*
    * @param window The window number
    * @param content The HTML, Or a local file
    * @param browser The web browser to be used
    */

    ui.show_browser(my_window, "<html><script src=\"/webui.js\"></script> ... </html>", .Chrome)
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### show_wv

Show a **WebView** window using embedded HTML, or a file. If the window is already open, it will be refreshed.

> WebUI's primary focus is using web browsers as GUI, but if you need to use WebView instead of a web browser, then you can use this API, which was added to WebUI starting from v2.5.

> Windows Dependencies: WebView2, and `WebView2Loader.dll`.
>
> Linux Dependencies: WebKit GTK v3.
>
> macOS Dependencies: WebKit (_WKWebView_).

```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param window The window number
    * @param content The HTML, URL, Or a local file
    */

    ui.show_wv(my_window, "index.html")
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### show_client

Show a window for a single connected client. If the window is already open, it will be refreshed. This is useful in multi-client mode to target a specific client.

```odin
package main

import ui "webui"

callback :: proc "c" (e: ^ui.Event) {
    context = runtime.default_context()

    /*
    * @param e The event struct
    * @param content The HTML, URL, Or a local file
    */

    ui.show_client(e, "<html><script src=\"/webui.js\"></script> Hello! </html>")
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### start_server

Start only the local web server and return the URL. No window will be shown. This is useful for web apps where you want to control the browser separately.

```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param window The window number
    * @param content The local file or local folder to serve
    */

    url: cstring = ui.start_server(myWindow, "/full/root/path")
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### wait

Wait until all opened windows get closed.

```odin
package main

import ui "webui"

main :: proc() {

    ui.wait()
}
```
Full Odin Example: https://github.com/webui-dev/odin-webui/blob/main/examples/minimal.odin


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### wait_async

A non-blocking alternative to `wait()`. Returns `true` if windows are still open, `false` when all windows are closed. Call this in a loop so your main thread can perform work between checks.

> In WebView mode, you must call this from the main thread.

```odin
package main

import ui "webui"

main :: proc() {

    // ...show windows...

    for ui.wait_async() {
        // Your main thread code runs here while windows are open
    }
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### close

Close a specific window. All connected clients will be disconnected.

> The window object will still exist and can be shown again later.

```odin
package main

import ui "webui"

main :: proc() {
    /*
    * @param window The window number
    */

    ui.close(win)
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### close_client

Close the connection for a specific client only. Other clients connected to the same window remain unaffected.

```odin
package main

import ui "webui"

callback :: proc "c" (e: ^ui.Event) {
    context = runtime.default_context()

    /*
    * @param e The event struct
    */

    ui.close_client(e)
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### destroy

Close a specific window and free all related memory resources.

> This will make the window object unavailable. If you want to simply close a window and reopen it later, use `close()` instead.

```odin
package main

import ui "webui"

main :: proc() {
    /*
    * @param window The window number
    */

    ui.destroy(win)
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### exit

Close all open windows. This will make `wait()` return (_Break_).

```odin
package main

import ui "webui"

main :: proc() {

    ui.exit()
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### is_shown

Check if the specified window is still running.

```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param window The window number
    */

    if ui.is_shown(my_window) {
        // Window is shown
    } else {
        // Window is closed
    }
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### focus

Bring a window to the front and give it input focus.

```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param window The window number
    */

    ui.focus(my_window)
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### minimize

Minimize a WebView window.

```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param window The window number
    */

    ui.minimize(my_window)
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### maximize

Maximize a WebView window.

```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param window The window number
    */

    ui.maximize(my_window)
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_timeout

Set the maximum time in seconds to wait for the window to connect. This affects `show()` and `wait()`. A value of `0` means wait forever.

```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param second The timeout in seconds
    */

    ui.set_timeout(30)

    ui.show(win, "index.html")
    ui.wait()
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_kiosk

Set the window in Kiosk mode (_Full screen_).

```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param window The window number
    * @param status True or False
    */

    ui.set_kiosk(my_window, true)
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_hide

Set a window in hidden mode.

> Should be called before `show()`.

```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param window The window number
    * @param status The status: True or False
    */

    ui.set_hide(my_window, true)
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_size

Set the window size.

```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param window The window number
    * @param width The window width
    * @param height The window height
    */

    ui.set_size(my_window, 800, 600)
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_minimum_size

Set the minimum window size. The user will not be able to resize the window below this size.

```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param window The window number
    * @param width The minimum window width
    * @param height The minimum window height
    */

    ui.set_minimum_size(my_window, 400, 300)
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_position

Set the window position on the screen.

```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param window The window number
    * @param x The window X coordinate
    * @param y The window Y coordinate
    */

    ui.set_position(my_window, 100, 100)
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_center

Center the window on the screen.

> Call this before `show()` for best results. Works better with WebView.

```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param window The window number
    */

    ui.set_center(my_window)
    ui.show(my_window, "index.html")
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_icon

Set the default embedded HTML favicon.

```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param window The window number
    * @param icon The icon as string: '<svg>...</svg>'
    * @param icon_type The icon type: 'image/svg+xml'
    */

    ui.set_icon(win, "<svg>...</svg>", "image/svg+xml")
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_high_contrast

Set the window with high-contrast support. Useful when you want to build a better high-contrast theme with CSS.

```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param window The window number
    * @param status True or False
    */

    ui.set_high_contrast(my_window, true)
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### is_high_contrast

Get the OS high-contrast preference. Use this to adapt your UI theme at startup.

```odin
package main

import ui "webui"

main :: proc() {

    if ui.is_high_contrast() {
        ui.show(my_window, "high_contrast.html")
    } else {
        ui.show(my_window, "index.html")
    }
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_resizable

Set whether the window frame is resizable or fixed. Works only with WebView windows.

```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param window The window number
    * @param status True (resizable) or False (fixed size)
    */

    ui.set_resizable(my_window, false)
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_frameless

Make a WebView window frameless, removing the title bar and window borders.

```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param window The window number
    * @param status True or False
    */

    ui.set_frameless(my_window, true)
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_transparent

Make a WebView window transparent.

```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param window The window number
    * @param status True or False
    */

    ui.set_transparent(my_window, true)
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_custom_parameters

Add user-defined CLI parameters that will be passed to the web browser when it is launched.

```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param window The window number
    * @param params Command line parameters
    */

    ui.set_custom_parameters(my_window, "--remote-debugging-port=9222")
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### bind

Bind an HTML element event with a backend function. An empty element name means all events.

```odin
package main

import ui "webui"

events :: proc "c" (e: ^ui.Event) {
    context = runtime.default_context()
    //
}

callback :: proc "c" (e: ^ui.Event) {
    context = runtime.default_context()
    //
}

main :: proc() {

    /*
    * @param window The window number
    * @param element The HTML element / JavaScript object
    * @param func The callback function
    */

    ui.bind(win, "", events)
    ui.bind(win, "callback", callback)
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_context

Associate arbitrary user data with a bound element. The data can be retrieved inside the callback using `get_context()`. Must be called after `bind()`.

```odin
package main

import ui "webui"

MyData :: struct {
    name: string,
    value: int,
}

callback :: proc "c" (e: ^ui.Event) {
    context = runtime.default_context()

    data := cast(^MyData)ui.get_context(e)
    fmt.println(data.name, data.value)
}

main :: proc() {

    data := new(MyData)
    data.name = "hello"
    data.value = 42

    /*
    * @param window The window number
    * @param element The HTML element / JavaScript object
    * @param context Any user data pointer
    */

    ui.bind(my_window, "callback", callback)
    ui.set_context(my_window, "callback", data)
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_context

Retrieve user data previously associated with a bound element using `set_context()`.

```odin
package main

import ui "webui"

MyData :: struct {
    name: string,
}

callback :: proc "c" (e: ^ui.Event) {
    context = runtime.default_context()

    /*
    * @param e The event struct
    */

    data := cast(^MyData)ui.get_context(e)
    fmt.println(data.name)
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### event

Every event comes with information about the current event, like id, name, type (_click, connect, disconnect..._) and more.

```odin
package main

import ui "webui"

/*
    Event :: struct {
        window:        c.size_t,     // The window number
        event_type:    EventType,    // Event type
        element:       cstring,      // HTML element ID
        event_number:  c.size_t,     // Internal WebUI
        bind_id:       c.size_t,     // Bind ID
        client_id:     c.size_t,     // Client's unique ID
        connection_id: c.size_t,     // Client's connection ID
        cookies:       cstring,      // Client's full cookies
    }

    EventType :: enum {
        Disconnected,         // 0. Window disconnection event
        Connected,            // 1. Window connection event
        MouseClick,           // 2. Mouse click event
        Navigation,           // 3. Window navigation event
        Callback,             // 4. Function call event
    }
*/

events :: proc "c" (e: ^ui.Event) {
    context = runtime.default_context()

    switch e.event_type {
        case .Connected:
            fmt.println("Connected.")
        case .Disconnected:
            fmt.println("Disconnected.")
        case .MouseClick:
            fmt.println("Click.")
        case .Navigation:
            target, _ := ui.get_arg(string, e)
            fmt.println("Starting navigation to:", target)
        case .Callback:
            fmt.println("Callback")
    }
}

main :: proc() {

    /*
    * @param window The window number
    * @param element The HTML ID
    * @param func The callback function
    */

    ui.bind(win, "", events)
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_count

Get how many arguments there are in an event.

```odin
package main

import ui "webui"

callback :: proc "c" (e: ^ui.Event) {
    context = runtime.default_context()

    // JavaScript:
    // callback("Foo", "Bar", 123, true);

    count: uint = ui.get_count(e) // 4
}
```

Full Odin Example: https://github.com/webui-dev/odin-webui/blob/main/examples/call_odin_from_js.odin


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_int_at

Get an argument as integer at a specific index.

```odin
package main

import ui "webui"

callback :: proc "c" (e: ^ui.Event) {
    context = runtime.default_context()

    // JavaScript:
    // callback(12345, 6789);

    n1: i64 = ui.get_int_at(e, 0)
    n2: i64 = ui.get_int_at(e, 1)
}
```

Full Odin Example: https://github.com/webui-dev/odin-webui/blob/main/examples/call_odin_from_js.odin


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_int

Get the first argument as integer.

```odin
package main

import ui "webui"

callback :: proc "c" (e: ^ui.Event) {
    context = runtime.default_context()

    // JavaScript:
    // callback(123456);

    num: i64 = ui.get_int(e)
}
```

Full Odin Example: https://github.com/webui-dev/odin-webui/blob/main/examples/call_odin_from_js.odin


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_float_at

Get an argument as float at a specific index.

```odin
package main

import ui "webui"

callback :: proc "c" (e: ^ui.Event) {
    context = runtime.default_context()

    // JavaScript:
    // callback(12.34, 56.789);

    f1: f64 = ui.get_float_at(e, 0)
    f2: f64 = ui.get_float_at(e, 1)
}
```

Full Odin Example: https://github.com/webui-dev/odin-webui/blob/main/examples/call_odin_from_js.odin


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_float

Get the first argument as float.

```odin
package main

import ui "webui"

callback :: proc "c" (e: ^ui.Event) {
    context = runtime.default_context()

    // JavaScript:
    // callback(123.456);

    f: f64 = ui.get_float(e)
}
```

Full Odin Example: https://github.com/webui-dev/odin-webui/blob/main/examples/call_odin_from_js.odin


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_string_at

Get an argument as string at a specific index.

```odin
package main

import ui "webui"

callback :: proc "c" (e: ^ui.Event) {
    context = runtime.default_context()

    // JavaScript:
    // callback("Foo", "Bar");

    foo: cstring = ui.get_string_at(e, 0)
    bar: cstring = ui.get_string_at(e, 1)
}
```

Full Odin Example: https://github.com/webui-dev/odin-webui/blob/main/examples/call_odin_from_js.odin


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_string

Get the first argument as string.

```odin
package main

import ui "webui"

callback :: proc "c" (e: ^ui.Event) {
    context = runtime.default_context()

    // JavaScript:
    // callback("Foo Bar");

    name: cstring = ui.get_string(e)
}
```

Full Odin Example: https://github.com/webui-dev/odin-webui/blob/main/examples/call_odin_from_js.odin


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_bool_at

Get an argument as boolean at a specific index.

```odin
package main

import ui "webui"

callback :: proc "c" (e: ^ui.Event) {
    context = runtime.default_context()

    // JavaScript:
    // callback(true, false);

    status1: bool = ui.get_bool_at(e, 0)
    status2: bool = ui.get_bool_at(e, 1)
}
```

Full Odin Example: https://github.com/webui-dev/odin-webui/blob/main/examples/call_odin_from_js.odin


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_bool

Get the first argument as boolean.

```odin
package main

import ui "webui"

callback :: proc "c" (e: ^ui.Event) {
    context = runtime.default_context()

    // JavaScript:
    // callback(true);

    status: bool = ui.get_bool(e)
}
```

Full Odin Example: https://github.com/webui-dev/odin-webui/blob/main/examples/call_odin_from_js.odin


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_size_at

Get the size in bytes of an argument at a specific index.

```odin
package main

import ui "webui"

callback :: proc "c" (e: ^ui.Event) {
    context = runtime.default_context()

    // JavaScript:
    // callback("Foo", "Bar");

    fooLen: uint = ui.get_size_at(e, 0)
    barLen: uint = ui.get_size_at(e, 1)
}
```

Full Odin Example: https://github.com/webui-dev/odin-webui/blob/main/examples/call_odin_from_js.odin


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_size

Get size in bytes of the first argument.

```odin
package main

import ui "webui"

callback :: proc "c" (e: ^ui.Event) {
    context = runtime.default_context()

    // JavaScript:
    // callback("Foo");

    fooLen: uint = ui.get_size(e)
}
```

Full Odin Example: https://github.com/webui-dev/odin-webui/blob/main/examples/call_odin_from_js.odin


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_arg

_Odin-specific helper._ Parse a JavaScript argument as a native Odin type. Supports numeric types, `string`, `bool`, and any JSON-deserializable struct.

```odin
package main

import ui "webui"

callback :: proc "c" (e: ^ui.Event) {
    context = runtime.default_context()

    // JavaScript:
    // callback("hello", 42, true);

    /*
    * @param T     The Odin type to parse into
    * @param e     The event struct
    * @param idx   The argument index (default 0)
    */

    name, _ := ui.get_arg(string, e, 0)
    num,  _ := ui.get_arg(int,    e, 1)
    flag, _ := ui.get_arg(bool,   e, 2)
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### return_int

Return the response to JavaScript as integer.

```js
async function myJavaScript() {
    var num = Number(await callback());
    console.log(num);
}
```

```odin
package main

import ui "webui"

callback :: proc "c" (e: ^ui.Event) {
    context = runtime.default_context()

    // Return integer
    ui.return_int(e, 123456)
}
```

Full Odin Example: https://github.com/webui-dev/odin-webui/blob/main/examples/call_odin_from_js.odin


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### return_float

Return the response to JavaScript as float.

```js
async function myJavaScript() {
    var f = Number(await callback());
    console.log(f);
}
```

```odin
package main

import ui "webui"

callback :: proc "c" (e: ^ui.Event) {
    context = runtime.default_context()

    // Return float
    ui.return_float(e, 123.456)
}
```

Full Odin Example: https://github.com/webui-dev/odin-webui/blob/main/examples/call_odin_from_js.odin


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### return_string

Return the response to JavaScript as string.

```js
async function myJavaScript() {
    var name = await callback();
    console.log(name);
}
```

```odin
package main

import ui "webui"

callback :: proc "c" (e: ^ui.Event) {
    context = runtime.default_context()

    // Return string
    ui.return_string(e, "Foo Bar")
}
```

Full Odin Example: https://github.com/webui-dev/odin-webui/blob/main/examples/call_odin_from_js.odin


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### return_bool

Return the response to JavaScript as boolean.

```js
async function myJavaScript() {
    var status = Boolean(Number(await callback()));
    console.log(status);
}
```

```odin
package main

import ui "webui"

callback :: proc "c" (e: ^ui.Event) {
    context = runtime.default_context()

    // Return boolean
    ui.return_bool(e, true)
}
```

Full Odin Example: https://github.com/webui-dev/odin-webui/blob/main/examples/call_odin_from_js.odin


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### result

_Odin-specific helper._ Return a response to JavaScript from a callback. Automatically selects the right return type for `int`, `float`, `string`, and `bool`.

```js
async function myJavaScript() {
    var res = await callback();
    console.log(res);
}
```

```odin
package main

import ui "webui"

callback :: proc "c" (e: ^ui.Event) {
    context = runtime.default_context()

    /*
    * @param e    The event struct
    * @param resp The value to return (int, float, string, or bool)
    */

    ui.result(e, "Foo Bar")  // string response
    // ui.result(e, 123)     // int response
    // ui.result(e, true)    // bool response
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### run

Run JavaScript without waiting for the response. All connected clients.

```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param window The window number
    * @param script The JavaScript to be run
    */

    // Run javascript for all connected clients in a window
    ui.run(my_window, "alert('Foo Bar');")
}
```

Full Odin Example: https://github.com/webui-dev/odin-webui/blob/main/examples/call_js_from_odin.odin


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### run_client

Run JavaScript without waiting for the response. Single connected client only.

```odin
package main

import ui "webui"

callback :: proc "c" (e: ^ui.Event) {
    context = runtime.default_context()

    /*
    * @param e      The event struct
    * @param script The JavaScript to be run
    */

    ui.run_client(e, "alert('Hello!');")
}
```

Full Odin Example: https://github.com/webui-dev/odin-webui/blob/main/examples/call_js_from_odin.odin


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### script

Run JavaScript and get the response back. Works only in single client mode.

```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param window The window number
    * @param script The JavaScript to be run
    * @param timeout The execution timeout
    * @param buffer The local buffer to hold the response
    * @param buffer_length The local buffer size
    */

    response: cstring = ""
    if !ui.webui_script(my_window, "return 4 + 6;", 0, response, 64) {
        fmt.printfln("JavaScript Error: %s", response)
    } else {
        fmt.printfln("JavaScript Response: %s", response) // 10
    }
}
```

Full Odin Example: https://github.com/webui-dev/odin-webui/blob/main/examples/call_js_from_odin.odin


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### script_client

Run JavaScript and get the response back for a single connected client.

```odin
package main

import ui "webui"

callback :: proc "c" (e: ^ui.Event) {
    context = runtime.default_context()

    /*
    * @param e             The event struct
    * @param script        The JavaScript to be run
    * @param timeout       The execution timeout in seconds
    * @param buffer        The local buffer to hold the response
    * @param buffer_length The local buffer size
    */

    response: cstring = ""
    if !ui.script_client(e, "return 4 + 6;", 0, response, 64) {
        fmt.printfln("JavaScript Error: %s", response)
    } else {
        fmt.printfln("JavaScript Response: %s", response) // 10
    }
}
```

Full Odin Example: https://github.com/webui-dev/odin-webui/blob/main/examples/call_js_from_odin.odin


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_runtime

Choose between `Deno`, `Bun` and `Nodejs` as runtime for `.js` and `.ts` files.

```odin
package main

import ui "webui"

main :: proc() {

    /*
        WebuiJSRuntime :: enum {
            None,   // 0. No runtime
            Deno,   // 1. Deno runtime
            NodeJS, // 2. Node.js runtime
            Bun,    // 3. Bun runtime
        }
    */

    /*
    * @param window The window number
    * @param runtime None | Deno | NodeJS | Bun
    */

    ui.set_runtime(my_window, .Deno)

    // Now, any HTTP request to any '.js' or '.ts' file
    // will be interpreted by Deno.
    //
    // JavaScript:
    //
    // var xmlHttp = new XMLHttpRequest();
    // xmlHttp.open('GET', 'test.ts?foo=123&bar=456', false);
    // xmlHttp.send(null);
    //
    // console.log(xmlHttp.responseText);
}
```

Full Odin Example: https://github.com/webui-dev/odin-webui/tree/main/examples/serve_a_folder


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### send_raw

Send raw binary data to the UI. All connected clients.

```js
function myJavaScriptFunc(rawData) {
    // `rawData` is a Uint8Array variable
}
```

```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param window   The window number
    * @param function The JavaScript function to receive raw data
    * @param raw      The raw data buffer
    * @param size     The raw data size in bytes
    */

    buffer: []byte = { 0x01, 0x02, 0x03 }
    ui.send_raw(my_window, "myJavaScriptFunc", raw_data(buffer), len(buffer))
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### send_raw_client

Send raw binary data to a single connected client.

```js
function myJavaScriptFunc(rawData) {
    // `rawData` is a Uint8Array variable
}
```

```odin
package main

import ui "webui"

callback :: proc "c" (e: ^ui.Event) {
    context = runtime.default_context()

    /*
    * @param e        The event struct
    * @param function The JavaScript function to receive raw data
    * @param raw      The raw data buffer
    * @param size     The raw data size in bytes
    */

    buffer: []byte = { 0x01, 0x02, 0x03 }
    ui.send_raw_client(e, "myJavaScriptFunc", raw_data(buffer), len(buffer))
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### navigate

Navigate to a specific URL. All connected clients.

```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param window The window number
    * @param url Full HTTP URL
    */

    ui.navigate(win, "/foo/bar.html")

    // [!] Note:
    //
    // If 'bar.html' does not include 'webui.js' then
    // WebUI will try to close the window and 'wait()'
    // will break. It's important to include 'webui.js'
    // in every HTML page.
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### navigate_client

Navigate a single connected client to a specific URL.

```odin
package main

import ui "webui"

callback :: proc "c" (e: ^ui.Event) {
    context = runtime.default_context()

    /*
    * @param e   The event struct
    * @param url Full HTTP URL
    */

    ui.navigate_client(e, "/foo/bar.html")
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_url

Get the current URL of a running window.

> By default WebUI allows access to the window URL only from localhost.

```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param window The window number
    */

    url: cstring = ui.get_url(my_window)
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### open_url

Open a URL in the system's default web browser.

```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param url The URL to open
    */

    ui.open_url("https://webui.me")
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_public

Allow a specific window address (_URL_) to be accessible from any public network. By default WebUI allows access only from localhost.

```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param window The window number
    * @param status True or False
    */

    ui.set_public(my_window, true)

    // Now, the URL of the window is accessible
    // from any device/mobile in the network.
}
```

Full Odin Example: https://github.com/webui-dev/odin-webui/tree/main/examples/public_network_access


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_root_folder

Set the web-server root folder path for a specific window.

> Should be used before `show()`.

```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param window The window number
    * @param path The local folder full path
    */

    ui.set_root_folder(my_window, "/home/Foo/Bar/")
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_default_root_folder

Set the web-server root folder path for all windows.

> Should be used before `show()`.

```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param path The local folder full path
    */

    ui.set_default_root_folder("/home/Foo/Bar/")
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_browser_folder

Set a custom folder path where WebUI should look for the web browser executable.

```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param path The browser folder path
    */

    ui.set_browser_folder("/home/Foo/browsers/")
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_file_handler

Set a custom handler to serve files. The handler should return a full HTTP response (headers + body). This deactivates any previous handler set with `set_file_handler_window()`.

```odin
package main

import ui "webui"

filesHandler :: proc "c" (filename: cstring, length: ^c.int) -> rawptr {
    context = runtime.default_context()

    // Return an HTTP response for the requested filename.
    // Set `length^` to the byte count of the response.
    return nil
}

main :: proc() {

    /*
    * @param window  The window number
    * @param handler The handler function
    */

    ui.set_file_handler(my_window, filesHandler)
}
```

Full Odin Example: https://github.com/webui-dev/odin-webui/tree/main/examples/serve_a_folder


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_file_handler_window

Set a custom handler to serve files with access to the window number. The handler should return a full HTTP response (headers + body). This deactivates any previous handler set with `set_file_handler()`.

```odin
package main

import ui "webui"

filesHandler :: proc "c" (window: c.size_t, filename: cstring, length: ^c.int) -> rawptr {
    context = runtime.default_context()

    // `window` identifies which window made the request.
    // Set `length^` to the byte count of the response.
    return nil
}

main :: proc() {

    /*
    * @param window  The window number
    * @param handler The handler function
    */

    ui.set_file_handler_window(my_window, filesHandler)
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### interface_set_response_file_handler

Provide an async response for `set_file_handler()`. Use this when your file handler needs to perform async work and cannot return a response immediately.

```odin
package main

import ui "webui"

filesHandler :: proc "c" (filename: cstring, length: ^c.int) -> rawptr {
    context = runtime.default_context()

    // Signal that the response will arrive asynchronously
    length^ = 0
    return nil
}

asyncResponder :: proc(window: uint, response: []byte) {
    ui.interface_set_response_file_handler(window, raw_data(response), len(response))
}

main :: proc() {

    ui.set_file_handler(my_window, filesHandler)
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_close_handler_wv

Set a callback to intercept the WebView window close event. Return `false` to prevent the window from closing, `true` to allow it.

```odin
package main

import ui "webui"
import "core:c"

closeHandler :: proc "c" (window: c.size_t) -> c.bool {
    context = runtime.default_context()

    // Perform cleanup or show a confirmation dialog.
    // Return false to block closing, true to allow it.
    return true
}

main :: proc() {

    /*
    * @param window        The window number
    * @param close_handler The close event callback
    */

    ui.set_close_handler_wv(my_window, closeHandler)
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_best_browser

Get the recommended web browser ID to use. If you are already using one, this function will return the same ID.

```odin
package main

import ui "webui"

main :: proc() {

    /*
        Browser :: enum {
            NoBrowser,      // 0. No web browser
            AnyBrowser,     // 1. Default recommended web browser
            Chrome,         // 2. Google Chrome
            Firefox,        // 3. Mozilla Firefox
            Edge,           // 4. Microsoft Edge
            Safari,         // 5. Apple Safari
            Chromium,       // 6. The Chromium Project
            Opera,          // 7. Opera Browser
            Brave,          // 8. The Brave Browser
            Vivaldi,        // 9. The Vivaldi Browser
            Epic,           // 10. The Epic Browser
            Yandex,         // 11. The Yandex Browser
            ChromiumBased,  // 12. Any Chromium based browser
            Webview,        // 13. WebView (Non-web-browser)
        }
    */

    /*
    * @param window The window number
    */

    browserID: uint = ui.get_best_browser(my_window)
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### browser_exist

Check if a specific web browser is installed on the system.

```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param browser The browser to check
    */

    if ui.browser_exist(.Chrome) {
        ui.show_browser(my_window, "index.html", .Chrome)
    } else {
        ui.show(my_window, "index.html")
    }
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_profile

Set the web browser profile to use. An empty `name` and `path` means the default user profile.

> Need to be called before `show()`.

```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param window The window number
    * @param name The web browser profile name
    * @param path The web browser profile full path
    */

    ui.set_profile(my_window, "Bar", "/Home/Foo/Bar")
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_proxy

Set the web browser proxy server to use.

> Need to be called before `show()`.

```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param window The window number
    * @param proxy_server The web browser proxy_server
    */

    ui.set_proxy(my_window, "http://127.0.0.1:8888")
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### delete_profile

Delete the web-browser local folder profile for a specific window.

> It's recommended to call this when the program exits, after all windows are closed.

> [!] This can break functionality of other running windows if they share the same web-browser profile.

```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param window The window number
    */

    ui.wait()
    ui.delete_profile(my_window)
    ui.clean()
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### delete_all_profiles

Delete all local web-browser profile folders.

> It's recommended to call this when the program exits, after all windows are closed.

> [!] This can break functionality of running windows if using the same web-browser profile.

```odin
package main

import ui "webui"

main :: proc() {

    ui.wait()
    ui.delete_all_profiles()
    ui.clean()
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_port

Get the network port of a running window. This can be useful to determine the HTTP link of `webui.js`.

```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param window The window number
    */

    port: uint = ui.get_port(my_window)
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_free_port

Get an available usable free network port.

```odin
package main

import ui "webui"

main :: proc() {

    port: uint = ui.get_free_port()
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_port

Set a custom web-server network port to be used by WebUI. This can be useful to determine the HTTP link of `webui.js` in case you are trying to use WebUI with an external web-server like NGNIX.

```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param window The window number
    * @param port The web-server network port WebUI should use
    */

    ret: bool = ui.set_port(my_window, 8080)

    if !ret {
        fmt.printfln("The port is busy and not usable.")
    }

    // The port '8080' will not be used by WebUI
    // until we show the window. The window URL
    // then will be: http://localhost:8080/
}
```

Full Odin Example: https://github.com/webui-dev/odin-webui/tree/main/examples/custom_web_server


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_config

Control and change the WebUI global settings.

> It's recommended to be called at the beginning.

```odin
package main

import ui "webui"

main :: proc() {

    /*
        Config :: enum {
        // Control if 'show()', 'show_browser()' and 'show_wv()' should
        // wait for the window to connect before returning or not.
        // Default: True
        show_wait_connection,
        // Control if WebUI should process UI events one at a time in a
        // single blocking thread (True), or in a new non-blocking thread
        // per event (False). Affects all windows.
        // Default: False
        ui_event_blocking,
        // Automatically refresh the window UI when any file in the root
        // folder changes.
        // Default: False
        folder_monitor,
        // Allow multiple clients to connect to the same window.
        // Helpful for web apps (non-desktop software).
        // Default: False
        multi_client,
        // Allow or prevent WebUI from adding 'webui_auth' cookies.
        // Keep True for single-client windows.
        // Default: True
        use_cookies,
        // If the backend uses asynchronous operations, set to True.
        // WebUI will wait until the backend calls a 'return_x()' API.
        asynchronous_response,
    */

    /*
    * @param option The desired option from 'Config' enum
    * @param status The status of the option, 'true' or 'false'
    */

    ui.set_config(.show_wait_connection, true)
    ui.set_config(.ui_event_blocking, false)
    ui.set_config(.folder_monitor, true)
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_event_blocking

Control if UI events from this window should be processed one at a time in a single blocking thread `true`, or each in a new non-blocking thread `false`. This setting applies to a single window. Use `set_config()` to update all windows.

> Note: If set to `true`, the API `script()` will not return until the current event handler finishes.

```odin
package main

import ui "webui"

foo :: proc "c" (e: ^ui.Event) {
    context = runtime.default_context()
    //
}

bar :: proc "c" (e: ^ui.Event) {
    context = runtime.default_context()
    //
}

main :: proc() {

    ui.bind(my_window, "foo", foo)
    ui.bind(my_window, "bar", bar)

    /*
    * @param window The window number
    * @param status The blocking status 'true' or 'false'
    */

    ui.set_event_blocking(my_window, true)

    // Now, every UI event will be processed
    // in one single thread; other UI events
    // will be blocked until the first event ends.

    // JavaScript:
    // foo();
    // bar();
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_tls_certificate

Set the SSL/TLS certificate and the private key content in PEM format. If set empty WebUI will generate a self-signed certificate.

> This works only with the TLS version of WebUI `webui-2-secure`.

```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param certificate_pem The SSL/TLS certificate content in PEM format
    * @param private_key_pem The private key content in PEM format
    */

    ret: bool = ui.set_tls_certificate(
        "-----BEGIN CERTIFICATE-----\n...",
        "-----BEGIN PRIVATE KEY-----\n..."
    )

    if !ret {
        fmt.printfln("Invalid TLS certificate.")
    }
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_logger

Set a custom logger function to receive WebUI internal log messages. Useful for debugging.

```odin
package main

import ui "webui"
import "core:c"

myLogger :: proc "c" (level: c.size_t, log: cstring, user_data: rawptr) {
    context = runtime.default_context()

    /*
        WEBUI_LOGGER_LEVEL_DEBUG = 0  // All logs with details
        WEBUI_LOGGER_LEVEL_INFO  = 1  // General logs only
        WEBUI_LOGGER_LEVEL_ERROR = 2  // Fatal error logs only
    */

    fmt.printfln("WebUI [%d]: %s", level, log)
}

main :: proc() {

    /*
    * @param func      The logger callback
    * @param user_data User data pointer passed to every call
    */

    ui.set_logger(myLogger, nil)
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_parent_process_id

Get the ID of the parent process (the current backend application process).

```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param window The window number
    */

    parent_pid: uint = ui.get_parent_process_id(my_window)
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_child_process_id

Get the ID of the child process created by WebUI, which refers to the web browser window.

> In WebView mode, this returns the parent process ID because the backend and the WebView window share the same process.

```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param window The window number
    */

    child_pid: uint = ui.get_child_process_id(my_window)
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### win32_get_hwnd

Get the Win32 `HWND` handle of a window. More reliable with WebView than with web browsers, as browser process IDs may change on launch.

> Windows only.

```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param window The window number
    */

    hwnd := ui.win32_get_hwnd(my_window)
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_hwnd

Get the native window handle. Returns `HWND` on Windows, `GtkWindow*` on Linux.

> On Linux, this works only with WebView windows.

```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param window The window number
    */

    handle := ui.get_hwnd(my_window)
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### encode

Encode text to Base64. The returned buffer must be freed with `free()`.

```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param str The string to encode
    */

    base64: cstring = ui.encode("Foo Bar")

    // Later
    ui.free(base64)
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### decode

Decode a Base64 encoded text. The returned buffer must be freed with `free()`.

```odin
package main

import ui "webui"

callback :: proc "c" (e: ^ui.Event) {
    context = runtime.default_context()

    // JavaScript:
    // callback(Base64)
    base64: cstring = ui.get_string(e)

    /*
    * @param str The string to decode
    */

    str: cstring = ui.decode(base64)

    // Later
    ui.free(str)
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### malloc

Safely allocate memory using the WebUI memory management system. The buffer can be freed at any time using `free()`.

```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param size The size of memory in bytes
    */

    myBuffer := cast(^u8)ui.malloc(1024)

    // Later
    ui.free(myBuffer)
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### memcpy

Copy raw data from one buffer to another.

```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param dest  Destination memory pointer
    * @param src   Source memory pointer
    * @param count Bytes to copy
    */

    dest := cast(^u8)ui.malloc(64)
    ui.memcpy(dest, src, 64)

    // Later
    ui.free(dest)
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### free

Safely free a buffer that was allocated by WebUI using `malloc()`.

```odin
package main

import ui "webui"

main :: proc() {
    myBuffer := cast(^u8)ui.malloc(1024)

    /*
    * @param ptr The buffer to be freed
    */

    ui.free(myBuffer)
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_mime_type

Get the HTTP MIME type of a file based on its extension.

```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param file The filename or path
    */

    mime: cstring = ui.get_mime_type("foo.png")   // "image/png"
    mime2: cstring = ui.get_mime_type("app.js")   // "application/javascript"
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_last_error_number

Get the last WebUI error code.

```odin
package main

import ui "webui"

main :: proc() {

    err_num: uint = ui.get_last_error_number()

    if err_num != 0 {
        fmt.printfln("WebUI error code: %d", err_num)
    }
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_last_error_message

Get the last WebUI error message.

```odin
package main

import ui "webui"

main :: proc() {

    err_msg: cstring = ui.get_last_error_message()
    fmt.printfln("WebUI error: %s", err_msg)
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### clean

Free all memory resources used by WebUI. Should be called only at the end of the program, after all windows are closed.

```odin
package main

import ui "webui"

main :: proc() {

    ui.wait()
    ui.clean()
}
```
