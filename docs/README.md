<div align="center">

![Logo](data/webui_120.svg)

# WebUI v2.5 - C Documentation

> Use any web browser or WebView as GUI, with your preferred language in the backend and modern web technologies in the frontend, all in a lightweight portable library.

![Screenshot](data/screenshot.png)

</div>


## Available APIs

- [Download And Install](#download-and-install)
- [Minimal Example](#minimal-example)

**Window Management**
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

**Window Configuration**
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

**Navigation**
- [navigate](#navigate)
- [navigate_client](#navigate_client)
- [get_url](#get_url)
- [open_url](#open_url)
- [set_public](#set_public)

**Events & Binding**
- [bind](#bind)
- [event](#event)
- [set_context](#set_context)
- [get_context](#get_context)
- [set_event_blocking](#set_event_blocking)
- [set_config](#set_config)
- [set_timeout](#set_timeout)

**JavaScript**
- [run](#run)
- [run_client](#run_client)
- [script](#script)
- [script_client](#script_client)
- [set_runtime](#set_runtime)

**Event Arguments**
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

**Event Response**
- [return_int](#return_int)
- [return_float](#return_float)
- [return_string](#return_string)
- [return_bool](#return_bool)

**Raw Data**
- [send_raw](#send_raw)
- [send_raw_client](#send_raw_client)

**File Serving**
- [set_file_handler](#set_file_handler)
- [set_file_handler_window](#set_file_handler_window)
- [set_root_folder](#set_root_folder)
- [set_default_root_folder](#set_default_root_folder)
- [get_mime_type](#get_mime_type)

**Browser & Profile**
- [get_best_browser](#get_best_browser)
- [browser_exist](#browser_exist)
- [set_profile](#set_profile)
- [set_proxy](#set_proxy)
- [set_browser_folder](#set_browser_folder)
- [delete_profile](#delete_profile)
- [delete_all_profiles](#delete_all_profiles)

**Network & Ports**
- [get_port](#get_port)
- [set_port](#set_port)
- [get_free_port](#get_free_port)

**Process & System**
- [get_parent_process_id](#get_parent_process_id)
- [get_child_process_id](#get_child_process_id)
- [get_hwnd](#get_hwnd)
- [win32_get_hwnd](#win32_get_hwnd)

**Memory Management**
- [malloc](#malloc)
- [free](#free)
- [memcpy](#memcpy)
- [encode](#encode)
- [decode](#decode)

**Security**
- [set_tls_certificate](#set_tls_certificate)

**Logging & Errors**
- [set_logger](#set_logger)
- [get_last_error_number](#get_last_error_number)
- [get_last_error_message](#get_last_error_message)

**Cleanup**
- [clean](#clean)

- [JavaScript APIs](javascript.md)


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### Download And Install

<p style="text-align: justify;">The WebUI library is written in pure C, but there is a wrapper in every popular programming language to provide easy-to-use APIs to write your backend application, while HTML5 and JavaScript are always used in the frontend inside a web browser or WebView window.</p>

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


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### Minimal Example

```c
#include "webui.h"

int main() {

    size_t win = webui_new_window();
    webui_show(win, "<html><script src=\"webui.js\"></script> Hello World from C! </html>");
    webui_wait();
    return 0;
}
```
[More C Examples](https://github.com/webui-dev/webui/tree/main/examples/C).


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### new_window

Create a new window object.

```c
#include "webui.h"

int main() {

    size_t win = webui_new_window();

    // Later
    webui_show(win, "index.html");
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### new_window_id

Create a new window object using a specified ID.

> The `id` should be between 1 and `WEBUI_MAX_IDS`

```c
#include "webui.h"

int main() {

    /*
    * @param window_number The window number (should be > 0, and < WEBUI_MAX_IDS)
    */

    size_t win = 1;
    webui_new_window_id(win);

    // Later
    webui_show(win, "index.html");
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_new_window_id

Get a free window ID that can be used with `new_window_id` to create a new window object.

```c
#include "webui.h"

int main() {

    size_t win = webui_get_new_window_id();

    // Later
    webui_new_window_id(win);
    webui_show(win, "index.html");
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### show

Show a window using embedded HTML, a URL, a local file, or a local folder. If the window is already open, it will be refreshed. This refreshes all clients in multi-client mode.

WebUI uses this detection order:

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

> To use only a specific browser please use `show_browser()`, and to use only WebView please use `show_wv()`

```c
#include "webui.h"

int main() {

    /*
    * @param window The window number
    * @param content The HTML, URL, local file, or local folder.
    *                Pass NULL or empty string to use the current root folder.
    */

    webui_show(win, "index.html");
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### show_browser

Show a window using a specific web browser.

> It's recommended to use `ChromiumBased` browser.

> On macOS, the browser's icon may still appear in the Dock after exit. We recommend using `show_wv` on macOS to avoid this.

```c
#include "webui.h"

int main() {

    /*
        enum webui_browser {
            NoBrowser = 0,  // 0. No web browser
            AnyBrowser = 1, // 1. Default recommended web browser
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
    * @param content The HTML, URL, local file, or local folder
    * @param browser The web browser to be used
    */

    webui_show_browser(win, "index.html", Chrome);
}
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

```c
#include "webui.h"

int main() {

    /*
    * @param window The window number
    * @param content The HTML, URL, or a local file
    */

    webui_show_wv(win, "index.html");
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### show_client

Show a window for a specific single client. Useful in multi-client mode to send different content to different connected clients.

```c
#include "webui.h"

void callback(webui_event_t* e) {

    /*
    * @param e The event struct
    * @param content The HTML, URL, or a local file
    */

    webui_show_client(e, "<html>...</html>");
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### start_server

Start only the local web server without opening a browser window, and return the URL. Useful for headless web app scenarios.

```c
#include "webui.h"

int main() {

    /*
    * @param window The window number
    * @param content The local file or local folder to serve.
    *                Pass NULL or empty string to use the current root folder.
    */

    const char* url = webui_start_server(win, "/full/root/path");
    printf("Server URL: %s\n", url);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### close

Close a specific window. The window object will still exist and can be shown again later.

```c
#include "webui.h"

int main() {

    /*
    * @param window The window number
    */

    webui_close(win);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### close_client

Close the connection for a specific single client only, without closing the window for other connected clients.

```c
#include "webui.h"

void callback(webui_event_t* e) {

    /*
    * @param e The event struct
    */

    webui_close_client(e);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### destroy

Close a specific window and free all related memory resources.

> This will make the `win` object unavailable. If you want to simply close a window and reopen it later, please use `close()` instead.

```c
#include "webui.h"

int main() {

    /*
    * @param window The window number
    */

    webui_destroy(win);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### exit

Close all open windows. This will make `wait()` return (_Break_).

```c
#include "webui.h"

int main() {

    webui_exit();
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### wait

Block the current thread and wait until all opened windows get closed.

```c
#include "webui.h"

int main() {

    webui_show(win, "index.html");
    webui_wait();
    return 0;
}
```

Full C Example: https://github.com/webui-dev/webui/tree/main/examples/C/minimal


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### wait_async

Non-blocking alternative to `wait()`. Returns `true` if at least one window is still open, `false` when all windows are closed. Call this in a loop from the main thread to interleave your own main-thread work.

> In WebView mode, you must call this from the main thread.

```c
#include "webui.h"

int main() {

    webui_show_wv(win, "index.html");

    while (webui_wait_async()) {
        // Your main thread code here
        // This loop runs while windows are open
    }

    return 0;
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### is_shown

Check if the specified window is still running.

```c
#include "webui.h"

int main() {

    /*
    * @param window The window number
    */

    if (webui_is_shown(win)) {
        // Window is shown
    }
    else {
        // Window is closed
    }
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### focus

Bring a window to the front and give it keyboard focus.

```c
#include "webui.h"

int main() {

    /*
    * @param window The window number
    */

    webui_focus(win);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### minimize

Minimize a WebView window.

```c
#include "webui.h"

int main() {

    /*
    * @param window The window number
    */

    webui_minimize(win);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### maximize

Maximize a WebView window.

```c
#include "webui.h"

int main() {

    /*
    * @param window The window number
    */

    webui_maximize(win);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_kiosk

Set the window in Kiosk mode (_Full screen_).

```c
#include "webui.h"

int main() {

    /*
    * @param window The window number
    * @param status True or False
    */

    webui_set_kiosk(win, true);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_size

Set the window size.

```c
#include "webui.h"

int main() {

    /*
    * @param window The window number
    * @param width The window width
    * @param height The window height
    */

    webui_set_size(win, 800, 600);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_minimum_size

Set the minimum allowed window size. The user will not be able to resize the window smaller than this.

> Currently works on Windows only.

```c
#include "webui.h"

int main() {

    /*
    * @param window The window number
    * @param width The minimum window width
    * @param height The minimum window height
    */

    webui_set_minimum_size(win, 400, 300);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_position

Set the window position on screen.

```c
#include "webui.h"

int main() {

    /*
    * @param window The window number
    * @param x The window X coordinate
    * @param y The window Y coordinate
    */

    webui_set_position(win, 100, 100);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_center

Center the window on the screen.

> Call this before `show()` for best results. Works better with WebView.

```c
#include "webui.h"

int main() {

    /*
    * @param window The window number
    */

    webui_set_center(win);
    webui_show(win, "index.html");
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_hide

Start the window in hidden mode. The window will be running but not visible.

> Should be called before `show()`.

```c
#include "webui.h"

int main() {

    /*
    * @param window The window number
    * @param status True or False
    */

    webui_set_hide(win, true);
    webui_show(win, "index.html");
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_frameless

Remove the window frame and title bar (borderless/frameless mode).

> Works with WebView windows only.

```c
#include "webui.h"

int main() {

    /*
    * @param window The window number
    * @param status True or False
    */

    webui_set_frameless(win, true);
    webui_show_wv(win, "index.html");
}
```

Full C Example: https://github.com/webui-dev/webui/tree/main/examples/C/frameless


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_transparent

Enable or disable window background transparency.

> Works with WebView windows only. The web content must also use a transparent/semi-transparent background for this to be visible.

```c
#include "webui.h"

int main() {

    /*
    * @param window The window number
    * @param status True or False
    */

    webui_set_transparent(win, true);
    webui_show_wv(win, "index.html");
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_resizable

Control whether the user can resize the window.

> Works with WebView windows only.

```c
#include "webui.h"

int main() {

    /*
    * @param window The window number
    * @param status True to allow resizing, False to fix the size
    */

    webui_set_resizable(win, false);
    webui_show_wv(win, "index.html");
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_icon

Set the default embedded HTML favicon. The icon is served automatically by WebUI's built-in server.

```c
#include "webui.h"

int main() {

    /*
    * @param window The window number
    * @param icon The icon as string: `<svg>...</svg>`
    * @param icon_type The icon MIME type: `image/svg+xml`
    */

    webui_set_icon(win, "<svg>...</svg>", "image/svg+xml");
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_high_contrast

Mark the window as supporting high-contrast mode. Use this together with CSS to build a better high-contrast theme.

```c
#include "webui.h"

int main() {

    /*
    * @param window The window number
    * @param status True or False
    */

    webui_set_high_contrast(win, true);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### is_high_contrast

Check if the operating system is currently using a high-contrast theme.

```c
#include "webui.h"

int main() {

    if (webui_is_high_contrast()) {
        // OS is using a high-contrast theme
        webui_set_high_contrast(win, true);
    }
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_custom_parameters

Add custom command-line parameters to the browser launch command. This can be used, for example, to enable remote debugging.

```c
#include "webui.h"

int main() {

    /*
    * @param window The window number
    * @param params Command line parameters string
    */

    webui_set_custom_parameters(win, "--remote-debugging-port=9222");
    webui_show(win, "index.html");
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_close_handler_wv

Set a callback to intercept the close event of a WebView window. Return `false` from the handler to prevent the window from closing; return `true` to allow it.

```c
#include "webui.h"

bool myCloseHandler(size_t window) {
    printf("User tried to close window %zu\n", window);
    // Return false to prevent closing, true to allow it
    return false;
}

int main() {

    /*
    * @param window The window number
    * @param close_handler The handler function
    */

    webui_set_close_handler_wv(win, myCloseHandler);
    webui_show_wv(win, "index.html");
    webui_wait();
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### navigate

Navigate all connected clients of a window to a specific URL.

```c
#include "webui.h"

int main() {

    /*
    * @param window The window number
    * @param url Full HTTP URL or relative path
    */

    webui_navigate(win, "/foo/bar.html");

    // [!] Note:
    //
    // If `bar.html` does not include `webui.js` then
    // WebUI will try to close the window and `wait()`
    // will break. It's important to include `webui.js`
    // in every HTML page.
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### navigate_client

Navigate a specific single client to a URL, without affecting other connected clients.

```c
#include "webui.h"

void callback(webui_event_t* e) {

    /*
    * @param e The event struct
    * @param url Full HTTP URL or relative path
    */

    webui_navigate_client(e, "/other-page.html");
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_url

Get the current URL of a running window's web server.

> By default, WebUI only allows access to this URL from localhost.

```c
#include "webui.h"

int main() {

    /*
    * @param window The window number
    */

    const char* url = webui_get_url(win);
    printf("Window URL: %s\n", url);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### open_url

Open a URL in the operating system's default web browser (not a WebUI window).

```c
#include "webui.h"

int main() {

    /*
    * @param url The URL to open
    */

    webui_open_url("https://webui.me");
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_public

Allow the window's web server URL to be accessible from external devices on the network. By default, WebUI only allows access from localhost.

```c
#include "webui.h"

int main() {

    /*
    * @param window The window number
    * @param status True or False
    */

    webui_set_public(win, true);

    // Now, the URL of the window is accessible
    // from any device/mobile on the network.
}
```

Full C Example: https://github.com/webui-dev/webui/tree/main/examples/C/public_network_access


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### bind

Bind an HTML element click event or a JavaScript function call to a C callback. Use an empty element name to catch all events from the window.

```c
#include "webui.h"

void events(webui_event_t* e) {
    // Catches all events from the window
}

void myFunction(webui_event_t* e) {
    // Called from JavaScript: myFunction("hello", 42);
}

int main() {

    /*
    * @param window The window number
    * @param element The HTML element ID or JavaScript function name
    * @param func The callback function
    */

    webui_bind(win, "", events);
    webui_bind(win, "myFunction", myFunction);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### event

Every event callback receives a `webui_event_t` pointer with information about the current event: window, type, element name, and more.

```c
#include "webui.h"

/*
    typedef struct webui_event_t {
        size_t window;        // The window object number
        size_t event_type;    // Event type
        char* element;        // HTML element ID
        size_t event_number;  // Internal WebUI
        size_t bind_id;       // Bind ID
        size_t client_id;     // Client's unique ID
        size_t connection_id; // Client's connection ID
        char* cookies;        // Client's full cookies
    };

    enum webui_event {
        WEBUI_EVENT_DISCONNECTED = 0, // Window disconnection event
        WEBUI_EVENT_CONNECTED,        // Window connection event
        WEBUI_EVENT_MOUSE_CLICK,      // Mouse click event
        WEBUI_EVENT_NAVIGATION,       // Window navigation event
        WEBUI_EVENT_CALLBACK,         // Function call event
    };
*/

void events(webui_event_t* e) {
    if (e->event_type == WEBUI_EVENT_CONNECTED)
        printf("Connected. \n");
    else if (e->event_type == WEBUI_EVENT_DISCONNECTED)
        printf("Disconnected. \n");
    else if (e->event_type == WEBUI_EVENT_MOUSE_CLICK)
        printf("Click on element: %s\n", e->element);
    else if (e->event_type == WEBUI_EVENT_NAVIGATION)
        printf("Navigating to: %s\n", webui_get_string(e));
}

int main() {

    webui_bind(win, "", events);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_context

Attach arbitrary user data to a specific binding. The data can be retrieved later inside the callback using `get_context()`. Must be called after `webui_bind()`.

```c
#include "webui.h"

typedef struct {
    int user_id;
    const char* name;
} MyData;

void myFunction(webui_event_t* e) {
    MyData* data = (MyData*) webui_get_context(e);
    printf("User: %s (ID: %d)\n", data->name, data->user_id);
}

int main() {

    MyData userData = { 42, "Alice" };

    /*
    * @param window The window number
    * @param element The HTML element / JavaScript function name
    * @param context Any user data pointer
    */

    webui_bind(win, "myFunction", myFunction);
    webui_set_context(win, "myFunction", &userData);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_context

Retrieve the user data pointer previously attached to a binding with `set_context()`.

```c
#include "webui.h"

void myFunction(webui_event_t* e) {

    /*
    * @param e The event struct
    */

    void* myData = webui_get_context(e);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_event_blocking

Control whether UI events from this window are processed one at a time in a single blocking thread (`true`), or each in a new non-blocking thread (`false`). Applies to this window only. Use `set_config(ui_event_blocking, ...)` to apply to all windows.

> Note: When set to `true`, the `script()` API will not return a response until the current event callback has finished.

```c
#include "webui.h"

void foo(webui_event_t* e) {
    // long running task
}

void bar(webui_event_t* e) {
    // runs after foo completes
}

int main() {
    webui_bind(win, "foo", foo);
    webui_bind(win, "bar", bar);

    /*
    * @param window The window number
    * @param status True for blocking (serial), False for non-blocking (parallel)
    */

    webui_set_event_blocking(win, true);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_config

Control WebUI global behaviour. It's recommended to call this at the beginning, before `show()`.

```c
#include "webui.h"

/*
    typedef enum {
        // Wait for browser to connect before show() returns.
        // Default: true
        show_wait_connection = 0,

        // Process all UI events in a single blocking thread (true)
        // or each in a new thread (false). Applies to all windows.
        // Default: false
        ui_event_blocking,

        // Auto-refresh the window when any file in the root folder changes.
        // Default: false
        folder_monitor,

        // Allow multiple browser clients to connect to the same window.
        // Useful for web apps. See documentation for details.
        // Default: false
        multi_client,

        // Use WebUI auth cookies to identify clients and block unauthorized
        // URL access. Keep true to restrict access to one client at a time.
        // Default: true
        use_cookies,

        // Set to true if your backend uses async operations and sets
        // responses via webui_return_x() after the callback returns.
        // Default: false
        asynchronous_response,
    } webui_config;
*/

int main() {

    /*
    * @param option The desired option from `webui_config` enum
    * @param status The status of the option, `true` or `false`
    */

    webui_set_config(show_wait_connection, true);
    webui_set_config(ui_event_blocking, false);
    webui_set_config(folder_monitor, true);
    webui_set_config(multi_client, false);
    webui_set_config(use_cookies, true);
    webui_set_config(asynchronous_response, false);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_timeout

Set the maximum time in seconds to wait for the browser window to connect after calling `show()`. A value of `0` means wait forever.

```c
#include "webui.h"

int main() {

    /*
    * @param second The timeout in seconds (0 = no timeout)
    */

    webui_set_timeout(30);

    webui_show(win, "index.html");
    webui_wait();
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### run

Run JavaScript in the window without waiting for a response. Sends to all connected clients.

```c
#include "webui.h"

void callback(webui_event_t* e) {

    // Run JavaScript for one specific client only
    webui_run_client(e, "alert('Hello from backend!');");
}

int main() {

    /*
    * @param window The window number
    * @param script The JavaScript to be run
    */

    // Run JavaScript for all connected clients
    webui_run(win, "alert('Hello from backend!');");
}
```

Full C Example: https://github.com/webui-dev/webui/tree/main/examples/C/call_js_from_c


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### run_client

Run JavaScript for a specific single client without waiting for a response.

```c
#include "webui.h"

void callback(webui_event_t* e) {

    /*
    * @param e The event struct
    * @param script The JavaScript to be run
    */

    webui_run_client(e, "document.title = 'Updated';");
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### script

Run JavaScript and get the response back. Works in single client mode. Make sure your buffer is large enough to hold the response.

```c
#include "webui.h"

void callback(webui_event_t* e) {

    // Run JavaScript for one specific client and get response
    char response[64];
    if (!webui_script_client(e, "return 4 + 6;", 0, response, 64)) {
        printf("JavaScript Error: %s\n", response);
    } else {
        printf("JavaScript Response: %s\n", response); // 10
    }
}

int main() {

    /*
    * @param window The window number
    * @param script The JavaScript to be run
    * @param timeout The execution timeout in seconds (0 = no timeout)
    * @param buffer The local buffer to hold the response
    * @param buffer_length The local buffer size
    */

    char response[64];
    if (!webui_script(win, "return 4 + 6;", 0, response, 64)) {
        printf("JavaScript Error: %s\n", response);
    } else {
        printf("JavaScript Response: %s\n", response); // 10
    }
}
```

Full C Example: https://github.com/webui-dev/webui/tree/main/examples/C/call_js_from_c


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### script_client

Run JavaScript for a specific single client and get the response back.

```c
#include "webui.h"

void callback(webui_event_t* e) {

    /*
    * @param e The event struct
    * @param script The JavaScript to be run
    * @param timeout The execution timeout in seconds (0 = no timeout)
    * @param buffer The local buffer to hold the response
    * @param buffer_length The local buffer size
    */

    char response[64];
    if (!webui_script_client(e, "return document.title;", 0, response, 64)) {
        printf("JavaScript Error: %s\n", response);
    } else {
        printf("Page title: %s\n", response);
    }
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_runtime

Choose between `Deno`, `Bun`, and `Nodejs` as the runtime for `.js` and `.ts` files served by the web server.

```c
#include "webui.h"

int main() {

    /*
    * @param window The window number
    * @param runtime Deno | Bun | NodeJS | None
    */

    webui_set_runtime(win, Deno);

    // Now, any HTTP request to any `.js` or `.ts` file
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

Full C Example: https://github.com/webui-dev/webui/tree/main/examples/C/serve_a_folder


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_count

Get the number of arguments passed to the callback from JavaScript.

```c
void callback(webui_event_t* e) {

    // JavaScript:
    // callback("Foo", "Bar", 123, true);

    size_t count = webui_get_count(e); // 4
}
```

Full C Example: https://github.com/webui-dev/webui/tree/main/examples/C/call_c_from_js


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_int

Get the first argument as a 64-bit integer.

```c
void callback(webui_event_t* e) {

    // JavaScript:
    // callback(123456);

    long long int num = webui_get_int(e);
}
```

Full C Example: https://github.com/webui-dev/webui/tree/main/examples/C/call_c_from_js


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_int_at

Get an argument as a 64-bit integer at a specific index.

```c
void callback(webui_event_t* e) {

    // JavaScript:
    // callback(12345, 6789);

    long long int n1 = webui_get_int_at(e, 0);
    long long int n2 = webui_get_int_at(e, 1);
}
```

Full C Example: https://github.com/webui-dev/webui/tree/main/examples/C/call_c_from_js


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_float

Get the first argument as a double-precision float.

```c
void callback(webui_event_t* e) {

    // JavaScript:
    // callback(123.456);

    double f = webui_get_float(e);
}
```

Full C Example: https://github.com/webui-dev/webui/tree/main/examples/C/call_c_from_js


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_float_at

Get an argument as a double-precision float at a specific index.

```c
void callback(webui_event_t* e) {

    // JavaScript:
    // callback(12.34, 56.789);

    double f1 = webui_get_float_at(e, 0);
    double f2 = webui_get_float_at(e, 1);
}
```

Full C Example: https://github.com/webui-dev/webui/tree/main/examples/C/call_c_from_js


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_string

Get the first argument as a null-terminated string.

```c
void callback(webui_event_t* e) {

    // JavaScript:
    // callback("Foo Bar");

    const char* name = webui_get_string(e);
}
```

Full C Example: https://github.com/webui-dev/webui/tree/main/examples/C/call_c_from_js


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_string_at

Get an argument as a null-terminated string at a specific index.

```c
void callback(webui_event_t* e) {

    // JavaScript:
    // callback("Foo", "Bar");

    const char* foo = webui_get_string_at(e, 0);
    const char* bar = webui_get_string_at(e, 1);
}
```

Full C Example: https://github.com/webui-dev/webui/tree/main/examples/C/call_c_from_js


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_bool

Get the first argument as a boolean.

```c
void callback(webui_event_t* e) {

    // JavaScript:
    // callback(true);

    bool status = webui_get_bool(e);
}
```

Full C Example: https://github.com/webui-dev/webui/tree/main/examples/C/call_c_from_js


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_bool_at

Get an argument as a boolean at a specific index.

```c
void callback(webui_event_t* e) {

    // JavaScript:
    // callback(true, false);

    bool status1 = webui_get_bool_at(e, 0);
    bool status2 = webui_get_bool_at(e, 1);
}
```

Full C Example: https://github.com/webui-dev/webui/tree/main/examples/C/call_c_from_js


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_size

Get the size in bytes of the first argument. Useful for raw binary data.

```c
void callback(webui_event_t* e) {

    // JavaScript:
    // callback("Foo");

    size_t len = webui_get_size(e);
}
```

Full C Example: https://github.com/webui-dev/webui/tree/main/examples/C/call_c_from_js


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_size_at

Get the size in bytes of an argument at a specific index.

```c
void callback(webui_event_t* e) {

    // JavaScript:
    // callback("Foo", "Bar");

    size_t fooLen = webui_get_size_at(e, 0);
    size_t barLen = webui_get_size_at(e, 1);
}
```

Full C Example: https://github.com/webui-dev/webui/tree/main/examples/C/call_c_from_js


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### return_int

Return a 64-bit integer as the response to a JavaScript `await` call.

```js
async function myJavaScript() {
    var num = Number(await callback());
    console.log(num); // 123456
}
```

```c
void callback(webui_event_t* e) {

    webui_return_int(e, 123456);
}
```

Full C Example: https://github.com/webui-dev/webui/tree/main/examples/C/call_c_from_js


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### return_float

Return a double-precision float as the response to a JavaScript `await` call.

```js
async function myJavaScript() {
    var f = Number(await callback());
    console.log(f); // 123.456
}
```

```c
void callback(webui_event_t* e) {

    webui_return_float(e, 123.456);
}
```

Full C Example: https://github.com/webui-dev/webui/tree/main/examples/C/call_c_from_js


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### return_string

Return a string as the response to a JavaScript `await` call.

```js
async function myJavaScript() {
    var name = await callback();
    console.log(name); // "Foo Bar"
}
```

```c
void callback(webui_event_t* e) {

    webui_return_string(e, "Foo Bar");
}
```

Full C Example: https://github.com/webui-dev/webui/tree/main/examples/C/call_c_from_js


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### return_bool

Return a boolean as the response to a JavaScript `await` call.

```js
async function myJavaScript() {
    var status = Boolean(Number(await callback()));
    console.log(status); // true
}
```

```c
void callback(webui_event_t* e) {

    webui_return_bool(e, true);
}
```

Full C Example: https://github.com/webui-dev/webui/tree/main/examples/C/call_c_from_js


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

```c
#include "webui.h"

int main() {

    /*
    * @param window The window number
    * @param function The JavaScript function name
    * @param raw The raw data buffer
    * @param size The raw data size in bytes
    */

    unsigned char buffer[3] = { 0x01, 0x02, 0x03 };
    webui_send_raw(win, "myJavaScriptFunc", buffer, 3);
}
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

```c
#include "webui.h"

void callback(webui_event_t* e) {

    /*
    * @param e The event struct
    * @param function The JavaScript function name
    * @param raw The raw data buffer
    * @param size The raw data size in bytes
    */

    unsigned char buffer[3] = { 0x01, 0x02, 0x03 };
    webui_send_raw_client(e, "myJavaScriptFunc", buffer, 3);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_file_handler

Set a custom handler to serve files. The handler receives the requested filename and must return a complete HTTP response (headers + body). Replaces any handler set with `set_file_handler_window`.

```c
#include "webui.h"

const void* myHandler(const char* filename, int* length) {
    if (strcmp(filename, "/hello.txt") == 0) {
        const char* response =
            "HTTP/1.1 200 OK\r\n"
            "Content-Type: text/plain\r\n\r\n"
            "Hello!";
        *length = strlen(response);
        return response;
    }
    return NULL; // fall back to default file serving
}

int main() {

    /*
    * @param window The window number
    * @param handler The handler function
    */

    webui_set_file_handler(win, myHandler);
}
```

Full C Example: https://github.com/webui-dev/webui/tree/main/examples/C/serve_a_folder


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_file_handler_window

Same as `set_file_handler`, but the handler also receives the window number. Useful when the same handler is shared across multiple windows. Replaces any handler set with `set_file_handler`.

```c
#include "webui.h"

const void* myHandler(size_t window, const char* filename, int* length) {
    printf("Window %zu requested: %s\n", window, filename);
    return NULL; // fall back to default file serving
}

int main() {

    /*
    * @param window The window number
    * @param handler The handler function
    */

    webui_set_file_handler_window(win, myHandler);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_root_folder

Set the web-server root folder path for a specific window.

> Should be called before `show()`.

```c
#include "webui.h"

int main() {

    /*
    * @param window The window number
    * @param path The local folder full path
    */

    webui_set_root_folder(win, "/home/Foo/Bar/");
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_default_root_folder

Set the web-server root folder path for all windows.

> Should be called before `show()`.

```c
#include "webui.h"

int main() {

    /*
    * @param path The local folder full path
    */

    webui_set_default_root_folder("/home/Foo/Bar/");
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_mime_type

Get the HTTP MIME type string for a given file extension.

```c
#include "webui.h"

int main() {

    /*
    * @param file A filename with extension
    */

    const char* mime = webui_get_mime_type("foo.png");
    // mime == "image/png"
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_best_browser

Get the recommended web browser ID to use for this window. If a browser is already in use, returns that browser's ID.

```c
#include "webui.h"

int main() {

    /*
    * @param window The window number
    */

    size_t browserID = webui_get_best_browser(win);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### browser_exist

Check whether a specific web browser is installed on the system.

```c
#include "webui.h"

int main() {

    /*
    * @param browser The browser ID from webui_browser enum
    */

    if (webui_browser_exist(Chrome)) {
        webui_show_browser(win, "index.html", Chrome);
    } else {
        webui_show(win, "index.html");
    }
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_profile

Set the web browser profile to use. An empty `name` and `path` uses the default user profile.

> Must be called before `show()`.

```c
#include "webui.h"

int main() {

    /*
    * @param window The window number
    * @param name The web browser profile name
    * @param path The web browser profile full path
    */

    webui_set_profile(win, "MyAppProfile", "/Home/Foo/Bar");

    // Use default profile:
    // webui_set_profile(win, "", "");
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_proxy

Set the web browser proxy server.

> Must be called before `show()`.

```c
#include "webui.h"

int main() {

    /*
    * @param window The window number
    * @param proxy_server The proxy server address
    */

    webui_set_proxy(win, "http://127.0.0.1:8888");
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_browser_folder

Set a custom folder path where WebUI should look for the browser executable.

```c
#include "webui.h"

int main() {

    /*
    * @param path The browser folder path
    */

    webui_set_browser_folder("/opt/my-browser/");
    webui_show(win, "index.html");
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### delete_profile

Delete the local web-browser profile folder for a specific window.

> Recommended to call after all windows are closed, before `clean()`.

> [!] This can break functionality of other windows using the same browser profile.

```c
#include "webui.h"

int main() {

    webui_wait();

    /*
    * @param window The window number
    */

    webui_delete_profile(win);
    webui_clean();
    return 0;
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### delete_all_profiles

Delete all local web-browser profile folders created by WebUI.

> Recommended to call after all windows are closed, before `clean()`.

> [!] This can break functionality of running windows using those profiles.

```c
#include "webui.h"

int main() {

    webui_wait();
    webui_delete_all_profiles();
    webui_clean();
    return 0;
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_port

Get the network port used by the running window's web server. Useful for constructing the `webui.js` URL when integrating with an external server.

```c
#include "webui.h"

int main() {

    /*
    * @param window The window number
    */

    size_t port = webui_get_port(win);
    printf("WebUI JS URL: http://localhost:%zu/webui.js\n", port);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_port

Set a specific network port for the window's web server. Useful when integrating with an external web server like NGINX.

```c
#include "webui.h"

int main() {

    /*
    * @param window The window number
    * @param port The port number to use
    */

    bool ret = webui_set_port(win, 8080);

    if (!ret) {
        printf("Port 8080 is busy.\n");
    }

    // Window URL will be: http://localhost:8080/
    webui_show(win, "index.html");
}
```

Full C Example: https://github.com/webui-dev/webui/tree/main/examples/C/custom_web_server


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_free_port

Find and return an available (unused) network port.

```c
#include "webui.h"

int main() {

    size_t port = webui_get_free_port();
    printf("Free port: %zu\n", port);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_parent_process_id

Get the process ID of the backend application (the parent process). The web browser may create additional child processes.

```c
#include "webui.h"

int main() {

    /*
    * @param window The window number
    */

    size_t parent_pid = webui_get_parent_process_id(win);
    printf("Backend PID: %zu\n", parent_pid);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_child_process_id

Get the process ID of the browser window child process. In WebView mode, returns the parent PID because backend and WebView run in the same process.

```c
#include "webui.h"

int main() {

    /*
    * @param window The window number
    */

    size_t child_pid = webui_get_child_process_id(win);
    printf("Browser PID: %zu\n", child_pid);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_hwnd

Get the native window handle. On Windows returns `HWND` (works with both WebView and web browser). On Linux returns `GtkWindow*` (WebView only).

```c
#include "webui.h"

int main() {

    /*
    * @param window The window number
    */

    void* hwnd = webui_get_hwnd(win);

    // On Windows: HWND hwnd = (HWND) webui_get_hwnd(win);
    // On Linux:   GtkWindow* w = (GtkWindow*) webui_get_hwnd(win);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### win32_get_hwnd

Get the Win32 `HWND` window handle. More reliable with WebView than with a web browser, as browser PIDs may change on launch.

> Windows only.

```c
#include "webui.h"

int main() {

    /*
    * @param window The window number
    */

    HWND hwnd = (HWND) webui_win32_get_hwnd(win);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### malloc

Safely allocate memory using the WebUI memory management system. The allocated buffer can be freed at any time with `webui_free()`.

```c
#include "webui.h"

int main() {

    /*
    * @param size The number of bytes to allocate
    */

    char* buffer = (char*) webui_malloc(1024);

    // Use buffer...

    // Free when done
    webui_free(buffer);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### free

Safely free a buffer previously allocated by `webui_malloc()` or returned by WebUI APIs like `webui_encode()` and `webui_decode()`.

```c
#include "webui.h"

int main() {

    char* base64 = webui_encode("Hello World");

    /*
    * @param ptr The buffer to free
    */

    webui_free(base64);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### memcpy

Copy raw bytes from a source buffer to a destination buffer.

```c
#include "webui.h"

int main() {

    char src[64] = "Hello WebUI";
    char dst[64];

    /*
    * @param dest Destination memory pointer
    * @param src  Source memory pointer
    * @param count Number of bytes to copy
    */

    webui_memcpy(dst, src, 64);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### encode

Encode a string to Base64. The returned buffer must be freed with `webui_free()`.

```c
#include "webui.h"

int main() {

    /*
    * @param str The null-terminated string to encode
    */

    char* base64 = webui_encode("Foo Bar");
    printf("Encoded: %s\n", base64);

    webui_free(base64);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### decode

Decode a Base64-encoded string. The returned buffer must be freed with `webui_free()`.

```c
#include "webui.h"

void callback(webui_event_t* e) {

    // JavaScript:
    // callback(btoa("Hello World"));

    const char* base64 = webui_get_string(e);

    /*
    * @param str The null-terminated Base64 string to decode
    */

    char* str = webui_decode(base64);
    printf("Decoded: %s\n", str);

    webui_free(str);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_tls_certificate

Set the SSL/TLS certificate and private key (both in PEM format). If called with empty strings, WebUI generates a self-signed certificate.

> This works only with the TLS build of WebUI (`webui-2-secure`).

```c
#include "webui.h"

int main() {

    /*
    * @param certificate_pem The SSL/TLS certificate content in PEM format
    * @param private_key_pem The private key content in PEM format
    */

    bool ret = webui_set_tls_certificate(
        "-----BEGIN CERTIFICATE-----\n...",
        "-----BEGIN PRIVATE KEY-----\n..."
    );

    if (!ret) {
        printf("Invalid TLS certificate.\n");
    }

    // All windows will now use HTTPS instead of HTTP
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_logger

Set a custom logging function to receive WebUI's internal log messages. Useful for debugging or integrating with your own logging system.

```c
#include "webui.h"

/*
    enum webui_logger_level {
        WEBUI_LOGGER_LEVEL_DEBUG = 0, // All logs with full details
        WEBUI_LOGGER_LEVEL_INFO,      // Only general logs
        WEBUI_LOGGER_LEVEL_ERROR,     // Only fatal error logs
    };
*/

void myLogger(size_t level, const char* log, void* user_data) {
    printf("[WebUI L%zu] %s\n", level, log);
}

int main() {

    /*
    * @param func The logger callback function
    * @param user_data Any user data to pass to the logger
    */

    webui_set_logger(myLogger, NULL);
    webui_show(win, "index.html");
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_last_error_number

Get the error code from the most recent WebUI operation that failed.

```c
#include "webui.h"

int main() {

    if (!webui_show(win, "index.html")) {
        size_t err = webui_get_last_error_number();
        printf("Error code: %zu\n", err);
    }
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_last_error_message

Get the human-readable error message from the most recent WebUI operation that failed.

```c
#include "webui.h"

int main() {

    if (!webui_show(win, "index.html")) {
        const char* msg = webui_get_last_error_message();
        printf("Error: %s\n", msg);
    }
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### clean

Free all memory resources used by WebUI. Should be called only once at the very end of your application, after `wait()` returns.

```c
#include "webui.h"

int main() {

    webui_show(win, "index.html");
    webui_wait();
    webui_clean();
    return 0;
}
```
