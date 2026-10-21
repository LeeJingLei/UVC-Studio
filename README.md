# UVC Studio

UVC Studio 是一款面向 Windows 的 USB 摄像头预览与控制工具。它可以查看摄像头实际输出的视频模式、调节常见 UVC 参数，并记录控制请求，适合摄像头调试和日常预览。

## 功能

- 枚举并切换摄像头，可手动刷新设备列表。
- 选择摄像头支持的分辨率、帧率和像素格式，并显示实际收到的视频帧格式与 FPS。
- 提供实时预览、适应窗口、铺满窗口和全屏显示。
- 调节标准 UVC Processing Unit 与 Camera Terminal 控件。可用项目由摄像头及其驱动决定。
- 将视频录制为 H.264/MP4，并将照片保存为 JPEG。
- 查看 UVC 控制请求日志，支持清空和导出 UTF-8 CSV。

## 运行要求

- Windows 10 或 Windows 11，x64。
- 支持 UVC 的 USB 摄像头及相应 Windows 驱动。
- 如果从源码构建：Qt 6.11.2 MSVC 2022 x64（含 Qt Multimedia 开发组件）、Visual Studio 2022 C++ 桌面开发工具和 Windows SDK、CMake、JOM、Ninja。

帧率修复插件依赖 Qt Multimedia 私有接口，应使用与项目匹配的 Qt 6.11.2 工具包构建。`third_party/qt-ffmpeg-camera/README.md` 说明了该插件的来源和修改内容。

## 从源码构建

1. 安装上面列出的 Qt 和 Visual Studio 组件。
2. 在 `build.ps1` 开头，将 `$QtRoot` 改为本机 Qt 安装根目录。该目录下应包含 `6.11.2/msvc2022_64` 和 Qt 的 CMake/JOM 工具。
3. 在 PowerShell 中运行：

   ```powershell
   .\build.cmd
   ```

   可执行文件会生成在 `build-release/UVC Studio.exe`。

如需生成包含 Qt 和运行库依赖的可运行目录，运行：

```powershell
.\build.cmd -Deploy
```

部署目录为 `package/`。构建和部署使用本机安装的 Qt、CMake、Visual Studio 工具，不需要把这些工具或构建产物提交到源码仓库。

也可以使用 Qt Creator 打开根目录中的 `CMakeLists.txt` 构建主程序。帧率修复插件仍需使用 `build.ps1` 构建流程，以确保它与主程序使用同一 Qt 版本。

## 生成安装程序

安装 Inno Setup 6 后，在项目根目录运行：

```powershell
.\build-installer.ps1
```

安装程序会生成在 `dist/`。`package/` 和 `dist/` 都是本地构建输出目录，不属于源码。

## 使用说明

- 选择摄像头和视频模式后，点击“开启相机”开始预览。刷新设备列表不会在后台持续扫描设备。
- 从“更多”菜单打开相机控制面板或 UVC 命令日志。摄像头和驱动支持的控制项目可能不同。
- 从“更多”菜单开始录制或拍照。视频默认保存到 Windows“视频/UVC Studio”目录，照片保存到“图片/UVC Studio”目录。
- 相机可能被其他程序占用；遇到无法开启时，先关闭正在使用该摄像头的应用。

## 第三方代码

`third_party/qt-ffmpeg-camera/` 包含基于 Qt Multimedia 6.11.2 的摄像头插件源码修改，用于按分辨率、像素格式和帧率匹配摄像头原生模式。该目录保留了 Qt 源码中的版权与 SPDX 许可声明；详细来源和改动见 [插件说明](third_party/qt-ffmpeg-camera/README.md)。
