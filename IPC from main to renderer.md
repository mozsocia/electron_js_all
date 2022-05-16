### IPC from main to renderer is easy

```js
// main process

mainWindow.webContents.send('ping-hm', '3nd wow!')


// preload.js process

ipcRenderer.on('ping-hm', (event, args) => {
    console.log(args); // prints 3nd wow!
})
