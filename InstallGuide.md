# Notice before install #

ezgdi is somehow unstable and not recommend to install on important system, it can make your system unbootable.

# Install ezgdi #

There are no installer for ezgdi right now, only zip files.

1. Download latest ezgdi-rXX.zip files in project [download](http://code.google.com/p/ezgdi/downloads/list) page.

2. Extract the zip, put easyhook64.dll, EasyHook32.dll in one of the directory in PATH, such as c:\windows or c:\windows\system32.

Currently, your need to modify registry to load ezgdi.

3. To install ezgdi on x64 windows, use notepad to edit ezgdi-x64.reg, fix the path of ezgdi-x86.dll and ezgdi-x64.dll, then import the reg file.

Open new program after the reg file is imported. ezgdi should be effective immediately.

# Uninstall ezgdi #

To uninstall ezgdi on x64 windows,

1. import ezgdi-x64-uninstall.reg, the registry key will be removed.

2. Reboot Windows

3. After Windows is rebooted, you can remove ezgdi DLL files in your system path.