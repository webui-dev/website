<div align="center">

![Logo](data/webui_120.svg)

# WebUI v2.5 - Bun Documentation

> Use any web browser or WebView as GUI, with your preferred language in the backend and modern web technologies in the frontend, all in a lightweight portable library.

![Screenshot](data/screenshot.png)

</div>


## Getting Started
- [Download And Install](#download-and-install)
- [Minimal Example](#minimal-example)

## Available APIs
- **Window**
  - [new WebUI()](#new-webui)
  - [show](#show)
  - [showBrowser](#showbrowser)
  - [showWebView](#showwebview)
  - [startServer](#startserver)
  - [close](#close)
  - [destroy](#destroy)
  - [isShown](#isshown)
  - [minimize](#minimize)
  - [maximize](#maximize)
  - [focus](#focus)
- **Binding & Events**
  - [bind](#bind)
  - [event](#event)
  - [run](#run)
  - [script](#script)
  - [scriptClient](#scriptclient)
  - [sendRaw](#sendraw)
  - [setFileHandler](#setfilehandler)
  - [setFileHandlerWindow](#setfilehandlerwindow)
- **Window Appearance**
  - [setSize](#setsize)
  - [setMinimumSize](#setminimumsize)
  - [setPosition](#setposition)
  - [setCenter](#setcenter)
  - [setHide](#sethide)
  - [setKiosk](#setkiosk)
  - [setFrameless](#setframeless)
  - [setResizable](#setresizable)
  - [setTransparent](#settransparent)
  - [setHighContrast](#sethighcontrast)
  - [setIcon](#seticon)
- **Window Settings**
  - [setRootFolder](#setrootfolder)
  - [setProfile](#setprofile)
  - [deleteProfile](#deleteprofile)
  - [setProxy](#setproxy)
  - [setPublic](#setpublic)
  - [setPort](#setport)
  - [getPort](#getport)
  - [getUrl](#geturl)
  - [navigate](#navigate)
  - [setRuntime](#setruntime)
  - [setCustomParameters](#setcustomparameters)
  - [setEventBlocking](#seteventblocking)
  - [setCloseHandlerWV](#setclosehandlerwv)
  - [getBestBrowser](#getbestbrowser)
- **Window Info**
  - [getParentProcessId](#getparentprocessid)
  - [getChildProcessId](#getchildprocessid)
  - [windowId](#windowid)
  - [getHwnd](#gethwnd)
  - [win32GetHwnd](#win32gethwnd)
- **Global (Static)**
  - [WebUI.wait](#webuiwait)
  - [WebUI.waitAsync](#webuiwaitasync)
  - [WebUI.exit](#webuiexit)
  - [WebUI.clean](#webuiclean)
  - [WebUI.setTimeout](#webuisettimeout)
  - [WebUI.setMultiClient](#webuisetmulticlient)
  - [WebUI.setDefaultRootFolder](#webuisetdefaultrootfolder)
  - [WebUI.setBrowserFolder](#webuisetbrowserfolder)
  - [WebUI.setFolderMonitor](#webuisetfoldermonitor)
  - [WebUI.setTLSCertificate](#webuitlscertificate)
  - [WebUI.browserExist](#webuibrowserexist)
  - [WebUI.isHighContrast](#webuiishighcontrast)
  - [WebUI.openUrl](#webuiopenurl)
  - [WebUI.getFreePort](#webuigetfreeport)
  - [WebUI.getNewWindowId](#webuigetnewwindowid)
  - [WebUI.newWindowId](#webuinewwindowid)
  - [WebUI.getMimeType](#webuigetmimetype)
  - [WebUI.setLogger](#webuilogger)
  - [WebUI.getLastErrorNumber](#webuilasterrornumber)
  - [WebUI.getLastErrorMessage](#webuilasterrormessage)
  - [WebUI.deleteAllProfiles](#webuideleteallprofiles)
  - [WebUI.encode](#webuiencode)
  - [WebUI.decode](#webuidecode)
  - [WebUI.malloc](#webuimalloc)
  - [WebUI.free](#webuifree)
  - [WebUI.version](#webuiversion)
- **Enums**
  - [WebUI.Browser](#webuibrowser)
  - [WebUI.EventType](#webuieventtype)
  - [WebUI.Runtime](#webuiruntime)
- [WebUI.LoggerLevel](#webuiloggerlevel)

[JavaScript APIs](javascript.md)


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### Download And Install

The WebUI library is written in pure C, but there is a wrapper in every popular programming language to provide easy-to-use APIs to write your backend application, while HTML5 and JavaScript are always used in the frontend inside a web browser or WebView window.

Install via npm:

```sh
npm install @webui-dev/bun-webui
```

Or via [JSR](https://jsr.io/@webui/bun-webui):

```sh
bun add jsr:@webui/bun-webui
```

Import in your project:

```ts
import { WebUI } from '@webui-dev/bun-webui';
// or, if using JSR:
import { WebUI } from 'jsr:@webui/bun-webui';
```

Run your app:

```sh
bun run app.ts
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### Minimal Example

```ts
import { WebUI } from '@webui-dev/bun-webui';

const myWindow = new WebUI();
await myWindow.show('<html><script src="webui.js"></script> Hello World! </html>');
await WebUI.wait();
```

[More Bun Examples](https://github.com/webui-dev/bun-webui/tree/main/examples/).


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### new WebUI()

Create a new window object. Each instance represents an independent browser window.

```ts
const myWindow = new WebUI();
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### show

Show a window using embedded HTML, a local file path, or a URL. If the window is already open, it will be refreshed. Resolves once the browser is connected and ready.

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

> To use only a specific browser please use `showBrowser()`

> To use only WebView please use `showWebView()`

```ts
await myWindow.show('<html><script src="webui.js"></script> Hello! </html>');
await myWindow.show('index.html');
await myWindow.show('https://mydomain.com');
```

<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### showBrowser

Show a window using a specific web browser. Resolves once the browser is connected.

> It is recommended to use `WebUI.Browser.ChromiumBased` for the widest compatibility.

> On macOS, the browser icon may linger in the Dock after exit. Use [`showWebView()`](#showwebview) to avoid this.

```ts
await myWindow.showBrowser(
  '<html><script src="webui.js"></script> Hello from Chrome! </html>',
  WebUI.Browser.Chrome
);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### showWebView

Show a native WebView window using embedded HTML, a local file, or a URL. If the window is already open it will be refreshed.

> Requires `WebView2Loader.dll` on Windows.

```ts
const ok = myWindow.showWebView('<html><script src="webui.js"></script> Hello! </html>');
// or
const ok = myWindow.showWebView('index.html');
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### startServer

Start only the built-in web server for this window without opening a browser. Returns the server URL, which you can open in any browser manually or use with an external server.

```ts
const url = myWindow.startServer('/full/path/to/root');
console.log(`Server running at ${url}`);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### close

Close the window. The window object remains alive and can be shown again later.

```ts
myWindow.close();
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### destroy

Close the window and free all associated memory resources. The window object should not be used after this call.

```ts
myWindow.destroy();
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### isShown

Check whether the window is currently open and connected.

```ts
if (myWindow.isShown) {
  console.log("Window is running.");
} else {
  console.log("Window is closed.");
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### minimize

Minimize the WebView window.

```ts
myWindow.minimize();
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### maximize

Maximize the WebView window.

```ts
myWindow.maximize();
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### focus

Bring the window to the front and give it keyboard focus.

```ts
myWindow.focus();
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### bind

Bind a Bun callback to an HTML element by its `id`. When the element is clicked (or the matching function is called from JavaScript), the callback is invoked with a [`WebUI.Event`](#event).

- An empty string `""` binds to **all** browser events (click, connect, disconnect, navigate).
- The callback's return value is sent back to the browser as the resolved value of the JavaScript promise.

```ts
myWindow.bind('myButton', (e: WebUI.Event) => {
  const name = e.arg.string(0);  // first argument from JS
  const age  = e.arg.number(1);  // second argument from JS
  console.log(`${e.element} clicked — name: ${name}, age: ${age}`);
  return 'Hello from Bun!';
});

// Catch-all: fires on connect, disconnect, clicks, navigation
myWindow.bind('', (e: WebUI.Event) => {
  console.log(`Event type: ${e.eventType}`);
});
```

From the browser side:

```html
<script src="webui.js"></script>
<button id="myButton" onclick="myButton('Alice', 30).then(r => alert(r))">Click me</button>
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### event

Every callback receives a `WebUI.Event` object with context about what happened.

| Property | Type | Description |
|----------|------|-------------|
| `e.window` | `WebUI` | The window that fired the event |
| `e.element` | `string` | The HTML element id |
| `e.eventType` | `WebUI.EventType` | Type of event (click, connect, …) |
| `e.eventNumber` | `number` | Internal event identifier |
| `e.arg.string(index)` | `string` | Get a string argument passed from JS |
| `e.arg.number(index)` | `number` | Get an integer argument passed from JS |
| `e.arg.float(index)` | `number` | Get a floating-point argument passed from JS |
| `e.arg.boolean(index)` | `boolean` | Get a boolean argument passed from JS |
| `e.arg.size(index)` | `number` | Get the byte size of an argument passed from JS |

```ts
myWindow.bind('', (e: WebUI.Event) => {
  if (e.eventType === WebUI.EventType.Connected) {
    console.log('Browser connected.');
  } else if (e.eventType === WebUI.EventType.Disconnected) {
    console.log('Browser disconnected.');
  }
});

myWindow.bind('myFunc', (e: WebUI.Event) => {
  const text    = e.arg.string(0);
  const value   = e.arg.number(1);
  const price   = e.arg.float(2);
  const checked = e.arg.boolean(3);
  return `received: ${text}, ${value}, ${price}, ${checked}`;
});
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### run

Execute a JavaScript string in the browser without waiting for a result. Use this for fire-and-forget UI updates.

```ts
myWindow.run('document.getElementById("status").innerText = "Done!";');
myWindow.run('alert("Hello from Bun!")');
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### script

Execute JavaScript in the browser and get the return value back as a string. Rejects if the script throws an error.

```ts
const result = await myWindow.script('return 2 * 2;').catch(console.error);
// result === "4"

// With a custom timeout (seconds) and response buffer size
const html = await myWindow.script('return document.body.innerHTML;', {
  timeout: 10,
  bufferSize: 1024 * 512,
});
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### scriptClient

Same as [`script()`](#script) but targets one specific connected client. Use this when [`WebUI.setMultiClient(true)`](#webuisetmulticlient) is enabled and you need to send a script to a particular browser tab.

```ts
WebUI.setMultiClient(true);

myWindow.bind('myFunc', async (e: WebUI.Event) => {
  // Run script only in the tab that triggered this event
  const result = await myWindow.scriptClient(e, 'return location.href;');
  console.log('URL of calling tab:', result);
});
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### sendRaw

Send binary data directly to a JavaScript function in the browser. The browser-side function receives the data as a `Uint8Array`.

```ts
const data = new Uint8Array([0x01, 0x02, 0x03]);
myWindow.sendRaw('myJsFunction', data);
```

Browser side:

```js
function myJsFunction(data) {
  console.log(new Uint8Array(data)); // Uint8Array [1, 2, 3]
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### setFileHandler

Register a custom HTTP request handler. All requests made by the browser to the WebUI built-in server will be routed through this callback. Return a complete HTTP response (headers + body) as a `string` or `Uint8Array`.

This is useful for serving dynamic content, virtual filesystems, or integrating with other server frameworks.

```ts
myWindow.setFileHandler(async (url: URL) => {
  if (url.pathname === '/api/time') {
    const body = new Date().toISOString();
    return `HTTP/1.1 200 OK\r\nContent-Length: ${body.length}\r\n\r\n${body}`;
  }
  // Return a 404 for everything else
  return 'HTTP/1.1 404 Not Found\r\nContent-Length: 0\r\n\r\n';
});

await myWindow.show('index.html');
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### setFileHandlerWindow

Like [`setFileHandler()`](#setfilehandler), but the callback also receives the `WebUI` window instance as its first argument. Use this when you need access to the window (e.g. to call `bind`, read `windowId`, or manage multiple windows) inside the handler.

Calling this method supersedes any handler previously set with `setFileHandler()`.

```ts
myWindow.setFileHandlerWindow(async (win: WebUI, url: URL) => {
  console.log(`Window ${win.windowId} requested: ${url.pathname}`);

  if (url.pathname === '/data') {
    const body = JSON.stringify({ id: win.windowId });
    return `HTTP/1.1 200 OK\r\nContent-Type: application/json\r\nContent-Length: ${body.length}\r\n\r\n${body}`;
  }

  return 'HTTP/1.1 404 Not Found\r\nContent-Length: 0\r\n\r\n';
});

await myWindow.show('index.html');
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### setSize

Set the window width and height in pixels.

```ts
myWindow.setSize(1024, 768);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### setMinimumSize

Set the minimum allowed window size. Only works with WebView windows.

```ts
myWindow.setMinimumSize(400, 300);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### setPosition

Set the window position (top-left corner) in screen pixels.

```ts
myWindow.setPosition(100, 200);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### setCenter

Center the window on the screen. Call before [`show()`](#show) for best results. Works best with WebView windows.

```ts
myWindow.setCenter();
await myWindow.show('<html>...</html>');
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### setHide

Launch the window in hidden mode. Call before [`show()`](#show). Useful for background processing or tray applications.

```ts
myWindow.setHide(true);
await myWindow.show('index.html');
// Window is running but not visible
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### setKiosk

Enable or disable kiosk mode (fullscreen, no browser chrome). Call before [`show()`](#show).

```ts
myWindow.setKiosk(true);
await myWindow.show('index.html');
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### setFrameless

Remove the window frame (title bar and border). Only works with WebView windows.

```ts
myWindow.setFrameless(true);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### setResizable

Control whether the user can resize the window. Only works with WebView windows.

```ts
myWindow.setResizable(false); // fixed size
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### setTransparent

Make the WebView window background transparent. Only works with WebView windows.

```ts
myWindow.setTransparent(true);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### setHighContrast

Force high-contrast mode for this window. Useful for building accessible UIs that respond to the OS accessibility setting via CSS.

```ts
myWindow.setHighContrast(true);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### setIcon

Set the favicon shown in the browser tab for embedded HTML content.

```ts
myWindow.setIcon('<svg xmlns="http://www.w3.org/2000/svg">...</svg>', 'image/svg+xml');
myWindow.setIcon(pngBase64String, 'image/png');
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### setRootFolder

Set the root folder from which local files are served for this window. Call before [`show()`](#show).

```ts
myWindow.setRootFolder('./my-app/dist');
await myWindow.show('index.html'); // serves ./my-app/dist/index.html
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### setProfile

Set the browser profile name and directory for this window. Useful for isolating browser data between windows or runs.

```ts
myWindow.setProfile('MyApp', '/path/to/profiles');
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### deleteProfile

Delete the local browser profile folder associated with this window.

```ts
myWindow.deleteProfile();
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### setProxy

Set a proxy server for the browser used by this window. Must be called before [`show()`](#show).

```ts
myWindow.setProxy('http://127.0.0.1:8888');
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### setPublic

Allow the window's built-in web server to be reachable on the local network (binds to `0.0.0.0` instead of `127.0.0.1`).

```ts
myWindow.setPublic(true);
console.log('Server URL:', myWindow.getUrl());
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### setPort

Assign a specific port to this window's built-in web server. Returns `true` if the port was successfully reserved.

```ts
const ok = myWindow.setPort(9000);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### getPort

Get the network port currently used by this window's web server. Useful for constructing the `webui.js` URL when embedding WebUI in an external server.

```ts
const port = myWindow.getPort();
console.log(`http://localhost:${port}/webui.js`);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### getUrl

Get the full URL of this window's built-in web server.

```ts
const url = myWindow.getUrl();
console.log(url); // e.g. "http://localhost:54321/"
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### navigate

Navigate the browser to a new URL or local file path without reopening the window.

```ts
myWindow.navigate('https://webui.me');
myWindow.navigate('other-page.html');
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### setRuntime

Choose the JavaScript runtime used to execute `.js` and `.ts` files served by WebUI.

```ts
myWindow.setRuntime(WebUI.Runtime.Bun);
myWindow.setRuntime(WebUI.Runtime.NodeJS);
myWindow.setRuntime(WebUI.Runtime.Deno);
myWindow.setRuntime(WebUI.Runtime.None); // disable runtime execution
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### setCustomParameters

Pass extra command-line arguments to the browser process launched by WebUI.

```ts
myWindow.setCustomParameters('--remote-debugging-port=9222');
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### setEventBlocking

Control how UI events are dispatched for this window.

- `true` — events are processed one at a time in a single blocking thread (serialized).
- `false` _(default)_ — each event is handled in its own non-blocking thread (concurrent).

```ts
myWindow.setEventBlocking(true); // serialize events
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### setCloseHandlerWV

Control whether the WebView close button is allowed to close the window. Pass `false` to intercept the close event (e.g. to show a confirmation dialog), and `true` to allow it.

> Only works with WebView windows. This is a simple boolean flag — there is no async callback. Set it at any time before the user clicks the close button.

```ts
// Prevent the window from closing when the user clicks the X button
myWindow.setCloseHandlerWV(false);
await myWindow.showWebView('index.html');

// Later, after the user confirms they want to quit:
myWindow.setCloseHandlerWV(true);
myWindow.close();
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### getBestBrowser

Get the ID of the recommended browser for this window. If a browser is already in use, returns that same browser's ID.

```ts
const browserId = myWindow.getBestBrowser();
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### getParentProcessId

Get the PID of the browser's parent process. The browser may spawn child processes — see [`getChildProcessId()`](#getchildprocessid).

```ts
const pid = myWindow.getParentProcessId();
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### getChildProcessId

Get the PID of the last browser child process spawned for this window.

```ts
const childPid = myWindow.getChildProcessId();
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### windowId

Get the unique numeric ID assigned to this window by WebUI. This is a read-only property.

```ts
const id = myWindow.windowId;
console.log(`Window ID: ${id}`);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### getHwnd

Get the native window handle for this window. Returns an `HWND` on Windows or a `GtkWindow*` pointer on Linux.

> Use [`win32GetHwnd()`](#win32gethwnd) instead when working with WebView on Windows, as it is more reliable.

```ts
const hwnd = myWindow.getHwnd();
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### win32GetHwnd

Get the Win32 `HWND` for the WebView window. More reliable than [`getHwnd()`](#gethwnd) when using WebView on Windows.

```ts
const hwnd = myWindow.win32GetHwnd();
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### WebUI.wait

Wait (block) until all open windows have been closed, or until [`WebUI.exit()`](#webuiexit) is called. This is typically the last call in a WebUI application.

```ts
const win = new WebUI();
await win.show('<html><script src="webui.js"></script> Hello! </html>');

await WebUI.wait(); // keep the app alive
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### WebUI.waitAsync

A non-blocking alternative to [`WebUI.wait()`](#webuiwait) designed for WebView mode, where some work must run on the main thread. Returns `true` while windows are still open, `false` when all are closed.

```ts
while (WebUI.waitAsync()) {
  // perform per-frame or per-tick work here
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### WebUI.exit

Close all open windows. This causes [`WebUI.wait()`](#webuiwait) to return.

```ts
WebUI.exit();
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### WebUI.clean

Release all memory and resources used by WebUI. WebUI cannot be used after this call.

```ts
await WebUI.wait();
WebUI.clean();
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### WebUI.setTimeout

Set the maximum number of seconds to wait for the browser to start. The default is 30 seconds.

```ts
WebUI.setTimeout(10); // fail fast after 10 seconds
WebUI.setTimeout(0);  // wait forever
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### WebUI.setMultiClient

Allow multiple browser tabs or clients to connect to the same window simultaneously. When enabled, use [`scriptClient()`](#scriptclient) to target individual clients.

```ts
WebUI.setMultiClient(true);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### WebUI.setDefaultRootFolder

Set the root folder for **all** windows. Must be called before any [`show()`](#show). Returns `true` if the path is valid.

```ts
WebUI.setDefaultRootFolder('/home/myapp/dist');
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### WebUI.setBrowserFolder

Override the directory where WebUI looks for the browser executable.

```ts
WebUI.setBrowserFolder('/opt/my-chromium/');
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### WebUI.setFolderMonitor

Automatically reload all windows when any file in the root folder changes. Handy during development.

```ts
WebUI.setFolderMonitor(true);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### WebUI.setTLSCertificate

Enable HTTPS by providing a PEM certificate and private key. Must be called before [`show()`](#show). Throws if the certificate is invalid.

> Requires the WebUI TLS-enabled native library build.

```ts
const cert = await Bun.file('cert.pem').text();
const key  = await Bun.file('key.pem').text();
WebUI.setTLSCertificate(cert, key);

await myWindow.show('index.html'); // served over https://
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### WebUI.browserExist

Check whether a given browser is installed on the system.

```ts
if (WebUI.browserExist(WebUI.Browser.Chrome)) {
  console.log('Chrome is available.');
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### WebUI.isHighContrast

Check the OS-level high-contrast accessibility preference.

```ts
if (WebUI.isHighContrast()) {
  console.log('User prefers high contrast.');
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### WebUI.openUrl

Open a URL in the system's default web browser (outside of any WebUI window).

```ts
WebUI.openUrl('https://webui.me');
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### WebUI.getFreePort

Find an available TCP port that is not currently in use.

```ts
const port = WebUI.getFreePort();
console.log(`Using port ${port}`);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### WebUI.getNewWindowId

Get the next available window ID that can be passed to [`WebUI.newWindowId()`](#webuinewwindowid). Useful when you need to know the ID in advance before creating the window.

```ts
const id = WebUI.getNewWindowId();
WebUI.newWindowId(id);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### WebUI.newWindowId

Create a new window with a specific numeric ID. Use [`WebUI.getNewWindowId()`](#webuigetnewwindowid) to get a valid available ID first.

```ts
const id = WebUI.getNewWindowId();
WebUI.newWindowId(id);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### WebUI.getMimeType

Get the HTTP MIME type string for a given file name or extension. Useful when building custom file handlers.

```ts
const mime = WebUI.getMimeType('style.css');   // "text/css"
const mime2 = WebUI.getMimeType('image.png');  // "image/png"

myWindow.setFileHandler(async (url: URL) => {
  const file = await Bun.file(`.${url.pathname}`).bytes();
  const mime = WebUI.getMimeType(url.pathname);
  return `HTTP/1.1 200 OK\r\nContent-Type: ${mime}\r\nContent-Length: ${file.length}\r\n\r\n` +
    Buffer.from(file).toString('binary');
});
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### WebUI.setLogger

Register a custom callback to receive internal WebUI log messages. The callback receives a log level (see [`WebUI.LoggerLevel`](#webuiloggerlevel)) and the message string.

```ts
WebUI.setLogger((level: number, message: string) => {
  if (level === WebUI.LoggerLevel.Error) {
    console.error('[WebUI Error]', message);
  } else if (level === WebUI.LoggerLevel.Info) {
    console.info('[WebUI Info]', message);
  } else {
    console.debug('[WebUI Debug]', message);
  }
});
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### WebUI.getLastErrorNumber

Get the numeric error code of the last WebUI error. Returns `0` if no error has occurred.

```ts
const code = WebUI.getLastErrorNumber();
if (code !== 0) {
  console.error('WebUI error code:', code);
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### WebUI.getLastErrorMessage

Get the human-readable message for the last WebUI error.

```ts
try {
  await myWindow.show('index.html');
} catch {
  console.error(WebUI.getLastErrorMessage());
}
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### WebUI.deleteAllProfiles

Delete all browser profile folders created by WebUI across all windows.

```ts
WebUI.deleteAllProfiles();
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### WebUI.encode

Base64-encode a string. Use this to safely transfer text data to the browser.

```ts
const encoded = WebUI.encode('Hello, World!');
myWindow.run(`receiveData('${encoded}')`);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### WebUI.decode

Base64-decode a string received from the browser.

```ts
myWindow.bind('submit', (e: WebUI.Event) => {
  const raw = e.arg.string(0);
  const text = WebUI.decode(raw);
  console.log('Decoded:', text);
});
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### WebUI.malloc

Allocate a memory block managed by WebUI's memory system. WebUI will free this block automatically when it is no longer needed.

```ts
const ptr = WebUI.malloc(256);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### WebUI.free

Free a memory block previously allocated with [`WebUI.malloc()`](#webuimalloc).

```ts
const ptr = WebUI.malloc(256);
// ... use ptr ...
WebUI.free(ptr);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### WebUI.version

Get the current WebUI Bun wrapper version string.

```ts
console.log(WebUI.version); // e.g. "2.5.4"
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### WebUI.Browser

Enum of supported browsers that can be passed to [`showBrowser()`](#showbrowser) or [`WebUI.browserExist()`](#webuibrowserexist).

| Value | Browser |
|-------|---------|
| `NoBrowser` (0) | No browser — use for server-only mode |
| `AnyBrowser` (1) | Let WebUI pick the best available browser |
| `Chrome` (2) | Google Chrome |
| `Firefox` (3) | Mozilla Firefox |
| `Edge` (4) | Microsoft Edge |
| `Safari` (5) | Apple Safari |
| `Chromium` (6) | The Chromium Project |
| `Opera` (7) | Opera Browser |
| `Brave` (8) | Brave Browser |
| `Vivaldi` (9) | Vivaldi Browser |
| `Epic` (10) | Epic Browser |
| `Yandex` (11) | Yandex Browser |
| `ChromiumBased` (12) | Any installed Chromium-based browser |
| `Webview` (13) | Native WebView (not a browser) |

```ts
await myWindow.showBrowser('index.html', WebUI.Browser.Edge);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### WebUI.EventType

Enum of event types received in bind callbacks via `e.eventType`.

| Value | Description |
|-------|-------------|
| `Disconnected` (0) | The browser disconnected from the window |
| `Connected` (1) | A browser connected to the window |
| `MouseClick` (2) | A mouse click on a bound element |
| `Navigation` (3) | The browser is navigating to a new URL |
| `Callback` (4) | A JavaScript function call via `webui.call()` |

```ts
myWindow.bind('', (e: WebUI.Event) => {
  if (e.eventType === WebUI.EventType.Connected) {
    console.log('New browser tab connected.');
  }
});
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### WebUI.Runtime

Enum for selecting the JavaScript runtime used to run `.js` / `.ts` files served by the window (see [`setRuntime()`](#setruntime)).

| Value | Description |
|-------|-------------|
| `None` (0) | No runtime — serve files as static assets |
| `Deno` (1) | Execute with Deno |
| `NodeJS` (2) | Execute with Node.js |
| `Bun` (3) | Execute with Bun |

```ts
myWindow.setRuntime(WebUI.Runtime.Bun);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### WebUI.LoggerLevel

Enum of log severity levels passed to the [`WebUI.setLogger()`](#webuilogger) callback.

| Value | Description |
|-------|-------------|
| `Debug` (0) | Verbose diagnostic messages |
| `Info` (1) | General informational messages |
| `Error` (2) | Error conditions |

```ts
WebUI.setLogger((level: number, message: string) => {
  switch (level) {
    case WebUI.LoggerLevel.Debug: console.debug(message); break;
    case WebUI.LoggerLevel.Info:  console.info(message);  break;
    case WebUI.LoggerLevel.Error: console.error(message); break;
  }
});
```
