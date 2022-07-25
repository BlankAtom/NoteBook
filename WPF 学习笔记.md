WPF 学习笔记



1、单实例（只开启一个窗口）

```c#
protected override void OnStartup(StartupEventArgs e){
   base.OnStartup(e); 
	Mutex mutex = new Mutex(true, "App Name", out bool isNewInstance)

	if( isNewInstance!=true){
//        MessageBox.Show("程序已启动");
        IntPtr intPtr = FindWindowW(null, "App Name");
        if(intPtr!= IntPtr.Zero){
            SetForegroundWindow(intPtr);
        }
        Shutdown();
    }
}

// HWND FindWindowW(
//	LPCWSTR lpClassName,
//	LPCWSTR lpWindowName
//)
//查找已开启的窗口
[DllImport("User32", Charset = CharSet.Unicode)]
static extern IntPtr FindWindowW(String lpClassName, String lpWindowName);

// BOOL SetForegroundWindow(
//	HWND hWnd
//)
//使已开启的窗口转到前台
[DllImport("User32", Charset = CharSet.Unicode)]
static extern Boolean SetForegroundWindow(IntPtr hWnd);
```

