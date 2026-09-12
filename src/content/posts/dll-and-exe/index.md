---
title: What are Dynamic Link Library(.dll) and Executables(.exe)?
published: 2026-07-09
description: Lets understand from offensive security perspective
tags:
  - Windows Internals
  - Dynamic Link Library
  - Executables
draft: false
category: "Windows"
image: ""
updated: 2026-07-09
aliases:
---
In Red Teaming, understanding Windows executables (`.exe`) and Dynamic Link Libraries (`.dll`) is fundamental. These concepts appear repeatedly (or may even confuse) in **code injection, process injection, malware development, EDR evasion, privilege escalation, persistence, and Windows internals**.

# Dynamic Link Library(`.dll`)

If you are (I hope you are) familiar with coding languages like c, c++, python, etc.... So, you already have heard of functions in the code >> the reusable code block which we can call throughout the code whenever we needed?

Windows applies the same idea at the operating system level. Instead of every application implementing the same functionality repeatedly, Windows provides shared libraries called **Dynamic Link Libraries (DLLs)**.

They are called **Dynamic Link Libraries** because Windows links them to a program at runtime (dynamically) rather than embedding all of their code into the executable during compilation.

Lets say we have an application notepad.exe. It uses the below DLLs in its execution.

```
notepad.exe
    │
    ├── kernel32.dll   → Memory, files, processes
    ├── user32.dll     → Windows and keyboard input
    └── gdi32.dll      → Drawing text and graphics
```

 The core concept is, instead of every application implementing :

- window management
- file operations
- network communication
- memory management
- graphics rendering
- cryptography

Windows stores these functions inside DLLs.

---

## Common DLLs in Red Teaming

These are a bunch of dlls which you'd commonly encounter in Windows internals, malware analysis, and Red Teaming.

```
Windows
│
├── kernel32.dll   → Basic operating system services
├── user32.dll     → Windows GUI
├── gdi32.dll      → Graphics and drawing
├── advapi32.dll   → Security and registry
├── ntdll.dll      → Low-level Native API
├── ws2_32.dll     → Networking
├── crypt32.dll    → Cryptography
├── wininet.dll    → Internet APIs
├── ole32.dll      → COM
└── shell32.dll    → Windows Shell
```

Okay, this isn't alot about dlls. Its much more interesting.

## How attackers use it?

The central idea is manipulating how a program uses Dynamic Link Libraries (DLLs) to execute your own code within the context of a legitimate, and often trusted, process. This is useful for both privilege escalation and evading security software.

---

# Executables(`.exe`)

An `exe` is also a Portable Executable (PE) in Windows Operating System that represents a complete application or program.

Unlike a DLL, an `.exe` contains an **entry point** (such as `main()` or `WinMain()`) where execution begins. When you launch an executable, Windows loads it into memory, loads any required DLLs, resolves imported functions, and then transfers execution to the program's entry point.

:::note[Key Points:]

- Every `.exe` is a Portable Executable (PE), but not every PE is an`.exe`. DLLs, drivers (`.sys`), and some other Windows binaries also use the PE format.
- An `.exe` can contain all of its own code. It doesn't _have_ to load custom DLLs, though it almost always loads Windows system DLLs like `kernel32.dll`.
- The key difference is:
  - **`.exe`** >> Starts a new process and has an entry point and can run by itself.
  - **`.dll`** >> Provides reusable code that another process loads and executes so it cannot run by itself.

:::

----
