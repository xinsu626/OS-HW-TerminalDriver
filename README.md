# Terminal Driver

In this homework assignment, you will be writing a terminal driver in C. A terminal driver has basically two jobs: (1) print characters to the screen and (2) receive input from the keyboard. In the first homework assignment, we used the BIOS to do all of our I/O for us, but in this project, we will be doing it ourselves.

This repo contains a skeleton for your project with a demo program in assembly that prints a bunch of characters to the screen. You can write your program in either C or assembly.

Whatever language you decide to use, your program should do the following things:

1. A `main` or `_start` function that calls all of your other functions to demonstrate them.
2. A function `puts` that prints a NULL-terminated string to the terminal.
    * Calling `puts` sequentially will print strings one after another. In other words, `puts` should not overwrite text that was previously written to the screen.
    * Printing a '\n' character should advance to the next line on the screen.
    * When the cursor gets to the bottom of the screen, advancing to a new line should scroll the existing text on the screen up one line.

**For extra credit:**

3. Interrupt handler for the keyboard that (a) reads the key's scancode, (b) translates the scancode to an ASCII character, and (c) puts the ASCII character into an input buffer.
4. `getc` reads a character from the keyboard input buffer.

## Printing Characters

See the [OSDev](https://wiki.osdev.org/Printing_To_Screen) page for help on how to do this. Basically, all you have to do is write ASCII values to video memory, which starts at address `0xB8000`. There's a demo program in C on the OSDev page that prints a string to the upper left corner of the screen. That would be a good place to start.


## Coding in C

If you want to write your project in C, you need to do the following steps to make it compile:

1. Create a new C file called `term.c` in the `src` directory. Put an empty function in it called `main`:

        int main() {
            for(;;);  // Loop forever
        }

2. Add your new C source file to the Makefile. On line 10 of the Makefile there is a list of source files to compile. Right now, there's only one file in the list: `terminaldemo.o`. You need to add your new C file to the list of objects to compile. Make a new line after `terminaldemo.o`:

        OBJS = \
            terminaldemo.o \
            term.o \

**Makefiles are picky** about indentation. Don't use spaces to indent in a Makefile, use tabs.

3. In `terminaldemo.s`, put `extern main` right above the `_start` function:

        section .text

        global _start
        extern main   ; DECLARE MAIN
        _start:
            call main ; CALL MAIN

This tells the assembler that there is a function called `main` in a different source file. If you don't put that line in, you'll get error when you try to compile.

4. Call your `main` function from `terminaldemo.s`. You probably want to make the call the first instruction in the program.

## Searching Google

Googling for answers tends to be difficult because OS development is an arcane topic. Try adding "osdev" to your Google search. For example: "osdev XXX" where XXX is the term you're looking for. This will bring up answers from the OSDev wiki and forums, which tend to be pretty useful.
