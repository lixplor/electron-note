# Hello world

本节创建一个Hello world项目

## 初始化项目

* 创建目录`helloworld`
* 进入目录, 并初始化: `npm init'
    - 初始化完毕后, 修改生成的`package.json`文件
    - 将`scripts`中添加`"start":"electron ."`, 表示使用`npm start`脚本时使用`electron`运行当前目录
* 编写代码(详细见下方)
* 安装electron到项目目录中: `npm i electron --save-dev`
* 运行项目: `npm start`

项目文件如下:

```shell
helloworld/
    - node_modules/
    - package.json
    - index.html
    - index.js
```

```html
// index.html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title>Hello world!</title>
    </head>
    <body>
        <h1>Hello world!</h1>
        当前使用node版本为<script>document.write(process.versions.node)</script>, Chrome版本为<script>document.write(process.versions.chrome)</script>, Electron版本为<script>document.write(process.versions.electron)</script>
    </body>
</html>
```

```javascript
// index.js
const {app, BrowserWindow} = require('electron')
const path = require('path')
const url = require('url')

// 保持一个对于 window 对象的全局引用，如果你不这样做，
// 当 JavaScript 对象被垃圾回收， window 会被自动地关闭
let win

function createWindow () {
  // 创建浏览器窗口。
  win = new BrowserWindow({width: 800, height: 600})

  // 加载应用的 index.html。
  win.loadURL(url.format({
    pathname: path.join(__dirname, 'index.html'),
    protocol: 'file:',
    slashes: true
  }))

  // 打开开发者工具。
  win.webContents.openDevTools()

  // 当 window 被关闭，这个事件会被触发。
  win.on('closed', () => {
    // 取消引用 window 对象，如果你的应用支持多窗口的话，
    // 通常会把多个 window 对象存放在一个数组里面，
    // 与此同时，你应该删除相应的元素。
    win = null
  })
}

// Electron 会在初始化后并准备
// 创建浏览器窗口时，调用这个函数。
// 部分 API 在 ready 事件触发后才能使用。
app.on('ready', createWindow)

// 当全部窗口关闭时退出。
app.on('window-all-closed', () => {
  // 在 macOS 上，除非用户用 Cmd + Q 确定地退出，
  // 否则绝大部分应用及其菜单栏会保持激活。
  if (process.platform !== 'darwin') {
    app.quit()
  }
})

app.on('activate', () => {
  // 在这文件，你可以续写应用剩下主进程代码。
  // 也可以拆分成几个文件，然后用 require 导入。
  if (win === null) {
    createWindow()
  }
})

// 在这文件，你可以续写应用剩下主进程代码。
// 也可以拆分成几个文件，然后用 require 导入。
```

运行程序:

```shell
npm start
```
