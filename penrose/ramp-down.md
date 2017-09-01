# Penrose Summer 2017 Ramp-down document

This document records the status of Penrose after Nimo's REU program in summer 2017, and provides some ideas about what to do next.

## Overview

- We will refer to the project root directory as "TOP" in this document

## Dependencies

- The fastest way to get the dependencies right is to `cabal install`, because all dependencies are specified in `TOP/penrose.cabal`. You can always inspect the file manually to see specific requirements.
- For Alloy support, you will need a JRE. Here is my version:
```
java version "1.8.0_25"
Java(TM) SE Runtime Environment (build 1.8.0_25-b17)
Java HotSpot(TM) 64-Bit Server VM (build 25.25-b02, mixed mode)
```
## Building the project

- There are three ways to build the project:
    - `ghc Main` in `TOP/src`. As usual, it creates all temp files in the current directory
    - `cabal build` in TOP, which will build the binary at `TOP/dist/build/penrose`
        - Feel free to add that build directory in your `$PATH`
    - `cabal install` in TOP, which will build the binary at `TOP/.cabal-sandbox/bin`
        - This install normally happens in a sandbox. You can simply execute `cabal sandbox init` in TOP the first time you clone the project from GitHub. It will take quite a while to build for the first time.
        - cabal seems to have problem resolving the dependencies sometimes. Try prepending `sudo`, and adding the flag `--reorder-goals` if there is any bizarre problem.

## Documentation

## Structure of the project

- Here is a diagram of the overall structure of the project:

- The project is now  

## Problems to solve

## Next semester...
