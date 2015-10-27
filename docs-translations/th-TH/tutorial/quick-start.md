# เริ่มต้นแบบรวดเร็ว 

Electron (อิเล็กทรอน) ช่วยให้คุณสร้างแอปพลิเคชันสำหรับเดสท็อปด้วยจาวาสคริปต์ (JavaScript) ด้ วยการจัดเตรียมรันไทม์พร้อมกับ API ที่เหมาะสมกับระบบปฎิบัติการ (Operating System) เห็นได้จากการเปลี่ยนแปลงรันไทม์ของ Node.js ที่จับจ้องไปยังแอปพลิเคชันบนเดสท็อปแทนที่จะเป็นเว็บไซต์

นั่นไม่ได้หมายความว่า Electron คือไลบรารี่สำหรับจาวาสคริปต์ที่ผูกกับส่วนต่อประสานกราฟฟิกกับผู้ใช้ (Graphical user interface - GUI) แต่จริงๆ แล้ว Electron ใช้หน้าเพจเป็นส่วนต่อประสานกราฟฟิกกับผู้ใช้ ซึ่งคุณจะมองว่าเป็นเบราว์เซอร์ Chromium ขนาดเล็กก็ได้ โดยทั้งหมดจะถูกจัดกาด้วยจาวาสคริปต์

### กระบวนการหลัก

ใน Electron กระบวนการ (Process) ที่ระบุไว้ใน `main` ของ `package.json` นั้นจะเรียกว่า __กระบวนการหลัก (the main process)__ โดยที่สคริปต์ที่ทำงานในกระบวนการหลักนั้นสามารถแสดงส่วนต่อประสานกราฟฟิกกับผู้ใช้ด้วยการสร้างหน้าเพจขึ้นมา

### กระบวนการวาดหน้าจอ (Renderer Process)

จากที่ Electron ใช้ Chromium เพื่อแสดงผลหน้าเพจ ซึ่งสถาปัตยกรรมแบบหลายขั้นตอน (Multi-process architecture) ก็ถูกใช้ด้วย โดยแต่ละเพจของ Electron นั้นจะทำงานด้วยกระบวนการของตัวเอง ซึ่งเรียกว่า __กระบวนการวาดหน้าจอ (the renderer process)__

เบราว์เซอร์ทั่วไป หน้าเพจมักจะทำงานอยู่ภายในสภาพแวดล้อมบนกระบะทราย (sandbox) 
โดยปรกติของเบราว์เซอร์ทั่วไป หน้าเพจจะทำงานอยู่ภายในสภาพแวดล้อมกระบะทราย (sandbox) และไม่ได้รับอนุญาตให้เข้าถึงทรัพยากรของระบบได้ อย่างไรก็ดีผู้ใช้งาน Electron นั้นสามารถมีปฏิสัมพันธ์กับทรัพยากรระดับล่างของระบบปฏิบัติการผ่านหน้าเว็บเพจด้วย API ของ Node.js ได้

### ความแตกต่างระหว่างกระบวนการหลัก และกระบวนการวาดหน้าจอ

กระบวนการหลักสร้างหน้าเพจด้วยการสร้างอินสเตนซ์ (Instances) ของ `BrowserWindow` ซึ่งแต่ละอินของ `BrowserWindows` จะสร้างหน้าเพจในกระบวนการที่แยกออกไปเป็นของตัวเอง เมื่ออินสเตนซ์ของ `BrowserWindow` ถูกทำลายลง กระบวนการที่ใช้วาดหน้าเพจนั้นก็จะสิ้นสุดลงเช่นเดียวกัน

กระบวนการหลักจะบริหารจัดการหน้าเพจอื่นๆ และกระบวนการวาดหน้าเพจที่เกี่ยวข้อง โดยแต่ละกระบวนการวาดหน้าเพจนั้นจะถูกแยกออกไม่เกี่ยวข้องซึ่งกันและกัน รวมถึงรับผิดชอบเพียงหน้าเพจเดียวที่กำลังใจงานกระบวนการนี้เท่านั้น

สำหรับหน้าเว็บ จะถูกปิดกันการเรียกใช้งานเอพีไอที่เกี่ยวข้องกับส่วนต่อประสานกราฟฟิกกับผู้ใช้หลัก (native GUI) นั่นเพราะการจัดการส่วนต่อประสานกราฟฟิกกับผู้ใช้หลักผ่านหน้าเว็บนั้นเป็นเรื่องที่อันตรายเป็นอย่างมาก และมีความเสี่ยงที่ทรัพยากรบางอย่างจะหลุดรอดออกไป หากคุณต้องการเรียกใช้งานส่วนต่อประสานกราฟฟิกกับผู้ใช้หลักในหน้าเว็บ กระบวนการวาดหน้าจอของหน้าเพจจะต้องติดต่อกับกระบวนการหลักเพื่อขอให้กระบวนการหลักเป็นผู้ดำเนินการตามที่ต้องร้องขอ

