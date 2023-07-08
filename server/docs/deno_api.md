# WebUI v2.3.0 Deno APIs

- [Import](/deno_api?id=import)
- [Examples](/deno_api?id=examples)
- Window
  - [New Window](/deno_api?id=new-window)
  - [Show Window](/deno_api?id=show-window)
  - [Window status](/deno_api?id=window-status)
- Binding & Events
  - [Bind](/deno_api?id=bind)
  - [Events](/deno_api?id=events)
- Application
  - [Wait](/deno_api?id=wait)
  - [Exit](/deno_api?id=exit)
  - [Close](/deno_api?id=close)
  - [Multi Access](/deno_api?id=multi-access)
- JavaScript
  - [Run Browser code From Deno](/deno_api?id=run-client-side-callback-from-backend)
  - [Run Deno From Browser](/deno_api?id=run-backend-callback-from-client-side)

---

### Import

No install needed, only import module from his url.

```ts
import { WebUI } from "https://deno.land/x/webui/mod.ts";
```

You can also import module directely from GitHub.

```ts
import { WebUI } from "https://raw.githubusercontent.com/webui-dev/deno-webui/main/mod.ts";
```

To run the module, the following deno flags are required:

| flag                        | purpose                                                               |
| :-------------------------- | :-------------------------------------------------------------------- |
| --allow-read                | Optional, stands for loading local ressources served by your UI       |
| --allow-read, --allow-write | Used to uncompress cached dynamic library in your user temp directory |
| --allow-ffi                 | Used to load all use of FFI                                           |
| --unstable                  | Used to allow FFI callbacks                                           |

Examples, documentation and sources can be directly seen from the Deno module
page at https://deno.land/x/webui.

