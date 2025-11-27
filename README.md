[Русский](README-RU.md)

# ReHLDS Insight: Your Helper for Diagnosing Counter-Strike 1.6 Server Crashes

**A simple tool for complex problems.**

---

## 🤔 What is ReHLDS Insight?

ReHLDS Insight is a special build of the Half-Life Dedicated Server (HLDS) for Counter-Strike 1.6. Its main feature is that all key components (ReHLDS, ReGameDLL, Metamod, AMX Mod X, etc.) are compiled with **debugging information**.

When a regular server crashes, it often leaves no trace or provides only cryptic error information. ReHLDS Insight, thanks to the embedded debugging information, creates a detailed **crash log** upon crashing. This report contains a wealth of technical information that can help understand *exactly why* the server crashed.<br/>
Furthermore, it includes the **name of the last executing AMX Mod X plugin** right before the crash occurred. This provides a crucial clue for finding the potential culprit.

## 💡 Why Use ReHLDS Insight?

The main goal of this project is to help CS 1.6 server administrators diagnose **unexplained or random server crashes**. If your server crashes from time to time and you can't figure out the reason, ReHLDS Insight could be your best investigation tool.

## 🚀 How to Use ReHLDS Insight to Find the Cause of Crashes?

If your main CS 1.6 server suffers from unexplained crashes, follow these basic steps:

1.  **Download and Prepare ReHLDS Insight:**
    * Go to the [Releases](https://github.com/hun1er/rehlds-insight/releases) section of this repository and download the latest version of ReHLDS Insight.
    * Create a **separate folder** for the test server. **Remember: ReHLDS Insight is intended for temporary diagnostics only and is not suitable for permanent use due to potential performance degradation!**
    * Extract the ReHLDS Insight archive into this new folder.

2.  **Transfer Your Configuration:**
    * Copy **all your plugins, modules, and settings** from your main server to the test server with ReHLDS Insight. This includes:
        * AMX Mod X plugins.
        * Additional Metamod or AMX Mod X modules you have installed.
        * Configuration files for plugins and modules.
        * Maps, models, sounds, sprites, etc.
        * Any other resources required for your server build to work.

    * ⚠️ **VERY IMPORTANT:** When copying, **DO NOT REPLACE (DO NOT OVERWRITE)** any files with the `.so` extension that are already present in the downloaded ReHLDS Insight build! These specific files contain the debugging information. If you replace them, ReHLDS Insight will not be able to generate a detailed crash report.

3.  **Run and Wait:**
    * Start the test server with ReHLDS Insight.
    * Wait for the server to crash (for the problem to reproduce).

4.  **Examine the Report:**
    * After the crash, find the report file named `debug.log` in the server's root directory.
    * Open it and look for the `------------ AMX Last Exec Plugin ------------` section at the end.<br/>
    The plugin name listed there is the one that was executing last before the crash.

## 📄 Example Crash Report (snippet)
```
##############################################
Start of crash report
... (lots of GDB technical info: call stack, registers, loaded libraries) ...

#0  0xf1255182 in set_tr2 (amx=0x9684540, params=0xf10fd58c) at /app/amxmodx/modules/fakemeta/fm_tr2.cpp:64
#1  0xf13b2c57 in amx_Callback (amx=0x9684540, index=3, result=0xffb61bd0, params=0xf10fd58c) at /app/amxmodx/amxmodx/amx.cpp:479
#2  0xf13f0f52 in JIT_OP_SYSREQ () from /home/hunter/rehlds-insight/cstrike/addons/amxmodx/dlls/amxmodx_mm_i386.so

...

------------ AMX Last Exec Plugin ------------
crash_test.amxx  <---- name of the last executing AMX Mod X plugin

End of crash report
##############################################
```

## ⚠️ Important Note on Crash Cause: 50/50

While the information about the last executing plugin is very useful, **it is not a 100% guarantee** that this specific plugin is guilty of the crash!

**Possibly**, the crash is indeed caused by a bug in this plugin's code.<br/>
**Possibly**, the plugin was just "in the wrong place at the wrong time." It might have performed an action (like calling an engine function or another module) that triggered the crash, but the actual bug lies deeper – in the server engine, a Metamod plugin, or even a third-party library.

Nevertheless, knowing the last active plugin is an excellent **starting point** for further investigation.<br/>
You can try temporarily disabling this plugin to see if the crashes stop.

## ⚠️ Important: Not for Production Use!

Builds with debugging information (`debug builds`) can run **somewhat slower** than standard (`release builds`) optimized for performance. This is due to the presence of additional data and the lack of certain compiler optimizations.

Therefore, ReHLDS Insight is **not recommended** for use as your main, "live" game server on a permanent basis. Its purpose is to be a **temporary tool** for:
* Diagnosing crash causes.
* Testing plugins by developers (scripters) during their creation process.

Once the cause of the crash is found, you should revert to using a standard, optimized (`release`) server build for everyday operation.

## 👨‍💻 For Plugin Developers (Scripters)

ReHLDS Insight can also be useful for those who write AMX Mod X plugins. If your plugin causes unexplained crashes during development or testing, running the server based on ReHLDS Insight can provide much more information about the cause of the failure than a standard server. The detailed GDB report (even if you are not a C++ programmer) can sometimes give hints on where to look for the error in your plugin.

## 🖥️ Running on Windows

This build is intended for **Linux**. If you are using Windows, you can run ReHLDS Insight using:

1.  **WSL (Windows Subsystem for Linux):** The recommended method. Install WSL and a Linux distribution (e.g., Debian) from the Microsoft Store, then work with the server inside the Linux environment.

2.  **Virtual Machine:** Use applications like Hyper-V, VirtualBox, or VMware to install a full Linux OS and run the server inside it.

## 📦 Components

ReHLDS Insight includes the following pre-installed key components (compiled with debugging information):

| Component | Version |
|-----------|--------|
| [**ReHLDS-M**](https://github.com/hun1er/rehlds-m)        | 1.3.1         |
| [**ReHLDS**](https://github.com/rehlds/rehlds)            | 3.14.0.857    |
| [**ReGameDLL**](https://github.com/rehlds/ReGameDLL_CS)   | 5.28.0.756    |
| [**Metamod-R**](https://github.com/rehlds/Metamod-R)      | 1.3.0.149     |
| [**ReUnion**](https://github.com/rehlds/reunion)          | 0.2.0.34      |
| [**ReSemiclip**](https://github.com/rehlds/resemiclip)    | 2.4.3         |
| [**AMX Mod X**](https://github.com/alliedmodders/amxmodx) | 1.10.0.5473   |
| [**ReAPI**](https://github.com/rehlds/reapi)              | 5.26.0.339    |
