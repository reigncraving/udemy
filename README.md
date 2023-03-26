# C practice. 
This repository will hold any projects related to `C Programming language` exercise,
and related environment for compiling and debuging. It will include all 
example C programms.

## C Language specifics

C provides 3 storage classes:

1. Auto
2. Register
3. Extern
4. Static

- `Register` is small set of data, taking place that is part of the  CPU
It can hold instructions, storage address, and any kind of data.
It is used to define local variables that should be stored in a register,
instead of RAM. This make the `Register` much faster than normal variables. It 
is `only` used with local variables.

  Good use of the `Register storage class` is when a variable gets used all the0 
  time in a program, and this will make it even faster. But this is a 
  compiler's choice whether to store marked variable, and some time the 
  compiler puts there variables for optimization automaticaly.

  When added variable scope is local to the block it's defined, the size
  is equal to the registry size, and you cannot obtain address inside of the
  `register`, nor have the unary '&' operator applyed to that variable, because 
  it does not have memory location.

  ```
  register int x;
  ```

- `External variables` - Functions in different files can communicate through
them => extension of the concept for `global variables`. `External Variables` can
be changed by a function in separate file => /mnt/e/C/C-Playground/project1a 
globally defined variable from another file. Prefixed with `extern`. To use it
must be declared as signature, mustnot be initialized. Variable needs to be
global to be used as extern.

  ```
    <file1 int n = 3> <== <file2 extern int n;>
  ```

  **file2 is the one accessing it, so the extern is palced there.
  Functions declared as `extern` can be accessed in any file in program, and it
  does not need to be defined (since it's defined in eternal file) in the header 
  of the file caller. Any variable defined outside of block or function is 
  considered `external` by default.

- `Static variables` - When a variable needs to be defined as global but not
external ( not available outside of the file). The static declaration more 
accurately reflects the viarble usage (no conflicts). Prefixed with `static`.

  Applied to `local varibles` => Tells compiler to keep the var during the life
  time of a program Preserve the state from last execution run, and no new memory
  will allocated for them on later calls. Scope is local for where they are
  declared. These variable are alocated on the
  `heap` not on the `stack`.

  Applied to `global variable` => Variable in scope only for current file.

  Applied to `function` => Same file only execution and call.

  `Static variable` should not be declared in the body of `structures`.

- `Auto variable` - exist only in block they are created, destroyed on scope change.
Keyworld is not used, also conflicts with C++. 

- `Local variables` -exist in functions bodies or block code. All variables of this
type have `automatic` storage by default, word auto is not used explicitly. No access 
from another functions => local scope.

### placeholders

`%lf` => long precision floating-point value (double)
`%f`  => single precision floating-point value (float)
`%i`  => (interger) 

## Compile

Compiler generates intermediate object files for each source it compiles. 
(file.o for windows file.obj)
```
basic flags: 
-c => compile do not link
-o => object file
```
Replacing the `'c'` extension of the `'o'` for object file passed to compiler will 
tells it to use the object files instead (already compiled ones).This could be 
used with compilers that do not delete the object files.

An `object file` is the real output from the compilation phase. It's mostly machine 
code, but has info that allows a linker to see what symbols are in it as well as 
symbols it requires in order to work. (For reference, "symbols" are basically 
names of global objects, functions, etc.) 

A `linker` takes all these object files and combines them to form one executable 
(assuming that it can, i.e.: that there aren't any duplicate or undefined symbols). 
A lot of compilers will do this for you (they run the linker on their own) 
if you don't tell them to "just compile" using command-line options. 

Most compilers and IDEs are smart enough to re-compile only changed files or
the one without object file.

```
gcc compile -c mod1.o mod2.o -o test
```

Compile multiple sources:

```
gcc <file or files> -o <executable object file>
```

Incremental compilation technique:

```
gcc -c mod1.c  # compiles mod1.c => mod1.o
gcc -c mod2.c  # compiles mod2.c => mod2.o
gcc -c mod3.c  # compiles mod3.c => mod3.o
```

custom script:
```
./build/helloworld.sh
```

when compiling multiple files the compiler compiles them independently, it does
not know of each structure, function return types, arguments. There need to be
a specification for the compiler to use.

## Make
Make is a utility that specifies a list of files and their dependencies in a
special file => `Makefile`

The make program automatically recompiles files only when necessary
- based on the modification times of a file.
- If `.c` file is newer than `.o` file it will re-compile it. (don't have to
  delete the object files to re-compile)
- You can specify source files that depend on header files (dependencies)

Example Makefile:

```
SRC = mod1.c mod2.c main.c

OBJ = mod1.o mod2.o main.o

# what to compile (name, $(PROG) => ENV variable)
PROG=myProgram$(PROG):$(OBJ)
# must be written with leading tab:
    gcc $(OBJ) -o $(PROG)

# Last line in Makefile declares depended files:
# each object file depends ont it's corresponding source file and
# if source file changes, the object file must be rebuilt
$(OBJ):$(SRC)
```

run make:
```
make
```

Make  will compile each file individually and link in the end.

## debug
`.vscode/launch.json` = script to laung debug on brake point in vscode using `gdb`

## C Coding Standard
https://users.ece.cmu.edu/~eno/coding/CCodingStandard.html
