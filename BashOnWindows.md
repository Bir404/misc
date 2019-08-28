# Bash on Windows

At first, official place for bugs of ‘Windows Subsystem for Linux’ is: https://github.com/Microsoft/WSL/issues/.

**WSL** or ‘Windows Subsystem for Linux’ or ‘Bash on Ubuntu on Windows’ was brought to you by efforts of Microsoft and Canonical. This subsystem allows users to run **native** linux binaries in Windows 10 without using of virtual machines or recompilations.

**Required** 64-bit version of Windows 10 Anniversary Update build 14316 or later!

![Bash on Windows in ConEmu](https://conemu.github.io/img/BashOnWindows.png)

- Installation
  - [Good places to start](https://conemu.github.io/en/BashOnWindows.html#start)
  - [TLDR: WSL Installation](https://conemu.github.io/en/BashOnWindows.html#TLDR)
- [Some techinfo](https://conemu.github.io/en/BashOnWindows.html#techinfo)
- Preferred way to run WSL
  - [Change startup shell in WSL](https://conemu.github.io/en/BashOnWindows.html#wsl-shell)
  - [Change drives mount point in WSL](https://conemu.github.io/en/BashOnWindows.html#wsl-mnt)
  - [Start WSL in Unix home directory](https://conemu.github.io/en/BashOnWindows.html#wsl-home)
  - [Support different WSL distributions](https://conemu.github.io/en/BashOnWindows.html#wsl-distro)
- Get arrows working in ConEmu
  - [Solution 1: Default task {bash}](https://conemu.github.io/en/BashOnWindows.html#arrows-sol-1)
  - [Solution 2: StatusBar’s Terminal modes](https://conemu.github.io/en/BashOnWindows.html#arrows-sol-2)
- WSLBridge: Get 24-bit colors working in ConEmu
  - [TLDR: Just run wslbridge](https://conemu.github.io/en/BashOnWindows.html#TLDR2)
  - [How to get 24-bit colors working](https://conemu.github.io/en/BashOnWindows.html#true-color-example)
  - [WSLBridge manual installation and Task contents](https://conemu.github.io/en/BashOnWindows.html#wslbridge-task)
  - [Other versions of WSLBridge](https://conemu.github.io/en/BashOnWindows.html#wslbridge-32)

### Good places to start

- https://msdn.microsoft.com/commandline/wsl/install_guide
- https://msdn.microsoft.com/commandline/wsl/about

### TLDR: WSL Installation

- ‘Settings’ -> ‘Update and Security’ -> ‘For developers’: Enable ‘Developer mode’
- Reboot
- ‘Administrator’s command prompt’ execute the following:



```
powershell -command Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
```

- After another reboot, run in the ‘command prompt’ to install required files:



```
%windir%\system32\bash.exe
```

## Some techinfo

Despite the fact WSL binaries runs in Windows console window, they are not native Windows console applications (obviously) and they are not using native [Windows Console API](https://msdn.microsoft.com/en-us/library/windows/desktop/ms681913.aspx).

When you run `%windir%\system32\bash.exe` this native Windows process starts ‘linux kernel’ outside of Windows console, and linux applications communicate with [conhost](https://conemu.github.io/en/ConsoleApplication.html#conhost) without use of Windows Console API.

That means ConEmu can’t ‘[hook](https://conemu.github.io/en/ConEmuHk.html)’ linux processes! Unfortunately `bash.exe` which may be hooked is only a sort of a loader for WSL, `bash.exe` does not do console output and all [ANSI sequences](https://conemu.github.io/en/AnsiEscapeCodes.html) are processed **before** ConEmu can see them. WSL process all ANSI and writes stripped output directly to [conhost](https://conemu.github.io/en/ConsoleApplication.html#conhost).

Another problem is that due to mistake in WSL design, keypresses written to [conhost](https://conemu.github.io/en/ConsoleApplication.html#conhost) input buffer using **standard** Windows API function [WriteConsoleInput](https://msdn.microsoft.com/en-us/library/windows/desktop/ms687403.aspx) are not converted into xterm keyboard sequences. But when user presses same key directly in [RealConsole](https://conemu.github.io/en/RealConsole.html) they are converted properly.

Both problem have workarounds, read further.

## Preferred way to run WSL

Ryan Prichard has created [wslbridge](https://github.com/rprichard/wslbridge) which allows anyone to run WSL in any POSIX enabled terminal like mintty or [ConEmu cygwin/msys connector](https://conemu.github.io/en/CygwinMsysConnector.html).

**Note** If you don’t use connector/wslbridge you may observe bugs with Bash.

The required files of wslbridge and connector are shipped with ConEmu since build [170730](https://conemu.github.io/blog/2017/07/30/Build-170730.html). Just download and install latest [Preview or Alpha](https://conemu.github.io/en/VersionComparison.html) version and be sure that your [Tasks](https://conemu.github.io/en/Tasks.html) are [updated](https://conemu.github.io/en/DefaultTasks.html#add-default-tasks).

You `{Bash::bash}` task command shall be something like:



```
set "PATH=%ConEmuBaseDirShort%\wsl;%PATH%" & %ConEmuBaseDirShort%\conemu-cyg-64.exe --wsl -cur_console:pm:/mnt
```

And its task parameters:



```
/dir %CD% /icon "%USERPROFILE%\AppData\Local\lxss\bash.ico"
```

### Change startup shell in WSL

ConEmu starts WSL via [wslbridge](https://github.com/rprichard/wslbridge) to be able render ANSI internally. That means if you type additional arguments after `--wsl` this line (with the exception of [-cur_console](https://conemu.github.io/en/NewConsole.html)) is passed to wslbridge intact. So the `-t` switch of wslbridge is required.

If you want to start your own shell, for example `fish -l`, append the `-t fish -l` at the end of default `{Bash::bash}` task command.



```
set "PATH=%ConEmuBaseDirShort%\wsl;%PATH%" & %ConEmuBaseDirShort%\conemu-cyg-64.exe --wsl -cur_console:pnm:/mnt -t fish -l
```

### Change drives mount point in WSL

[Configuration file](https://blogs.msdn.microsoft.com/commandline/2018/02/07/automatically-configuring-wsl/) `/etc/wsl.conf` may be used to change drives mount point (default is `/mnt`). So you may access your files like `/c/path` instead of default `/mnt/c/path`.

- If wslbridge fails to start, update ConEmu (preferred) or update wslbridge binaries from [this issue](https://github.com/Maximus5/ConEmu/issues/1538#issuecomment-386838630).
- To get proper conversion of Windows paths during Paste change `-new_console:m:/mnt` to `-new_console:m:""`.

### Start WSL in Unix home directory

Add after `--wsl` the `-C~` switch:



```
set "PATH=%ConEmuBaseDirShort%\wsl;%PATH%" & %ConEmuBaseDirShort%\conemu-cyg-64.exe --wsl -C~ -cur_console:pm:/mnt
```

### Support different WSL distributions

If you want to install and run different WSL distributions simultaneously (Debian, Ubuntu, openSUSE, etc.) do the following steps:

1. Find the GUID of your distribution in the registry under `HKCU\Software\Microsoft\Windows\CurrentVersion\Lxss`. In my case, Debian distro has GUID `{1f6b2238-ec8d-4066-8e2b-ee31ce97ad3f}`.
2. Modify [task](https://conemu.github.io/en/Tasks.html) for your WSL by inserting after `--wsl` switch `--distro-guid={DISTRO-GUID}`. So, the task to run Debian distro on my machine looks like:



```
set "PATH=%ConEmuBaseDirShort%\wsl;%PATH%" & %ConEmuBaseDirShort%\conemu-cyg-64.exe --wsl --distro-guid={1f6b2238-ec8d-4066-8e2b-ee31ce97ad3f} -cur_console:pm:/mnt
```

## Get arrows working in ConEmu

This solution is **only for Bash on Windows (WSL)**! It does not rely to [Cygwin, MSYS](https://conemu.github.io/en/CygwinMsys.html) or [Git-for-Windows](https://conemu.github.io/en/GitForWindows.html)!

Due to the bug [BashOnWindows#111](https://github.com/Microsoft/BashOnWindows/issues/111) arrows may not be working in some cases if you start just a `bash.exe`. More details in tickets [BashOnWindows#111](https://github.com/Microsoft/BashOnWindows/issues/111) and [ConEmu#629](https://github.com/Maximus5/ConEmu/issues/629).

### Solution 1: Default task {bash}

1. Don’t use [Old ConEmu Builds](https://conemu.github.io/en/OldBuild.html)! There is no sense to complain on things changed months ago! [Update](https://conemu.github.io/en/UpdateModes.html)your installation!
2. ConEmu creates new task for ‘Bash on Windows’ automatically, you may check this by running `ConEmu64.exe -basic -run {bash}`. Also, you have to call [Add/refresh default tasks…](https://conemu.github.io/en/SettingsTasks.html#id2632) from [Tasks page](https://conemu.github.io/en/SettingsTasks.html) on your existing config.
3. So, just run `{bash}` [task](https://conemu.github.io/en/Tasks.html), arrow keys are expected to be working!

### Solution 2: StatusBar’s Terminal modes

You may enable [StatusBar](https://conemu.github.io/en/StatusBar.html) column ‘Terminal modes’. LeftClick the column and select ‘XTerm’ and ‘AppKeys’ when tab with Bash on Windows is active.

When ‘XTerm’ mode is turned on, ConEmu posts into the console input buffer ANSI sequences instead of native Windows key-codes. For example, Linux application expect to receive `^[[A` instead of `VK_UP`.

However there are two notations, and some applications turns on ‘App Keys’ mode to receive `^[OA` instead of `^[[A`. That is the problem, because without [wslbridge](https://conemu.github.io/en/BashOnWindows.html#wslbridge) ConEmu doesn’t receive the request to change the mode!

So, if keys are not working properly, it may mean that application expects another mode of ‘App Keys’. The solution is simple: just LeftClick the ‘Terminal modes’ [StatusBar](https://conemu.github.io/en/StatusBar.html) column and change ‘AppKeys’ mode!

You may change Task startup defaults with [-new_console](https://conemu.github.io/en/NewConsole.html#syntax) switch. Just add to your command:

- `-new_console:p5` to enable ‘XTerm’ *and* ‘AppKeys’;
- `-new_console:p1` to enable ‘XTerm’ *without* ‘AppKeys’.

## Get 24-bit colors working in ConEmu

As described in [Preferred way to run WSL](https://conemu.github.io/en/BashOnWindows.html#connector), wslbridge and connector are shipped with ConEmu since build [170730](https://conemu.github.io/blog/2017/07/30/Build-170730.html).

### TLDR: Just run wslbridge

Well, you may run `wsl-con.cmd` which starts wslbridge in new ConEmu tab for you.

### How to get 24-bit colors working

You need just few more files:

1) `256colors2.pl` download it from [./256colors2.pl]

2) `wsl-con.bat` to start new tab in ConEmu



```
@echo off
if "%~1" == "-run" goto run
ConEmuC -c "%~0" -run "-new_console:C:%LOCALAPPDATA%\lxss\bash.ico" "-new_console:d:%~dp0" -new_console:np
goto :EOF
:run
call SetEscChar
echo %ESC%[9999H
conemu-cyg-32.exe ./wslbridge.exe -t ./boot.sh
```

3) and `boot.sh` to print gradient map, system information and run bash prompt



```
#/bin/sh
uname -a
./256colors2.pl
cd ~
bash -l -i
```

### WSLBridge installation and Task contents

To run wslbridge in ConEmu, just do simple steps:

1. Install ‘Windows Subsystem for Linux (WSL)’ and some Linux distro (e.g. Ubuntu) from Microsoft Store.

2. Download latest

    

   ConEmu

    

   and install it.

   - If you run [Installer](https://conemu.github.io/en/VersionComparison.html#installer) ensure that feature ‘WSL support’ and ‘cygwin/msys connector’ are enabled.
   - Required files `wslbridge.exe`, `cygwin1.dll`, `wslbridge-backend` and `conemu-cyg-64.exe` should be installed into `%ConEmuBaseDir%\wsl` and `%ConEmuBaseDir%` folders.

3. Recreate [default tasks](https://conemu.github.io/en/Tasks.html#add-default-tasks), the Task `{Bash::bash}` should appear.

Task **command**:



```
set "PATH=%ConEmuBaseDirShort%\wsl;%PATH%" & %ConEmuBaseDirShort%\conemu-cyg-64.exe --wsl -cur_console:pm:/mnt
```

Task parameters (icon):



```
-icon "%ProgramW6432%\WindowsApps\CanonicalGroupLimited.UbuntuonWindows_1604.2017.922.0_x64__79rhkp1fndgsc\images\icon.ico"
```

To pass environment variable to WSL, you have two options:

1) Forward it from host system:



```
set "PATH=%ConEmuBaseDirShort%\wsl;%PATH%" & %ConEmuBaseDirShort%\conemu-cyg-64.exe --wsl -eHOST_VARIABLE -cur_console:pm:/mnt
```

2) Define it directly to wslbridge:



```
set "PATH=%ConEmuBaseDirShort%\wsl;%PATH%" & %ConEmuBaseDirShort%\conemu-cyg-64.exe --wsl -eDISPLAY=:0 -cur_console:pm:/mnt
```

Task can contain initializing commands by evaluating a passed environment parameter. The method itself is detailed [here](https://conemu.github.io/en/CygwinStartCmd.html#bashrc). You can use this in case you would like to have different Tasks corresponding to different environment and the the environment variable setting is not enough.

After following the linked **.bashrc** guide, you can pass different initializer commands to WSL for each Task.



```
set "PATH=%ConEmuBaseDirShort%\wsl;%PATH%" & %ConEmuBaseDirShort%\conemu-cyg-64.exe --wsl -eSTARTUP_CMD='. $HOME/sdk/environment-setup' -cur_console:pm:/mnt
```

### Other versions of WSLBridge

If 64-bit version is not working for same reasons, you may try other WSLBridge versions: 32-bit cygwin or 32/64-bit msys2.

1. Download [desired wslbridge release](https://github.com/rprichard/wslbridge/releases).
2. Obtain required dlls:
   - either `cygwin1.dll` from https://cygwin.com/
   - or `msys-2.0.dll` from http://www.msys2.org/
3. Collect all files in some folder, for example: `C:\Tools\ConEmu\wsl`.
4. Create the task `{WSL::Bridge}`.

If you selected cygwin-32, so the Task command would be:



```
C:\Tools\ConEmu\ConEmu\conemu-cyg-32.exe /cygdrive/c/Tools/ConEmu/wsl/wslbridge.exe -t -cur_console:pm:/mnt
```

[Back to the table of contents](https://conemu.github.io/en/TableOfContents.html) | [Suggest better edit](https://github.com/ConEmu/ConEmu.github.io/blob/master/en/BashOnWindows.md)
