<div align="center">

![Logo](data/webui_120.svg)

# WebUI v2.5 - C++ Documentation

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

```cpp
#include "webui.hpp"

int main() {

    webui::window win;
    win.show("<html><script src=\"webui.js\"></script> Hello World from C++! </html>");
    webui::wait();
    return 0;
}
```
[More C++ Examples](https://github.com/webui-dev/webui/tree/main/examples/C%2B%2B).


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### new_window

Create a new window object.

```cpp
#include "webui.hpp"

int main() {

    webui::window win;

    // Later
    win.show("index.html");
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### new_window_id

Create a new window object using a specified ID.

> The `id` should be between 1 and `WEBUI_MAX_IDS`

```cpp
#include "webui.hpp"

int main() {

    size_t id = 1;
    webui::window win(id);

    // Later
    win.show("index.html");
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_new_window_id

Get a free window ID that can be used with `new_window_id` to create a new window object.

```cpp
#include "webui.hpp"

int main() {

    size_t id = webui::get_new_window_id();

    // Later
    webui::window win(id);
    win.show("index.html");
}
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

> To use only a specific browser please use `show_browser()`

> To use only WebView please use `show_wv()`

```cpp
#include "webui.hpp"

int main() {

    /*
    * @param content The HTML, URL, local file, or local folder.
    *                Leave empty to use the current root folder.
    */

    win.show("index.html");
}
```

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### show_browser

Show a window using a specific web browser.

> It's recommended to use `ChromiumBased` browser.

> On macOS, the browser's icon may still appear in the Dock after exit. We recommend using `show_wv` on macOS to avoid this.

```cpp
#include "webui.hpp"

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
    * @param content The HTML, URL, local file, or local folder
    * @param browser The web browser to be used
    */

    win.show_browser("index.html", Chrome);
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

```cpp
#include "webui.hpp"

int main() {

    /*
    * @param content The HTML, URL, or a local file
    */

    win.show_wv("index.html");
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### show_client

Show a window for a specific single client. Useful in multi-client mode to send different content to different connected clients.

```cpp
#include "webui.hpp"

void callback(webui::window::event* e) {

    /*
    * @param content The HTML, URL, or a local file
    */

    e->show_client("<html>...</html>");
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### start_server

Start only the local web server without opening a browser window, and return the URL. Useful for headless web app scenarios.

```cpp
#include "webui.hpp"

int main() {

    /*
    * @param content The local file or folder to serve.
    *                Leave empty to use the current root folder.
    */

    std::string_view url = win.start_server("/full/root/path");
    std::cout << "Server URL: " << url << std::endl;
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### close

Close a specific window. The window object will still exist and can be shown again later.

```cpp
#include "webui.hpp"

int main() {

    win.close();
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### close_client

Close the connection for a specific single client only, without closing the window for other connected clients.

```cpp
#include "webui.hpp"

void callback(webui::window::event* e) {

    e->close_client();
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### destroy

Close a specific window and free all related memory resources.

> This will make the `win` object unavailable. If you want to simply close a window and reopen it later, please use `close()` instead.

```cpp
#include "webui.hpp"

int main() {

    win.destroy();
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### exit

Close all open windows. This will make `wait()` return (_Break_).

```cpp
#include "webui.hpp"

int main() {

    webui::exit();
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### wait

Block the current thread and wait until all opened windows get closed.

```cpp
#include "webui.hpp"

int main() {

    win.show("index.html");
    webui::wait();
    return 0;
}
```

Full C++ Example: https://github.com/webui-dev/webui/tree/main/examples/C%2B%2B/minimal


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### wait_async

Non-blocking alternative to `wait()`. Returns `true` if at least one window is still open, `false` when all windows are closed. Call this in a loop from the main thread to interleave your own main-thread work.

> In WebView mode, you must call this from the main thread.

```cpp
#include "webui.hpp"

int main() {

    win.show_wv("index.html");

    while (webui::wait_async()) {
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

```cpp
#include "webui.hpp"

int main() {

    if (win.is_shown()) {
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

```cpp
#include "webui.hpp"

int main() {

    win.focus();
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### minimize

Minimize a WebView window.

```cpp
#include "webui.hpp"

int main() {

    win.minimize();
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### maximize

Maximize a WebView window.

```cpp
#include "webui.hpp"

int main() {

    win.maximize();
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_kiosk

Set the window in Kiosk mode (_Full screen_).

```cpp
#include "webui.hpp"

int main() {

    /*
    * @param status True or False
    */

    win.set_kiosk(true);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_size

Set the window size.

```cpp
#include "webui.hpp"

int main() {

    /*
    * @param width The window width
    * @param height The window height
    */

    win.set_size(800, 600);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_minimum_size

Set the minimum allowed window size. The user will not be able to resize the window smaller than this.

> Currently works on Windows only.

```cpp
#include "webui.hpp"

int main() {

    /*
    * @param width The minimum window width
    * @param height The minimum window height
    */

    win.set_minimum_size(400, 300);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_position

Set the window position on screen.

```cpp
#include "webui.hpp"

int main() {

    /*
    * @param x The window X coordinate
    * @param y The window Y coordinate
    */

    win.set_position(100, 100);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_center

Center the window on the screen.

> Call this before `show()` for best results. Works better with WebView.

```cpp
#include "webui.hpp"

int main() {

    win.set_center();
    win.show("index.html");
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_hide

Start the window in hidden mode. The window will be running but not visible.

> Should be called before `show()`.

```cpp
#include "webui.hpp"

int main() {

    /*
    * @param status True or False
    */

    win.set_hide(true);
    win.show("index.html");
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_frameless

Remove the window frame and title bar (borderless/frameless mode).

> Works with WebView windows only.

```cpp
#include "webui.hpp"

int main() {

    /*
    * @param status True or False
    */

    win.set_frameless(true);
    win.show_wv("index.html");
}
```

Full C++ Example: https://github.com/webui-dev/webui/tree/main/examples/C%2B%2B/frameless


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_transparent

Enable or disable window background transparency.

> Works with WebView windows only. The web content must also use a transparent background for this to be visible.

```cpp
#include "webui.hpp"

int main() {

    /*
    * @param status True or False
    */

    win.set_transparent(true);
    win.show_wv("index.html");
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_resizable

Control whether the user can resize the window.

> Works with WebView windows only.

```cpp
#include "webui.hpp"

int main() {

    /*
    * @param status True to allow resizing, False to fix the size
    */

    win.set_resizable(false);
    win.show_wv("index.html");
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_icon

Set the default embedded HTML favicon. The icon is served automatically by WebUI's built-in server.

```cpp
#include "webui.hpp"

int main() {

    /*
    * @param icon The icon as string: `<svg>...</svg>`
    * @param icon_type The icon MIME type: `image/svg+xml`
    */

    win.set_icon("<svg>...</svg>", "image/svg+xml");
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_high_contrast

Mark the window as supporting high-contrast mode. Use this together with CSS to build a better high-contrast theme.

```cpp
#include "webui.hpp"

int main() {

    /*
    * @param status True or False
    */

    win.set_high_contrast(true);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### is_high_contrast

Check if the operating system is currently using a high-contrast theme.

```cpp
#include "webui.hpp"

int main() {

    if (webui::is_high_contrast()) {
        // OS is using a high-contrast theme
        win.set_high_contrast(true);
    }
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_custom_parameters

Add custom command-line parameters to the browser launch command.

```cpp
#include "webui.hpp"

int main() {

    /*
    * @param params Command line parameters string
    */

    win.set_custom_parameters("--remote-debugging-port=9222");
    win.show("index.html");
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_close_handler_wv

Set a callback to intercept the close event of a WebView window. Return `false` from the handler to prevent the window from closing; return `true` to allow it.

```cpp
#include "webui.hpp"

bool myCloseHandler(size_t window) {
    std::cout << "User tried to close the window" << std::endl;
    return false; // Prevent closing
}

int main() {

    /*
    * @param close_handler The handler function
    */

    win.set_close_handler_wv(myCloseHandler);
    win.show_wv("index.html");
    webui::wait();
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### navigate

Navigate all connected clients of a window to a specific URL.

```cpp
#include "webui.hpp"

int main() {

    /*
    * @param url Full HTTP URL or relative path
    */

    win.navigate("/foo/bar.html");

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

```cpp
#include "webui.hpp"

void callback(webui::window::event* e) {

    /*
    * @param url Full HTTP URL or relative path
    */

    e->navigate_client("/other-page.html");
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_url

Get the current URL of a running window's web server.

> By default, WebUI only allows access to this URL from localhost.

```cpp
#include "webui.hpp"

int main() {

    std::string_view url = win.get_url();
    std::cout << "Window URL: " << url << std::endl;
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### open_url

Open a URL in the operating system's default web browser (not a WebUI window).

```cpp
#include "webui.hpp"

int main() {

    webui::open_url("https://webui.me");
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_public

Allow the window's web server URL to be accessible from external devices on the network.

```cpp
#include "webui.hpp"

int main() {

    /*
    * @param status True or False
    */

    win.set_public(true);

    // Now, the URL of the window is accessible
    // from any device/mobile on the network.
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### bind

Bind an HTML element click event or a JavaScript function call to a C++ callback. Use an empty element name to catch all events from the window. Supports lambdas and class member functions.

```cpp
#include "webui.hpp"

class MyApp {
public:
    void onButtonClick(webui::window::event* e) {
        std::cout << "Button clicked!" << std::endl;
    }
};

void myFunction(webui::window::event* e) {
    // Called from JavaScript: myFunction("hello", 42);
}

int main() {

    MyApp app;

    /*
    * @param element The HTML element ID or JavaScript function name
    * @param func The callback function or lambda
    */

    // Bind a free function
    win.bind("myFunction", myFunction);

    // Bind a lambda
    win.bind("greet", [](webui::window::event* e) {
        std::cout << "Hello, " << e->get_string() << "!" << std::endl;
    });

    // Bind a class member function
    win.bind("click", &app, &MyApp::onButtonClick);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### event

Every event callback receives a `webui::window::event*` pointer with information about the current event: window reference, type, element name, client ID, and more.

```cpp
#include "webui.hpp"

void events(webui::window::event* e) {

    if (e->event_type == WEBUI_EVENT_CONNECTED)
        std::cout << "Connected." << std::endl;
    else if (e->event_type == WEBUI_EVENT_DISCONNECTED)
        std::cout << "Disconnected." << std::endl;
    else if (e->event_type == WEBUI_EVENT_MOUSE_CLICK)
        std::cout << "Click on: " << e->get_element() << std::endl;
    else if (e->event_type == WEBUI_EVENT_NAVIGATION)
        std::cout << "Navigating to: " << e->get_string() << std::endl;
    else if (e->event_type == WEBUI_EVENT_CALLBACK)
        std::cout << "Function call." << std::endl;

    // Access window object
    webui::window& win = e->get_window();
}

int main() {

    win.bind("", events); // Catch all window events
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_context

Attach arbitrary user data to a specific binding. The data can be retrieved later inside the callback using `e->get_context()`. Must be called after `bind()`.

```cpp
#include "webui.hpp"

struct UserInfo {
    int id;
    std::string name;
};

void myFunction(webui::window::event* e) {
    auto* info = static_cast<UserInfo*>(e->get_context());
    std::cout << "User: " << info->name << " (ID: " << info->id << ")" << std::endl;
}

int main() {

    UserInfo user{ 42, "Alice" };

    /*
    * @param element The HTML element / JavaScript function name
    * @param context Any user data pointer
    */

    win.bind("myFunction", myFunction);
    win.set_context("myFunction", &user);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_context

Retrieve the user data pointer previously attached to a binding with `set_context()`.

```cpp
#include "webui.hpp"

void myFunction(webui::window::event* e) {

    void* data = e->get_context();
    auto* info = static_cast<UserInfo*>(data);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_event_blocking

Control whether UI events from this window are processed one at a time in a single blocking thread (`true`), or each in a new non-blocking thread (`false`). Use `webui::set_config(ui_event_blocking, ...)` to apply to all windows.

```cpp
#include "webui.hpp"

void foo(webui::window::event* e) { /* long task */ }
void bar(webui::window::event* e) { /* runs after foo */ }

int main() {
    win.bind("foo", foo);
    win.bind("bar", bar);

    /*
    * @param status True for blocking (serial), False for non-blocking (parallel)
    */

    win.set_event_blocking(true);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_config

Control WebUI global behaviour. It's recommended to call this at the beginning, before `show()`.

```cpp
#include "webui.hpp"

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
        // responses via return_x() after the callback returns.
        // Default: false
        asynchronous_response,
    } webui_config;
*/

int main() {

    /*
    * @param option The desired option from webui_config enum
    * @param status The status: true or false
    */

    webui::set_config(show_wait_connection, true);
    webui::set_config(ui_event_blocking, false);
    webui::set_config(folder_monitor, true);
    webui::set_config(multi_client, false);
    webui::set_config(use_cookies, true);
    webui::set_config(asynchronous_response, false);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_timeout

Set the maximum time in seconds to wait for the browser window to connect after calling `show()`. A value of `0` means wait forever.

```cpp
#include "webui.hpp"

int main() {

    /*
    * @param second The timeout in seconds (0 = no timeout)
    */

    webui::set_timeout(30);

    win.show("index.html");
    webui::wait();
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### run

Run JavaScript in the window without waiting for a response. Sends to all connected clients.

```cpp
#include "webui.hpp"

void callback(webui::window::event* e) {

    // Run JavaScript for one specific client only
    e->run_client("alert('Hello from backend!');");
}

int main() {

    /*
    * @param script The JavaScript to be run
    */

    // Run JavaScript for all connected clients
    win.run("alert('Hello from backend!');");
}
```

Full C++ Example: https://github.com/webui-dev/webui/tree/main/examples/C%2B%2B/call_js_from_cpp


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### run_client

Run JavaScript for a specific single client without waiting for a response.

```cpp
#include "webui.hpp"

void callback(webui::window::event* e) {

    /*
    * @param script The JavaScript to be run
    */

    e->run_client("document.title = 'Updated';");
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### script

Run JavaScript and get the response back. Make sure your buffer is large enough to hold the response.

```cpp
#include "webui.hpp"

int main() {

    /*
    * @param script The JavaScript to be run
    * @param timeout The execution timeout in seconds (0 = no timeout)
    * @param buffer The local buffer to hold the response
    * @param buffer_length The local buffer size
    */

    char response[64];
    if (!win.script("return 4 + 6;", 0, response, 64)) {
        std::cout << "JavaScript Error: " << response << std::endl;
    }
    else {
        std::cout << "JavaScript Response: " << response << std::endl; // 10
    }
}
```

Full C++ Example: https://github.com/webui-dev/webui/tree/main/examples/C%2B%2B/call_js_from_cpp


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### script_client

Run JavaScript for a specific single client and get the response back.

```cpp
#include "webui.hpp"

void callback(webui::window::event* e) {

    /*
    * @param script The JavaScript to be run
    * @param timeout The execution timeout in seconds (0 = no timeout)
    * @param buffer The local buffer to hold the response
    * @param buffer_length The local buffer size
    */

    char response[64];
    if (!e->script_client("return document.title;", 0, response, 64)) {
        std::cout << "JavaScript Error: " << response << std::endl;
    } else {
        std::cout << "Page title: " << response << std::endl;
    }
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_runtime

Choose between `Deno`, `Bun`, and `Nodejs` as the runtime for `.js` and `.ts` files served by the web server.

```cpp
#include "webui.hpp"

int main() {

    /*
    * @param runtime Deno | Bun | NodeJS | None
    */

    win.set_runtime(Deno);
}
```

Full C++ Example: https://github.com/webui-dev/webui/tree/main/examples/C%2B%2B/serve_a_folder


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_count

Get the number of arguments passed to the callback from JavaScript.

```cpp
void callback(webui::window::event* e) {

    // JavaScript:
    // callback("Foo", "Bar", 123, true);

    size_t count = e->get_count(); // 4
}
```

Full C++ Example: https://github.com/webui-dev/webui/tree/main/examples/C%2B%2B/call_cpp_from_js


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_int

Get the first argument as a 64-bit integer.

```cpp
void callback(webui::window::event* e) {

    // JavaScript:
    // callback(123456);

    long long int num = e->get_int();
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_int_at

Get an argument as a 64-bit integer at a specific index.

```cpp
void callback(webui::window::event* e) {

    // JavaScript:
    // callback(12345, 6789);

    long long int n1 = e->get_int(0);
    long long int n2 = e->get_int(1);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_float

Get the first argument as a double-precision float.

```cpp
void callback(webui::window::event* e) {

    // JavaScript:
    // callback(123.456);

    double f = e->get_float();
}
```

Full C++ Example: https://github.com/webui-dev/webui/tree/main/examples/C%2B%2B/call_cpp_from_js


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_float_at

Get an argument as a double-precision float at a specific index.

```cpp
void callback(webui::window::event* e) {

    // JavaScript:
    // callback(12.34, 56.789);

    double f1 = e->get_float(0);
    double f2 = e->get_float(1);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_string

Get the first argument as a `std::string`.

```cpp
void callback(webui::window::event* e) {

    // JavaScript:
    // callback("Foo Bar");

    std::string name = e->get_string();
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_string_at

Get an argument as a `std::string` at a specific index.

```cpp
void callback(webui::window::event* e) {

    // JavaScript:
    // callback("Foo", "Bar");

    std::string foo = e->get_string(0);
    std::string bar = e->get_string(1);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_bool

Get the first argument as a boolean.

```cpp
void callback(webui::window::event* e) {

    // JavaScript:
    // callback(true);

    bool status = e->get_bool();
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_bool_at

Get an argument as a boolean at a specific index.

```cpp
void callback(webui::window::event* e) {

    // JavaScript:
    // callback(true, false);

    bool s1 = e->get_bool(0);
    bool s2 = e->get_bool(1);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_size

Get the size in bytes of the first argument. Useful for raw binary data.

```cpp
void callback(webui::window::event* e) {

    // JavaScript:
    // callback("Foo");

    size_t len = e->get_size();
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_size_at

Get the size in bytes of an argument at a specific index.

```cpp
void callback(webui::window::event* e) {

    // JavaScript:
    // callback("Foo", "Bar");

    size_t fooLen = e->get_size(0);
    size_t barLen = e->get_size(1);
}
```


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

```cpp
void callback(webui::window::event* e) {

    e->return_int(123456);
}
```


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

```cpp
void callback(webui::window::event* e) {

    e->return_float(123.456);
}
```


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

```cpp
void callback(webui::window::event* e) {

    e->return_string("Foo Bar");
}
```


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

```cpp
void callback(webui::window::event* e) {

    e->return_bool(true);
}
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

```cpp
#include "webui.hpp"

int main() {

    /*
    * @param function The JavaScript function name
    * @param raw The raw data buffer
    * @param size The raw data size in bytes
    */

    unsigned char buffer[3] = { 0x01, 0x02, 0x03 };
    win.send_raw("myJavaScriptFunc", buffer, 3);
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

```cpp
#include "webui.hpp"

void callback(webui::window::event* e) {

    /*
    * @param function The JavaScript function name
    * @param raw The raw data buffer
    * @param size The raw data size in bytes
    */

    unsigned char buffer[3] = { 0x01, 0x02, 0x03 };
    e->send_raw_client("myJavaScriptFunc", buffer, 3);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_file_handler

Set a custom handler to serve files. The handler receives the requested filename and must return a complete HTTP response (headers + body). Replaces any handler set with `set_file_handler_window`.

```cpp
#include "webui.hpp"

const void* myHandler(const char* filename, int* length) {
    if (std::string_view(filename) == "/hello.txt") {
        const char* resp =
            "HTTP/1.1 200 OK\r\n"
            "Content-Type: text/plain\r\n\r\n"
            "Hello!";
        *length = strlen(resp);
        return resp;
    }
    return nullptr;
}

int main() {

    /*
    * @param handler The handler function
    */

    win.set_file_handler(myHandler);
}
```

Full C++ Example: https://github.com/webui-dev/webui/tree/main/examples/C%2B%2B/serve_a_folder


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_file_handler_window

Same as `set_file_handler`, but the handler also receives the window number. Replaces any handler set with `set_file_handler`.

```cpp
#include "webui.hpp"

const void* myHandler(size_t window, const char* filename, int* length) {
    std::cout << "Window " << window << " requested: " << filename << std::endl;
    return nullptr;
}

int main() {

    /*
    * @param handler The handler function
    */

    win.set_file_handler_window(myHandler);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_root_folder

Set the web-server root folder path for a specific window.

> Should be called before `show()`.

```cpp
#include "webui.hpp"

int main() {

    /*
    * @param path The local folder full path
    */

    win.set_root_folder("/home/Foo/Bar/");
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_default_root_folder

Set the web-server root folder path for all windows.

> Should be called before `show()`.

```cpp
#include "webui.hpp"

int main() {

    /*
    * @param path The local folder full path
    */

    webui::set_default_root_folder("/home/Foo/Bar/");
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_mime_type

Get the HTTP MIME type string for a given file extension.

```cpp
#include "webui.hpp"

int main() {

    std::string mime = webui::get_mime_type("foo.png");
    // mime == "image/png"
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_best_browser

Get the recommended web browser ID for this window. If a browser is already in use, returns that browser's ID.

```cpp
#include "webui.hpp"

int main() {

    size_t browserID = win.get_best_browser();
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### browser_exist

Check whether a specific web browser is installed on the system.

```cpp
#include "webui.hpp"

int main() {

    if (webui::browser_exist(Chrome)) {
        win.show_browser("index.html", Chrome);
    } else {
        win.show("index.html");
    }
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_profile

Set the web browser profile to use. An empty `name` and `path` uses the default user profile.

> Must be called before `show()`.

```cpp
#include "webui.hpp"

int main() {

    /*
    * @param name The web browser profile name
    * @param path The web browser profile full path
    */

    win.set_profile("MyAppProfile", "/Home/Foo/Bar");

    // Use default profile:
    // win.set_profile("", "");
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_proxy

Set the web browser proxy server.

> Must be called before `show()`.

```cpp
#include "webui.hpp"

int main() {

    /*
    * @param proxy_server The proxy server address
    */

    win.set_proxy("http://127.0.0.1:8888");
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_browser_folder

Set a custom folder path where WebUI should look for the browser executable.

```cpp
#include "webui.hpp"

int main() {

    /*
    * @param path The browser folder path
    */

    webui::set_browser_folder("/opt/my-browser/");
    win.show("index.html");
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### delete_profile

Delete the local web-browser profile folder for a specific window.

> Recommended to call after all windows are closed, before `clean()`.

> [!] This can break functionality of other windows using the same browser profile.

```cpp
#include "webui.hpp"

int main() {

    webui::wait();
    win.delete_profile();
    webui::clean();
    return 0;
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### delete_all_profiles

Delete all local web-browser profile folders created by WebUI.

> Recommended to call after all windows are closed, before `clean()`.

```cpp
#include "webui.hpp"

int main() {

    webui::wait();
    webui::delete_all_profiles();
    webui::clean();
    return 0;
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_port

Get the network port used by the running window's web server.

```cpp
#include "webui.hpp"

int main() {

    size_t port = win.get_port();
    std::cout << "WebUI JS URL: http://localhost:" << port << "/webui.js" << std::endl;
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_port

Set a specific network port for the window's web server.

```cpp
#include "webui.hpp"

int main() {

    /*
    * @param port The port number to use
    */

    bool ret = win.set_port(8080);

    if (!ret) {
        std::cout << "Port 8080 is busy." << std::endl;
    }

    // Window URL will be: http://localhost:8080/
    win.show("index.html");
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_free_port

Find and return an available (unused) network port.

```cpp
#include "webui.hpp"

int main() {

    size_t port = webui::get_free_port();
    std::cout << "Free port: " << port << std::endl;
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_parent_process_id

Get the process ID of the backend application (the parent process).

```cpp
#include "webui.hpp"

int main() {

    size_t parent_pid = win.get_parent_process_id();
    std::cout << "Backend PID: " << parent_pid << std::endl;
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_child_process_id

Get the process ID of the browser window child process. In WebView mode, returns the parent PID.

```cpp
#include "webui.hpp"

int main() {

    size_t child_pid = win.get_child_process_id();
    std::cout << "Browser PID: " << child_pid << std::endl;
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_hwnd

Get the native window handle. On Windows returns `HWND`. On Linux returns `GtkWindow*` (WebView only).

```cpp
#include "webui.hpp"

int main() {

    void* hwnd = win.get_hwnd();

    // On Windows: HWND hwnd = (HWND) win.get_hwnd();
    // On Linux:   GtkWindow* w = (GtkWindow*) win.get_hwnd();
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### win32_get_hwnd

Get the Win32 `HWND` window handle.

> Windows only.

```cpp
#include "webui.hpp"

int main() {

    HWND hwnd = (HWND) win.win32_get_hwnd();
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### malloc

Safely allocate memory using the WebUI memory management system.

```cpp
#include "webui.hpp"

int main() {

    /*
    * @param size The number of bytes to allocate
    */

    char* buffer = (char*) webui::malloc(1024);

    // Use buffer...

    webui::free(buffer);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### free

Safely free a buffer previously allocated by `webui::malloc()` or returned by WebUI APIs.

```cpp
#include "webui.hpp"

int main() {

    std::string b64 = webui::encode("Hello World");

    // webui::encode returns a std::string copy, no manual free needed.
    // But if using the raw C API webui_encode(), free with webui::free().
    char* raw = webui_encode("Hello World");
    webui::free(raw);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### memcpy

Copy raw bytes from a source buffer to a destination buffer.

```cpp
#include "webui.hpp"

int main() {

    char src[64] = "Hello WebUI";
    char dst[64];

    /*
    * @param dest Destination memory pointer
    * @param src  Source memory pointer
    * @param count Number of bytes to copy
    */

    webui::memcpy(dst, src, 64);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### encode

Encode a string to Base64. Returns a `std::string`.

```cpp
#include "webui.hpp"

int main() {

    /*
    * @param str The string to encode
    */

    std::string b64 = webui::encode("Foo Bar");
    std::cout << "Encoded: " << b64 << std::endl;
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### decode

Decode a Base64-encoded string. Returns a `std::string`.

```cpp
#include "webui.hpp"

void callback(webui::window::event* e) {

    // JavaScript:
    // callback(btoa("Hello World"));

    std::string base64 = e->get_string();

    /*
    * @param str The Base64 string to decode
    */

    std::string decoded = webui::decode(base64);
    std::cout << "Decoded: " << decoded << std::endl;
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_tls_certificate

Set the SSL/TLS certificate and private key (both in PEM format). If called with empty strings, WebUI generates a self-signed certificate.

> This works only with the TLS build of WebUI (`webui-2-secure`).

```cpp
#include "webui.hpp"

int main() {

    /*
    * @param certificate_pem The SSL/TLS certificate in PEM format
    * @param private_key_pem The private key in PEM format
    */

    bool ret = webui::set_tls_certificate(
        "-----BEGIN CERTIFICATE-----\n...",
        "-----BEGIN PRIVATE KEY-----\n..."
    );

    if (!ret) {
        std::cout << "Invalid TLS certificate." << std::endl;
    }
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_logger

Set a custom logging function to receive WebUI's internal log messages.

```cpp
#include "webui.hpp"

/*
    enum webui_logger_level {
        WEBUI_LOGGER_LEVEL_DEBUG = 0, // All logs with full details
        WEBUI_LOGGER_LEVEL_INFO,      // Only general logs
        WEBUI_LOGGER_LEVEL_ERROR,     // Only fatal error logs
    };
*/

void myLogger(size_t level, const char* log, void* user_data) {
    std::cout << "[WebUI L" << level << "] " << log << std::endl;
}

int main() {

    /*
    * @param func The logger callback function
    * @param user_data Any user data to pass to the logger
    */

    webui::set_logger(myLogger, nullptr);
    win.show("index.html");
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_last_error_number

Get the error code from the most recent WebUI operation that failed.

```cpp
#include "webui.hpp"

int main() {

    if (!win.show("index.html")) {
        size_t err = webui::get_last_error_number();
        std::cout << "Error code: " << err << std::endl;
    }
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_last_error_message

Get the human-readable error message from the most recent WebUI operation that failed.

```cpp
#include "webui.hpp"

int main() {

    if (!win.show("index.html")) {
        std::string_view msg = webui::get_last_error_message();
        std::cout << "Error: " << msg << std::endl;
    }
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### clean

Free all memory resources used by WebUI. Should be called only once at the very end of your application, after `wait()` returns.

```cpp
#include "webui.hpp"

int main() {

    win.show("index.html");
    webui::wait();
    webui::clean();
    return 0;
}
```
