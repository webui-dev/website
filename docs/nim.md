<div align="center">

![Logo](data/webui_120.svg)

# WebUI v2.5 - Nim Documentation

> Use any web browser or WebView as GUI, with your preferred language in the backend and modern web technologies in the frontend, all in a lightweight portable library.

![Screenshot](data/screenshot.png)

</div>


## Available APIs

### Setup
- [Download And Install](#download-and-install)
- [Minimal Example](#minimal-example)

### Window Lifecycle
- [newWindow](#new_window)
- [newWindow (with ID)](#new_window_id)
- [getNewWindowId](#get_new_window_id)
- [show](#show)
- [showWv](#show_wv)
- [showClient](#show_client)
- [startServer](#start_server)
- [wait](#wait)
- [waitAsync](#wait_async)
- [close](#close)
- [closeClient](#close_client)
- [destroy](#destroy)
- [exit](#exit)
- [shown](#is_shown)
- [focus](#focus)

### Window Configuration
- [kiosk=](#set_kiosk)
- [setSize](#set_size)
- [setPos](#set_pos)
- [hidden=](#set_hidden)
- [highContrast=](#set_high_contrast_window)
- [eventBlocking=](#set_event_blocking)
- [proxy=](#set_proxy)
- [setProfile](#set_profile)
- [rootFolder=](#set_root_folder)
- [setDefaultRootFolder](#set_default_root_folder)
- [setIcon](#set_icon)
- [public=](#set_public)
- [runtime=](#set_runtime)
- [setFileHandler](#set_file_handler)
- [port](#port)
- [url](#url)
- [navigate](#navigate)
- [navigateClient](#navigate_client)
- [deleteProfile](#delete_profile)
- [setTimeout](#set_timeout)

### Browser & Network
- [getBestBrowser](#get_best_browser)
- [browserExist](#browser_exist)
- [openUrl](#open_url)
- [getFreePort](#get_free_port)
- [setTlsCertificate](#set_tls_certificate)

### Events & Binding
- [Event Object](#event)
- [bind](#bind)
- [webuiCb / bindCb](#webui_cb)
- [getCount](#get_count)
- [getInt / getFloat / getString / getBool / getSize](#get_args)
- [returnInt / returnFloat / returnString / returnBool](#return_values)

### JavaScript Execution
- [script](#script)
- [scriptClient](#script_client)
- [run](#run)
- [runClient](#run_client)
- [sendRaw](#send_raw)
- [sendRawClient](#send_raw_client)

### Application Configuration
- [setConfig](#set_config)
- [isHighContrast](#is_high_contrast)
- [deleteAllProfiles](#delete_all_profiles)
- [clean](#clean)

### Encoding & Utilities
- [encode](#encode)
- [decode](#decode)
- [getMimeType](#get_mime_type)
- [parentProcessId](#parent_process_id)
- [childProcessId](#child_process_id)

### Memory
- [malloc](#malloc)
- [free](#free)

- [JavaScript APIs](javascript.md)


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### Download And Install

<p style="text-align: justify;">The WebUI library is written in pure C, but there is a wrapper in every popular programming language to provide easy-to-use APIs to write your backend application, while HTML5 and JavaScript are always used in the frontend inside a web browser or WebView window.</p>

```sh
nimble install webui
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### Minimal Example

```nim
import webui

let window = newWindow()
window.show("""<html><script src="/webui.js"></script> Hello World! </html>""")
wait()
```

[More Nim Examples](https://github.com/webui-dev/nim-webui/tree/main/examples).


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### new_window

Create a new window object.

```nim
let window = newWindow()

# later...
window.show("index.html")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### new_window_id

Create a new window object using a specified ID.

> The `id` should be between 1 and `WEBUI_MAX_IDS`

```nim
let
  windowId = 1
  window = newWindow(windowId)

# later...
window.show("index.html")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_new_window_id

Get a free window ID that can be used with `newWindow(id)` to create a new window object.

```nim
let
  windowId = getNewWindowId()
  window = newWindow(windowId)

# later...
window.show("index.html")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### show

Show a window using embedded HTML, a local file, or a URL. If the window is already open, it will be refreshed. This refreshes all windows in multi-client mode.

> Always include `<script src="/webui.js"></script>` in your HTML for proper window communication.

WebUI will try this pattern to find a renderer:

- Windows
  - `WebView2Loader.dll` exists? → Use the WebView2 window.
  - Any Chromium-based browser? → Use it (_usually Microsoft Edge_).
  - Any other browser? → Use it (_e.g. Firefox_).
  - All failed? → Open in the default browser.
- Linux
  - `WebKit GTK v3` exists? → Use the WebView GTK window.
  - Any Chromium-based browser? → Use it (_e.g. Chromium_).
  - Any other browser? → Use it (_usually Firefox_).
  - All failed? → Open in the default browser.
- macOS
  - `WebKit` exists? → Use the WebView window (_most cases_).
  - Any Chromium-based browser? → Use it (_e.g. Chrome_).
  - Any other browser? → Use it (_e.g. Firefox_).
  - All failed? → Open in Safari.

> To target a specific browser use `show(window, content, browser)`. To use WebView exclusively use `showWv()`.

```nim
# Show HTML content
window.show("<html><script src=\"/webui.js\"></script>Hello!</html>")

# Show a local file
window.show("index.html")

# Show a URL
window.show("https://mydomain.com")

# Show using a specific browser
window.show("index.html", Chrome)

# Show using the first available browser from a set
window.show("index.html", {Webview, Chrome, Edge})
```

The `WebuiBrowser` enum:

```nim
type WebuiBrowser* = enum
  NoBrowser     ## 0. No web browser
  AnyBrowser    ## 1. Default recommended web browser
  Chrome        ## 2. Google Chrome
  Firefox       ## 3. Mozilla Firefox
  Edge          ## 4. Microsoft Edge
  Safari        ## 5. Apple Safari
  Chromium      ## 6. The Chromium Project
  Opera         ## 7. Opera Browser
  Brave         ## 8. The Brave Browser
  Vivaldi       ## 9. The Vivaldi Browser
  Epic          ## 10. The Epic Browser
  Yandex        ## 11. The Yandex Browser
  ChromiumBased ## 12. Any Chromium-based browser
  Webview       ## 13. WebView (not a web browser)
```

> On macOS, the browser icon may linger in the Dock after exit. Use `showWv()` to avoid this.


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### show_wv

Show a WebView window using embedded HTML, a local file, or a URL. If the window is already open, it will be refreshed.

> WebUI's primary focus is using web browsers as GUI. Use this API when you need a WebView instead of a browser.

> Windows: Requires WebView2 and `WebView2Loader.dll`.
> Linux: Requires WebKit GTK v3.
> macOS: Requires WebKit (_WKWebView_).

```nim
window.showWv("index.html")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### show_client

Show content in the window for a single connected client only. Useful in multi-client mode to target one specific client.

```nim
window.bind("refreshBtn") do (e: Event):
  e.showClient("updated.html")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### start_server

Start the local web server for a window and return the URL without opening a browser. Useful for web apps or embedding WebUI in an external browser or web view.

```nim
let url = window.startServer("/full/path/to/root/folder")
echo "Server running at: ", url
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### wait

Block until all opened windows are closed, or until `exit()` is called.

```nim
# Create windows...
# Bind HTML elements...
# Show the windows...

wait()
```

Full Nim example: <https://github.com/webui-dev/nim-webui/tree/main/examples/minimal.nim>


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### wait_async

Non-blocking variant of `wait()`. Returns `true` while windows are still open. Call this in a loop from the main thread when you need to do work alongside the UI — required in WebView mode.

```nim
while waitAsync():
  # your main-thread work here
  doSomething()
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### close

Close a specific window. The window object remains alive and can be shown again later.

```nim
window.close()
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### close_client

Close the connection for a single client only, without affecting other connected clients.

```nim
window.bind("disconnectBtn") do (e: Event):
  e.closeClient()
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### destroy

Close a specific window and free all associated memory resources. The window object becomes invalid afterwards.

> Use `close()` if you want to reopen the window later. Use `destroy()` only when you are done with it.

```nim
window.destroy()
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### exit

Close all open windows. `wait()` will return.

```nim
exit()
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### is_shown

Check if the specified window is still running.

```nim
if window.shown:
  echo "Window is running."
else:
  echo "Window is closed."
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### focus

Bring a window to the front and focus it.

```nim
window.focus()
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_kiosk

Set the window in Kiosk mode (_full screen_).

```nim
window.kiosk = true
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_size

Set the window size.

```nim
window.setSize(800, 600)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_pos

Set the window position on screen.

```nim
window.setPos(100, 100)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_hidden

Start the window in hidden mode. Call before `show()`.

```nim
window.hidden = true
window.show("index.html")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_high_contrast_window

Enable or disable high-contrast support for a specific window. Useful when building a high-contrast CSS theme.

```nim
window.highContrast = true
```

> To check the OS-level high contrast preference globally, see [`isHighContrast()`](#is_high_contrast).


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_event_blocking

Control whether UI events from this window are processed one at a time in a single blocking thread (`true`), or each in a new non-blocking thread (`false`). Affects only this window.

> To update all windows at once, use `setConfig(ui_event_blocking, ...)`.

```nim
window.eventBlocking = true
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_proxy

Set the web browser proxy server. Must be called before `show()`.

```nim
window.proxy = "http://127.0.0.1:8888"
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_profile

Set the web browser profile to use. An empty `name` and `path` means the default user profile. Must be called before `show()`.

```nim
# Custom profile
window.setProfile("MyProfile", "/home/Foo/profiles/MyProfile")

# Default profile
window.setProfile("", "")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_root_folder

Set the web-server root folder path for a specific window.

> Should be called before `show()`.

```nim
window.rootFolder = "/home/Foo/Bar/"
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_default_root_folder

Set the web-server root folder path for all windows.

> Should be called before `show()`.

```nim
setDefaultRootFolder("/home/Foo/Bar/")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_icon

Set the default embedded HTML favicon.

```nim
# SVG icon
window.setIcon("<svg>...</svg>", "image/svg+xml")

# PNG icon
window.setIcon(pngData, "image/png")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_public

Allow the window address to be accessible from a public network.

```nim
window.public = true
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_runtime

Choose a JavaScript runtime for `.js` and `.ts` files.

```nim
# Use Deno
window.runtime = Deno

# Use Bun
window.runtime = Bun

# Use Node.js
window.runtime = NodeJS

# Disable runtime
window.runtime = None
```

The `WebuiRuntime` enum:

```nim
type WebuiRuntime* = enum
  None   ## 0. No runtime
  Deno   ## 1. Deno
  NodeJS ## 2. Node.js
  Bun    ## 3. Bun
```

Once set, any HTTP request to a `.js` or `.ts` file is handled by that runtime:

```javascript
// JavaScript (frontend):
var xhr = new XMLHttpRequest();
xhr.open('GET', 'script.ts?foo=123', false);
xhr.send(null);
console.log(xhr.responseText);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_file_handler

Set a custom handler to serve files. Return the full HTTP response (headers + body) as a string. Return an empty string to fall back to WebUI's default file serving.

```nim
var count: int

window.setFileHandler() do (filename: string) -> string:
  echo "File: ", filename

  case filename
  of "/test.txt":
    return "HTTP/1.1 200 OK\r\nContent-Type: text/plain\r\n\r\nHello from file handler!"
  of "/dynamic.html":
    inc count
    return fmt"""HTTP/1.1 200 OK\r\nContent-Type: text/html\r\n\r\n
<html>
  <script src="/webui.js"></script>
  Count: {count} <a href="dynamic.html">[Refresh]</a>
</html>"""

  # Return "" to let WebUI serve the file from disk
```

Full Nim example: <https://github.com/webui-dev/nim-webui/tree/main/examples/serve_folder/serve_folder.nim>


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### port

Get or set the network port used by the window's web server. Useful for determining the URL of `webui.js` when integrating with an external server like NGINX.

```nim
# Get the current port
let p = window.port
echo "WebUI is on port: ", p

# Set a custom port
window.port = 8080
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### url

Get the full current URL of a running window.

```nim
let currentUrl = window.url
echo "Current URL: ", currentUrl
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### navigate

Navigate the window to a specific URL. Affects all connected clients.

```nim
window.navigate("http://domain.com")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### navigate_client

Navigate to a specific URL for a single client only.

```nim
window.bind("goBtn") do (e: Event):
  e.navigateClient("http://domain.com")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### delete_profile

Delete the local web-browser profile folder for a specific window.

> Call at the end of your program, after `wait()`.
> This may affect other windows using the same browser profile.

```nim
wait()
window.deleteProfile()
clean()
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_timeout

Set the maximum time in seconds to wait for the window to connect. Affects `show()` and `wait()`. Set to `0` to wait forever.

```nim
setTimeout(30)

window.show("index.html")
wait()
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_best_browser

Get the recommended web browser to use. If `show()` is already running, returns the browser currently in use.

```nim
let browser = window.getBestBrowser()
echo "Best browser: ", browser
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### browser_exist

Check if a specific web browser is installed on the system.

```nim
if browserExist(Chrome):
  echo "Chrome is available"
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### open_url

Open a URL in the system's default web browser (outside of WebUI).

```nim
openUrl("https://webui.me")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_free_port

Get an available and usable free network port.

```nim
let port = getFreePort()
echo "Free port: ", port
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_tls_certificate

Set the SSL/TLS certificate and private key (both in PEM format). Works only with the `webui-2-secure` library build. If left empty, WebUI generates a self-signed certificate.

```nim
let ok = setTlsCertificate(
  "-----BEGIN CERTIFICATE-----\n...",
  "-----BEGIN PRIVATE KEY-----\n..."
)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### event

Every bound function receives an `Event` object that describes what triggered the call.

```nim
#[
Event fields:
  e.window       : Window  -- the window that fired the event
  e.eventType    : WebuiEvent -- type of event
  e.element      : string  -- the HTML element ID
  e.eventNumber  : int     -- internal event number
  e.bindId       : int     -- bind ID
  e.clientId     : int     -- unique client ID
  e.connectionId : int     -- client connection ID
  e.cookies      : string  -- client's full cookies string

WebuiEvent enum:
  WEBUI_EVENTS_DISCONNECTED  ## 0. Window disconnection event
  WEBUI_EVENTS_CONNECTED     ## 1. Window connection event
  WEBUI_EVENTS_MOUSE_CLICK   ## 2. Mouse click event
  WEBUI_EVENTS_NAVIGATION    ## 3. Window navigation event
  WEBUI_EVENTS_CALLBACK      ## 4. Function call event
]#

# Bind all events on all elements (empty element string)
window.bind("") do (e: Event):
  case e.eventType
  of WEBUI_EVENTS_CONNECTED:
    echo "Client connected."
  of WEBUI_EVENTS_DISCONNECTED:
    echo "Client disconnected."
  of WEBUI_EVENTS_MOUSE_CLICK:
    echo "Click on: ", e.element
  of WEBUI_EVENTS_NAVIGATION:
    echo "Navigating to: ", e.element
  of WEBUI_EVENTS_CALLBACK:
    echo "Function called: ", e.element
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### bind

Bind an HTML element or JavaScript function name to a Nim callback. An empty element string binds to all events on the window.

```nim
# Using do notation
window.bind("myButton") do (e: Event):
  echo "Button clicked!"

# Using a named proc
proc onLogin(e: Event) =
  let user = e.getString(0)
  let pass = e.getString(1)
  echo "Login: ", user

window.bind("login", onLogin)

# Bind all window events
window.bind("") do (e: Event):
  echo "Event type: ", e.eventType

# Bind with automatic return value forwarding
# The proc return type determines which returnX() is called automatically
window.bind("getCount") do (e: Event) -> int:
  return 42

window.bind("getLabel") do (e: Event) -> string:
  return "Hello"
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### webui_cb

`webuiCb` is a pragma that generates a WebUI callback wrapper for a regular Nim proc, allowing it to receive typed arguments directly from JavaScript without manually calling `getInt`/`getString`/etc.

`bindCb` is a macro that binds a `webuiCb`-annotated proc to a window element.

Supported argument types: `int`, `float`, `string`, `bool`, `JsonNode`, `Event` (and most C numeric aliases).
Supported return types: `int`, `float`, `string`, `bool`, `void`.

```nim
import webui, json

proc getSystemInfo(query: JsonNode): string {.webuiCb.} =
  echo "Query: ", query.pretty
  return $(%*{"status": "ok", "count": 42})

proc onButtonClick() {.webuiCb.} =
  echo "Button clicked!"

let window = newWindow()
window.bindCb("getInfo", getSystemInfo)
window.bindCb("btnClick", onButtonClick)

window.show("""
  <html>
    <script src="/webui.js"></script>
    <script>
      webui.call('getInfo', '{"key": 1}').then(r => console.log(r));
    </script>
  </html>
""")
wait()
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_count

Get the number of arguments passed in an event.

```nim
window.bind("myFunc") do (e: Event):
  echo "Argument count: ", e.getCount()
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_args

Read arguments passed from JavaScript to a bound callback.

```nim
window.bind("myFunc") do (e: Event):
  # Get by index (0-based)
  let name   = e.getString(0)
  let age    = e.getInt(1)
  let score  = e.getFloat(2)
  let active = e.getBool(3)
  let size   = e.getSize(0)   # byte size of the string at index 0

  # Get first argument (shorthand, no index)
  let first = e.getString()
```

| Proc | JavaScript type |
|---|---|
| `getInt(index)` / `getInt()` | number (integer) |
| `getFloat(index)` / `getFloat()` | number (float) |
| `getString(index)` / `getString()` | string |
| `getBool(index)` / `getBool()` | boolean |
| `getSize(index)` / `getSize()` | byte length of the argument |


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### return_values

Return a value from a Nim callback back to JavaScript.

```nim
window.bind("add") do (e: Event):
  let a = e.getInt(0)
  let b = e.getInt(1)
  e.returnInt(a + b)

window.bind("greet") do (e: Event):
  e.returnString("Hello, " & e.getString(0) & "!")

window.bind("isReady") do (e: Event):
  e.returnBool(true)

window.bind("getScore") do (e: Event):
  e.returnFloat(98.6)
```

In JavaScript, the returned value is resolved from the `webui.call()` promise:

```javascript
const result = await webui.call("add", 3, 4);
console.log(result); // "7"
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### script

Run JavaScript in the window and wait for the response. Works in single-client mode.

```nim
window.bind("calculate") do (e: Event):
  let res = e.window.script("return 2 * 21;")

  if res.error:
    echo "JS Error: ", res.data
  else:
    echo "Result: ", res.data  # "42"

# Optional parameters:
# timeout  - execution timeout in seconds (0 = no timeout)
# bufferLen - response buffer size in bytes (default: 8192)
let res = window.script("return document.title;", timeout = 5, bufferLen = 1024)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### script_client

Same as `script()`, but targets a single connected client.

```nim
window.bind("getInput") do (e: Event):
  let res = e.scriptClient("return document.getElementById('box').value;")

  if not res.error:
    echo "Input value: ", res.data
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### run

Run JavaScript without waiting for a response. Sent to all connected clients.

```nim
# Fire and forget
window.run("alert('Hello from Nim!')")
window.run("document.getElementById('status').innerText = 'Ready'")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### run_client

Run JavaScript without waiting for a response. Targets a single connected client.

```nim
window.bind("notify") do (e: Event):
  e.runClient("showToast('Message received!')")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### send_raw

Send raw binary data to a JavaScript function. Targets all connected clients.

```nim
var data = [byte 0x01, 0x02, 0x03, 0x04]
window.sendRaw("onBinaryData", addr data[0], uint data.len)
```

In JavaScript:

```javascript
function onBinaryData(data) {
  const view = new Uint8Array(data);
  console.log(view);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### send_raw_client

Send raw binary data to a JavaScript function for a single client only.

```nim
window.bind("requestData") do (e: Event):
  var payload = [byte 0xDE, 0xAD, 0xBE, 0xEF]
  e.sendRawClient("onData", addr payload[0], uint payload.len)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_config

Control WebUI's global behaviour. Best called at the start of your program before any windows are shown.

```nim
# Single option
setConfig(show_wait_connection, false)

# Multiple options at once
setConfig([multi_client, folder_monitor], true)
```

The `WebuiConfig` enum:

```nim
type WebuiConfig* = enum
  show_wait_connection  ## Wait for browser to connect before show() returns (default: true)
  ui_event_blocking     ## Process all UI events in a single blocking thread (default: false)
  folder_monitor        ## Auto-refresh window when root folder files change (default: false)
  multi_client          ## Allow multiple clients per window, for web apps (default: false)
  use_cookies           ## Use auth cookies to restrict window access (default: true)
  asynchronous_response ## Wait for backend to call returnX() before responding (default: false)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### is_high_contrast

Get the OS-level high contrast preference.

```nim
if isHighContrast():
  echo "OS is using a high contrast theme."
```

> To enable high contrast support for a specific window, see [`highContrast=`](#set_high_contrast_window).


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### delete_all_profiles

Delete all local web-browser profile folders. Call at the end of your program.

```nim
wait()
deleteAllProfiles()
clean()
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### clean

Free all memory resources allocated by WebUI. Call only at the very end of your program.

```nim
wait()
clean()
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### encode

Encode a string to Base64. Returns an empty string on failure.

```nim
let encoded = encode("Hello, World!")
echo encoded  # "SGVsbG8sIFdvcmxkIQ=="
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### decode

Decode a Base64-encoded string. Returns an empty string on failure.

```nim
let original = decode("SGVsbG8sIFdvcmxkIQ==")
echo original  # "Hello, World!"
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_mime_type

Get the HTTP MIME type string for a file based on its extension.

```nim
echo getMimeType("image.png")   # "image/png"
echo getMimeType("script.js")   # "application/javascript"
echo getMimeType("style.css")   # "text/css"
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### parent_process_id

Get the ID of the parent process (your backend application).

```nim
let pid = window.parentProcessId()
echo "Backend PID: ", pid
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### child_process_id

Get the ID of the child process spawned by WebUI (the web browser window).

> In WebView mode, returns the parent process ID since both run in the same process.

```nim
let pid = window.childProcessId()
echo "Browser PID: ", pid
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### malloc

Safely allocate memory using WebUI's memory management system. The buffer can be freed with `webui_free()` at any time.

```nim
from webui/bindings import nil

let buffer = bindings.malloc(csize_t 1024)
# use buffer...
bindings.free(buffer)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### free

Safely free a buffer that was allocated by WebUI's `malloc`.

```nim
from webui/bindings import nil

let buffer = bindings.malloc(csize_t 1024)
# use buffer...
bindings.free(buffer)
```
