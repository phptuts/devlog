# Electron JS Simplified Courses

## Getting Started

1\. run `npm init -y`

2\. run `npm install electron`

3\. in your package js add start to your scripts

```json
{
  "name": "electron-series",
  "version": "1.0.0",
  "main": "index.js",
  "scripts": {
    "start": "electron .",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "description": "Tutorials for electron",
  "dependencies": {
    "electron": "^17.0.0"
  }
}

```

## Backend Getting Started

This will get the node js process of electron going.

Electron has node backend process called main and render processes that execute on the windows.

```js
// first require electron
const { app } = require('electron');

// When ready will return a promise once the electron app is initialize
app.whenReady().then(main);

// This function will run once whenReady is good.
async function main() {

}

```

## Useful functions

```js
   // you can require standard or any node package in your main process
   const os = require('os');


  // This returns true if the app is packed
  // This is for dev mode
  const isPacked = app.isPackaged;
  
  // Gets the common path
  const desktopPath = app.getPath('desktop');

  // gets the ISO character country code
  const countryCode = app.getLocaleCountryCode();

   // This wrapped in platform darwin which means mac only
   // The reason is that app.isInApplicationsFolder() is checking if
   // the electron app is in the application folder which
   // is a mac only thing
   if (os.platform() == 'darwin') {
    const isInAppFolder = app.isInApplicationsFolder();
    console.log(isInAppFolder);
  }
  
  // quits the app
  app.quit();


```

## Backend With Window

This will get the node js process of electron going.

Electron has node backend process called main and render processes that execute on the windows.

```js
// first require electron
const { app, BrowserWindow } = require('electron');
const os = require('os');
const path = require('path');

// When ready will return a promise once the electron app is initialize
app.whenReady().then(main);

// This function will run once whenReady is good.
async function main() {
  window = new BrowserWindow({
    width: 800, // px
    height: 600, // px
    resizable: true, // toggles resizes
    maximizable: false, // toggles maximizing the window
    autoHideMenuBar: true, // This will hide menu bar on windows only
  });
  // load the html
  window.loadFile(path.join(__dirname, '/index.html'));
  
  
  // allows to respond to window events
  window.on('resize', (e) => {
    console.log(window.getBounds());
  });
  window.on('move', (e) => {
    console.log(window.getBounds());
  });

}

```

## Preventing Flickering on Windows

This will create window once it's ready and will only show the window once the dom is completely ready to show.

```js
const { app, BrowserWindow } = require('electron');
const os = require('os');
const path = require('path');

app.whenReady().then(main);

let window;

async function main() {
  window = new BrowserWindow({
    icon: path.join(__dirname, 'icon.ico'),
    show: false, // have to set to invisible
  });
  window.loadFile(path.join(__dirname, '/index.html'));

  // prevents flickering on windows
  window.on('ready-to-show', window.show);
}
```

## Sending Messages and Functons from main process to browser

### index.js / node server

```js

const { app, BrowserWindow, ipcMain } = require('electron');
const os = require('os');
const path = require('path');
const { currentLoad, cpu } = require('systeminformation');
require('electron-reload')(__dirname);

app.whenReady().then(main);

let window;

async function main() {
  window = new BrowserWindow({
    icon: path.join(__dirname, 'icon.ico'),
    autoHideMenuBar: true, // This will hide menu bar
    show: false, // have to set to invisible
    webPreferences: {
      //   devTools: false,
      preload: path.join(__dirname, 'backend', 'preload.js'), // This load the preview script which is the glue for connecting browser to the node process.
    },
  });
  window.loadFile(path.join(__dirname, 'app', 'index.html'));
  window.webContents.openDevTools();
  // prevents flickering on windows
  window.on('ready-to-show', window.show);
}

// This how we respond to events being sent from the browser
ipcMain.handle('cpu/get', async (event, data) => {
  const usage = await currentLoad();

  return usage;
});
```

### Prelaod

This will both on the browser and node server.  

ipcRenderer is what triggers an event in the node server.  Notice in the index.js file we are listening for `cpu/get`. 

contextBridge attached an object to the window called app.

```js
const os = require('os');
const { ipcRenderer, contextBridge } = require('electron');


const API = {
  cpusUsage: () => ipcRenderer.invoke('cpu/get', 'Hello from renderer'),
};

contextBridge.exposeInMainWorld('app', API);
```

### Renderer.js

This is plain javascript file that has no access to anything running on the node server.  The only thing it will have access to is what is put into the API variable in the preload script.

```js
const CPU_USED_TXT = document.getElementById('used-cpu');
const INNER_BAR = document.getElementById('inner-bar');

async function getCpuUsage() {
  const usage = await app.cpusUsage();
  const usedOverall = usage.currentLoad;
  updatePercentage(usedOverall);
}

function updatePercentage(percent = 0) {
  INNER_BAR.style.width = `${percent.toFixed(1)}%`;
  CPU_USED_TXT.textContent = `${percent.toFixed(1)}%`;
}

setInterval(() => {
  getCpuUsage();
}, 2000);
```
