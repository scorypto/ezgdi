# Introduction #

ezgdi aims to provide [gdi++](http://drwatson.nobody.jp/gdi++/index-en.html) alike advanced font rendering for 64-bit applications on Windows.

This project is originated from gdi++, which depends on [Detours](http://research.microsoft.com/en-us/projects/detours/). Detours is a great library, but it is not freely available for 64bit applications, which is the reason why gdi++ can't render 64-bit application on amd64 version of Windows 7/Vista.

Similar to Detours, EasyHook also provides Windows API hooking feature, which is essential to gdi++. And more importantly, EasyHook is an opensource project that supports 64-bit application. If we replace detours related codes in gdi++ source code with Easyhook equivalent ones, we can build a gdi++ DLL for x64 platform, which supports 64-bit applications.

However, ezgdi is not the only project providing gdi++ alike font rendering for Windows. Other programs like: [gdipp](http://code.google.com/p/gdipp), and [dwType](http://free.flop.jp/gdi++/src/gdi0903.zip) also support 64-bit applications. You can also check these projects.

ezgdi project is still in alpha test. It may cause application crash occasionally, or making system unbootable. Thus should only be used for testing purpose.

  * [Download and Test](http://code.google.com/p/ezgdi/downloads)
  * [More Information](DocumentIndex.md)

# 介绍 #

ezgdi项目目的是为64位应用提供类似gdi++的字体渲染功能.

本项目源自gdi++. gdi++中核心的操作系统API替换功能依赖于detours库. 由于detours库的64位版本不免费向公众提供, 导致gdi++不能渲染amd64版本Windows 7/vista下的64位应用. easyhook提供与detours类似的系统调用截获功能, 开源且支持amd64平台. 将gdi++对detours的依赖移植到easyhook上, 就可以编译出64位的gdi++动态连接库, 用于渲染64位程序.

ezgdi项目仍然处在alpha测试中, 可能会引起程序崩溃, 甚至系统无法启动. 请注意不要在重要系统上安装.

  * [下载并测试](http://code.google.com/p/ezgdi/downloads)
  * [更多信息](DocumentIndex.md)

# License #

This project is released under both FreeType license and GPLv3, according to [gnu website](http://www.gnu.org/philosophy/license-list.html), these two licenses are compatible.