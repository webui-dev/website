<div align="center">

![Logo](data/webui_120.svg)

# WebUI v2.5 Documentation
***[ ! ] Beta 3 - Not Complete Yet***

> Use any web browser or WebView as GUI, with your preferred language in the backend and modern web technologies in the frontend, all in a lightweight portable library.

![Screenshot](data/screenshot.png)

</div>

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### Download And Install

<p style="text-align: justify;">The WebUI library is written in pure C, but there is a wrapper in every popular programming language to provide easy-to-use APIs to write your backend application, while HTML5 and JavaScript are always used in the frontend inside a web browser or WebView window.</p>

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
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
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
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
# Add odin-webui as a submodule to your project
git submodule add https://github.com/webui-dev/odin-webui.git webui

# Linux/MacOS
webui/setup.sh

# Windows
webui/setup.ps1
```
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->

**Zig `0.11`**

1. Add this to `build.zig.zon`

```zig
.@"zig-webui" = .{
        // It is recommended to replace the following branch with commit id
        .url = "https://github.com/webui-dev/zig-webui/archive/main.tar.gz",
        .hash = <hash value>,
    },
```

This tells zig to fetch zig-webui from a tarball provided by GitHub. Make sure to replace the COMMIT part with an actual commit SHA in long form, like `219faa2a5cd5a268a865a1100e92805df4b84610`. Every time you want to update zig-webui you'll have to update this commit.

2. Config `build.zig`

Add this:

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

**Zig `nightly`**

> To be honest, I don’t recommend using the nightly version because the API of the build system is not yet stable, which means that there may be problems with not being able to build after nightly is updated.

1. Add to `build.zig.zon`

```sh
# It is recommended to replace the following branch with commit id
zig fetch --save https://github.com/webui-dev/zig-webui/archive/main.tar.gz
```

2. Config `build.zig`

Add this:

```zig
const zig_webui = b.dependency("zig-webui", .{
    .target = target,
    .optimize = optimize,
    .enable_tls = false, // whether enable tls support
    .is_static = true, // whether static link
});

// add module
exe.root_module.addImport("webui", zig_webui.module("webui"));
// note: see this issue for the API changes above,
// https://github.com/ziglang/zig/pull/18160

// link library
exe.linkLibrary(zig_webui.artifact("webui"));
```

> It is not recommended to dynamically link libraries under Windows, which may cause some symbol duplication problems.
> see this issue: https://github.com/ziglang/zig/issues/15107

<!-- ---------- -->
#### **Rust**
<!-- ---------- -->
1. Add `webui` to your Cargo dependencies:
  
  ```sh
  webui = { git = "https://github.com/webui-dev/rust-webui/", branch = "main" }

  # Or by git tag
  webui = { git = "https://github.com/webui-dev/rust-webui/", tag = "v2.5.0" }

  # Or by git commit
  webui = { git = "https://github.com/webui-dev/rust-webui/", rev = "a1b2c3d4" }
  ```

2. Bring in the static [WebUI static release](https://github.com/webui-dev/webui/releases) or [build action](https://github.com/webui-dev/webui/actions?query=branch%3Amain) file for your platform and place it in your project's root directory.

<!-- ---------- -->
#### ** PHP **
<!-- ---------- -->

```sh
composer require kingbes/webui
```

<!-- ---------- -->
#### **Other...**
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
**Swift**
<!-- ---------- -->
```sh
// In development...
```
<!-- ---------- -->
**C-Sharp**
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
### Minimal Example

Minimal example

<!-- tabs:start -->
#### **C**
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
#### **C++**
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
#### **Python**
```python
from webui import webui

my_window = webui.Window()
my_window.show('<html><script src="webui.js"></script> Hello World from Python! </html>')
webui.wait()
```
[More Python Examples](https://github.com/webui-dev/python-webui/tree/main/examples).
#### **Deno**
```ts
import { WebUI } from "https://deno.land/x/webui/mod.ts";

const win = new WebUI();
win.show('<html><script src="webui.js"></script> Hello World from Deno! </html>');
await WebUI.wait();
```
[More Deno Examples](https://github.com/webui-dev/deno-webui/tree/main/examples/).
#### **Go**
```go
package main

import "github.com/webui-dev/go-webui"

func main() {
    win := webui.NewWindow()
    win.Show("<html><script src=\"webui.js\"></script> Hello World from Go! </html>")
    webui.Wait()
}
```
[More Go Examples](https://github.com/webui-dev/go-webui/tree/main/examples).
#### **Nim**
```nim
import webui

let window = newWindow()
window.show("""<html><script src="/webui.js"></script> Hello World from Nim! </html>""")
wait()
```
[More Nim Examples](https://github.com/webui-dev/nim-webui/tree/main/examples).
#### **V**
```v
import vwebui as ui

mut win := ui.new_window()
win.show('<html><script src="webui.js"></script> Hello World from V! </html>')!
ui.wait()
```
[More V Examples](https://github.com/webui-dev/v-webui/tree/main/examples).
#### **Odin**
```odin
    // main.odin
    package main

    import ui "webui"

    main :: proc() {
        my_window: uint = ui.new_window()
        ui.show(my_window, "<html> <script src=\"webui.js\"></script> Hello World from Odin! </html>")
        ui.wait()
    }
```
[More Odin Examples](https://github.com/webui-dev/odin-webui/tree/main/examples).
#### **Zig**
```zig
const webui = @import("webui");

pub fn main() !void {
    var win = webui.newWindow();
    _ = win.show("<html><script src=\"webui.js\"></script> Hello World from Zig! </html>");
    webui.wait();
}
```
[More Zig Examples](https://github.com/webui-dev/zig-webui/tree/main/examples).
#### **Rust**
```rust
use webui_rs::webui;

pub fn main() {
  let win = webui::Window::new();
  win.show("<html><script src=\"webui.js\"></script> Hello World from Rust! </html>");
  webui::wait();
}
```
[More Rust Examples](https://github.com/webui-dev/rust-webui/tree/main/examples).

#### **PHP **

```php
require "./vendor/autoload.php";

use Kingbes\Webui;

$Webui = new Webui;
$win = $Webui->newWindow();
$Webui->show($win, "<html><script src=\"webui.js\"></script> Hello World from PHP! </html>");
$Webui->wait();
```

#### **Other...**
**Pascal**
```sh
// In development...
```
**Bun**
```sh
// In development...
```
**Swift**
```sh
// In development...
```
**C-Sharp**
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
### new_window

Create a new window object.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
#include "webui.h"

int main() {

    size_t win = webui_new_window();

    // Later
    webui_show(win, "index.html");
}
```
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
#include "webui.hpp"

int main() {

    webui::window win;

    // Later
    win.show("index.html");
}
```
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python
from webui import webui

# this way of initialization uses "new_window" internally
my_window = webui.Window()

# More code...

webui.wait()
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
let window = newWindow()

# later...
window.show("index.html")
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
package main

import ui "webui"

main :: proc() {
    my_window: uint = ui.new_window()

    // Later
    ui.show(my_window, "index.html")
}
```
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->
```zig
var new_window = webui.newWindow();
```
<!-- ---------- -->
#### **Rust**
<!-- ---------- -->
```rust
let win = webui::Window::new();
```
<!-- ---------- -->
#### **PHP**
<!-- ---------- -->
```php
use Kingbes\Webui;

$Webui = new Webui;
$win = $Webui->newWindow();
// Later
$Webui->show($win, "index.html");
```
<!-- ---------- -->
#### **Other...**
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
**Swift**
<!-- ---------- -->
```swift
// In development...
```
<!-- ---------- -->
**C-Sharp**
<!-- ---------- -->
```csharp
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

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### new_window_id

Create a new window object using a specified ID.

> The `id` should be between 1 and 255

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
#include "webui.h"

int main() {

    /*
    * @param window The window number
    */

    size_t win = 1;
    webui_new_window_id(win);

    // Later
    webui_show(win, "index.html");
}
```
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
#include "webui.hpp"

int main() {

    size_t id = 1;
    webui::window win(id);
    
    // Later
    win.show("index.html");
}
```
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python
from webui import webui

# this way of initialization uses "new_window_id" internally
my_window = webui.Window(1)

# More code...

webui.wait()
```
<!-- ---------- -->
#### **Deno**
<!-- ---------- -->
```ts
// In development...
```
<!-- ---------- -->
#### **Go**
<!-- ---------- -->
```go
// In development...
```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
let
  windowId = 1
  window = newWindow(windowId)

# later...
window.show("index.html")
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v
// In development...
```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```odin
package main

import ui "webui"

main :: proc() {
    /*
    * @param window_number The window number (should be > 0, and < 255)
    */

    win: uint = 1
    ui.new_window_id(win)

    // Later
    ui.show(win, "index.html")
}
```
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->
```zig
// In development...
```
<!-- ---------- -->
#### **Rust**
<!-- ---------- -->
```rust
// In development...
```
<!-- ---------- -->
#### **PHP**
<!-- ---------- -->
```php
use Kingbes\Webui;

$Webui = new Webui;

/*
* @param window The window number
*/
$win = 1;
$Webui->newWindowId($win);

// Later
$Webui->show($win,"index.html");
```
<!-- ---------- -->
#### **Other...**
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
**Swift**
<!-- ---------- -->
```swift
// In development...
```
<!-- ---------- -->
**C-Sharp**
<!-- ---------- -->
```csharp
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

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_new_window_id

Get a free window ID that can be used later with `new_window_id` to create a new window object.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
#include "webui.h"

int main() {

    size_t win = webui_get_new_window_id();

    // Later
    webui_new_window_id(win);
    webui_show(win, "index.html");
}
```
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
#include "webui.hpp"

int main() {

    size_t id = webui::get_new_window_id();

    // Later
    webui::window win(id);
    win.show("index.html");
}
```
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python
from webui import webui

window_id = webui.get_new_window_id()
print(f"Available window ID: {window_id}")
```
<!-- ---------- -->
#### **Deno**
<!-- ---------- -->
```ts
// In development...
```
<!-- ---------- -->
#### **Go**
<!-- ---------- -->
```go
// In development...
```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
let
  windowId = getNewWindowId()
  window = newWindow(windowId)

# later...
window.show("index.html")
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v
// In development...
```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```odin
package main

import ui "webui"

main :: proc() {

    win: uint = ui.get_new_window_id()

    // Later
    ui.new_window_id(win)
    ui.show(win, "index.html")
}
```
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->
```zig
// In development...
```
<!-- ---------- -->
#### **Rust**
<!-- ---------- -->
```rust
// In development...
```
<!-- ---------- -->
#### **PHP**
<!-- ---------- -->
```php
use Kingbes\Webui;

$Webui = new Webui;

$win = $Webui->getNewWindowId();

// Later
$Webui->newWindowId($win);
$Webui->show($win,"index.html");
```
<!-- ---------- -->
#### **Other...**
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
**Swift**
<!-- ---------- -->
```swift
// In development...
```
<!-- ---------- -->
**C-Sharp**
<!-- ---------- -->
```csharp
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

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### bind

Bind an HTML element event with a function. Empty element means all events.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
#include "webui.h"

void events(webui_event_t* e) {
    //
}

void callback(webui_event_t* e) {
    //
}

int main() {

    /*
    * @param window The window number
    * @param element The HTML ID
    * @param func The callback function
    */
    
    webui_bind(win, "", events);
    webui_bind(win, "callback", callback);
}
```
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
#include "webui.hpp"

class myClass {
    public: void class_callback(webui::event* e) {
        // 
    }
};

// Because WebUI is written in C, therefor it can not
// access `myClass` directly. That's why we should add
// a simple wrapper for our C++ class.
myClass obj;
void class_callback_wrapper(webui::event* e) { obj.class_callback(e); }

void events(webui::window::event* e) {
    //
}

void callback(webui::window::event* e) {
    //
}

int main() {

    /*
    * @param element The HTML ID
    * @param func The callback function
    */
    
    win.bind("", events);
    win.bind("callback", callback);
    win.bind("class_callback", class_callback_wrapper);
}
```
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
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

webui.Bind(win, "MyID", my_function)
```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
# `bind` accepts three arguments:
# * window - The window to bind the function onto
# * element - The HTML element / JavaScript object to bind the function `func` onto
# * `func` - The callback proc -- the function to bind to `element`. 

# there are two ways to use `bind` -- you may either pass a proc into it or use `do` notation

# --- do notation. binds anon func to `callback`
window.bind("callback") do (e: Event):
  # do stuff here
  echo "called!"

# --- create proc and pass it to `bind`
proc events(e: Event) =
  # do stuff here
  echo "caught event ", e.eventType

# bind proc to all events
window.bind("", events)
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v
import vwebui as webui

fn my_function_string(e &webui.Event) {
    // <button id="MyID">Hello</button> gets clicked!
}

win.bind("MyID", my_function)
```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```odin
package main

import ui "webui"

events :: proc "c" (e: ^ui.Event) {
    context = runtime.default_context()
    //
}

callback :: proc "c" (e: ^ui.Event) {
    context = runtime.default_context()
    //
}

main :: proc() {

    /*
    * @param window The window number
    * @param element The HTML element / JavaScript object
    * @param func The callback function
    */

    ui.bind(win, "", events)
    ui.bind(win, "callback", callback)
}
```
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->
```zig
fn myFunction(e: *webui.Event) void {
    // <button id="MyID">Hello</button> gets clicked!
}

win.bind("MyID", myFunction);
```
<!-- ---------- -->
#### **Rust**
<!-- ---------- -->
```rust
// With external function
fn my_function(e: webui::Event) -> String {
    // <button id="MyID">Hello</button> gets clicked!
    "".to_string()
}

win.bind("MyID", my_function);

// With closure
win.bind("MyID2", |_: webui::Event| -> String {
    // <button id="MyID2">Hello</button> gets clicked!
    "".to_string()
});
```
<!-- ---------- -->
#### **PHP**
<!-- ---------- -->
```php
use Kingbes\Webui;
use Kingbes\JavaScript;

$Webui = new Webui;
$win = $Webui->newWindow();

// bind
$bind = $Webui->bind(
    $win, 
    "hello", 
    function ($event, JavaScript $js) {
    //
});
```
<!-- ---------- -->
#### **Other...**
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
**Swift**
<!-- ---------- -->
```swift
// In development...
```
<!-- ---------- -->
**C-Sharp**
<!-- ---------- -->
```csharp
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

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### event

Every event comes with information about the current event, like id, name, type (_click, connect, disconnect..._) and more.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
#include "webui.h"

/*
    webui_event_t {
        size_t window;        // The window object number
        size_t event_type;    // Event type
        char* element;        // HTML element ID
        size_t event_number;  // Internal
        size_t bind_id;       // Bind ID
        size_t client_id;     // Client's unique ID
        size_t connection_id; // Client's connection ID
        char* cookies;        // Client's full cookies
    };

    enum {
        WEBUI_EVENT_DISCONNECTED = 0, // 0. Window disconnection event
        WEBUI_EVENT_CONNECTED,        // 1. Window connection event
        WEBUI_EVENT_MOUSE_CLICK,      // 2. Mouse click event
        WEBUI_EVENT_NAVIGATION,       // 3. Window navigation event
        WEBUI_EVENT_CALLBACK,         // 4. Function call event
    };
*/

void events(webui_event_t* e) {
    if(e->event_type == WEBUI_EVENT_CONNECTED)
        printf("Connected. \n");
    else if(e->event_type == WEBUI_EVENT_DISCONNECTED)
        printf("Disconnected. \n");
    else if(e->event_type == WEBUI_EVENT_MOUSE_CLICK)
        printf("Click. \n");
    else if(e->event_type == WEBUI_EVENT_NAVIGATION)
        printf("Starting navigation to: %s \n", e->data);
}

int main() {

    /*
    * @param window The window number
    * @param element The HTML ID
    * @param func The callback function
    */
    
    webui_bind(win, "", events);
}
```
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
#include "webui.hpp"

void events(webui::window::event* e) {
    if(e->event_type == WEBUI_EVENT_CONNECTED)
        std::cout << "Connected." << std::endl;
    else if(e->event_type == WEBUI_EVENT_DISCONNECTED)
        std::cout << "Disconnected." << std::endl;
    else if(e->event_type == WEBUI_EVENT_MOUSE_CLICK)
        std::cout << "Click." << std::endl;
    else if(e->event_type == WEBUI_EVENT_NAVIGATION)
        std::cout << "Starting navigation to: " << e->data << std::endl;
}

int main() {

    /*
    * @param element The HTML ID
    * @param func The callback function
    */
    
    win.bind("", events);
}
```
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python
from webui import webui

def events(e: webui.Event):
    print('Hi!, You clicked on ' + e.element + ' element')

