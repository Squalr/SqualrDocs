## Engine Components

Squalr's functionality is broken down into separate packages, each of which are available on NuGet. The high level descriptions of each are given below. For details on the APIs, view the Cheat Development guide.

## Engine

The base engine project contains utils for the other projects to use, and Process APIs.

## Memory

A must-have project for any memory editor. This provides abstractions to reading, writing, allocating, and querying memory in the target process seamlessly regardless if the process is 32 or 64 bit.

### Architecture

The architecture project is for assembling and disassembling instructions.

The library [SharpDisasm](https://github.com/spazzarama/SharpDisasm) is used for disassembling. This is written entirely in C#. This supports x86/x64.

Unfortunately, there is no pure C# assembler implementations. Instead, Squalr invokes nasm.exe to perform assembly operations. While this likely isn't very performant, assemblying generally only takes place at the beginning of scripts, so this overhead is negligable. This also supports x86/x64.

### Debugger

This project is responsible for debugging a target process. Debugging is what enables the highly-used "Find What Reads/Writes" feature in Squalr.

Currently, there is only one debugger supported, DbgEng (the engine behind WinDbg). This is used via COM interfaces, which C# provides a convenient way to interface with via annotations.

## Graphics

Work in progress. The idea is to have an abstraction over OpenGL and DirectX for rendering overlays, or manipulating graphics in the target process. This would be ideal for aimbot creation.

## Input

Mostly a wrapper over SharpDX's input management. This serves as a way for scripts to query input events.

## Scripting

This library provides a set of interfaces for managing scripts, which leverage the previously mentioned project's APIs.
