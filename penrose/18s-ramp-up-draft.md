# Summer 2018 Penrose Ramp-up Document

_This is a work in progress_

## TODOs

- [ ] google doc links (permission?)
- [ ] blog (permission
- [ ] Working with the repo
- [ ] Modify the ramp-down doc to make an official guide to the system

## Introduction

Welcome to Penrose!

## Background

We published two workshop papers [1, 2], which explains the progress up until summer 2017. Check them out in [References section](#references).

## Theory

TODO: google docs
TODO: relevant blog posts

### The core language

### Language Design

## Technical Prerequisites

- Haskell
- Third-party Haskell libraries
- JavaScript
- Java: the Alloy extension

## The Implementation

Refer to [Nimo's Ramp-down document](https://github.com/wodeni/notes-public/blob/master/penrose/ramp-down.md) first.

### Compilers

The Substance and Style languages have their own parsers that parse them into separate ASTs. The ASTs then gets sent to a common analyzer that generates geometries, objectives, and constraints that will eventually be optimized to produce diagrams.

Nimo wrote [Style compiler documentation](https://github.com/wodeni/notes-public/blob/master/penrose/style-compiler-doc.md), which explains how the Style language gets parsed, analyzed, and provides example programs.

## References

1. [Designing extensible, domain-specific languages for mathematical diagrams](http://www.cs.cmu.edu/~kqy/resources/Penrose_OBT.pdf)
Katherine Ye, Keenan Crane, Jonathan Aldrich, and Joshua Sunshine. In Off the Beaten Track ’17 (co-located with POPL).
2. [Substance and Style: domain-specific languages for mathematical diagrams](http://wodenimoni.com/assets/dsldi.pdf)
Wode Ni*, Katherine Ye*, Joshua Sunshine, Jonathan Aldrich, and Keenan Crane. In DSLDI '17 (co-located with SPLASH).