my_window = webui.Window()
# Empty ID means all events on all elements
my_window.bind("", events)
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
webui.Bind(win, "", events)
```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
#[
This is the `WebuiEvent` enum:

  WebuiEvent* = enum
    weDisconnected       ## 0. Window disconnection event
    weConnected          ## 1. Window connection event
    weMultiConnection    ## 2. New window connection event
    weUnwantedConnection ## 3. New unwanted window connection event
    weMouseClick         ## 4. Mouse click event
    weNavigation         ## 5. Window navigation event
    weCallback           ## 6. Function call event

It is held in the `eventType` property of the `Event` object, and describes the
type of the event
]#

# empty `element` means bind all events on all elements
window.bind("") do (e: Event):
  case e.eventType
  of weConnected:
    echo "Connected."
  of weDisconnected:
    echo "Disconnected."
  of weMouseClick:
    echo "Click."
  of weEventNavigation:
    echo "Starting navigation to: ", e.data
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
package main

import ui "webui"

/*
    Event :: struct {
        window: c.size_t,
        event_type: EventType,
        element: cstring,
        event_number: c.size_t,
        bind_id: c.size_t,
        client_id: c.size_t,
        connection_id: c.size_t,
        cookies: cstring,
    }

    EventType :: enum {
        Disconnected,         // 0. Window disconnection event
        Connected,            // 1. Window connection event
        MouseClick,          // 2. Mouse click event
        Navigation,           // 3. Window navigation event
        Callback,             // 4. Function call event
    }
*/

events :: proc "c" (e: ^ui.Event) {
    context = runtime.default_context()

    switch e.event_type {
        case .Connected:
            fmt.println("Connected.")
        case .Disconnected:
            fmt.println("Disconnected.")
        case .MouseClick:
            fmt.println("Click.")
        case .Navigation:
            target, _ := ui.get_arg(string, e)
            fmt.println("Starting navigation to:", target)
        case .Callback:
            fmt.println("Callback")
    }
}

main :: proc() {

    /*
    * @param window The window number
    * @param element The HTML ID
    * @param func The callback function
    */

    ui.bind(win, "", events)
}
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

fn myFunction(e: *webui.Event) void {

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
win.bind("", myFunction);
```
<!-- ---------- -->
#### **Rust**
<!-- ---------- -->
```rust
fn event_handler(e: webui::Event) {
  println!("Hi!, You clicked on {} element", e.element);
}

// Empty ID means all events on all elements
win.bind("", event_handler);
```
<!-- ---------- -->
#### **PHP**
<!-- ---------- -->
```php
$Webui->bind(
    $win, 
    "hello", 
    function ($event, JavaScript $js) {

    /*$event = object(FFI\CData:struct webui_event_t*)#6 (1) {
        [0]=>
        object(FFI\CData:struct webui_event_t)#9 (8) {
            ["window"]=> int // The window object number
            ["event_type"]=> int // Event type
            ["element"]=> string // HTML element ID
            ["event_number"]=> int // Internal
            ["bind_id"]=> int // Bind ID
            ["client_id"]=> int // Client's unique ID
            ["connection_id"]=> int // Client's connection ID
            ["cookies"]=> string // Client's full cookies
        }
    } */

});
```
<!-- ---------- -->
#### **Other...**
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
**Swift**
<!-- ---------- -->
```swift
// In development...
```
<!-- ---------- -->
**C-Sharp**
<!-- ---------- -->
```csharp
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

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_best_browser

Get the recommended web browser ID to use. If you are already using one, this function will return the same ID.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
#include "webui.h"

int main() {

    /*
        enum {
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
            WebView,        // 13. WebView (Non-web-browser)
        }
    */

    /*
    * @param window The window number
    */
    
    size_t browserID = webui_get_best_browser(win);
}
```
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
#include "webui.hpp"

int main() {

    /*
        enum {
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
            WebView,        // 13. WebView (Non-web-browser)
        }
    */

    size_t browserID = win.get_best_browser();
}
```
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python
from webui import webui

my_window = webui.Window()

browser_id = my_window.get_best_browser()
print(f"Recommended browser ID: {browser_id}")
```
<!-- ---------- -->
#### **Deno**
<!-- ---------- -->
```ts
// In development...
```
<!-- ---------- -->
#### **Go**
<!-- ---------- -->
```go
// In development...
```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
#[
This is the `WebuiBrowser` enum:

  type
    WebuiBrowser* = enum
      wbNoBrowser     ## 0. No web browser
      wbAny           ## 1. Default recommended web browser
      wbChrome        ## 2. Google Chrome
      wbFirefox       ## 3. Mozilla Firefox
      wbEdge          ## 4. Microsoft Edge
      wbSafari        ## 5. Apple Safari
      wbChromium      ## 6. The Chromium Project
      wbOpera         ## 7. Opera Browser
      wbBrave         ## 8. The Brave Browser
      wbVivaldi       ## 9. The Vivaldi Browser
      wbEpic          ## 10. The Epic Browser
      wbYandex        ## 11. The Yandex Browser
      wbChromiumBased ## 12. Any Chromium based browser
      wbWebview       ## 13. Webview (not a web browser)
]#

# `getBestBrowser` will return an item from the `WebuiBrowser` enum
let bestBrowser = window.getBestBrowser()
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v
// In development...
```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```odin
package main

import ui "webui"

main :: proc() {

    /*
        Browser :: enum {
            NoBrowser,      // 0. No web browser
            AnyBrowser,     // 1. Default recommended web browser
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
    */

    browserID: uint = ui.get_best_browser(my_window)
}
```
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->
```zig
// In development...
```
<!-- ---------- -->
#### **Rust**
<!-- ---------- -->
```rust
// In development...
```
<!-- ---------- -->
#### **PHP**
<!-- ---------- -->
```php
use Kingbes\Webui;

$Webui = new Webui;

/*
    NoBrowser,      // 0. No web browser
    AnyBrowser,     // 1. Default recommended web browser
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
    WebView,        // 13. WebView (Non-web-browser)
*/

/*
* @param window The window number
*/
$Webui->getBestBrowser($win);

```
<!-- ---------- -->
#### **Other...**
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
**Swift**
<!-- ---------- -->
```swift
// In development...
```
<!-- ---------- -->
**C-Sharp**
<!-- ---------- -->
```csharp
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

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### show

Show a window using embedded HTML, or a file. If the window is already open, it will be refreshed.

WebUI will try this patern:

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

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
#include "webui.h"

int main() {

    /*
    * @param window The window number
    * @param content The HTML, URL, Or a local file
    */
    
    webui_show(win, "index.html");
}
```
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
#include "webui.hpp"

int main() {

    /*
    * @param content The HTML, URL, Or a local file
    */
    
    win.show("index.html");
}
```
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
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
<!-- ---------- -->
#### **Deno**
<!-- ---------- -->
```ts
myWindow.show('<html><script src="/webui.js"> ... </html>')
myWindow.show('file.html')
myWindow.show('https://mydomain.com')
```
<!-- ---------- -->
#### **Go**
<!-- ---------- -->
```go
myWindow.Show("<html><script src=\"/webui.js\"> ... </html>")
myWindow.Show("file.html")
myWindow.Show("https://mydomain.com")
```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
# `show` accepts 2 (or 3) parameters:
# * window - The window to show content in
# * content - The content to show in `window`, may be a file or some HTML code
# * browser - The browser to open the window in (OPTIONAL)

window.show("<html><script src=\"/webui.js\"> ... </html>")
window.show("file.html")
window.show("https://mydomain.com")
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v
myWindow.show('<html><script src="/webui.js"> ... </html>')
myWindow.show('file.html')
myWindow.show('https://mydomain.com')
```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param window The window number
    * @param content The HTML, URL, Or a local file
    */

    ui.show(my_window, "<html><script src=\"/webui.js\"></script> ... </html>")
    ui.show(my_window, "file.html")
    ui.show(my_window, "https://mydomain.com")
}
```
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->
```zig
const successed = myWindow.show("<html><script src=\"webui.js\"> ... </html>");
const successed = myWindow.show("file.html");
const successed = myWindow.show("https://mydomain.com");
```
<!-- ---------- -->
#### **Rust**
<!-- ---------- -->
```rust
win.show("<html><script src=\"/webui.js\"> ... </html>");
win.show("file.html");
win.show("https://mydomain.com");
```
<!-- ---------- -->
#### **PHP**
<!-- ---------- -->
```php
use Kingbes\Webui;

$Webui = new Webui;
$win = $Webui->newWindow();


/*
* @param window The window number
* @param content The HTML, URL, Or a local file
*/
$Webui->show($win, "index.html");
```
<!-- ---------- -->
#### **Other...**
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
**Swift**
<!-- ---------- -->
```swift
// In development...
```
<!-- ---------- -->
**C-Sharp**
<!-- ---------- -->
```csharp
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

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### show_browser

Show a window using a specific web browser.

> It's recommended to use `ChromiumBased` browser

> in macOS, the browser's icon may still shown in the Duck icons after exit, therefor we recommend to use `show_wv` instead, this icon issue exist only in macOS.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
#include "webui.h"

int main() {

    /*
        enum {
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
            WebView,        // 13. WebView (Non-web-browser)
        }
    */

    /*
    * @param window The window number
    * @param content The HTML, Or a local file
    * @param browser The web browser to be used
    */
    
    webui_show_browser(win, "index.html", Chrome);
}
```
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
#include "webui.hpp"

int main() {

    /*
        enum {
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
            WebView,        // 13. WebView (Non-web-browser)
        }
    */

    /*
    * @param content The HTML, Or a local file
    * @param browser The web browser to be used
    */
    
    win.show_browser("index.html", Chrome);
}
```
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python
from webui import webui

my_window = webui.Window()

# success = my_window.show_browser("<html>...</html>", webui.Browser.Chrome)
# success = my_window.show_browser("index.html", webui.Browser.Firefox)

# Handling the return is optional.
my_window.show_browser("index.html", webui.Browser.AnyBrowser)
```
<!-- ---------- -->
#### **Deno**
<!-- ---------- -->
```ts
myWindow.showBrowser('<html><script src="/webui.js"> ... </html>', WebUI.Browser.Chrome)
```
<!-- ---------- -->
#### **Go**
<!-- ---------- -->
```go
myWindow.ShowBrowser("<html><script src=\"/webui.js\"> ... </html>", webui.Chrome)
```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
# pass a single web browser that you wish to used
window.show("index.html", wbChrome)

# you may also pass a set or openArray if you want to use a specific set of web browsers
window.show("index.html", {wbWebview, wbChrome, wbEdge})
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v
myWindow.show_browser('<html><script src="/webui.js"> ... </html>', .chrome)
```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```odin
package main

import ui "webui"

main :: proc() {

    /*
        Browser :: enum {
            NoBrowser,      // 0. No web browser
            AnyBrowser,     // 1. Default recommended web browser
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
    * @param content The HTML, Or a local file
    * @param browser The web browser to be used
    */

    ui.show_browser(my_window, "<html><script src=\"/webui.js\"></script> ... </html>", .Chrome)
}
```
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->
```zig
const successed = myWindow.showBrowser("<html><script src=\"webui.js\"> ... </html>", .Chrome);
```
<!-- ---------- -->
#### **Rust**
<!-- ---------- -->
```rust
win.show_browser("<html><script src=\"/webui.js\"> ... </html>", webui::WebUIBrowser::Chrome);
```
<!-- ---------- -->
#### **PHP**
<!-- ---------- -->
```php
use Kingbes\Webui;

$Webui = new Webui;
$win = $Webui->newWindow();

/*  NoBrowser = 0,  // 0. No web browser
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
    WebView,        // 13. WebView (Non-web-browser) */

/*
* @param window The window number
* @param content The HTML, Or a local file
* @param browser The web browser to be used number
*/
$Webui->showBrowser($win, "index.html", 4);
```
<!-- ---------- -->
#### **Other...**
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
**Swift**
<!-- ---------- -->
```swift
// In development...
```
<!-- ---------- -->
**C-Sharp**
<!-- ---------- -->
```csharp
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

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
#include "webui.h"

int main() {

    /*
    * @param window The window number
    * @param content The HTML, URL, Or a local file
    */
    
    webui_show_wv(win, "index.html");
}
```
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
#include "webui.hpp"

int main() {

    /*
    * @param content The HTML, URL, Or a local file
    */
    
    win.show_wv("index.html");
}
```
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python
from webui import webui

my_window = webui.Window()

# success = my_window.show_wv("<html>...</html>")
# success = my_window.show_wv("index.html")
# success = my_window.show_wv("http://example.com")

my_window.show_wv("index.html")
```
<!-- ---------- -->
#### **Deno**
<!-- ---------- -->
```ts
// In development...
```
<!-- ---------- -->
#### **Go**
<!-- ---------- -->
```go
// In development...
```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
window.showWv("index.html")
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v
// In development...
```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param window The window number
    * @param content The HTML, URL, Or a local file
    */

    ui.show_wv(my_window, "index.html")
}
```
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->
```zig
// In development...
```
<!-- ---------- -->
#### **Rust**
<!-- ---------- -->
```rust
// In development...
```
<!-- ---------- -->
#### **PHP**
<!-- ---------- -->
```php
// In development...
```
<!-- ---------- -->
#### **Other...**
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
**Swift**
<!-- ---------- -->
```swift
// In development...
```
<!-- ---------- -->
**C-Sharp**
<!-- ---------- -->
```csharp
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

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_kiosk

Set the window in Kiosk mode (_Full screen_).

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
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
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
#include "webui.hpp"

int main() {

    /*
    * @param status True or False
    */
    
    win.set_kiosk(true);
}
```
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python
from webui import webui

my_window = webui.Window()

my_window.set_kiosk(True)    # Enable Kiosk mode
# my_window.set_kiosk(False) # Disable Kiosk mode
```
<!-- ---------- -->
#### **Deno**
<!-- ---------- -->
```ts
// In development...
```
<!-- ---------- -->
#### **Go**
<!-- ---------- -->
```go
// In development...
```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
window.kiosk = true
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v
// In development...
```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param window The window number
    * @param status True or False
    */

    ui.set_kiosk(my_window, true)
}
```
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->
```zig
// In development...
```
<!-- ---------- -->
#### **Rust**
<!-- ---------- -->
```rust
// In development...
```
<!-- ---------- -->
#### **PHP**
<!-- ---------- -->
```php
use Kingbes\Webui;

$Webui = new Webui;
$win = $Webui->newWindow();


/*
* @param window The window number
* @param status True or False
*/
$Webui->setKiosk($win, true);
```
<!-- ---------- -->
#### **Other...**
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
**Swift**
<!-- ---------- -->
```swift
// In development...
```
<!-- ---------- -->
**C-Sharp**
<!-- ---------- -->
```csharp
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

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### wait

Wait until all opened windows get closed.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
#include "webui.h"

int main() {

    webui_wait();
}
```

Full C Example: https://github.com/webui-dev/webui/tree/main/examples/C/minimal
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
#include "webui.hpp"

int main() {

    webui::wait();
}
```

Full C++ Example: https://github.com/webui-dev/webui/tree/main/examples/C%2B%2B/minimal
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python
from webui import webui

# Other webui function calls / code...

# At the end of the main driver
webui.wait()  # Wait until all windows are closed before continuing
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
# or when calling `exit()`
wait()
```

Full Nim example: <https://github.com/webui-dev/nim-webui/tree/main/examples/minimal.nim>
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
    win.wait()
}
```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```odin
package main

import ui "webui"

main :: proc() {

    ui.wait()
}
```
Full Odin Example: https://github.com/webui-dev/odin-webui/blob/main/examples/minimal.odin
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
#### **Rust**
<!-- ---------- -->
```rust
use webui_rs::webui;

fn main() {
  // ...
  webui::wait();
}
```
<!-- ---------- -->
#### **PHP**
<!-- ---------- -->
```php
use Kingbes\Webui;

$Webui = new Webui;


$Webui->wait();
```
<!-- ---------- -->
#### **Other...**
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
**Swift**
<!-- ---------- -->
```swift
// In development...
```
<!-- ---------- -->
**C-Sharp**
<!-- ---------- -->
```csharp
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

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### close

Close a specific window.

> The `win` object will still alive, and it can be reopen later.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
#include "webui.h"

void callback(webui_event_t* e) {

    /*
    * @param e The event struct
    */

    webui_close_client(e);
}

int main() {

    /*
    * @param window The window number
    */
    
    webui_close(win);
}
```
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
#include "webui.hpp"

void callback(webui::window::event* e) {

    /*
    * @param e The event struct
    */

    webui_close_client(e);
}

int main() {

    win.close();
}
```
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python
from webui import webui

my_window = webui.Window()

