---
title: "Getting Started With Android Analyst"
linktitle: "Getting Started With Android Analyst"
author:
- name: "0x3a"
date: 2025-08-01T19:27:37Z
draft: false
toc: false
images:
tags:
  - getting started
---
Reverse engineering is the process of analyzing a software system to identify its components and their interrelationships. This can be useful for understanding how an application works, identifying vulnerabilities, or even learning from the code. In this post, we'll focus on reverse engineering Android APKs.

> **Important Legal Disclaimer**: Reverse engineering is a gray legal area. You need to make sure you have permission to reverse a piece of software. I assume no legal responsibility for any legal actions taken against you.

---

## Where Do We Even Start?

This guide is not the standard way of doing reverse engineering; it's just how I do it. If you haven't read "Practical Malware Analysis," you should. However, it doesn't go into Android much.

---

## Understanding APK Structure

Let's talk about what an APK and other binaries are. A binary is a folder that has the ability to execute its contents within the OS. Inside every executable, from **exe** (also called 'PE') to **dmg** on macOS to **deb** on Debian Linux to **apk** on Android, they all have a file structure inside of them that gives us info on the program they are contained within.


Your standard apk structure looks like this:
```
MyApp.apk
│
├── META-INF
│   ├── MANIFEST.MF
│   ├── CERT.SF
│   └── CERT.RSA
│
├── res
│   ├── drawable
│   ├── layout
│   └── values
│
├── assets
│   └── (various asset files)
│
├── lib
│   ├── armeabi-v7a
│   │   └── libnative.so
│   └── arm64-v8a
│       └── libnative.so
│
├── AndroidManifest.xml
├── classes.dex
└── resources.arsc
```
----

## How To Extract An apk To A Folder.

While I said that APKs are just folders with the ability to execute code on the OS, we still need tools to go from APK to a folder that we can open in VSCode.

I use [JADX](https://github.com/skylot/jadx). While there are others, I like it because you download the release and run path/to/jadx nameofapk.apk, and it just works. It comes with a GUI that I never use, and all around, it's a simple and easy-to-use tool.

----


## How To Figure Out What Language & Framework the App Is Using

One of the main reasons we need to know this is because the different frameworks cause us to use different ways of decompile the app. 

Some of the main ones are **React**/**React Native**, **Flutter**, and Kotlin/java stacks like **Jetpack compose**. Their ones that are on longer around like Microsoft's **Xamarin**, and **Apache Cordova**. I have even seen apps built in game engines like **Unity**, **Godot**, and 2d frameworks like **Love2d**.

The reason why we care so much about the framework and language used is because the classes.dex file are only the setup for something like React, Flutter, or whatever else. 

---

Here are some of the most common ways of telling what is what:

- React and React Native uses a bundle file found in the *assets* folder called `index.android.bundle`.

- Flutter will have an `libflutter.so` file in the libs folder. You can also check the **AndroidMaifest.xml** file for something like `io.flutter.app.FlutterActivity`

- Anything Java or Kotlin will be in the dex files and Jadx will handle that for you. 

- Xamarin will have `libmonosgen-2.0.so` in the libs folder. 

---

## Code Obfuscation & Packing. 

Nowadays it's common for even simple apks to be Obfuscated or Packed. If you don't know what Obfuscation is. It's process of making code harder to reverse engineer. Packing is the act of packing code or compressing code. So pack code might go from this:

```c
  struct Player {
    int hp,
    int lives 
  }

```

to this 
```c 
  struct g {
    int a,
    int e
  }
```

## Identifying Obfuscation 

You can figure out if an react native app uses obfuscation by running 
```sh 
  file assets/index.android.bundle
``` 
This shouldn't return something like

```sh 
  filetype: data
```

This means it's heavily obfuscated and probably encrypted still. No static tool can help with this. You have to run in on a android and pull the JS code of the ram at runtime using something like [Frida Server](https://frida.re/docs/android/) and some python skills.

If your just starting to learn I would save this for later since the process is a long and drawn out one. 

## Okay, I Know What Framework The App Is Using Now What?

> **Note**: Some app use more that one language or framework to make an app. In the era of Libraries on-demand and startups, I have seen React Native apps with Love2D to handle 2d filters for cameras.

---

### Decompilling Different File Types

- Native Libraries (.so files): Any `.so` file can be loaded into Binary Ninja, IDA, or Ghidra.

- Dex Files: Any `.dex` class file can be converted to a java/kt class file using Jadx.

- Bundle Files: Any bundle file should be able to be opened in something like VSCode. If not, theres a tool called `react-native-decompiler` that you can use.

- Other Files: For anything else, you can do the same thing you would for a PC program. 

---

## All The Steps Together Now

1.    Extract the APK: Use Jadx or another tool to extract the APK.

2.    Identify the Framework: Use the commands and indicators mentioned above.

3.    Decompile the Files: Use the appropriate tools for the identified framework.

4.    Analyze the Code: Start analyzing the decompiled code to understand the application's functionality.

--- 

## Conclusion

Reverse engineering Android APKs can be a complex but rewarding process. By following the steps outlined in this guide, you should be able to get started with identifying the framework, decompiling the APK, and analyzing the code. Remember to always ensure you have the necessary permissions before reverse engineering any software.

Happy reverse engineering!