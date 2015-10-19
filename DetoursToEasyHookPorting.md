本页面第一部分保存了2009/8/24-/2009/8/26期间的ezgdi移植过程记录。

---


# Introduction #

gdi++依赖于detours 2.1, 而后者的64bits版本不免费, 导致gdi++不能渲染x64版本的win7. easyhook提供与detours类似的功能, 并且开源, 支持x64平台. 所以理论上, 将gdi++对detours的依赖移植到easyhook上, 应该就可以编译出64位的gdi++.dll, 用于渲染64位win7程序。

# Details #

建议在英文界面的Windows虚拟机里试用下面dll, **强烈的不建议在自己的机器上试用, 否则可能永远登录不了Windows**

附件有两个zip文件, 分别是32和64位的easyhook版的gdi++.dll, 必须用注册表Appinit\_DLLs的方式加载, 先把easyhook32.dll/easyhook64.dl拷贝到c:\windows\system32.

(另外, Win7 x64的LoadAppinit\_DLLs默认是0, 也就是不加载, 请改成1, 不然没效果)

# 最新进展 #

  * 2009.8.24 感谢oveage指点, 已经拿到 [URL](URL.md)http://free.flop.jp/gdi++/src/gdi0850.zip[/URL] 的源码, 正在分析
  * 2009.8.25 更新: 移植的进度报告见下
  * 2009.8.26 更新: 移植到easyhook版成功编译, 生成32bit dll成功加载
  * 2009.8.26 更新: Win7 WinXp检验, 可以渲染英文, 不能渲染中文
  * 2009.8.26 更新: 上传32bit easyhook版gdi++的zip包
  * 2009.8.26 更新: 上传64bit easyhook版gdi++的zip包

# 移植计划 #

  1. 先自行编译32位版freetype版的gdi++的dll (2009.8.25已完成)
  1. 学习detours 32的原理, 写几个简单的例子 取消
  1. 分析gdi++怎么使用的detours (2009.8.25已完成)
  1. 学习easyhook 64的原理, 写几个简单的例子 (2009.8.25已完成)
  1. 分析gdi++使用easyhook 64的可行性 (2009.8.25已完成)
  1. 尝试修改gdi++的源码, 移植到easyhook上, 并生成32位dll, 看是否能够正确运行 (2009.8.26完成, 有bug)
  1. 生成64的gdi++.dll, 看是否能够正确运行 (2009.8.26完成, 需要改进)

# gdi++对detours的依赖分析 #

  1. 真正使用到Detours的源文件只有两个: hook.cpp 编译进gdi++.dll, run.cpp编译进 gdi++.exe
  1. gdi++.dll中需要替换的函数只有两个: hook\_init, hook\_term
  1. 使用到的Detours函数只有6个:DetourRestoreAfterWith, DetourUpdateThread, DetourTransactionBegin/Commit, DetourAttach/Detach
  1. 被截获的系统调用 8对(多字节/Unicode系统调用)+5个其他, 也就是21个Win32系统调用
> 实现注册表方式加载, 应该只需要修改hook.cpp, 其他方式加载需要修改类似gditray的源码.

# easyhook总结和gdi++使用easyhook 64可行性分析 #

  1. easyhook的文档太少了, 几乎所有的资料都要看代码. 而且easyhook目前大部分例子都是.NET的例子.
  1. 修改了easyhook的unmanaged hook的例子, 在32/64位下编译成功, 并成功截获系统调用MessageBeep, 说明easyhook确实可以截获64位程序的系统调用。
  1. 分析了easyhook和detours提供函数的对应关系, 除了DetourRestoreAfterWith不确信对应哪个函数, 其他函数都找到了与之对应的版本。
    * a. LhInstallHook + LhSetInclusiveACL 对应 DetourAttach
    * b. LhSeGlobalExclusiveACL + LhSeGlobalInclusiveACL 对应 DetourTransactionBegin + DetourUpdateThread + DetourTransactionCommit
    * c. LhUninstallHook 对应 DetourDetach
  1. easyhook替换的系统调用的时候, 不能用函数指针, 必须用原函数的地址, 否则会出现很多稀奇古怪的错误

# 用easyhook替换detours的奋斗过程 #

  1. 已修正：已经按照上述分析用easyhook替换对detours的调用, 编译通过, 但运行出错
  1. 目前已确认easyhook支持注册表加载Appinit\_DLLs方式加载
  1. 之前分析的easyhook用法有误, 已更新
  1. 成功编译依赖easyhook的32bits版的gdi++.dll, 用注册表方式成功加载, 可以正确渲染, 有些软件中文无渲染效果
  1. easyhook的\*运行效率\*好像比detours差, 不知道是不是心理作用
  1. 编译生成64位的 freetype.lib
  1. 在64位下编译gdi++.dll, 简单修改了下gdi++其他文件中的源码
    * 去掉了cache.cpp的32位内嵌汇编
    * expfunc里面的InjectDll相关函数注释
    * 注释 optimize/optimize.h 和 memcpy\_amd.cpp 中的32位汇编, 先暂时改用非优化的原版 memcpy/memset/memzero
  1. 上述改动完成后, 生成64位的gdi++.dll, 可以用注册表方式加载. Win7下可成功渲染64位程序. 但中文字体无法渲染, 打开themex.net会导致IE崩溃