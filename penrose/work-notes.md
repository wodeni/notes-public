# Penrose Work Notes - Summer 2018

This document contains Nimo's progress reports of Penrose project in Summer 2018.

## TOC

<!-- TOC depthFrom:1 depthTo:6 withLinks:1 updateOnSave:1 orderedList:0 -->

- [Penrose Work Notes - Summer 2018](#penrose-work-notes-summer-2018)
	- [TOC](#toc)
	- [TODOs](#todos)
	- [Weekly Notes](#weekly-notes)
		- [Week 0 + 1: Ramp-up and RA domain](#week-0-1-ramp-up-and-ra-domain)
	- [Work Log](#work-log)

<!-- /TOC -->

## TODOs

- Pending:
    - [ ] Dynamic parsing: read https://docs.google.com/document/d/1DPpAvBKGnr96MyohJAj2M40zrppjPV7sbd25Nz-qfkg/edit
    - [ ] Open GitHub issue about structural editor
	- [ ] Obj shape refactoring?
- Now
	- [ ] Pair with lily on FSM, BVH first example with the `contains` function

## Weekly Notes

### Week 4: Style Rewrite RA domain

### Week 3: LaTeX labels, RA domain - design
- LaTeX label: https://github.com/penrose/penrose/pull/94

### Week 0 - 2: Ramp-up, Code refactoring, Property access

- Ramp-up: https://github.com/penrose/penrose/milestone/3
- Refactoring:
- Resources
    - RA spec: https://docs.google.com/document/d/1I1KlD4fHZoOUYxg9PFmhTpLe_LoIob30ArWW7KfMORw/edit
- Questions about DSLL + Core lang:
    - What is `Map2`? How is it different from `Map`?
    - Any particular reason that `emptyArgList` requires parens?
- Refactoring `procExpr` and implementing property access in a systematic way
	- No shortcut to shapes using just the Substance identifier in `procExpr`
	- What does nested computation mean?
	- resolveObjs
	- `resolveObjs` has the synonym logic

## Work Log

- [180703]
	- [x] adding the BVH/levelset code into the main system
	- [x] started considering coordinate systems, grid resolution, and alignment problems
- [180702]
	- [x] debugging and testing BVH construction
	- [x] modularized all BVH/levelset related code into `Graphics`
	- [x] visualized BVHs for circles in the frontend
- [180701]
	- [x] Implementing BVH construction
- [180630]
	- [x] started implementing the FSM algorithm in Haskell
	- [x] write utility functions that work with `Vector` and realized how poorly the thing supports multi-dimensional vectors
- [180629]
	- [x] finished Style language parser: layering and list parsing
	- [x] added pretty printing to all DSLs (and DSLL)
	- [x] advisor meeting
- [180628]
	- [x] almost finished Style language parser
- [180627]
	- [x] paired with Katherine on Style grammar
	- [x] started implementing Style parser
- [180622] - [180626]
	- weekend: out of town
- [180621]
	- [x] Meetings: graphics with Keenan, advisor meeting
- [180620]
	- [x] Polish examples
	- [x] Penrose terminology page
	- [x] Made alternative RA style diagram
- [180619]
	- [x] Finish implementing Labels
	- [x] PR template
- [180618]
	- [x] Latex labeling support in front-end
	- [x] Implement `Label` in Substance parser
	- [x] Implement `AutoLabel` in Substance parser
	- [x] Implement `Nolabel` in Substance parser
- [180616 - 180617]
	- weekend
- [180615]
	- [x] organize JS file
- [180614]
	- [x] address Katherine's comments
	- [x] investigated how exactly property access behaves with different ordering of Substance and/or Style programs
- [180608] - [180613]
	- [x] Multiple meetings: Josh, Advisors, PL
	- [x] fix test cases
	- [x] figure out how to run Haddock with `stack` and write wiki page for documenting code
	- [x] clean up repo
    - [x] label language design
    - [x] Deprecate gloss
	- [x] Implement property access
- [180607]
	- [x] Meeting: advisors, Katherine on refactoring
- [180606]
	- [x] Realized problems with property access and started working on that
	- [x] Jonathan Group lunch
- [180605]:
    - [x] Meeting: students, PL
    - [x] Catch Keenan and schedule the meeting with him
- [180604]
    - [x] Sync with Dor about progress in DSLL and fix empty paren issues together
    - [x] SSSG: Mary Shaw
- [180603]
    - [x] Implement RA DSLL File
- [180602]
    - [x] Set up Travis CI
- [180601]
    - [x] change Dor's `[Object] : [Type]` to something like `[Object : Type]`
    - [x] Fix the extra empty parens
    - [x] Meetings: Jenna, Advisors
- [180531]
    - [x] Read and comment on Issue #75
    - [x] Reply to the project idea email
    - [x] Meeting: Josh
    - [x] Read up on Dor's DSLL + Core Substance work
    - [x] Switched the primary package manager to `stack`
- [180530]
    - [x] Fix firefox issue
    - [x] Meetings: GC, Jonathan, Katherine
