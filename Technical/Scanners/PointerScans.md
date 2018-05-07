## Pointer Scans

Many games are written in a high level compiled language, the most common being C/C++. When the game is compiled, the variables end up in one of three locations: the heap, the stack, or in static memory. Pointer scans are a method of locating variables reliably in the heap, and sometimes the stack.

To view the stacks, heaps, and static memory of a running process, check out the tool [Process Explorer](https://docs.microsoft.com/en-us/sysinternals/downloads/process-explorer) by Microsoft (no, this is not the Task Manager that comes with Windows). This is a great way to visualize how memory is laid out in a game.

Note: Pointer scans are not a solution for games based on the CLR, JVM, or another interpreter. Different technology specific solutions are used for these that take advantage of the particular interpreter.

### The Problem

Variables in static memory are always in a fixed location, although not at a specific address. More specifically, they can be found at an offset from a module loaded in memory. For example, in the Windows XP version of minesweeper, 'winmine.exe' + 0x100579c. Depending on where the module 'winmine.exe' is loaded, the resulting address may vary, but it can be easily found. Once the program is launched, the resulting address will not change.

The problem lies in heap variables, where most variables are found. These do not follow the same rules as static memory. These variables are created at runtime, and are assigned an address **randomly** by the operating system. The first time the game runs, health may be at address 0x359001c, and the next time it may be at address 0x214701c, and so on.

This creates the following issue: If a variable, such as health, is assigned a location randomly, how do we find it easily again? We do not want to have perform tedius memory scans every time we want to change health. Pointer scans are the solution to this problem.

### The Intuition

Note: There are a few things in the example below that are not 100% correct, but the examples here will give a really good intuition for what is happening behind the scenes.

To best understand pointers, lets go over an intuitive example. The user launches a video game, and opens up  their saved game. All static memory has been created at this point.

They encounter a loading screen, which means new things are being created and loaded into memory. If we focus on the player, the chain of events may look something like this:

1. The game is loaded to static memory when the user double clicks the exe
2. The user loads their save file or starts a new game
3. The game allocates a level object
4. The level allocates a player object
5. The player object contains the player's health

Note the use of the term **allocates**. Allocates generally implies that the variable is created on the heap, and can be found through a pointer. In other words, our static 'game.exe' has a pointer to the level, and the level has a pointer to the player. If we can find that first pointer, we can follow it to our player's health.
