<div align="center">

![Logo](data/webui_120.svg)

# WebUI v2.5 - D Documentation

> Use any web browser or WebView as GUI, with your preferred language in the backend and modern web technologies in the frontend, all in a lightweight portable library.

![Screenshot](data/screenshot.png)

</div>


## Getting Started
- [Download And Install](#download-and-install)
- [Minimal Example](#minimal-example)
- [Memory Safety](#memory-safety)

## Available APIs
- **Window**
  - [new_window](#new_window)
  - [new_window_id](#new_window_id)
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
  - [set_frameless](#set_frameless)
  - [set_transparent](#set_transparent)
  - [set_resizable](#set_resizable)
  - [set_high_contrast](#set_high_contrast)
  - [is_high_contrast](#is_high_contrast)
  - [set_icon](#set_icon)
  - [set_icon_file](#set_icon_file)
  - [set_custom_parameters](#set_custom_parameters)
  - [set_close_handler](#set_close_handler)
- **Binding & Events**
  - [bind](#bind)
  - [set_context](#set_context)
  - [get_context](#get_context)
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
  - [get_raw](#get_raw)
  - [get_raw_at](#get_raw_at)
  - [return_int](#return_int)
  - [return_float](#return_float)
  - [return_string](#return_string)
  - [return_bool](#return_bool)
  - [return_value](#return_value)
- **JavaScript**
  - [run](#run)
  - [script](#script)
  - [set_runtime](#set_runtime)
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
  - [set_root_folder](#set_root_folder)
  - [set_default_root_folder](#set_default_root_folder)
  - [set_file_handler](#set_file_handler)
  - [set_file_handler_window](#set_file_handler_window)
  - [return_http](#return_http)
  - [get_mime_type](#get_mime_type)
- **Data Transfer**
  - [send_raw](#send_raw)
  - [encode](#encode)
  - [decode](#decode)
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
  - [set_logger](#set_logger)
  - [set_tls_certificate](#set_tls_certificate)
- **Process & System**
  - [get_parent_process_id](#get_parent_process_id)
  - [get_child_process_id](#get_child_process_id)
  - [get_hwnd](#get_hwnd)
  - [is_app_running](#is_app_running)
- **Error Handling**
  - [get_last_error](#get_last_error)
- **Enums**
  - [Browser](#browser)
  - [Runtime](#runtime)
  - [EventType](#eventtype)
  - [Config](#config)
  - [LoggerLevel](#loggerlevel)
- **Cleanup**
  - [clean](#clean)

- [JavaScript APIs](javascript.md)


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### Download And Install

WebUI is written in pure C, but provides a D wrapper with idiomatic APIs. HTML5 and JavaScript run in the frontend inside a web browser or WebView window.

**Requires DMD 2.100+ or LDC 1.30+.**

Add the package to your project:

```sh
dub add webui
```

That is the whole setup. The WebUI C library is not vendored and needs no C toolchain: on the first build the package downloads the prebuilt library published by the WebUI CI and links it statically.

To pin a specific WebUI release instead of the default `nightly` tag, set `WEBUI_TAG` before building:

```sh
WEBUI_TAG=2.5.0-beta.4 dub build
```

Dropping your own `webui-2-static.lib` (Windows) or `libwebui-2-static.a` (Linux/macOS) into the package's `lib/` folder works too, the download is skipped when the file already exists.

> On macOS use LDC. DMD only emits x86_64 binaries, which cannot link against the arm64 build of the C library.

> On Windows, add `"dflags-windows-dmd": ["-L/SUBSYSTEM:windows", "-L/ENTRY:mainCRTStartup"]` to your `dub.json` to hide the console window.


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### Minimal Example

```d
import webui;

void main()
{
    auto win = newWindow();
    win.show(`<html><script src="/webui.js"></script> Hello World from D! </html>`);
    wait();
}
```

[More D Examples](https://github.com/webui-dev/d-webui/tree/main/examples).


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### Memory Safety

The API is usable from `@safe` code. Everything that crosses into C is marked
`@trusted`, so a module with `@safe:` at the top can create windows, bind
callables, run JavaScript and read event arguments without escaping to `@system`.

```d
@safe:

import webui;

void main()
{
    auto win = newWindow();
    win.bind("add", (long a, long b) => a + b);
    win.show("index.html");
    wait();
}
```

Four entry points stay `@system` on purpose, because no honest `@trusted`
wrapper exists for them:

| API | Why |
| --- | --- |
| `Event.getRaw`, `Event.getRawAt` | Return a slice borrowed from WebUI that dies when the handler returns. Take a `const(ubyte)[]` parameter on the bound callable instead — it receives an owned copy. |
| `Event.getContext`, `Window.setContext` | Traffic in `void*` whose lifetime the wrapper cannot police. |
| `Window.getHwnd`, `Window.win32GetHwnd` | Hand out a raw native handle. |

`Window.handle` and the `Event` accessors are read-only for the same reason: the
C library indexes a fixed-size window table with that number and never
range-checks it, so a writable field would let `@safe` code cause an
out-of-bounds access.


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### new_window

Create a new window object.

```d
auto win = newWindow();
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### new_window_id

Create a new window object using a specific window ID. The ID must be between `1` and `maxIds`.

```d
auto win = newWindow(42);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_new_window_id

Get a free window ID that can be used with `newWindow(id)`.

```d
const id = getNewWindowId();
auto win = newWindow(id);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### show

Show a window using embedded HTML, a local file path, a folder, or a URL. If the window is already open, it will be refreshed. In multi-client mode, this refreshes all connected clients.

Returns `false` when the window could not be shown, see [get_last_error](#get_last_error).

WebUI will try this pattern:

<div class="mermaid">
flowchart TD
    A[Microsoft Windows] --> B(Explicitly call Show Webview?)
    B --> |No| D[Any Chromium Based Browser Installed ?]
    B --> |Yes| C[<strong>Show WebView2 window</strong>]
    D --> |Not Found| F[Any Other Non-chromium Browser ?]
    D --> |Found| E[<strong>Show chromium-based browser window</strong>. <em>Most cases will be Microsoft Edge</em>]
    F --> |Not Found| J[Use default browser]
    F --> |Found| I[<strong>Use that browser</strong>. <em>e.g. Firefox</em>]
</div>

---

<div class="mermaid">
flowchart TD
    A[Linux] --> B(Explicitly call Show Webview?)
    B --> |No| D[Any Chromium Based Browser Installed ?]
    B --> |Yes| C[<strong>Show WebView GTK window</strong>]
    D --> |Not Found| F[Any Other Non-chromium Browser ?]
    D --> |Found| E[<strong>Show chromium-based browser window</strong>. <em>e.g. Chromium</em>]
    F --> |Not Found| J[Use default browser]
    F --> |Found| I[<strong>Use that browser</strong>. <em>Most cases will be Firefox</em>]
</div>

---

<div class="mermaid">
flowchart TD
    A[macOS] --> B(Explicitly call Show Webview?)
    B --> |No| D[Any Chromium Based Browser Installed ?]
    B --> |Yes| C[<strong>Show WebKit window</strong>]
    D --> |Not Found| F[Any Other Non-chromium Browser ?]
    D --> |Found| E[<strong>Show chromium-based browser window</strong>. <em>e.g. Chrome</em>]
    F --> |Not Found| J[Use default browser. <em>e.g. Safari</em>]
    F --> |Found| I[<strong>Use that browser</strong>. <em>e.g. Firefox</em>]
</div>

> To use only a specific browser please use `showBrowser()`

> To use only WebView please use `showWv()`

```d
win.show(`<html><script src="/webui.js"></script> ... </html>`);
win.show("index.html");
win.show("https://mydomain.com");

// Folder mode using the current root folder
win.show();
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### show_browser

Show a window using a specific web browser.

> It's recommended to use `Browser.chromiumBased` for the most consistent experience.

> On macOS, the browser icon may remain in the Dock after exit. Use `showWv()` to avoid this.

```d
win.showBrowser(`<html><script src="/webui.js"></script> ... </html>`, Browser.chrome);
win.showBrowser("index.html", Browser.firefox);
win.showBrowser("https://mydomain.com", Browser.chromiumBased);
```

Falling back when the preferred browser is missing:

```d
if (!win.showBrowser("index.html", Browser.chromiumBased))
    win.show("index.html");
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### show_wv

Show a WebView window using embedded HTML, a file, or a URL. If the window is already open, it will be refreshed.

> The Windows build of the D package links the static WebView2 loader, so no `WebView2Loader.dll` is needed at runtime.

```d
win.showWv(`<html><script src="/webui.js"></script> ... </html>`);
win.showWv("index.html");
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### start_server

Start only the web server and return the URL. No window will be shown. Useful for headless operation or for integrating with an external browser.

```d
import std.stdio;

const url = win.startServer("/full/path/to/root/folder");
writeln("Server URL: ", url);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### bind

Bind an HTML element event or a JavaScript function name to a D callable. An empty element name binds all events of the window, which is how window events such as `EventType.connected` are received.

`bind` accepts **any** callable: a function pointer, a delegate, or a lambda. The JavaScript arguments are converted to the parameter types in order, so most handlers never touch the raw event.

| Parameter type | Filled with |
| --- | --- |
| `Event*`, `Event`, `ref Event` | the event itself, consumes no JavaScript argument |
| `bool` | the argument as a boolean |
| any integral type | the argument as an integer |
| any floating point type | the argument as a float |
| `string`, `const(char)[]` | the argument as a string |
| `const(ubyte)[]`, `const(void)[]` | the argument as raw bytes |

A non-`void` return value is sent back to the JavaScript promise, see [return_value](#return_value).

Returns the unique bind ID, or `0` when binding failed.

```d
import std.stdio;

// Typed parameters, the return value goes back to JavaScript
win.bind("add", (long a, long b) => a + b);

// A string argument
win.bind("greet", (string name) { writeln("Hello ", name); });

// No arguments at all
win.bind("quit", () { exit(); });

// The raw event
win.bind("MyID", (Event* e) {
    writeln("Clicked: ", e.element);
});

// All events of the window
win.bind("", (Event* e) {
    if (e.eventType == EventType.connected)
        writeln("Connected.");
});
```

```html
<script src="/webui.js"></script>
<script>
  add(4, 6).then((result) => console.log(result)); // 10
  greet('D');
</script>
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_context

Attach arbitrary user data to a binding. The data can be read back inside the handler with `getContext()`.

```d
struct MyData { int counter; }

__gshared MyData myData;

win.bind("MyID", &myFunction);
win.setContext("MyID", &myData);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_context

Read the user data attached with `setContext()`.

```d
void myFunction(Event* e)
{
    auto data = cast(MyData*) e.getContext();
    data.counter++;
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### event

Every event carries information about what triggered it. `Event` repeats the layout of the C `webui_event_t` struct, so no marshalling happens when a handler is called.

```d
struct Event
{
    size_t windowHandle();  // The window handle
    EventType eventType();  // Event type (connected, click, navigation...)
    size_t eventNumber();   // Internal WebUI event number
    size_t bindId();        // Bind ID
    size_t clientId();      // Client's unique ID
    size_t connectionId();  // Client's connection ID

    Window window();        // The window that raised the event
    string element();       // HTML element ID
    string cookies();       // Client's full cookies
}
```

These are read-only accessors rather than fields. The C library indexes its
window table with the window number and never range-checks it, so a writable
field would let `@safe` code trigger an out-of-bounds access.

```d
import std.stdio;

void myFunction(Event* e)
{
    writeln("Event on element: ", e.element);

    switch (e.eventType)
    {
    case EventType.connected:
        writeln("Connected");
        break;

    case EventType.disconnected:
        writeln("Disconnected");
        break;

    case EventType.mouseClick:
        writeln("Click");
        break;

    case EventType.navigation:
        writeln("Navigating to: ", e.getString());
        break;

    case EventType.callback:
        writeln("JavaScript function call");
        break;

    default:
        break;
    }

    // Get the parent window
    auto win = e.window;
}
```

> Strings returned by `element`, `cookies` and the `getString*` accessors are garbage-collected copies, so they stay valid after the handler returns. `getRaw*` returns a borrowed slice instead, see [get_raw](#get_raw).

> Handlers run on threads owned by the C library. The wrapper attaches them to the D runtime for the duration of the call, so a handler may allocate, use the GC and throw freely.


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_count

Get the number of arguments passed from JavaScript in this event.

```d
void myFunction(Event* e)
{
    const count = e.getCount();
    writeln("Received ", count, " arguments");
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_int

Get the first JavaScript argument as a 64-bit integer.

```d
void myFunction(Event* e)
{
    const value = e.getInt();
    writeln(value);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_int_at

Get the JavaScript argument at a specific index as a 64-bit integer. Indices start at `0`.

```d
void myFunction(Event* e)
{
    const first = e.getIntAt(0);
    const second = e.getIntAt(1);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_float

Get the first JavaScript argument as a `double`.

```d
void myFunction(Event* e)
{
    const value = e.getFloat();
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_float_at

Get the JavaScript argument at a specific index as a `double`.

```d
void myFunction(Event* e)
{
    const value = e.getFloatAt(2);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_string

Get the first JavaScript argument as a `string`.

```d
void myFunction(Event* e)
{
    const text = e.getString();
    writeln(text);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_string_at

Get the JavaScript argument at a specific index as a `string`.

```d
void myFunction(Event* e)
{
    const first = e.getStringAt(0);
    const second = e.getStringAt(1);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_bool

Get the first JavaScript argument as a `bool`.

```d
void myFunction(Event* e)
{
    const flag = e.getBool();
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_bool_at

Get the JavaScript argument at a specific index as a `bool`.

```d
void myFunction(Event* e)
{
    const flag = e.getBoolAt(1);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_size

Get the size in bytes of the first JavaScript argument. Useful for binary data.

```d
void myFunction(Event* e)
{
    const length = e.getSize();
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_size_at

Get the size in bytes of the JavaScript argument at a specific index.

```d
void myFunction(Event* e)
{
    const length = e.getSizeAt(1);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_raw

Get the first JavaScript argument as raw bytes.

> The returned slice points into memory owned by WebUI and is only valid until the handler returns. Use `.dup` to keep it.

```d
void myFunction(Event* e)
{
    const(ubyte)[] data = e.getRaw();
    writeln(data.length, " bytes, first: ", data[0]);
}
```

The same data is available directly through a bound parameter, which receives an
owned copy rather than a borrowed slice and therefore works from `@safe` code:

```d
win.bind("upload", (const(ubyte)[] data) {
    writeln("Received ", data.length, " bytes");
});
```

```html
<script>
  upload(new Uint8Array([0x41, 0x42, 0x43]));
</script>
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_raw_at

Get the JavaScript argument at a specific index as raw bytes. Same lifetime rules as [get_raw](#get_raw).

```d
void myFunction(Event* e)
{
    const(ubyte)[] data = e.getRawAt(1);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### return_int

Send an integer back to the JavaScript promise.

```d
void myFunction(Event* e)
{
    e.returnInt(123);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### return_float

Send a floating point number back to the JavaScript promise.

```d
void myFunction(Event* e)
{
    e.returnFloat(123.456);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### return_string

Send a string back to the JavaScript promise.

```d
void myFunction(Event* e)
{
    e.returnString("Response...");
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### return_bool

Send a boolean back to the JavaScript promise.

```d
void myFunction(Event* e)
{
    e.returnBool(true);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### return_value

Send any supported value back to JavaScript without picking the right call by hand. Accepts `bool`, any integral or floating point type, an enum, and any string.

```d
void myFunction(Event* e)
{
    e.returnValue(42);          // integer
    e.returnValue(3.14);        // float
    e.returnValue(true);        // boolean
    e.returnValue("Hello");     // string
}
```

Returning a value from a bound callable does this automatically:

```d
win.bind("add", (long a, long b) => a + b);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### run

Run JavaScript in all connected clients without waiting for a response.

```d
import std.format;

win.run("alert('Hello');");
win.run(format("SetCount(%d);", count));
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### script

Run JavaScript and wait for the response. Works in single-client mode only, use [script_client](#script_client) with multiple clients.

Returns a `ScriptResponse`:

```d
struct ScriptResponse
{
    bool ok;      // true when JavaScript executed without an error
    string data;  // the value returned, or the error message when `ok` is false
}
```

`ScriptResponse` converts to `bool`, so it can be tested directly:

```d
import std.stdio;

auto response = win.script("return 4 + 6;");
if (response)
    writeln("Result: ", response.data); // 10
else
    writeln("JavaScript error: ", response.data);
```

The execution timeout in seconds and the response buffer size can both be given, `0` means no timeout:

```d
auto response = win.script("return heavyWork();", 30, 65_536);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_runtime

Choose the runtime WebUI uses to interpret the `.js` and `.ts` files it serves.

```d
win.setRuntime(Runtime.deno);
win.setRuntime(Runtime.bun);
win.setRuntime(Runtime.nodeJS);
win.setRuntime(Runtime.none);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### show_client

Show content in the single client that raised the event, leaving the other clients untouched.

```d
void myFunction(Event* e)
{
    e.showClient("second.html");
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### close_client

Close only the client that raised the event.

```d
void myFunction(Event* e)
{
    e.closeClient();
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### navigate_client

Navigate only the client that raised the event.

```d
void myFunction(Event* e)
{
    e.navigateClient("https://mydomain.com");
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### run_client

Run JavaScript in the client that raised the event, without waiting for a response.

```d
void myFunction(Event* e)
{
    e.runClient("alert('Only you see this');");
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### script_client

Run JavaScript in the client that raised the event and wait for the response. Same `ScriptResponse` as [script](#script).

```d
void myFunction(Event* e)
{
    auto response = e.scriptClient("return 4 + 6;");
    if (response)
        writeln("Result: ", response.data);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### send_raw_client

Send raw binary data to a JavaScript function in the client that raised the event.

```d
void myFunction(Event* e)
{
    ubyte[] buffer = [0x01, 0x02, 0x03];
    e.sendRawClient("myJavaScriptFunc", buffer);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### navigate

Navigate all connected clients to a specific URL.

```d
win.navigate("https://mydomain.com");
```

When a window binds `""`, WebUI blocks every `href` click and forwards it as an `EventType.navigation` event instead, so links can be filtered before they are followed:

```d
win.bind("", (Event* e) {
    if (e.eventType != EventType.navigation)
        return;

    const url = e.getString();
    writeln("Starting navigation to: ", url);
    e.window.navigate(url);
});
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_url

Get the full current URL of a running window. Best called after the window is shown.

```d
const url = win.getUrl();
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### open_url

Open a URL in the native default web browser.

```d
openUrl("https://webui.me");
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_public

Allow a window's address to be reachable from a public network. Off by default, the server only listens on the loopback interface.

```d
win.setPublic(true);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_root_folder

Set the web-server root folder for a specific window.

```d
import std.file : thisExePath;
import std.path : buildPath, dirName;

// Resolve the folder relative to the executable so the app works from any
// working directory
win.setRootFolder(buildPath(dirName(thisExePath), "ui"));
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_default_root_folder

Set the web-server root folder for all windows. Must be called before `show()`.

```d
setDefaultRootFolder("/home/Foo/Bar/");
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_file_handler

Serve files from D instead of from disk. The handler receives the requested path and returns the **full HTTP response**, header and body, or an empty slice to let WebUI serve the file itself.

The returned bytes are copied into WebUI-owned memory, so any D buffer may be returned safely.

```d
import std.format;
import std.string : representation;

const(ubyte)[] myFilesHandler(string filename)
{
    if (filename == "/test.txt")
    {
        const content = "Hello from D!";
        return format(
            "HTTP/1.1 200 OK\r\nContent-Type: text/plain\r\nContent-Length: %d\r\n\r\n%s",
            content.length, content).representation;
    }

    // Anything else: let WebUI serve it from the root folder
    return null;
}

win.setFileHandler(&myFilesHandler);
```

> The C callback carries no window number, so a single handler is shared by all windows. Use [set_file_handler_window](#set_file_handler_window) when different windows need different handlers.


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_file_handler_window

Same as [set_file_handler](#set_file_handler), but the handler also receives the `Window` the request came from.

```d
import std.string : representation;

const(ubyte)[] myFilesHandler(Window window, string filename)
{
    if (window.handle == 1 && filename == "/config.json")
        return "HTTP/1.1 200 OK\r\nContent-Type: application/json\r\n\r\n{}".representation;

    return null;
}

win.setFileHandlerWindow(&myFilesHandler);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### return_http

Send an HTTP response asynchronously. Use it together with [set_file_handler_window](#set_file_handler_window) when the backend needs to answer a request later instead of returning the bytes right away.

```d
win.returnHttp("HTTP/1.1 200 OK\r\n...");
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_mime_type

Get the HTTP mime type WebUI would use for a file name.

```d
const mime = getMimeType("foo.png"); // image/png
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### send_raw

Send raw binary data to a JavaScript function in all connected clients.

```d
ubyte[] buffer = [0x01, 0x02, 0x03];
win.sendRaw("myJavaScriptFunc", buffer);
```

```html
<script>
  function myJavaScriptFunc(myData) {
    // myData is a Uint8Array
  }
</script>
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### encode

Base64-encode text so it can be safely sent to the UI. Returns `null` on failure. The C buffer is freed for you and the result is a garbage-collected `string`.

```d
const encoded = encode("Foo Bar");
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### decode

Decode Base64 text received from the UI. Returns `null` on failure.

```d
const decoded = decode("SGVsbG8="); // Hello
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_port

Get the network port of a running window. Useful to build the HTTP link of `webui.js`.

```d
const port = win.getPort();
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_port

Set the web-server and WebSocket port WebUI should use. Returns `false` when the port is already taken. Useful when serving the UI from an external web server such as NGINX.

```d
// `webui.js` becomes available at http://localhost:8081/webui.js
win.setPort(8081);

// Point the window at the external web server
win.show("http://localhost:8080/");
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_free_port

Get an available free network port.

```d
const port = getFreePort();
win.setPort(port);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_best_browser

Get the recommended web browser for a window. If a browser is already in use, the same one is returned.

```d
const browser = win.getBestBrowser();
win.showBrowser("index.html", browser);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### browser_exist

Check whether a specific web browser is installed.

```d
if (browserExist(Browser.chrome))
    win.showBrowser("index.html", Browser.chrome);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_profile

Set the web browser profile to use. Empty `name` and `path` mean the default user profile. Must be called before `show()`.

```d
win.setProfile("Bar", "/Home/Foo/Bar");

// Use the browser's default profile
win.setProfile("", "");
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_proxy

Set the web browser proxy server. Must be called before `show()`.

```d
win.setProxy("http://127.0.0.1:8888");
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_browser_folder

Set a custom folder to look for web browsers in.

```d
setBrowserFolder("/home/Foo/Bar/");
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### delete_profile

Delete a window's local web-browser profile folder. Call it at the end.

> This can break other windows using the same web browser.

```d
wait();
win.deleteProfile();
clean();
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### delete_all_profiles

Delete all local web-browser profile folders. Call it at the end.

```d
wait();
deleteAllProfiles();
clean();
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_kiosk

Put a window in Kiosk mode (full screen).

```d
win.setKiosk(true);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_size

Set the window size in pixels.

```d
win.setSize(800, 600);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_minimum_size

Set the smallest size the window may be resized to.

```d
win.setMinimumSize(400, 300);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_position

Set the window position on screen.

```d
win.setPosition(100, 100);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_center

Center the window on the screen. Works best with WebView, and gives better results when called before `show()`.

```d
win.setCenter();
win.showWv("index.html");
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_hide

Start the window hidden. Must be called before `show()`.

```d
win.setHide(true);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_frameless

Remove the window frame, so a custom title bar can be drawn in HTML. WebView only.

```d
win.setFrameless(true);
win.showWv(html);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_transparent

Make the window background transparent. WebView only, and usually combined with `setFrameless`.

```d
win.setFrameless(true);
win.setTransparent(true);
win.showWv(html);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_resizable

Choose whether the window frame is resizable or fixed. WebView only.

```d
win.setResizable(false);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_high_contrast

Enable high-contrast support, so a dedicated high-contrast CSS theme can be applied.

```d
win.setHighContrast(true);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### is_high_contrast

Get the operating system's high-contrast preference.

```d
if (isHighContrast())
    win.setHighContrast(true);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_icon

Set the embedded HTML favicon.

```d
win.setIcon("<svg>...</svg>", "image/svg+xml");
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_icon_file

Set the window icon from an icon file. Also used as the taskbar icon on Linux (GTK).

```d
win.setIconFile("icon.png");
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_custom_parameters

Add user-defined CLI parameters to the web browser process.

```d
win.setCustomParameters("--remote-debugging-port=9222");
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_close_handler

Intercept the close event of a WebView window. Return `false` to prevent the window from closing, `true` to allow it.

```d
win.setCloseHandler((Window window) {
    writeln("Window ", window.handle, " is closing");
    return true; // Allow close
});
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_config

Change a library-wide behaviour. Best called before creating any window. See [Config](#config) for the full list.

```d
// Let several browser tabs connect to the same window
setConfig(Config.multiClient, true);
setConfig(Config.useCookies, false);

// Do not block in show() waiting for the window to connect
setConfig(Config.showWaitConnection, false);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_timeout

Set the maximum time in seconds to wait for a window to connect. This affects `show()` and `wait()`. A value of `0` waits forever.

```d
setTimeout(30);

// Never time out, useful for public network windows
setTimeout(0);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_event_blocking

Process this window's UI events one at a time in a single blocking thread (`true`), or every event in its own thread (`false`, the default). Use `setConfig(Config.uiEventBlocking, ...)` to change it for all windows.

```d
win.setEventBlocking(true);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_logger

Install a custom handler for WebUI's internal log messages.

```d
import std.stdio;

setLogger((LoggerLevel level, string message) {
    writefln("[%s] %s", level, message);
});
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_tls_certificate

Set the SSL/TLS certificate and private key, both in PEM format. This only works against the `webui-2-secure` library build. Passing empty strings makes WebUI generate a self-signed certificate.

```d
setTlsCertificate(
    "-----BEGIN CERTIFICATE-----\n...",
    "-----BEGIN PRIVATE KEY-----\n...");
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_parent_process_id

Get the parent process ID, which is the current backend application process.

```d
const pid = win.getParentProcessId();
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_child_process_id

Get the child process ID, which is the web browser window. In WebView mode this returns the parent process ID, because the backend and the WebView window share one process.

```d
const pid = win.getChildProcessId();
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_hwnd

Get the native window handle: a Win32 `HWND` on Windows, a `GtkWindow*` on Linux. More reliable with WebView than with a browser window, since browser PIDs may change on launch.

```d
void* handle = win.getHwnd();
```

On Windows, `win32GetHwnd()` is also available and only compiles on that platform:

```d
version (Windows)
{
    void* hwnd = win.win32GetHwnd();
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### is_app_running

Check whether the app is still running. Handy for background threads that need to stop when the UI goes away.

```d
while (isAppRunning())
{
    // Background work
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### wait

Block the current thread until all open windows are closed or `exit()` is called. Call this at the end of `main()`.

```d
// Create windows, bind elements, show windows...

wait(); // Blocks here until all windows are closed
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### wait_async

Non-blocking alternative to `wait()`. Returns `true` while at least one window is still open. Call it in a loop when the main thread has other work to do.

> In WebView mode, this must be called from the main thread.

```d
while (waitAsync())
{
    // Your main thread work here
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### is_shown

Check whether a specific window is still running.

```d
if (win.isShown())
    writeln("Window is running");
else
    writeln("Window is closed");
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### close

Close a window. The window object stays valid and can be shown again later.

```d
win.close();
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### destroy

Close a window and free all its memory resources. The window object is no longer usable afterwards.

```d
win.destroy();
```

> Call it as a method. The free-function form `destroy(win)` resolves to D's built-in `object.destroy`, which does something else entirely.


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### focus

Bring a window to the front and focus it.

```d
win.focus();
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### minimize

Minimize a WebView window.

```d
win.minimize();
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### maximize

Maximize a WebView window.

```d
win.maximize();
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### exit

Close all open windows and make `wait()` return.

```d
exit();
```

> `exit` is also the name of a C standard library function. If a module imports both `webui` and `core.stdc.stdlib`, qualify the call as `webui.exit()`.


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### clean

Free all WebUI memory resources. Call only at the very end of the program, after `wait()`.

```d
wait();
clean();
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_last_error

Get the last error number and message reported by the C library. Useful when an API returns `false`.

```d
import std.stdio;

if (!win.show("index.html"))
{
    const err = getLastError();
    writefln("Error #%d: %s", err.number, err.message);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### Browser

Supported web browsers.

```d
enum Browser : size_t
{
    noBrowser = 0,   // No browser
    anyBrowser,      // Default recommended browser
    chrome,          // Google Chrome
    firefox,         // Mozilla Firefox
    edge,            // Microsoft Edge
    safari,          // Apple Safari
    chromium,        // The Chromium Project
    opera,           // Opera Browser
    brave,           // Brave Browser
    vivaldi,         // Vivaldi Browser
    epic,            // Epic Browser
    yandex,          // Yandex Browser
    chromiumBased,   // Any Chromium-based browser
    webview,         // WebView (not a web browser)
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### Runtime

Runtimes available for the `.js` and `.ts` files WebUI serves.

```d
enum Runtime : size_t
{
    none = 0,  // No runtime
    deno,      // Deno
    nodeJS,    // Node.js
    bun,       // Bun
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### EventType

Kinds of event received in bound handlers.

```d
enum EventType : size_t
{
    disconnected = 0,  // Browser disconnected
    connected,         // Browser connected
    mouseClick,        // Mouse click on element
    navigation,        // Page navigation (URL in the first argument)
    callback,          // JavaScript function call
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### Config

Library-wide options for `setConfig()`.

```d
enum Config : int
{
    /// Wait for the browser to connect before show() returns. Default: true
    showWaitConnection = 0,

    /// Process all UI events in a single blocking thread. Default: false
    uiEventBlocking,

    /// Auto-refresh the window when files in the root folder change. Default: false
    folderMonitor,

    /// Allow multiple browser clients to connect to the same window,
    /// useful for web apps. Default: false
    multiClient,

    /// Add `webui_auth` cookies to block unauthorized URL access.
    /// Keep true for single-client desktop apps. Default: true
    useCookies,

    /// Make WebUI wait for the backend to call a return method before
    /// answering JavaScript. Default: false
    asynchronousResponse,
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### LoggerLevel

Log levels passed to `setLogger()`.

```d
enum LoggerLevel : size_t
{
    debug_ = 0,  // All logs with full details
    info,        // General informational logs
    error,       // Fatal error logs only
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