my_window.close()  # Close the current window.
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
webui.Close(win)
```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
window.close()
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v
win.close()
```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```odin
package main

import ui "webui"

callback :: proc "c" (e: ^ui.Event) {
    context = runtime.default_context()

    /*
    * @param e The event struct
    */

    ui.close_client(e)
}

main :: proc() {
    /*
    * @param window The window number
    */

    ui.close(win)
}
```
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->
```zig
win.close();
```
<!-- ---------- -->
#### **Rust**
<!-- ---------- -->
```rust
win.close();
```
<!-- ---------- -->
#### **PHP**
<!-- ---------- -->
```php
use Kingbes\Webui;

$Webui = new Webui;
$win = $Webui->newWindow();


/*
* @param window The window number
*/
$Webui->close($win);
```
<!-- ---------- -->
#### **Other...**
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
**Swift**
<!-- ---------- -->
```swift
// In development...
```
<!-- ---------- -->
**C-Sharp**
<!-- ---------- -->
```csharp
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

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### destroy

Close a specific window and free all related memory resources.

> This will make the `win` object not available. If you want to simply close a window and reopen it later, please use `close()` instead.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
#include "webui.h"

int main() {

    /*
    * @param window The window number
    */
    
    webui_destroy(win);
}
```
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
#include "webui.hpp"

int main() {

    win.destroy();
}
```
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python
from webui import webui

my_window = webui.Window()

my_window.destroy()  # Close and free resources for the current window.
```
<!-- ---------- -->
#### **Deno**
<!-- ---------- -->
```ts
// In development...
```
<!-- ---------- -->
#### **Go**
<!-- ---------- -->
```go
// In development...
```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
window.destroy()
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v
// In development...
```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```odin
package main

import ui "webui"

main :: proc() {
    /*
    * @param window The window number
    */

    ui.destroy(win)
}
```
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->
```zig
// In development...
```
<!-- ---------- -->
#### **Rust**
<!-- ---------- -->
```rust
// In development...
```
<!-- ---------- -->
#### **PHP**
<!-- ---------- -->
```php
use Kingbes\Webui;

$Webui = new Webui;
$win = $Webui->newWindow();


/*
* @param window The window number
*/
$Webui->destroy($win);
```
<!-- ---------- -->
#### **Other...**
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
**Swift**
<!-- ---------- -->
```swift
// In development...
```
<!-- ---------- -->
**C-Sharp**
<!-- ---------- -->
```csharp
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

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### exit

Close all open windows. This will make `wait()` return (_Break_).

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
#include "webui.h"

int main() {

    webui_exit();
}
```
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
#include "webui.hpp"

int main() {

    webui::exit();
}
```
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python
from webui import webui

# Other code...

webui.exit()  # Close all WebUI windows and stop waiting
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
package main

import ui "webui"

main :: proc() {

    ui.exit()
}
```
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->
```zig
webui.exit();
```
<!-- ---------- -->
#### **Rust**
<!-- ---------- -->
```rust
webui::exit();
```
<!-- ---------- -->
#### **PHP**
<!-- ---------- -->
```php
use Kingbes\Webui;

$Webui = new Webui;


$Webui->exit();
```
<!-- ---------- -->
#### **Other...**
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
**Swift**
<!-- ---------- -->
```swift
// In development...
```
<!-- ---------- -->
**C-Sharp**
<!-- ---------- -->
```csharp
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

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_root_folder

Set the web-server root folder path for a specific window.

> Should be used before `show()`.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
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
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
#include "webui.hpp"

int main() {

    /*
    * @param path The local folder full path
    */
    
    win.set_root_folder("/home/Foo/Bar/");
}
```
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python
from webui import webui

my_window = webui.Window()

# Handling the return boolean is optional.
success = my_window.set_root_folder("/home/Foo/Bar/")
if success:
    print("Root folder set successfully.")
```
<!-- ---------- -->
#### **Deno**
<!-- ---------- -->
```ts
// In development...
```
<!-- ---------- -->
#### **Go**
<!-- ---------- -->
```go
// In development...
```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
window.rootFolder = "/home/Foo/Bar/"
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v
// In development...
```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param window The window number
    * @param path The local folder full path
    */

    ui.set_root_folder(my_window, "/home/Foo/Bar/")
}
```
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->
```zig
// In development...
```
<!-- ---------- -->
#### **Rust**
<!-- ---------- -->
```rust
// In development...
```
<!-- ---------- -->
#### **PHP**
<!-- ---------- -->
```php
use Kingbes\Webui;

$Webui = new Webui;
$win = $Webui->newWindow();


/*
* @param window The window number
* @param path The local folder full path
*/
$Webui->setRootFolder($win, "/home/Foo/Bar/");
```
<!-- ---------- -->
#### **Other...**
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
**Swift**
<!-- ---------- -->
```swift
// In development...
```
<!-- ---------- -->
**C-Sharp**
<!-- ---------- -->
```csharp
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

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_default_root_folder

Set the web-server root folder path for all windows.

> Should be used before `show()`.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
#include "webui.h"

int main() {

    /*
    * @param path The local folder full path
    */
    
    webui_set_default_root_folder("/home/Foo/Bar/");
}
```
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
#include "webui.hpp"

int main() {

    /*
    * @param path The local folder full path
    */
    
    webui::set_default_root_folder("/home/Foo/Bar/");
}
```
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python
from webui import webui

# Handling the return boolean is optional.
success = webui.set_default_root_folder("/home/Foo/Bar/")
if success:
    print("Default root folder set successfully.")
```
<!-- ---------- -->
#### **Deno**
<!-- ---------- -->
```ts
// In development...
```
<!-- ---------- -->
#### **Go**
<!-- ---------- -->
```go
// In development...
```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
setDefaultRootFolder("/home/Foo/Bar/")
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v
// In development...
```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param path The local folder full path
    */

    ui.set_default_root_folder("/home/Foo/Bar/")
}
```
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->
```zig
// In development...
```
<!-- ---------- -->
#### **Rust**
<!-- ---------- -->
```rust
// In development...
```
<!-- ---------- -->
#### **PHP**
<!-- ---------- -->
```php
use Kingbes\Webui;

$Webui = new Webui;
$win = $Webui->newWindow();


/*
* @param path The local folder full path
*/
$Webui->setDefaultRootFolder("/home/Foo/Bar/");
```
<!-- ---------- -->
#### **Other...**
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
**Swift**
<!-- ---------- -->
```swift
// In development...
```
<!-- ---------- -->
**C-Sharp**
<!-- ---------- -->
```csharp
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

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_file_handler

Set a custom handler to serve files.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
#include "webui.h"

void filesHandler(const char* filename, int* length) {
    //
}

int main() {

    /*
    * @param window The window number
    * @param handler The handler function
    */
    
    webui_set_file_handler(win, filesHandler);
}
```

Full C Example: https://github.com/webui-dev/webui/tree/main/examples/C/serve_a_folder
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
#include "webui.hpp"

void filesHandler(const char* filename, int* length) {
    //
}

int main() {

    /*
    * @param handler The handler function
    */
    
    win.set_file_handler(filesHandler);
}
```

Full C++ Example: https://github.com/webui-dev/webui/tree/main/examples/C%2B%2B/serve_a_folder
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python
# In development...
```
<!-- ---------- -->
#### **Deno**
<!-- ---------- -->
```ts
// In development...
```
<!-- ---------- -->
#### **Go**
<!-- ---------- -->
```go
// In development...
```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
var count: int

window.setFileHandler() do (filename: string) -> string:
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

  # By default, this function returns an empty string.
  # Returning an empty string will make WebUI look for
  # the requested file locally
```

Full Nim example: <https://github.com/webui-dev/nim-webui/tree/main/examples/serve_folder/serve_folder.nim>
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v
// In development...
```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```odin
package main

import ui "webui"

filesHandler :: proc "c" (filename: cstring, length: i32) -> rawptr {
    context = runtime.default_context()

    // code
    return rawptr
}

main :: proc() {

    /*
    * @param window The window number
    * @param handler The handler function
    */

    ui.set_file_handler(my_window, filesHandler)
}
```

Full Odin Example: https://github.com/webui-dev/odin-webui/tree/main/examples/serve_a_folder
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
            \\        Count: {} <a href="dynamic.html">[Refresh]</a><br>
            \\        <script src="webui.js"></script>
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
win.setFileHandler(my_files_handler);
```
<!-- ---------- -->
#### **Rust**
<!-- ---------- -->
```rust
static mut COUNT: i32 = 0;

fn my_file_handler(filename: *const i8, len: *mut i32) -> *const std::os::raw::c_void {
    let filename = unsafe { std::ffi::CStr::from_ptr(filename).to_str().unwrap() };

    println!("File: {}", filename);

    if filename == "/test.txt" {
        // Const static file example
        // Note: The connection will drop if the content
        // does not have `<script src="/webui.js"></script>`
        let content = "This is a embedded file content example.";
        unsafe { *len = content.len() as i32 };
        return content.as_ptr() as *const std::os::raw::c_void;
    } else if filename == "/dynamic.html" {
        // Dynamic file example
        unsafe { COUNT += 1 };
        let content = format!(
            "<html>This is a dynamic file content example.<br>Count: {} <a href=\"dynamic.html\">[Refresh]</a><br><script src=\"/webui.js\"></script></html>",
            unsafe { COUNT }
        );
        unsafe { *len = content.len() as i32 };
        return content.as_ptr() as *const std::os::raw::c_void;
    }

    // A NULL return will make WebUI
    // looks for the file locally. if
    // not, WebUI will generate `404`
    std::ptr::null()
}

// Set a custom files handler
win.set_file_handler(my_file_handler);
```
<!-- ---------- -->
#### **PHP**
<!-- ---------- -->
```php
use Kingbes\Webui;

$Webui = new Webui;
$win = $Webui->newWindow();


/*
* @param window The window number
* @param handler The handler function
*/
$Webui->setFileHandler($win, function(string $filename,int $length){
    //
});
```
<!-- ---------- -->
#### **Other...**
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
**Swift**
<!-- ---------- -->
```swift
// In development...
```
<!-- ---------- -->
**C-Sharp**
<!-- ---------- -->
```csharp
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

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### is_shown

Check if the specified window is still running.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
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
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
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
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python
from webui import webui

my_window = webui.Window()

if my_window.is_shown():
    print("The window is still open.")
else:
    print("The window has been closed.")
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
if win.IsShown() {
    fmt.Printf("The window is still running")
} else {
    fmt.Printf("The window is closed.")
}
```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
if window.shown:
  echo "A window is running..."
else:
  echo "No window is running."
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v
if webui.is_shown(win) {
    println('The window is still running')
} else {
    println('The window is closed.')
}
```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param window The window number
    */

    if ui.is_shown(my_window) {
        // Window is shown
    } else {
        // Window is closed
    }
}
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
#### **Rust**
<!-- ---------- -->
```rust
if win.is_shown() {
    println!("The window is still running");
} else {
    println!("The window is closed.");
}
```
<!-- ---------- -->
#### **PHP**
<!-- ---------- -->
```php
use Kingbes\Webui;

$Webui = new Webui;
$win = $Webui->newWindow();


/*
* @param window The window number
*/
if($Webui->isShown($win)){
    // Window is shown
}else{
    // Window is closed
}
```
<!-- ---------- -->
#### **Other...**
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
**Swift**
<!-- ---------- -->
```swift
// In development...
```
<!-- ---------- -->
**C-Sharp**
<!-- ---------- -->
```csharp
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

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_timeout

Set the maximum time in seconds to wait for the window to connect. This effect `show()` and `wait()`.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
#include "webui.h"

void callback(webui_event_t* e) {

    webui_show_client(e, "<html>...</html>");
}

int main() {

    /*
    * @param second The timeout in seconds
    */
    
    webui_set_timeout(30);

    webui_show(win, "index.html");
    webui_wait();
}
```
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
#include "webui.hpp"

void callback(webui::window::event* e) {

    win.show_client(e, "<html>...</html>");
}

int main() {

    /*
    * @param second The timeout in seconds
    */
    
    webui::set_timeout(30);

    win.show("index.html");
    webui::wait();
}
```
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python
from webui import webui

# Set a timeout of 30 seconds for window connections
webui.set_timeout(30)
```
<!-- ---------- -->
#### **Deno**
<!-- ---------- -->
```ts
// In development...
```
<!-- ---------- -->
#### **Go**
<!-- ---------- -->
```go
// In development...
```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
# `setTimeout` accepts a single parameter:
# * timeout - The maximum time in seconds to wait for browser to start.
#             Set to 0 to wait forever.

setTimeout(30)

window.show("index.html")
wait()
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v
// In development...
```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param second The timeout in seconds
    */

    ui.set_timeout(30)

    ui.show(win, "index.html")
    ui.wait()
}
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
<!-- ---------- -->
#### **Rust**
<!-- ---------- -->
```rust
// Set timeout for 10 seconds
webui::set_timeout(10);

// Now, After 10 seconds, if the browser did
// not get started, wait() will break
webui::wait();
```
<!-- ---------- -->
#### **PHP**
<!-- ---------- -->
```php
use Kingbes\Webui;

$Webui = new Webui;
$win = $Webui->newWindow();


/*
* @param second The timeout in seconds
*/
$Webui->setTimeout(30);

$Webui->show($win, "index.html");
$Webui->wait();
```
<!-- ---------- -->
#### **Other...**
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
**Swift**
<!-- ---------- -->
```swift
// In development...
```
<!-- ---------- -->
**C-Sharp**
<!-- ---------- -->
```csharp
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

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_icon

Set the default embedded HTML favicon.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
#include "webui.h"

int main() {

    /*
    * @param window The window number
    * @param icon The icon as string: `<svg>...</svg>`
    * @param icon_type The icon type: `image/svg+xml`
    */
    
    webui_set_icon(win, "<svg>...</svg>", "image/svg+xml");
}
```
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
#include "webui.hpp"

int main() {

    /*
    * @param icon The icon as string: `<svg>...</svg>`
    * @param icon_type The icon type: `image/svg+xml`
    */
    
    win.set_icon("<svg>...</svg>", "image/svg+xml");
}
```
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python
from webui import webui

my_window = webui.Window()

my_window.set_icon("<svg>...</svg>", "image/svg+xml")
```
<!-- ---------- -->
#### **Deno**
<!-- ---------- -->
```ts
// In development...
```
<!-- ---------- -->
#### **Go**
<!-- ---------- -->
```go
// In development...
```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
# SVG Icon
let
  icon = "<svg>...</svg>"
  mimeType = "image/svg+xml"

# --- PNG Icon
# let
#   mimeType = "data:image/..."
#   mimeType = "image/png"

# `setIcon` accepts 3 arguments:
# * window - The window to set the icon for
# * icon - The icon as a string
# * mime - The MIME type of the icon

window.setIcon(icon, mimeType)
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v
// In development...
```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param window The window number
    * @param icon The icon as string: '<svg>...</svg>'
    * @param icon_type The icon type: 'image/svg+xml'
    */

    ui.set_icon(win, "<svg>...</svg>", "image/svg+xml")
}
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
#### **Rust**
<!-- ---------- -->
```rust
// SVG Icon
let my_icon = "<svg>...</svg>";
let my_icon_type = "image/svg+xml";

// PNG Icon
// let my_icon = "data:image/...";
// let my_icon_type = "image/png";

// When the web browser ask for `favicon.ico`, WebUI will
// send a redirection to `favicon.svg`, the body will be `my_icon`
// and the mime-type will be `my_icon_type`
win.set_icon(my_icon, my_icon_type);
```
<!-- ---------- -->
#### **PHP**
<!-- ---------- -->
```php
use Kingbes\Webui;

$Webui = new Webui;
$win = $Webui->newWindow();


/*
* @param window The window number
* @param icon The icon as string: `<svg>...</svg>`
* @param icon_type The icon type: `image/svg+xml`
*/
$Webui->setIcon($win, "<svg>...</svg>", "image/svg+xml");
```
<!-- ---------- -->
#### **Other...**
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
**Swift**
<!-- ---------- -->
```swift
// In development...
```
<!-- ---------- -->
**C-Sharp**
<!-- ---------- -->
```csharp
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

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### encode

Encode text to Base64.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
#include "webui.h"

int main() {

    /*
    * @param str The string to encode (Should be null terminated)
    */
    
    char* base64 = webui_encode("Foo Bar");

    // Later
    webui_free(base64);
}
```
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
#include "webui.hpp"

int main() {

    /*
    * @param str The string to encode (Should be null terminated)
    */
    
    char* base64 = webui::encode("Foo Bar");

    // Later
    webui::free(base64);
}
```
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python
from webui import webui

encoded_string = webui.ui_encode("Foo Bar")
print(f"Base64 Encoded: {encoded_string}")
```
<!-- ---------- -->
#### **Deno**
<!-- ---------- -->
```ts
// In development...
```
<!-- ---------- -->
#### **Go**
<!-- ---------- -->
```go
// In development...
```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
# `encode` accepts a single argument:
# * str - The string to encode into Base64.
#         Note: the string is nil-terminated

