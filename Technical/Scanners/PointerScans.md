## Pointer Scans

Many games are written in a high level compiled language, the most common being C/C++. When the game is compiled, the variables end up in one of three locations: the heap, the stack, or in static memory. Pointer scans are a method of locating variables reliably in the heap, and sometimes the stack.

Variables in static memory are always in a fixed location, although not at a specific address. More specifically, they can be found at an offset from a module loaded in memory. For example, in the Windows XP version of minesweeper, 'winmine.exe' + 0x100579c. Depending on where the module 'winmine.exe' is loaded, the resulting address may vary, but it can be easily found. Once the program is launched, the resulting address will not change.

The problem lies in heap variables. These do not follow the same rules as static memory.
