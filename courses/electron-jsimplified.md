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
