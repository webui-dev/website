<div align="center">

![ScreenShot](data/webui_240_shadow.png)

# WebUI v2.5 Documentation

> Use any web browser or WebView as GUI, with your preferred language in the backend and HTML5 in the frontend, all in a lightweight portable library.

![ScreenShot](data/screenshot.png)

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

# Windows - Microsoft Visual Studio
nmake -f Makefile.nmake

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

# Windows - Microsoft Visual Studio
nmake -f Makefile.nmake

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
git submodule add https://github.com/webui-dev/odin-webui.git webui
webui/setup.sh
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

win = webui.window()
win.show('<html><script src="webui.js"></script> Hello World from Python! </html>')
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

let win = newWindow()
win.show("<html><script src=\"webui.js\"></script> Hello World from Nim! </html>")
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
package main

import ui "webui/webui.odin"

main :: proc() {
	win := ui.new_window()
	ui.show(win, "<html><script src=\"webui.js\"></script> Hello World from Odin! </html>")
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
#### **Other...**
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
### Backend - new_window

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
// In development...
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
### Backend - new_window_id

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
// In development...
```
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
// In development...
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
### Backend - get_new_window_id

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
// In development...
```
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
// In development...
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
### Backend - bind

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
}
```
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
// In development...
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
### Backend - get_best_browser

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
        }
    */

    size_t browserID = win.get_best_browser();
}
```
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
// In development...
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
### Backend - show

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
// In development...
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
### Backend - show_browser

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
// In development...
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
### Backend - show_wv

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
// In development...
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
### Backend - set_kiosk

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
// In development...
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
### Backend - wait

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
// In development...
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
### Backend - close

Close a specific window.

> The `win` object will still alive, and it can be reopen later.

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
    
    webui_close(win);
}
```
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
#include "webui.hpp"

int main() {

    win.close();
}
```
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
// In development...
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
### Backend - destroy

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
// In development...
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
### Backend - exit

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
// In development...
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
### Backend - set_root_folder

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
// In development...
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
### Backend - set_default_root_folder

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
// In development...
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
### Backend - set_file_handler

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
// In development...
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
### Backend - is_shown

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
// In development...
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
### Backend - set_timeout

Set the maximum time in seconds to wait for the window to connect. This effect `show()` and `wait()`.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
#include "webui.h"

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
// In development...
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
### Backend - set_icon

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
// In development...
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
### Backend - encode

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
// In development...
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
### Backend - decode

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
// In development...
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
### Backend - free

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
// In development...
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
### Backend - malloc

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
// In development...
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
### Backend - send_raw

Send raw data (_binary_) to the UI.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
#include "webui.h"

int main() {

    /*
    * @param window The window number
    * @param function The JavaScript function to receive raw data
    * @param raw The raw data buffer
    * @param size The raw data size in bytes
    */
    
    unsigned char buffer[3] = { 0x01, 0x02, 0x03 }; // Any data type
    webui_send_raw(win, "myJavaScriptFunc", buffer, 3);

    // JavaScript:
    //
    // function myJavaScriptFunc(rawData) {
    //    `rawData` is Uint8Array type
    // }
}
```
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
#include "webui.hpp"

int main() {

    /*
    * @param function The JavaScript function to receive raw data
    * @param raw The raw data buffer
    * @param size The raw data size in bytes
    */
    
    unsigned char buffer[3] = { 0x01, 0x02, 0x03 }; // Any data type
    win.send_raw("myJavaScriptFunc", buffer, 3);

    // JavaScript:
    //
    // function myJavaScriptFunc(rawData) {
    //    `rawData` is Uint8Array type
    // }
}
```
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
// In development...
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
### Backend - set_hide

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
// In development...
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
### Backend - set_size

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
// In development...
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
### Backend - set_position

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
// In development...
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
### Backend - set_profile

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
// In development...
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
### Backend - set_proxy

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
// In development...
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
### Backend - get_url

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
// In development...
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
### Backend - set_public

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
// In development...
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
### Backend - navigate

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
// In development...
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
### Backend - clean

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
// In development...
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
### Backend - delete_all_profiles

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
// In development...
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
### Backend - delete_profile

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
// In development...
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
### Backend - get_parent_process_id

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
// In development...
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
### Backend - get_child_process_id

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
// In development...
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
### Backend - set_port

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
// In development...
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
### Backend - set_config

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
        // before return or not.
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
    };
*/

int main() {

    /*
    * @param option The desired option from `webui_config` enum
    * @param status The status of the option, `true` or `false`
    */

    webui_config(show_wait_connection, true);
    webui_config(ui_event_blocking, false);
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
        // before return or not.
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
    };
*/

