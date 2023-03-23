# dm.dll

基于[大漠插件](http://www.dmwebsite.net/)封装的 JS 版按键精灵！从此可以用 JS 写自动脚本了~

[文档地址](https://aweiu.com/documents/dm.dll/)

# 使用
建议使用 typescript 来调用本插件，插件内置了很多 有用类型，你可以获得更好的代码提示

如果你是想用它来写游戏脚本，强烈推荐配合 wow-state-machine 来使用。

# 推荐阅读 用 [JS 写游戏自动脚本是什么体验？](https://aweiu.com/%E7%94%A8%20JS%20%E5%86%99%E6%B8%B8%E6%88%8F%E8%87%AA%E5%8A%A8%E8%84%9A%E6%9C%AC%E6%98%AF%E4%BB%80%E4%B9%88%E4%BD%93%E9%AA%8C%EF%BC%9F/)

请务必使用管理员身份运行 node，因为大漠插件的注册和某些后台绑定模式需要管理员权限
dm.dll 可能会被杀毒软件误杀，这个暂时没啥好办法…

# 开发工具
大漠综合工具可能会被杀毒软件误杀，自行决定是否使用

大漠综合工具 + 按键精灵抓抓工具 + 大漠接口文档
# 环境
windows 平台
node版本要14.18.3 32位；（14.19.0rebuild都失败）
32位rebuild且32位的node来运行
32 位 node（[winax](https://github.com/durs/node-activex)最高支持的 node 版本）

预装 [windows-build-tools](https://github.com/felixrieseberg/windows-build-tools) 编译环境

// 管理员身份运行
npm install --global --production windows-build-tools --add-python-to-path
# 安装
npm install dm.dll
# API
本插件的 api 基本同[大漠说明文档](https://github.com/aweiu/dm.dll/blob/master/tools/%E5%A4%A7%E6%BC%A0%E6%8E%A5%E5%8F%A3%E8%AF%B4%E6%98%8E.CHM?raw=true)保持一致，稍有改动的部分也会在后文指出，所以你可以对照它来查看对应 api 的详细说明

目前只封装了大漠插件最常用的功能，API 太多，待整理…不过如果你需要的 api 本插件没有提供，你可以通过如下方式直接调用大漠插件的 api

// node
// const dm = require('dm.dll')
// typescript
import dm = require('dm.dll')
console.log(dm.dll.ver())
# 基本设置
getPath (): string
setPath (path: string): DmRet
setErrorDisplay (flag: ErrorDisplay): DmRet
同大漠插件的 SetShowErrorMsg

# 窗口
findWindow (className: string, title: string, parentHWnd?: number): number | undefined
增强了原生 findWindow 的功能，你可以直接传入一个父窗口句柄来查找子窗口句柄

enumWindow (className: string, title: string, filter: number, parentHWnd = 0): number[]
getWindow (hWnd: number, flag:GetWindowFlag): number
getPointWindow (x: number, y: number): number
getClientSize (hWnd: number): Size
moveWindow (hWnd: number, x: number, y: number): DmRet
setWindowSize (hWnd: number, width: number, height: number): DmRet
setWindowState (hWnd: number, state: WindowState): DmRet
sendPaste (hWnd: number): DmRet
sendString (hWnd: number, content: string): DmRet
# 后台
bindWindow (hWnd: number, display: DisplayType, mouse: MouseType, keypad: KeypadType,mode: 0 | 2 | 4): DmRet

unBindWindow (): DmRet

# 键鼠
setMouseRange (x1: number, y1: number, x2: number, y2: number): void
不同于 dm.dll.LockMouseRect 该方法只会限制 moveTo 的活动范围，不传参则取消限制

getCursorPos (): Coordinate
getKeyState (keyCode: number): KeyState
moveTo (x: number, y: number): DmRet
leftClick (): DmRet
leftDoubleClick (): DmRet
leftDown (): DmRet
leftUp (): DmRet
rightClick (): DmRet
rightDown (): DmRet
rightUp (): DmRet
wheelDown (): DmRet
wheelUp (): DmRet
keyPress (keyCode: number): DmRet
keyDown (keyCode: number): DmRet
keyUp (keyCode: number): DmRet
# 图色
capture (x1: number, y1: number, x2: number, y2: number, fileName: string)
findPic (x1: number, y1: number, x2: number, y2: number, picName: string, deltaColor: string, sim: number, dir: FindPicDir: FindRet | undefined
findPicEx (x1: number, y1: number, x2: number, y2: number, picName: string, deltaColor: string, sim: number, dir: FindPicDir: FindRet[]
getColor (x: number, y: number): string
getColorNum (x1: number, y1: number, x2: number, y2: number, color: string, sim: number): number
getAveRGB (x1: number, y1: number, x2: number, y2: number): string
findColor (x1: number, y1: number, x2: number, y2: number, color: string, sim: number, dir: FindDir: Coordinate | undefined
# 文字识别
getNowDict (): number
setDict (index: number, file: string): DmRet
findStr (x1: number, y1: number, x2: number, y2: number, string: string, colorFormat: string, sim: number): FindRet | undefined
ocr (x1: number, y1: number, x2: number, y2: number, colorFormat: string, sim: number): string
getWords (x1: number, y1: number, x2: number, y2: number, colorFormat: string, sim: number): OcrRet | undefined
# 系统
getScreenSize (): Size
获取屏幕大小，该函数合并了 GetScreenWidth 和 GetScreenHeight

