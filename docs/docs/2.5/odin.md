<div align="center">

![Logo](data/webui_120.svg)

# WebUI v2.5 - Odin Documentation
***[ ! ] Beta 3 - Not Complete Yet***

> Use any web browser or WebView as GUI, with your preferred language in the backend and modern web technologies in the frontend, all in a lightweight portable library.

![Screenshot](data/screenshot.png)

</div>


## Available APIs

- [Download And Install](#download-and-install)
- [Minimal Example](#minimal-example)
- [new_window](#new_window)
- [new_window_id](#new_window_id)
- [get_new_window_id](#get_new_window_id)
- [bind](#bind)
- [event](#event)
- [get_best_browser](#get_best_browser)
- [show](#show)
- [show_browser](#show_browser)
- [show_wv](#show_wv)
- [set_kiosk](#set_kiosk)
- [wait](#wait)
- [close](#close)
- [destroy](#destroy)
- [exit](#exit)
- [set_root_folder](#set_root_folder)
- [set_default_root_folder](#set_default_root_folder)
- [set_file_handler](#set_file_handler)
- [is_shown](#is_shown)
- [set_timeout](#set_timeout)
- [set_icon](#set_icon)
- [encode](#encode)
- [decode](#decode)
- [free](#free)
- [malloc](#malloc)
- [send_raw](#send_raw)
- [set_hide](#set_hide)
- [set_size](#set_size)
- [set_position](#set_position)
- [set_profile](#set_profile)
- [set_proxy](#set_proxy)
- [get_url](#get_url)
- [set_public](#set_public)
- [navigate](#navigate)
- [clean](#clean)
- [delete_all_profiles](#delete_all_profiles)
- [delete_profile](#delete_profile)
- [get_parent_process_id](#get_parent_process_id)
- [get_child_process_id](#get_child_process_id)
- [set_port](#set_port)
- [set_config](#set_config)
- [set_event_blocking](#set_event_blocking)
- [set_tls_certificate](#set_tls_certificate)
- [run](#run)
- [script](#script)
- [set_runtime](#set_runtime)
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
- [return_int](#return_int)
- [return_float](#return_float)
- [return_string](#return_string)
- [return_bool](#return_bool)
- [open_url](#open_url)
- [start_server](#start_server)
- [get_mime_type](#get_mime_type)
- [get_port](#get_port)
- [get_free_port](#get_free_port)
- [JavaScript APIs](javascript.md)


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

Minimal example

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
### bind

Bind an HTML element event with a function. Empty element means all events.

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
### event

Every event comes with information about the current event, like id, name, type (_click, connect, disconnect..._) and more.

```odin
package main

import ui "webui"

/*
    Event :: struct {
        window: c.size_t,
        event_type: EventType,
        element: cstring,
        event_number: c.size_t,
        bind_id: c.size_t,
        client_id: c.size_t,
        connection_id: c.size_t,
        cookies: cstring,
    }

    EventType :: enum {
        Disconnected,         // 0. Window disconnection event
        Connected,            // 1. Window connection event
        MouseClick,          // 2. Mouse click event
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
### close

Close a specific window.

> The `win` object will still alive, and it can be reopen later.

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

main :: proc() {
    /*
    * @param window The window number
    */

    ui.close(win)
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### destroy

Close a specific window and free all related memory resources.

> This will make the `win` object not available. If you want to simply close a window and reopen it later, please use `close()` instead.

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
### set_file_handler

Set a custom handler to serve files.

```odin
package main

import ui "webui"

filesHandler :: proc "c" (filename: cstring, length: i32) -> rawptr {
    context = runtime.default_context()

    // code
    return rawptr
}

main :: proc() {

    /*
    * @param window The window number
    * @param handler The handler function
    */

    ui.set_file_handler(my_window, filesHandler)
}
```

Full Odin Example: https://github.com/webui-dev/odin-webui/tree/main/examples/serve_a_folder


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
### set_timeout

Set the maximum time in seconds to wait for the window to connect. This effect `show()` and `wait()`.

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
### encode

Encode text to Base64.

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

Decode a Base64 encoded text.

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
### free

Safely free a buffer allocated by WebUI.

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
### malloc

Safely allocate memory using the WebUI memory management system. It can be safely freed using WebUI's _free_ API at any time later.

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
### send_raw

Send raw data (_binary_) to the UI.

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

    ui.send_raw_client(e, "myJavaScriptFunc", raw_data(buffer), 3)
}

main :: proc() {

    /*
    * @param window The window number
    * @param function The JavaScript function to receive raw data
    * @param raw The raw data buffer
    * @param size The raw data size in bytes
    */

    buffer: []byte = { 0x01, 0x02, 0x03 } // Any data type
    ui.send_raw(my_window, "myJavaScriptFunc", raw_data(buffer), len(buffer))
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
### set_position

Set the window position.

```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param window The window number
    * @param x The window X
    * @param y The window Y
    */

    ui.set_position(my_window, 100, 100)
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
### get_url

Get current URL of a running window.

> By default WebUI allow access to the URL of a window only from localhost.

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
### set_public

Allow a specific window address (_URL_) to be accessible from any public network. By default WebUI allow access to the URL of a window only from localhost.

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
### navigate

Navigate to a specific URL.

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
    // in every HTML.
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### clean

Free all memory resources. Should be called only at the end.

```odin
package main

import ui "webui"

main :: proc() {

    ui.clean()
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### delete_all_profiles

Delete a specific window web-browser local folder profile.

> It's recommended to be called when program exit, and after all windows are closed.

> [!] This can break functionality of running windows if using the same web-browser profile.

```odin
package main

import ui "webui"

main :: proc() {

    ui.delete_all_profiles()
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### delete_profile

Delete a specific window web-browser local folder profile.

> It's recommended to be called when program exit, and after all windows are closed.

> [!] This can break functionality of other running windows if using the same web-browser profile.

```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param window The window number
    */

    ui.delete_profile(my_window)
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_parent_process_id

Get the ID of the parent process (_The web browser may re-create another new process_).

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

Get the ID of the last child process (_The web browser may re-create other child process_).

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
        // Control if 'webui_show()', 'webui_show_browser()' and
        // 'webui_show_wv()' should wait for the window to connect
        // before returns or not.
        //
        // Default: True
        show_wait_connection,
        // Control if WebUI should block and process the UI events
        // one a time in a single thread 'True', or process every
        // event in a new non-blocking thread 'False'. This updates
        // all windows. You can use 'webui_set_event_blocking()' for
        // a specific single window update.
        //
        // Default: False
        ui_event_blocking,
        // Automatically refresh the window UI when any file in the
        // root folder gets changed.
        //
        // Default: False
        folder_monitor,
        // Allow multiple clients to connect to the same window,
        // This is helpful for web apps (non-desktop software),
        // Please see the documentation for more details.
        //
        // Default: False
        multi_client,
        // Allow or prevent WebUI from adding 'webui_auth' cookies.
        // WebUI uses these cookies to identify clients and block
        // unauthorized access to the window content using a URL.
        // Please keep this option to 'True' if you want only a single
        // client to access the window content.
        //
        // Default: True
        use_cookies,
        // If the backend uses asynchronous operations, set this
        // option to 'True'. This will make webui wait until the
        // backend sets a response using 'webui_return_x()'.
        asynchronous_response
    */

    /*
    * @param option The desired option from 'webui_config' enum
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

Control if UI events comming from this window should be processed one a time in a single blocking thread `True`, or process every event in a new non-blocking thread `False`. This update single window. You can use `set_config()` to update all windows.

> Note: If this is set to `True`, the API `script()` won't return any response until this current event is finished.

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
    // in one single thread, other UI events
    // will be blocked until first event end

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
### run

Run JavaScript without waiting for the response.

```odin
package main

import ui "webui"

callback :: proc "c" (e: ^ui.Event) {

    /*
    * @param e The event struct
    * @param script The JavaScript to be run
    */

    // Run javascript for one specific client

    ui.run_client(e, "alert('Foo Bar');")
}

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
### script

Run JavaScript and get the response back.

```odin
package main

import ui "webui"

callback :: proc "c" (e: ^ui.Event) {
    context = runtime.default_context()

    /*
    * @param e The event struct
    * @param script The JavaScript to be run
    * @param timeout The execution timeout
    * @param buffer The local buffer to hold the response
    * @param buffer_length The local buffer size
    */

    // Run javascript for one specific client

    response: cstring = ""
    if !ui.script_client(e, "return 4 + 6;", 0, response, 64) {
        fmt.printfln("JavaScript Error: %s", response)
    }
    else {
        fmt.printfln("JavaScript Response: %s", response) // 10
    }
}

main :: proc() {

    /*
    * @param window The window number
    * @param script The JavaScript to be run
    * @param timeout The execution timeout
    * @param buffer The local buffer to hold the response
    * @param buffer_length The local buffer size
    */

    // Run javascript

    response: cstring = ""
    if !ui.webui_script(my_window, "return 4 + 6;", 0, response, 64) {
        fmt.printfln("JavaScript Error: %s", response)
    }
    else {
        fmt.printfln("JavaScript Response: %s", response) // 10
    }
}
```

Full Odin Example: https://github.com/webui-dev/odin-webui/blob/main/examples/call_js_from_odin.odin


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_runtime

Chose between `Deno`, `Bun` and `Nodejs` as runtime for `.js` and `.ts` files.

```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param window The window number
    * @param runtime None | Deno | Nodejs | Bun
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
### open_url

Open an URL in the native default web browser.

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
### start_server

Start only the web server and return the URL. This is useful for web app. No window will be shown.

```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param window The window number
    * @param content The HTML, Or a local file
    */

    url: cstring = ui.start_server(myWindow, "/full/root/path")
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_mime_type

Get the HTTP mime type of a file.

```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param file The path to and or, with the file included
    */

    mime: cstring = ui.get_mime_type("foo.png")
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
