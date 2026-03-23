<div align="center">

![Logo](data/webui_120.svg)

# WebUI v2.5 - Go Documentation

> Use any web browser or WebView as GUI, with your preferred language in the backend and modern web technologies in the frontend, all in a lightweight portable library.

![Screenshot](data/screenshot.png)

</div>


## Getting Started
- [Download And Install](#download-and-install)
- [Minimal Example](#minimal-example)

## Available APIs
- **Window**
  - [new_window](#new_window)
  - [show](#show)
  - [show_browser](#show_browser)
  - [show_webview](#show_webview)
  - [show_client](#show_client)
  - [start_server](#start_server)
  - [close](#close)
  - [destroy](#destroy)
  - [exit](#exit)
  - [wait](#wait)
  - [wait_async](#wait_async)
  - [is_shown](#is_shown)
  - [focus](#focus)
  - [minimize](#minimize)
  - [maximize](#maximize)
- **Binding & Events**
  - [bind](#bind)
  - [event](#event)
  - [set_context](#set_context)
  - [get_context](#get_context)
- **Arguments**
  - [get_arg](#get_arg)
  - [get_arg_at](#get_arg_at)
  - [get_count](#get_count)
  - [get_size](#get_size)
- **Return Values**
  - [return_int](#return_int)
  - [return_float](#return_float)
  - [return_string](#return_string)
  - [return_bool](#return_bool)
- **JavaScript**
  - [run](#run)
  - [run_client](#run_client)
  - [script](#script)
  - [script_client](#script_client)
  - [set_runtime](#set_runtime)
- **Window Configuration**
  - [set_size](#set_size)
  - [set_minimum_size](#set_minimum_size)
  - [set_position](#set_position)
  - [set_center](#set_center)
  - [set_kiosk](#set_kiosk)
  - [set_hide](#set_hide)
  - [set_frameless](#set_frameless)
  - [set_transparent](#set_transparent)
  - [set_resizable](#set_resizable)
  - [set_high_contrast](#set_high_contrast)
  - [set_icon](#set_icon)
  - [set_close_handler_webview](#set_close_handler_webview)
  - [set_event_blocking](#set_event_blocking)
  - [set_timeout](#set_timeout)
  - [set_config](#set_config)
- **Navigation & Network**
  - [navigate](#navigate)
  - [navigate_client](#navigate_client)
  - [get_url](#get_url)
  - [get_port](#get_port)
  - [set_port](#set_port)
  - [get_free_port](#get_free_port)
  - [set_public](#set_public)
  - [open_url](#open_url)
- **Browser**
  - [get_best_browser](#get_best_browser)
  - [browser_exists](#browser_exists)
  - [set_profile](#set_profile)
  - [set_proxy](#set_proxy)
  - [set_browser_folder](#set_browser_folder)
  - [set_custom_parameters](#set_custom_parameters)
  - [delete_profile](#delete_profile)
  - [delete_all_profiles](#delete_all_profiles)
- **File Serving**
  - [set_root_folder](#set_root_folder)
  - [set_default_root_folder](#set_default_root_folder)
  - [set_file_handler](#set_file_handler)
  - [set_file_handler_window](#set_file_handler_window)
  - [set_response_file_handler](#set_response_file_handler)
- **Raw Data**
  - [send_raw](#send_raw)
  - [send_raw_client](#send_raw_client)
- **Multi-Client**
  - [close_client](#close_client)
- **Process & System**
  - [get_parent_process_id](#get_parent_process_id)
  - [get_child_process_id](#get_child_process_id)
  - [get_hwnd](#get_hwnd)
- **Memory & Utilities**
  - [encode](#encode)
  - [decode](#decode)
  - [get_mime_type](#get_mime_type)
  - [malloc](#malloc)
  - [free](#free)
  - [memcpy](#memcpy)
- **TLS/Security**
  - [set_tls_certificate](#set_tls_certificate)
- **Logging & Errors**
  - [set_logger](#set_logger)
  - [get_last_error_number](#get_last_error_number)
  - [get_last_error_message](#get_last_error_message)
- **Cleanup**
  - [clean](#clean)

[JavaScript APIs](javascript.md)


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### Download And Install

The WebUI library is written in pure C, but there is a Go wrapper that provides easy-to-use APIs for the backend, while HTML5 and JavaScript are used in the frontend inside a web browser or WebView window.

```sh
go get github.com/webui-dev/go-webui/v2@latest
```

Then run the setup tool once to fetch the WebUI C library:

```sh
go run github.com/webui-dev/go-webui/v2/sync-webui@main
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### Minimal Example

```go
package main

import "github.com/webui-dev/go-webui/v2"

func main() {
    win := webui.NewWindow()
    win.Show("<html><script src=\"webui.js\"></script><body>Hello from Go!</body></html>")
    webui.Wait()
}
```

[More Go Examples](https://github.com/webui-dev/go-webui/tree/main/examples).


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### new_window

Create a new window object. You can also create a window with a specific ID using `NewWindowId()`, or reserve an ID first and create the window later.

```go
// Create a new window with an auto-assigned ID
win := webui.NewWindow()

// Reserve a free window ID (useful when you need the ID before creation)
id := webui.NewWindowId()

// Create a window using a specific reserved ID
id.NewWindow()
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### show

Show a window using embedded HTML, a local file, a local folder, or a URL. If the window is already open, it will be refreshed. This affects all clients in multi-client mode.

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

> To use only WebView please use `show_webview()`

```go
win.Show("<html><script src=\"webui.js\"></script><body>Hello!</body></html>")
win.Show("index.html")
win.Show("https://mydomain.com")

// Start in folder mode (serves root folder with index fallback)
win.Show("")
```

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### show_browser

Show a window using a specific web browser. Returns an error if the browser is not available.

> On macOS, browser icons may linger in the Dock after exit. Consider using [`show_webview`](#show_webview) on macOS to avoid this.

```go
// Available browser constants:
// webui.AnyBrowser, webui.Chrome, webui.Firefox, webui.Edge,
// webui.Safari, webui.Chromium, webui.Opera, webui.Brave,
// webui.Vivaldi, webui.Epic, webui.Yandex, webui.ChromiumBased, webui.Webview

if err := win.ShowBrowser("index.html", webui.Chrome); err != nil {
    log.Println(err)
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### show_webview

Show a window using the platform's native WebView (WKWebView on macOS, WebKit GTK on Linux, WebView2 on Windows). Returns an error if WebView is not available.

> On Windows, `WebView2Loader.dll` must be present.

```go
if err := win.ShowWebView("index.html"); err != nil {
    log.Println(err)
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### show_client

Show or refresh the window for a specific connected client only. Called from within a bound event handler.

```go
webui.Bind[webui.Void](win, "myFunc", func(e webui.Event) webui.Void {
    e.ShowClient("new_page.html")
    return nil
})
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### start_server

Start the local web server and return its URL without showing any browser window. Useful when integrating WebUI's HTTP server with an external browser or tool.

```go
url := win.StartServer("index.html")
fmt.Println("Server running at:", url)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### close

Close a specific window. The window object remains valid and the window can be shown again later.

```go
win.Close()
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### destroy

Close a specific window and free all of its memory resources. Use this when you are done with the window entirely.

```go
win.Destroy()
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### exit

Close all open windows. `Wait()` will return after this call.

```go
webui.Exit()
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### wait

Block and wait until all opened windows are closed. Call this at the end of `main()`.

```go
// Set up windows, bindings, and show...

webui.Wait()
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### wait_async

A non-blocking alternative to `Wait()` for use when you need to run code on the main thread between UI events (required in WebView mode). Call it in a loop; it returns `false` when all windows are closed.

```go
for webui.WaitAsync() {
    // Your main thread work here (e.g. update state, process messages)
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### is_shown

Check if the specified window is still running.

```go
if win.IsShown() {
    fmt.Println("Window is open")
} else {
    fmt.Println("Window is closed")
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### focus

Bring a window to the front and give it focus.

```go
win.Focus()
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### minimize

Minimize a WebView window.

```go
win.Minimize()
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### maximize

Maximize a WebView window.

```go
win.Maximize()
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### bind

Bind an HTML element (by ID) or a JavaScript function name to a Go callback. When the element is clicked or the JS function is called, the callback runs. Use an empty string to catch all events on all elements.

Use the generic `webui.Bind[T]` form to return a value back to JavaScript. Use `webui.Void` as the type when no return value is needed.

```go
// Return a string to JavaScript
webui.Bind[string](win, "myButton", func(e webui.Event) string {
    return "Hello from Go!"
})

// No return value
webui.Bind[webui.Void](win, "myButton", func(e webui.Event) webui.Void {
    fmt.Println("Button clicked!")
    return nil
})

// Catch all window events (connect, disconnect, navigate, click...)
webui.Bind[webui.Void](win, "", func(e webui.Event) webui.Void {
    fmt.Printf("Event type %d on element: %s\n", e.EventType, e.Element)
    return nil
})
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### event

Every callback receives an `Event` struct with information about the current event.

```go
type Event struct {
    Window       Window    // The window that triggered this event
    EventType    EventType // Disconnected, Connected, MouseClick, Navigation, Callback
    Element      string    // The HTML element ID or JS function name
    ClientId     uint      // Unique ID of the connected client
    ConnectionId uint      // Client's connection ID
    Cookies      string    // Client's full cookie string
}
```

Event type constants:

```go
webui.Disconnected // 0 - client disconnected
webui.Connected    // 1 - client connected
webui.MouseClick   // 2 - mouse click
webui.Navigation   // 3 - page navigation
webui.Callback     // 4 - function call from JS
```

Example:

```go
webui.Bind[webui.Void](win, "", func(e webui.Event) webui.Void {
    switch e.EventType {
    case webui.Connected:
        fmt.Printf("Client %d connected\n", e.ClientId)
    case webui.Disconnected:
        fmt.Printf("Client %d disconnected\n", e.ClientId)
    case webui.MouseClick:
        fmt.Printf("Click on element: %s\n", e.Element)
    case webui.Navigation:
        fmt.Printf("Navigating to: %s\n", e.Element)
    }
    return nil
})
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_context

Attach arbitrary user data to a binding so it can be retrieved later inside the callback via `GetContext()`. Must be called after `Bind()`.

```go
type MyData struct{ UserID int }
data := &MyData{UserID: 42}

webui.Bind[webui.Void](win, "myFunc", func(e webui.Event) webui.Void {
    ctx := (*MyData)(e.GetContext())
    fmt.Println("UserID:", ctx.UserID)
    return nil
})

win.SetContext("myFunc", unsafe.Pointer(data))
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_context

Retrieve user data previously attached with `SetContext()`. Called from within the event callback.

```go
ctx := e.GetContext() // returns unsafe.Pointer
```

See [set_context](#set_context) for a full example.


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_arg

Parse the first JavaScript argument into a Go type. Supports `string`, `int`, `float64`, `bool`, and any JSON-decodable struct.

```go
webui.Bind[string](win, "greet", func(e webui.Event) string {
    name, err := webui.GetArg[string](e)
    if err != nil {
        return "error: " + err.Error()
    }
    return "Hello, " + name + "!"
})
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_arg_at

Parse the JavaScript argument at a specific index (zero-based) into a Go type.

```go
webui.Bind[webui.Void](win, "add", func(e webui.Event) webui.Void {
    a, _ := webui.GetArgAt[int](e, 0)
    b, _ := webui.GetArgAt[int](e, 1)
    fmt.Printf("%d + %d = %d\n", a, b, a+b)
    return nil
})
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_count

Get the number of arguments passed from JavaScript in this event.

```go
webui.Bind[webui.Void](win, "myFunc", func(e webui.Event) webui.Void {
    count := e.GetCount()
    fmt.Printf("Received %d argument(s)\n", count)
    return nil
})
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_size

Get the size in bytes of a JavaScript argument. Useful when receiving raw binary data.

```go
// Size of the first argument
size := e.GetSize()

// Size of the argument at a specific index
size := e.GetSizeAt(2)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### return_int

Return an integer response to the JavaScript caller. Use this for manual response control instead of returning a value from the `Bind` callback.

```go
webui.Bind[webui.Void](win, "getCount", func(e webui.Event) webui.Void {
    e.ReturnInt(42)
    return nil
})
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### return_float

Return a float response to the JavaScript caller.

```go
webui.Bind[webui.Void](win, "getPi", func(e webui.Event) webui.Void {
    e.ReturnFloat(3.14159)
    return nil
})
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### return_string

Return a string response to the JavaScript caller.

```go
webui.Bind[webui.Void](win, "greet", func(e webui.Event) webui.Void {
    e.ReturnString("Hello from Go!")
    return nil
})
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### return_bool

Return a boolean response to the JavaScript caller.

```go
webui.Bind[webui.Void](win, "isReady", func(e webui.Event) webui.Void {
    e.ReturnBool(true)
    return nil
})
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### run

Execute JavaScript in the window without waiting for a response. This is fire-and-forget and targets all connected clients.

```go
win.Run("document.getElementById('status').innerText = 'Updated!';")
win.Run("alert('Hello from Go!')")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### run_client

Execute JavaScript without waiting for a response, targeting only the specific client that triggered the event.

```go
webui.Bind[webui.Void](win, "myFunc", func(e webui.Event) webui.Void {
    e.RunClient("alert('Hello, just you!')")
    return nil
})
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### script

Execute JavaScript and wait for the response. Works only in single-client mode. Use `ScriptOptions` to set a timeout (seconds, `0` = no timeout) and response buffer size (bytes, `0` = 8 KiB default).

```go
resp, err := win.Script("return 2 * 21;", webui.ScriptOptions{})
if err != nil {
    fmt.Println("Script error:", err)
} else {
    fmt.Println("Result:", resp) // "42"
}

// With a timeout and larger buffer
resp, err = win.Script(
    "return document.title;",
    webui.ScriptOptions{Timeout: 5, BufferSize: 1024 * 16},
)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### script_client

Execute JavaScript and wait for the response from a specific client. Called from within an event handler.

```go
webui.Bind[webui.Void](win, "myFunc", func(e webui.Event) webui.Void {
    resp, err := e.ScriptClient("return document.title;", webui.ScriptOptions{})
    if err != nil {
        fmt.Println("Error:", err)
    } else {
        fmt.Println("Page title:", resp)
    }
    return nil
})
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_runtime

Set the runtime for `.js` and `.ts` files. When a browser requests one of these files, WebUI will execute it through the chosen runtime instead of serving it statically.

```go
win.SetRuntime(webui.Deno)   // Use Deno for .js and .ts
win.SetRuntime(webui.Bun)    // Use Bun for .js and .ts
win.SetRuntime(webui.Nodejs) // Use Node.js for .js
win.SetRuntime(webui.None)   // Disable runtime (default)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_size

Set the window size in pixels.

```go
win.SetSize(1024, 768)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_minimum_size

Set the minimum allowed window size in pixels.

```go
win.SetMinimumSize(400, 300)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_position

Set the window position on screen (in pixels from the top-left corner).

```go
win.SetPosition(100, 100)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_center

Center the window on the screen. Works best with WebView. Call before `Show()` for reliable results.

```go
win.SetCenter()
win.Show("index.html")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_kiosk

Enable or disable kiosk mode (borderless full-screen). Must be called before `Show()`.

```go
win.SetKiosk(true)
win.Show("index.html")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_hide

Show or hide the window. Must be called before `Show()`.

```go
win.SetHide(true) // Window will open hidden
win.Show("index.html")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_frameless

Remove the window frame and title bar. Works only with WebView windows.

```go
win.SetFrameless(true)
win.ShowWebView("index.html")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_transparent

Make the WebView window background transparent. Useful for custom-shaped windows.

```go
win.SetTransparent(true)
win.ShowWebView("index.html")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_resizable

Control whether a WebView window can be resized by the user.

```go
win.SetResizable(false) // Fixed size
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_high_contrast

Enable high-contrast support for a window. This sets a flag your CSS can detect to switch to an accessible high-contrast theme.

```go
win.SetHighContrast(true)
```

You can also check the OS-level high-contrast preference:

```go
if webui.IsHighContrast() {
    win.SetHighContrast(true)
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_icon

Set the favicon for embedded HTML content. Pass the icon content as a string and its MIME type.

```go
win.SetIcon("<svg xmlns='http://www.w3.org/2000/svg'>...</svg>", "image/svg+xml")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_close_handler_webview

Set a callback to intercept the WebView window close event. Return `false` to prevent the window from closing, `true` to allow it.

```go
win.SetCloseHandlerWebView(func(w webui.Window) bool {
    fmt.Println("Close requested")
    // Return false to block, true to allow
    return true
})
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_event_blocking

Control event processing for a specific window: `true` processes events one-at-a-time in a single thread; `false` processes each event in a new non-blocking goroutine. To change this for all windows at once, use `SetConfig(webui.UiEventBlocking, ...)`.

```go
win.SetEventBlocking(true)  // Serialized, blocking
win.SetEventBlocking(false) // Concurrent, non-blocking (default)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_timeout

Set the maximum time in seconds that WebUI will wait for the browser window to connect after `Show()` is called. A value of `0` means wait forever. This also affects how long `Wait()` will block when no window has connected yet.

```go
// Wait up to 30 seconds for the browser to connect
webui.SetTimeout(30)

// Wait forever (default behaviour)
webui.SetTimeout(0)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_config

Control global WebUI behaviour. Call at the start of your program before showing any windows.

```go
// Available config options:
// webui.ShowWaitConnection  - wait for browser to connect before Show() returns (default: true)
// webui.UiEventBlocking     - process events in a single thread (default: false)
// webui.FolderMonitor       - auto-refresh UI when root folder files change (default: false)
// webui.MultiClient         - allow multiple clients per window (default: false)
// webui.UseCookies          - use WebUI auth cookies to restrict access (default: true)
// webui.AsynchronousResponse - wait for webui_return_x() before responding (default: false)

webui.SetConfig(webui.ShowWaitConnection, false)
webui.SetConfig(webui.MultiClient, true)
webui.SetConfig(webui.FolderMonitor, true)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### navigate

Navigate all connected clients to a different URL.

```go
win.Navigate("https://mydomain.com/other-page")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### navigate_client

Navigate a single connected client to a different URL. Called from within an event handler.

```go
webui.Bind[webui.Void](win, "goToSettings", func(e webui.Event) webui.Void {
    e.NavigateClient("/settings.html")
    return nil
})
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_url

Get the full URL of a running window's web server.

```go
url := win.GetUrl()
fmt.Println("Window URL:", url)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_port

Get the network port used by the window's web server. Returns an error if the window is not running.

```go
port, err := win.GetPort()
if err == nil {
    fmt.Printf("WebUI JS: http://localhost:%d/webui.js\n", port)
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_port

Assign a specific network port for the window's web server. Returns `true` if the port is free and successfully reserved.

```go
if win.SetPort(8080) {
    fmt.Println("Listening on port 8080")
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_free_port

Get an available free network port.

```go
port := webui.GetFreePort()
fmt.Println("Free port:", port)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_public

Make the window's web server accessible on the local network (binds to `0.0.0.0` instead of `127.0.0.1`).

```go
win.SetPublic(true)
win.Show("index.html")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### open_url

Open a URL in the system's default web browser (outside of WebUI).

```go
webui.OpenURL("https://webui.me")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_best_browser

Get the recommended browser ID for this window. If a browser is already in use, the same ID is returned.

```go
browser := win.GetBestBrowser()
win.ShowBrowser("index.html", browser)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### browser_exists

Check if a specific web browser is installed on the system.

```go
if webui.BrowserExists(webui.Chrome) {
    win.ShowBrowser("index.html", webui.Chrome)
} else {
    win.Show("index.html")
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_profile

Set the browser profile (user data directory) to use. Pass empty strings to use the default profile. Must be called before `Show()`.

```go
// Use a custom profile
win.SetProfile("MyApp", "/home/user/.config/myapp-profile")

// Use the default browser profile
win.SetProfile("", "")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_proxy

Set a proxy server for the browser. Must be called before `Show()`.

```go
win.SetProxy("http://127.0.0.1:8888")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_browser_folder

Set a custom path from which WebUI will look for the browser executable.

```go
webui.SetBrowserFolder("/opt/my-browser/")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_custom_parameters

Append custom CLI parameters when launching the browser. Useful for enabling remote debugging or other browser flags.

```go
win.SetCustomParameters("--remote-debugging-port=9222")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### delete_profile

Delete the local browser profile folder for a specific window. Call after `Wait()`. Note that this may affect other windows using the same browser profile.

```go
webui.Wait()
win.DeleteProfile()
webui.Clean()
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### delete_all_profiles

Delete all local browser profile folders created by WebUI. Call after `Wait()`.

```go
webui.Wait()
webui.DeleteAllProfiles()
webui.Clean()
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_root_folder

Set the web server root folder for a specific window. The browser will resolve relative file paths from this directory.

```go
win.SetRootFolder("/home/user/myapp/public")
win.Show("index.html")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_default_root_folder

Set the web server root folder for all windows. Must be called before `Show()`.

```go
if err := webui.SetDefaultRootFolder("/home/user/myapp/public"); err != nil {
    log.Fatal(err)
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_file_handler

Set a custom Go function to intercept and serve HTTP requests. The handler receives the requested filename and must return the full HTTP response (headers + body) as a byte slice, or `nil` to fall back to normal file serving.

```go
win.SetFileHandler(func(filename string) ([]byte, int) {
    if filename == "/api/hello" {
        body := "HTTP/1.1 200 OK\r\nContent-Type: text/plain\r\n\r\nHello!"
        b := []byte(body)
        return b, len(b)
    }
    return nil, 0 // Fall back to normal file serving
})
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_file_handler_window

Same as `SetFileHandler()`, but the handler also receives the window number. Useful when the same handler function is shared across multiple windows.

```go
win.SetFileHandlerWindow(func(w webui.Window, filename string) ([]byte, int) {
    fmt.Printf("Window %d requested: %s\n", w, filename)
    return nil, 0
})
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_response_file_handler

Send a file handler response asynchronously. Use this when your `SetFileHandler` or `SetFileHandlerWindow` callback cannot produce the response immediately (e.g. it needs to fetch data from a database or another goroutine) and must return `nil` initially, then push the response later.

```go
win.SetFileHandler(func(filename string) ([]byte, int) {
    if filename == "/api/data" {
        // Kick off async work and return nil now
        go func() {
            data := fetchDataFromDB() // some async operation
            win.SetResponseFileHandler(data)
        }()
        return nil, 0
    }
    return nil, 0
})
```

> Note: `SetFileHandler` automatically sets `ShowWaitConnection` and `UseCookies` to `false`, since the custom handler takes over full HTTP response responsibility.


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### send_raw

Send raw binary data to a JavaScript function on all connected clients. The JavaScript function receives an `ArrayBuffer`.

```go
// JavaScript: function onData(data) { ... }
win.SendRaw("onData", []byte{0x01, 0x02, 0x03})
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### send_raw_client

Send raw binary data to a JavaScript function on a specific client only.

```go
webui.Bind[webui.Void](win, "requestData", func(e webui.Event) webui.Void {
    e.SendRawClient("onData", []byte{0x01, 0x02, 0x03})
    return nil
})
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### close_client

Disconnect and close a specific client connection. Called from within an event handler.

```go
webui.Bind[webui.Void](win, "logout", func(e webui.Event) webui.Void {
    e.CloseClient()
    return nil
})
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_parent_process_id

Get the process ID of the current backend application process.

```go
pid := win.GetParentProcessID()
fmt.Println("Backend PID:", pid)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_child_process_id

Get the process ID of the browser or WebView child process created for this window. In WebView mode, this returns the same PID as `GetParentProcessID()` since both run in the same process.

```go
pid := win.GetChildProcessID()
fmt.Println("Browser PID:", pid)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_hwnd

Get the native window handle. Returns `HWND` on Windows (works with both WebView and web browser), and `GtkWindow*` on Linux (WebView only).

```go
hwnd := win.GetHwnd() // unsafe.Pointer — cast to platform type as needed
```

On Windows, you can also use the Win32-specific variant:

```go
hwnd := win.Win32GetHwnd()
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### encode

Encode a string to Base64.

```go
encoded := webui.Encode("Hello, World!")
fmt.Println(encoded) // "SGVsbG8sIFdvcmxkIQ=="
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### decode

Decode a Base64-encoded string.

```go
decoded := webui.Decode("SGVsbG8sIFdvcmxkIQ==")
fmt.Println(decoded) // "Hello, World!"
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_mime_type

Look up the HTTP MIME type for a given filename or extension.

```go
mime := webui.GetMimeType("image.png")  // "image/png"
mime  = webui.GetMimeType("style.css")  // "text/css"
mime  = webui.GetMimeType("app.wasm")   // "application/wasm"
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### malloc

Allocate memory using WebUI's memory management system. Memory allocated this way can be safely freed with `Free()` at any time.

```go
ptr := webui.Malloc(1024)
defer webui.Free(ptr)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### free

Free a buffer that was previously allocated with `Malloc()`.

```go
ptr := webui.Malloc(256)
// ... use ptr ...
webui.Free(ptr)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### memcpy

Copy raw bytes between two memory pointers.

```go
src := []byte("Hello")
dst := webui.Malloc(uint(len(src)))
defer webui.Free(dst)
webui.Memcpy(dst, unsafe.Pointer(&src[0]), uint(len(src)))
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_tls_certificate

Set the SSL/TLS certificate and private key (both in PEM format) to enable HTTPS. Requires building with the `webui_tls` build tag. If both arguments are empty strings, WebUI generates a self-signed certificate automatically.

```go
// go build -tags webui_tls

cert := "-----BEGIN CERTIFICATE-----\n..."
key  := "-----BEGIN PRIVATE KEY-----\n..."

if !webui.SetTLSCertificate(cert, key) {
    log.Fatal("Invalid TLS certificate or key")
}

win.Show("index.html")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_logger

Set a custom function to receive WebUI log messages. Useful for integrating WebUI logs into your application's logging system.

```go
webui.SetLogger(func(level webui.LoggerLevel, message string, userData unsafe.Pointer) {
    switch level {
    case webui.LoggerLevelDebug:
        fmt.Println("[DEBUG]", message)
    case webui.LoggerLevelInfo:
        fmt.Println("[INFO]", message)
    case webui.LoggerLevelError:
        fmt.Println("[ERROR]", message)
    }
}, nil)
```

Logger level constants:

```go
webui.LoggerLevelDebug // All logs with full details
webui.LoggerLevelInfo  // General informational logs only
webui.LoggerLevelError // Fatal error logs only
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_last_error_number

Get the error code from the last WebUI operation that failed.

```go
code := webui.GetLastErrorNumber()
if code != 0 {
    fmt.Println("Error code:", code)
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_last_error_message

Get the human-readable error message from the last WebUI operation that failed.

```go
if msg := webui.GetLastErrorMessage(); msg != "" {
    fmt.Println("WebUI error:", msg)
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### clean

Free all memory resources allocated by WebUI. Call this at the very end of your program, after `Wait()`.

```go
webui.Wait()
webui.Clean()
```