ใน Electron เราได้เตรียมส่วนต่อขยาย [ipc](../api/ipc-renderer.md) สำหรับติดต่อระหว่างกระบวนการหลักและกระบวนการวาดหน้าจอ ซึ่งก็มีส่วนต่อขยาย [remote](../api/remote.md) สำหรับรูปแบบการเชื่อมต่อแบบ RPC

## Write your First Electron App

Generally, an Electron app is structured like this:

```text
your-app/
├── package.json
├── main.js
└── index.html
```

The format of `package.json` is exactly the same as that of Node's modules, and
the script specified by the `main` field is the startup script of your app,
which will run the main process. An example of your `package.json` might look
like this:

```json
{
  "name"    : "your-app",
  "version" : "0.1.0",
  "main"    : "main.js"
}
```

__Note__: If the `main` field is not present in `package.json`, Electron will
attempt to load an `index.js`.

The `main.js` should create windows and handle system events, a typical
example being:

```javascript
var app = require('app');  // Module to control application life.
var BrowserWindow = require('browser-window');  // Module to create native browser window.

// Report crashes to our server.
require('crash-reporter').start();

// Keep a global reference of the window object, if you don't, the window will
// be closed automatically when the JavaScript object is garbage collected.
var mainWindow = null;

// Quit when all windows are closed.
app.on('window-all-closed', function() {
  // On OS X it is common for applications and their menu bar
  // to stay active until the user quits explicitly with Cmd + Q
  if (process.platform != 'darwin') {
    app.quit();
  }
});

// This method will be called when Electron has finished
// initialization and is ready to create browser windows.
app.on('ready', function() {
  // Create the browser window.
  mainWindow = new BrowserWindow({width: 800, height: 600});

  // and load the index.html of the app.
  mainWindow.loadUrl('file://' + __dirname + '/index.html');

  // Open the DevTools.
  mainWindow.webContents.openDevTools();

  // Emitted when the window is closed.
  mainWindow.on('closed', function() {
    // Dereference the window object, usually you would store windows
    // in an array if your app supports multi windows, this is the time
    // when you should delete the corresponding element.
    mainWindow = null;
  });
});
```

Finally the `index.html` is the web page you want to show:

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <title>Hello World!</title>
  </head>
  <body>
    <h1>Hello World!</h1>
    We are using node <script>document.write(process.versions.node)</script>,
    Chrome <script>document.write(process.versions.chrome)</script>,
    and Electron <script>document.write(process.versions.electron)</script>.
  </body>
</html>
```

## Run your app

Once you've created your initial `main.js`, `index.html`, and `package.json` files,
you'll probably want to try running your app locally to test it and make sure it's
working as expected.

### electron-prebuilt

If you've installed `electron-prebuilt` globally with `npm`, then you will only need
to run the following in your app's source directory:

```bash
electron .
```

If you've installed it locally, then run:

```bash
./node_modules/.bin/electron .
```

### Manually Downloaded Electron Binary

If you downloaded Electron manually, you can also use the included
binary to execute your app directly.

#### Windows

```bash
$ .\electron\electron.exe your-app\
```

#### Linux

```bash
$ ./electron/electron your-app/
```

#### OS X

```bash
$ ./Electron.app/Contents/MacOS/Electron your-app/
```

`Electron.app` here is part of the Electron's release package, you can download
it from [here](https://github.com/atom/electron/releases).

### Run as a distribution

After you're done writing your app, you can create a distribution by
following the [Application Distribution](./application-distribution.md) guide
and then executing the packaged app.

### Try this Example

Clone and run the code in this tutorial by using the [`atom/electron-quick-start`](https://github.com/atom/electron-quick-start)
repository.

**Note**: Running this requires [Git](https://git-scm.com) and [Node.js](https://nodejs.org/en/download/) (which includes [npm](https://npmjs.org)) on your system.

```bash
# Clone the repository
$ git clone https://github.com/atom/electron-quick-start
# Go into the repository
$ cd electron-quick-start
# Install dependencies and run the app
$ npm install && npm start
```
