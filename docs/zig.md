<div align="center">

![Logo](data/webui_120.svg)

# WebUI v2.5 - Zig Documentation

> Use any web browser or WebView as GUI, with your preferred language in the backend and modern web technologies in the frontend, all in a lightweight portable library.

![Screenshot](data/screenshot.png)

</div>


## Getting Started
- [Download And Install](#download-and-install)
- [Minimal Example](#minimal-example)

## Available APIs
- **Window**
  - [new_window](#new_window)
  - [new_window_with_id](#new_window_with_id)
  - [get_new_window_id](#get_new_window_id)
  - [show](#show)
  - [show_browser](#show_browser)
  - [show_wv](#show_wv)
  - [start_server](#start_server)
  - [wait](#wait)
  - [wait_async](#wait_async)
  - [is_shown](#is_shown)
  - [close](#close)
  - [destroy](#destroy)
  - [exit](#exit)
  - [focus](#focus)
  - [minimize](#minimize)
  - [maximize](#maximize)
- **Window Configuration**
  - [set_kiosk](#set_kiosk)
  - [set_size](#set_size)
  - [set_minimum_size](#set_minimum_size)
  - [set_position](#set_position)
  - [set_center](#set_center)
  - [set_hide](#set_hide)
  - [set_resizable](#set_resizable)
  - [set_frameless](#set_frameless)
  - [set_transparent](#set_transparent)
  - [set_high_contrast](#set_high_contrast)
  - [set_custom_parameters](#set_custom_parameters)
  - [set_icon](#set_icon)
  - [set_close_handler_wv](#set_close_handler_wv)
- **Binding & Events**
  - [bind](#bind)
  - [binding](#binding)
  - [set_context](#set_context)
  - [event](#event)
  - [get_count](#get_count)
  - [get_int](#get_int)
  - [get_int_at](#get_int_at)
  - [get_float](#get_float)
  - [get_float_at](#get_float_at)
  - [get_string](#get_string)
  - [get_string_at](#get_string_at)
  - [get_bool](#get_bool)
  - [get_bool_at](#get_bool_at)
  - [get_size](#get_size)
  - [get_size_at](#get_size_at)
  - [get_context](#get_context)
  - [return_int](#return_int)
  - [return_float](#return_float)
  - [return_string](#return_string)
  - [return_bool](#return_bool)
  - [return_value](#return_value)
- **JavaScript**
  - [run](#run)
  - [script](#script)
- **Multi-Client**
  - [show_client](#show_client)
  - [close_client](#close_client)
  - [navigate_client](#navigate_client)
  - [run_client](#run_client)
  - [script_client](#script_client)
  - [send_raw_client](#send_raw_client)
- **Navigation**
  - [navigate](#navigate)
  - [get_url](#get_url)
  - [open_url](#open_url)
  - [set_public](#set_public)
- **File Serving**
  - [set_file_handler](#set_file_handler)
  - [set_file_handler_window](#set_file_handler_window)
  - [set_root_folder](#set_root_folder)
  - [set_default_root_folder](#set_default_root_folder)
- **Data Transfer**
  - [send_raw](#send_raw)
  - [encode](#encode)
  - [decode](#decode)
  - [get_mime_type](#get_mime_type)
  - [malloc](#malloc)
  - [free](#free)
- **Network & Ports**
  - [get_port](#get_port)
  - [set_port](#set_port)
  - [get_free_port](#get_free_port)
- **Browser & Profile**
  - [get_best_browser](#get_best_browser)
  - [browser_exist](#browser_exist)
  - [set_profile](#set_profile)
  - [set_proxy](#set_proxy)
  - [set_browser_folder](#set_browser_folder)
  - [delete_profile](#delete_profile)
  - [delete_all_profiles](#delete_all_profiles)
- **Configuration**
  - [set_config](#set_config)
  - [set_timeout](#set_timeout)
  - [set_event_blocking](#set_event_blocking)
  - [set_runtime](#set_runtime)
  - [set_logger](#set_logger)
  - [set_tls_certificate](#set_tls_certificate)
- **Process & System**
  - [get_parent_process_id](#get_parent_process_id)
  - [get_child_process_id](#get_child_process_id)
  - [get_hwnd](#get_hwnd)
- **Error Handling**
  - [get_last_error](#get_last_error)
- **Enums**
  - [Browser](#browser)
  - [Runtime](#runtime)
  - [EventKind](#eventkind)
  - [Config](#config)
  - [LoggerLevel](#loggerlevel)
- **Cleanup**
  - [clean](#clean)

- [JavaScript APIs](javascript.md)


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### Download And Install

WebUI is written in pure C, but provides a Zig wrapper with idiomatic APIs. HTML5 and JavaScript run in the frontend inside a web browser or WebView window.

**Minimum Zig version: `0.14.0`**

1. Add to `build.zig.zon`

```sh
zig fetch --save https://github.com/webui-dev/zig-webui/archive/main.tar.gz
```

2. Configure `build.zig`

```zig
const zig_webui = b.dependency("zig-webui", .{
    .target = target,
    .optimize = optimize,
    .enable_tls = false, // enable TLS support
    .is_static = true,   // static linking (recommended on Windows)
});

exe.root_module.addImport("webui", zig_webui.module("webui"));
exe.linkLibrary(zig_webui.artifact("webui"));
```

> Dynamic linking under Windows may cause symbol duplication problems. Static linking is recommended.


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### Minimal Example

```zig
const webui = @import("webui");

pub fn main() !void {
    var win = webui.newWindow();
    try win.show("<html><script src=\"webui.js\"></script> Hello World from Zig! </html>");
    webui.wait();
}
```

[More Zig Examples](https://github.com/webui-dev/zig-webui/tree/main/examples).


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### new_window

Create a new window object.

```zig
var win = webui.newWindow();
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### new_window_with_id

Create a new window object using a specific window ID. The ID must be between `1` and `WEBUI_MAX_IDS`.

```zig
var win = try webui.newWindowWithId(42);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_new_window_id

Get a free window ID that can be used with `newWindowWithId()`.

```zig
const id = webui.getNewWindowId();
var win = try webui.newWindowWithId(id);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### show

Show a window using embedded HTML, a local file path, or a URL. If the window is already open, it will be refreshed. In multi-client mode, this refreshes all connected clients.

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

> To use only a specific browser please use `showBrowser()`

> To use only WebView please use `showWv()`

```zig
try win.show("<html><script src=\"webui.js\"></script> ... </html>");
try win.show("index.html");
try win.show("https://mydomain.com");
```

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### show_browser

Show a window using a specific web browser.

> It's recommended to use `ChromiumBased` for the most consistent experience.

> On macOS, the browser icon may remain in the Dock after exit. Use `showWv()` to avoid this.

```zig
try win.showBrowser("<html><script src=\"webui.js\"></script> ... </html>", .Chrome);
try win.showBrowser("index.html", .Firefox);
try win.showBrowser("https://mydomain.com", .ChromiumBased);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### show_wv

Show a WebView window using embedded HTML, a file, or a URL. If the window is already open, it will be refreshed.

> On Win32, `WebView2Loader.dll` is required.

```zig
try win.showWv("<html><script src=\"webui.js\"></script> ... </html>");
try win.showWv("index.html");
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### start_server

Start only the web server and return the URL. No window will be shown. Useful for headless operation or integrating with an external browser.

```zig
const url = try win.startServer("/full/path/to/root/folder");
std.debug.print("Server URL: {s}\n", .{url});
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### bind

Bind an HTML element event or a JavaScript function name to a Zig callback. An empty element name binds to all events on all elements.

```zig
fn myFunction(e: *webui.Event) void {
    // Called when the button with id="MyID" is clicked
}

_ = try win.bind("MyID", myFunction);

// Bind to all events on all elements
_ = try win.bind("", myFunction);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### binding

A higher-level version of `bind()` that automatically extracts JavaScript arguments and maps them to Zig callback parameters. The callback can accept typed parameters directly instead of reading from the event manually.

Supported parameter types: `bool`, any integer type, any float type, `[:0]const u8`, `[*]const u8`, `webui.Event`, `*webui.Event`.

`Event` and `*Event` parameters do not consume a JavaScript argument slot; all other types consume arguments in order.

```zig
fn onClick(e: *webui.Event) void {
    // Receives the full event object
}

fn onAdd(a: i32, b: i32) void {
    // Arguments come directly from JavaScript: webui.call('onAdd', [3, 4])
    std.debug.print("Sum: {}\n", .{a + b});
}

fn onGreet(name: [:0]const u8) void {
    std.debug.print("Hello, {s}!\n", .{name});
}

_ = try win.binding("onClick", onClick);
_ = try win.binding("onAdd", onAdd);
_ = try win.binding("onGreet", onGreet);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_context

Attach arbitrary user data to a binding. The data can be retrieved later inside the callback using `event.getContext()`.

```zig
var my_data: MyStruct = .{};

win.setContext("MyID", &my_data);

fn myFunction(e: *webui.Event) void {
    const data: *MyStruct = @ptrCast(@alignCast(try e.getContext()));
    _ = data;
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### event

Every event carries information about what triggered it.

```zig
pub const Event = extern struct {
    window: usize,          // The window handle
    event_type: EventKind,  // Event type (connected, click, navigation...)
    element: [*:0]u8,       // HTML element ID
    event_number: usize,    // Internal WebUI event number
    bind_id: usize,         // Bind ID
    client_id: usize,       // Client's unique ID
    connection_id: usize,   // Client's connection ID
    cookies: [*:0]u8,       // Client's full cookies
};
```

```zig
fn myFunction(e: *webui.Event) void {
    const element = std.mem.span(e.element);
    std.debug.print("Event on element: {s}\n", .{element});

    switch (e.event_type) {
        .EVENT_CONNECTED    => std.debug.print("Connected\n", .{}),
        .EVENT_DISCONNECTED => std.debug.print("Disconnected\n", .{}),
        .EVENT_MOUSE_CLICK  => std.debug.print("Click\n", .{}),
        .EVENT_NAVIGATION   => {
            const url = e.getString();
            std.debug.print("Navigating to: {s}\n", .{url});
        },
        .EVENT_CALLBACK     => std.debug.print("JS function call\n", .{}),
    }

    // Get the parent window
    var win = e.getWindow();
    _ = win;
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_count

Get the number of arguments passed from JavaScript in this event.

```zig
fn myFunction(e: *webui.Event) void {
    const count = e.getCount();
    std.debug.print("Received {} arguments\n", .{count});
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_int

Get the first JavaScript argument as a 64-bit integer.

```zig
fn myFunction(e: *webui.Event) void {
    const value = e.getInt();
    std.debug.print("{}\n", .{value});
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_int_at

Get a JavaScript argument as a 64-bit integer at a specific index.

```zig
fn myFunction(e: *webui.Event) void {
    const first = e.getIntAt(0);
    const third = e.getIntAt(2);
    std.debug.print("{} {}\n", .{ first, third });
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_float

Get the first JavaScript argument as a 64-bit float.

```zig
fn myFunction(e: *webui.Event) void {
    const value = e.getFloat();
    std.debug.print("{d}\n", .{value});
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_float_at

Get a JavaScript argument as a 64-bit float at a specific index.

```zig
fn myFunction(e: *webui.Event) void {
    const first = e.getFloatAt(0);
    const second = e.getFloatAt(1);
    std.debug.print("{d} {d}\n", .{ first, second });
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_string

Get the first JavaScript argument as a null-terminated string slice.

```zig
fn myFunction(e: *webui.Event) void {
    const value = e.getString();
    std.debug.print("{s}\n", .{value});
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_string_at

Get a JavaScript argument as a null-terminated string slice at a specific index.

```zig
fn myFunction(e: *webui.Event) void {
    const first = e.getStringAt(0);
    const second = e.getStringAt(1);
    std.debug.print("{s} {s}\n", .{ first, second });
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_bool

Get the first JavaScript argument as a boolean.

```zig
fn myFunction(e: *webui.Event) void {
    const value = e.getBool();
    std.debug.print("{}\n", .{value});
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_bool_at

Get a JavaScript argument as a boolean at a specific index.

```zig
fn myFunction(e: *webui.Event) void {
    const first = e.getBoolAt(0);
    const second = e.getBoolAt(1);
    std.debug.print("{} {}\n", .{ first, second });
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_size

Get the size in bytes of the first JavaScript argument. Useful for raw binary data.

```zig
fn myFunction(e: *webui.Event) void {
    const size = e.getSize() catch 0;
    std.debug.print("First argument is {} bytes\n", .{size});
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_size_at

Get the size in bytes of a JavaScript argument at a specific index.

```zig
fn myFunction(e: *webui.Event) void {
    const size = e.getSizeAt(0) catch 0;
    std.debug.print("Argument 0 is {} bytes\n", .{size});
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_context

Retrieve user data previously attached with `setContext()`.

```zig
fn myFunction(e: *webui.Event) void {
    const ptr = e.getContext() catch return;
    const data: *MyStruct = @ptrCast(@alignCast(ptr));
    _ = data;
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### return_int

Return an integer response to JavaScript.

```zig
fn myFunction(e: *webui.Event) void {
    e.returnInt(123);
}
```

JavaScript:
```js
const result = await webui.call('myFunction');
console.log(result); // 123
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### return_float

Return a float response to JavaScript.

```zig
fn myFunction(e: *webui.Event) void {
    e.returnFloat(3.14);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### return_string

Return a string response to JavaScript.

```zig
fn myFunction(e: *webui.Event) void {
    e.returnString("Hello from Zig!");
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### return_bool

Return a boolean response to JavaScript.

```zig
fn myFunction(e: *webui.Event) void {
    e.returnBool(true);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### return_value

A generic convenience function that dispatches to the correct `returnInt`, `returnFloat`, `returnString`, or `returnBool` based on the type of the value at compile time.

```zig
fn myFunction(e: *webui.Event) void {
    e.returnValue(42);          // calls returnInt
    e.returnValue(3.14);        // calls returnFloat
    e.returnValue(true);        // calls returnBool
    e.returnValue("Hello!");    // calls returnString
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### run

Run JavaScript on the page without waiting for a response. Targets all connected clients.

```zig
win.run("alert('Hello from Zig!');");
win.run("document.getElementById('output').innerText = 'Updated';");
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### script

Run JavaScript and get the response back. Works in single-client mode. Provide a buffer large enough to hold the response string.

```zig
fn myFunction(e: *webui.Event) void {
    var response: [64]u8 = std.mem.zeroes([64]u8);
    var win = e.getWindow();

    win.script(
        "return 2 * 2;",
        0,            // timeout in seconds (0 = no timeout)
        &response,
    ) catch {
        std.debug.print("JS Error: {s}\n", .{response});
        return;
    };

    std.debug.print("Result: {s}\n", .{response}); // "4"
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### navigate

Navigate all connected clients to a specific URL.

```zig
win.navigate("https://webui.me");
win.navigate("index.html");
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_url

Get the full current URL of the running window.

```zig
const url = try win.getUrl();
std.debug.print("Current URL: {s}\n", .{url});
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### open_url

Open a URL in the system's default web browser (outside WebUI).

```zig
webui.openUrl("https://webui.me");
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_public

Allow the window's server to be accessible from a public network (not just localhost).

```zig
win.setPublic(true);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_kiosk

Set the window to kiosk mode (full screen, borderless). Useful for dedicated display applications.

```zig
win.setKiosk(true);
try win.show("index.html");
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_size

Set the window width and height in pixels.

```zig
win.setSize(800, 600);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_minimum_size

Set the minimum allowed window size in pixels.

```zig
win.setMinimumSize(400, 300);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_position

Set the window position (top-left corner) in screen pixels.

```zig
win.setPosition(100, 100);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_center

Center the window on the screen. Works best with WebView. Call before `show()` for best results.

```zig
win.setCenter();
try win.show("index.html");
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_hide

Start the window in hidden mode. Call before `show()`.

```zig
win.setHide(true);
try win.show("index.html");
// Window is running but not visible; show it later with setHide(false)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_resizable

Control whether the window frame is resizable or fixed. Works only on WebView windows.

```zig
win.setResizable(false); // Fixed size
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_frameless

Remove the window frame (title bar, borders). Works only on WebView windows.

```zig
win.setFrameless(true);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_transparent

Make the WebView window background transparent. Combine with CSS `background: transparent` for custom shapes.

```zig
win.setTransparent(true);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_high_contrast

Enable high-contrast support for the window. Useful for building accessible themes with CSS.

```zig
win.setHighContrast(true);

// Check if the OS is using high contrast
if (webui.isHighConstrast()) {
    // Apply high contrast styles
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_custom_parameters

Add custom CLI parameters to the web browser launch command. Useful for enabling browser features such as remote debugging.

```zig
win.setCustomParameters("--remote-debugging-port=9222");
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### focus

Bring the window to the front and give it focus.

```zig
win.focus();
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### minimize

Minimize a WebView window.

```zig
win.minimize();
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### maximize

Maximize a WebView window.

```zig
win.maximize();
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_icon

Set the default embedded HTML favicon. When the browser requests `favicon.ico`, WebUI sends a redirect to the provided icon.

```zig
// SVG icon
win.setIcon("<svg>...</svg>", "image/svg+xml");

// PNG icon (base64 data URL)
// win.setIcon("data:image/png;base64,...", "image/png");
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_file_handler

Set a custom handler function to serve files. The handler receives the requested filename and returns the file content (full HTTP response headers + body), or `null` to let WebUI serve the file from disk.

```zig
var count: i32 = 0;

fn myFilesHandler(filename: []const u8) ?[]const u8 {
    std.debug.print("Requested: {s}\n", .{filename});

    if (std.mem.eql(u8, filename, "/test.txt")) {
        return "This is embedded file content.";
    } else if (std.mem.eql(u8, filename, "/dynamic.html")) {
        var buf = webui.malloc(1024) catch return null;
        @memset(buf, 0);
        count += 1;
        const content = std.fmt.bufPrint(buf,
            \\<html>
            \\  Count: {} <a href="dynamic.html">[Refresh]</a>
            \\  <script src="webui.js"></script>
            \\</html>
        , .{count}) catch return null;
        return content;
    }

    // Return null to let WebUI look for the file locally
    return null;
}

win.setFileHandler(myFilesHandler);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_file_handler_window

Same as `setFileHandler()`, but the handler also receives the window handle. Useful when multiple windows share one handler. This deactivates any handler set with `setFileHandler()`.

```zig
fn myHandler(window_handle: usize, filename: []const u8) ?[]const u8 {
    std.debug.print("Window {}: {s}\n", .{ window_handle, filename });
    return null;
}

win.setFileHandlerWindow(myHandler);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_root_folder

Set the web-server root folder for a specific window.

```zig
try win.setRootFolder("/path/to/web/files");
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_default_root_folder

Set the web-server root folder for all windows. Should be called before `show()`.

```zig
try webui.setDefaultRootFolder("/path/to/web/files");
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### show_client

Show a page to a single connected client (from inside an event callback). If the page is already shown, it is refreshed.

```zig
fn onConnect(e: *webui.Event) void {
    _ = e.showClient("<html><script src=\"webui.js\"></script> Hello! </html>");
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### close_client

Close the connection for a single client (from inside an event callback).

```zig
fn onButton(e: *webui.Event) void {
    e.closeClient();
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### navigate_client

Navigate a single client to a specific URL.

```zig
fn onButton(e: *webui.Event) void {
    e.navigateClient("https://webui.me");
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### run_client

Run JavaScript on a single client without waiting for a response.

```zig
fn onButton(e: *webui.Event) void {
    e.runClient("alert('Hello, single client!');");
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### script_client

Run JavaScript on a single client and get the response back. Provide a buffer large enough to hold the response.

```zig
fn onButton(e: *webui.Event) void {
    var response: [64]u8 = std.mem.zeroes([64]u8);

    e.scriptClient("return 10 + 5;", 0, &response) catch {
        std.debug.print("JS Error: {s}\n", .{response});
        return;
    };

    std.debug.print("Result: {s}\n", .{response}); // "15"
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### send_raw_client

Send raw binary data to a single client. The JavaScript side receives it via the named function.

```zig
fn onButton(e: *webui.Event) void {
    var data = [_]u8{ 0x01, 0x02, 0x03, 0x04 };
    e.sendRawClient("myJsReceiver", &data);
}
```

JavaScript:
```js
function myJsReceiver(data) {
    const view = new Uint8Array(data);
    console.log(view);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### send_raw

Send raw binary data to the UI. Targets all connected clients.

```zig
var data = [_]u8{ 0xDE, 0xAD, 0xBE, 0xEF };
win.sendRaw("myJsReceiver", &data);
```

JavaScript:
```js
function myJsReceiver(data) {
    const view = new Uint8Array(data);
    console.log(view);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### encode

Encode a string to Base64. The returned slice is allocated by WebUI and must be freed with `webui.free()`.

```zig
const encoded = try webui.encode("Hello, World!");
defer webui.free(encoded);
std.debug.print("Encoded: {s}\n", .{encoded});
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### decode

Decode a Base64 string. The returned slice is allocated by WebUI and must be freed with `webui.free()`.

```zig
const decoded = try webui.decode("SGVsbG8sIFdvcmxkIQ==");
defer webui.free(decoded);
std.debug.print("Decoded: {s}\n", .{decoded});
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_mime_type

Get the HTTP MIME type for a given filename extension.

```zig
const mime = webui.getMimeType("image.png");
std.debug.print("MIME: {s}\n", .{mime}); // "image/png"
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### malloc

Allocate memory using the WebUI memory management system. Memory allocated this way can be safely freed with `webui.free()` or will be freed automatically in some contexts (e.g. file handler returns).

```zig
const buf = try webui.malloc(1024);
defer webui.free(buf);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### free

Free a buffer that was allocated with `webui.malloc()` or returned by `webui.encode()` / `webui.decode()`.

```zig
const encoded = try webui.encode("test");
webui.free(encoded);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_port

Get the network port used by the running window server. Useful for building the `webui.js` URL manually.

```zig
const port = try win.getPort();
std.debug.print("http://localhost:{}/webui.js\n", .{port});
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_port

Set a specific network port for the window's web server. Useful when integrating with an external server like NGINX.

```zig
try win.setPort(8080);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_free_port

Get an available free network port that WebUI can use.

```zig
const port = webui.getFreePort();
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_best_browser

Get the recommended browser ID to use for this window. If a browser is already in use, the same ID is returned.

```zig
const browser = win.getBestBrowser();
try win.showBrowser("index.html", browser);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### browser_exist

Check if a specific web browser is installed on the system.

```zig
if (webui.browserExist(.Chrome)) {
    std.debug.print("Chrome is available\n", .{});
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_profile

Set the browser profile to use. An empty name and path means the default user profile. Must be called before `show()`.

```zig
// Use a custom profile
win.setProfile("MyAppProfile", "/path/to/profiles/MyAppProfile");

// Use the default profile
win.setProfile("", "");
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_proxy

Set a proxy server for the browser. Must be called before `show()`.

```zig
win.setProxy("http://127.0.0.1:8888");
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_browser_folder

Set a custom path where WebUI should look for the browser executable.

```zig
webui.setBrowserFolder("/opt/myapp/browser");
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### delete_profile

Delete the local web-browser profile folder for a specific window. Should be called at the end, after `wait()`.

> This can break other windows that use the same browser profile.

```zig
webui.wait();
win.deleteProfile();
webui.clean();
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### delete_all_profiles

Delete all local web-browser profile folders. Should be called at the end, after `wait()`.

```zig
webui.wait();
webui.deleteAllProfiles();
webui.clean();
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_config

Control global WebUI behaviour. It's recommended to call this at the beginning before any windows are shown.

```zig
// Wait for browser to connect before show() returns
webui.setConfig(.show_wait_connection, true);

// Allow multiple browser clients per window (web app mode)
webui.setConfig(.multi_client, true);

// Auto-refresh when files in root folder change
webui.setConfig(.folder_monitor, true);

// Block all UI events in a single thread
webui.setConfig(.ui_event_blocking, false);

// Disable WebUI auth cookies (allow any URL access)
webui.setConfig(.use_cookies, false);

// Wait for backend to set a response before returning to JS
webui.setConfig(.asynchronous_response, true);
```

See the [Config](#config) enum for all options.


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_timeout

Set the maximum time in seconds to wait for the browser to connect. Affects `show()` and `wait()`. A value of `0` means wait forever.

```zig
// Wait at most 10 seconds for the browser to start
webui.setTimeout(10);
webui.wait();
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_event_blocking

Control whether UI events from this window are processed one-at-a-time in a single blocking thread (`true`) or each in their own non-blocking thread (`false`). Affects only this window; use `setConfig(.ui_event_blocking, ...)` to affect all windows.

```zig
win.setEventBlocking(true);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_runtime

Choose a JavaScript runtime for `.js` and `.ts` files served by WebUI.

```zig
win.setRuntime(.Deno);
win.setRuntime(.Bun);
win.setRuntime(.NodeJS);
win.setRuntime(.None); // Disable runtime (default)
```

Once set, HTTP requests to `.js` or `.ts` files will be executed by the chosen runtime.

JavaScript example:
```js
var xhr = new XMLHttpRequest();
xhr.open('GET', 'server.ts?name=WebUI', false);
xhr.send(null);
console.log(xhr.responseText);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_logger

Set a custom log handler to receive WebUI internal log messages.

```zig
fn myLogger(level: webui.LoggerLevel, message: []const u8, user_data: ?*anyopaque) void {
    _ = user_data;
    std.debug.print("[{s}] {s}\n", .{ @tagName(level), message });
}

webui.setLogger(myLogger, null);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_parent_process_id

Get the ID of the parent browser process. The browser may spawn a new process on launch, so this gives the original launcher PID.

```zig
const pid = try win.getParentProcessId();
std.debug.print("Parent PID: {}\n", .{pid});
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_child_process_id

Get the ID of the last spawned child browser process.

```zig
const pid = try win.getChildProcessId();
std.debug.print("Child PID: {}\n", .{pid});
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_hwnd

Get the native window handle as a `*anyopaque`. Returns a Win32 `HWND` on Windows or a `GtkWindow*` on Linux. More reliable with WebView than a browser window.

```zig
const hwnd = try win.getHwnd();
```

On Windows, you can use `win32GetHwnd()` to get a typed `HWND`:

```zig
// Windows only — compile error on other platforms
const hwnd = try win.win32GetHwnd();
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_close_handler_wv

Set a callback to intercept the WebView window close event. Return `false` from the callback to prevent the window from closing, `true` to allow it.

```zig
fn onClose(window_handle: usize) bool {
    std.debug.print("Window {} is closing\n", .{window_handle});
    return true; // Allow close
}

win.setCloseHandlerWv(onClose);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### wait

Block the current thread until all open windows are closed or `exit()` is called. Call this at the end of `main()`.

```zig
// Create windows, bind elements, show windows...

webui.wait(); // Blocks here until all windows are closed
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### wait_async

Non-blocking alternative to `wait()`. Returns `true` while at least one window is still open. Call this in a loop from your main thread when you need to interleave UI event processing with other work.

> In WebView mode, this must be called from the main thread.

```zig
while (webui.waitAsync()) {
    // Your main thread work here
    // (process events, update state, etc.)
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### is_shown

Check if the specified window is still running and connected.

```zig
if (win.isShown()) {
    std.debug.print("Window is running\n", .{});
} else {
    std.debug.print("Window is closed\n", .{});
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### close

Close a specific window. The window object remains valid and the window can be reopened later with `show()`.

```zig
win.close();
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### destroy

Close a specific window and free all its memory resources. The window object is no longer usable after this call.

```zig
win.destroy();
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### exit

Close all open windows and make `wait()` return.

```zig
webui.exit();
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### clean

Free all WebUI memory resources. Call only at the very end of the program, after `wait()`.

```zig
webui.wait();
webui.clean();
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_last_error

Get the last WebUI error number and message. Useful for diagnosing failures when an API returns an error.

```zig
const err = webui.getLastError();
std.debug.print("Error #{}: {s}\n", .{ err.num, err.msg });
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_tls_certificate

Set the SSL/TLS certificate and private key (both in PEM format). This only works when linking against the `webui-2-secure` library. If both strings are empty, WebUI generates a self-signed certificate.

> Build with `-Denable_tls=true` to enable TLS support.

```zig
try webui.setTlsCertificate(
    "-----BEGIN CERTIFICATE-----\n...",
    "-----BEGIN PRIVATE KEY-----\n...",
);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### Browser

Enum of supported web browsers.

```zig
pub const Browser = enum(usize) {
    NoBrowser = 0,   // No browser
    AnyBrowser,      // Default recommended browser
    Chrome,          // Google Chrome
    Firefox,         // Mozilla Firefox
    Edge,            // Microsoft Edge
    Safari,          // Apple Safari
    Chromium,        // The Chromium Project
    Opera,           // Opera Browser
    Brave,           // Brave Browser
    Vivaldi,         // Vivaldi Browser
    Epic,            // Epic Browser
    Yandex,          // Yandex Browser
    ChromiumBased,   // Any Chromium-based browser
    Webview,         // WebView (not a web browser)
};
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### Runtime

Enum of supported JavaScript runtimes for `.js` and `.ts` files.

```zig
pub const Runtime = enum(usize) {
    None = 0,  // No runtime
    Deno,      // Deno
    NodeJS,    // Node.js
    Bun,       // Bun
};
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### EventKind

Enum of event types received in bind callbacks.

```zig
pub const EventKind = enum(usize) {
    EVENT_DISCONNECTED = 0,  // Browser disconnected
    EVENT_CONNECTED,         // Browser connected
    EVENT_MOUSE_CLICK,       // Mouse click on element
    EVENT_NAVIGATION,        // Page navigation (URL in first argument)
    EVENT_CALLBACK,          // JavaScript function call
};
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### Config

Enum of configuration options for `setConfig()`.

```zig
pub const Config = enum(c_int) {
    /// Wait for browser to connect before show() returns. Default: true
    show_wait_connection = 0,

    /// Process all UI events in a single blocking thread. Default: false
    ui_event_blocking = 1,

    /// Auto-refresh the window when files in the root folder change. Default: false
    folder_monitor,

    /// Allow multiple browser clients to connect to the same window.
    /// Useful for web apps. Default: false
    multi_client,

    /// Add `webui_auth` cookies to block unauthorized URL access.
    /// Keep true for single-client desktop apps. Default: true
    use_cookies,

    /// Make WebUI wait for the backend to call returnX() before
    /// responding to JavaScript. Default: false
    asynchronous_response,
};
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### LoggerLevel

Enum of log levels for `setLogger()`.

```zig
pub const LoggerLevel = enum(usize) {
    Debug = 0,  // All logs with full details
    Info,       // General informational logs
    Error,      // Fatal error logs only
};
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---

- [JavaScript APIs](javascript.md)
