<div align="center">

![Logo](data/webui_120.svg)

# WebUI v2.5 - Python Documentation

> Use any web browser or WebView as GUI, with your preferred language in the backend and modern web technologies in the frontend, all in a lightweight portable library.

![Screenshot](data/screenshot.png)

</div>


## Available APIs

- [Download And Install](#download-and-install)
- [Minimal Example](#minimal-example)

**Window**
- [new_window](#new_window)
- [new_window_id](#new_window_id)
- [get_new_window_id](#get_new_window_id)
- [show](#show)
- [show_browser](#show_browser)
- [show_wv](#show_wv)
- [start_server](#start_server)
- [is_shown](#is_shown)
- [focus](#focus)
- [minimize](#minimize)
- [maximize](#maximize)
- [close](#close)
- [destroy](#destroy)
- [wait](#wait)
- [wait_async](#wait_async)
- [exit](#exit)

**Window Appearance**
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
- [get_hwnd](#get_hwnd)
- [win32_get_hwnd](#win32_get_hwnd)

**Binding & Events**
- [bind](#bind)
- [set_context](#set_context)
- [event](#event)
- [get_context](#get_context)
- [set_event_blocking](#set_event_blocking)

**Reading Event Arguments**
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
- [get_bytes_at](#get_bytes_at)
- [get_bytes](#get_bytes)

**Returning Values to JavaScript**
- [return_int](#return_int)
- [return_float](#return_float)
- [return_string](#return_string)
- [return_bool](#return_bool)

**JavaScript Execution**
- [run](#run)
- [script](#script)
- [send_raw](#send_raw)
- [set_runtime](#set_runtime)

**Multi-Client**
- [show_client](#show_client)
- [close_client](#close_client)
- [send_raw_client](#send_raw_client)
- [navigate_client](#navigate_client)
- [run_client](#run_client)
- [script_client](#script_client)

**Navigation**
- [navigate](#navigate)
- [open_url](#open_url)
- [get_url](#get_url)
- [set_public](#set_public)

**File Serving**
- [set_root_folder](#set_root_folder)
- [set_default_root_folder](#set_default_root_folder)
- [set_file_handler](#set_file_handler)
- [set_file_handler_window](#set_file_handler_window)
- [set_close_handler_wv](#set_close_handler_wv)
- [get_mime_type](#get_mime_type)

**Network & Ports**
- [get_port](#get_port)
- [get_free_port](#get_free_port)
- [set_port](#set_port)

**Browser**
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

**Configuration**
- [set_timeout](#set_timeout)
- [set_config](#set_config)
- [set_tls_certificate](#set_tls_certificate)

**Diagnostics & Logging**
- [set_logger](#set_logger)
- [get_last_error_number](#get_last_error_number)
- [get_last_error_message](#get_last_error_message)
- [is_high_contrast](#is_high_contrast)

**Utilities**
- [encode](#encode)
- [decode](#decode)
- [malloc](#malloc)
- [free](#free)
- [memcpy](#memcpy)

**Other**
- [JavaScript APIs](javascript.md)


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### Download And Install

<p style="text-align: justify;">The WebUI library is written in pure C, but there is a wrapper in every popular programming language to provide easy-to-use APIs to write your backend application, while HTML5 and JavaScript are always used in the frontend inside a web browser or WebView window.</p>

```sh
pip install webui2
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### Minimal Example

Minimal example

```python
from webui import webui

my_window = webui.Window()
my_window.show('<html><script src="webui.js"></script> Hello World from Python! </html>')
webui.wait()
```
[More Python Examples](https://github.com/webui-dev/python-webui/tree/main/examples).


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### new_window

Create a new window object.

```python
from webui import webui

# this way of initialization uses "new_window" internally
my_window = webui.Window()

# More code...

webui.wait()
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### new_window_id

Create a new window object using a specified ID.

> The `id` should be between 1 and 255

```python
from webui import webui

# this way of initialization uses "new_window_id" internally
my_window = webui.Window(1)

# More code...

webui.wait()
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_new_window_id

Get a free window ID that can be used later with `new_window_id` to create a new window object.

```python
from webui import webui

window_id = webui.get_new_window_id()
print(f"Available window ID: {window_id}")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### bind

Bind an HTML element event with a function. Empty element means all events.

```python
from webui import webui

def my_function(event: webui.Event):
    print("Event received:", event)

my_window = webui.Window()
# The bind function returns the bind id.
# Handling this return is optional.
# my_window.bind("myFunction", my_function)
bind_id = my_window.bind("myFunction", my_function)
print(f"Function bound with ID: {bind_id}")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_context

Attach arbitrary user data to a binding so it can be retrieved later inside the event callback via `event.get_context()`. Must be called after `bind()` for the same element.

> Keep a reference to the context object alive for as long as the binding exists, otherwise it may be garbage-collected.

```python
from webui import webui

my_data = {"user": "Alice", "role": "admin"}

def my_handler(e: webui.Event):
    ctx = e.get_context()
    print(f"Context pointer: {ctx}")

my_window = webui.Window()
my_window.bind("myFunction", my_handler)
my_window.set_context("myFunction", my_data)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### event

Every event comes with information about the current event, like id, name, type (_click, connect, disconnect..._) and more.

```python
from webui import webui

def events(e: webui.Event):
    print('Hi!, You clicked on ' + e.element + ' element')

my_window = webui.Window()
# Empty ID means all events on all elements
my_window.bind("", events)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_context

Get user data previously attached with `Window.set_context()`. Returns the context pointer as an integer, or `None` if not set.

```python
from webui import webui

def my_handler(e: webui.Event):
    ctx_ptr = e.get_context()
    print(f"Context pointer: {ctx_ptr}")

my_window = webui.Window()
my_window.bind("myFunction", my_handler)
my_window.set_context("myFunction", {"key": "value"})
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_best_browser

Get the recommended web browser ID to use. If you are already using one, this function will return the same ID.

```python
from webui import webui

my_window = webui.Window()

browser_id = my_window.get_best_browser()
print(f"Recommended browser ID: {browser_id}")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### browser_exist

Check if a specific web browser is installed on the system.

```python
from webui import webui

if webui.browser_exist(webui.Browser.Chrome):
    print("Chrome is installed.")
else:
    print("Chrome is not available.")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### show

Show a window using embedded HTML, or a file. If the window is already open, it will be refreshed.

WebUI will try this pattern:

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

```python
from webui import webui

my_window = webui.Window()

# The "show" function returns true if the window shows
# with zero problem, False if something went wrong.
# success = my_window.show("<html>...</html>")
# success = my_window.show("index.html")
# success = my_window.show("http://example.com")

# Handling the return is optional.
my_window.show("index.html")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### show_browser

Show a window using a specific web browser.

> It's recommended to use `ChromiumBased` browser

> in macOS, the browser's icon may still shown in the Duck icons after exit, therefor we recommend to use `show_wv` instead, this icon issue exist only in macOS.

```python
from webui import webui

my_window = webui.Window()

# success = my_window.show_browser("<html>...</html>", webui.Browser.Chrome)
# success = my_window.show_browser("index.html", webui.Browser.Firefox)

# Handling the return is optional.
my_window.show_browser("index.html", webui.Browser.AnyBrowser)
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

```python
from webui import webui

my_window = webui.Window()

# success = my_window.show_wv("<html>...</html>")
# success = my_window.show_wv("index.html")
# success = my_window.show_wv("http://example.com")

my_window.show_wv("index.html")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### start_server

Start only the web server and return the URL. This is useful for web apps where you want to control the browser separately. No window will be shown.

```python
from webui import webui

my_window = webui.Window()

url = my_window.start_server("/full/root/path")
print(f"Server started at: {url}")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### is_shown

Check if the specified window is still running.

```python
from webui import webui

my_window = webui.Window()

if my_window.is_shown():
    print("The window is still open.")
else:
    print("The window has been closed.")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### focus

Bring the window to the front and give it keyboard focus.

```python
from webui import webui

my_window = webui.Window()

my_window.focus()
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### minimize

Minimize the WebView window.

> Works with WebView windows only.

```python
from webui import webui

my_window = webui.Window()

my_window.minimize()
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### maximize

Maximize the WebView window.

> Works with WebView windows only.

```python
from webui import webui

my_window = webui.Window()

my_window.maximize()
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_kiosk

Set the window in Kiosk mode (_Full screen_).

```python
from webui import webui

my_window = webui.Window()

my_window.set_kiosk(True)    # Enable Kiosk mode
# my_window.set_kiosk(False) # Disable Kiosk mode
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_hide

Set a window in hidden mode.

> Should be called before `show()`.

```python
from webui import webui

my_window = webui.Window()

my_window.set_hide(True)  # Hide the window
my_window.set_hide(False) # Show the window
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_size

Set the window size.

```python
from webui import webui

my_window = webui.Window()

my_window.set_size(800, 600)  # Set window size to 800x600 pixels
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_minimum_size

Set the minimum allowable window size. The user will not be able to resize the window below these dimensions.

```python
from webui import webui

my_window = webui.Window()

my_window.set_minimum_size(400, 300)  # Minimum 400x300 pixels
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_position

Set the window position on the screen.

```python
from webui import webui

my_window = webui.Window()

my_window.set_position(100, 100)  # Move window to (100, 100) on the screen
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_center

Center the window on the screen.

> Works best with WebView. Call before `show()` for best results.

```python
from webui import webui

my_window = webui.Window()

my_window.set_center()
my_window.show_wv("index.html")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_resizable

Control whether the WebView window frame can be resized by the user.

> Works with WebView windows only.

```python
from webui import webui

my_window = webui.Window()

my_window.set_resizable(True)   # Allow resizing (default)
my_window.set_resizable(False)  # Fix the window size
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_frameless

Remove the window title bar and borders (borderless/frameless mode).

> Works with WebView windows only.

```python
from webui import webui

my_window = webui.Window()

my_window.set_frameless(True)   # Remove the title bar and frame
my_window.set_frameless(False)  # Restore the default frame
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_transparent

Make the WebView window background transparent, allowing the underlying desktop to show through areas not covered by the UI.

> Works with WebView windows only.

```python
from webui import webui

my_window = webui.Window()

my_window.set_transparent(True)   # Enable background transparency
my_window.set_transparent(False)  # Disable background transparency
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_high_contrast

Enable or disable high-contrast mode for the window. Use this to apply a high-contrast CSS theme to improve accessibility.

```python
from webui import webui

my_window = webui.Window()

my_window.set_high_contrast(True)   # Enable high-contrast mode
my_window.set_high_contrast(False)  # Disable high-contrast mode
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_icon

Set the default embedded HTML favicon.

```python
from webui import webui

my_window = webui.Window()

my_window.set_icon("<svg>...</svg>", "image/svg+xml")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_root_folder

Set the web-server root folder path for a specific window.

> Should be used before `show()`.

```python
from webui import webui

my_window = webui.Window()

# Handling the return boolean is optional.
success = my_window.set_root_folder("/home/Foo/Bar/")
if success:
    print("Root folder set successfully.")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_default_root_folder

Set the web-server root folder path for all windows.

> Should be used before `show()`.

```python
from webui import webui

# Handling the return boolean is optional.
success = webui.set_default_root_folder("/home/Foo/Bar/")
if success:
    print("Default root folder set successfully.")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_file_handler

Set a custom handler to serve files. The handler receives the requested filename and must return a complete HTTP response (headers + body) as a string, or `None` to let WebUI fall back to its default file serving.

```python
from webui import webui

def my_handler(filename: str):
    if filename == "/hello":
        body = "Hello from Python!"
        return (
            "HTTP/1.1 200 OK\r\n"
            "Content-Type: text/plain\r\n"
            f"Content-Length: {len(body)}\r\n"
            "\r\n"
            + body
        )
    # Return None to let WebUI serve the file normally
    return None

my_window = webui.Window()
my_window.set_file_handler(my_handler)
my_window.show("index.html")
webui.wait()
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_file_handler_window

Like `set_file_handler()`, but the handler also receives the window ID so it can serve different content per window. Setting this overrides any handler previously set with `set_file_handler()`.

```python
from webui import webui

def my_handler(window_id: int, filename: str):
    body = f"Window {window_id} says hello for '{filename}'!"
    return (
        "HTTP/1.1 200 OK\r\n"
        "Content-Type: text/plain\r\n"
        f"Content-Length: {len(body)}\r\n"
        "\r\n"
        + body
    )

my_window = webui.Window()
my_window.set_file_handler_window(my_handler)
my_window.show("index.html")
webui.wait()
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_close_handler_wv

Set a callback to intercept the WebView window close button event. Return `False` from the handler to prevent the window from closing, or `True` to allow it.

> Works with WebView windows only.

```python
from webui import webui

def on_close(window_id: int) -> bool:
    print(f"Window {window_id} is trying to close.")
    return False  # Prevent the window from closing

my_window = webui.Window()
my_window.set_close_handler_wv(on_close)
my_window.show_wv("index.html")
webui.wait()
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_port

Get the network port of a running window. This can be useful to determine the HTTP link of `webui.js`.

```python
from webui import webui

my_window = webui.Window()

port = my_window.get_port()
print(f"WebUI is running on port: {port}")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_free_port

Get an available usable free network port.

```python
from webui import webui

port = webui.get_free_port()
print(f"Available port: {port}")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_port

Set a custom web-server network port to be used by WebUI. This can be useful to determine the HTTP link of `webui.js` in case you are trying to use WebUI with an external web-server like NGINX.

```python
from webui import webui

my_window = webui.Window()

success = my_window.set_port(8080)
if success:
    print("WebUI is now using port 8080.")
else:
    print("Port 8080 is unavailable.")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_public

Allow a specific window address (_URL_) to be accessible from any public network. By default WebUI allow access to the URL of a window only from localhost.

```python
from webui import webui

my_window = webui.Window()

my_window.set_public(True)  # Enable public network access
my_window.set_public(False) # Restrict access to local connections
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_url

Get current URL of a running window.

> By default WebUI allow access to the URL of a window only from localhost.

```python
from webui import webui

my_window = webui.Window()

url = my_window.get_url()
print(f"Current URL: {url}")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### navigate

Navigate to a specific URL. This affects all connected clients.

```python
from webui import webui

my_window = webui.Window()

my_window.navigate("http://domain.com")  # Navigate to the specified URL
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_profile

Set the web browser profile to use. An empty `name` and `path` means the default user profile.

> Need to be called before `show()`.

```python
from webui import webui

my_window = webui.Window()

my_window.set_profile("Bar", "/Home/Foo/Bar")  # Use a custom profile
my_window.set_profile("", "")  # Use the default profile
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_proxy

Set the web browser proxy server to use.

> Need to be called before `show()`.

```python
from webui import webui

my_window = webui.Window()

my_window.set_proxy("http://127.0.0.1:8888")  # Set the proxy server
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_browser_folder

Set a custom folder path where WebUI should look for the browser executable. Useful when the browser is installed in a non-standard location.

```python
from webui import webui

webui.set_browser_folder("/opt/my-browser/")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_custom_parameters

Add user-defined command-line parameters that are passed to the web browser on launch. Useful for enabling developer tools, setting flags, or configuring remote debugging.

```python
from webui import webui

my_window = webui.Window()

my_window.set_custom_parameters("--remote-debugging-port=9222")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_parent_process_id

Get the ID of the parent process (_The web browser may re-create another new process_).

```python
from webui import webui

my_window = webui.Window()

parent_pid = my_window.get_parent_process_id()
print(f"Parent Process ID: {parent_pid}")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_child_process_id

Get the ID of the last child process (_The web browser may re-create other child process_).

```python
from webui import webui

my_window = webui.Window()

child_pid = my_window.get_child_process_id()
print(f"Child Process ID: {child_pid}")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### delete_profile

Delete a specific window web-browser local folder profile.

> It's recommended to be called when program exit, and after all windows are closed.

> [!] This can break functionality of other running windows if using the same web-browser profile.

```python
from webui import webui

my_window = webui.Window()

my_window.delete_profile()  # Delete the browser profile for this window
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### delete_all_profiles

Delete all local web browser profile folders used by WebUI.

> It's recommended to be called when program exit, and after all windows are closed.

> [!] This can break functionality of running windows if using the same web-browser profile.

```python
from webui import webui

webui.wait()
webui.delete_all_profiles()  # Delete all browser profiles
webui.clean()
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_hwnd

Get the native window handle — `HWND` on Windows (works with both WebView and browser windows), `GtkWindow*` on Linux (WebView only).

```python
from webui import webui

my_window = webui.Window()
my_window.show_wv("index.html")

hwnd = my_window.get_hwnd()
if hwnd:
    print(f"Native window handle: {hwnd}")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### win32_get_hwnd

Get the Win32 `HWND` of the window. More reliable than `get_hwnd()` when using WebView, since browser PIDs may change on launch.

> Windows only.

```python
from webui import webui

my_window = webui.Window()
my_window.show_wv("index.html")

hwnd = my_window.win32_get_hwnd()
if hwnd:
    print(f"Win32 HWND: {hwnd}")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### wait

Wait until all opened windows get closed.

```python
from webui import webui

# Other webui function calls / code...

# At the end of the main driver
webui.wait()  # Wait until all windows are closed before continuing
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### wait_async

Non-blocking check that returns `True` while at least one window is still open. Use this instead of `wait()` when you need to run your own main-thread loop (required in WebView mode on some platforms).

```python
from webui import webui

my_window = webui.Window()
my_window.show_wv("index.html")

while webui.wait_async():
    pass  # do main-thread work here
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### close

Close a specific window.

> The `win` object will still alive, and it can be reopen later.

```python
from webui import webui

my_window = webui.Window()

my_window.close()  # Close the current window.
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### destroy

Close a specific window and free all related memory resources.

> This will make the `win` object not available. If you want to simply close a window and reopen it later, please use `close()` instead.

```python
from webui import webui

my_window = webui.Window()

my_window.destroy()  # Close and free resources for the current window.
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### exit

Close all open windows. This will make `wait()` return (_Break_).

```python
from webui import webui

# Other code...

webui.exit()  # Close all WebUI windows and stop waiting
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### clean

Free all memory resources. Should be called only at the end.

```python
from webui import webui

webui.wait()
webui.clean() # Free all WebUI-related resources
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### run

Run JavaScript without waiting for the response. This sends the script to all connected clients.

```python
from webui import webui

my_window = webui.Window()

my_window.run("alert('Hello');")  # Run an alert in the web UI
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### script

Run JavaScript and get the response back. Works in single-client mode.

> The result is available in `result.data`. Check `result.error` to detect execution failures.

```python
from webui import webui

my_window = webui.Window()

result = my_window.script("return 4 + 6;")
if result.error:
    print("JavaScript execution failed.")
else:
    print(f"JavaScript result: {result.data}")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### send_raw

Send raw binary data to a JavaScript function in the UI. The data is broadcast to all connected clients.

```python
from webui import webui

my_window = webui.Window()

# Send bytes to the JavaScript function `myJavaScriptFunc`
my_window.send_raw("myJavaScriptFunc", bytearray([0x01, 0x0A, 0xFF]))
```

On the JavaScript side, the function receives a `Uint8Array`:

```js
function myJavaScriptFunc(data) {
    console.log("Received bytes:", new Uint8Array(data));
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_runtime

Chose between `Deno`, `Bun` and `Nodejs` as runtime for `.js` and `.ts` files.

```python
from webui import webui

my_window = webui.Window()

my_window.set_runtime(webui.Runtime.Bun)  # Use Bun as the JavaScript/TypeScript runtime
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### show_client

Show a window for a **single client** using embedded HTML, a file, or a URL. Called from inside an event callback using the event object. If the window is already open, it will be refreshed.

> Use this in multi-client mode when you want to show different content to individual clients.

```python
from webui import webui

def my_handler(e: webui.Event):
    e.show_client("<html><body>Hello, just you!</body></html>")

my_window = webui.Window()
my_window.bind("showMe", my_handler)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### close_client

Close the connection for a **single client**. Called from inside an event callback using the event object.

```python
from webui import webui

def my_handler(e: webui.Event):
    e.close_client()  # Disconnect this specific client

my_window = webui.Window()
my_window.bind("disconnect", my_handler)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### send_raw_client

Send raw binary data to a JavaScript function for a **single client**. Called from inside an event callback using the event object.

```python
from webui import webui

def my_handler(e: webui.Event):
    e.send_raw_client("myJavaScriptFunc", bytearray([0x01, 0x0A, 0xFF]))

my_window = webui.Window()
my_window.bind("getData", my_handler)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### navigate_client

Navigate a **single client** to a specific URL. Called from inside an event callback using the event object.

```python
from webui import webui

def my_handler(e: webui.Event):
    e.navigate_client("http://domain.com")

my_window = webui.Window()
my_window.bind("goSomewhere", my_handler)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### run_client

Run JavaScript on a **single client** without waiting for a response. Called from inside an event callback using the event object.

```python
from webui import webui

def my_handler(e: webui.Event):
    e.run_client("alert('Hello, just you!');")

my_window = webui.Window()
my_window.bind("greet", my_handler)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### script_client

Run JavaScript on a **single client** and retrieve the response. Called from inside an event callback using the event object.

> The result is available in `result.data`. Check `result.error` to detect execution failures.

```python
from webui import webui

def my_handler(e: webui.Event):
    result = e.script_client("return 4 + 6;", timeout=2)
    if not result.error:
        print(f"Client response: {result.data}")  # Output: "10"

my_window = webui.Window()
my_window.bind("calculate", my_handler)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_count

Get how many arguments there are in an event.

```python
from webui import webui

def callback(e: webui.Event):
    count = e.get_count()
    print(f"The event has {count} arguments.")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_int_at

Get an argument as integer at a specific index.

```python
from webui import webui

def callback(e: webui.Event):
    value = e.get_int_at(0)
    print(f"The integer at index 0 is {value}.")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_int

Get the first argument as integer.

```python
from webui import webui

def callback(e: webui.Event):
    value = e.get_int()
    print(f"The first argument is {value}.")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_float_at

Get an argument as float at a specific index.

```python
from webui import webui

def callback(e: webui.Event):
    value = e.get_float_at(0)
    print(f"The float at index 0 is {value}.")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_float

Get the first argument as float.

```python
from webui import webui

def callback(e: webui.Event):
    value = e.get_float()
    print(f"The first argument as a float is {value}.")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_string_at

Get an argument as string at a specific index.

```python
from webui import webui

def callback(e: webui.Event):
    value = e.get_string_at(0)
    print(f"The string at index 0 is '{value}'.")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_string

Get the first argument as string.

```python
from webui import webui

def callback(e: webui.Event):
    value = e.get_string()
    print(f"The first argument as a string is '{value}'.")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_bool_at

Get an argument as boolean at a specific index.

```python
from webui import webui

def callback(e: webui.Event):
    is_valid = e.get_bool_at(0)
    print(f"The boolean value at index 0 is {is_valid}.")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_bool

Get the first argument as boolean.

```python
from webui import webui

def callback(e: webui.Event):
    is_valid = e.get_bool()
    print(f"The first argument as a boolean is {is_valid}.")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_size_at

Get the size in bytes of an argument at a specific index.

```python
from webui import webui

def callback(e: webui.Event):
    arg_size = e.get_size_at(0)
    print(f"The size of the argument at index 0 is {arg_size} bytes.")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_size

Get size in bytes of the first argument.

```python
from webui import webui

def callback(e: webui.Event):
    arg_size = e.get_size()
    print(f"The size of the first argument is {arg_size} bytes.")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_bytes_at

Get an argument as raw bytes at a specific index. Unlike `get_string_at()`, this is safe for binary data — it reads the exact byte count without assuming null-termination or UTF-8 encoding.

```python
from webui import webui

def callback(e: webui.Event):
    data = e.get_bytes_at(0)
    print(f"Received {len(data)} bytes: {data.hex()}")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_bytes

Get the first argument as raw bytes. Unlike `get_string()`, this is safe for binary data — it reads the exact byte count without assuming null-termination or UTF-8 encoding.

```python
from webui import webui

def callback(e: webui.Event):
    data = e.get_bytes()
    print(f"Received {len(data)} bytes: {data.hex()}")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### return_int

Return the response to JavaScript as integer.

```python
from webui import webui

def callback(e: webui.Event):
    e.return_int(123)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### return_float

Return the response to JavaScript as float.

```python
from webui import webui

def callback(e: webui.Event):
    e.return_float(123.456)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### return_string

Return the response to JavaScript as string.

```python
from webui import webui

def callback(e: webui.Event):
    e.return_string("Response...")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### return_bool

Return the response to JavaScript as boolean.

```python
from webui import webui

def callback(e: webui.Event):
    e.return_bool(True)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_timeout

Set the maximum time in seconds to wait for the window to connect. This effect `show()` and `wait()`.

```python
from webui import webui

# Set a timeout of 30 seconds for window connections
webui.set_timeout(30)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_config

Control and change the WebUI global settings.

> It's recommended to be called at the beginning.

```python
from webui import webui

# Control if `show()`, `show_browser()` and
# `show_wv()` should wait for the window to connect
# before returns or not.
#
# show_wait_connection = 0,

# Control if WebUI should block and process the UI events
# one a time in a single thread `True`, or process every
# event in a new non-blocking thread `False`. This updates
# all windows. You can use `set_event_blocking()` for
# a specific single window update.
#
# ui_event_blocking,

# Automatically refresh the window UI when any file in the
# root folder gets changed.
#
# folder_monitor,

# Allow multiple clients to connect to the same window,
# This is helpful for web apps (non-desktop software),
# Please see the documentation for more details.
#
# multi_client,

# Allow or prevent WebUI from adding `webui_auth` cookies.
# WebUI uses these cookies to identify clients and block
# unauthorized access to the window content using a URL.
#
# use_cookies,

# If the backend uses asynchronous operations, set this
# option to True. This will make WebUI wait until the
# backend sets a response using return_x().
#
# asynchronous_response,

webui.set_config(webui.Config.show_wait_connection, False)  # Disable waiting for connection
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_event_blocking

Control if UI events coming from this window should be processed one at a time in a single blocking thread `True`, or process every event in a new non-blocking thread `False`. This updates a single window. You can use `set_config()` to update all windows.

> Note: If this is set to `True`, the API `script()` won't return any response until this current event is finished.

```python
from webui import webui

my_window = webui.Window()

my_window.set_event_blocking(True)  # Enable blocking event processing
my_window.set_event_blocking(False) # Enable non-blocking event processing
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_tls_certificate

Set the SSL/TLS certificate and the private key content in PEM format. If set empty WebUI will generate a self-signed certificate.

> This works only with the TLS version of WebUI `webui-2-secure`.

```python
from webui import webui

success = webui.set_tls_certificate(
    "-----BEGIN CERTIFICATE-----\n...",
    "-----BEGIN PRIVATE KEY-----\n..."
)

if success:
    print("TLS certificate successfully set.")
else:
    print("Failed to set TLS certificate.")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_logger

Set a custom logger callback to receive WebUI's internal log messages. Useful for debugging or integrating WebUI logs into your own logging system.

```python
from webui import webui

def my_logger(level: int, message: str) -> None:
    level_name = {0: "DEBUG", 1: "INFO", 2: "ERROR"}.get(level, str(level))
    print(f"[WebUI {level_name}] {message}")

webui.set_logger(my_logger)
```

The `level` parameter maps to `webui.LoggerLevel`:

| Value | Constant | Description |
|-------|----------|-------------|
| 0 | `LoggerLevel.DEBUG` | All logs with full details |
| 1 | `LoggerLevel.INFO` | General informational logs |
| 2 | `LoggerLevel.ERROR` | Fatal errors only |


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_last_error_number

Get the last WebUI error code. Useful for diagnosing failures after an API call returns `False`.

```python
from webui import webui

err = webui.get_last_error_number()
print(f"Last error code: {err}")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_last_error_message

Get the last WebUI error message as a human-readable string.

```python
from webui import webui

msg = webui.get_last_error_message()
print(f"Last error: {msg}")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### is_high_contrast

Check if the operating system is currently using a high-contrast theme. Use this to automatically apply an accessible UI theme.

```python
from webui import webui

if webui.is_high_contrast():
    print("High-contrast mode is active.")
else:
    print("Standard display mode.")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### open_url

Open an URL in the native default web browser.

```python
from webui import webui

webui.open_url("https://webui.me")  # Open the WebUI website in the default browser
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_mime_type

Get the HTTP mime type of a file based on its extension.

```python
from webui import webui

mime_type = webui.get_mime_type("foo.png")
print(f"MIME type: {mime_type}")  # Output: image/png
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### encode

Encode text to Base64.

```python
from webui import webui

encoded_string = webui.ui_encode("Foo Bar")
print(f"Base64 Encoded: {encoded_string}")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### decode

Decode a Base64 encoded text.

```python
from webui import webui

decoded_string = webui.ui_decode("SGVsbG8=")
print(f"Decoded String: {decoded_string}")  # Output: Hello
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### malloc

Safely allocate memory using the WebUI memory management system. It can be safely freed using WebUI's `free()` API at any time later.

```python
from webui import webui

buffer = webui.malloc(1024)
if buffer:
    print(f"Memory allocated at address: {buffer}")
    webui.free(buffer)  # Free the allocated memory
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### free

Safely free a buffer allocated by WebUI.

```python
from webui import webui

my_buffer = webui.malloc(1024)

webui.free(my_buffer)  # Free the allocated buffer
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### memcpy

Copy raw memory between two WebUI-managed buffers.

```python
from webui import webui

src  = webui.malloc(64)
dest = webui.malloc(64)

webui.memcpy(dest, src, 64)  # Copy 64 bytes from src to dest

webui.free(src)
webui.free(dest)
```
