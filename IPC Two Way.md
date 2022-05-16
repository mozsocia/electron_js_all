### Two way IPC 

```js
// Renderer process
ipcRenderer.invoke('some-name', someArgument).then((result) => {
  // ...
})

// Main process
ipcMain.handle('some-name', async (event, someArgument) => {
  const result = await doSomeWork(someArgument)
  return result
})

```


### Example :
renderer.js
```js
btn.addEventListener('click', async () => {
  const data = await window.electronAPI.sendMessage()
  textElement.innerText = data
})
```

preload.js
```js
const { contextBridge, ipcRenderer } = require('electron')

contextBridge.exposeInMainWorld('electronAPI', {
    sendMessage: () => ipcRenderer.invoke('event:sendMsg').then((result) => {
        // ...
    })
})

```

main.js
```js
ipcMain.handle('event:sendMsg', async (event, args) => {
  const result = await doSomeWork(args)
  return result
})

```