let base64 = encode("Foo Bar")
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v
// In development...
```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param str The string to encode
    */

    base64: cstring = ui.encode("Foo Bar")

    // Later
    ui.free(base64)
}
```
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->
```zig
// In development...
```
<!-- ---------- -->
#### **Rust**
<!-- ---------- -->
```rust
// In development...
```
<!-- ---------- -->
#### **PHP**
<!-- ---------- -->
```php
// In development...
```
<!-- ---------- -->
#### **Other...**
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
**Swift**
<!-- ---------- -->
```swift
// In development...
```
<!-- ---------- -->
**C-Sharp**
<!-- ---------- -->
```csharp
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

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### decode

Decode a Base64 encoded text.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
#include "webui.h"

void callback(webui_event_t* e) {

    // JavaScript:
    // callback(Base64);
    const char* base64 = webui_get_string(e);

    /*
    * @param str The string to decode (Should be null terminated)
    */

    const char* str = webui_decode(base64);

    // Later
    webui_free(str);
}
```
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
#include "webui.hpp"

void callback(webui::window::event* e) {

    // JavaScript:
    // callback(Base64);
    const char* base64 = e->get_string();

    /*
    * @param str The string to decode (Should be null terminated)
    */

    const char* str = webui::decode(base64);

    // Later
    webui::free(str);
}
```
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python
from webui import webui

decoded_string = webui.ui_decode("SGVsbG8=")
print(f"Decoded String: {decoded_string}")  # Output: Hello
```
<!-- ---------- -->
#### **Deno**
<!-- ---------- -->
```ts
// In development...
```
<!-- ---------- -->
#### **Go**
<!-- ---------- -->
```go
// In development...
```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
// In development...
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v
// In development...
```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```odin
package main

import ui "webui"

callback :: proc "c" (e: ^ui.Event) {
    context = runtime.default_context()

    // JavaScript:
    // callback(Base64)
    base64: cstring = ui.get_string(e)

    /*
    * @param str The string to decode
    */

    str: cstring = ui.decode(base64)

    // Later
    ui.free(str)
}
```
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->
```zig
// In development...
```
<!-- ---------- -->
#### **Rust**
<!-- ---------- -->
```rust
// In development...
```
<!-- ---------- -->
#### **PHP**
<!-- ---------- -->
```php
// In development...
```
<!-- ---------- -->
#### **Other...**
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
**Swift**
<!-- ---------- -->
```swift
// In development...
```
<!-- ---------- -->
**C-Sharp**
<!-- ---------- -->
```csharp
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

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### free

Safely free a buffer allocated by WebUI.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
#include "webui.h"

int main() {
    char* buffer = (char*) webui_malloc(1024);

    /*
    * @param ptr The buffer to be freed
    */
    
    webui_free(buffer);
}
```
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
#include "webui.hpp"

int main() {
    char* buffer = (char*) webui::malloc(1024);

    /*
    * @param ptr The buffer to be freed
    */
    
    webui::free(buffer);
}
```
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python
from webui import webui

my_buffer = webui.malloc(1024)

webui.free(my_buffer)  # Free the allocated buffer
```
<!-- ---------- -->
#### **Deno**
<!-- ---------- -->
```ts
// In development...
```
<!-- ---------- -->
#### **Go**
<!-- ---------- -->
```go
// In development...
```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
from webui/bindings import nil

let buffer = bindings.malloc(csize_t 1024)

# `free` accepts a single argument:
# * `ptr` - The buffer to be freed

bindings.free(buffer)
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v
// In development...
```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```odin
package main

import ui "webui"

main :: proc() {
    myBuffer := cast(^u8)ui.malloc(1024)

    /*
    * @param ptr The buffer to be freed
    */

    ui.free(myBuffer)
}
```
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->
```zig
// In development...
```
<!-- ---------- -->
#### **Rust**
<!-- ---------- -->
```rust
// In development...
```
<!-- ---------- -->
#### **PHP**
<!-- ---------- -->
```php
// In development...
```
<!-- ---------- -->
#### **Other...**
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
**Swift**
<!-- ---------- -->
```swift
// In development...
```
<!-- ---------- -->
**C-Sharp**
<!-- ---------- -->
```csharp
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

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### malloc

Safely allocate memory using the WebUI memory management system. It can be safely freed using WebUI's _free_ API at any time later.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
#include "webui.h"

int main() {

    /*
    * @param size The size of memory in bytes
    */
    
    char* buffer = (char*) webui_malloc(1024);

    // Later
    webui_free(buffer);
}
```
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
#include "webui.hpp"

int main() {

    /*
    * @param size The size of memory in bytes
    */
    
    char* buffer = (char*) webui::malloc(1024);

    // Later
    webui::free(buffer);
}
```
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python
from webui import webui

buffer = webui.malloc(1024)
if buffer:
    print(f"Memory allocated at address: {buffer}")
    webui.free(buffer)  # Free the allocated memory
```
<!-- ---------- -->
#### **Deno**
<!-- ---------- -->
```ts
// In development...
```
<!-- ---------- -->
#### **Go**
<!-- ---------- -->
```go
// In development...
```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
from webui/bindings import nil

# `malloc` accepts a single argument:
# * size - The size of the memory in bytes

let buffer = bindings.malloc(csize_t 1024)
bindings.free(buffer)
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v
// In development...
```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param size The size of memory in bytes
    */

    myBuffer := cast(^u8)ui.malloc(1024)

    // Later
    ui.free(myBuffer)
}
```
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->
```zig
// In development...
```
<!-- ---------- -->
#### **Rust**
<!-- ---------- -->
```rust
// In development...
```
<!-- ---------- -->
#### **PHP**
<!-- ---------- -->
```php
// In development...
```
<!-- ---------- -->
#### **Other...**
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
**Swift**
<!-- ---------- -->
```swift
// In development...
```
<!-- ---------- -->
**C-Sharp**
<!-- ---------- -->
```csharp
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

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### send_raw

Send raw data (_binary_) to the UI.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```js
    function myJavaScriptFunc(rawData) {
        // `rawData` is a Uint8Array variable
    }
```

```c
#include "webui.h"

void callback(webui::window::event* e) {

    webui_send_raw_client(e, "myJavaScriptFunc", buffer, 3);
}

int main() {

    /*
    * @param window The window number
    * @param function The JavaScript function to receive raw data
    * @param raw The raw data buffer
    * @param size The raw data size in bytes
    */
    
    unsigned char buffer[3] = { 0x01, 0x02, 0x03 }; // Any data type
    webui_send_raw(win, "myJavaScriptFunc", buffer, 3);
}
```
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```js
    function myJavaScriptFunc(rawData) {
        // `rawData` is a Uint8Array variable
    }
```

```cpp
#include "webui.hpp"

void callback(webui::window::event* e) {

    win.send_raw_client(e, buffer, 3);
}

int main() {

    /*
    * @param function The JavaScript function to receive raw data
    * @param raw The raw data buffer
    * @param size The raw data size in bytes
    */
    
    unsigned char buffer[3] = { 0x01, 0x02, 0x03 }; // Any data type
    win.send_raw("myJavaScriptFunc", buffer, 3);
}
```
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python
from webui import webui

my_window = webui.Window()
my_buffer = webui.malloc(1024)

my_window.send_raw("myJavaScriptFunc", my_buffer, 64)
# Sends 64 bytes of raw data to the JavaScript function `myJavaScriptFunc`.
```
<!-- ---------- -->
#### **Deno**
<!-- ---------- -->
```ts
// In development...
```
<!-- ---------- -->
#### **Go**
<!-- ---------- -->
```go
// In development...
```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
// In development...
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v
// In development...
```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```js
    function myJavaScriptFunc(rawData) {
        // `rawData` is a Uint8Array variable
    }
```

```odin
package main

import ui "webui"

callback :: proc "c" (e: ^ui.Event) {
    context = runtime.default_context()

    ui.send_raw_client(e, "myJavaScriptFunc", raw_data(buffer), 3)
}

main :: proc() {

    /*
    * @param window The window number
    * @param function The JavaScript function to receive raw data
    * @param raw The raw data buffer
    * @param size The raw data size in bytes
    */

    buffer: []byte = { 0x01, 0x02, 0x03 } // Any data type
    ui.send_raw(my_window, "myJavaScriptFunc", raw_data(buffer), len(buffer))
}
```
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->
```zig
// In development...
```
<!-- ---------- -->
#### **Rust**
<!-- ---------- -->
```rust
// In development...
```
<!-- ---------- -->
#### **PHP**
<!-- ---------- -->
```php
// In development...
```
<!-- ---------- -->
#### **Other...**
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
**Swift**
<!-- ---------- -->
```swift
// In development...
```
<!-- ---------- -->
**C-Sharp**
<!-- ---------- -->
```csharp
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

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_hide

Set a window in hidden mode.

> Should be called before `show()`.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
#include "webui.h"

int main() {

    /*
    * @param window The window number
    * @param status The status: True or False
    */
    
    webui_set_hide(win, true);
}
```
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
#include "webui.hpp"

int main() {

    /*
    * @param status The status: True or False
    */
    
    win.set_hide(true);
}
```
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python
from webui import webui

my_window = webui.Window()

my_window.set_hide(True)  # Hide the window
my_window.set_hide(False) # Show the window
```
<!-- ---------- -->
#### **Deno**
<!-- ---------- -->
```ts
// In development...
```
<!-- ---------- -->
#### **Go**
<!-- ---------- -->
```go
// In development...
```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
// In development...
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v
// In development...
```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param window The window number
    * @param status The status: True or False
    */

    ui.set_hide(my_window, true)
}
```
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->
```zig
// In development...
```
<!-- ---------- -->
#### **Rust**
<!-- ---------- -->
```rust
// In development...
```
<!-- ---------- -->
#### **PHP**
<!-- ---------- -->
```php
use Kingbes\Webui;

$Webui = new Webui;
$win = $Webui->newWindow();


/*
* @param window The window number
* @param status True or False
*/
$Webui->setHide($win, true);
```
<!-- ---------- -->
#### **Other...**
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
**Swift**
<!-- ---------- -->
```swift
// In development...
```
<!-- ---------- -->
**C-Sharp**
<!-- ---------- -->
```csharp
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

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_size

Set the window size.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
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
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
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
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python
from webui import webui

my_window = webui.Window()

my_window.set_size(800, 600)  # Set window size to 800x600 pixels
```
<!-- ---------- -->
#### **Deno**
<!-- ---------- -->
```ts
// In development...
```
<!-- ---------- -->
#### **Go**
<!-- ---------- -->
```go
// In development...
```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
// In development...
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v
// In development...
```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param window The window number
    * @param width The window width
    * @param height The window height
    */

    ui.set_size(my_window, 800, 600)
}
```
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->
```zig
// In development...
```
<!-- ---------- -->
#### **Rust**
<!-- ---------- -->
```rust
// In development...
```
<!-- ---------- -->
#### **PHP**
<!-- ---------- -->
```php
use Kingbes\Webui;

$Webui = new Webui;
$win = $Webui->newWindow();


/*
* @param window The window number
* @param width The window width
* @param height The window height
*/
$Webui->setSize($win, 800, 600);
```
<!-- ---------- -->
#### **Other...**
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
**Swift**
<!-- ---------- -->
```swift
// In development...
```
<!-- ---------- -->
**C-Sharp**
<!-- ---------- -->
```csharp
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

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_position

Set the window position.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
#include "webui.h"

int main() {

    /*
    * @param window The window number
    * @param x The window X
    * @param y The window Y
    */
    
    webui_set_position(win, 100, 100);
}
```
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
#include "webui.hpp"

int main() {

    /*
    * @param x The window X
    * @param y The window Y
    */
    
    win.set_position(100, 100);
}
```
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python
from webui import webui

my_window = webui.Window()

my_window.set_position(100, 100)  # Move window to (100, 100) on the screen
```
<!-- ---------- -->
#### **Deno**
<!-- ---------- -->
```ts
// In development...
```
<!-- ---------- -->
#### **Go**
<!-- ---------- -->
```go
// In development...
```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
// In development...
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v
// In development...
```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param window The window number
    * @param x The window X
    * @param y The window Y
    */

    ui.set_position(my_window, 100, 100)
}
```
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->
```zig
// In development...
```
<!-- ---------- -->
#### **Rust**
<!-- ---------- -->
```rust
// In development...
```
<!-- ---------- -->
#### **PHP**
<!-- ---------- -->
```php
use Kingbes\Webui;

$Webui = new Webui;
$win = $Webui->newWindow();


/*
 * @param window The window number
* @param x The window X
* @param y The window Y
*/
$Webui->setPosition($win, 100, 100);
```
<!-- ---------- -->
#### **Other...**
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
**Swift**
<!-- ---------- -->
```swift
// In development...
```
<!-- ---------- -->
**C-Sharp**
<!-- ---------- -->
```csharp
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

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_profile

Set the web browser profile to use. An empty `name` and `path` means the default user profile. 

> Need to be called before `show()`.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
#include "webui.h"

int main() {

    /*
    * @param window The window number
    * @param name The web browser profile name
    * @param path The web browser profile full path
    */
    
    webui_set_profile(win, "Bar", "/Home/Foo/Bar");
}
```
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
#include "webui.hpp"

int main() {

    /*
    * @param name The web browser profile name
    * @param path The web browser profile full path
    */
    
    win.set_profile("Bar", "/Home/Foo/Bar");
}
```
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python
from webui import webui

my_window = webui.Window()

my_window.set_profile("Bar", "/Home/Foo/Bar")  # Use a custom profile
my_window.set_profile("", "")  # Use the default profile
```
<!-- ---------- -->
#### **Deno**
<!-- ---------- -->
```ts
// In development...
```
<!-- ---------- -->
#### **Go**
<!-- ---------- -->
```go
// In development...
```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
// In development...
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v
// In development...
```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param window The window number
    * @param name The web browser profile name
    * @param path The web browser profile full path
    */

    ui.set_profile(my_window, "Bar", "/Home/Foo/Bar")
}
```
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->
```zig
// In development...
```
<!-- ---------- -->
#### **Rust**
<!-- ---------- -->
```rust
// In development...
```
<!-- ---------- -->
#### **PHP**
<!-- ---------- -->
```php
// In development...
```
<!-- ---------- -->
#### **Other...**
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
**Swift**
<!-- ---------- -->
```swift
// In development...
```
<!-- ---------- -->
**C-Sharp**
<!-- ---------- -->
```csharp
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

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_proxy

Set the web browser proxy server to use.

> Need to be called before `show()`.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
#include "webui.h"

int main() {

    /*
    * @param window The window number
    * @param proxy_server The web browser proxy_server
    */
    
    webui_set_proxy(win, "http://127.0.0.1:8888"); 
}
```
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
#include "webui.hpp"

int main() {

    /*
    * @param proxy_server The web browser proxy_server
    */
    
    win.set_proxy("http://127.0.0.1:8888"); 
}
```
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python
from webui import webui

my_window = webui.Window()

my_window.set_proxy("http://127.0.0.1:8888")  # Set the proxy server
```
<!-- ---------- -->
#### **Deno**
<!-- ---------- -->
```ts
// In development...
```
<!-- ---------- -->
#### **Go**
<!-- ---------- -->
```go
// In development...
```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
// In development...
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v
// In development...
```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param window The window number
    * @param proxy_server The web browser proxy_server
    */

    ui.set_proxy(my_window, "http://127.0.0.1:8888")
}
```
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->
```zig
// In development...
```
<!-- ---------- -->
#### **Rust**
<!-- ---------- -->
```rust
// In development...
```
<!-- ---------- -->
#### **PHP**
<!-- ---------- -->
```php
// In development...
```
<!-- ---------- -->
#### **Other...**
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
**Swift**
<!-- ---------- -->
```swift
// In development...
```
<!-- ---------- -->
**C-Sharp**
<!-- ---------- -->
```csharp
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

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_url

Get current URL of a running window.

> By default WebUI allow access to the URL of a window only from localhost.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
#include "webui.h"

int main() {

    /*
    * @param window The window number
    */
    
    const char* url = webui_get_url(win);
}
```
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
#include "webui.hpp"

int main() {

    const char* url = win.get_url();
}
```
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python
from webui import webui

my_window = webui.Window()

