<div align="center">

![Logo](data/webui_120.svg)

# WebUI v2.5 - Pascal Documentation

> Use any web browser or WebView as GUI, with your preferred language in the backend and modern web technologies in the frontend, all in a lightweight portable library.

![Screenshot](data/screenshot.png)

</div>


## Getting Started
- [Download And Install](#download-and-install)
- [Minimal Example](#minimal-example)

## Available APIs
- **Window Management**
  - [webui_new_window](#new_window)
  - [webui_new_window_id](#new_window_id)
  - [webui_get_new_window_id](#get_new_window_id)
  - [webui_show](#show)
  - [webui_show_browser](#show_browser)
  - [webui_show_wv](#show_wv)
  - [webui_show_client](#show_client)
  - [webui_start_server](#start_server)
  - [webui_close](#close)
  - [webui_close_client](#close_client)
  - [webui_destroy](#destroy)
  - [webui_exit](#exit)
  - [webui_wait](#wait)
  - [webui_wait_async](#wait_async)
  - [webui_is_shown](#is_shown)
  - [webui_focus](#focus)
  - [webui_minimize](#minimize)
  - [webui_maximize](#maximize)
- **Window Configuration**
  - [webui_set_kiosk](#set_kiosk)
  - [webui_set_size](#set_size)
  - [webui_set_minimum_size](#set_minimum_size)
  - [webui_set_position](#set_position)
  - [webui_set_center](#set_center)
  - [webui_set_hide](#set_hide)
  - [webui_set_frameless](#set_frameless)
  - [webui_set_transparent](#set_transparent)
  - [webui_set_resizable](#set_resizable)
  - [webui_set_icon](#set_icon)
  - [webui_set_high_contrast](#set_high_contrast)
  - [webui_is_high_contrast](#is_high_contrast)
  - [webui_set_custom_parameters](#set_custom_parameters)
  - [webui_set_close_handler_wv](#set_close_handler_wv)
- **Navigation**
  - [webui_navigate](#navigate)
  - [webui_navigate_client](#navigate_client)
  - [webui_get_url](#get_url)
  - [webui_open_url](#open_url)
  - [webui_set_public](#set_public)
- **Events & Binding**
  - [webui_bind](#bind)
  - [TWebUIEvent](#event)
  - [webui_set_context](#set_context)
  - [webui_get_context](#get_context)
  - [webui_set_event_blocking](#set_event_blocking)
  - [webui_set_config](#set_config)
  - [webui_set_timeout](#set_timeout)
- **JavaScript**
  - [webui_run](#run)
  - [webui_run_client](#run_client)
  - [webui_script](#script)
  - [webui_script_client](#script_client)
  - [webui_set_runtime](#set_runtime)
- **Event Arguments**
  - [webui_get_count](#get_count)
  - [webui_get_int](#get_int)
  - [webui_get_int_at](#get_int_at)
  - [webui_get_float](#get_float)
  - [webui_get_float_at](#get_float_at)
  - [webui_get_string](#get_string)
  - [webui_get_string_at](#get_string_at)
  - [webui_get_bool](#get_bool)
  - [webui_get_bool_at](#get_bool_at)
  - [webui_get_size](#get_size)
  - [webui_get_size_at](#get_size_at)
- **Event Response**
  - [webui_return_int](#return_int)
  - [webui_return_float](#return_float)
  - [webui_return_string](#return_string)
  - [webui_return_bool](#return_bool)
- **Raw Data**
  - [webui_send_raw](#send_raw)
  - [webui_send_raw_client](#send_raw_client)
- **File Serving**
  - [webui_set_file_handler](#set_file_handler)
  - [webui_set_file_handler_window](#set_file_handler_window)
  - [webui_set_root_folder](#set_root_folder)
  - [webui_set_default_root_folder](#set_default_root_folder)
  - [webui_get_mime_type](#get_mime_type)
- **Browser & Profile**
  - [webui_get_best_browser](#get_best_browser)
  - [webui_browser_exist](#browser_exist)
  - [webui_set_profile](#set_profile)
  - [webui_set_proxy](#set_proxy)
  - [webui_set_browser_folder](#set_browser_folder)
  - [webui_delete_profile](#delete_profile)
  - [webui_delete_all_profiles](#delete_all_profiles)
- **Network & Ports**
  - [webui_get_port](#get_port)
  - [webui_set_port](#set_port)
  - [webui_get_free_port](#get_free_port)
- **Process & System**
  - [webui_get_parent_process_id](#get_parent_process_id)
  - [webui_get_child_process_id](#get_child_process_id)
  - [webui_get_hwnd](#get_hwnd)
  - [webui_win32_get_hwnd](#win32_get_hwnd)
- **Memory Management**
  - [webui_malloc](#malloc)
  - [webui_free](#free)
  - [webui_memcpy](#memcpy)
  - [webui_encode](#encode)
  - [webui_decode](#decode)
- **Security**
  - [webui_set_tls_certificate](#set_tls_certificate)
- **Logging & Errors**
  - [webui_set_logger](#set_logger)
  - [webui_get_last_error_number](#get_last_error_number)
  - [webui_get_last_error_message](#get_last_error_message)
- **Cleanup**
  - [webui_clean](#clean)

[JavaScript APIs](javascript.md)


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### Download And Install

<p style="text-align: justify;">The WebUI library is written in pure C, but there is a Pascal binding that wraps the C API, letting you write your backend in Free Pascal / Lazarus while the frontend runs in any web browser or WebView window.</p>

**Step 1 — Get the WebUI binary**

Download the latest pre-release from the [GitHub releases page](https://github.com/webui-dev/webui/releases) and place `webui-2.dll` (Windows), `webui-2.so` (Linux), or `webui-2.dyn` (macOS) next to your compiled executable.

Alternatively, build from source:

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

**Step 2 — Get the Pascal binding**

```sh
git clone https://github.com/webui-dev/pascal-webui.git
```

Add `pascal-webui/src/` to your project's search path and add `WebUI` to your `uses` clause.

**Linking modes:**

| Mode | Platform | How |
|------|----------|-----|
| Dynamic (default) | Windows | Place `webui-2.dll` next to the executable |
| Dynamic | macOS | Place `webui-2.dyn` next to the executable |
| Dynamic | Linux | Place `webui-2.so` next to the executable |
| Static | Windows | Compile with `-dSTATICLINK`; requires `.a` files in `staticlibs/` |


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### Minimal Example

```pascal
program Minimal;

uses WebUI;

begin
  var win := webui_new_window();
  webui_show(win, '<html><script src="webui.js"></script><body>Hello World!</body></html>');
  webui_wait();
  webui_clean();
end.
```
[More Pascal Examples](https://github.com/webui-dev/pascal-webui/tree/main/examples).


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### new_window

Create a new window and return its handle (`size_t`). All subsequent API calls use this handle to refer to the window.

```pascal
uses WebUI;

var
  win: size_t;
begin
  win := webui_new_window();

  // Later
  webui_show(win, 'index.html');
end.
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### new_window_id

Create a new window using a specific ID. The ID must be between 1 and `WEBUI_MAX_IDS`.

```pascal
uses WebUI;

var
  win: size_t;
begin
  win := webui_new_window_id(1);

  // Later
  webui_show(win, 'index.html');
end.
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_new_window_id

Get a free window ID that can be used with `webui_new_window_id()`.

```pascal
uses WebUI;

var
  id, win: size_t;
begin
  id  := webui_get_new_window_id();
  win := webui_new_window_id(id);

  // Later
  webui_show(win, 'index.html');
end.
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### show

Show a window using embedded HTML, a URL, a local file, or a local folder. If the window is already open, it will be refreshed. Sends content to all connected clients.

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

```pascal
uses WebUI;

var
  win: size_t;
begin
  win := webui_new_window();

  // The HTML, URL, local file, or local folder.
  // Leave empty to use the current root folder.
  webui_show(win, 'index.html');
end.
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### show_browser

Show a window using a specific web browser.

> It's recommended to use `WEBUI_ChromiumBased` browser.

> On macOS, the browser's icon may still appear in the Dock after exit. We recommend using `webui_show_wv` on macOS to avoid this.

```pascal
uses WebUI;

var
  win: size_t;
begin
  win := webui_new_window();

  // Browser constants:
  //   WEBUI_NoBrowser     = 0   No web browser
  //   WEBUI_AnyBrowser    = 1   Default recommended web browser
  //   WEBUI_Chrome        = 2   Google Chrome
  //   WEBUI_Firefox       = 3   Mozilla Firefox
  //   WEBUI_Edge          = 4   Microsoft Edge
  //   WEBUI_Safari        = 5   Apple Safari
  //   WEBUI_Chromium      = 6   The Chromium Project
  //   WEBUI_Opera         = 7   Opera Browser
  //   WEBUI_Brave         = 8   The Brave Browser
  //   WEBUI_Vivaldi       = 9   The Vivaldi Browser
  //   WEBUI_Epic          = 10  The Epic Browser
  //   WEBUI_Yandex        = 11  The Yandex Browser
  //   WEBUI_ChromiumBased = 12  Any Chromium based browser
  //   WEBUI_Webview       = 13  WebView (Non-web-browser)

  webui_show_browser(win, 'index.html', WEBUI_Chrome);
end.
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

```pascal
uses WebUI;

var
  win: size_t;
begin
  win := webui_new_window();
  webui_show_wv(win, 'index.html');
  webui_wait();
end.
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### show_client

Show a window for a specific single client. Useful in multi-client mode to send different content to different connected clients.

```pascal
uses WebUI;

procedure MyCallback(e: PWebUIEvent); cdecl;
begin
  // Show content to this specific client only
  webui_show_client(e, '<html>...</html>');
end;
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### start_server

Start only the local web server without opening a browser window, and return the URL. Useful for headless web app scenarios.

```pascal
uses WebUI;

var
  win: size_t;
  url: PAnsiChar;
begin
  win := webui_new_window();

  // The local file or folder to serve.
  // Leave empty to use the current root folder.
  url := webui_start_server(win, '/full/root/path');

  WriteLn('Server URL: ', url);
end.
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### close

Close a specific window. The window handle remains valid and the window can be shown again later with `webui_show()`.

```pascal
uses WebUI;

var
  win: size_t;
begin
  win := webui_new_window();
  webui_show(win, 'index.html');
  webui_wait();

  webui_close(win);
end.
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### close_client

Close the connection for a specific single client only, without closing the window for other connected clients.

```pascal
uses WebUI;

procedure MyCallback(e: PWebUIEvent); cdecl;
begin
  webui_close_client(e);
end;
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### destroy

Close a specific window and free all related memory resources.

> This makes the window handle invalid. If you want to simply close a window and reopen it later, use `webui_close()` instead.

```pascal
uses WebUI;

var
  win: size_t;
begin
  win := webui_new_window();
  webui_show(win, 'index.html');
  webui_wait();

  webui_destroy(win);
end.
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### exit

Close all open windows. This will make `webui_wait()` return.

```pascal
uses WebUI;

procedure CloseAll(e: PWebUIEvent); cdecl;
begin
  webui_exit();
end;
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### wait

Block the current thread and wait until all opened windows get closed.

```pascal
uses WebUI;

var
  win: size_t;
begin
  win := webui_new_window();
  webui_show(win, 'index.html');
  webui_wait();   // Blocks here until the window is closed
  webui_clean();
end.
```

[More Pascal Examples](https://github.com/webui-dev/pascal-webui/tree/main/examples).


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### wait_async

Non-blocking alternative to `webui_wait()`. Returns `True` if at least one window is still open, `False` when all windows are closed. Call this in a loop from the main thread to interleave your own work.

> In WebView mode, you must call this from the main thread.

```pascal
uses WebUI;

var
  win: size_t;
begin
  win := webui_new_window();
  webui_show_wv(win, 'index.html');

  while webui_wait_async() do
  begin
    // Your main thread code here
    // This loop runs while windows are open
  end;

  webui_clean();
end.
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### is_shown

Check if the specified window is still running.

```pascal
uses WebUI;

var
  win: size_t;
begin
  win := webui_new_window();
  webui_show(win, 'index.html');

  if webui_is_shown(win) then
    WriteLn('Window is open')
  else
    WriteLn('Window is closed');
end.
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### focus

Bring a window to the front and give it keyboard focus.

```pascal
uses WebUI;

var
  win: size_t;
begin
  win := webui_new_window();
  webui_show(win, 'index.html');
  webui_focus(win);
end.
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### minimize

Minimize a WebView window.

```pascal
uses WebUI;

var
  win: size_t;
begin
  win := webui_new_window();
  webui_show_wv(win, 'index.html');
  webui_minimize(win);
end.
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### maximize

Maximize a WebView window.

```pascal
uses WebUI;

var
  win: size_t;
begin
  win := webui_new_window();
  webui_show_wv(win, 'index.html');
  webui_maximize(win);
end.
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_kiosk

Set the window in Kiosk mode (_Full screen_).

```pascal
uses WebUI;

var
  win: size_t;
begin
  win := webui_new_window();
  webui_set_kiosk(win, True);
  webui_show(win, 'index.html');
  webui_wait();
end.
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_size

Set the window size in pixels.

```pascal
uses WebUI;

var
  win: size_t;
begin
  win := webui_new_window();

  // width, height
  webui_set_size(win, 800, 600);

  webui_show(win, 'index.html');
  webui_wait();
end.
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_minimum_size

Set the minimum allowed window size. The user will not be able to resize the window smaller than this.

> Currently works on Windows only.

```pascal
uses WebUI;

var
  win: size_t;
begin
  win := webui_new_window();

  // minimum width, minimum height
  webui_set_minimum_size(win, 400, 300);

  webui_show(win, 'index.html');
  webui_wait();
end.
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_position

Set the window position on screen.

```pascal
uses WebUI;

var
  win: size_t;
begin
  win := webui_new_window();

  // x, y coordinates
  webui_set_position(win, 100, 100);

  webui_show(win, 'index.html');
  webui_wait();
end.
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_center

Center the window on the screen.

> Call this before `webui_show()` for best results. Works better with WebView.

```pascal
uses WebUI;

var
  win: size_t;
begin
  win := webui_new_window();
  webui_set_center(win);
  webui_show(win, 'index.html');
  webui_wait();
end.
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_hide

Start the window in hidden mode. The window will be running but not visible.

> Should be called before `webui_show()`.

```pascal
uses WebUI;

var
  win: size_t;
begin
  win := webui_new_window();
  webui_set_hide(win, True);
  webui_show(win, 'index.html');
  webui_wait();
end.
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_frameless

Remove the window frame and title bar (borderless/frameless mode).

> Works with WebView windows only.

```pascal
uses WebUI;

var
  win: size_t;
begin
  win := webui_new_window();
  webui_set_frameless(win, True);
  webui_show_wv(win, 'index.html');
  webui_wait();
end.
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_transparent

Enable or disable window background transparency.

> Works with WebView windows only. The web content must also use a transparent background for this to be visible.

```pascal
uses WebUI;

var
  win: size_t;
begin
  win := webui_new_window();
  webui_set_transparent(win, True);
  webui_show_wv(win, 'index.html');
  webui_wait();
end.
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_resizable

Control whether the user can resize the window.

> Works with WebView windows only.

```pascal
uses WebUI;

var
  win: size_t;
begin
  win := webui_new_window();
  webui_set_resizable(win, False);  // Fixed size
  webui_show_wv(win, 'index.html');
  webui_wait();
end.
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_icon

Set the default embedded HTML favicon. The icon is served automatically by WebUI's built-in server.

```pascal
uses WebUI;

var
  win: size_t;
begin
  win := webui_new_window();

  // icon content (SVG string), MIME type
  webui_set_icon(win, '<svg>...</svg>', 'image/svg+xml');

  webui_show(win, 'index.html');
  webui_wait();
end.
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_high_contrast

Mark the window as supporting high-contrast mode. Use this together with CSS to build a better high-contrast theme.

```pascal
uses WebUI;

var
  win: size_t;
begin
  win := webui_new_window();
  webui_set_high_contrast(win, True);
  webui_show(win, 'index.html');
  webui_wait();
end.
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### is_high_contrast

Check if the operating system is currently using a high-contrast theme.

```pascal
uses WebUI;

var
  win: size_t;
begin
  win := webui_new_window();

  if webui_is_high_contrast() then
    webui_set_high_contrast(win, True);

  webui_show(win, 'index.html');
  webui_wait();
end.
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_custom_parameters

Add custom command-line parameters to the browser launch command.

```pascal
uses WebUI;

var
  win: size_t;
begin
  win := webui_new_window();
  webui_set_custom_parameters(win, '--remote-debugging-port=9222');
  webui_show(win, 'index.html');
  webui_wait();
end.
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_close_handler_wv

Set a callback to intercept the close event of a WebView window. Return `False` from the handler to prevent the window from closing; return `True` to allow it.

```pascal
uses WebUI;

function MyCloseHandler(window: size_t): Boolean; cdecl;
begin
  WriteLn('User tried to close the window');
  Result := False; // Prevent closing
end;

var
  win: size_t;
begin
  win := webui_new_window();
  webui_set_close_handler_wv(win, @MyCloseHandler);
  webui_show_wv(win, 'index.html');
  webui_wait();
end.
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### navigate

Navigate all connected clients of a window to a specific URL.

```pascal
uses WebUI;

var
  win: size_t;
begin
  win := webui_new_window();
  webui_show(win, 'index.html');
  webui_wait();

  // Note: The target page must include webui.js, otherwise
  // WebUI will close the connection and wait() will return.
  webui_navigate(win, '/foo/bar.html');
end.
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### navigate_client

Navigate a specific single client to a URL, without affecting other connected clients.

```pascal
uses WebUI;

procedure MyCallback(e: PWebUIEvent); cdecl;
begin
  webui_navigate_client(e, '/other-page.html');
end;
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_url

Get the current URL of a running window's web server.

> By default, WebUI only allows access to this URL from localhost.

```pascal
uses WebUI;

var
  win: size_t;
  url: PAnsiChar;
begin
  win := webui_new_window();
  webui_show(win, 'index.html');

  url := webui_get_url(win);
  WriteLn('Window URL: ', url);

  webui_wait();
end.
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### open_url

Open a URL in the operating system's default web browser (not a WebUI window).

```pascal
uses WebUI;

begin
  webui_open_url('https://webui.me');
end.
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_public

Allow the window's web server URL to be accessible from external devices on the network.

```pascal
uses WebUI;

var
  win: size_t;
begin
  win := webui_new_window();
  webui_set_public(win, True);

  // Now the URL is accessible from any device on the network.
  webui_show(win, 'index.html');
  webui_wait();
end.
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### bind

Bind an HTML element click event or a JavaScript function call to a Pascal callback procedure. Use an empty element name (`''`) to catch all events from the window.

```pascal
uses WebUI;

procedure MyFunction(e: PWebUIEvent); cdecl;
begin
  // Called from JavaScript: myFunction("hello", 42);
  WriteLn('myFunction called');
end;

procedure OnClick(e: PWebUIEvent); cdecl;
begin
  WriteLn('Button clicked!');
end;

var
  win: size_t;
begin
  win := webui_new_window();

  // Bind a JavaScript function name to a Pascal procedure
  webui_bind(win, 'myFunction', @MyFunction);

  // Bind an HTML element ID (click event)
  webui_bind(win, 'myButton', @OnClick);

  // Bind '' to catch all window events
  webui_bind(win, '', @MyFunction);

  webui_show(win, 'index.html');
  webui_wait();
  webui_clean();
end.
```

[More Pascal Examples](https://github.com/webui-dev/pascal-webui/tree/main/examples).


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### event

Every event callback receives a `PWebUIEvent` pointer with information about the current event. The `TWebUIEvent` record contains:

| Field | Type | Description |
|-------|------|-------------|
| `window` | `size_t` | The window handle |
| `event_type` | `size_t` | Event type constant |
| `element` | `PAnsiChar` | Bound HTML element ID or JS function name |
| `event_number` | `size_t` | Internal event number |
| `bind_id` | `size_t` | Bind ID |
| `client_id` | `size_t` | Unique client ID |
| `connection_id` | `size_t` | Client connection ID |
| `cookies` | `PAnsiChar` | Client cookies string |

```pascal
uses WebUI;

procedure Events(e: PWebUIEvent); cdecl;
begin
  if e^.event_type = WEBUI_EVENT_CONNECTED then
    WriteLn('Connected.')
  else if e^.event_type = WEBUI_EVENT_DISCONNECTED then
    WriteLn('Disconnected.')
  else if e^.event_type = WEBUI_EVENT_MOUSE_CLICK then
    WriteLn('Click on: ', e^.element)
  else if e^.event_type = WEBUI_EVENT_NAVIGATION then
    WriteLn('Navigating to: ', webui_get_string(e))
  else if e^.event_type = WEBUI_EVENT_CALLBACK then
    WriteLn('Function call.');
end;

var
  win: size_t;
begin
  win := webui_new_window();
  webui_bind(win, '', @Events); // Catch all window events
  webui_show(win, 'index.html');
  webui_wait();
  webui_clean();
end.
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_context

Attach arbitrary user data to a specific binding. The data can be retrieved later inside the callback using `webui_get_context()`. Must be called after `webui_bind()`.

```pascal
uses WebUI;

type
  PUserInfo = ^TUserInfo;
  TUserInfo = record
    id: Integer;
    name: PAnsiChar;
  end;

procedure MyFunction(e: PWebUIEvent); cdecl;
var
  info: PUserInfo;
begin
  info := PUserInfo(webui_get_context(e));
  WriteLn('User: ', info^.name, ' (ID: ', info^.id, ')');
end;

var
  win: size_t;
  user: TUserInfo;
begin
  win := webui_new_window();

  user.id   := 42;
  user.name := 'Alice';

  webui_bind(win, 'myFunction', @MyFunction);
  webui_set_context(win, 'myFunction', @user);

  webui_show(win, 'index.html');
  webui_wait();
  webui_clean();
end.
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_context

Retrieve the user data pointer previously attached to a binding with `webui_set_context()`.

```pascal
uses WebUI;

procedure MyFunction(e: PWebUIEvent); cdecl;
var
  data: Pointer;
begin
  data := webui_get_context(e);
  // Cast to your specific type
end;
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_event_blocking

Control whether UI events from this window are processed one at a time in a single blocking thread (`True`), or each in a new non-blocking thread (`False`). Use `webui_set_config(WEBUI_CONFIG_UI_EVENT_BLOCKING, ...)` to apply to all windows.

```pascal
uses WebUI;

procedure Foo(e: PWebUIEvent); cdecl;
begin
  // long task
end;

procedure Bar(e: PWebUIEvent); cdecl;
begin
  // runs after Foo when blocking is enabled
end;

var
  win: size_t;
begin
  win := webui_new_window();
  webui_bind(win, 'foo', @Foo);
  webui_bind(win, 'bar', @Bar);

  // Process events one at a time (serial)
  webui_set_event_blocking(win, True);

  webui_show(win, 'index.html');
  webui_wait();
  webui_clean();
end.
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_config

Control WebUI global behaviour. It's recommended to call this at the beginning, before `webui_show()`.

```pascal
uses WebUI;

begin
  // Config constants:
  //
  //   WEBUI_CONFIG_SHOW_WAIT_CONNECTION  = 0
  //     Wait for browser to connect before show() returns. Default: True
  //
  //   WEBUI_CONFIG_UI_EVENT_BLOCKING     = 1
  //     Process all UI events in a single blocking thread (True)
  //     or each in a new thread (False). Applies to all windows. Default: False
  //
  //   WEBUI_CONFIG_FOLDER_MONITOR        = 2
  //     Auto-refresh the window when any file in the root folder changes. Default: False
  //
  //   WEBUI_CONFIG_MULTI_CLIENT          = 3
  //     Allow multiple browser clients to connect to the same window. Default: False
  //
  //   WEBUI_CONFIG_USE_COOKIES           = 4
  //     Use WebUI auth cookies to identify clients and block unauthorized access. Default: True
  //
  //   WEBUI_CONFIG_ASYNCHRONOUS_RESPONSE = 5
  //     Set to True if your backend uses async operations and sets responses
  //     via webui_return_x() after the callback returns. Default: False

  webui_set_config(WEBUI_CONFIG_SHOW_WAIT_CONNECTION,  True);
  webui_set_config(WEBUI_CONFIG_UI_EVENT_BLOCKING,     False);
  webui_set_config(WEBUI_CONFIG_FOLDER_MONITOR,        True);
  webui_set_config(WEBUI_CONFIG_MULTI_CLIENT,          False);
  webui_set_config(WEBUI_CONFIG_USE_COOKIES,           True);
  webui_set_config(WEBUI_CONFIG_ASYNCHRONOUS_RESPONSE, False);
end.
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_timeout

Set the maximum time in seconds to wait for the browser window to connect after calling `webui_show()`. A value of `0` means wait forever.

```pascal
uses WebUI;

var
  win: size_t;
begin
  win := webui_new_window();

  // Wait up to 30 seconds for the browser to connect
  webui_set_timeout(30);

  webui_show(win, 'index.html');
  webui_wait();
  webui_clean();
end.
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### run

Run JavaScript in the window without waiting for a response. Sends to all connected clients.

```pascal
uses WebUI;

procedure MyCallback(e: PWebUIEvent); cdecl;
begin
  // Run JavaScript for one specific client only
  webui_run_client(e, 'alert(''Hello from backend!'');');
end;

var
  win: size_t;
begin
  win := webui_new_window();
  webui_bind(win, 'myButton', @MyCallback);
  webui_show(win, 'index.html');

  // Run JavaScript for all connected clients
  webui_run(win, 'alert(''Hello from backend!'');');

  webui_wait();
  webui_clean();
end.
```

[More Pascal Examples](https://github.com/webui-dev/pascal-webui/tree/main/examples).


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### run_client

Run JavaScript for a specific single client without waiting for a response.

```pascal
uses WebUI;

procedure MyCallback(e: PWebUIEvent); cdecl;
begin
  webui_run_client(e, 'document.title = ''Updated'';');
end;
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### script

Run JavaScript and get the response back. Make sure your buffer is large enough to hold the response.

```pascal
uses WebUI;

var
  win: size_t;
  response: array[0..63] of AnsiChar;
begin
  win := webui_new_window();
  webui_show(win, 'index.html');

  // script, timeout (0 = no timeout), buffer, buffer size
  if not webui_script(win, 'return 4 + 6;', 0, @response[0], SizeOf(response)) then
    WriteLn('JavaScript Error: ', PAnsiChar(@response[0]))
  else
    WriteLn('JavaScript Response: ', PAnsiChar(@response[0])); // 10

  webui_wait();
  webui_clean();
end.
```

[More Pascal Examples](https://github.com/webui-dev/pascal-webui/tree/main/examples).


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### script_client

Run JavaScript for a specific single client and get the response back.

```pascal
uses WebUI;

procedure MyCallback(e: PWebUIEvent); cdecl;
var
  response: array[0..63] of AnsiChar;
begin
  // script, timeout, buffer, buffer size
  if not webui_script_client(e, 'return document.title;', 0, @response[0], SizeOf(response)) then
    WriteLn('JavaScript Error: ', PAnsiChar(@response[0]))
  else
    WriteLn('Page title: ', PAnsiChar(@response[0]));
end;
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_runtime

Choose between `Deno`, `Bun`, and `NodeJS` as the runtime for `.js` and `.ts` files served by the web server.

```pascal
uses WebUI;

var
  win: size_t;
begin
  win := webui_new_window();

  // Runtime constants:
  //   WEBUI_RUNTIME_None   = 0  No runtime
  //   WEBUI_RUNTIME_Deno   = 1  Use Deno
  //   WEBUI_RUNTIME_NodeJS = 2  Use Node.js
  //   WEBUI_RUNTIME_Bun    = 3  Use Bun

  webui_set_runtime(win, WEBUI_RUNTIME_Deno);

  webui_show(win, 'index.html');
  webui_wait();
  webui_clean();
end.
```

[More Pascal Examples](https://github.com/webui-dev/pascal-webui/tree/main/examples).


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_count

Get the number of arguments passed to the callback from JavaScript.

```pascal
uses WebUI;

procedure MyCallback(e: PWebUIEvent); cdecl;
var
  count: size_t;
begin
  // JavaScript:
  // myCallback("Foo", "Bar", 123, true);

  count := webui_get_count(e); // 4
end;
```

[More Pascal Examples](https://github.com/webui-dev/pascal-webui/tree/main/examples).


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_int

Get the first argument as a 64-bit integer.

```pascal
uses WebUI;

procedure MyCallback(e: PWebUIEvent); cdecl;
var
  num: Int64;
begin
  // JavaScript:
  // myCallback(123456);

  num := webui_get_int(e);
end;
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_int_at

Get an argument as a 64-bit integer at a specific index (zero-based).

```pascal
uses WebUI;

procedure MyCallback(e: PWebUIEvent); cdecl;
var
  n1, n2: Int64;
begin
  // JavaScript:
  // myCallback(12345, 6789);

  n1 := webui_get_int_at(e, 0);
  n2 := webui_get_int_at(e, 1);
end;
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_float

Get the first argument as a double-precision float.

```pascal
uses WebUI;

procedure MyCallback(e: PWebUIEvent); cdecl;
var
  f: Double;
begin
  // JavaScript:
  // myCallback(123.456);

  f := webui_get_float(e);
end;
```

[More Pascal Examples](https://github.com/webui-dev/pascal-webui/tree/main/examples).


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_float_at

Get an argument as a double-precision float at a specific index (zero-based).

```pascal
uses WebUI;

procedure MyCallback(e: PWebUIEvent); cdecl;
var
  f1, f2: Double;
begin
  // JavaScript:
  // myCallback(12.34, 56.789);

  f1 := webui_get_float_at(e, 0);
  f2 := webui_get_float_at(e, 1);
end;
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_string

Get the first argument as a `PAnsiChar` string pointer.

```pascal
uses WebUI;

procedure MyCallback(e: PWebUIEvent); cdecl;
var
  name: PAnsiChar;
begin
  // JavaScript:
  // myCallback("Foo Bar");

  name := webui_get_string(e);
  WriteLn('Received: ', name);
end;
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_string_at

Get an argument as a `PAnsiChar` string pointer at a specific index (zero-based).

```pascal
uses WebUI;

procedure MyCallback(e: PWebUIEvent); cdecl;
var
  foo, bar: PAnsiChar;
begin
  // JavaScript:
  // myCallback("Foo", "Bar");

  foo := webui_get_string_at(e, 0);
  bar := webui_get_string_at(e, 1);
end;
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_bool

Get the first argument as a boolean.

```pascal
uses WebUI;

procedure MyCallback(e: PWebUIEvent); cdecl;
var
  status: Boolean;
begin
  // JavaScript:
  // myCallback(true);

  status := webui_get_bool(e);
end;
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_bool_at

Get an argument as a boolean at a specific index (zero-based).

```pascal
uses WebUI;

procedure MyCallback(e: PWebUIEvent); cdecl;
var
  s1, s2: Boolean;
begin
  // JavaScript:
  // myCallback(true, false);

  s1 := webui_get_bool_at(e, 0);
  s2 := webui_get_bool_at(e, 1);
end;
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_size

Get the size in bytes of the first argument. Useful for raw binary data.

```pascal
uses WebUI;

procedure MyCallback(e: PWebUIEvent); cdecl;
var
  len: size_t;
begin
  // JavaScript:
  // myCallback("Foo");

  len := webui_get_size(e);
end;
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_size_at

Get the size in bytes of an argument at a specific index (zero-based).

```pascal
uses WebUI;

procedure MyCallback(e: PWebUIEvent); cdecl;
var
  fooLen, barLen: size_t;
begin
  // JavaScript:
  // myCallback("Foo", "Bar");

  fooLen := webui_get_size_at(e, 0);
  barLen := webui_get_size_at(e, 1);
end;
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

```pascal
uses WebUI;

procedure MyCallback(e: PWebUIEvent); cdecl;
begin
  webui_return_int(e, 123456);
end;
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

```pascal
uses WebUI;

procedure MyCallback(e: PWebUIEvent); cdecl;
begin
  webui_return_float(e, 123.456);
end;
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

```pascal
uses WebUI;

procedure MyCallback(e: PWebUIEvent); cdecl;
begin
  webui_return_string(e, 'Foo Bar');
end;
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

```pascal
uses WebUI;

procedure MyCallback(e: PWebUIEvent); cdecl;
begin
  webui_return_bool(e, True);
end;
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

```pascal
uses WebUI;

var
  win: size_t;
  buffer: array[0..2] of Byte;
begin
  win := webui_new_window();
  webui_show(win, 'index.html');

  buffer[0] := $01;
  buffer[1] := $02;
  buffer[2] := $03;

  // JavaScript function name, buffer pointer, size in bytes
  webui_send_raw(win, 'myJavaScriptFunc', @buffer[0], SizeOf(buffer));

  webui_wait();
  webui_clean();
end.
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

```pascal
uses WebUI;

procedure MyCallback(e: PWebUIEvent); cdecl;
var
  buffer: array[0..2] of Byte;
begin
  buffer[0] := $01;
  buffer[1] := $02;
  buffer[2] := $03;

  webui_send_raw_client(e, 'myJavaScriptFunc', @buffer[0], SizeOf(buffer));
end;
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_file_handler

Set a custom handler to serve files. The handler receives the requested filename and must return a complete HTTP response (headers + body). Replaces any handler set with `webui_set_file_handler_window`.

```pascal
uses WebUI;

function MyHandler(filename: PAnsiChar; len: PInteger): PAnsiChar; cdecl;
const
  Resp = 'HTTP/1.1 200 OK'#13#10 +
         'Content-Type: text/plain'#13#10#13#10 +
         'Hello!';
begin
  if AnsiString(filename) = '/hello.txt' then
  begin
    len^ := Length(Resp);
    Result := PAnsiChar(Resp);
  end
  else
    Result := nil;
end;

var
  win: size_t;
begin
  win := webui_new_window();
  webui_set_file_handler(win, @MyHandler);
  webui_show(win, 'index.html');
  webui_wait();
  webui_clean();
end.
```

[More Pascal Examples](https://github.com/webui-dev/pascal-webui/tree/main/examples).


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_file_handler_window

Same as `webui_set_file_handler`, but the handler also receives the window number. Replaces any handler set with `webui_set_file_handler`.

```pascal
uses WebUI;

function MyHandler(window: size_t; filename: PAnsiChar; len: PInteger): Pointer; cdecl;
begin
  WriteLn('Window ', window, ' requested: ', filename);
  Result := nil;
end;

var
  win: size_t;
begin
  win := webui_new_window();
  webui_set_file_handler_window(win, @MyHandler);
  webui_show(win, 'index.html');
  webui_wait();
  webui_clean();
end.
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_root_folder

Set the web-server root folder path for a specific window.

> Should be called before `webui_show()`.

```pascal
uses WebUI;

var
  win: size_t;
begin
  win := webui_new_window();
  webui_set_root_folder(win, '/home/Foo/Bar/');
  webui_show(win, 'index.html');
  webui_wait();
  webui_clean();
end.
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_default_root_folder

Set the web-server root folder path for all windows.

> Should be called before `webui_show()`.

```pascal
uses WebUI;

var
  win: size_t;
begin
  webui_set_default_root_folder('/home/Foo/Bar/');

  win := webui_new_window();
  webui_show(win, 'index.html');
  webui_wait();
  webui_clean();
end.
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_mime_type

Get the HTTP MIME type string for a given filename or extension.

```pascal
uses WebUI;

var
  mime: PAnsiChar;
begin
  mime := webui_get_mime_type('foo.png');
  WriteLn(mime); // image/png
end.
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_best_browser

Get the recommended web browser ID for this window. If a browser is already in use, returns that browser's ID.

```pascal
uses WebUI;

var
  win, browserID: size_t;
begin
  win := webui_new_window();
  browserID := webui_get_best_browser(win);
  WriteLn('Best browser ID: ', browserID);
end.
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### browser_exist

Check whether a specific web browser is installed on the system.

```pascal
uses WebUI;

var
  win: size_t;
begin
  win := webui_new_window();

  if webui_browser_exist(WEBUI_Chrome) then
    webui_show_browser(win, 'index.html', WEBUI_Chrome)
  else
    webui_show(win, 'index.html');

  webui_wait();
  webui_clean();
end.
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_profile

Set the web browser profile to use. An empty `name` and `path` uses the default user profile.

> Must be called before `webui_show()`.

```pascal
uses WebUI;

var
  win: size_t;
begin
  win := webui_new_window();

  // Custom profile
  webui_set_profile(win, 'MyAppProfile', '/Home/Foo/Bar');

  // Use default profile:
  // webui_set_profile(win, '', '');

  webui_show(win, 'index.html');
  webui_wait();
  webui_clean();
end.
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_proxy

Set the web browser proxy server.

> Must be called before `webui_show()`.

```pascal
uses WebUI;

var
  win: size_t;
begin
  win := webui_new_window();
  webui_set_proxy(win, 'http://127.0.0.1:8888');
  webui_show(win, 'index.html');
  webui_wait();
  webui_clean();
end.
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_browser_folder

Set a custom folder path where WebUI should look for the browser executable.

```pascal
uses WebUI;

var
  win: size_t;
begin
  webui_set_browser_folder('/opt/my-browser/');

  win := webui_new_window();
  webui_show(win, 'index.html');
  webui_wait();
  webui_clean();
end.
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### delete_profile

Delete the local web-browser profile folder for a specific window.

> Recommended to call after all windows are closed, before `webui_clean()`.

> [!] This can break functionality of other windows using the same browser profile.

```pascal
uses WebUI;

var
  win: size_t;
begin
  win := webui_new_window();
  webui_show(win, 'index.html');
  webui_wait();

  webui_delete_profile(win);
  webui_clean();
end.
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### delete_all_profiles

Delete all local web-browser profile folders created by WebUI.

> Recommended to call after all windows are closed, before `webui_clean()`.

```pascal
uses WebUI;

var
  win: size_t;
begin
  win := webui_new_window();
  webui_show(win, 'index.html');
  webui_wait();

  webui_delete_all_profiles();
  webui_clean();
end.
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_port

Get the network port used by the running window's web server.

```pascal
uses WebUI;

var
  win, port: size_t;
begin
  win  := webui_new_window();
  webui_show(win, 'index.html');

  port := webui_get_port(win);
  WriteLn('WebUI JS URL: http://localhost:', port, '/webui.js');

  webui_wait();
  webui_clean();
end.
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_port

Set a specific network port for the window's web server.

```pascal
uses WebUI;

var
  win: size_t;
begin
  win := webui_new_window();

  if not webui_set_port(win, 8080) then
    WriteLn('Port 8080 is busy.');

  // Window URL will be: http://localhost:8080/
  webui_show(win, 'index.html');
  webui_wait();
  webui_clean();
end.
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_free_port

Find and return an available (unused) network port.

```pascal
uses WebUI;

var
  port: size_t;
begin
  port := webui_get_free_port();
  WriteLn('Free port: ', port);
end.
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_parent_process_id

Get the process ID of the backend application (the parent process).

```pascal
uses WebUI;

var
  win, parent_pid: size_t;
begin
  win        := webui_new_window();
  webui_show(win, 'index.html');

  parent_pid := webui_get_parent_process_id(win);
  WriteLn('Backend PID: ', parent_pid);

  webui_wait();
  webui_clean();
end.
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_child_process_id

Get the process ID of the browser window child process. In WebView mode, returns the parent PID.

```pascal
uses WebUI;

var
  win, child_pid: size_t;
begin
  win       := webui_new_window();
  webui_show(win, 'index.html');

  child_pid := webui_get_child_process_id(win);
  WriteLn('Browser PID: ', child_pid);

  webui_wait();
  webui_clean();
end.
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_hwnd

Get the native window handle. On Windows returns `HWND`. On Linux returns `GtkWindow*` (WebView only).

```pascal
uses WebUI;

var
  win:  size_t;
  hwnd: Pointer;
begin
  win  := webui_new_window();
  webui_show_wv(win, 'index.html');

  hwnd := webui_get_hwnd(win);

  // On Windows: cast to HWND via Windows unit
  // On Linux:   cast to PGtkWindow (requires GTK unit)

  webui_wait();
  webui_clean();
end.
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### win32_get_hwnd

Get the Win32 `HWND` window handle. More reliable with WebView than the browser window.

> Windows only.

```pascal
uses WebUI, Windows;

var
  win:  size_t;
  hwnd: HWND;
begin
  win  := webui_new_window();
  webui_show_wv(win, 'index.html');

  hwnd := HWND(webui_win32_get_hwnd(win));

  webui_wait();
  webui_clean();
end.
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### malloc

Safely allocate memory using the WebUI memory management system. Free with `webui_free()`.

```pascal
uses WebUI;

var
  buffer: PAnsiChar;
begin
  buffer := PAnsiChar(webui_malloc(1024));

  // Use buffer...

  webui_free(buffer);
end.
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### free

Safely free a buffer previously allocated by `webui_malloc()` or returned by WebUI APIs such as `webui_encode()`.

```pascal
uses WebUI;

var
  encoded: PAnsiChar;
begin
  encoded := webui_encode('Hello World');

  // Use encoded...

  webui_free(encoded);
end.
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### memcpy

Copy raw bytes from a source buffer to a destination buffer using the WebUI memory management system.

```pascal
uses WebUI;

var
  src: array[0..63] of AnsiChar;
  dst: array[0..63] of AnsiChar;
begin
  src := 'Hello WebUI';

  // destination, source, byte count
  webui_memcpy(@dst[0], @src[0], SizeOf(src));
end.
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### encode

Encode a string to Base64. The returned pointer must be freed with `webui_free()`.

```pascal
uses WebUI;

var
  encoded: PAnsiChar;
begin
  encoded := webui_encode('Foo Bar');
  WriteLn('Encoded: ', encoded);
  webui_free(encoded);
end.
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### decode

Decode a Base64-encoded string. The returned pointer must be freed with `webui_free()`.

```pascal
uses WebUI;

procedure MyCallback(e: PWebUIEvent); cdecl;
var
  base64, decoded: PAnsiChar;
begin
  // JavaScript:
  // myCallback(btoa("Hello World"));

  base64  := webui_get_string(e);
  decoded := webui_decode(base64);
  WriteLn('Decoded: ', decoded);
  webui_free(decoded);
end;
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_tls_certificate

Set the SSL/TLS certificate and private key (both in PEM format). If called with empty strings, WebUI generates a self-signed certificate.

> This works only with the TLS build of WebUI (`webui-2-secure`).

```pascal
uses WebUI;

var
  win: size_t;
begin
  // certificate PEM, private key PEM
  if not webui_set_tls_certificate(
    '-----BEGIN CERTIFICATE-----'#10'...',
    '-----BEGIN PRIVATE KEY-----'#10'...') then
    WriteLn('Invalid TLS certificate.');

  win := webui_new_window();
  webui_show(win, 'index.html');
  webui_wait();
  webui_clean();
end.
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_logger

Set a custom logging function to receive WebUI's internal log messages.

```pascal
uses WebUI;

// Logger level constants:
//   WEBUI_LOGGER_LEVEL_DEBUG = 0  All logs with full details
//   WEBUI_LOGGER_LEVEL_INFO  = 1  Only general logs
//   WEBUI_LOGGER_LEVEL_ERROR = 2  Only fatal error logs

procedure MyLogger(level: size_t; const log: PAnsiChar; user_data: Pointer); cdecl;
begin
  WriteLn('[WebUI L', level, '] ', log);
end;

var
  win: size_t;
begin
  // logger procedure, user data (nil = none)
  webui_set_logger(@MyLogger, nil);

  win := webui_new_window();
  webui_show(win, 'index.html');
  webui_wait();
  webui_clean();
end.
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_last_error_number

Get the error code from the most recent WebUI operation that failed.

```pascal
uses WebUI;

var
  win: size_t;
  err: size_t;
begin
  win := webui_new_window();

  if not webui_show(win, 'index.html') then
  begin
    err := webui_get_last_error_number();
    WriteLn('Error code: ', err);
  end;

  webui_wait();
  webui_clean();
end.
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_last_error_message

Get the human-readable error message from the most recent WebUI operation that failed.

```pascal
uses WebUI;

var
  win: size_t;
  msg: PAnsiChar;
begin
  win := webui_new_window();

  if not webui_show(win, 'index.html') then
  begin
    msg := webui_get_last_error_message();
    WriteLn('Error: ', msg);
  end;

  webui_wait();
  webui_clean();
end.
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### clean

Free all memory resources used by WebUI. Should be called only once at the very end of your application, after `webui_wait()` returns.

```pascal
uses WebUI;

var
  win: size_t;
begin
  win := webui_new_window();
  webui_show(win, 'index.html');
  webui_wait();
  webui_clean();
end.
```
