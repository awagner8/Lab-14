# Lab 14

## Overview

This lab requires you to learn a few things about compilation, linking, and
dependencies. To get full points you will have to compile and link `main.c`,
`func1.c` and `func2.c` using the instructions in the next section, then answer
a few questions on Gradescope about the resulting files.

An additional section is provided in this `README` to elaborate on dependencies,
linking and the `make` tool.

---

### `main.c`, `func1.c` and `func2.c`

First, use `gcc` to generate separate `.o` files for each of `main.c`,
`func1.c`, and `func2.c`:

```bash
gcc -c main.c
gcc -c func1.c
gcc -c func2.c
```

This will generate the files `main.o`, `func1,o`, and `func2.o`.

Now use `objdump` with the `-t` flag to examine the symbol tables of `main.o`:

```bash
objdump -t main.o
```

Notice what it says for `func1`, `func2`, and `printf`.  Also take note of what
it says for the global variables defined in `main` and `func1`.

Now use `objdump` with the `-r` flag to see the relocatable object information:

```bash
objdump -r main.o
objdump -r func1.o
objdump -r func2.o
```

Again look at all three `.o` files. Notice that `func2.o` doesn't have any
symbols that need relocation, as there is no table of relocation records for the
`.text` section, which is the section containing the instructions.

Now create an executable using `gcc`:

```bash
gcc main.o func1.o func2.o
```

This produces a file called `a.out`. Use `objdump` to look at the symbol table
(use the `-t` flag) of `a.out` and notice the changes for the `func1` and
`func2` entries compared to how they show up in `main.o` (Note that we are
referring to the entries in the symbol table titled `func1` and `func2`, not the
ones titled `func1.c` and `func2.c`).

You should see that `func1` and `func2` are now defined in the final executable.
`gcc` invokes the linker (`ld`) to link the three object files and libraries
together to create the final binary executable.

For many of the questions it is helpful to use `objdump`'s `-d` flag to view the
disassembled x86 instructions of a compiled object files or executables, much
like using `disas` in `gdb`:

```bash
objdump -d main.o
objdump -d func1.o
objdump -d func2.o
objdump -d a.out
```

---
### Dependencies, Linking and Makefiles

Creating distinct object files and then linking them together enables separate
compilation.  

You can experiment with this by changing `func1` to return a different value.
Generate a new `func1.o` and the use `gcc main.o func1.o func2.o` to generate a
new executable `a.out`.

The process of editing and recompiling only the modified files is a big time
saver for large programs. It also represents an instance of an important systems
concept: *dependencies*.

`a.out` depends on `main.o`, `func1.o` and `func2.o`.  Each of the `.o` files
depend on the corresponding `.c` file. The `make` utility is designed to
automate the construction of binaries and enables specifying the dependence
relationship among files.

The included `Makefile` is set up to provide separate compilation. Test this by
modifying one of the source files and then type:

```bash
make
```  

You should see that it only builds the `.o` files of the modified source files
and then the final executable binary. The file `Makefile.2` shows a way to use
the variables within a `Makefile` to create a more compact solution. You can
invoke it instead of the normal `Makefile` using the `make` utility's `-f` flag:

```bash
make -f Makefile.2
```

We will not study the `make` utility in more detail, but it is a cool tool to
know so feel free to do more research into it.

If you want to see all the different libraries in Linux use the command `ls` to
look at the directory `/usr/lib/x86_64-linux-gnu`.  You should see numerous
files starting with the name `lib` and ending with either `.a` or `.so`. The
`.a` are static libraries and the `.so` are the shared libraries.  See lecture
and the textbook for more information about static vs. shared libraries.

---

### Submission

Answer the questions for Lab 14 on Gradescope.
