# electron_js_all

### IPC from renderer to preload to main

renderer.js
```js

  const handleClick = () => {
    window.electronAPI.setTitleFn("this is from renderer ")
    console.log(window.electronAPI);
  }
  return (
    <div className="App">
      <h1>This is Electron React APP</h1>
      <button onClick={handleClick}>Click</button>
    </div>
  );
 ```
 
 preload.js
 ```js
 const { contextBridge, ipcRenderer } = require("electron");

contextBridge.exposeInMainWorld('electronAPI', {
    setTitleFn: (title) => ipcRenderer.send('set-title-fn', title)
})
```


main process: electron.js
```js
   ipcMain.on('set-title-fn', (event, args) => {
        console.log(args);

    })
```


 