url = my_window.get_url()
print(f"Current URL: {url}")
```
<!-- ---------- -->
#### **Deno**
<!-- ---------- -->
```ts
// In development...
```
<!-- ---------- -->
#### **Go**
<!-- ---------- -->
```go
// In development...
```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
// In development...
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v
// In development...
```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param window The window number
    */

    url: cstring = ui.get_url(my_window)
}
```
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->
```zig
// In development...
```
<!-- ---------- -->
#### **Rust**
<!-- ---------- -->
```rust
// In development...
```
<!-- ---------- -->
#### **PHP**
<!-- ---------- -->
```php
// In development...
```
<!-- ---------- -->
#### **Other...**
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
**Swift**
<!-- ---------- -->
```swift
// In development...
```
<!-- ---------- -->
**C-Sharp**
<!-- ---------- -->
```csharp
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

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_public

Allow a specific window address (_URL_) to be accessible from any public network. By default WebUI allow access to the URL of a window only from localhost.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
#include "webui.h"

int main() {

    /*
    * @param window The window number
    * @param status True or False
    */
    
    webui_set_public(win, true);

    // Now, the URL of the window is accessible
    // from any device/mobile in the network.
}
```

Full C Example: https://github.com/webui-dev/webui/tree/main/examples/C/public_network_access
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
#include "webui.hpp"

int main() {

    /*
    * @param status True or False
    */
    
    win.set_public(true);

    // Now, the URL of the window is accessible
    // from any device/mobile in the network.
}
```
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python
from webui import webui

my_window = webui.Window()

my_window.set_public(True)  # Enable public network access
my_window.set_public(False) # Restrict access to local connections
```
<!-- ---------- -->
#### **Deno**
<!-- ---------- -->
```ts
// In development...
```
<!-- ---------- -->
#### **Go**
<!-- ---------- -->
```go
// In development...
```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
// In development...
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v
// In development...
```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param window The window number
    * @param status True or False
    */

    ui.set_public(my_window, true)

    // Now, the URL of the window is accessible
    // from any device/mobile in the network.
}
```

Full Odin Example: https://github.com/webui-dev/odin-webui/tree/main/examples/public_network_access
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->
```zig
// In development...
```
<!-- ---------- -->
#### **Rust**
<!-- ---------- -->
```rust
// In development...
```
<!-- ---------- -->
#### **PHP**
<!-- ---------- -->
```php
// In development...
```
<!-- ---------- -->
#### **Other...**
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
**Swift**
<!-- ---------- -->
```swift
// In development...
```
<!-- ---------- -->
**C-Sharp**
<!-- ---------- -->
```csharp
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

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### navigate

Navigate to a specific URL.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
#include "webui.h"

int main() {

    /*
    * @param window The window number
    * @param url Full HTTP URL
    */

    webui_navigate(win, "/foo/bar.html");

    // [!] Note:
    //
    // If `bar.html` does not include `webui.js` then
    // WebUI will try to close the window and `wait()`
    // will break. It's important to include `webui.js`
    // in every HTML.
}
```
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
#include "webui.hpp"

int main() {

    /*
    * @param url Full HTTP URL
    */

    win.navigate("/foo/bar.html");

    // [!] Note:
    //
    // If `bar.html` does not include `webui.js` then
    // WebUI will try to close the window and `wait()`
    // will break. It's important to include `webui.js`
    // in every HTML.
}
```
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python
from webui import webui

my_window = webui.Window()

my_window.navigate("http://domain.com")  # Navigate to the specified URL
```
<!-- ---------- -->
#### **Deno**
<!-- ---------- -->
```ts
// In development...
```
<!-- ---------- -->
#### **Go**
<!-- ---------- -->
```go
// In development...
```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
// In development...
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v
// In development...
```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param window The window number
    * @param url Full HTTP URL
    */

    ui.navigate(win, "/foo/bar.html")

    // [!] Note:
    //
    // If 'bar.html' does not include 'webui.js' then
    // WebUI will try to close the window and 'wait()'
    // will break. It's important to include 'webui.js'
    // in every HTML.
}
```
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->
```zig
// In development...
```
<!-- ---------- -->
#### **Rust**
<!-- ---------- -->
```rust
// In development...
```
<!-- ---------- -->
#### **PHP**
<!-- ---------- -->
```php
// In development...
```
<!-- ---------- -->
#### **Other...**
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
**Swift**
<!-- ---------- -->
```swift
// In development...
```
<!-- ---------- -->
**C-Sharp**
<!-- ---------- -->
```csharp
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

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### clean

Free all memory resources. Should be called only at the end.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
#include "webui.h"

int main() {

    webui_clean();
    return 0;
}
```
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
#include "webui.hpp"

int main() {

    webui::clean();
    return 0;
}
```
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python
from webui import webui

webui.wait()
webui.clean() # Free all WebUI-related resources
```
<!-- ---------- -->
#### **Deno**
<!-- ---------- -->
```ts
// In development...
```
<!-- ---------- -->
#### **Go**
<!-- ---------- -->
```go
// In development...
```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
// In development...
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v
// In development...
```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```odin
package main

import ui "webui"

main :: proc() {

    ui.clean()
}
```
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->
```zig
// In development...
```
<!-- ---------- -->
#### **Rust**
<!-- ---------- -->
```rust
// In development...
```
<!-- ---------- -->
#### **PHP**
<!-- ---------- -->
```php
// In development...
```
<!-- ---------- -->
#### **Other...**
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
**Swift**
<!-- ---------- -->
```swift
// In development...
```
<!-- ---------- -->
**C-Sharp**
<!-- ---------- -->
```csharp
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

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### delete_all_profiles

Delete a specific window web-browser local folder profile.

> It's recommended to be called when program exit, and after all windows are closed.

> [!] This can break functionality of running windows if using the same web-browser profile.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
#include "webui.h"

int main() {

    webui_delete_all_profiles();
    return 0;
}
```
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
#include "webui.hpp"

int main() {

    webui::delete_all_profiles();
    return 0;
}
```
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python
from webui import webui

webui.wait()  
webui.delete_all_profiles()  # Delete all browser profiles
webui.clean()  
```
<!-- ---------- -->
#### **Deno**
<!-- ---------- -->
```ts
// In development...
```
<!-- ---------- -->
#### **Go**
<!-- ---------- -->
```go
// In development...
```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
// In development...
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v
// In development...
```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```odin
package main

import ui "webui"

main :: proc() {

    ui.delete_all_profiles()
}
```
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->
```zig
// In development...
```
<!-- ---------- -->
#### **Rust**
<!-- ---------- -->
```rust
// In development...
```
<!-- ---------- -->
#### **PHP**
<!-- ---------- -->
```php
// In development...
```
<!-- ---------- -->
#### **Other...**
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
**Swift**
<!-- ---------- -->
```swift
// In development...
```
<!-- ---------- -->
**C-Sharp**
<!-- ---------- -->
```csharp
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

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### delete_profile

Delete a specific window web-browser local folder profile.

> It's recommended to be called when program exit, and after all windows are closed.

> [!] This can break functionality of other running windows if using the same web-browser profile.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
#include "webui.h"

int main() {

    /*
    * @param window The window number
    */

    webui_delete_profile(win);
    return 0;
}
```
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
#include "webui.hpp"

int main() {

    win.delete_profile();
    return 0;
}
```
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python
from webui import webui

my_window = webui.Window()

my_window.delete_profile()  # Delete the browser profile for this window
```
<!-- ---------- -->
#### **Deno**
<!-- ---------- -->
```ts
// In development...
```
<!-- ---------- -->
#### **Go**
<!-- ---------- -->
```go
// In development...
```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
// In development...
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v
// In development...
```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param window The window number
    */

    ui.delete_profile(my_window)
}
```
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->
```zig
// In development...
```
<!-- ---------- -->
#### **Rust**
<!-- ---------- -->
```rust
// In development...
```
<!-- ---------- -->
#### **PHP**
<!-- ---------- -->
```php
// In development...
```
<!-- ---------- -->
#### **Other...**
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
**Swift**
<!-- ---------- -->
```swift
// In development...
```
<!-- ---------- -->
**C-Sharp**
<!-- ---------- -->
```csharp
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

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_parent_process_id

Get the ID of the parent process (_The web browser may re-create another new process_).

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
#include "webui.h"

int main() {

    /*
    * @param window The window number
    */

    size_t parent_pid = webui_get_parent_process_id(win);
}
```
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
#include "webui.hpp"

int main() {

    size_t parent_pid = win.get_parent_process_id();
}
```
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python
from webui import webui

my_window = webui.Window()

parent_pid = my_window.get_parent_process_id()
print(f"Parent Process ID: {parent_pid}")
```
<!-- ---------- -->
#### **Deno**
<!-- ---------- -->
```ts
// In development...
```
<!-- ---------- -->
#### **Go**
<!-- ---------- -->
```go
// In development...
```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
// In development...
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v
// In development...
```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param window The window number
    */

    parent_pid: uint = ui.get_parent_process_id(my_window)
}
```
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->
```zig
// In development...
```
<!-- ---------- -->
#### **Rust**
<!-- ---------- -->
```rust
// In development...
```
<!-- ---------- -->
#### **PHP**
<!-- ---------- -->
```php
// In development...
```
<!-- ---------- -->
#### **Other...**
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
**Swift**
<!-- ---------- -->
```swift
// In development...
```
<!-- ---------- -->
**C-Sharp**
<!-- ---------- -->
```csharp
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

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_child_process_id

Get the ID of the last child process (_The web browser may re-create other child process_).

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
#include "webui.h"

int main() {

    /*
    * @param window The window number
    */

    size_t child_pid = webui_get_child_process_id(win);
}
```
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
#include "webui.hpp"

int main() {

    size_t child_pid = win.get_child_process_id();
}
```
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python
from webui import webui

my_window = webui.Window()

child_pid = my_window.get_child_process_id()
print(f"Child Process ID: {child_pid}")
```
<!-- ---------- -->
#### **Deno**
<!-- ---------- -->
```ts
// In development...
```
<!-- ---------- -->
#### **Go**
<!-- ---------- -->
```go
// In development...
```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
// In development...
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v
// In development...
```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param window The window number
    */

    child_pid: uint = ui.get_child_process_id(my_window)
}
```
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->
```zig
// In development...
```
<!-- ---------- -->
#### **Rust**
<!-- ---------- -->
```rust
// In development...
```
<!-- ---------- -->
#### **PHP**
<!-- ---------- -->
```php
// In development...
```
<!-- ---------- -->
#### **Other...**
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
**Swift**
<!-- ---------- -->
```swift
// In development...
```
<!-- ---------- -->
**C-Sharp**
<!-- ---------- -->
```csharp
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

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_port

Set a custom web-server network port to be used by WebUI. This can be useful to determine the HTTP link of `webui.js` in case you are trying to use WebUI with an external web-server like NGNIX.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
#include "webui.h"

int main() {

    /*
    * @param window The window number
    * @param port The web-server network port WebUI should use
    */

    bool ret = webui_set_port(win, 8080);

    if (!ret) {
        printf("The port is busy and not usable.\n");
    }

    // The port `8080` will not be used by WebUI
    // until we show the window. The window URL
    // then will be: http://localhost:8080/
}
```

Full C Example: https://github.com/webui-dev/webui/tree/main/examples/C/custom_web_server
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
#include "webui.hpp"

int main() {

    /*
    * @param port The web-server network port WebUI should use
    */

    bool ret = win.set_port(8080);

    if (!ret) {
        printf("The port is busy and not usable.\n");
    }

    // The port `8080` will not be used by WebUI
    // until we show the window. The window URL
    // then will be: http://localhost:8080/
}
```
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python
from webui import webui

my_window = webui.Window()

success = my_window.set_port(8080)
if success:
    print("WebUI is now using port 8080.")
else:
    print("Port 8080 is unavailable.")
```
<!-- ---------- -->
#### **Deno**
<!-- ---------- -->
```ts
// In development...
```
<!-- ---------- -->
#### **Go**
<!-- ---------- -->
```go
// In development...
```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
// In development...
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v
// In development...
```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param window The window number
    * @param port The web-server network port WebUI should use
    */

    ret: bool = ui.set_port(my_window, 8080)

    if !ret {
        fmt.printfln("The port is busy and not usable.")
    }

    // The port '8080' will not be used by WebUI
    // until we show the window. The window URL
    // then will be: http://localhost:8080/
}
```

Full Odin Example: https://github.com/webui-dev/odin-webui/tree/main/examples/custom_web_server
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->
```zig
// In development...
```
<!-- ---------- -->
#### **Rust**
<!-- ---------- -->
```rust
// In development...
```
<!-- ---------- -->
#### **PHP**
<!-- ---------- -->
```php
// In development...
```
<!-- ---------- -->
#### **Other...**
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
**Swift**
<!-- ---------- -->
```swift
// In development...
```
<!-- ---------- -->
**C-Sharp**
<!-- ---------- -->
```csharp
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

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_config

Control and change the WebUI global settings.

> It's recommended to be called at the beginning.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
#include "webui.h"

/*
    enum {
        // Control if `webui_show()`, `webui_show_browser()` and
        // `webui_show_wv()` should wait for the window to connect
        // before returns or not.
        //
        // Default: True
        show_wait_connection = 0,
        // Control if WebUI should block and process the UI events
        // one a time in a single thread `True`, or process every
        // event in a new non-blocking thread `False`. This updates
        // all windows. You can use `webui_set_event_blocking()` for
        // a specific single window update.
        //
        // Default: False
        ui_event_blocking,
        // Automatically refresh the window UI when any file in the
        // root folder gets changed.
        //
        // Default: False
        folder_monitor,
        // Allow multiple clients to connect to the same window,
        // This is helpful for web apps (non-desktop software),
        // Please see the documentation for more details.
        //
        // Default: False
        multi_client,
        // Allow multiple clients to connect to the same window,
        // This is helpful for web apps (non-desktop software),
        // Please see the documentation for more details.
        //
        // Default: False
        use_cookies,
    };
*/

int main() {

    /*
    * @param option The desired option from `webui_config` enum
    * @param status The status of the option, `true` or `false`
    */

    webui_set_config(show_wait_connection, true);
    webui_set_config(ui_event_blocking, false);
    webui_set_config(folder_monitor, true);
}
```
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
#include "webui.hpp"

/*
    enum {
        // Control if `webui_show()`, `webui_show_browser()` and
        // `webui_show_wv()` should wait for the window to connect
        // before returns or not.
        //
        // Default: True
        show_wait_connection = 0,
        // Control if WebUI should block and process the UI events
        // one a time in a single thread `True`, or process every
        // event in a new non-blocking thread `False`. This updates
        // all windows. You can use `webui_set_event_blocking()` for
        // a specific single window update.
        //
        // Default: False
        ui_event_blocking,
        // Automatically refresh the window UI when any file in the
        // root folder gets changed.
        //
        // Default: False
        folder_monitor,
        // Allow multiple clients to connect to the same window,
        // This is helpful for web apps (non-desktop software),
        // Please see the documentation for more details.
        //
        // Default: False
        multi_client,
        // Allow multiple clients to connect to the same window,
        // This is helpful for web apps (non-desktop software),
        // Please see the documentation for more details.
        //
        // Default: False
        use_cookies,
    };
*/

int main() {

    /*
    * @param option The desired option from `webui_config` enum
    * @param status The status of the option, `true` or `false`
    */

    webui::set_config(show_wait_connection, true);
    webui::set_config(ui_event_blocking, false);
    webui::set_config(folder_monitor, true);
}
```
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
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

# Allow multiple clients to connect to the same window,
# This is helpful for web apps (non-desktop software),
# Please see the documentation for more details.
#
# use_cookies,

