# Electron

# asar 加密

> asar是一种加密方法，可以将项目中原本明文可见的代码进行加密打包处理。

在Electron中，打包的概念是很多的，而asar的打包就是最小的打包概念，这个包仅针对resource/app目录的封装。



正确的使用方法应该是搭配electron-packager添加--asar参数进行使用。



# ipcMain

`ipcMain`扩展了[ `EventEmitter` ](https://nodejs.org/api/events.html#events_emitter_on_eventname_listener)，它是一个Node类，它维护用于管理事件的自身内部结构(这是`on`和`once`定义的模块)。
因此，只要您使用适当的机制触发`ipcMain.on("test", ...)`和`ipcMain.handle("test", ...)`，就应该能够安全地使用它们:`send`/`sendSync`与`on`/`once`相对应，而`invoke`与`handle`/`handleOnce`相对应。





# ipcMain、ipcRenderer通信