<div align="center">

![Logo](data/webui_120.svg)

# WebUI v2.5 - JavaScript Frontend APIs

> These APIs are available in your HTML frontend via the virtual `webui.js` file.

</div>


## Available APIs

- [JavaScript - call](#javascript---call)
- [JavaScript - isConnected](#javascript---isconnected)
- [JavaScript - setEventCallback](#javascript---seteventcallback)
- [JavaScript - encode](#javascript---encode)
- [JavaScript - decode](#javascript---decode)
- [JavaScript - isHighContrast](#javascript---ishighcontrast)
- [JavaScript - setLogging](#javascript---setlogging)
- [JavaScript - allowNavigation](#javascript---allownavigation)


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
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


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
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


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
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


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### JavaScript - encode

Encode text into base64 string.

> You need to include the virtual file `webui.js` in your HTML

```js
const base64 = webui.encode(str);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### JavaScript - decode

Decode base64 string into text.

```js
const str = webui.encode(base64);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### JavaScript - isHighContrast

Get OS high contrast preference.

```js
const hc = webui.isHighContrast();
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### JavaScript - setLogging

Enable WebUI debug logging in the console logs.

```js
webui.setLogging(true);
```


<!-- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -->
---
### JavaScript - allowNavigation

When binding all events on the backend, WebUI blocks all navigation events and sends them to the backend. This API allows you to control that behavior.

```js
webui.allowNavigation(true); // Allow navigation
window.location.replace('https://test.com'); // This will now proceed as usual
```
