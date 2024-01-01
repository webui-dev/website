<div align="center">

![ScreenShot](data/webui_240_shadow.png)

# WebUI v2.4.2 Documentation

> Use any web browser as GUI, with your preferred language in the backend and HTML5 in the frontend, all in a lightweight portable library.

![ScreenShot](data/screenshot.png)

</div>

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### Download And Install

<p style="text-align: justify;">The WebUI library is written in pure C, but there is a wrapper in every popular programming language to provide easy-to-use APIs to write your backend application, while HTML5 and JavaScript are always used in the frontend inside a web browser window.</p>

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
You can [download](https://github.com/webui-dev/webui/releases) the latest pre-release binaries from the GitHub release section, or follow those instructions to build WebUI from the source.

```sh
# Clone
git clone https://github.com/webui-dev/webui.git
cd webui

# Windows - Microsoft Visual Studio
nmake -f Makefile.nmake

# Windows - MinGW
mingw32-make

# Linux - GCC
make

# Linux Clang
make COMPILER=clang

# macOS Clang
make
```
The compiled binaries will be generated in `webui/dist` folder.<br>
More details [here](https://github.com/webui-dev/webui#build).
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
You can [download](https://github.com/webui-dev/webui/releases) the latest pre-release binaries from the GitHub release section, or follow those instructions to build WebUI from the source.

```sh
# Clone
git clone https://github.com/webui-dev/webui.git
cd webui

# Windows - Microsoft Visual Studio
nmake -f Makefile.nmake

# Windows - MinGW
mingw32-make

# Linux - GCC
make

# Linux Clang
make COMPILER=clang

# macOS Clang
make
```
The compiled binaries will be generated in `webui/dist` folder.<br>
More details [here](https://github.com/webui-dev/webui#build).
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```sh
pip install webui2
```
<!-- ---------- -->
#### **Deno**
<!-- ---------- -->
```sh
import { WebUI } from "https://deno.land/x/webui/mod.ts";
```
<!-- ---------- -->
#### **Go**
<!-- ---------- -->
```sh
go get github.com/webui-dev/go-webui/v2/@latest
```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```sh
nimble install webui
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```sh
v install https://github.com/webui-dev/v-webui
```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```sh
git submodule add https://github.com/webui-dev/odin-webui.git webui
webui/setup.sh
```
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->

1. Add dependency

For zig `nightly`, use this:

```sh
# for zig nightly 
zig fetch --save https://github.com/webui-dev/zig-webui/archive/main.tar.gz
```

For zig `0.11.0`, add this to `build.zig.zon`:

```zig
.@"zig-webui" = .{
        .url = "https://github.com/webui-dev/zig-webui/archive/main.tar.gz",
        .hash = <hash value>,
    },
```

This tells zig to fetch zig-webui from a tarball provided by GitHub. Make sure to replace the COMMIT part with an actual commit SHA in long form, like 219faa2a5cd5a268a865a1100e92805df4b84610. Every time you want to update zig-webui you'll have to update this commit.

2. Config `build.zig`

Add this to `build.zig`:

```zig
const zig_webui = b.dependency("zig-webui", .{
    .target = target,
    .optimize = optimize,
    .enable_tls = false, // whether enable tls support
    .is_static = true, // whether static link
});

// add module
exe.addModule("webui", zig_webui.module("webui"));

// link library
exe.linkLibrary(zig_webui.artifact("webui"));
```

<!-- ---------- -->
#### **Other...**
<!-- ---------- -->
**Rust**
<!-- ---------- -->
```sh
// In development...
```
<!-- ---------- -->
**Pascal**
<!-- ---------- -->
```sh
// In development...
```
<!-- ---------- -->
**Bun**
<!-- ---------- -->
```sh
// In development...
```
<!-- ---------- -->
**Basic**
<!-- ---------- -->
```sh
// In development...
```
<!-- ---------- -->
<!-- tabs:end -->

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### Examples

Minimal example

<!-- tabs:start -->
#### **C**
```c
#include "webui.h"

int main() {

	size_t myWindow = webui_new_window();
	webui_show(myWindow, "<html><script src=\"webui.js\"></script> Hello World from C! </html>");
	webui_wait();
	return 0;
}
```
[More C Examples](https://github.com/webui-dev/webui/tree/main/examples/C).
#### **C++**
```cpp
#include "webui.hpp"

int main() {

	webui::window myWindow;
	myWindow.show("<html><script src=\"webui.js\"></script> Hello World from C++! </html>");
	webui::wait();
	return 0;
}
```
[More C++ Examples](https://github.com/webui-dev/webui/tree/main/examples/C%2B%2B).
#### **Python**
```python
from webui import webui

myWindow = webui.window()
myWindow.show('<html><script src="webui.js"></script> Hello World from Python! </html>')
webui.wait()
```
[More Python Examples](https://github.com/webui-dev/python-webui/tree/main/examples).
#### **Deno**
```ts
import { WebUI } from "https://deno.land/x/webui/mod.ts";

const myWindow = new WebUI();
myWindow.show('<html><script src="webui.js"></script> Hello World from Deno! </html>');
await WebUI.wait();
```
[More Deno Examples](https://github.com/webui-dev/deno-webui/tree/main/examples/).
#### **Go**
```go
package main

import "github.com/webui-dev/go-webui"

func main() {
	myWindow := webui.NewWindow()
	myShow("<html><script src=\"webui.js\"></script> Hello World from Go! </html>")
	webui.Wait()
}
```
[More Go Examples](https://github.com/webui-dev/go-webui/tree/main/examples).
#### **Nim**
```nim
import webui

let window = newWindow()
myWindow.show("<html><script src=\"webui.js\"></script> Hello World from Nim! </html>")
wait()
```
[More Nim Examples](https://github.com/webui-dev/nim-webui/tree/main/examples).
#### **V**
```v
import vwebui as ui

mut myWindow := ui.new_window()
myWindow.show('<html><script src="webui.js"></script> Hello World from V! </html>')!
ui.wait()
```
[More V Examples](https://github.com/webui-dev/v-webui/tree/main/examples).
#### **Odin**
```odin
package main

import ui "webui/webui.odin"

main :: proc() {
	myWindow := ui.new_window()
	ui.show(myWindow, "<html><script src=\"webui.js\"></script> Hello World from Odin! </html>")
	ui.wait()
}
```
[More Odin Examples](https://github.com/webui-dev/odin-webui/tree/main/examples).
#### **Zig**
```zig
const webui = @import("webui");

pub fn main() !void {
    var nwin = webui.newWindow();
    _ = nwin.show("<html><head><script src=\"webui.js\"></script></head> Hello World ! </html>");
    webui.wait();
}
```
#### **Other...**
**Rust**
```sh
// In development...
```
**Pascal**
```sh
// In development...
```
**Bun**
```sh
// In development...
```
**Basic**
```sh
// In development...
```
<!-- tabs:end -->

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### New Window

Creating a new WebUI window object.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
size_t myWindow = webui_new_window();
```
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
webui::window myWindow;
```
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python
myWindow = webui.window()
```
<!-- ---------- -->
#### **Deno**
<!-- ---------- -->
```ts
const myWindow = new WebUI();
```
<!-- ---------- -->
#### **Go**
<!-- ---------- -->
```go
myWindow := webui.NewWindow()
```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
let myWindow = newWindow()
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v
mut myWindow := ui.new_window()
```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```odin
myWindow := ui.new_window()
```
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->
```zig
var new_window = webui.newWindow();
```
<!-- ---------- -->
#### **Other...**
<!-- ---------- -->
**Rust**
<!-- ---------- -->
```rust
// In development...
```
<!-- ---------- -->
**Pascal**
<!-- ---------- -->
```pascal
// In development...
```
<!-- ---------- -->
**Bun**
<!-- ---------- -->
```ts
// In development...
```
<!-- ---------- -->
**Basic**
<!-- ---------- -->
```basic
// In development...
```
<!-- ---------- -->
<!-- tabs:end -->


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### Show Window

You can show the WebUI window, which is a web browser window. If the window is already shown, the UI will get refreshed by the new content in the same window.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
webui_show(myWindow, "<html><script src=\"/webui.js\"> ... </html>");

webui_show(myWindow, "file.html");

webui_show(myWindow, "https://mydomain.com");

webui_show_browser(myWindow, "<html><script src=\"/webui.js\"> ... </html>", Chrome);
```

<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
myWindow.show("<html><script src=\"/webui.js\"> ... </html>");

myWindow.show("file.html");

myWindow.show("https://mydomain.com");

myWindow.show_browser("<html><script src=\"/webui.js\"> ... </html>", Chrome);
```

<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python
myWindow.show('<html><script src="/webui.js"> ... </html>')

myWindow.show('file.html')

myWindow.show('https://mydomain.com')

myWindow.show('<html><script src="/webui.js"> ... </html>', webui.browser.chrome)
```
<!-- ---------- -->
#### **Deno**
<!-- ---------- -->
```ts
myWindow.show('<html><script src="/webui.js"> ... </html>')

myWindow.show('file.html')

myWindow.show('https://mydomain.com')

myWindow.showBrowser('<html><script src="/webui.js"> ... </html>', WebUI.Browser.Chrome)
```
<!-- ---------- -->
#### **Go**
<!-- ---------- -->
```go
myWindow.Show("<html><script src=\"/webui.js\"> ... </html>")

myWindow.Show("file.html")

myWindow.Show("https://mydomain.com")

myWindow.ShowBrowser("<html><script src=\"/webui.js\"> ... </html>", webui.Chrome)
```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
myWindow.show("<html><script src=\"/webui.js\"> ... </html>")

myWindow.show("file.html")

myWindow.show("https://mydomain.com")

myWindow.show("<html><script src=\"/webui.js\"> ... </html>", Browsers.Chrome)
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v
myWindow.show('<html><script src="/webui.js"> ... </html>')

myWindow.show('file.html')

myWindow.show('https://mydomain.com')

myWindow.show_browser('<html><script src="/webui.js"> ... </html>', .chrome)
```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```odin
ui.show(myWindow, "<html><script src=\"/webui.js\"> ... </html>");

ui.show(myWindow, "file.html");

ui.show(myWindow, "https://mydomain.com");

ui.show_browser(myWindow, <html><script src=\"/webui.js\"> ... </html>", Chrome);
```
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->
```zig
const successed = myWindow.show("<html><script src=\"webui.js\"> ... </html>");

const successed = myWindow.show("file.html");

const successed = myWindow.show("https://mydomain.com");

const successed = myWindow.showBrowser("<html><script src=\"webui.js\"> ... </html>", .Chrome);
```
<!-- ---------- -->
#### **Other...**
<!-- ---------- -->
**Rust**
<!-- ---------- -->
```rust
// In development...
```
<!-- ---------- -->
**Pascal**
<!-- ---------- -->
```pascal
// In development...
```
<!-- ---------- -->
**Bun**
<!-- ---------- -->
```ts
// In development...
```
<!-- ---------- -->
**Basic**
<!-- ---------- -->
```basic
// In development...
```
<!-- ---------- -->
<!-- tabs:end -->

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### Window Status

To know if a specific window is running.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
if(webui_is_shown(myWindow))
    printf("The window is still running");
else
    printf("The window is closed.");
```
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
if(my_window.is_shown())
    std::cout << "The window is still running" << std::endl;
else
    std::cout << "The window is closed." << std::endl;
```
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python
if MyWindow.is_shown():
	print("The window is still running")
else
	print("The window is closed.")
```
<!-- ---------- -->
#### **Deno**
<!-- ---------- -->
```ts
if (MyWindow.isShown)
	console.log("The window is still running")
else
	console.log("The window is closed.")
```
<!-- ---------- -->
#### **Go**
<!-- ---------- -->
```go
if my_window.IsShown() {
    fmt.Printf("The window is still running")
} else {
    fmt.Printf("The window is closed.")
}
```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
if myWindow.shown():
  echo "A window is running..."
else:
  echo "No window is running."
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v
if webui.is_shown(my_window) {
	println('The window is still running')
} else {
	println('The window is closed.')
}
```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```odin
// In development...
```
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->
```zig
if (new_window.isShown()) {
    std.debug.print("The window is still running", .{});
} else {
    std.debug.print("The window is closed.", .{});
}
```
<!-- ---------- -->
#### **Other...**
<!-- ---------- -->
**Rust**
<!-- ---------- -->
```rust
// In development...
```
<!-- ---------- -->
**Pascal**
<!-- ---------- -->
```pascal
// In development...
```
<!-- ---------- -->
**Bun**
<!-- ---------- -->
```ts
// In development...
```
<!-- ---------- -->
**Basic**
<!-- ---------- -->
```basic
// In development...
```
<!-- ---------- -->
<!-- tabs:end -->

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### Embedded Icon

To embed an icon (_String format_) whtin the application.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
// SVG Icon
const char* myIcon = "<svg>...</svg>";
const char* myIconType = "image/svg+xml"

// PNG Icon
// const char* myIcon = "data:image/...";
// const char* myIconType = "image/png"

// When the web browser ask for `favicon.ico`, WebUI will
// send a redirection to `favicon.svg`, the body will be `myIcon`
// and the mime-type will be `myIconType`
webui_set_icon(myWindow, myIcon, myIconType);
```
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp

```
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python

```
<!-- ---------- -->
#### **Deno**
<!-- ---------- -->
```ts

```
<!-- ---------- -->
#### **Go**
<!-- ---------- -->
```go

```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
# SVG Icon
let
  myIcon = "<svg>...</svg>";
  myIconType = "image/svg+xml"

# PNG Icon
# const char* myIcon = "data:image/...";
# const char* myIconType = "image/png"

# When the web browser ask for `favicon.ico`, WebUI will
# send a redirection to `favicon.svg`, the body will be `myIcon`
# and the mime-type will be `myIconType`
myWindow.setIcon(myIcon, myIconType)
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v

```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```odin
// In development...
```
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->
```zig
// SVG Icon
const my_icon = "<svg>...</svg>";
const my_icon_type = "image/svg+xml";
// PNG Icon
// const my_icon = "data:image/...";
// const my_icon_type = "image/png";

// When the web browser ask for `favicon.ico`, WebUI will
// send a redirection to `favicon.svg`, the body will be `my_icon`
// and the mime-type will be `my_icon_type`
new_window.setIcon(my_icon, my_icon_type);
```
<!-- ---------- -->
#### **Other...**
<!-- ---------- -->
**Rust**
<!-- ---------- -->
```rust
// In development...
```
<!-- ---------- -->
**Pascal**
<!-- ---------- -->
```pascal
// In development...
```
<!-- ---------- -->
**Bun**
<!-- ---------- -->
```ts
// In development...
```
<!-- ---------- -->
**Basic**
<!-- ---------- -->
```basic
// In development...
```
<!-- ---------- -->
<!-- tabs:end -->

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### Bind

To receive click events when the user clicks on any HTML element with a specific ID, for example `<button id="MyID">Hello</button>`.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
void my_function(webui_event_t* e) {
    // <button id="MyID">Hello</button> gets clicked!
}

webui_bind(myWindow, "MyID", my_function);
```
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
void my_function(webui::event* e) {
    // <button id="MyID">Hello</button> gets clicked!
}

my_window.bind("MyID", my_function);
```

Using `webui::bind()` to call a class member method

```cpp
class MyClass {
    public: void my_function(webui::event* e) {
        // <button id="MyID">Hello</button> gets clicked!
    }
};

// Wrapper:
// Because WebUI is written in C, so it can not
// access `MyClass` directly. That's why we should
// create a simple C++ wrapper.
MyClass obj;
void my_function_wrapper(webui::event* e) { obj.my_function(e); }

my_window.bind("MyID", my_function_wrapper);
```
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python
def my_function(e : webui.event):
	# <button id="MyID">Hello</button> gets clicked!

MyWindow.bind("MyID", my_function)
```
<!-- ---------- -->
#### **Deno**
<!-- ---------- -->
```ts
webui.bind("MyID", ({ data }) => {
  console.log(`ui send "${data}"`);
  return "backend response";
});
```
<!-- ---------- -->
#### **Go**
<!-- ---------- -->
```go
func my_function(e webui.Event) string {
	// <button id="MyID">Hello</button> gets clicked!
	return ""
}

webui.Bind(my_window, "MyID", my_function)
```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
myWindow.bind("MyID") do (e: Event):
  # <button id="MyID">Hello</button> gets clicked!
  echo "Binding element ", e.element, "!"
```

Instead of using `do` notation, you can also define a proc separately and
bind it manually:

```nim
proc exitApp(e: Event) =
  exit()

myWindow.bind(exitApp)
```

You can also have a return value on your function, it must be either a `string`, `int`, or `bool`. The return value will be automatically passed back to the Javascript code for you.

```nim
myWindow.bind("MyID") do (e: Event) -> int:
  return 1 + 2  # 3
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v
import vwebui as webui

fn my_function_string(e &webui.Event) {
	// <button id="MyID">Hello</button> gets clicked!
}

my_window.bind("MyID", my_function)
```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```odin
// In development...
```
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->
```zig
fn myFunction(e: webui.Event) void {
    // <button id="MyID">Hello</button> gets clicked!
}

my_window.bind("MyID", myFunction);
```
<!-- ---------- -->
#### **Other...**
<!-- ---------- -->
**Rust**
<!-- ---------- -->
```rust
// In development...
```
<!-- ---------- -->
**Pascal**
<!-- ---------- -->
```pascal
// In development...
```
<!-- ---------- -->
**Bun**
<!-- ---------- -->
```ts
// In development...
```
<!-- ---------- -->
**Basic**
<!-- ---------- -->
```basic
// In development...
```
<!-- ---------- -->
<!-- tabs:end -->

### Events

Every event comes with the `e` object that contains the event information, like click, function call, connect, disconnect...

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
// webui_event_t
//    size_t window;       // The window object number
//    size_t event_type;   // Event type
//    char* element;       // HTML element ID
//    size_t event_number; // Internal WebUI
//    size_t bind_id;      // Bind ID

void my_function(webui_event_t* e){

    printf("Hi!, You clicked on %s element\n", e.element);

    if(e->event_type == WEBUI_EVENT_CONNECTED)
        printf("Connected. \n");
    else if(e->event_type == WEBUI_EVENT_DISCONNECTED)
        printf("Disconnected. \n");
    else if(e->event_type == WEBUI_EVENT_MOUSE_CLICK)
        printf("Click. \n");
    else if(e->event_type == WEBUI_EVENT_NAVIGATION)
        printf("Starting navigation to: %s \n", e->data);    

    // Send back a response to JavaScript
    webui_return_int(e, 123); // As integer
    webui_return_bool(e, true); // As boolean
    webui_return_string(e, "My Response"); // As string
}

// Empty ID means all events on all elements
webui_bind(myWindow, "", my_function);
```
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
void my_function(webui::event* e){

    std::cout << "Hi!, You clicked on " << e.element << std::endl;

    if (e->event_type == webui::CONNECTED)
        std::cout << "Connected." << std::endl;
    else if (e->event_type == webui::DISCONNECTED)
        std::cout << "Disconnected." << std::endl;
    else if (e->event_type == webui::MOUSE_CLICK)
        std::cout << "Click." << std::endl;
    else if (e->event_type == webui::NAVIGATION)
        std::cout << "Starting navigation to: " << e->data << std::endl;

    // Send back a response to JavaScript
    e->window.return_int(e, 123); // As integer
    e->window.return_bool(e, true); // As boolean
    e->window.return_string(e, "My Response"); // As string
}

// Empty ID means all events
my_window.bind("", my_function);
```
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python
def events(e : webui.event):
	print('Hi!, You clicked on ' + e.element + ' element')

# Empty ID means all events on all elements
MyWindow.bind("", events)
```
<!-- ---------- -->
#### **Deno**
<!-- ---------- -->
```ts
function handleEvent(event: WebUI.Event) {
  console.log(`new ui event was fired (${event.element})`);
}

webui.bind("", handleEvent);
```
<!-- ---------- -->
#### **Go**
<!-- ---------- -->
```go
func my_function(e webui.Event) string {
	fmt.Printf("Hi!, You clicked on element: %s\n", e.Element)
	return ""
}

// Empty ID means all events on all elements
webui.Bind(my_window, "", events)
```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
# Empty ID means bind all events on all elements
myWindow.bind("") do (e: Event):
  echo "Hi!, You clicked on ", e.element, " element"
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v
fn my_function(e &webui.Event) {
	// Get target element
	target_element := e.element
}
```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```odin
// In development...
```
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->
```zig
// Event
//    window_handle: usize,     // The window handler
//    event_type: Events,       // Event type
//    element: []u8,            // HTML element ID
//    event_number: usize,      // Internal WebUI
//    bind_id: usize,           // Bind ID

fn myFunction(e: webui.Event) void {

    std.debug.print("Hi!, You clicked on {s} element\n", e.element);

    switch (e.event_type) {
        .EVENT_CONNECTED => {
            std.debug.print("Connected. \n", .{});
        },
        .EVENT_DISCONNECTED => {
            std.debug.print("Disconnected. \n", .{});
        },
        .EVENT_MOUSE_CLICK => {
            std.debug.print("Click. \n", .{});
        },
        .EVENT_NAVIGATION => {
            const url = webui.getString(e);
            std.debug.print("Starting navigation to: {s} \n", .{url});
        },
        else => {},
    }

    // Send back a response to JavaScript
    webui.returnInt(e, 123); // As integer
    webui.returnBool(e, true); // As boolean
    webui.returnString(e, "My Response"); // As string
}

// Empty ID means all events on all elements
my_window.bind("", myFunction);
```
<!-- ---------- -->
#### **Other...**
<!-- ---------- -->
**Rust**
<!-- ---------- -->
```rust
// In development...
```
<!-- ---------- -->
**Pascal**
<!-- ---------- -->
```pascal
// In development...
```
<!-- ---------- -->
**Bun**
<!-- ---------- -->
```ts
// In development...
```
<!-- ---------- -->
**Basic**
<!-- ---------- -->
```basic
// In development...
```
<!-- ---------- -->
<!-- tabs:end -->

### Files Handler

You can generate files dynamicaly, his become handy when you would like to create a REST APIs for your UI.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
const void* my_files_handler(const char* filename, int* length) {

    printf("File: %s \n", filename);

    if(!strcmp(filename, "/test.txt")) {

        // Const static file example
        // Note: The connection will drop if the content
        // does not have `<script src="/webui.js"></script>`
        return "This is a embedded file content example.";
    }
    else if(!strcmp(filename, "/dynamic.html")) {

        // Dynamic file example

        // Safely allocate memory
        char* dynamic_content = webui_malloc(1024);

        // Generate a test content
        static int count = 0;
        sprintf(
            dynamic_content,
            "<html>"
            "   This is a dynamic file content example. <br>"
            "   Count: %d <a href=\"dynamic.html\">[Refresh]</a><br>"
            "   <script src=\"/webui.js\"></script>" // To keep connection with WebUI
            "</html>",
            ++count
        );

        // Set len (optional for strings)
        *length = strlen(dynamic_content);

        // By allocating resources using webui_malloc()
        // WebUI will automaticaly free the resources
        return dynamic_content;
    }

    // A NULL return will make WebUI
    // looks for the file locally. if
    // not, WebUI will generate `404`
    return NULL;
}

// Set a custom files handler
webui_set_file_handler(myWindow, my_files_handler);
```
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp

```
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python

```
<!-- ---------- -->
#### **Deno**
<!-- ---------- -->
```ts

```
<!-- ---------- -->
#### **Go**
<!-- ---------- -->
```go

```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
var count: int

myWindow.setFileHandler() do (filename: string) -> string:
  echo "File: ", filename

  case filename
  of "/test.txt":
    # Const static file example
    # Note: The connection will drop if the content
    # does not have `<script src="/webui.js"></script>`

    return "This is a embedded file content example."
  of "/dynamic.html":
    # Dynamic file example
    inc count

    return fmt"""
<html>
  This is a dynamic file content example.
  <br>
  Count: {count} <a href="dynamic.html">[Refresh]</a>
  <br>

  <script src="/webui.js"></script>
</html>
"""

  # By default, this function returns an empty string
  # returning an empty string will make WebUI look for 
  # the requested file locally
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v

```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```odin
// In development...
```
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->
```zig
var count: i32 = 0;

fn my_files_handler(filename: []const u8) ?[]u8 {

    std.debug.print("File: {s}\n", .{filename});

    if (std.mem.eql(u8, filename, "/test.txt")) {

        // Const static file example
        // Note: The connection will drop if the content
        // does not have `<script src="/webui.js"></script>`
        return "This is a embedded file content example.";
    } else if (std.mem.eql(u8, filename, "/dynamic.html")) {

        // Dynamic file example

        // Safely allocate memory
        var dynamic_content = webui.malloc(1024);

        for (0..dynamic_content.len) |i| {
            dynamic_content[i] = 0;
        }

        count += 1;

        // Generate a test content
        const buf = std.fmt.bufPrint(dynamic_content,
            \\  <html>
            \\      This is a dynamic file content example. <br>
            \\	    Count: {} <a href="dynamic.html">[Refresh]</a><br>
            \\	    <script src="webui.js"></script> 
            \\  </html>
            // webui.js, to keep connection with WebUI
        , .{count}) catch unreachable;

        // By allocating resources using webui.malloc()
        // WebUI will automaticaly free the resources
        return buf;
    }

    // A NULL return will make WebUI
    // looks for the file locally. if
    // not, WebUI will generate `404`
    return null;
}

// Set a custom files handler
my_window.setFileHandler(my_files_handler);
```
<!-- ---------- -->
#### **Other...**
<!-- ---------- -->
**Rust**
<!-- ---------- -->
```rust
// In development...
```
<!-- ---------- -->
**Pascal**
<!-- ---------- -->
```pascal
// In development...
```
<!-- ---------- -->
**Bun**
<!-- ---------- -->
```ts
// In development...
```
<!-- ---------- -->
**Basic**
<!-- ---------- -->
```basic
// In development...
```
<!-- ---------- -->
<!-- tabs:end -->

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### Wait

It is essential to call the `wait` function at the end of your primary function after you create/show all your windows. This will make your application run until the user closes all visible windows or when calling *[exit()](/?id=exit)*. You can show again the same windows, create a new one, or call the `wait` function again.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
int main() {

	// Create windows...
	// Bind HTML elements...
    // Show the windows...

    // Wait until all windows get closed
    // or when calling webui_exit()
	webui_wait();

    return 0;
}
```
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
int main() {

	// Create windows...
	// Bind HTML elements...
    // Show the windows...

    // Wait until all windows get closed
    // or when calling webui::exit()
	webui::wait();

    return 0;
}
```
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python
# Create windows...
# Bind HTML elements...
# Show the windows...

# Wait until all windows get closed
# or when calling MyWindow.exit()
webui.wait()
```
<!-- ---------- -->
#### **Deno**
<!-- ---------- -->
```ts
// Create windows...
// Bind HTML elements...
// Show the windows...

// Wait until all windows get closed
// or when calling WebUI.exit()
await WebUI.wait();
```
<!-- ---------- -->
#### **Go**
<!-- ---------- -->
```go
// Create windows...
// Bind HTML elements...
// Show the windows...

// Wait until all windows get closed
// or when calling webui.Exit()
webui.Wait()
```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
# Create windows...
# Bind HTML elements...
# Show the windows...

# Wait until all windows get closed
# or when calling exit()
wait()
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v
fn main() {
	// Create windows...
	// Bind HTML elements...
	// Show the windows...
	// Show a window using the embedded HTML

	// Wait until all windows get closed
	// or when calling webui.exit()
	my_window.wait()
}
```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```odin
// In development...
```
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->
```zig
// Create windows...
// Bind HTML elements...
// Show the windows...

// Wait until all windows get closed
// or when calling webui.exit()
webui.wait();
```
<!-- ---------- -->
#### **Other...**
<!-- ---------- -->
**Rust**
<!-- ---------- -->
```rust
// In development...
```
<!-- ---------- -->
**Pascal**
<!-- ---------- -->
```pascal
// In development...
```
<!-- ---------- -->
**Bun**
<!-- ---------- -->
```ts
// In development...
```
<!-- ---------- -->
**Basic**
<!-- ---------- -->
```basic
// In development...
```
<!-- ---------- -->
<!-- tabs:end -->

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### Exit

You can call `exit` at any moment, which tries to close all opened windows and make *[wait](/?id=wait)* break. All WebUI memory resources will be freed, which makes WebUI unusable.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
webui_exit();
```
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
webui::exit();
```
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python
webui.exit()
```
<!-- ---------- -->
#### **Deno**
<!-- ---------- -->
```ts
WebUI.exit();
```
<!-- ---------- -->
#### **Go**
<!-- ---------- -->
```go
webui.Exit()
```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
exit()
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v
webui.exit()
```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```odin
// In development...
```
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->
```zig
webui.exit();
```
<!-- ---------- -->
#### **Other...**
<!-- ---------- -->
**Rust**
<!-- ---------- -->
```rust
// In development...
```
<!-- ---------- -->
**Pascal**
<!-- ---------- -->
```pascal
// In development...
```
<!-- ---------- -->
**Bun**
<!-- ---------- -->
```ts
// In development...
```
<!-- ---------- -->
**Basic**
<!-- ---------- -->
```basic
// In development...
```
<!-- ---------- -->
<!-- tabs:end -->

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### Close

You can call `close` to close a specific window, if there is no running window left *[wait](/?id=wait)* will break.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
webui_close(myWindow);
```
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
my_window.close();
```
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python
MyWindow.close()
```
<!-- ---------- -->
#### **Deno**
<!-- ---------- -->
```ts
myWindow.close();
```
<!-- ---------- -->
#### **Go**
<!-- ---------- -->
```go
webui.Close(my_window)
```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
myWindow.close()
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v
my_window.close()
```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```odin
// In development...
```
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->
```zig
my_window.close();
```
<!-- ---------- -->
#### **Other...**
<!-- ---------- -->
**Rust**
<!-- ---------- -->
```rust
// In development...
```
<!-- ---------- -->
**Pascal**
<!-- ---------- -->
```pascal
// In development...
```
<!-- ---------- -->
**Bun**
<!-- ---------- -->
```ts
// In development...
```
<!-- ---------- -->
**Basic**
<!-- ---------- -->
```basic
// In development...
```
<!-- ---------- -->
<!-- tabs:end -->

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### Startup Timeout

WebUI waits a couple of seconds (_Default is 30 seconds_) to let the web browser start and connect. You can control this behavior by using `timeout`.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
// Wait 10 seconds for the browser to start
webui_set_timeout(10);

// Now, After 10 seconds, if the browser did
// not get started, wait() will break
webui_wait();
```

```c
// Wait forever.
webui_set_timeout(0);

// webui_wait() will never end
webui_wait();
```
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp

```
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python

```
<!-- ---------- -->
#### **Deno**
<!-- ---------- -->
```ts

```
<!-- ---------- -->
#### **Go**
<!-- ---------- -->
```go

```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
# Wait 10 seconds for the browser to start
setTimeout(10);

# Now, After 10 seconds, if the browser did
# not get started, wait() will break
wait();
```

```nim
# Wait forever.
setTimeout(0);

# wait() will never end
wait();
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v

```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```odin
// In development...
```
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->
```zig
// Wait 10 seconds for the browser to start
webui.setTimeout(10);

// Now, After 10 seconds, if the browser did
// not get started, wait() will break
webui.wait();
```

```zig
// Wait forever.
webui.setTimeout(0);

// webui.wait() will never end
webui.wait();
```
<!-- ---------- -->
#### **Other...**
<!-- ---------- -->
**Rust**
<!-- ---------- -->
```rust
// In development...
```
<!-- ---------- -->
**Pascal**
<!-- ---------- -->
```pascal
// In development...
```
<!-- ---------- -->
**Bun**
<!-- ---------- -->
```ts
// In development...
```
<!-- ---------- -->
**Basic**
<!-- ---------- -->
```basic
// In development...
```
<!-- ---------- -->
<!-- tabs:end -->

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### Run JavaScript From Backend

You can run JavaScript on any window to read values, update the view, or anything else. In addition, you can check if the script execution has errors, as well as reading the received data.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
void my_function(webui_event_t* e){

	// Create a buffer to hold the response
    char response[64];

    // Run JavaScript
    if(!webui_script(
        e->window, // Window
        "return 2*2;", // JavaScript to be executed
        0, // Maximum waiting time in second
        response, // Local buffer to hold the JavaScript response
        64) // Size of the local buffer
    ) {
        printf("JavaScript Error: %s\n", response);
        return;
    }

    // Print the result
    printf("JavaScript Response: %s\n", response); // 4

    // Run JavaScript quickly with no waiting for the response
    webui_run(e->window, "alert('Fast!');");
}
```
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
void my_function(webui::event* e){

	// Create a buffer to hold the response
    char response[64];

    // This is another way to create a buffer:
    //  std::string buffer;
    //  buffer.reserve(64);
    //  my_window.script(..., ..., &buffer[0], 64);

    // Run JavaScript
    if(!e->window.script(
        "return 2*2;", // JavaScript to be executed
        0, // Maximum waiting time in second
        response, // Local buffer to hold the JavaScript response
        64) // Size of the local buffer
    ) {
        std::cout << "JavaScript Error: " << response << std::endl;
        return;
    }

    // Print the result
    std::cout << "JavaScript Response: " << response << std::endl; // 4

    // Run JavaScript quickly with no waiting for the response
    e->window.run("alert('Fast!');");
}
```
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python
# Run JavaScript to get the password
res = e.window.script("return 2*2;")

# Check for any error
if res.error is True:
	print("JavaScript Error: " + res.data)
else:
	print("JavaScript Response: " + res.data) # 4

# Run JavaScript quickly with no waiting for the response
e.window.run("alert('Fast!')")
```
<!-- ---------- -->
#### **Deno**
<!-- ---------- -->
```ts
const response = await webui.script('return 2*2;').catch(
  console.error,
);
// response == "4"
```
<!-- ---------- -->
#### **Go**
<!-- ---------- -->
```go
// Create new JavaScript object
js := webui.NewJavaScript()

// Run the script
if !webui.Script(e.Window, &js, "return 2*2;") {
    // Error
    fmt.Printf("JavaScript Error: %s\n", js.Response)
}

// Print the Response
fmt.Printf("JavaScript Response: %s\n", js.Response)

// Run JavaScript quickly with no waiting for the response
webui.Run(e.Window, "alert('Fast!')")
```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
myWindow.bind("ExampleElement") do (e: Event):
  # Run JavaScript to get the result
  let res = e.window.script("return 2*2;")

  # Check for any error
  if res.error:
    echo "JavaScript Error: ", res.data
  else:
    echo "JavaScript Response: ", res.data # 4

  # Run JavaScript quickly with no waiting for the response
  e.window.run("alert('Fast!')")
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v
fn my_function(e &webui.Event) {
	// Run JavaScript
	response := e.window.script('return 2*2;', 0, 64)

	// Print the result
	println('JavaScript Response: ${response.int()}') // 4

	// Run JavaScript quickly with no waiting for the response
	e.window.run("alert('Fast!');")
}
```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```odin
// In development...
```
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->
```zig
fn myFunction(e: webui.Event) void {
    // Create a buffer to hold the response
    var response: [64]u8 = std.mem.zeroes([64]u8);

    var tmp_e = e;
    var win = tmp_e.getWindow();

    // Run JavaScript
    if (!win.script("return 2*2;", // JavaScript to be executed
        0, // Maximum waiting time in second
        &response // Local buffer to hold the JavaScript response
    )) {
        std.debug.print("JavaScript Error: {s}\n", .{response});
        return;
    }

    // Print the result
    std.debug.print("JavaScript Response: {s}\n", .{response}); // 4

    // Run JavaScript quickly with no waiting for the response
    win.run("alert('Fast!');");
}
```
<!-- ---------- -->
#### **Other...**
<!-- ---------- -->
**Rust**
<!-- ---------- -->
```rust
// In development...
```
<!-- ---------- -->
**Pascal**
<!-- ---------- -->
```pascal
// In development...
```
<!-- ---------- -->
**Bun**
<!-- ---------- -->
```ts
// In development...
```
<!-- ---------- -->
**Basic**
<!-- ---------- -->
```basic
// In development...
```
<!-- ---------- -->
<!-- tabs:end -->

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### Call a Backend Function From JavaScript

To call a backend function from JavaScript and get the result back, please use `myBackendFunction(arguments... ).then((response) => { ... });`.<br>
The backend function is also availbale at `webui.myBackendFunction(arguments... ).then((response) => { ... });`.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
void my_c_function(webui_event_t* e) {

    // Reading the first argument from JavaScript
    const char* str = webui_get_string_at(e, 0);

    // Print the received data
    printf("Data from JavaScript: %s\n", str); // Message from JS

    // Return back a response to JavaScript
    webui_return_string(e, "Message from C");

    // Other functions:
    //
    // long long number = webui_get_int_at(e, 0);
    // bool status = webui_get_bool_at(e, 0);
    //
    // webui_return_int(e, number);
    // webui_return_bool(e, true);
}

webui_bind(myWindow, "my_c_function", my_c_function);
```

JavsScript:

```js
my_c_function('Message from JS').then((response) => {
    console.log(response); // "Message from C
});

// Or

webui.my_c_function('Message from JS').then((response) => {
    console.log(response); // "Message from C
});

// Or

webui.call('my_c_function', 'Message from JS').then((response) => {
    console.log(response); // "Message from C
});
```
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```c
void my_cpp_function(webui::event* e) {

    // Get data from JavaScript
    std::string str = e->data;
    // Or 
    // std::string str = e->window.get_string(e);
    // long long number = e->window.get_int(e);
    // bool status = e->window.get_bool(e);

    // Print the received data
    std::cout << "Data from JavaScript: " << str << std::endl; // Message from JS

    // Return back a response to JavaScript
    e->window.return_string(e, "Message from C++");
    // e->window.return_int(e, number);
    // e->window.return_bool(e, true);
}

my_window.bind("my_cpp_function", my_cpp_function);
```

JavsScript:

```js
my_cpp_function('Message from JS').then((response) => {
    console.log(response); // "Message from C
});

// Or

webui.my_cpp_function('Message from JS').then((response) => {
    console.log(response); // "Message from C
});

// Or

webui.call('my_cpp_function', 'Message from JS').then((response) => {
    console.log(response); // "Message from C
});
```
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python
# webui.event
#    window # Window (Object)
#    event_type # Event Type (Number)
#    element # Element name (String)
#    event_num # Event Number (Number)
#    bind_id # Bind ID (Number)

def my_python_function(e : webui.event):
	print("Data from JavaScript: " + e.window.get_str(e, 0)) # Message from JS
    return "Message from Python"

MyWindow.bind("my_python_function", my_python_function)
```

JavsScript:

```js
my_python_function('Message from JS').then((response) => {
    console.log(response); // "Message from C
});

// Or

webui.my_python_function('Message from JS').then((response) => {
    console.log(response); // "Message from C
});

// Or

webui.call('my_python_function', 'Message from JS').then((response) => {
    console.log(response); // "Message from C
});
```
<!-- ---------- -->
#### **Deno**
<!-- ---------- -->
```ts
myWindow.bind("my_deno_function", ({ data }) => {
  console.log(`ui send "${data}"`);
  return "backend response";
});
```

JavsScript:

```js
my_deno_function('Message from JS').then((response) => {
    console.log(response); // "Message from C
});

// Or

webui.my_deno_function('Message from JS').then((response) => {
    console.log(response); // "Message from C
});

// Or

webui.call('my_deno_function', 'Message from JS').then((response) => {
    console.log(response); // "Message from C
});
```
<!-- ---------- -->
#### **Go**
<!-- ---------- -->
```go
func my_go_function(e webui.Event) string {
	
    // Get data from JavaScript
    fmt.Printf("Data from JavaScript: %s\n", e.Data) // Message from JS

    // Return back a response to JavaScript
    return "Message from Go"
}

webui.Bind(my_window, "my_go_function", my_go_function)
```

JavsScript:

```js
my_go_function('Message from JS').then((response) => {
    console.log(response); // "Message from C
});

// Or

webui.my_go_function('Message from JS').then((response) => {
    console.log(response); // "Message from C
});

// Or

webui.call('my_go_function', 'Message from JS').then((response) => {
    console.log(response); // "Message from C
});
```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
myWindow.bind("my_nim_function") do (e: Event) -> string:
  echo "Data from JavaScript: ", e.data # Message from JS

  return "Message from Nim"
```

JavaScript:

```js
my_nim_function('Message from JS').then((response) => {
    console.log(response); // "Message from C
});

// Or

webui.my_nim_function('Message from JS').then((response) => {
    console.log(response); // "Message from C
});

// Or

webui.call('my_nim_function', 'Message from JS').then((response) => {
    console.log(response); // "Message from C
});
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v
import vwebui as webui

mut my_window := webui.new_window()

fn my_v_function(e &webui.Event) webui.Response {
	str := e.data.string
	// number = e.data.int
	// status = e.data.bool

	// Print the received data
	println('Data from JavaScript: ${str}') // Message from JS

	// Return a response to JavaScript
	// WebUI will handle types automatically
	return 'Message from V'
}

my_window.bind('my_v_function', my_v_function)
```

JavsScript:

```js
my_v_function('Message from JS').then((response) => {
    console.log(response); // "Message from C
});

// Or

webui.my_v_function('Message from JS').then((response) => {
    console.log(response); // "Message from C
});

// Or

webui.call('my_v_function', 'Message from JS').then((response) => {
    console.log(response); // "Message from C
});
```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```odin
// In development...
```
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->
```zig
fn myCFunction(e: webui.Event) void {

    // Reading the first argument from JavaScript
    const str = webui.getStringAt(e, 0);

    // get string length
    const str_len = webui.str_len(str);

    // Print the received data
    std.debug.print("Data from JavaScript: {s}\n", .{str[0..str_len]}); // Message from JS
    
    // Return back a response to JavaScript
    webui.returnString(e, "Message from C");

    // Other functions:
    //
    // var number = webui.getIntAt(e, 0);
    // var status = webui.getBoolAt(e, 0);
    //
    // webui.returnInt(e, number);
    // webui.returnBool(e, true);
}

my_window.bind("my_c_function", myCFunction);
```

JavsScript:

```js
my_v_function('Message from JS').then((response) => {
    console.log(response); // "Message from C
});

// Or

webui.my_v_function('Message from JS').then((response) => {
    console.log(response); // "Message from C
});

// Or

webui.call('my_v_function', 'Message from JS').then((response) => {
    console.log(response); // "Message from C
});
```
<!-- ---------- -->
#### **Other...**
<!-- ---------- -->
**Rust**
<!-- ---------- -->
```rust
// In development...
```
<!-- ---------- -->
**Pascal**
<!-- ---------- -->
```pascal
// In development...
```
<!-- ---------- -->
**Bun**
<!-- ---------- -->
```ts
// In development...
```
<!-- ---------- -->
**Basic**
<!-- ---------- -->
```basic
// In development...
```
<!-- ---------- -->
<!-- tabs:end -->

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### TypeScript Runtimes

You may want to interpret JavaScript & TypeScript files and show the output in the UI. You can use `set_runtime` and choose between `Deno` or `Nodejs` as your runtimes.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
// Deno
webui_set_runtime(myWindow, Deno);
webui_show(myWindow, "my_file.ts");

// Nodejs
webui_set_runtime(myWindow, Nodejs);
webui_show(myWindow, "my_file.js");
```
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
// Deno
my_window.set_runtime(Deno);
my_window.show("my_file.ts");

// Nodejs
my_window.set_runtime(Nodejs);
my_window.show("my_file.js");
```
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python
# Deno
MyWindow.set_runtime(webui.runtime.deno)
MyWindow.show("my_file.ts")

# Nodejs
MyWindow.set_runtime(webui.runtime.nodejs)
MyWindow.show("my_file.js")
```
<!-- ---------- -->
#### **Deno**
<!-- ---------- -->
```ts
// Seriously...
```
<!-- ---------- -->
#### **Go**
<!-- ---------- -->
```go
// Deno
webui.SetRuntime(my_window, webui.Deno)
webui.Show(my_window, "my_file.ts")

// Nodejs
webui.SetRuntime(my_window, webui.Nodejs)
webui.Show(my_window, "my_file.js")
```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
# Deno
myWindow.runtime = Deno
myWindow.show("my_file.ts")

# Nodejs
myWindow.runtime = NodeJS
myWindow.show("my_file.js")
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v
// Deno
my_window.set_runtime(.runtime_deno)
my_window.show("my_file.ts")

// Nodejs
my_window.set_runtime(.runtime_nodejs)
my_window.show("my_file.js")
```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```odin
// In development...
```
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->
```zig
// Deno
my_window.setRuntime(.Deno);
my_window.show("my_file.ts");

// Nodejs
my_window.setRuntime(.Nodejs);
my_window.show("my_file.js");
```
<!-- ---------- -->
#### **Other...**
<!-- ---------- -->
**Rust**
<!-- ---------- -->
```rust
// In development...
```
<!-- ---------- -->
**Pascal**
<!-- ---------- -->
```pascal
// In development...
```
<!-- ---------- -->
**Bun**
<!-- ---------- -->
```ts
// In development...
```
<!-- ---------- -->
**Basic**
<!-- ---------- -->
```basic
// In development...
```
<!-- ---------- -->
<!-- tabs:end -->