webui.set_config(webui.Config.show_wait_connection, False)  # Disable waiting for connection
```
<!-- ---------- -->
#### **Deno**
<!-- ---------- -->
```ts
// In development...
```
<!-- ---------- -->
#### **Go**
<!-- ---------- -->
```go
// In development...
```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
// In development...
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v
// In development...
```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```odin
package main

import ui "webui"

main :: proc() {

    /*
        Config :: enum {
        // Control if 'webui_show()', 'webui_show_browser()' and
        // 'webui_show_wv()' should wait for the window to connect
        // before returns or not.
        //
        // Default: True
        show_wait_connection,
        // Control if WebUI should block and process the UI events
        // one a time in a single thread 'True', or process every
        // event in a new non-blocking thread 'False'. This updates
        // all windows. You can use 'webui_set_event_blocking()' for
        // a specific single window update.
        //
        // Default: False
        ui_event_blocking,
        // Automatically refresh the window UI when any file in the
        // root folder gets changed.
        //
        // Default: False
        folder_monitor,
        // Allow multiple clients to connect to the same window,
        // This is helpful for web apps (non-desktop software),
        // Please see the documentation for more details.
        //
        // Default: False
        multi_client,
        // Allow or prevent WebUI from adding 'webui_auth' cookies.
        // WebUI uses these cookies to identify clients and block
        // unauthorized access to the window content using a URL.
        // Please keep this option to 'True' if you want only a single
        // client to access the window content.
        //
        // Default: True
        use_cookies,
        // If the backend uses asynchronous operations, set this
        // option to 'True'. This will make webui wait until the
        // backend sets a response using 'webui_return_x()'.
        asynchronous_response
    */

    /*
    * @param option The desired option from 'webui_config' enum
    * @param status The status of the option, 'true' or 'false'
    */

    ui.set_config(.show_wait_connection, true)
    ui.set_config(.ui_event_blocking, false)
    ui.set_config(.folder_monitor, true)
}
```
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->
```zig
// In development...
```
<!-- ---------- -->
#### **Rust**
<!-- ---------- -->
```rust
// In development...
```
<!-- ---------- -->
#### **PHP**
<!-- ---------- -->
```php
// In development...
```
<!-- ---------- -->
#### **Other...**
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
**Swift**
<!-- ---------- -->
```swift
// In development...
```
<!-- ---------- -->
**C-Sharp**
<!-- ---------- -->
```csharp
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

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_event_blocking

Control if UI events comming from this window should be processed one a time in a single blocking thread `True`, or process every event in a new non-blocking thread `False`. This update single window. You can use `set_config()` to update all windows.

> Note: If this is set to `True`, the API `script()` won't return any response until this current event is finished.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
#include "webui.h"

void foo(webui_event_t* e) {
    //
}

void bar(webui_event_t* e) {
    //
}

int main() {
    webui_bind(win, "foo", foo);
    webui_bind(win, "bar", bar);

    /*
    * @param window The window number
    * @param status The blocking status `true` or `false`
    */

    webui_set_event_blocking(win, true);

    // Now, every UI event will be processed
    // in one single thread, other UI events
    // will be blocked until first event end

    // JavaScript:
    // foo();
    // bar();
}
```
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
#include "webui.hpp"

void foo(webui::window::event* e) {
    //
}

void bar(webui::window::event* e) {
    //
}

int main() {
    win.bind("foo", foo);
    win.bind("bar", bar);

    /*
    * @param status The blocking status `true` or `false`
    */

    win.set_event_blocking(true);

    // Now, every UI event will be processed
    // in one single thread, other UI events
    // will be blocked until first event end

    // JavaScript:
    // foo();
    // bar();
}
```
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python
from webui import webui

my_window = webui.Window()

my_window.set_event_blocking(True)  # Enable blocking event processing
my_window.set_event_blocking(False) # Enable non-blocking event processing
```
<!-- ---------- -->
#### **Deno**
<!-- ---------- -->
```ts
// In development...
```
<!-- ---------- -->
#### **Go**
<!-- ---------- -->
```go
// In development...
```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
// In development...
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v
// In development...
```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```odin
package main

import ui "webui"

foo :: proc "c" (e: ^ui.Event) {
    context = runtime.default_context()
    //
}

bar :: proc "c" (e: ^ui.Event) {
    context = runtime.default_context()
    //
}

main :: proc() {

    ui.bind(my_window, "foo", foo)
    ui.bind(my_window, "bar", bar)

    /*
    * @param window The window number
    * @param status The blocking status 'true' or 'false'
    */

    ui.set_event_blocking(my_window, true)

    // Now, every UI event will be processed
    // in one single thread, other UI events
    // will be blocked until first event end

    // JavaScript:
    // foo();
    // bar();
}
```
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->
```zig
// In development...
```
<!-- ---------- -->
#### **Rust**
<!-- ---------- -->
```rust
// In development...
```
<!-- ---------- -->
#### **PHP**
<!-- ---------- -->
```php
// In development...
```
<!-- ---------- -->
#### **Other...**
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
**Swift**
<!-- ---------- -->
```swift
// In development...
```
<!-- ---------- -->
**C-Sharp**
<!-- ---------- -->
```csharp
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

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_tls_certificate

Set the SSL/TLS certificate and the private key content in PEM format. If set empty WebUI will generate a self-signed certificate.

> This works only with the TLS version of WebUI `webui-2-secure`.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
#include "webui.h"

int main() {

    /*
    * @param certificate_pem The SSL/TLS certificate content in PEM format
    * @param private_key_pem The private key content in PEM format
    */

    bool ret = webui_set_tls_certificate(
        "-----BEGIN CERTIFICATE-----"
        "...",
        "-----BEGIN PRIVATE KEY-----"
        "..."
    );

    if (!ret) {
        printf("Invalid TLS certificate.\n");
    }

    // Now, all windows will use `HTTPS` instead of `HTTP`.
}
```
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
#include "webui.hpp"

int main() {

    /*
    * @param certificate_pem The SSL/TLS certificate content in PEM format
    * @param private_key_pem The private key content in PEM format
    */

    bool ret = webui::set_tls_certificate(
        "-----BEGIN CERTIFICATE-----"
        "...",
        "-----BEGIN PRIVATE KEY-----"
        "..."
    );

    if (!ret) {
        printf("Invalid TLS certificate.\n");
    }

    // Now, all windows will use `HTTPS` instead of `HTTP`.
}
```
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
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
<!-- ---------- -->
#### **Deno**
<!-- ---------- -->
```ts
// In development...
```
<!-- ---------- -->
#### **Go**
<!-- ---------- -->
```go
// In development...
```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
// In development...
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v
// In development...
```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param certificate_pem The SSL/TLS certificate content in PEM format
    * @param private_key_pem The private key content in PEM format
    */

    ret: bool = ui.set_tls_certificate(
        "-----BEGIN CERTIFICATE-----\n...",
        "-----BEGIN PRIVATE KEY-----\n..."
    )

    if !ret {
        fmt.printfln("Invalid TLS certificate.")
    }
}
```
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->
```zig
// In development...
```
<!-- ---------- -->
#### **Rust**
<!-- ---------- -->
```rust
// In development...
```
<!-- ---------- -->
#### **PHP**
<!-- ---------- -->
```php
// In development...
```
<!-- ---------- -->
#### **Other...**
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
**Swift**
<!-- ---------- -->
```swift
// In development...
```
<!-- ---------- -->
**C-Sharp**
<!-- ---------- -->
```csharp
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

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### run

Run JavaScript without waiting for the response.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
#include "webui.h"

void callback(webui_event_t* e) {

    /*
    * @param e The event struct
    * @param script The JavaScript to be run
    */

    // Run javascript for one specific client

    webui_run_client(e, "alert('Foo Bar');");
}

int main() {

    /*
    * @param window The window number
    * @param script The JavaScript to be run
    */

    // Run javascript for all connected clients in a window

    webui_run(win, "alert('Foo Bar');");
}
```

Full C Example: https://github.com/webui-dev/webui/tree/main/examples/C/call_js_from_c
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
#include "webui.hpp"

void callback(webui::window::event* e) {

    /*
    * @param e The event struct
    * @param script The JavaScript to be run
    */

    // Run javascript for one specific client

    win.run_client(e, "alert('Foo Bar');");
}

int main() {

    /*
    * @param script The JavaScript to be run
    */

    // Run javascript for all connected clients in a window

    win.run("alert('Foo Bar');");
}
```

Full C++ Example: https://github.com/webui-dev/webui/tree/main/examples/C%2B%2B/call_js_from_cpp
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python
from webui import webui

my_window = webui.Window()

my_window.run("alert('Hello');")  # Run an alert in the web UI
```
<!-- ---------- -->
#### **Deno**
<!-- ---------- -->
```ts
// In development...
```
<!-- ---------- -->
#### **Go**
<!-- ---------- -->
```go
// In development...
```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
// In development...
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v
// In development...
```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```odin
package main

import ui "webui"

callback :: proc "c" (e: ^ui.Event) {

    /*
    * @param e The event struct
    * @param script The JavaScript to be run
    */

    // Run javascript for one specific client

    ui.run_client(e, "alert('Foo Bar');")
}

main :: proc() {

    /*
    * @param window The window number
    * @param script The JavaScript to be run
    */

    // Run javascript for all connected clients in a window

    ui.run(my_window, "alert('Foo Bar');")
}
```

Full Odin Example: https://github.com/webui-dev/odin-webui/blob/main/examples/call_js_from_odin.odin
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->
```zig
// In development...
```
<!-- ---------- -->
#### **Rust**
<!-- ---------- -->
```rust
// In development...
```
<!-- ---------- -->
#### **PHP**
<!-- ---------- -->
```php
// In development...
```
<!-- ---------- -->
#### **Other...**
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
**Swift**
<!-- ---------- -->
```swift
// In development...
```
<!-- ---------- -->
**C-Sharp**
<!-- ---------- -->
```csharp
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

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### script

Run JavaScript and get the response back.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
#include "webui.h"

void callback(webui_event_t* e) {

    /*
    * @param e The event struct
    * @param script The JavaScript to be run
    * @param timeout The execution timeout
    * @param buffer The local buffer to hold the response
    * @param buffer_length The local buffer size
    */

    // Run javascript for one specific client

    char response[64];
    if (!webui_script_client(e, "return 4 + 6;", 0, response, 64)) {
        printf("JavaScript Error: %s\n", response);
    }
    else {
        printf("JavaScript Response: %s\n", response); // 10
    }
}

int main() {

    /*
    * @param window The window number
    * @param script The JavaScript to be run
    * @param timeout The execution timeout
    * @param buffer The local buffer to hold the response
    * @param buffer_length The local buffer size
    */

    // Run javascript

    char response[64];
    if (!webui_script(win, "return 4 + 6;", 0, response, 64)) {
        printf("JavaScript Error: %s\n", response);
    }
    else {
        printf("JavaScript Response: %s\n", response); // 10
    }
}
```

Full C Example: https://github.com/webui-dev/webui/tree/main/examples/C/call_js_from_c
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
#include "webui.hpp"

int main() {

    /*
    * @param script The JavaScript to be run
    * @param timeout The execution timeout
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
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python
from webui import webui

my_window = webui.Window()

result = my_window.script("return 4 + 6;")
if result.error:
    print("JavaScript execution failed.")
else:
    print(f"JavaScript result: {result.response}")
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
package main

import ui "webui"

callback :: proc "c" (e: ^ui.Event) {
    context = runtime.default_context()

    /*
    * @param e The event struct
    * @param script The JavaScript to be run
    * @param timeout The execution timeout
    * @param buffer The local buffer to hold the response
    * @param buffer_length The local buffer size
    */

    // Run javascript for one specific client

    response: cstring = ""
    if !ui.script_client(e, "return 4 + 6;", 0, response, 64) {
        fmt.printfln("JavaScript Error: %s", response)
    }
    else {
        fmt.printfln("JavaScript Response: %s", response) // 10
    }
}

main :: proc() {

    /*
    * @param window The window number
    * @param script The JavaScript to be run
    * @param timeout The execution timeout
    * @param buffer The local buffer to hold the response
    * @param buffer_length The local buffer size
    */

    // Run javascript

    response: cstring = ""
    if !ui.webui_script(my_window, "return 4 + 6;", 0, response, 64) {
        fmt.printfln("JavaScript Error: %s", response)
    }
    else {
        fmt.printfln("JavaScript Response: %s", response) // 10
    }
}
```

Full Odin Example: https://github.com/webui-dev/odin-webui/blob/main/examples/call_js_from_odin.odin
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->
```zig
fn myFunction(e: *webui.Event) void {
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
#### **Rust**
<!-- ---------- -->
```rust
fn main() {
  // ...
  win.run_js("console.log('Hello from the backend!')");
}

fn event_handler(e: webui::Event) {
  e.get_window().run_js("console.log('Hello from the event handler!')");
}
```
<!-- ---------- -->
#### **PHP**
<!-- ---------- -->
```php
// In development...
```
<!-- ---------- -->
#### **Other...**
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
**Swift**
<!-- ---------- -->
```swift
// In development...
```
<!-- ---------- -->
**C-Sharp**
<!-- ---------- -->
```csharp
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

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### set_runtime

Chose between `Deno`, `Bun` and `Nodejs` as runtime for `.js` and `.ts` files.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
#include "webui.h"