int main() {

    /*
    * @param option The desired option from `webui_config` enum
    * @param status The status of the option, `true` or `false`
    */

    webui::config(show_wait_connection, true);
    webui::config(ui_event_blocking, false);
}
```
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
// In development...
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
### Backend - set_event_blocking

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
// In development...
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
### Backend - set_tls_certificate

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
// In development...
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
### Backend - run

Run JavaScript without waiting for the response.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
#include "webui.h"

int main() {

    /*
    * @param window The window number
    * @param script The JavaScript to be run
    */

    webui_run(win, "alert('Foo Bar');");
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
    */

    win.run("alert('Foo Bar');");
}
```

Full C++ Example: https://github.com/webui-dev/webui/tree/main/examples/C%2B%2B/call_js_from_cpp
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
// In development...
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
### Backend - script

Run JavaScript and get the response back.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
#include "webui.h"

int main() {

    /*
    * @param window The window number
    * @param script The JavaScript to be run
    * @param timeout The execution timeout
    * @param buffer The local buffer to hold the response
    * @param buffer_length The local buffer size
    */

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
// In development...
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
### Backend - set_runtime

Chose between Deno and Nodejs as runtime for `.js` and `.ts` files.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
#include "webui.h"

int main() {

    /*
    * @param window The window number
    * @param runtime Deno | Nodejs | None
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
    * @param runtime Deno | Nodejs | None
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
// In development...
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
### Backend - get_count

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
// In development...
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
### Backend - get_int_at

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

    long long int n1 = e->get_int_at(0);
    long long int n2 = e->get_int_at(1);
}
```
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
// In development...
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
### Backend - get_int

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
// In development...
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
### Backend - get_float_at

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

    double f1 = e->get_float_at(0);
    double f2 = e->get_float_at(1);
}
```
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
// In development...
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
### Backend - get_float

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
// In development...
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
### Backend - get_string_at

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

    const char* foo = e->get_string_at(0);
    const char* bar = e->get_string_at(1);
}
```
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
// In development...
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
### Backend - get_string

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
// In development...
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
### Backend - get_bool_at

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

    bool status1 = e->get_bool_at(0);
    bool status2 = e->get_bool_at(1);
}
```
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
// In development...
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
### Backend - get_bool

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
// In development...
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
### Backend - get_size_at

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

    size_t fooLen = e->get_size_at(0);
    size_t barLen = e->get_size_at(1);
}
```
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
// In development...
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
### Backend - get_size

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
// In development...
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
### Backend - return_int

Return the response to JavaScript as integer.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
void callback(webui_event_t* e) {

	// JavaScript:
	// var num = await callback();

    // Return
	webui_return_int(e, 123456);
}
```

Full C Example: https://github.com/webui-dev/webui/tree/main/examples/C/call_c_from_js
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
void callback(webui::window::event* e) {

	// JavaScript:
	// var num = await callback();

    // Return
	e->return_int(123456);
}
```
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
// In development...
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
### Backend - return_float

Return the response to JavaScript as float.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
void callback(webui_event_t* e) {

	// JavaScript:
	// var f = await callback();

    // Return
	webui_return_float(e, 123.456);
}
```

Full C Example: https://github.com/webui-dev/webui/tree/main/examples/C/call_c_from_js
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
void callback(webui::window::event* e) {

	// JavaScript:
	// var f = await callback();

    // Return
	e->return_float(123.456);
}
```
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
// In development...
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
### Backend - return_string

Return the response to JavaScript as string.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
void callback(webui_event_t* e) {

	// JavaScript:
	// var name = await callback();

    // Return
	webui_return_string(e, "Foo Bar");
}
```

Full C Example: https://github.com/webui-dev/webui/tree/main/examples/C/call_c_from_js
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
void callback(webui::window::event* e) {

	// JavaScript:
	// var name = await callback();

    // Return
	e->return_string("Foo Bar");
}
```
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
// In development...
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
### Backend - return_bool

Return the response to JavaScript as boolean.

<!-- tabs:start -->
<!-- ---------- -->
#### **C**
<!-- ---------- -->
```c
void callback(webui_event_t* e) {

	// JavaScript:
	// var status = await callback();

    // Return
	webui_return_bool(e, true);
}
```

Full C Example: https://github.com/webui-dev/webui/tree/main/examples/C/call_c_from_js
<!-- ---------- -->
#### **C++**
<!-- ---------- -->
```cpp
void callback(webui::window::event* e) {

	// JavaScript:
	// var status = await callback();

    // Return
	e->return_bool(true);
}
```
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
// In development...
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

### JavaScript - setLogging

Enable WebUI debug logging in the console logs.

```js
webui.setLogging(true);
```
