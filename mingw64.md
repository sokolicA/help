# Installation

View [this link](https://www.msys2.org/) for more details.

Open the msys2 mingw application/terminal by pressing the options/start button and searching msys2 mingw64.

- pacman = package manager :see pacman -h

- S Sync

Run

```
pacman -S mingw-w64-x86_64-gcc
```

to install the C/C++ compilers or

```
pacman -S mingw-w64-x86_64-toolchain
```

to install the whole toolchain (including fortran, ada, objective C,...).

Run `g++ --version` to see the version of the successfully installed compiler.