int main() {

    /*
    * @param window The window number
    * @param runtime Deno | Bun | Nodejs | None
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
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
#include "webui.hpp"

int main() {

    /*
    * @param runtime Deno | Bun | Nodejs | None
    */

    win.set_runtime(Deno);

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

Full C++ Example: https://github.com/webui-dev/webui/tree/main/examples/C++/serve_a_folder
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python
from webui import webui

my_window = webui.Window()

my_window.set_runtime(webui.Runtime.Bun)  # Use Bun as the JavaScript/TypeScript runtime
```
<!-- ---------- -->
#### **Deno**
<!-- ---------- -->
```ts
// In development...
```
<!-- ---------- -->
#### **Go**
<!-- ---------- -->
```go
// Deno
webui.SetRuntime(win, webui.Deno)

// Bun
webui.SetRuntime(win, webui.Bun)

// Nodejs
webui.SetRuntime(win, webui.Nodejs)

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
```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
# Deno
myWindow.runtime = Deno

# Bun
myWindow.runtime = Bun

# Nodejs
myWindow.runtime = NodeJS

# Now, any HTTP request to any `.js` or `.ts` file
# will be interpreted by Deno.
# 
# JavaScript:
# 
# var xmlHttp = new XMLHttpRequest();
# xmlHttp.open('GET', 'test.ts?foo=123&bar=456', false);
# xmlHttp.send(null);
# 
# console.log(xmlHttp.responseText);
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v
// Deno
win.set_runtime(.runtime_deno)

// Bun
win.set_runtime(.runtime_bun)

// Nodejs
win.set_runtime(.runtime_nodejs)

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
```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param window The window number
    * @param runtime None | Deno | Nodejs | Bun
    */

    ui.set_runtime(my_window, .Deno)

    // Now, any HTTP request to any '.js' or '.ts' file
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

Full Odin Example: https://github.com/webui-dev/odin-webui/tree/main/examples/serve_a_folder
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->
```zig
// Deno
win.setRuntime(.Deno);

// Bun
win.setRuntime(.Bun);

// Nodejs
win.setRuntime(.Nodejs);

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
```
<!-- ---------- -->
#### **Rust**
<!-- ---------- -->
```rust
// Deno
win.set_runtime(webui::WebUIRuntime::Deno);

// Bun
win.set_runtime(webui::WebUIRuntime::Bun);

// Nodejs
win.set_runtime(webui::WebUIRuntime::Nodejs);

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
```
<!-- ---------- -->
#### **PHP**
<!-- ---------- -->
```php
// In development...
```
<!-- ---------- -->
#### **Other...**
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
**Swift**
<!-- ---------- -->
```swift
// In development...
```
<!-- ---------- -->
**C-Sharp**
<!-- ---------- -->
```csharp
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

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_count

Get how many arguments there are in an event.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
void callback(webui_event_t* e) {

    // JavaScript:
    // callback("Foo", "Bar", 123, true);

    size_t count = webui_get_count(e); // 4
}
```

Full C Example: https://github.com/webui-dev/webui/tree/main/examples/C/call_c_from_js
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
void callback(webui::window::event* e) {

    // JavaScript:
    // callback("Foo", "Bar", 123, true);

    size_t count = e->get_count(); // 4
}
```

Full C++ Example: https://github.com/webui-dev/webui/tree/main/examples/C++/call_cpp_from_js
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python
from webui import webui

def callback(e: webui.Event):
    count = e.get_count()
    print(f"The event has {count} arguments.")
```
<!-- ---------- -->
#### **Deno**
<!-- ---------- -->
```ts
// In development...
```
<!-- ---------- -->
#### **Go**
<!-- ---------- -->
```go
// In development...
```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
// In development...
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v
// In development...
```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```odin
package main

import ui "webui"

callback :: proc "c" (e: ^ui.Event) {
    context = runtime.default_context()

    // JavaScript:
    // callback("Foo", "Bar", 123, true);

    count: uint = ui.get_count(e) // 4
}
```

Full Odin Example: https://github.com/webui-dev/odin-webui/blob/main/examples/call_odin_from_js.odin
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->
```zig
// In development...
```
<!-- ---------- -->
#### **Rust**
<!-- ---------- -->
```rust
// In development...
```
<!-- ---------- -->
#### **PHP**
<!-- ---------- -->
```php
// In development...
```
<!-- ---------- -->
#### **Other...**
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
**Swift**
<!-- ---------- -->
```swift
// In development...
```
<!-- ---------- -->
**C-Sharp**
<!-- ---------- -->
```csharp
// In development...
```
<!-- ---------- -->
**Swift**
<!-- ---------- -->
```swift
// In development...
```
<!-- ---------- -->
**C-Sharp**
<!-- ---------- -->
```csharp
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

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_int_at

Get an argument as integer at a specific index.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
void callback(webui_event_t* e) {

    // JavaScript:
    // callback(12345, 6789);

    long long int n1 = webui_get_int_at(e, 0);
    long long int n2 = webui_get_int_at(e, 1);
}
```

Full C Example: https://github.com/webui-dev/webui/tree/main/examples/C/call_c_from_js
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
void callback(webui::window::event* e) {

    // JavaScript:
    // callback(12345, 6789);

    long long int n1 = e->get_int(0);
    long long int n2 = e->get_int(1);
}
```
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python
from webui import webui

def callback(e: webui.Event):
    value = e.get_int_at(0)
    print(f"The integer at index 0 is {value}.")
```
<!-- ---------- -->
#### **Deno**
<!-- ---------- -->
```ts
// In development...
```
<!-- ---------- -->
#### **Go**
<!-- ---------- -->
```go
// In development...
```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
// In development...
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v
// In development...
```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```odin
package main

import ui "webui"

callback :: proc "c" (e: ^ui.Event) {
    context = runtime.default_context()

    // JavaScript:
    // callback(12345, 6789);

    n1: i64 = ui.get_int_at(e, 0)
    n2: i64 = ui.get_int_at(e, 1)
}
```

Full Odin Example: https://github.com/webui-dev/odin-webui/blob/main/examples/call_odin_from_js.odin
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->
```zig
// In development...
```
<!-- ---------- -->
#### **Rust**
<!-- ---------- -->
```rust
// In development...
```
<!-- ---------- -->
#### **PHP**
<!-- ---------- -->
```php
// In development...
```
<!-- ---------- -->
#### **Other...**
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
**Swift**
<!-- ---------- -->
```swift
// In development...
```
<!-- ---------- -->
**C-Sharp**
<!-- ---------- -->
```csharp
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

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_int

Get the first argument as integer.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
void callback(webui_event_t* e) {

    // JavaScript:
    // callback(123456);

    long long int num = webui_get_int(e);
}
```

Full C Example: https://github.com/webui-dev/webui/tree/main/examples/C/call_c_from_js
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
void callback(webui::window::event* e) {

    // JavaScript:
    // callback(123456);

    long long int num = e->get_int();
}
```
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python
from webui import webui

def callback(e: webui.Event):
    value = e.get_int()
    print(f"The first argument is {value}.")
```
<!-- ---------- -->
#### **Deno**
<!-- ---------- -->
```ts
// In development...
```
<!-- ---------- -->
#### **Go**
<!-- ---------- -->
```go
// In development...
```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
// In development...
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v
// In development...
```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```odin
package main

import ui "webui"

callback :: proc "c" (e: ^ui.Event) {
    context = runtime.default_context()

    // JavaScript:
    // callback(123456);

    num: i64 = ui.get_int(e)
}
```

Full Odin Example: https://github.com/webui-dev/odin-webui/blob/main/examples/call_odin_from_js.odin
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->
```zig
// In development...
```
<!-- ---------- -->
#### **Rust**
<!-- ---------- -->
```rust
// In development...
```
<!-- ---------- -->
#### **PHP**
<!-- ---------- -->
```php
// In development...
```
<!-- ---------- -->
#### **Other...**
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
**Swift**
<!-- ---------- -->
```swift
// In development...
```
<!-- ---------- -->
**C-Sharp**
<!-- ---------- -->
```csharp
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

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_float_at

Get an argument as float at a specific index.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
void callback(webui_event_t* e) {

    // JavaScript:
    // callback(12.34, 56.789);

    double f1 = webui_get_float_at(e, 0);
    double f2 = webui_get_float_at(e, 1);
}
```

Full C Example: https://github.com/webui-dev/webui/tree/main/examples/C/call_c_from_js
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
void callback(webui::window::event* e) {

    // JavaScript:
    // callback(12.34, 56.789);

    double f1 = e->get_float(0);
    double f2 = e->get_float(1);
}
```
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python
from webui import webui

def callback(e: webui.Event):
    value = e.get_float_at(0)
    print(f"The float at index 0 is {value}.")
```
<!-- ---------- -->
#### **Deno**
<!-- ---------- -->
```ts
// In development...
```
<!-- ---------- -->
#### **Go**
<!-- ---------- -->
```go
// In development...
```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
// In development...
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v
// In development...
```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```odin
package main

import ui "webui"

callback :: proc "c" (e: ^ui.Event) {
    context = runtime.default_context()

    // JavaScript:
    // callback(12.34, 56.789);

    f1: f64 = ui.get_float_at(e, 0)
    f2: f64 = ui.get_float_at(e, 1)
}
```

Full Odin Example: https://github.com/webui-dev/odin-webui/blob/main/examples/call_odin_from_js.odin
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->
```zig
// In development...
```
<!-- ---------- -->
#### **Rust**
<!-- ---------- -->
```rust
// In development...
```
<!-- ---------- -->
#### **PHP**
<!-- ---------- -->
```php
// In development...
```
<!-- ---------- -->
#### **Other...**
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
**Swift**
<!-- ---------- -->
```swift
// In development...
```
<!-- ---------- -->
**C-Sharp**
<!-- ---------- -->
```csharp
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

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_float

Get the first argument as float.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
void callback(webui_event_t* e) {

    // JavaScript:
    // callback(123.456);

    double f = webui_get_float(e);
}
```

Full C Example: https://github.com/webui-dev/webui/tree/main/examples/C/call_c_from_js
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
void callback(webui::window::event* e) {

    // JavaScript:
    // callback(123.456);

    double f = e->get_float();
}
```

Full C++ Example: https://github.com/webui-dev/webui/tree/main/examples/C++/call_cpp_from_js
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python
from webui import webui

def callback(e: webui.Event):
    value = e.get_float()
    print(f"The first argument as a float is {value}.")
```
<!-- ---------- -->
#### **Deno**
<!-- ---------- -->
```ts
// In development...
```
<!-- ---------- -->
#### **Go**
<!-- ---------- -->
```go
// In development...
```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
// In development...
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v
// In development...
```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```odin
package main

import ui "webui"

callback :: proc "c" (e: ^ui.Event) {
    context = runtime.default_context()

    // JavaScript:
    // callback(123.456);

    f: f64 = ui.get_float(e)
}
```

Full Odin Example: https://github.com/webui-dev/odin-webui/blob/main/examples/call_odin_from_js.odin
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->
```zig
// In development...
```
<!-- ---------- -->
#### **Rust**
<!-- ---------- -->
```rust
// In development...
```
<!-- ---------- -->
#### **PHP**
<!-- ---------- -->
```php
// In development...
```
<!-- ---------- -->
#### **Other...**
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
**Swift**
<!-- ---------- -->
```swift
// In development...
```
<!-- ---------- -->
**C-Sharp**
<!-- ---------- -->
```csharp
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

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_string_at

Get an argument as string at a specific index.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
void callback(webui_event_t* e) {

    // JavaScript:
    // callback("Foo", "Bar");

    const char* foo = webui_get_string_at(e, 0);
    const char* bar = webui_get_string_at(e, 1);
}
```

Full C Example: https://github.com/webui-dev/webui/tree/main/examples/C/call_c_from_js
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
void callback(webui::window::event* e) {

    // JavaScript:
    // callback("Foo", "Bar");

    const char* foo = e->get_string(0);
    const char* bar = e->get_string(1);
}
```
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python
from webui import webui

def callback(e: webui.Event):
    value = e.get_string_at(0)
    print(f"The string at index 0 is '{value}'.")
```
<!-- ---------- -->
#### **Deno**
<!-- ---------- -->
```ts
// In development...
```
<!-- ---------- -->
#### **Go**
<!-- ---------- -->
```go
// In development...
```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
// In development...
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v
// In development...
```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```odin
package main

import ui "webui"

callback :: proc "c" (e: ^ui.Event) {
    context = runtime.default_context()

    // JavaScript:
    // callback("Foo", "Bar");

    foo: cstring = ui.get_string_at(e, 0)
    bar: cstring = ui.get_string_at(e, 1)
}
```

Full Odin Example: https://github.com/webui-dev/odin-webui/blob/main/examples/call_odin_from_js.odin
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->
```zig
// In development...
```
<!-- ---------- -->
#### **Rust**
<!-- ---------- -->
```rust
// In development...
```
<!-- ---------- -->
#### **PHP**
<!-- ---------- -->
```php
// In development...
```
<!-- ---------- -->
#### **Other...**
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
**Swift**
<!-- ---------- -->
```swift
// In development...
```
<!-- ---------- -->
**C-Sharp**
<!-- ---------- -->
```csharp
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

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_string

Get the first argument as string.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
void callback(webui_event_t* e) {

    // JavaScript:
    // callback("Foo Bar");

    const char* name = webui_get_string(e);
}
```

Full C Example: https://github.com/webui-dev/webui/tree/main/examples/C/call_c_from_js
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
void callback(webui::window::event* e) {

    // JavaScript:
    // callback("Foo Bar");

    const char* name = e->get_string();
}
```
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python
from webui import webui

def callback(e: webui.Event):
    value = e.get_string()
    print(f"The first argument as a string is '{value}'.")
```
<!-- ---------- -->
#### **Deno**
<!-- ---------- -->
```ts
// In development...
```
<!-- ---------- -->
#### **Go**
<!-- ---------- -->
```go
// In development...
```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
// In development...
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v
// In development...
```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```odin
package main

import ui "webui"

callback :: proc "c" (e: ^ui.Event) {
    context = runtime.default_context()

    // JavaScript:
    // callback("Foo Bar");

    name: cstring = ui.get_string(e)
}
```

Full Odin Example: https://github.com/webui-dev/odin-webui/blob/main/examples/call_odin_from_js.odin
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->
```zig
// In development...
```
<!-- ---------- -->
#### **Rust**
<!-- ---------- -->
```rust
// In development...
```
<!-- ---------- -->
#### **PHP**
<!-- ---------- -->
```php
// In development...
```
<!-- ---------- -->
#### **Other...**
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
**Swift**
<!-- ---------- -->
```swift
// In development...
```
<!-- ---------- -->
**C-Sharp**
<!-- ---------- -->
```csharp
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

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_bool_at

Get an argument as boolean at a specific index.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
void callback(webui_event_t* e) {

    // JavaScript:
    // callback(true, false);

    bool status1 = webui_get_bool_at(e, 0);
    bool status2 = webui_get_bool_at(e, 1);
}
```

Full C Example: https://github.com/webui-dev/webui/tree/main/examples/C/call_c_from_js
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
void callback(webui::window::event* e) {

    // JavaScript:
    // callback(true, false);

    bool status1 = e->get_bool(0);
    bool status2 = e->get_bool(1);
}
```
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python
from webui import webui

def callback(e: webui.Event):
    is_valid = e.get_bool_at(0)
    print(f"The boolean value at index 0 is {is_valid}.")
```
<!-- ---------- -->
#### **Deno**
<!-- ---------- -->
```ts
// In development...
```
<!-- ---------- -->
#### **Go**
<!-- ---------- -->
```go
// In development...
```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
// In development...
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v
// In development...
```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```odin
package main

import ui "webui"

callback :: proc "c" (e: ^ui.Event) {
    context = runtime.default_context()

    // JavaScript:
    // callback(true, false);

    status1: bool = ui.get_bool_at(e, 0)
    status2: bool = ui.get_bool_at(e, 1)
}
```

Full Odin Example: https://github.com/webui-dev/odin-webui/blob/main/examples/call_odin_from_js.odin
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->
```zig
// In development...
```
<!-- ---------- -->
#### **Rust**
<!-- ---------- -->
```rust
// In development...
```
<!-- ---------- -->
#### **PHP**
<!-- ---------- -->
```php
// In development...
```
<!-- ---------- -->
#### **Other...**
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
**Swift**
<!-- ---------- -->
```swift
// In development...
```
<!-- ---------- -->
**C-Sharp**
<!-- ---------- -->
```csharp
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

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_bool

Get the first argument as boolean.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
void callback(webui_event_t* e) {

    // JavaScript:
    // callback(true);

    bool status = webui_get_bool(e);
}
```

Full C Example: https://github.com/webui-dev/webui/tree/main/examples/C/call_c_from_js
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
void callback(webui::window::event* e) {

    // JavaScript:
    // callback(true);

    bool status = e->get_bool();
}
```
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python
from webui import webui

def callback(e: webui.Event):
    is_valid = e.get_bool()
    print(f"The first argument as a boolean is {is_valid}.")
```
<!-- ---------- -->
#### **Deno**
<!-- ---------- -->
```ts
// In development...
```
<!-- ---------- -->
#### **Go**
<!-- ---------- -->
```go
// In development...
```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
// In development...
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v
// In development...
```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```odin
package main

import ui "webui"

callback :: proc "c" (e: ^ui.Event) {
    context = runtime.default_context()

    // JavaScript:
    // callback(true);

    status: bool = ui.get_bool(e)
}
```

Full Odin Example: https://github.com/webui-dev/odin-webui/blob/main/examples/call_odin_from_js.odin
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->
```zig
// In development...
```
<!-- ---------- -->
#### **Rust**
<!-- ---------- -->
```rust
// In development...
```
<!-- ---------- -->
#### **PHP**
<!-- ---------- -->
```php
// In development...
```
<!-- ---------- -->
#### **Other...**
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
**Swift**
<!-- ---------- -->
```swift
// In development...
```
<!-- ---------- -->
**C-Sharp**
<!-- ---------- -->
```csharp
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

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_size_at

Get the size in bytes of an argument at a specific index.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
void callback(webui_event_t* e) {

    // JavaScript:
    // callback("Foo", "Bar");

    size_t fooLen = webui_get_size_at(e, 0);
    size_t barLen = webui_get_size_at(e, 1);
}
```

Full C Example: https://github.com/webui-dev/webui/tree/main/examples/C/call_c_from_js
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
void callback(webui::window::event* e) {

    // JavaScript:
    // callback("Foo", "Bar");

    size_t fooLen = e->get_size(0);
    size_t barLen = e->get_size(1);
}
```
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python
from webui import webui

def callback(e: webui.Event):
    arg_size = e.get_size_at(0)
    print(f"The size of the argument at index 0 is {arg_size} bytes.")
```
<!-- ---------- -->
#### **Deno**
<!-- ---------- -->
```ts
// In development...
```
<!-- ---------- -->
#### **Go**
<!-- ---------- -->
```go
// In development...
```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
// In development...
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v
// In development...
```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```odin
package main

import ui "webui"

callback :: proc "c" (e: ^ui.Event) {
    context = runtime.default_context()

    // JavaScript:
    // callback("Foo", "Bar");

    fooLen: uint = ui.get_size_at(e, 0)
    barLen: uint = ui.get_size_at(e, 1)
}
```

Full Odin Example: https://github.com/webui-dev/odin-webui/blob/main/examples/call_odin_from_js.odin
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->
```zig
// In development...
```
<!-- ---------- -->
#### **Rust**
<!-- ---------- -->
```rust
// In development...
```
<!-- ---------- -->
#### **PHP**
<!-- ---------- -->
```php
// In development...
```
<!-- ---------- -->
#### **Other...**
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
**Swift**
<!-- ---------- -->
```swift
// In development...
```
<!-- ---------- -->
**C-Sharp**
<!-- ---------- -->
```csharp
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

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_size

Get size in bytes of the first argument.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
void callback(webui_event_t* e) {

    // JavaScript:
    // callback("Foo");

    size_t fooLen = webui_get_size(e);
}
```

Full C Example: https://github.com/webui-dev/webui/tree/main/examples/C/call_c_from_js
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
void callback(webui::window::event* e) {

    // JavaScript:
    // callback("Foo");

    size_t fooLen = e->get_size();
}
```
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python
from webui import webui

def callback(e: webui.Event):
    arg_size = e.get_size()
    print(f"The size of the first argument is {arg_size} bytes.")
```
<!-- ---------- -->
#### **Deno**
<!-- ---------- -->
```ts
// In development...
```
<!-- ---------- -->
#### **Go**
<!-- ---------- -->
```go
// In development...
```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
// In development...
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v
// In development...
```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```odin
package main

import ui "webui"

callback :: proc "c" (e: ^ui.Event) {
    context = runtime.default_context()

    // JavaScript:
    // callback("Foo");

    fooLen: uint = ui.get_size(e)
}
```

Full Odin Example: https://github.com/webui-dev/odin-webui/blob/main/examples/call_odin_from_js.odin
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->
```zig
// In development...
```
<!-- ---------- -->
#### **Rust**
<!-- ---------- -->
```rust
// In development...
```
<!-- ---------- -->
#### **PHP**
<!-- ---------- -->
```php
// In development...
```
<!-- ---------- -->
#### **Other...**
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
**Swift**
<!-- ---------- -->
```swift
// In development...
```
<!-- ---------- -->
**C-Sharp**
<!-- ---------- -->
```csharp
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

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### return_int

Return the response to JavaScript as integer.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```js
async function myJavaScript() {
    var num = Number(await callback());
    console.log(num);
}
```

```c
void callback(webui_event_t* e) {

    // Return integer
    webui_return_int(e, 123456);
}
```

Full C Example: https://github.com/webui-dev/webui/tree/main/examples/C/call_c_from_js
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```js
async function myJavaScript() {
    var num = Number(await callback());
    console.log(num);
}
```

```cpp
void callback(webui::window::event* e) {

    // Return integer
    e->return_int(123456);
}
```
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python
from webui import webui

def callback(e: webui.Event):
    e.return_int(123)
```
<!-- ---------- -->
#### **Deno**
<!-- ---------- -->
```ts
// In development...
```
<!-- ---------- -->
#### **Go**
<!-- ---------- -->
```go
// In development...
```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
// In development...
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v
// In development...
```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```js
async function myJavaScript() {
    var num = Number(await callback());
    console.log(num);
}
```

```odin
package main

import ui "webui"

callback :: proc "c" (e: ^ui.Event) {
    context = runtime.default_context()

    // Return integer
    ui.return_int(e, 123456)
}
```

Full Odin Example: https://github.com/webui-dev/odin-webui/blob/main/examples/call_odin_from_js.odin
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->
```zig
// In development...
```
<!-- ---------- -->
#### **Rust**
<!-- ---------- -->
```rust
// In development...
```
<!-- ---------- -->
#### **PHP**
<!-- ---------- -->
```php
// In development...
```
<!-- ---------- -->
#### **Other...**
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
**Swift**
<!-- ---------- -->
```swift
// In development...
```
<!-- ---------- -->
**C-Sharp**
<!-- ---------- -->
```csharp
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

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### return_float

Return the response to JavaScript as float.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```js
async function myJavaScript() {
    var f = Number(await callback());
    console.log(f);
}
```

```c
void callback(webui_event_t* e) {

    // Return float
    webui_return_float(e, 123.456);
}
```

Full C Example: https://github.com/webui-dev/webui/tree/main/examples/C/call_c_from_js
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```js
async function myJavaScript() {
    var f = Number(await callback());
    console.log(f);
}
```

```cpp
void callback(webui::window::event* e) {

    // Return float
    e->return_float(123.456);
}
```
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python
from webui import webui

def callback(e: webui.Event):
    e.return_float(123.456)
```
<!-- ---------- -->
#### **Deno**
<!-- ---------- -->
```ts
// In development...
```
<!-- ---------- -->
#### **Go**
<!-- ---------- -->
```go
// In development...
```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
// In development...
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v
// In development...
```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```js
async function myJavaScript() {
    var f = Number(await callback());
    console.log(f);
}
```

```odin
package main

import ui "webui"

callback :: proc "c" (e: ^ui.Event) {
    context = runtime.default_context()

    // Return float
    ui.return_float(e, 123.456)
}
```

Full Odin Example: https://github.com/webui-dev/odin-webui/blob/main/examples/call_odin_from_js.odin
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->
```zig
// In development...
```
<!-- ---------- -->
#### **Rust**
<!-- ---------- -->
```rust
// In development...
```
<!-- ---------- -->
#### **PHP**
<!-- ---------- -->
```php
// In development...
```
<!-- ---------- -->
#### **Other...**
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
**Swift**
<!-- ---------- -->
```swift
// In development...
```
<!-- ---------- -->
**C-Sharp**
<!-- ---------- -->
```csharp
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

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### return_string

Return the response to JavaScript as string.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```js
async function myJavaScript() {
    var name = await callback();
    console.log(name);
}
```

```c
void callback(webui_event_t* e) {

    // Return string
    webui_return_string(e, "Foo Bar");
}
```

Full C Example: https://github.com/webui-dev/webui/tree/main/examples/C/call_c_from_js
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```js
async function myJavaScript() {
    var name = await callback();
    console.log(name);
}
```

```cpp
void callback(webui::window::event* e) {

    // Return string
    e->return_string("Foo Bar");
}
```
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python
from webui import webui

def callback(e: webui.Event):
    e.return_string("Response...")
```
<!-- ---------- -->
#### **Deno**
<!-- ---------- -->
```ts
// In development...
```
<!-- ---------- -->
#### **Go**
<!-- ---------- -->
```go
// In development...
```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
// In development...
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v
// In development...
```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```js
async function myJavaScript() {
    var name = await callback();
    console.log(name);
}
```

```odin
package main

import ui "webui"

callback :: proc "c" (e: ^ui.Event) {
    context = runtime.default_context()

    // Return string
    ui.return_string(e, "Foo Bar")
}
```

Full Odin Example: https://github.com/webui-dev/odin-webui/blob/main/examples/call_odin_from_js.odin
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->
```zig
// In development...
```
<!-- ---------- -->
#### **Rust**
<!-- ---------- -->
```rust
// In development...
```
<!-- ---------- -->
#### **PHP**
<!-- ---------- -->
```php
// In development...
```
<!-- ---------- -->
#### **Other...**
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
**Swift**
<!-- ---------- -->
```swift
// In development...
```
<!-- ---------- -->
**C-Sharp**
<!-- ---------- -->
```csharp
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

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### return_bool

Return the response to JavaScript as boolean.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```js
async function myJavaScript() {
    var status = Boolean(Number(await callback()));
    console.log(status);
}
```

```c
void callback(webui_event_t* e) {

    // Return boolean
    webui_return_bool(e, true);
}
```

Full C Example: https://github.com/webui-dev/webui/tree/main/examples/C/call_c_from_js
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```js
async function myJavaScript() {
    var status = Boolean(Number(await callback()));
    console.log(status);
}
```

```cpp
void callback(webui::window::event* e) {

    // Return boolean
    e->return_bool(true);
}
```
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python
from webui import webui

def callback(e: webui.Event):
    e.return_bool(True)
```
<!-- ---------- -->
#### **Deno**
<!-- ---------- -->
```ts
// In development...
```
<!-- ---------- -->
#### **Go**
<!-- ---------- -->
```go
// In development...
```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
// In development...
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v
// In development...
```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```js
async function myJavaScript() {
    var status = Boolean(Number(await callback()));
    console.log(status);
}
```

```odin
package main

import ui "webui"

callback :: proc "c" (e: ^ui.Event) {
    context = runtime.default_context()

    // Return boolean
    ui.return_bool(e, true)
}
```

Full Odin Example: https://github.com/webui-dev/odin-webui/blob/main/examples/call_odin_from_js.odin
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->
```zig
// In development...
```
<!-- ---------- -->
#### **Rust**
<!-- ---------- -->
```rust
// In development...
```
<!-- ---------- -->
#### **PHP**
<!-- ---------- -->
```php
// In development...
```
<!-- ---------- -->
#### **Other...**
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
**Swift**
<!-- ---------- -->
```swift
// In development...
```
<!-- ---------- -->
**C-Sharp**
<!-- ---------- -->
```csharp
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

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### open_url

Open an URL in the native default web browser.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
webui_open_url("https://webui.me");
```
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
webui::open_url("https://webui.me");
```
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python
from webui import webui

webui.open_url("https://webui.me")  # Open the WebUI website in the default browser
```
<!-- ---------- -->
#### **Deno**
<!-- ---------- -->
```ts
// In development...
```
<!-- ---------- -->
#### **Go**
<!-- ---------- -->
```go
// In development...
```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
// In development...
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v
// In development...
```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param url The URL to open
    */

    ui.open_url("https://webui.me")
}
```
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->
```zig
// In development...
```
<!-- ---------- -->
#### **Rust**
<!-- ---------- -->
```rust
// In development...
```
<!-- ---------- -->
#### **PHP**
<!-- ---------- -->
```php
// In development...
```
<!-- ---------- -->
#### **Other...**
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
**Swift**
<!-- ---------- -->
```swift
// In development...
```
<!-- ---------- -->
**C-Sharp**
<!-- ---------- -->
```csharp
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

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### start_server

