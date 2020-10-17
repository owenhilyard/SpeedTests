# Speed Tests

When I learn a new programming language, I always implement the
Münchausen numbers problem in the given language. The problem is
simple but it includes a lot of computations, thus it gives an
idea of the execution speed of a language.

## Münchausen numbers

A [Münchausen number](https://en.wikipedia.org/wiki/Perfect_digit-to-digit_invariant)
is a number equal to the sum of its digits raised to each digit's power.

For instance, 3435 is a Münchausen number because
3<sup>3</sup>+4<sup>4</sup>+3<sup>3</sup>+5<sup>5</sup> = 3435.

0<sup>0</sup> is not well-defined, thus we'll consider 0<sup>0</sup>=0.
In this case there are four Münchausen numbers: 0, 1, 3435, and 438579088.

## Exercise

Write a program that finds all the Münchausen numbers. We know that the largest
Münchausen number is less than 440 million.

## Updates

Dates are in `yyyy-mm-dd` format.

**2020-06-24:** Cache should be a global array everywhere. Remove buffered output, we only
print 4 lines. I also started to measure the execution times with [hyperfine](https://github.com/sharkdp/hyperfine).
A benchmark automation solution is also being built.

**2020-06-23:** Debug output was removed, thus the output of the programs is only 4 lines now.
All benchmarks were re-run. Lesson learned: printing to stdout is expensive.

## Implementations

In the implementations I tried to use the same (simple) algorithm in order
to make the comparisons as fair as possible.

All the tests were run on my home desktop machine (Intel Core i5-2500 CPU @ 3.30GHz with 4 CPU cores)
using Linux. Execution times are wall-clock times and they are measured with
[hyperfine](https://github.com/sharkdp/hyperfine) (warmup runs: 2, benchmarked runs: 3).

The following implementations were received as pull requests: D, Lua, Zig.

If you know how to make something faster, let me know!

Languages are listed in alphabetical order.

The EXE files were not stripped. The indicated sizes could be further reduced with the
command `strip -s`.

### C

* gcc (GCC) 10.2.0
* clang version 10.0.1

| Compilation | Runtime (sec) | EXE size (bytes) |
|-----|:---:|:---:|
| `gcc -O2 main.c -o main -lm` | 5.699 ± 0.005 | 16,744 |
| `clang -O2 main.c -o main -lm` | 4.387 ± 0.001 | 16,696 |

Note: switches `-O3` and `-Ofast` gave the same result as `-O2`, so
they were removed from the table.

[see source](c)


### C#

* .NET Core SDK (3.1.108)

| Compilation | Runtime (sec) | EXE size (bytes) |
|-----|:---:|:---:|
| `dotnet publish -o dist -c Release` | 8.041 ± 0.016 | 97,672 |

[see source](cs)


### C++

* g++ (GCC) 10.2.0
* clang version 10.0.1

| Compilation | Runtime (sec) | EXE size (bytes) |
|-----|:---:|:---:|
| `g++ -O2 --std=gnu++2a main.cpp -o main` | 5.656 ± 0.006 | 17,264 |
| `clang++ -O2 --std=c++2a main.cpp -o main` | 4.828 ± 0.001 | 17,216 |

[see source](cpp)


### D

* DMD64 D Compiler v2.093.1
* gdc (GCC) 10.2.0
* LDC - the LLVM D compiler (1.23.0)

| Compilation | Runtime (sec) | EXE size (bytes) |
|-----|:---:|:---:|
| `dmd -release -O main.d` | 12.303 ± 0.043 | 1,969,344 |
| `gdc -frelease -Ofast main.d -o main` | 5.833 ± 0.005 | 2,328,872 |
| `ldc2 -release -O main.d` | 4.835 ± 0.002 | 20,216 |

[see source](d)


### Dart

* Dart SDK version: 2.9.3 (stable) (Tue Sep 8 11:21:00 2020 +0200) on "linux_x64"
* Node.js v14.12.0

| Execution | Runtime (sec) | compiled / transpiled output size (bytes) |
|-----|:---:|:---:|
| `dart main.dart` | 30.498 ± 0.016 | -- |
| `dart2native main.dart -o main && ./main` | 17.645 ± 0.156 | 5,867,712 |
| `dart2js main.dart -m -o main.js && node main.js` | 14.845 ± 0.002 | 34,790 |

(`*`): in the first case, the Dart code is executed as a script

[see source](dart)


### Go

* go version go1.15.2 linux/amd64

| Compilation | Runtime (sec) | EXE size (bytes) |
|-----|:---:|:---:|
| `go build -o main` | 7.975 ± 0.042 | 2,047,825 |

[see source](go)


### Java

* openjdk version "11.0.8" 2020-07-14

| Execution | Runtime (sec) | Binary size (bytes) |
|-----|:---:|:---:|
| `javac Main.java && java Main` | 7.848 ± 0.01 | 1,027 |

(`*`): the binary size is the size of the `.class` file

[see source](java)


### Kotlin

* Kotlin version 1.4.10-release-411 (JRE 11.0.8+10)
* openjdk version "11.0.8" 2020-07-14

| Execution | Runtime (sec) | JAR size (bytes) |
|-----|:---:|:---:|
| `kotlinc main.kt -include-runtime -d main.jar && java -jar main.jar` | 7.834 ± 0.004 | 1,472,421 |

[see source](kotlin)


### Lua

* Lua 5.4.0  Copyright (C) 1994-2020 Lua.org, PUC-Rio
* LuaJIT 2.0.5 -- Copyright (C) 2005-2017 Mike Pall. http://luajit.org/

| Compilation | Runtime (sec) | Notes |
|-----|:---:|:---:|
| `lua main.lua` | 258.386 ± 16.899 | -- |
| `luajit main.lua` | 23.314 ± 0.006 | -- |

[see source](lua)


### Nim

* Nim Compiler Version 1.4.0 [Linux: amd64]
* gcc (GCC) 10.2.0
* clang version 10.0.1

| Compilation | Runtime (sec) | EXE size (bytes) |
|-----|:---:|:---:|
| `nim c -d:release --gc:arc main.nim` | 7.002 ± 0.009 | 60,784 |
| `nim c -d:release main.nim` | 6.896 ± 0.004 | 89,352 |
| `nim c -d:danger --gc:arc main.nim` | 6.761 ± 0.004 | 46,784 |
| `nim c -d:danger main.nim` | 6.599 ± 0.002 | 80,304 |
| `nim c --cc:clang -d:release main.nim` | 6.467 ± 0.043 | 69,040 |
| `nim c --cc:clang -d:release --gc:arc main.nim` | 5.849 ± 0.003 | 48,632 |
| `nim c --cc:clang -d:danger main.nim` | 5.675 ± 0.006 | 64,136 |
| `nim c --cc:clang -d:danger --gc:arc main.nim` | 5.566 ± 0.003 | 38,832 |

(`*`): if `--cc:clang` is missing, then the default `gcc` was used

[see source](nim)


### Python 3

* Python 3.8.3
* Python 3.6.9 (?, Apr 17 2020, 09:36:06) [PyPy 7.3.1 with GCC 9.3.0]

| Compilation | Runtime (sec) | Notes |
|-----|:---:|:---:|
| `python3 main.py` | 418.923 ± 1.797 | -- |
| `pypy3 main.py` | 25.615 ± 0.097 | -- |

[see source](python3)


### Rust

* rustc 1.47.0 (18bf6b4f0 2020-10-07)

| Compilation | Runtime (sec) | EXE size (bytes) |
|-----|:---:|:---:|
| `cargo build --release` | 4.961 ± 0.032 | 3,185,704 |

Stripped size of the EXE: `284,832` bytes.

[see source](rust)


### Zig

* zig 0.6.0

| Compilation | Runtime (sec) | EXE size (bytes) |
|-----|:---:|:---:|
| `zig build -Drelease-fast` | 4.88 ± 0.008 | 172,552 |

Stripped size of the EXE: `6,000` bytes. And it's statically linked!

[see source](zig)
