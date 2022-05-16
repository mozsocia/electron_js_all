Context Isolation is a feature that ensures that both your preload scripts and Electron's internal logic run in a separate context to the website you load in a webContents. This is important for security purposes as it helps prevent the website from accessing Electron internals or the powerful APIs your preload script has access to.

This means that the `window` object that your preload script has access to is actually a different object than the website would have access to. For example, if you set `window.hello = 'wave'` in your preload script and context isolation is enabled `window.hello` will be undefined if the website tries to access it.

Every single application should have context isolation enabled and from Electron 12 it will be enabled by default.

more details [https://docs.w3cub.com/electron/tutorial/context-isolation]
### How do I enable it?
```js
const mainWindow = new BrowserWindow({
  webPreferences: {
    contextIsolation: true
  }
})
```


# Preload
- preload.js is a js file for renderer process,
- it is just like a js file for html but it has access to the **node and backend** but other js file of html has not access to that

To load preload.js
```js
const { BrowserWindow } = require('electron')

const win = new BrowserWindow({
  webPreferences: {
    preload: 'path/to/preload.js'
  }
})

```
- To access from html js to preload.js
```js
// Preload (Isolated World)
const { contextBridge, ipcRenderer } = require('electron')

contextBridge.exposeInMainWorld(
  'electron',
  {
    doThing: () => ipcRenderer.send('do-a-thing')
  }
)
```
```js
// Renderer (Main World)

window.electron.doThing()
```
