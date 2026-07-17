<div align="center">

![Logo](data/webui_120.svg)

# WebUI v2.5 - Elixir Documentation

> Use any web browser or WebView as GUI, with your preferred language in the backend and modern web technologies in the frontend, all in a lightweight portable library.

![Screenshot](data/screenshot.png)

</div>


## Getting Started
- [Download And Install](#download-and-install)
- [Minimal Example](#minimal-example)

## Available APIs
- **Window**
  - [new_window](#new_window)
  - [get_new_window_id](#get_new_window_id)
  - [show](#show)
  - [show_browser](#show_browser)
  - [show_wv](#show_wv)
  - [start_server](#start_server)
  - [shown?](#shown)
  - [focus](#focus)
  - [minimize](#minimize)
  - [maximize](#maximize)
  - [close](#close)
  - [destroy](#destroy)
  - [wait](#wait)
  - [app_running?](#app_running)
  - [exit](#exit)
  - [clean](#clean)
- **Window Appearance**
  - [set_kiosk](#set_kiosk)
  - [set_hide](#set_hide)
  - [set_size](#set_size)
  - [set_minimum_size](#set_minimum_size)
  - [set_position](#set_position)
  - [set_center](#set_center)
  - [set_resizable](#set_resizable)
  - [set_frameless](#set_frameless)
  - [set_transparent](#set_transparent)
  - [set_high_contrast](#set_high_contrast)
  - [set_icon](#set_icon)
- **Binding & Events**
  - [bind](#bind)
  - [event](#event)
  - [returning values](#returning-values-to-javascript)
  - [set_event_blocking](#set_event_blocking)
- **Reading Event Arguments**
  - [get_int](#get_int)
  - [get_float](#get_float)
  - [get_string](#get_string)
  - [get_bool](#get_bool)
  - [get_size](#get_size)
  - [get_raw](#get_raw)
- **JavaScript Execution**
  - [run](#run)
  - [script](#script)
  - [send_raw](#send_raw)
  - [set_runtime](#set_runtime)
- **Per-Client (Multi-Client)**
  - [show_client](#show_client)
  - [close_client](#close_client)
  - [navigate_client](#navigate_client)
  - [run_client](#run_client)
  - [send_raw_client](#send_raw_client)
- **Navigation**
  - [navigate](#navigate)
  - [open_url](#open_url)
  - [get_url](#get_url)
  - [set_public](#set_public)
- **File Serving**
  - [set_root_folder](#set_root_folder)
  - [set_default_root_folder](#set_default_root_folder)
  - [get_mime_type](#get_mime_type)
- **Network & Ports**
  - [get_port](#get_port)
  - [get_free_port](#get_free_port)
  - [set_port](#set_port)
- **Browser & Profile**
  - [get_best_browser](#get_best_browser)
  - [browser_exist](#browser_exist)
  - [set_browser_folder](#set_browser_folder)
  - [set_custom_parameters](#set_custom_parameters)
  - [set_profile](#set_profile)
  - [set_proxy](#set_proxy)
  - [delete_profile](#delete_profile)
  - [delete_all_profiles](#delete_all_profiles)
  - [get_parent_process_id](#get_parent_process_id)
  - [get_child_process_id](#get_child_process_id)
- **Configuration**
  - [set_timeout](#set_timeout)
  - [set_config](#set_config)
- **Diagnostics**
  - [last_error](#last_error)
  - [high_contrast?](#high_contrast)
  - [version](#version)
- **Utilities**
  - [encode](#encode)
  - [decode](#decode)

[JavaScript APIs](javascript.md)


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### Download And Install

<p style="text-align: justify;">The WebUI library is written in pure C, but there is a wrapper in every popular programming language to provide easy-to-use APIs to write your backend application, while HTML5 and JavaScript are always used in the frontend inside a web browser or WebView window.</p>

Unlike most WebUI bindings, the Elixir binding compiles a small C shim, because
Erlang/OTP has no runtime FFI. A C compiler is therefore required: `gcc`/`make`
on Linux, the Xcode command line tools on macOS, or MSVC on Windows (run from a
**Developer Command Prompt**).

Add the dependency to `mix.exs`:

```elixir
def deps do
  [{:webui, github: "webui-dev/elixir-webui"}]
end
```

Then fetch dependencies, download the WebUI native library, and build the NIF:

```sh
mix setup
```

WebUI is linked statically into the NIF, so there is no shared library to ship
or locate at runtime. See the [repository](https://github.com/webui-dev/elixir-webui)
for the full build instructions.


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### Minimal Example

```elixir
win = WebUI.new_window()
WebUI.show(win, ~S(<html><script src="webui.js"></script> Hello World from Elixir! </html>))
WebUI.wait()
WebUI.clean()
```

[More Elixir Examples](https://github.com/webui-dev/elixir-webui/tree/main/examples).

Every page you serve must load the bridge with `<script src="webui.js"></script>`.
WebUI generates that file in memory at runtime; it does not exist on disk. Bound
Elixir functions then appear in the page as async JavaScript functions of the
same name, returning Promises.


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### new_window

Create a new window and return its handle. A window is an integer, not a
process. Pass a specific number (`1..65534`) to choose the handle yourself.

```elixir
win = WebUI.new_window()

# Or with a specific id:
win = WebUI.new_window(123)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_new_window_id

Get the first unused window number, for use with `new_window/1`.

```elixir
id = WebUI.get_new_window_id()
win = WebUI.new_window(id)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### show

Show a window using embedded HTML, a local file, a folder, or a URL. If the
window is already open it is refreshed. Returns whether the window was shown.

By default this waits for the browser to connect before returning; see
[set_config](#set_config) with `:show_wait_connection` to change that.

```elixir
# Embedded HTML
WebUI.show(win, "<html><script src=\"webui.js\"></script> Hello! </html>")

# A local file
WebUI.show(win, "index.html")

# A URL
WebUI.show(win, "https://webui.me")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### show_browser

Same as [show](#show), but using a specific browser. See `WebUI.Browser` for
the available atoms.

```elixir
WebUI.show_browser(win, "<html>...</html>", :chrome)
WebUI.show_browser(win, "index.html", :firefox)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### show_wv

Same as [show](#show), but in a native WebView window instead of a browser.
Required for frameless and transparent windows.

> **Not usable from Elixir on macOS.** WebView mode needs its event loop on the
> process's main thread, which on the BEAM belongs to the VM. Windows and Linux
> are unaffected.

```elixir
WebUI.set_frameless(win, true)
WebUI.set_transparent(win, true)
WebUI.show_wv(win, "<html>...</html>")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### start_server

Start only the local web server and return its URL. No window is opened.

```elixir
url = WebUI.start_server(win, "index.html")
IO.puts("Serving at #{url}")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### shown?

Whether the window is currently shown.

```elixir
if WebUI.shown?(win), do: IO.puts("still open")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### focus

Bring the window to the front and focus it.

```elixir
WebUI.focus(win)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### minimize

Minimize a WebView window.

```elixir
WebUI.minimize(win)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### maximize

Maximize a WebView window.

```elixir
WebUI.maximize(win)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### close

Close the window for all clients. The window object survives and can be shown
again; use [destroy](#destroy) to free it.

```elixir
WebUI.close(win)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### destroy

Close the window, free it, and drop its bound handlers.

```elixir
WebUI.destroy(win)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### wait

Block until every open window is closed. Runs on a dirty scheduler, so it
blocks only the calling process, not the VM.

```elixir
WebUI.wait()
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### app_running?

Whether any window is still running.

```elixir
if WebUI.app_running?(), do: IO.puts("app is alive")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### exit

Close all windows, causing [wait](#wait) to return.

```elixir
WebUI.exit()
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### clean

Free WebUI's resources. Call once, after [wait](#wait).

```elixir
WebUI.wait()
WebUI.clean()
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_kiosk

Full-screen kiosk mode.

```elixir
WebUI.set_kiosk(win, true)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_hide

Start the window hidden. Call before [show](#show).

```elixir
WebUI.set_hide(win, true)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_size

Set the window size in pixels.

```elixir
WebUI.set_size(win, 800, 600)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_minimum_size

Set the minimum window size in pixels.

```elixir
WebUI.set_minimum_size(win, 640, 480)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_position

Set the window position in pixels.

```elixir
WebUI.set_position(win, 100, 100)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_center

Center the window on the screen. Best called before [show](#show).

```elixir
WebUI.set_center(win)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_resizable

Whether a WebView window can be resized.

```elixir
WebUI.set_resizable(win, false)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_frameless

Remove a WebView window's frame.

```elixir
WebUI.set_frameless(win, true)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_transparent

Make a WebView window transparent.

```elixir
WebUI.set_transparent(win, true)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_high_contrast

Enable high-contrast support, for theming with CSS.

```elixir
WebUI.set_high_contrast(win, true)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_icon

Set the favicon. The icon is an SVG/HTML string, the type is a MIME string.

```elixir
WebUI.set_icon(win, "<svg>...</svg>", "image/svg+xml")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### bind

Bind an HTML element or JavaScript function name to an Elixir function, and
return the bind ID. The function receives a [%WebUI.Event{}](#event) and its
return value resolves the JavaScript Promise.

Binding `""` catches every event on the window, which is how connect,
disconnect and navigation events are observed.

```elixir
WebUI.bind(win, "my_button", fn e ->
  name = WebUI.Event.get_string(e, 0)
  "Hello #{name}"
end)

# Catch all events:
WebUI.bind(win, "", fn
  %WebUI.Event{type: :connected} -> IO.puts("connected")
  %WebUI.Event{type: :disconnected} -> IO.puts("disconnected")
  _ -> :ok
end)
```

Handlers run in supervised tasks, so blocking inside one is fine and a crash is
contained (it is logged and the Promise resolves to `""`).


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### event

Every bound function receives a `%WebUI.Event{}` struct describing the event.
Arguments sent from JavaScript are not in the struct; they are fetched on
demand with the `WebUI.Event.get_*` functions.

```elixir
# %WebUI.Event{
#   window:  non_neg_integer(),  # the window handle
#   type:    :disconnected | :connected | :mouse_click | :navigation | :callback,
#   element: binary(),           # the HTML element / function name
#   number:  non_neg_integer(),  # internal event number
#   bind_id: non_neg_integer()   # the bind id
# }

WebUI.bind(win, "", fn e ->
  IO.inspect(e.type)
  :ok
end)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### Returning Values to JavaScript

Unlike the C API, there are no `return_int` / `return_string` functions. A bound
function's **return value** is sent back to resolve the JavaScript Promise.

Responses cross to JavaScript as strings. Numbers, booleans and atoms are
converted for you; `nil` and `:ok` become `""`. Return structured data by
encoding it yourself.

```elixir
WebUI.bind(win, "add", fn e ->
  WebUI.Event.get_int(e, 0) + WebUI.Event.get_int(e, 1)
end)
# In the page:  add(2, 3).then(r => console.log(r))  //=> "5"

WebUI.bind(win, "load_user", fn e ->
  e |> WebUI.Event.get_int(0) |> Users.get!() |> Jason.encode!()
end)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_event_blocking

Process events one at a time for this window only (`true`), or each in its own
task (`false`, the default). See [set_config](#set_config) for the global flag.

> The binding leaves the global `:ui_event_blocking` at `false` so handlers run
> in parallel. Setting it `true` will deadlock any handler that calls
> [script](#script).

```elixir
WebUI.set_event_blocking(win, true)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_int

Read an event argument as an integer, by index (default `0`).

```elixir
WebUI.bind(win, "f", fn e ->
  n = WebUI.Event.get_int(e, 0)
  :ok
end)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_float

Read an event argument as a float, by index (default `0`).

```elixir
x = WebUI.Event.get_float(e, 0)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_string

Read an event argument as a string, by index (default `0`).

```elixir
s = WebUI.Event.get_string(e, 0)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_bool

Read an event argument as a boolean, by index (default `0`).

```elixir
b = WebUI.Event.get_bool(e, 0)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_size

Number of bytes in the argument at an index (default `0`). Useful for raw
payloads.

```elixir
len = WebUI.Event.get_size(e, 0)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_raw

Read an event argument as raw bytes, by index (default `0`). Unlike
[get_string](#get_string) this keeps embedded NUL bytes, so use it for data sent
from JS as a `Uint8Array`.

```elixir
data = WebUI.Event.get_raw(e, 0)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### run

Run JavaScript in every client of the window, without waiting for a result.

```elixir
WebUI.run(win, "document.title = 'Set from Elixir'")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### script

Run JavaScript and wait for the result. Single-client mode only. Returns
`{:ok, result}`, or `{:error, message}` on error or timeout. `return` is
required to get a value back.

Options: `:timeout` (seconds, `0` waits forever) and `:buffer_size` (bytes,
default `8192`; longer results are truncated).

```elixir
{:ok, "42"} = WebUI.script(win, "return 6 * 7;")
{:ok, title} = WebUI.script(win, "return document.title;", timeout: 5)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### send_raw

Send raw bytes to a JavaScript function, which receives them as a `Uint8Array`.

```elixir
WebUI.send_raw(win, "receiveBytes", <<1, 2, 3>>)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_runtime

Choose the runtime used for served `.js`/`.ts` files. See `WebUI.Runtime`:
`:none`, `:deno`, `:nodejs`, `:bun`.

```elixir
WebUI.set_runtime(win, :deno)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### show_client

Refresh only the client that raised the current event.

```elixir
WebUI.bind(win, "reload", fn e ->
  WebUI.Event.show_client(e, "index.html")
end)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### close_client

Close only the client that raised the current event.

```elixir
WebUI.bind(win, "logout", fn e -> WebUI.Event.close_client(e) end)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### navigate_client

Navigate only the client that raised the current event.

```elixir
WebUI.Event.navigate_client(e, "https://webui.me")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### run_client

Run JavaScript in only the client that raised the current event.

```elixir
WebUI.Event.run_client(e, "alert('Hello');")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### send_raw_client

Send raw bytes to a JS function, for only the client that raised the event.

```elixir
WebUI.Event.send_raw_client(e, "receiveBytes", <<1, 2, 3>>)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### navigate

Navigate every client of the window to a URL.

```elixir
WebUI.navigate(win, "https://webui.me")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### open_url

Open a URL in the system's default browser.

```elixir
WebUI.open_url("https://webui.me")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_url

Current URL of a running window. Best called after [show](#show).

```elixir
url = WebUI.get_url(win)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_public

Allow the window to be reached from the public network.

```elixir
WebUI.set_public(win, true)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_root_folder

Set the web server root folder for a window.

```elixir
WebUI.set_root_folder(win, "/path/to/ui")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_default_root_folder

Set the web server root folder for all windows. Call before [show](#show).

```elixir
WebUI.set_default_root_folder("/path/to/ui")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_mime_type

HTTP mime type for a filename.

```elixir
"image/png" = WebUI.get_mime_type("logo.png")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_port

The network port the window is running on.

```elixir
port = WebUI.get_port(win)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_free_port

Any free port on the system.

```elixir
port = WebUI.get_free_port()
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_port

Pin the window to a specific port. Returns whether the port was free.

```elixir
true = WebUI.set_port(win, 8080)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_best_browser

The browser WebUI recommends for a window, as a `WebUI.Browser` atom.

```elixir
:chrome = WebUI.get_best_browser(win)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### browser_exist

Whether a browser is installed.

```elixir
if WebUI.browser_exist(:firefox), do: IO.puts("Firefox available")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_browser_folder

Set a custom browser folder path.

```elixir
WebUI.set_browser_folder("/path/to/browser")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_custom_parameters

Add extra CLI parameters for the browser.

```elixir
WebUI.set_custom_parameters(win, "--remote-debugging-port=9222")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_profile

Set the browser profile. Empty name and path mean the default profile. Call
before [show](#show).

```elixir
WebUI.set_profile(win, "Bar", "/home/Foo/Bar")

# Default profile:
WebUI.set_profile(win)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_proxy

Set a proxy server. Call before [show](#show).

```elixir
WebUI.set_proxy(win, "http://127.0.0.1:8888")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### delete_profile

Delete the window's local browser profile folder. Call after [wait](#wait).

```elixir
WebUI.wait()
WebUI.delete_profile(win)
WebUI.clean()
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### delete_all_profiles

Delete all local browser profile folders. Call after [wait](#wait).

```elixir
WebUI.delete_all_profiles()
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_parent_process_id

OS process ID of the backend, i.e. this application.

```elixir
pid = WebUI.get_parent_process_id(win)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_child_process_id

OS process ID of the browser window.

```elixir
pid = WebUI.get_child_process_id(win)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_timeout

Seconds to wait for a window to connect. `0` waits forever. Affects
[show](#show) and [wait](#wait).

```elixir
WebUI.set_timeout(30)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_config

Set a global behaviour flag. See `WebUI.Config`: `:show_wait_connection`,
`:ui_event_blocking`, `:folder_monitor`, `:multi_client`, `:use_cookies`,
`:asynchronous_response`.

> `:asynchronous_response` is set to `true` when the NIF loads and the binding
> depends on it; do not set it `false`. `:ui_event_blocking` is left `false` so
> handlers run in parallel; setting it `true` deadlocks any handler that calls
> [script](#script).

```elixir
WebUI.set_config(:show_wait_connection, false)
WebUI.set_config(:multi_client, true)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### last_error

The last WebUI error, as `{number, message}`.

```elixir
{code, message} = WebUI.last_error()
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### high_contrast?

Whether the OS is using a high-contrast theme.

```elixir
if WebUI.high_contrast?(), do: IO.puts("high contrast is on")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### version

The version of the WebUI C library the binding was built against.

```elixir
IO.puts(WebUI.version())
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### encode

Base64-encode a string.

```elixir
"SGVsbG8=" = WebUI.encode("Hello")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### decode

Base64-decode a string.

```elixir
"Hello" = WebUI.decode("SGVsbG8=")
```