Dynamic library can be found compiled in the project repository
([webui-dev/deno-webui](https://github.com/webui-dev/deno-webui/)) or with the
source at the C repository
([webui-dev/webui](https://github.com/webui-dev/webui/)).

To report an issue, browse code or contribute, please visit
[Deno WebUI](https://github.com/webui-dev/deno-webui/) in our GitHub repository.

---

### Examples

A minimal Deno example.

```ts
import { WebUI } from "https://deno.land/x/webui/mod.ts";

const myWindow = new WebUI(); //Init and load lib
webui.show("<html>Hello World</html>"); //Displays and update the window
WebUI.wait(); //Prevent the main trhead from exiting until all windows are closed
```

Using a local HTML file. Please note that you need to add
`<script src="/webui.js"></script>` to all your HTML files to load the js
bridge.

```ts
import { WebUI } from "https://deno.land/x/webui/mod.ts";

const myWindow = new WebUI();
myWindow.show("my_file.html");
WebUI.wait();
```

Using a specific web browser.

```ts
import { WebUI } from "https://deno.land/x/webui/mod.ts";

const myWindow = new WebUI();
myWindow.showBrowse("my_file.html", WebUI.Firefox);
WebUI.Wait();
```

Live example.

```sh
deno run -A --unstable https://deno.land/x/webui/examples/file_explorer/main.ts
```

Please visit
[Deno Examples](https://github.com/webui-dev/deno-webui/tree/main/examples) in
our GitHub repository for more complete examples.

---

### New Window

To create a new window object, you need to instanciate a `new WebUI()`. It will
load the dynamic library and return you an webui window.

```ts
//Use the default precached lib
const webui1 = new WebUI();
// Use a local use (for specific use)
const webui2 = new WebUi({ libPath: "./local_webui_2.dll" });
```

About dynamic library import strategy, mutli-os builds will be dowload and
cached at the first code execution or by the lsp of this module. Files will be
persisted on deno cache accross all new usage of the same module version. No
network is needed after this first step.

---

### Show Window

To show a window, you can use `show()` method. If the window is already shown,
the UI will get refreshed in the same window.

```ts
// Show a window using the embedded HTML
myWindow.show("<html>Hello!</html>");
```

```ts
// Show a window using an .html local file
// Please add <script src="/webui.js"></script> to your HTML files
myWindow.show("my_file.html");
```

Show a window using a specific web browser.

```ts
const myHtml = "<html>Hello!</html>";

// Google Chrome
myWindow.showBrowser(myHtml, WebUI.Chrome);

// Mozilla Firefox
myWindow.showBrowser(myHtml, WebUI.Firefox);

// Microsoft Edge
myWindow.showBrowser(myHtml, WebUI.Edge);

// Apple Safari (Not Ready)
myWindow.showBrowser(myHtml, WebUI.Safari);

// Default recommended web browser
myWindow.showBrowser(myHtml, WebUI.AnyBrowser);
```

Check supported web browsers at
[webui-dev/webui/#supported-web-browsers](https://github.com/webui-dev/webui/#supported-web-browsers).

If you need to update the whole UI content, you can also use the same function
`show()`, which allows you to refresh the window UI with any new HTML content.

```ts
const html = "<html>Hello</html>";
const newHtml = "<html>New World!</html>";

// Open a window
myWindow.show(html);

// Later...

// Refresh the same window with the new content
myWindow.show(newHtml);
```

---

### Window Status

Checks if the window is currently running by using the getter `isShown`.

```ts
const webui1 = new WebUI();
const webui2 = new WebUI();
webui1.show(`<html><p>View 1</p></html>`);

webui1.isShown; //true
webui2.isShown; //false
```

---

### Bind

Use `bind()` to receive a DOM event or defined a hook for client js app.

```ts
const webui = new WebUI();
webui.show(
  `<html>
        <button id="btn"></button>
            <script>
             const response = await webui_fn('myLabel', 'payload'); //global function injected by webui loader
        </script>
    </html>`,
);

//Bind to and element
webui.bind("btn", ({ element }) => console.log(`${element} was clicked`));
//Bind to a hook
webui.bind("myLabel", ({ data }) => {
  console.log(`ui send "${data}"`);
  return "backend response";
});
//Bind all DOM element (not all event are handled by the webui lib)
webui.bind(
  "",
  (event) => console.log(`new ui event was fired (${JSON.stringify(event)})`),
);
```

### Events

```ts
function handleEvent(event: WebUI.Event) {
  console.log(`new ui event was fired (${JSON.stringify(event)})`);
}

webui.bind("", handleEvent);
```

Detail of the Event interface:

```ts
interface WebUiEvent {
  window: WebUi; //WebUI instance that fire the event
  eventType: number; //Event type id
  element: string; //Id of the element (or hook name) that fire the event
  data: string; //Payload send by the hook
}
```

---

### Wait

It is essential to call static method `WebUI.wait()` at the end of your main
module, after you create/shows all your windows. This will make your application
run until the user closes all visible windows or when calling
_[WebUI.exit()](/deno_api?id=exit)_.

```ts
// Create windows...
// Bind HTML elements...
// Show the windows...

// Wait until all windows get closed
// or when calling WebUI.exit()
WebUI.wait(); //return a promise, ne need to await if there is no code below
```

```ts
// ...
await WebUI.wait();
console.log("All windows are now closed");
//...
```

---

### Exit

At any moment, you can call static method `WebUI.exit()`, which tries to close
all related opened windows and make _[WebUI.wait](/deno_api?id=wait)_ break.

```ts
WebUI.exit();
```

---

### Close

You can call `close()` to close a specific window, if there is no running window
left _[WebUI.wait](/deno_api?id=wait)_ will break.

```ts
myWindow.close();
```

---

### Multi Access

![access_denied](data/webui_access_denied.png)

After the window is loaded, the URL is not valid anymore for safety. WebUI will
show an error if someone else tries to access the URL. To allow multi-user
access to the same URL, you can use `setMultiAccess()`.

```ts
myWindow.setMultiAccess(true);
```

---

### Run client side callback from backend

You can run JavaScript on any window to read values, update the view, or
anything else. In addition, you can check if the script execution has errors, as
well as receive data.

```ts
const webui = new WebUI();
webui.show(
  `<html>
        <p id="text"></p>
        <script>
            function updateText(text) {
                document.getElementById('text').innerText = text;
                return 'ok';
            }
        </script>
    </html>`,
);

const response = await webui.script('updateText("backend action")').catch(
  console.error,
);
//response == "ok"
```

---

### Run backend callback from client side

To call a Deno function from JavaScript and get the result back please use
`const response = await webui_fn('hook', 'payload');` defined in global scope.
If the function does.

```ts
myWindow.bind("myLabel", ({ data }) => {
  console.log(`ui send "${data}"`);
  return "backend response";
});
```

JavsScript:

```js
const response = await webui_fn("myLabel", "payload data");
```
