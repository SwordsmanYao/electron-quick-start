## electron 环境部署

### 概述

​	eletron是一种跨平台的构建桌面app的一种解决方案。你可以把它看做是node+Chromium 的一个载体或者集合体。本人在electron早期的版本0.33版本就开始运用到开发中，趟过不少坑。LayaFlash的第一款IDE就是用的electron开发，后来引擎升级于是集成了vscode，进行vscode的二次开发，开发出来现在的[LayaAirIDE](https://www.layabox.com/)。 庆幸的是electron越来越完善。关于它的更多的介绍读者可以去官网看相关的介绍。中文网推荐大家去[这里](https://electron.org.cn/).这里更新速度也很快。 本教程只是快速的带着开发者进入实战，创建一个自己的app。

### 环境搭建。

​	electron集成的node，所以在开发自己的app过程中难免会用到node的模块，或者其他的npm的工具。所以安装node也是必要的。首先到node的官网去下载nodejs，下载地址https://nodejs.org/en/。下载完成之后一路next安装就可以。下载安装完成之后打开命令行:

```shell
node -v 
```

假如打印出相应的版本号表明已经安装成功。

​	接下来我们要下载electron的底层运行环境。开发者可以去他指定官网地址下载，但是天朝的网络很容易被墙或者下载的事龟速。所以笔者建议开发者去淘宝的镜像下载。或者安装cnpm。这里笔者安装的事cnpm。通过cnpm下载安装。

​	安装cnpm 。首先打开命令行，全局进行安装cnpm输入命令行：

```shell
 npm install -g cnpm --registry=https://registry.npm.taobao.org
```

等待安装完成即可。cnpm安装完成之后我们就可以下载electron啦。命令行继续输入：

```shell
cnpm install -g electron
```

等待下载完成后 命令行输入

```
electron
```

假如出现如下图所示的画面，那么恭喜你，已经成功一半啦。![1](imgs\1.png)

为了开发方便我们把electron的运行环境单独复制出来。

上图中的`C:\Users\wzh\AppData\Roaming\npm\node_modules\electron\dist\`就是运行环境需要的所有文件。

这里我复制到e盘，修改文件夹的名字为`myapp`。

​	在E:\myapp\resources目录下新建一个app文件夹，在app目录下需要创建三个文件。

- package.json

  ```json
  {
    "name"    : "your-app",
    "version" : "0.1.0",
    "main"    : "main.js"
  }
  ```

- main.js

  ```javascript
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
    // win.webContents.openDevTools()

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
  setTimeout(function() {
    console.log("asadasd")
  }, 10000);
  // 在这文件，你可以续写应用剩下主进程代码。
  // 也可以拆分成几个文件，然后用 require 导入。
  ```

  ​

- index.html

  ```html
  <!DOCTYPE html>
  <html>
    <head>
      <title>Hello World!</title>
    </head>
    <body>
      <h1>Hello World!</h1>
      We are using io.js <script>document.write(process.version)</script>
      and Electron <script>document.write(process.versions['electron'])</script>.
    </body>
  </html>
  ```

  然后双击myapp文件夹下的electron.exe 即可。假如出现如下的画面 ，那么恭喜你，你的第一个app已经成功了。

  ![2](imgs\2.png)

### 关于调试

​	调试分为主进程的调试和渲染进程的调试。

- 主进程的调试

  主进程的调试我这里借助的事vscode，当然读者可以运用其他的调试方式。

  (1) 首先开发者到vscode的官网去下载，下载完成之后自行安装即可。

  (2)安装完成之后，用vscode打开我们的E:\myapp\resources这个文件夹。

  (3)然后创建一个.vscode的文件夹，新建一个launch.json的文件

  ```json
  {
    "version": "0.2.0",
    "configurations": [
      {
        "name": "Debug Main Process",
        "type": "node",
        "request": "launch",
        "cwd": "${workspaceRoot}/app",
        "runtimeExecutable": "E:\\myapp\\electron",
        "args" : ["."]
      }
    ]
  }
  ```

  ![3](imgs\3.png)

(4)创建完成后，按F5进行调试。我们可以在main.js中进行断点。效果如下图。

![1](imgs\1.gif)

- 渲染进程的调试

  渲染进程的调试比较简单，只需要我们创建的窗口打开开发者工具即可。我们找到main.js这文件，添加一句这样的代码即可。

  ![4](imgs\4.png)

然后在f5调试，可以看到很熟悉的界面，对，就是谷歌调试，由此我们知道了，渲染进程我们之间在自带的谷歌调试器中进行即可。![2](imgs\2.gif)

### 结束

​	到此我们已经可以进行我们的app开发和调试了。假如有什么问题可以qq联系我。719184910.