<div align="center">

![Logo](data/webui_120.svg)

# WebUI v2.5 - PureBasic Documentation

> Use any web browser or WebView as GUI, with your preferred language in the backend and modern web technologies in the frontend, all in a lightweight portable library.

![Screenshot](data/screenshot.png)

</div>


## Getting Started
- [Download And Install](#download-and-install)
- [Minimal Example](#minimal-example)

## Available APIs
- **Window Management**
  - [new_window](#new_window)
  - [new_window_id](#new_window_id)
  - [get_new_window_id](#get_new_window_id)
  - [show](#show)
  - [show_browser](#show_browser)
  - [show_wv](#show_wv)
  - [show_client](#show_client)
  - [start_server](#start_server)
  - [close](#close)
  - [close_client](#close_client)
  - [destroy](#destroy)
  - [exit](#exit)
  - [wait](#wait)
  - [wait_async](#wait_async)
  - [is_shown](#is_shown)
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
  - [set_icon](#set_icon)
  - [set_high_contrast](#set_high_contrast)
  - [is_high_contrast](#is_high_contrast)
  - [set_custom_parameters](#set_custom_parameters)
  - [set_close_handler_wv](#set_close_handler_wv)
- **Navigation**
  - [navigate](#navigate)
  - [navigate_client](#navigate_client)
  - [get_url](#get_url)
  - [open_url](#open_url)
  - [set_public](#set_public)
- **Events & Binding**
  - [bind](#bind)
  - [event](#event)
  - [set_context](#set_context)
  - [get_context](#get_context)
  - [set_event_blocking](#set_event_blocking)
  - [set_config](#set_config)
  - [set_timeout](#set_timeout)
- **JavaScript**
  - [run](#run)
  - [run_client](#run_client)
  - [script](#script)
  - [script_client](#script_client)
  - [set_runtime](#set_runtime)
- **Event Arguments**
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
- **Event Response**
  - [return_int](#return_int)
  - [return_float](#return_float)
  - [return_string](#return_string)
  - [return_bool](#return_bool)
- **Raw Data**
  - [send_raw](#send_raw)
  - [send_raw_client](#send_raw_client)
- **File Serving**
  - [set_file_handler](#set_file_handler)
  - [set_file_handler_window](#set_file_handler_window)
  - [set_root_folder](#set_root_folder)
  - [set_default_root_folder](#set_default_root_folder)
  - [get_mime_type](#get_mime_type)
- **Browser & Profile**
  - [get_best_browser](#get_best_browser)
  - [browser_exist](#browser_exist)
  - [set_profile](#set_profile)
  - [set_proxy](#set_proxy)
  - [set_browser_folder](#set_browser_folder)
  - [delete_profile](#delete_profile)
  - [delete_all_profiles](#delete_all_profiles)
- **Network & Ports**
  - [get_port](#get_port)
  - [set_port](#set_port)
  - [get_free_port](#get_free_port)
- **Process & System**
  - [get_parent_process_id](#get_parent_process_id)
  - [get_child_process_id](#get_child_process_id)
  - [get_hwnd](#get_hwnd)
  - [win32_get_hwnd](#win32_get_hwnd)
- **Memory Management**
  - [malloc](#malloc)
  - [free](#free)
  - [memcpy](#memcpy)
  - [encode](#encode)
  - [decode](#decode)
- **Security**
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

<p style="text-align: justify;">The WebUI library is written in pure C, but there is a PureBasic wrapper that provides easy-to-use APIs to write your backend application, while HTML5 and JavaScript are always used in the frontend inside a web browser or WebView window.</p>

You can [download](https://github.com/webui-dev/webui/releases) the latest pre-release binaries from the GitHub release section, or follow those instructions to build WebUI from the source.

```sh
# Clone
git clone https://github.com/webui-dev/webui.git
cd webui

# Windows - MSVC
nmake

# Windows - MinGW
mingw32-make

# Linux - GCC
make

# Linux Clang
make CC=clang

# macOS Clang
make
```
The compiled binaries will be generated in `webui/dist` folder.<br>
More details [here](https://github.com/webui-dev/webui#build).

Copy `webui.pb` and the appropriate library file for your platform into your project:

| Platform | Library File |
|----------|-------------|
| Windows  | `webui-2-x64.lib` or `webui-2-static-x64.lib` |
| Linux    | `webui-2-x64.so` |
| macOS    | `webui-2-static-x64.a` |

> **Important:** PureBasic must be compiled with **Thread Safe** mode enabled (`Compiler Options > Thread Safe`).


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### Minimal Example

```basic
XIncludeFile "webui.pb"

Define window.i

window = webui_new_window()
webui_show(window, "<html><script src=""webui.js""></script><body>Hello World from PureBasic!</body></html>")
webui_wait()
```
[More PureBasic Examples](https://github.com/webui-dev/webui/tree/main/examples/purebasic).


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### new_window

Create a new window object. Returns a window number used with all other WebUI APIs.

```basic
XIncludeFile "webui.pb"

Define window.i

window = webui_new_window()

; Later...
webui_show(window, "index.html")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### new_window_id

Create a new window object using a specified ID.

> The `id` should be between 1 and `#WEBUI_MAX_IDS`

```basic
XIncludeFile "webui.pb"

Define window.i

#MyWindowID = 1
window = webui_new_window_id(#MyWindowID)

; Later...
webui_show(window, "index.html")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_new_window_id

Get a free window ID that can be used with `webui_new_window_id()`.

```basic
XIncludeFile "webui.pb"

Define id.i, window.i

id = webui_get_new_window_id()

; Later...
window = webui_new_window_id(id)
webui_show(window, "index.html")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### show

Show a window using embedded HTML, a URL, a local file, or a local folder. If the window is already open, it will be refreshed.

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

> To use only a specific browser please use `webui_show_browser()`

> To use only WebView please use `webui_show_wv()`

```basic
XIncludeFile "webui.pb"

Define window.i

;
; @param content The HTML, URL, local file, or local folder.
;                Leave empty to use the current root folder.
;

window = webui_new_window()

; Show inline HTML
webui_show(window, "<html><script src=""webui.js""></script><body>Hello!</body></html>")

; Show a local file
; webui_show(window, "index.html")

; Show a URL
; webui_show(window, "http://localhost:8080")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### show_browser

Show a window using a specific web browser.

> It's recommended to use `#ChromiumBased` browser.

> On macOS, the browser's icon may still appear in the Dock after exit. We recommend using `webui_show_wv()` on macOS to avoid this.

```basic
XIncludeFile "webui.pb"

Define window.i

;
;   Enumeration webui_browsers
;     #NoBrowser     ; 0. No web browser
;     #AnyBrowser    ; 1. Default recommended web browser
;     #Chrome        ; 2. Google Chrome
;     #Firefox       ; 3. Mozilla Firefox
;     #Edge          ; 4. Microsoft Edge
;     #Safari        ; 5. Apple Safari
;     #Chromium      ; 6. The Chromium Project
;     #Opera         ; 7. Opera Browser
;     #Brave         ; 8. The Brave Browser
;     #Vivaldi       ; 9. The Vivaldi Browser
;     #Epic          ; 10. The Epic Browser
;     #Yandex        ; 11. The Yandex Browser
;     #ChromiumBased ; 12. Any Chromium based browser
;     #Webview       ; 13. WebView (Non-web-browser)
;   EndEnumeration
;

;
; @param content The HTML, URL, local file, or local folder
; @param browser The web browser to be used
;

window = webui_new_window()
webui_show_browser(window, "index.html", #Chrome)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### show_wv

Show a **WebView** window using embedded HTML, a URL, or a local file. If the window is already open, it will be refreshed.

> WebUI's primary focus is using web browsers as GUI, but if you need to use WebView instead of a web browser, then you can use this API, which was added to WebUI starting from v2.5.

> Windows Dependencies: WebView2, and `WebView2Loader.dll`.
>
> Linux Dependencies: WebKit GTK v3.
>
> macOS Dependencies: WebKit (_WKWebView_).

```basic
XIncludeFile "webui.pb"

Define window.i

;
; @param content The HTML, URL, or a local file
;

window = webui_new_window()
webui_show_wv(window, "index.html")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### show_client

Show a window for a specific single client. Useful in multi-client mode to send different content to different connected clients.

```basic
XIncludeFile "webui.pb"

ProcedureC myCallback(*e.webui_event_t)

    ;
    ; @param content The HTML, URL, or a local file
    ;

    webui_show_client(*e, "<html>...</html>")
EndProcedure
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### start_server

Start only the local web server without opening a browser window, and return the URL. Useful for headless web app scenarios.

```basic
XIncludeFile "webui.pb"

Define window.i, *url

;
; @param content The local file or folder to serve.
;                Leave empty to use the current root folder.
;

window = webui_new_window()
*url = webui_start_server(window, "/full/root/path")
Debug "Server URL: " + PeekS(*url, -1, #PB_Ascii)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### close

Close a specific window. The window object will still exist and can be shown again later.

```basic
XIncludeFile "webui.pb"

Define window.i

window = webui_new_window()
webui_show(window, "index.html")

; Later...
webui_close(window)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### close_client

Close the connection for a specific single client only, without closing the window for other connected clients.

```basic
XIncludeFile "webui.pb"

ProcedureC myCallback(*e.webui_event_t)

    webui_close_client(*e)
EndProcedure
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### destroy

Close a specific window and free all related memory resources.

> This will make the window object unavailable. If you want to simply close a window and reopen it later, please use `webui_close()` instead.

```basic
XIncludeFile "webui.pb"

Define window.i

window = webui_new_window()
webui_show(window, "index.html")
webui_wait()
webui_destroy(window)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### exit

Close all open windows. This will make `webui_wait()` return (_Break_).

```basic
XIncludeFile "webui.pb"

ProcedureC myCallback(*e.webui_event_t)

    webui_exit()
EndProcedure

Define window.i

window = webui_new_window()
webui_bind(window, "exitButton", @myCallback())
webui_show(window, "index.html")
webui_wait()
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### wait

Block the current thread and wait until all opened windows get closed.

```basic
XIncludeFile "webui.pb"

Define window.i

window = webui_new_window()
webui_show(window, "index.html")
webui_wait()
```

Full PureBasic Example: https://github.com/webui-dev/webui/tree/main/examples/purebasic/minimal


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### wait_async

Non-blocking alternative to `webui_wait()`. Returns `#True` if at least one window is still open, `#False` when all windows are closed. Call this in a loop from the main thread to interleave your own main-thread work.

> In WebView mode, you must call this from the main thread.

```basic
XIncludeFile "webui.pb"

Define window.i

window = webui_new_window()
webui_show_wv(window, "index.html")

While webui_wait_async()
    ; Your main thread code here
    ; This loop runs while windows are open
Wend
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### is_shown

Check if the specified window is still running.

```basic
XIncludeFile "webui.pb"

Define window.i

window = webui_new_window()
webui_show(window, "index.html")

If webui_is_shown(window)
    Debug "Window is shown."
Else
    Debug "Window is closed."
EndIf
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### focus

Bring a window to the front and give it keyboard focus.

```basic
XIncludeFile "webui.pb"

Define window.i

window = webui_new_window()
webui_show(window, "index.html")
webui_focus(window)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### minimize

Minimize a WebView window.

```basic
XIncludeFile "webui.pb"

Define window.i

window = webui_new_window()
webui_show_wv(window, "index.html")
webui_minimize(window)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### maximize

Maximize a WebView window.

```basic
XIncludeFile "webui.pb"

Define window.i

window = webui_new_window()
webui_show_wv(window, "index.html")
webui_maximize(window)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_kiosk

Set the window in Kiosk mode (_Full screen_).

```basic
XIncludeFile "webui.pb"

Define window.i

;
; @param status #True or #False
;

window = webui_new_window()
webui_set_kiosk(window, #True)
webui_show(window, "index.html")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_size

Set the window size.

```basic
XIncludeFile "webui.pb"

Define window.i

;
; @param width  The window width
; @param height The window height
;

window = webui_new_window()
webui_set_size(window, 800, 600)
webui_show(window, "index.html")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_minimum_size

Set the minimum allowed window size. The user will not be able to resize the window smaller than this.

> Currently works on Windows only.

```basic
XIncludeFile "webui.pb"

Define window.i

;
; @param width  The minimum window width
; @param height The minimum window height
;

window = webui_new_window()
webui_set_minimum_size(window, 400, 300)
webui_show(window, "index.html")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_position

Set the window position on screen.

```basic
XIncludeFile "webui.pb"

Define window.i

;
; @param x The window X coordinate
; @param y The window Y coordinate
;

window = webui_new_window()
webui_set_position(window, 100, 100)
webui_show(window, "index.html")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_center

Center the window on the screen.

> Call this before `webui_show()` for best results. Works better with WebView.

```basic
XIncludeFile "webui.pb"

Define window.i

window = webui_new_window()
webui_set_center(window)
webui_show(window, "index.html")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_hide

Start the window in hidden mode. The window will be running but not visible.

> Should be called before `webui_show()`.

```basic
XIncludeFile "webui.pb"

Define window.i

;
; @param status #True or #False
;

window = webui_new_window()
webui_set_hide(window, #True)
webui_show(window, "index.html")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_frameless

Remove the window frame and title bar (borderless/frameless mode).

> Works with WebView windows only.

```basic
XIncludeFile "webui.pb"

Define window.i

;
; @param status #True or #False
;

window = webui_new_window()
webui_set_frameless(window, #True)
webui_show_wv(window, "index.html")
```

Full PureBasic Example: https://github.com/webui-dev/webui/tree/main/examples/purebasic/frameless


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_transparent

Enable or disable window background transparency.

> Works with WebView windows only. The web content must also use a transparent background for this to be visible.

```basic
XIncludeFile "webui.pb"

Define window.i

;
; @param status #True or #False
;

window = webui_new_window()
webui_set_transparent(window, #True)
webui_show_wv(window, "index.html")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_resizable

Control whether the user can resize the window.

> Works with WebView windows only.

```basic
XIncludeFile "webui.pb"

Define window.i

;
; @param status #True to allow resizing, #False to fix the size
;

window = webui_new_window()
webui_set_resizable(window, #False)
webui_show_wv(window, "index.html")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_icon

Set the default embedded HTML favicon. The icon is served automatically by WebUI's built-in server.

```basic
XIncludeFile "webui.pb"

Define window.i

;
; @param icon      The icon as string: "<svg>...</svg>"
; @param icon_type The icon MIME type: "image/svg+xml"
;

window = webui_new_window()
webui_set_icon(window, "<svg>...</svg>", "image/svg+xml")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_high_contrast

Mark the window as supporting high-contrast mode. Use this together with CSS to build a better high-contrast theme.

```basic
XIncludeFile "webui.pb"

Define window.i

;
; @param status #True or #False
;

window = webui_new_window()
webui_set_high_contrast(window, #True)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### is_high_contrast

Check if the operating system is currently using a high-contrast theme.

```basic
XIncludeFile "webui.pb"

Define window.i

window = webui_new_window()

If webui_is_high_contrast()
    ; OS is using a high-contrast theme
    webui_set_high_contrast(window, #True)
EndIf
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_custom_parameters

Add custom command-line parameters to the browser launch command.

```basic
XIncludeFile "webui.pb"

Define window.i

;
; @param params Command line parameters string
;

window = webui_new_window()
webui_set_custom_parameters(window, "--remote-debugging-port=9222")
webui_show(window, "index.html")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_close_handler_wv

Set a callback to intercept the close event of a WebView window. Return `#False` from the handler to prevent the window from closing; return `#True` to allow it.

```basic
XIncludeFile "webui.pb"

ProcedureC.i myCloseHandler(window.i)
    Debug "User tried to close window " + Str(window)
    ProcedureReturn #False ; Prevent closing
EndProcedure

Define window.i

;
; @param close_handler The handler function (receives window ID, returns #True/#False)
;

window = webui_new_window()
webui_set_close_handler_wv(window, @myCloseHandler())
webui_show_wv(window, "index.html")
webui_wait()
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### navigate

Navigate all connected clients of a window to a specific URL.

```basic
XIncludeFile "webui.pb"

Define window.i

;
; @param url Full HTTP URL or relative path
;

window = webui_new_window()
webui_show(window, "index.html")
webui_navigate(window, "/foo/bar.html")

; [!] Note:
;
; If "bar.html" does not include "webui.js" then
; WebUI will try to close the window and webui_wait()
; will break. It's important to include "webui.js"
; in every HTML page.
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### navigate_client

Navigate a specific single client to a URL, without affecting other connected clients.

```basic
XIncludeFile "webui.pb"

ProcedureC myCallback(*e.webui_event_t)

    ;
    ; @param url Full HTTP URL or relative path
    ;

    webui_navigate_client(*e, "/other-page.html")
EndProcedure
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_url

Get the current URL of a running window's web server.

> By default, WebUI only allows access to this URL from localhost.

```basic
XIncludeFile "webui.pb"

Define window.i, *url

window = webui_new_window()
webui_show(window, "index.html")

*url = webui_get_url(window)
Debug "Window URL: " + PeekS(*url, -1, #PB_Ascii)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### open_url

Open a URL in the operating system's default web browser (not a WebUI window).

```basic
XIncludeFile "webui.pb"

webui_open_url("https://webui.me")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_public

Allow the window's web server URL to be accessible from external devices on the network.

```basic
XIncludeFile "webui.pb"

Define window.i

;
; @param status #True or #False
;

window = webui_new_window()
webui_set_public(window, #True)

; Now, the URL of the window is accessible
; from any device/mobile on the network.
webui_show(window, "index.html")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### bind

Bind an HTML element click event or a JavaScript function call to a PureBasic callback. Use an empty element name to catch all events from the window.

> Callback procedures must be declared with `ProcedureC` (C calling convention) and receive a `*e.webui_event_t` pointer.

```basic
XIncludeFile "webui.pb"

ProcedureC myFunction(*e.webui_event_t)
    ; Called from JavaScript: myFunction("hello", 42);
    Debug "myFunction called!"
EndProcedure

ProcedureC greet(*e.webui_event_t)
    Protected *str
    *str = webui_get_string(*e)
    Debug "Hello, " + PeekS(*str, -1, #PB_Ascii) + "!"
EndProcedure

Define window.i

;
; @param element The HTML element ID or JavaScript function name
; @param func    The callback procedure address (@MyProc())
;

window = webui_new_window()
webui_bind(window, "myFunction", @myFunction())
webui_bind(window, "greet", @greet())
webui_show(window, "index.html")
webui_wait()
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### event

Every callback receives a `*e.webui_event_t` pointer with information about the current event.

```basic
XIncludeFile "webui.pb"

ProcedureC events(*e.webui_event_t)

    Select *e\event_type
        Case #WEBUI_EVENT_CONNECTED
            Debug "Connected."
        Case #WEBUI_EVENT_DISCONNECTED
            Debug "Disconnected."
        Case #WEBUI_EVENT_MOUSE_CLICK
            Debug "Click on: " + PeekS(*e\element, -1, #PB_Ascii)
        Case #WEBUI_EVENT_NAVIGATION
            Protected *url
            *url = webui_get_string(*e)
            Debug "Navigating to: " + PeekS(*url, -1, #PB_Ascii)
        Case #WEBUI_EVENT_CALLBACK
            Debug "Function call."
    EndSelect

    ; Available event fields:
    ; *e\window        - Window number
    ; *e\event_type    - Event type (see webui_events enum)
    ; *e\element       - Pointer to HTML element ID string
    ; *e\event_number  - Internal event number
    ; *e\bind_id       - Bind ID
    ; *e\client_id     - Client's unique ID
    ; *e\connection_id - Client's connection ID
    ; *e\cookies       - Pointer to client's cookies string
EndProcedure

Define window.i

window = webui_new_window()
webui_bind(window, "", @events()) ; Catch all window events
webui_show(window, "index.html")
webui_wait()
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_context

Attach arbitrary user data to a specific binding. The data can be retrieved later inside the callback using `webui_get_context()`. Must be called after `webui_bind()`.

```basic
XIncludeFile "webui.pb"

Structure UserInfo
    id.i
    name.s
EndStructure

ProcedureC myFunction(*e.webui_event_t)
    Protected *info.UserInfo
    *info = webui_get_context(*e)
    Debug "User: " + *info\name + " (ID: " + Str(*info\id) + ")"
EndProcedure

Define window.i
Define user.UserInfo

user\id   = 42
user\name = "Alice"

;
; @param element The HTML element / JavaScript function name
; @param context Any user data pointer
;

window = webui_new_window()
webui_bind(window, "myFunction", @myFunction())
webui_set_context(window, "myFunction", @user)
webui_show(window, "index.html")
webui_wait()
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_context

Retrieve the user data pointer previously attached to a binding with `webui_set_context()`.

```basic
XIncludeFile "webui.pb"

ProcedureC myFunction(*e.webui_event_t)
    Protected *data
    *data = webui_get_context(*e)
    ; Cast *data to the appropriate structure pointer type
EndProcedure
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_event_blocking

Control whether UI events from this window are processed one at a time in a single blocking thread (`#True`), or each in a new non-blocking thread (`#False`). Use `webui_set_config(#ui_event_blocking, ...)` to apply to all windows.

```basic
XIncludeFile "webui.pb"

ProcedureC foo(*e.webui_event_t) : EndProcedure ; long task
ProcedureC bar(*e.webui_event_t) : EndProcedure ; runs after foo

Define window.i

;
; @param status #True for blocking (serial), #False for non-blocking (parallel)
;

window = webui_new_window()
webui_bind(window, "foo", @foo())
webui_bind(window, "bar", @bar())
webui_set_event_blocking(window, #True)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_config

Control WebUI global behaviour. It's recommended to call this at the beginning, before `webui_show()`.

```basic
XIncludeFile "webui.pb"

;
;   Enumeration webui_config
;     #show_wait_connection   ; Wait for browser to connect before show() returns. Default: #True
;     #ui_event_blocking      ; Process all UI events in a single blocking thread. Default: #False
;     #folder_monitor         ; Auto-refresh the window when any file in root folder changes. Default: #False
;     #multi_client           ; Allow multiple browser clients to connect to same window. Default: #False
;     #use_cookies            ; Use WebUI auth cookies to identify clients. Default: #True
;     #asynchronous_response  ; Set #True if backend uses async ops with return_x(). Default: #False
;   EndEnumeration
;

;
; @param option The desired option from webui_config enumeration
; @param status #True or #False
;

webui_set_config(#show_wait_connection,  #True)
webui_set_config(#ui_event_blocking,     #False)
webui_set_config(#folder_monitor,        #True)
webui_set_config(#multi_client,          #False)
webui_set_config(#use_cookies,           #True)
webui_set_config(#asynchronous_response, #False)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_timeout

Set the maximum time in seconds to wait for the browser window to connect after calling `webui_show()`. A value of `0` means wait forever.

```basic
XIncludeFile "webui.pb"

Define window.i

;
; @param second The timeout in seconds (0 = no timeout)
;

webui_set_timeout(30)

window = webui_new_window()
webui_show(window, "index.html")
webui_wait()
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### run

Run JavaScript in the window without waiting for a response. Sends to all connected clients.

```basic
XIncludeFile "webui.pb"

ProcedureC myCallback(*e.webui_event_t)

    ; Run JavaScript for one specific client only
    webui_run_client(*e, "alert('Hello from backend!');")
EndProcedure

Define window.i

;
; @param script The JavaScript to be run
;

window = webui_new_window()
webui_bind(window, "myCallback", @myCallback())
webui_show(window, "index.html")

; Run JavaScript for all connected clients
webui_run(window, "alert('Hello from backend!');")
webui_wait()
```

Full PureBasic Example: https://github.com/webui-dev/purebasic-webui/tree/main/examples


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### run_client

Run JavaScript for a specific single client without waiting for a response.

```basic
XIncludeFile "webui.pb"

ProcedureC myCallback(*e.webui_event_t)

    ;
    ; @param script The JavaScript to be run
    ;

    webui_run_client(*e, "document.title = 'Updated';")
EndProcedure
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### script

Run JavaScript and get the response back. Make sure your buffer is large enough to hold the response.

```basic
XIncludeFile "webui.pb"

Define window.i, *response

;
; @param script        The JavaScript to be run
; @param timeout       The execution timeout in seconds (0 = no timeout)
; @param buffer        The local buffer to hold the response
; @param buffer_length The local buffer size
;

window = webui_new_window()
webui_show(window, "index.html")

*response = AllocateMemory(64)
If Not webui_script(window, "return 4 + 6;", 0, *response, 64)
    Debug "JavaScript Error: " + PeekS(*response, -1, #PB_Ascii)
Else
    Debug "JavaScript Response: " + PeekS(*response, -1, #PB_Ascii) ; 10
EndIf
FreeMemory(*response)
```

Full PureBasic Example: https://github.com/webui-dev/purebasic-webui/tree/main/examples


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### script_client

Run JavaScript for a specific single client and get the response back.

```basic
XIncludeFile "webui.pb"

ProcedureC myCallback(*e.webui_event_t)
    Protected *response

    ;
    ; @param script        The JavaScript to be run
    ; @param timeout       The execution timeout in seconds (0 = no timeout)
    ; @param buffer        The local buffer to hold the response
    ; @param buffer_length The local buffer size
    ;

    *response = AllocateMemory(64)
    If Not webui_script_client(*e, "return document.title;", 0, *response, 64)
        Debug "JavaScript Error: " + PeekS(*response, -1, #PB_Ascii)
    Else
        Debug "Page title: " + PeekS(*response, -1, #PB_Ascii)
    EndIf
    FreeMemory(*response)
EndProcedure
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_runtime

Choose between `Deno`, `Bun`, and `Nodejs` as the runtime for `.js` and `.ts` files served by the web server.

```basic
XIncludeFile "webui.pb"

Define window.i

;
;   Enumeration webui_runtimes
;     #None   ; 0. No runtime
;     #Deno   ; 1. Deno runtime for .js and .ts files
;     #NodeJS ; 2. Nodejs runtime for .js files
;     #Bun    ; 3. Bun runtime for .js and .ts files
;   EndEnumeration
;

;
; @param runtime #Deno | #Bun | #NodeJS | #None
;

window = webui_new_window()
webui_set_runtime(window, #Deno)
```

Full PureBasic Example: https://github.com/webui-dev/purebasic-webui/tree/main/examples


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_count

Get the number of arguments passed to the callback from JavaScript.

```basic
XIncludeFile "webui.pb"

ProcedureC myCallback(*e.webui_event_t)

    ; JavaScript:
    ; myCallback("Foo", "Bar", 123, true);

    Protected count.i
    count = webui_get_count(*e) ; 4
    Debug "Argument count: " + Str(count)
EndProcedure
```

Full PureBasic Example: https://github.com/webui-dev/purebasic-webui/tree/main/examples


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_int

Get the first argument as a 64-bit integer.

```basic
XIncludeFile "webui.pb"

ProcedureC myCallback(*e.webui_event_t)

    ; JavaScript:
    ; myCallback(123456);

    Protected num.q
    num = webui_get_int(*e)
    Debug "Number: " + Str(num)
EndProcedure
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_int_at

Get an argument as a 64-bit integer at a specific index.

```basic
XIncludeFile "webui.pb"

ProcedureC myCallback(*e.webui_event_t)

    ; JavaScript:
    ; myCallback(12345, 6789);

    Protected n1.q, n2.q
    n1 = webui_get_int_at(*e, 0)
    n2 = webui_get_int_at(*e, 1)
    Debug "n1=" + Str(n1) + " n2=" + Str(n2)
EndProcedure
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_float

Get the first argument as a double-precision float.

```basic
XIncludeFile "webui.pb"

ProcedureC myCallback(*e.webui_event_t)

    ; JavaScript:
    ; myCallback(123.456);

    Protected f.d
    f = webui_get_float(*e)
    Debug "Float: " + StrD(f)
EndProcedure
```

Full PureBasic Example: https://github.com/webui-dev/purebasic-webui/tree/main/examples


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_float_at

Get an argument as a double-precision float at a specific index.

```basic
XIncludeFile "webui.pb"

ProcedureC myCallback(*e.webui_event_t)

    ; JavaScript:
    ; myCallback(12.34, 56.789);

    Protected f1.d, f2.d
    f1 = webui_get_float_at(*e, 0)
    f2 = webui_get_float_at(*e, 1)
    Debug "f1=" + StrD(f1) + " f2=" + StrD(f2)
EndProcedure
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_string

Get the first argument as a pointer to a null-terminated ASCII string.

```basic
XIncludeFile "webui.pb"

ProcedureC myCallback(*e.webui_event_t)

    ; JavaScript:
    ; myCallback("Foo Bar");

    Protected *str
    *str = webui_get_string(*e)
    Debug "String: " + PeekS(*str, -1, #PB_Ascii)
EndProcedure
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_string_at

Get an argument as a pointer to a null-terminated ASCII string at a specific index.

```basic
XIncludeFile "webui.pb"

ProcedureC myCallback(*e.webui_event_t)

    ; JavaScript:
    ; myCallback("Foo", "Bar");

    Protected *foo, *bar
    *foo = webui_get_string_at(*e, 0)
    *bar = webui_get_string_at(*e, 1)
    Debug PeekS(*foo, -1, #PB_Ascii) + " " + PeekS(*bar, -1, #PB_Ascii)
EndProcedure
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_bool

Get the first argument as a boolean (non-zero = `#True`, zero = `#False`).

```basic
XIncludeFile "webui.pb"

ProcedureC myCallback(*e.webui_event_t)

    ; JavaScript:
    ; myCallback(true);

    Protected status.i
    status = webui_get_bool(*e)
    If status
        Debug "True"
    Else
        Debug "False"
    EndIf
EndProcedure
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_bool_at

Get an argument as a boolean at a specific index.

```basic
XIncludeFile "webui.pb"

ProcedureC myCallback(*e.webui_event_t)

    ; JavaScript:
    ; myCallback(true, false);

    Protected s1.i, s2.i
    s1 = webui_get_bool_at(*e, 0)
    s2 = webui_get_bool_at(*e, 1)
    Debug "s1=" + Str(s1) + " s2=" + Str(s2)
EndProcedure
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_size

Get the size in bytes of the first argument. Useful for raw binary data.

```basic
XIncludeFile "webui.pb"

ProcedureC myCallback(*e.webui_event_t)

    ; JavaScript:
    ; myCallback("Foo");

    Protected len.i
    len = webui_get_size(*e)
    Debug "Size: " + Str(len) + " bytes"
EndProcedure
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_size_at

Get the size in bytes of an argument at a specific index.

```basic
XIncludeFile "webui.pb"

ProcedureC myCallback(*e.webui_event_t)

    ; JavaScript:
    ; myCallback("Foo", "Bar");

    Protected fooLen.i, barLen.i
    fooLen = webui_get_size_at(*e, 0)
    barLen = webui_get_size_at(*e, 1)
    Debug "Foo: " + Str(fooLen) + " bytes, Bar: " + Str(barLen) + " bytes"
EndProcedure
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### return_int

Return a 64-bit integer as the response to a JavaScript `await` call.

```js
async function myJavaScript() {
    var num = Number(await myCallback());
    console.log(num); // 123456
}
```

```basic
XIncludeFile "webui.pb"

ProcedureC myCallback(*e.webui_event_t)

    webui_return_int(*e, 123456)
EndProcedure
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### return_float

Return a double-precision float as the response to a JavaScript `await` call.

```js
async function myJavaScript() {
    var f = Number(await myCallback());
    console.log(f); // 123.456
}
```

```basic
XIncludeFile "webui.pb"

ProcedureC myCallback(*e.webui_event_t)

    webui_return_float(*e, 123.456)
EndProcedure
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### return_string

Return a string as the response to a JavaScript `await` call.

```js
async function myJavaScript() {
    var name = await myCallback();
    console.log(name); // "Foo Bar"
}
```

```basic
XIncludeFile "webui.pb"

ProcedureC myCallback(*e.webui_event_t)

    webui_return_string(*e, "Foo Bar")
EndProcedure
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### return_bool

Return a boolean as the response to a JavaScript `await` call.

```js
async function myJavaScript() {
    var status = Boolean(Number(await myCallback()));
    console.log(status); // true
}
```

```basic
XIncludeFile "webui.pb"

ProcedureC myCallback(*e.webui_event_t)

    webui_return_bool(*e, #True)
EndProcedure
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### send_raw

Send raw binary data to a JavaScript function in the UI. Sends to all connected clients.

```js
function myJavaScriptFunc(rawData) {
    // `rawData` is a Uint8Array
    console.log(rawData[0]); // 1
}
```

```basic
XIncludeFile "webui.pb"

Define window.i, *buffer

;
; @param function The JavaScript function name
; @param raw      The raw data buffer
; @param size     The raw data size in bytes
;

window = webui_new_window()
webui_show(window, "index.html")

*buffer = AllocateMemory(3)
PokeB(*buffer,     $01)
PokeB(*buffer + 1, $02)
PokeB(*buffer + 2, $03)
webui_send_raw(window, "myJavaScriptFunc", *buffer, 3)
FreeMemory(*buffer)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### send_raw_client

Send raw binary data to a JavaScript function for a specific single client only.

```js
function myJavaScriptFunc(rawData) {
    // `rawData` is a Uint8Array
}
```

```basic
XIncludeFile "webui.pb"

ProcedureC myCallback(*e.webui_event_t)
    Protected *buffer

    ;
    ; @param function The JavaScript function name
    ; @param raw      The raw data buffer
    ; @param size     The raw data size in bytes
    ;

    *buffer = AllocateMemory(3)
    PokeB(*buffer,     $01)
    PokeB(*buffer + 1, $02)
    PokeB(*buffer + 2, $03)
    webui_send_raw_client(*e, "myJavaScriptFunc", *buffer, 3)
    FreeMemory(*buffer)
EndProcedure
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_file_handler

Set a custom handler to serve files. The handler receives the requested filename and must return a complete HTTP response (headers + body). Replaces any handler set with `webui_set_file_handler_window()`.

```basic
XIncludeFile "webui.pb"

ProcedureC.i myHandler(filename.p-Ascii, *length)
    Protected *response, response$

    If filename = "/hello.txt"
        response$ = "HTTP/1.1 200 OK" + #CRLF$ +
                    "Content-Type: text/plain" + #CRLF$ + #CRLF$ +
                    "Hello!"
        *response = Ascii(response$)
        PokeL(*length, MemorySize(*response) - 1)
        ProcedureReturn *response
    EndIf

    ProcedureReturn #Null
EndProcedure

Define window.i

;
; @param handler The handler function
;

window = webui_new_window()
webui_set_file_handler(window, @myHandler())
webui_show(window, "index.html")
webui_wait()
```

Full PureBasic Example: https://github.com/webui-dev/purebasic-webui/tree/main/examples


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_file_handler_window

Same as `webui_set_file_handler()`, but the handler also receives the window number. Replaces any handler set with `webui_set_file_handler()`.

```basic
XIncludeFile "webui.pb"

ProcedureC.i myHandler(window.i, filename.p-Ascii, *length)
    Debug "Window " + Str(window) + " requested: " + filename
    ProcedureReturn #Null
EndProcedure

Define window.i

;
; @param handler The handler function
;

window = webui_new_window()
webui_set_file_handler_window(window, @myHandler())
webui_show(window, "index.html")
webui_wait()
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_root_folder

Set the web-server root folder path for a specific window.

> Should be called before `webui_show()`.

```basic
XIncludeFile "webui.pb"

Define window.i

;
; @param path The local folder full path
;

window = webui_new_window()
webui_set_root_folder(window, "/home/Foo/Bar/")
webui_show(window, "index.html")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_default_root_folder

Set the web-server root folder path for all windows.

> Should be called before `webui_show()`.

```basic
XIncludeFile "webui.pb"

;
; @param path The local folder full path
;

webui_set_default_root_folder("/home/Foo/Bar/")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_mime_type

Get the HTTP MIME type string for a given file extension.

```basic
XIncludeFile "webui.pb"

Define *mime

*mime = webui_get_mime_type("foo.png")
Debug PeekS(*mime, -1, #PB_Ascii) ; "image/png"
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_best_browser

Get the recommended web browser ID for this window. If a browser is already in use, returns that browser's ID.

```basic
XIncludeFile "webui.pb"

Define window.i, browserID.i

window = webui_new_window()
browserID = webui_get_best_browser(window)
Debug "Best browser ID: " + Str(browserID)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### browser_exist

Check whether a specific web browser is installed on the system.

```basic
XIncludeFile "webui.pb"

Define window.i

window = webui_new_window()

If webui_browser_exist(#Chrome)
    webui_show_browser(window, "index.html", #Chrome)
Else
    webui_show(window, "index.html")
EndIf
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_profile

Set the web browser profile to use. An empty `name` and `path` uses the default user profile.

> Must be called before `webui_show()`.

```basic
XIncludeFile "webui.pb"

Define window.i

;
; @param name The web browser profile name
; @param path The web browser profile full path
;

window = webui_new_window()
webui_set_profile(window, "MyAppProfile", "/Home/Foo/Bar")

; Use default profile:
; webui_set_profile(window, "", "")

webui_show(window, "index.html")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_proxy

Set the web browser proxy server.

> Must be called before `webui_show()`.

```basic
XIncludeFile "webui.pb"

Define window.i

;
; @param proxy_server The proxy server address
;

window = webui_new_window()
webui_set_proxy(window, "http://127.0.0.1:8888")
webui_show(window, "index.html")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_browser_folder

Set a custom folder path where WebUI should look for the browser executable.

```basic
XIncludeFile "webui.pb"

Define window.i

;
; @param path The browser folder path
;

webui_set_browser_folder("/opt/my-browser/")
window = webui_new_window()
webui_show(window, "index.html")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### delete_profile

Delete the local web-browser profile folder for a specific window.

> Recommended to call after all windows are closed, before `webui_clean()`.

> [!] This can break functionality of other windows using the same browser profile.

```basic
XIncludeFile "webui.pb"

Define window.i

window = webui_new_window()
webui_show(window, "index.html")
webui_wait()
webui_delete_profile(window)
webui_clean()
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### delete_all_profiles

Delete all local web-browser profile folders created by WebUI.

> Recommended to call after all windows are closed, before `webui_clean()`.

```basic
XIncludeFile "webui.pb"

Define window.i

window = webui_new_window()
webui_show(window, "index.html")
webui_wait()
webui_delete_all_profiles()
webui_clean()
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_port

Get the network port used by the running window's web server.

```basic
XIncludeFile "webui.pb"

Define window.i, port.i

window = webui_new_window()
webui_show(window, "index.html")

port = webui_get_port(window)
Debug "WebUI JS URL: http://localhost:" + Str(port) + "/webui.js"
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_port

Set a specific network port for the window's web server.

```basic
XIncludeFile "webui.pb"

Define window.i

;
; @param port The port number to use
;

window = webui_new_window()

If Not webui_set_port(window, 8080)
    Debug "Port 8080 is busy."
EndIf

; Window URL will be: http://localhost:8080/
webui_show(window, "index.html")
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_free_port

Find and return an available (unused) network port.

```basic
XIncludeFile "webui.pb"

Define port.i

port = webui_get_free_port()
Debug "Free port: " + Str(port)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_parent_process_id

Get the process ID of the backend application (the parent process).

```basic
XIncludeFile "webui.pb"

Define window.i, pid.i

window = webui_new_window()
webui_show(window, "index.html")

pid = webui_get_parent_process_id(window)
Debug "Backend PID: " + Str(pid)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_child_process_id

Get the process ID of the browser window child process. In WebView mode, returns the parent PID.

```basic
XIncludeFile "webui.pb"

Define window.i, pid.i

window = webui_new_window()
webui_show(window, "index.html")

pid = webui_get_child_process_id(window)
Debug "Browser PID: " + Str(pid)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_hwnd

Get the native window handle. On Windows returns `HWND`. On Linux returns `GtkWindow*` (WebView only).

```basic
XIncludeFile "webui.pb"

Define window.i, *hwnd

window = webui_new_window()
webui_show_wv(window, "index.html")

*hwnd = webui_get_hwnd(window)

; On Windows: use *hwnd as HWND
; On Linux:   use *hwnd as GtkWindow*
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### win32_get_hwnd

Get the Win32 `HWND` window handle. More reliable with WebView than web browser windows.

> Windows only.

```basic
XIncludeFile "webui.pb"

Define window.i, *hwnd

window = webui_new_window()
webui_show(window, "index.html")

*hwnd = webui_win32_get_hwnd(window)
; Use *hwnd as HWND
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### malloc

Safely allocate memory using the WebUI memory management system. Can be freed with `webui_free()`.

```basic
XIncludeFile "webui.pb"

Define *buffer

;
; @param size The number of bytes to allocate
;

*buffer = webui_malloc(1024)

; Use *buffer...

webui_free(*buffer)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### free

Safely free a buffer previously allocated by `webui_malloc()` or returned by WebUI APIs such as `webui_encode()` and `webui_decode()`.

```basic
XIncludeFile "webui.pb"

Define *encoded

; webui_encode() returns a buffer that must be freed
*encoded = webui_encode("Hello World")
Debug PeekS(*encoded, -1, #PB_Ascii)
webui_free(*encoded)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### memcpy

Copy raw bytes from a source buffer to a destination buffer.

```basic
XIncludeFile "webui.pb"

Define *src, *dst

;
; @param dest  Destination memory pointer
; @param src   Source memory pointer
; @param count Number of bytes to copy
;

*src = Ascii("Hello WebUI")
*dst = AllocateMemory(64)
webui_memcpy(*dst, *src, 11)
Debug PeekS(*dst, 11, #PB_Ascii)
FreeMemory(*src)
FreeMemory(*dst)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### encode

Encode a string to Base64. Returns a pointer to the encoded buffer — must be freed with `webui_free()`.

```basic
XIncludeFile "webui.pb"

Define *encoded

;
; @param str The string to encode (null-terminated ASCII)
;

*encoded = webui_encode("Foo Bar")
Debug "Encoded: " + PeekS(*encoded, -1, #PB_Ascii)
webui_free(*encoded)
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### decode

Decode a Base64-encoded string. Returns a pointer to the decoded buffer — must be freed with `webui_free()`.

```basic
XIncludeFile "webui.pb"

ProcedureC myCallback(*e.webui_event_t)

    ; JavaScript:
    ; myCallback(btoa("Hello World"));

    Protected *b64, *decoded
    *b64     = webui_get_string(*e)

    ;
    ; @param str The Base64 string to decode (null-terminated ASCII)
    ;

    *decoded = webui_decode(PeekS(*b64, -1, #PB_Ascii))
    Debug "Decoded: " + PeekS(*decoded, -1, #PB_Ascii)
    webui_free(*decoded)
EndProcedure
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_tls_certificate

Set the SSL/TLS certificate and private key (both in PEM format). If called with empty strings, WebUI generates a self-signed certificate.

> This works only with the TLS build of WebUI (`webui-2-secure`).

```basic
XIncludeFile "webui.pb"

;
; @param certificate_pem The SSL/TLS certificate in PEM format
; @param private_key_pem The private key in PEM format
;

If Not webui_set_tls_certificate(
        "-----BEGIN CERTIFICATE-----" + #LF$ + "...",
        "-----BEGIN PRIVATE KEY-----"  + #LF$ + "...")
    Debug "Invalid TLS certificate."
EndIf
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_logger

Set a custom logging function to receive WebUI's internal log messages.

```basic
XIncludeFile "webui.pb"

;
;   Enumeration webui_logger_level
;     #WEBUI_LOGGER_LEVEL_DEBUG ; 0. All logs with full details
;     #WEBUI_LOGGER_LEVEL_INFO  ; 1. Only general logs
;     #WEBUI_LOGGER_LEVEL_ERROR ; 2. Only fatal error logs
;   EndEnumeration
;

ProcedureC myLogger(level.i, *log, *user_data)
    Debug "[WebUI L" + Str(level) + "] " + PeekS(*log, -1, #PB_Ascii)
EndProcedure

Define window.i

;
; @param func      The logger callback function
; @param user_data Any user data pointer to pass to the logger (or #Null)
;

webui_set_logger(@myLogger(), #Null)

window = webui_new_window()
webui_show(window, "index.html")
webui_wait()
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_last_error_number

Get the error code from the most recent WebUI operation that failed.

```basic
XIncludeFile "webui.pb"

Define window.i, err.i

window = webui_new_window()

If Not webui_show(window, "index.html")
    err = webui_get_last_error_number()
    Debug "Error code: " + Str(err)
EndIf
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_last_error_message

Get the human-readable error message from the most recent WebUI operation that failed.

```basic
XIncludeFile "webui.pb"

Define window.i, *msg

window = webui_new_window()

If Not webui_show(window, "index.html")
    *msg = webui_get_last_error_message()
    Debug "Error: " + PeekS(*msg, -1, #PB_Ascii)
EndIf
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### clean

Free all memory resources used by WebUI. Should be called only once at the very end of your application, after `webui_wait()` returns.

```basic
XIncludeFile "webui.pb"

Define window.i

window = webui_new_window()
webui_show(window, "index.html")
webui_wait()
webui_clean()
```