Start only the web server and return the URL. This is useful for web app. No window will be shown.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
const char* url = webui_start_server(myWindow, "/full/root/path");
```
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
std::string url = myWindow.start_server("/full/root/path");
```
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python
from webui import webui

my_window = webui.Window()

url = my_window.start_server("/full/root/path")
print(f"Server started at: {url}")
```
<!-- ---------- -->
#### **Deno**
<!-- ---------- -->
```ts
// In development...
```
<!-- ---------- -->
#### **Go**
<!-- ---------- -->
```go
// In development...
```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
// In development...
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v
// In development...
```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param window The window number
    * @param content The HTML, Or a local file
    */

    url: cstring = ui.start_server(myWindow, "/full/root/path")
}
```
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->
```zig
// In development...
```
<!-- ---------- -->
#### **Rust**
<!-- ---------- -->
```rust
// In development...
```
<!-- ---------- -->
#### **PHP**
<!-- ---------- -->
```php
// In development...
```
<!-- ---------- -->
#### **Other...**
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
**Swift**
<!-- ---------- -->
```swift
// In development...
```
<!-- ---------- -->
**C-Sharp**
<!-- ---------- -->
```csharp
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

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_mime_type

Get the HTTP mime type of a file.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
const char* mime = webui_get_mime_type("foo.png");
```
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
std::string mime = webui::get_mime_type("foo.png");
```
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python
from webui import webui

mime_type = webui.get_mime_type("foo.png")
print(f"MIME type: {mime_type}")  # Output: image/png
```
<!-- ---------- -->
#### **Deno**
<!-- ---------- -->
```ts
// In development...
```
<!-- ---------- -->
#### **Go**
<!-- ---------- -->
```go
// In development...
```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
// In development...
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v
// In development...
```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param file The path to and or, with the file included
    */

    mime: cstring = ui.get_mime_type("foo.png")
}
```
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->
```zig
// In development...
```
<!-- ---------- -->
#### **Rust**
<!-- ---------- -->
```rust
// In development...
```
<!-- ---------- -->
#### **PHP**
<!-- ---------- -->
```php
// In development...
```
<!-- ---------- -->
#### **Other...**
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
**Swift**
<!-- ---------- -->
```swift
// In development...
```
<!-- ---------- -->
**C-Sharp**
<!-- ---------- -->
```csharp
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

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_port

Get the network port of a running window. This can be useful to determine the HTTP link of `webui.js`.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
size_t port = webui_get_port(myWindow);
```
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
size_t port = myWindow.get_port();
```
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python
from webui import webui

my_window = webui.Window()

port = my_window.get_port()
print(f"WebUI is running on port: {port}")
```
<!-- ---------- -->
#### **Deno**
<!-- ---------- -->
```ts
// In development...
```
<!-- ---------- -->
#### **Go**
<!-- ---------- -->
```go
// In development...
```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
// In development...
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v
// In development...
```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```odin
package main

import ui "webui"

main :: proc() {

    /*
    * @param window The window number
    */

    port: uint = ui.get_port(my_window)
}
```
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->
```zig
// In development...
```
<!-- ---------- -->
#### **Rust**
<!-- ---------- -->
```rust
// In development...
```
<!-- ---------- -->
#### **PHP**
<!-- ---------- -->
```php
// In development...
```
<!-- ---------- -->
#### **Other...**
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
**Swift**
<!-- ---------- -->
```swift
// In development...
```
<!-- ---------- -->
**C-Sharp**
<!-- ---------- -->
```csharp
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

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### get_free_port

Get an available usable free network port.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
size_t port = webui_get_free_port();
```
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
size_t port = webui::get_free_port();
```
<!-- ---------- -->
#### **Python**
<!-- ---------- -->
```python
from webui import webui

port = webui.get_free_port()
print(f"Available port: {port}")
```
<!-- ---------- -->
#### **Deno**
<!-- ---------- -->
```ts
// In development...
```
<!-- ---------- -->
#### **Go**
<!-- ---------- -->
```go
// In development...
```
<!-- ---------- -->
#### **Nim**
<!-- ---------- -->
```nim
// In development...
```
<!-- ---------- -->
#### **V**
<!-- ---------- -->
```v
// In development...
```
<!-- ---------- -->
#### **Odin**
<!-- ---------- -->
```odin
package main

import ui "webui"

main :: proc() {

    port: uint = ui.get_free_port()
}
```
<!-- ---------- -->
#### **Zig**
<!-- ---------- -->
```zig
// In development...
```
<!-- ---------- -->
#### **Rust**
<!-- ---------- -->
```rust
// In development...
```
<!-- ---------- -->
#### **PHP**
<!-- ---------- -->
```php
// In development...
```
<!-- ---------- -->
#### **Other...**
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
**Swift**
<!-- ---------- -->
```swift
// In development...
```
<!-- ---------- -->
**C-Sharp**
<!-- ---------- -->
```csharp
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

<!-- *********************************************************************** -->
---

### JavaScript - call

> You need to include the virtual file `webui.js` in your HTML

> **Supported arguments types**: `string`, `number`, `boolean`, `Uint8Array`.

There are three different ways to call a backend function from JavaScript:

**1. Using the global section `backend(...)`**

When you bind a function in the backend, WebUI will try to create the backend function object in the global section. If any similar object name already exists in the global section, WebUI will skip the creation of the object but will continue to try it in other places.

```js
// Calling a backend function as global object

myBackendFunction(foo, bar).then((response) => {
    // Do something with `response`
});

// async
const response = await myBackendFunction(foo, bar);

// Without response
myBackendFunction(foo, bar);
```

**2. Using the property of webui object `webui.backend(...)`**

When you bind a function in the backend, WebUI will try to add the backend function object to the properties of the `webui` object. If any similar property already exists, WebUI will skip the creation of the object but will continue to try it in other places.

```js
// Calling a backend function as property of `webui` object

webui.myBackendFunction(foo, bar).then((response) => {
    // Do something with `response`
});

// async
const await webui.myBackendFunction(foo, bar);

// Without response
webui.myBackendFunction(foo, bar);
```

**3. Using the method `call` of webui object `webui.call('backend', ...)`**

If the two above places are not available, then this option is the last way to call a backend function, which is guaranteed to always be available if binded correctly.

```js
// Calling a backend function using the method `webui.call()`

webui.call('myBackendFunction', foo, bar).then((response) => {
    // Do something with `response`
});

// async
const response = await webui.call('myBackendFunction', foo, bar);

// Without response
webui.call('myBackendFunction', foo, bar);
```

How to safely call a backend function when the HTML is loaded? 

```js
document.addEventListener('DOMContentLoaded', function() {
    // DOM is loaded. Check if `webui` object is available
    if (typeof webui !== 'undefined') {
        // Set events callback
        webui.setEventCallback((e) => {
            if (e == webui.event.CONNECTED) {
                // Connection to the backend is established
                // Now it's safe to call a backend function

                myBackendFunction();
            }
        });
    }
});
```

### JavaScript - isConnected

Check if the connection with the backend is established using `webui.isConnected()`.

> You need to include the virtual file `webui.js` in your HTML

```js
if (typeof webui !== 'undefined') {
    if (webui.isConnected()) {
        console.log('Connected.');
    }
}
```

### JavaScript - setEventCallback

Set a callback to receive connection and disconnection events.

> You need to include the virtual file `webui.js` in your HTML

```js
/*
    {
        CONNECTED: 0,
        DISCONNECTED: 1,
    }
*/

document.addEventListener('DOMContentLoaded', function() {
    // DOM is loaded. Check if `webui` object is available
    if (typeof webui !== 'undefined') {
        // Set events callback
        webui.setEventCallback((e) => {
            if (e == webui.event.CONNECTED) {
                // Connection to the backend is established
                console.log('Connected.');
            } else if (e == webui.event.DISCONNECTED) {
                // Connection to the backend is lost
                console.log('Disconnected.');
            }
        });
    } else {
        // The virtual file `webui.js` is not included
        alert('Please add webui.js to your HTML.');
    }
});
```

### JavaScript - encode

Encode text into base64 string.

> You need to include the virtual file `webui.js` in your HTML

```js
const base64 = webui.encode(str);
```

### JavaScript - decode

Decode base64 string into text.

```js
const str = webui.encode(base64);
```

### JavaScript - isHighContrast

Get OS high contrast preference.

```js
const hc = webui.isHighContrast();
```

### JavaScript - setLogging

Enable WebUI debug logging in the console logs.

```js
webui.setLogging(true);
```

### JavaScript - allowNavigation

When binding all events on the backend, WebUI blocks all navigation events and sends them to the backend. This API allows you to control that behavior.

```js
webui.allowNavigation(true); // Allow navigation
window.location.replace('https://test.com'); // This will now proceed as usual
```
