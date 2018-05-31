# Penrose Work Notes

<!-- TOC depthFrom:1 depthTo:6 withLinks:1 updateOnSave:1 orderedList:0 -->

- [Penrose Work Notes](#penrose-work-notes)
- [TODOs](#todos)
- [Future Schedule](#future-schedule)
- [Weekly Notes](#weekly-notes)
	- [[Week 7] Substance for \*jection, Style Language V: Style inheritance](#week-7-substance-for-jection-style-language-v-style-inheritance)
		- [New Substance](#new-substance)
	- [[Week 6] Discussion on the next Substance, Style Language IV, Layout](#week-6-discussion-on-the-next-substance-style-language-iv-layout)
		- [Next Substance: \*jective function](#next-substance-jective-function)
		- [Optimization of layout](#optimization-of-layout)
		- [Style Language IV](#style-language-iv)
		- [Others](#others)
	- [[Week 5] Tree diagram, Style Language III](#week-5-tree-diagram-style-language-iii)
		- [New Style language design](#new-style-language-design)
		- [Grammar](#grammar)
		- [Tree diagram](#tree-diagram)
		- [Others](#others)
	- [[Week 4] Style Language II, Porting to Web](#week-4-style-language-ii-porting-to-web)
		- [Style Language II](#style-language-ii)
		- [Porting to Web](#porting-to-web)
		- [Other notes](#other-notes)
	- [[Week 3] Continuous Map II, Style language I](#week-3-continuous-map-ii-style-language-i)
		- [Continuous Map II](#continuous-map-ii)
		- [Style language I](#style-language-i)
		- [Other Notes](#other-notes)
	- [[Week 2] Continuous Map and miscellaneous fixes](#week-2-continuous-map-and-miscellaneous-fixes)
		- [Should we do Template Haskell??](#should-we-do-template-haskell)
		- [Parsing the `Style` specifications](#parsing-the-style-specifications)
		- [Fix to the size problem with `Subset` constraints](#fix-to-the-size-problem-with-subset-constraints)
	- [[Week 1] Starter Project](#week-1-starter-project)
		- [Adding Points](#adding-points)
		- [Color Support](#color-support)
		- [Centering the texts](#centering-the-texts)
- [Side project: generating random Penrose program](#side-project-generating-random-penrose-program)
- [Random quotes and notes](#random-quotes-and-notes)
- [Work log](#work-log)
- [What abstractions do we want?](#what-abstractions-do-we-want)

<!-- /TOC -->

---------
# TODOs

- Today:
    - [ ] Documentation for the parser
    - [ ] a clear language spec - design doc. More examples of correct/incorrect programs
    - [ ] Document function synthesis, comparing with the old version from Kathrine
    - [ ] Migrate all my documentation to a Penrose wiki
    - [ ] Less important: server documentation
    - [ ] Document big TODOs in the code base
        - [ ] NaNs for `layers.sub`
    - [ ] read this http://brickisland.net/diagrams/2016/11/02/related-work-graph-drawing-in-tikz/
    - [ ] links to tutorials and blogs
    - [ ] ask anybody who works in MoMath
    - [ ] document the cartesian thing
- DSLDI
    - [x] ask lindsey about the length of talks, time for questions.
    - [ ] Use the proposals from Katherine and profs as resources for motivation  
    - [ ] other DSLs: TikZ and DSL is TikZ, graphviz, asymtote, tikz gallary

---------


---------------------------------------------------
# Weekly Notes

## [Week 9] New viz for maps: Cartesian Coordinate System

### Improvement to Style language
### Sample Style and Substance program for the new viz


## [Week 8] Substance for \*jection, Alloy support

- See PR [here](https://github.com/penrose/penrose/pull/6)

## [Week 7] Substance for \*jection

### New Substance

- Examples of FOL definitions:
	- “forall x in X, exists y in Y : f(x) = y” --- just by the definition of function
	- “forall x in X, f(x) = 5”
	- “exists x in X, forall y in Y, f^{-1}(y) = x” (ok, this is not a function, but it is a relation)
	- this works if we start with the forall y, and deal with the exists, fixing one sampled x instead of sampling a new one for each y
	- is there some way to convert these statements into a “forall x, y” “normal” form?
	- “forall y, y’ in Y, f^{-1}(y) = f^{-1}(y’)” (same as above statement)
	- “forall y, y’ in Y, f^{-1}(y) = f^{-1}(y’) -> ??”
	- “forall x, x’ in X, exists y in Y : y = f(x) /\ y = f(x’)” (equiv to f(x)=f(x’))
	- ““forall x, x’ in X, exists y in Y : y = f(x) \/ y = f(x’)” (i don’t even know what this is supposed to be)
	- “forall x in X, exists x’ in X : f(x) = f(x’) ”
	- this is trivially true unless you specify:
	- “forall x in X, exists x’ in X : x != x’ /\ f(x) = f(x’) ”



## [Week 6] Discussion on the next Substance, Style Language IV, Layout

### Next Substance: \*jective function

- See blog post for motivation
- New components needed:
	- Substance grammar and compiler
	- New objective function that generates the Wikipedia images
	- Alternative visualization of these definitions
		- Jonathan mentioned [Commutative Diagram](https://en.wikipedia.org/wiki/Commutative_diagram), which he often draws
		- Seems like bi/in/surjection are iso/epi/monomorphisms in category theory, not sure if we can use its diagramming convention. For example, the diagram for [Monomorphism](https://en.wikipedia.org/wiki/Monomorphism):
		 ![](https://upload.wikimedia.org/wikipedia/commons/thumb/2/2d/Monomorphism_scenarios.svg/340px-Monomorphism_scenarios.svg.png)
	- Style support for the new types. How should we pattern-match against, say, an injective function from set X to set Y?
- Sample style and substance program
	- Substance:
	```
	-- surjection.sub

	Surjection {A B : Set} (f : A -> B) : Prop :=
	forall (x x' : A), In x A /\ In x' A -> f x = f x' -> x = x'

	Set X = { 1, 2, 3, 4 }
	Set Y = { D, B, C }
	Map f X Y

	-- Applying the definition on concrete mathematical objects
	Surjection X Y f
	```
	- This application of `Surjection` on set `X` and `Y` can be translated either directly into some Style file, or into some intermediate representation. I am not sure what this representation be.
	- Style: This is just a draft, but paste it to start the discussion
	```
	-- surjection.sty
	Map `f` `X` `Y` {
		-- FIXME: I don't have a good idea to refer to `Surjection`, now we are
		-- just drawing arrows for all 2-tuple from `X` and `Y`
		-- TODO: how do we write the selector properly?
		Point a in `X`.elements, Point b in `Y` elements {
			shape = Arrow {
				start = a
				end   = b
				label = None
			}
		}
	}

	-- Style for sets and points
	Set x {
		shape = Ellipse {
			direction = vertical
		}
		objective onTop(x.label, x)
	}
	Point p {
		shape = Text { }
	}

	-- For the layouts, here I am assuming `Set X = { 1, 2, 3, 4 }` is
	-- generating a bunch of `Point` declarations and `PointIn`
	-- constraints
	PointIn p s {
		objective center(p, s)
	}
	PointIn p `X`, PointIn q `X` {
		objective sameX(p, q)
	}
	PointIn p `Y`, PointIn q `Y` {
		objective sameX(p, q)
	}
	PointIn p `X`, PointIn q `Y` {
		objective sameHeight(p, q)
	}
	```

### Optimization of layout

* Allowed sizes to be `Varying`
	- Problem: we only make sure the subset-superset size relationship hold when sampling init state, but when we have `Set A, B, C; Subset B A; Subset C A; NoIntersect B C`, we don't make sure `A` is big enough to hold its subsets
	- Solution: Made the size a `Varying` parameter in the optimizer. To make sure the sets are visible, two constraints are imposed on every object on canvas: (1) min and max sizes; (2) visible within canvas
* Worked on the continuous map and tree example to make sure they are working perfectly.
	- Continuous map: ![](assets/170709-map.gif)
	- tree version of the subset example: ![](assets/170709-tree.gif)
	- venn version of the subset example: ![](assets/170709-venn.gif)
	- (Proposed by Josh) Multi-layer subsets: ![](assets/170709-layers.gif)

### Style Language IV

- Deletion of `:` and `_`:
	- Problem: the syntax `Subset _ A : x` may cause confusion for functional programmer because `_` usually means "don't care" and `A` usually means binding rather than matching
	- Solution: following Jonathan's advice, used Scala’s stable identifier-like syntax. When an identifier like `x` appears in the selector, it is matching anything and initializing a new variable `x` as an alias to what gets matched.
	- To refer to a concrete identifier declared in Substance, use backticks. For instance, `` Subset `A` x`` would match to all supersets of `A`
- Global namespace:
	- Problem: Suppose I want to add an objective function that operates on two objects unrelated in Substance, for example, `sameSize` for `R1` and `R2`, the current selection mechanism would not allow it.
	- Solution: made all Substance ids accessible in a `global` block

### Others

## [Week 5] Tree diagram, Style Language III

### New Style language design

Here is a summary of the new grammar:
- A Style program is a collection of `Block`s
- Each `Block` has a series of selectors, which are pattern matching against constructors declared in the Substance program. For example, `Subset A _ : x` matches to all supersets of `A`, and `x` is an identifier given to the superset of A, which can be used through out the block
- The content of the block is a collection of `Stmt`s, which can be objective/constraint function calls, or assignment statement that assign some `Expr` to a key of `String` type.
- The compiler would translate the Style program to an Ast, and the Runtime will walk through the Ast and generate:
    - a set of objective functions
    - a set of constraint functions
    - a map that maps a Substance ID, to a data structure called `StySpec`, which is a giant record that contains all sorts things about a Substance object, and the Styling of it. The important components are:
        - The type of Style object it corresponds to. For example, `Set` is `Circle` in Venn diagram Style
        - A map that stores attributes related to the Style object. For example, the `start` and `end` of an arrow.

- There are two ways to create calls to objective and constraint functions now:
    - directly call them inside of blocks, with leading keywords `constraint` and `objective`
    - called by the constructors of Style objects. For example, making an `Arrow` with `start = A` and `end = B` will implicitly call `centerMap(A, B`, which is an objective function that make sure the start and end of the arrow are close to `A` and `B`

### Grammar

```haskell
data SubType = Set | Pt | Map | Intersect | NoIntersect | NoSubset | Subset | PointIn | PointNotIn | AllTypes deriving (Show, Eq)

data StyObj = Circle | Box | Dot | Arrow | NoShape | Color | Text
    deriving (Show)

type StyObjInfo
    = (StyObj, M.Map String Expr)

data StySpec = StySpec {
    spType :: SubType,
    spId :: String,
    spArgs :: [String],
    spShape :: StyObjInfo,
    spShpMap :: M.Map String StyObjInfo
} deriving (Show)

type StyProg = [Block]

type Block = ([Selector], [Stmt])

data Selector = Selector
              { selTyp :: SubType
              , selPatterns :: [Pattern]
              , selIds :: [String] }
              deriving (Show)
data Pattern
    = RawID String
    | WildCard String
    deriving (Show)

data Stmt
    = Assign String Expr
    | ObjFn String [Expr]
    | ConstrFn String [Expr]
    | Avoid String [Expr]
    -- | E Expr  -- TODO: is an expression a statement?
    deriving (Show)

data Expr
    = IntLit Integer
    | FloatLit Float
    | Id String
    | BinOp BinaryOp Expr Expr
    | Cons StyObj [Stmt] -- Constructors for objects
    deriving (Show)

data BinaryOp = Access deriving (Show)

data Color = RndColor | Colo
          { r :: Float
          , g :: Float
          , b :: Float
          , a :: Float }
          deriving (Show)
```

### Tree diagram

![170629-tree](https://user-images.githubusercontent.com/11740102/27758834-9ef46baa-5dec-11e7-8e30-941d5e3fa61b.gif)

### Others


## [Week 4] Style Language II, Porting to Web

### Style Language II

- No concrete implementation is done this week, but various discussions
- Advisor meeting: [notes](http://brickisland.net/diagrams/2017/06/22/notes-from-advisor-meeting-on-062117/), [two examples](http://brickisland.net/diagrams/2017/06/20/two-examples-of-the-new-style-language/)
	- a couple important decisions:
		- No more `constraint` block, objectives go to appropriate blocks
		- "Substance constraints" are now just another kind of object (`Subset A B` = 2-tuple with the type `Subset`)
		- Using general positions as the motivation, we might want to implement an `avoid` keyword, which tells the optimizer to stay away from certain region of the function: is this an objective, or a constraint?
- My meeting with Josh on Friday:
	- Agreed that Style language should be our next focus, and start implementing another domain will help develop the language
	- An interesting discussion about the "types" of set:  
		- In the continuous map example, the sets `R^n` and `R^m` are just normal sets, but Josh said we might want to treat them differently.
		- Josh thinks a styling language should work on classes, rather than picking some individual elements
		- remember that our current version of continuous map is a bunch of individual settings overwriting the global ones:
		```
		/*
		* This is the same continuous map program, written in the new syntax.
		*/
		Set {
			shape = Circle,
			color = Random
		}

		Map {
			shape = SolidArrow,
			color = Black
		}

		R1 {
			shape = Box,
			color = Yellow
		}

		R2 {
			shape = Box,
			color = Red
		}
		```
		- So here are a couple of possible solutions:
			- Some intermediate step that create more substance classes: The names of sets often tell us some story. We as human math students know that `R`, `N`, `C` and so on are special sets. Can we use that on Penrose? Josh said we can just do regex matching before blocks
				- Example:
				```
				[R^*] {
					shape = square
				}
				```
			- Grouping, or individual block for multiple elements:
			 	- in css we can do:
			 	```css
				h1, body {
					text-align: center;
				}
				```
			- Or we can just declare another class in Substance.

### Porting to Web

![](assets/work-notes-ebdbb.png)
- Reorganized the code into modules:
	- `Compiler.hs` and `StyAst.hs`: the parsers for Substance and Style, respectively
	- `Main.hs`: contains the main function
	- `Server.hs`: a WebSockets server
	- `Runtime.hs`: all the rest of the system
- `Server.hs` and the web frontend talk to each other via JSON packages. The current JSON formats are:
	- From server to frontend:
		- `[Obj]`: encode all objects to be rendered
	- From frontend to server:
		- `Command { command :: String }`: a single command that can be `resample`, `step`, or `autostep`
		- `DragEvent { name :: String, xm :: Float, ym :: Float }`: triggered when an object is dragged. Note that we are using the name of the object as the unique identifier. Should this be true?
- Snap.svg: working perfectly well for now
	- The Bboxes for labels are now perfect: ![](assets/work-notes-08c9c.png)
	- We can obtain the BBoxes from Snap, before we optimize anything, over JSON


### Other notes

## [Week 3] Continuous Map II, Style language I

### Continuous Map II

- Now the map example is almost fully working, with minor issues on labels. We decided not to deal with accurate label bbox measuring until the migration to JS.
    - `toLeft` force the from-set to the left of the to-set
    - `centerMap` force the map to start from just outside the shape of the from-set and end just outside the shape of the to-set. When the two sets are too close, just point from center to center instead
- Implemented a simple constraint function, triggered by `Subset`, forcing the label of a set the stay out of its subsets
    - Problem: this rule does not work all the time, because there can be overlapping sets that are not of subset relationship. A mutual repel function is needed for this purpose.
- Preview of the new continuous map example:
![](assets/170615-label.gif)
- The right combination of these constraint function is an art. I don't think there is any quantitative way other than running NN on all of Keenan's work.
    - This can be a good project: what is a good looking mathematical diagram?

### Style language I

- [The central design document]( https://docs.google.com/document/d/1si_Wncsq5PjLw8M1_w_KWr4n9jhVlLI90cFCoI4lk68/edit#heading=h.5pqdcw5sk0sh)
- Still debating...
    - Alex and Happy: the good old lex/yacc syntax, and debugging for grammar(remember those shift/reduce errors?)
    - Megaparsec: seems to be well-maintained and a popular choice, but I have no clue how to write them though.
- [UPDATE]: decided to use Megaparsec because of their compelling README document [here](https://github.com/mrkkrp/megaparsec#comparison-with-other-solutions), which lists comparisons with other popular solutions.
- I have implemented a simple parser in `StyAst.hs`, which would only parse global blocks with shape specs right now, but this means it at least works.
    - To run it, simply `ghc StyAst; ./StyAst <a-style-src-file>`

### Other Notes

- Refactoring of the code base:
    - I only refactored all constraint functions, because they took up the majority of the boilerplate code.
    - The `M.lookup` calls is handled in `lookupNames`
    - I force all constraint functions to have the same signature `    (Floating a, Real a, Show a, Ord a) => [Obj' a] -> a`, and factored out a list of `Name`s, just like Katherine suggested
    - To add a constraint function, just (1)add a function to the list, along with functions like `subsetFn`; (2) add a line in `genConstrFn`.
    - Notice that for a constraint line in Substance, you can choose to have multiple functions associated with it. For instance, `avoidSubsets` and `subsetFn` are all triggered by a `Subset` statement in the Substance program
- SVG migration: what is the interface that we need to expose to Snap.SVG?
    - There are people who used `ghcjs` to create Haskell bindings of JS program, or even compiling Haskell to JS altogether.
    - Translating the state to JSON might just to too slow, but worth a shot
    - Doesn't need to be real-time, can manually control stepping in the worst case
    - Collection of some Canvas-based vector graphic library in JS: https://stackoverflow.com/questions/4340040/html5-canvas-vector-graphics
- WebGL: Seems like it only produces raster images, but we want vector images
    - If we were to switch to 3D at some point, `three.js` might be a good library to work on


## [Week 2] Continuous Map and miscellaneous fixes

### Should we do Template Haskell??

- Adding a square into the system take two hundred insertions (rough estimation). An incomplete list:
    - Add a type `Square` and a "primed" type `Square'` to make `autodiff` happy
    - Add a render function
    - Add `inObj` function for selection by the user
    - Add energy functions:
        - For set, `subsetFn` has to match with square-square, square-circle, circle-square subset relationships.
    - Add pack and unpack functions, and decided which parameter is varying and which ones are fixed. For instance, I decided to not optimize the angle of the square and make it fixed.
- So, we might really need to add some templated stuffs.

### Parsing the `Style` specifications

- Thoughts on the representation of objects in the system:
    - I am writing because the mapping of the Style dictionary is not exactly clear to me.  
    - We start from a Substance program, which contains:
        - Abstract math objects denoted by identifiers: `Set A`
        - Pairwise (for now) constraints across types: `PointIn P B`, `Subset A B`
    - And then, we parse Substance and get a bunch of `SubDecl`s and `SubConstr`s. We also parse style and get `Styline`s
    - When we start up our runtime, we need to know what the shape each of those math objects has and some specifications about these concrete objects
    - It is clear that we have a new set of objects: math objects with Style information -> styled objects
    - What are those information?
        - Color, line, priority, direction, label, scale, absPos
    - Notice that these styled objects are still different from what we are going to render. For example, labels should be rendered individually (and labels might be the only exception here)
    - Therefore, it might be a good idea to add another layer of abstraction here?


- Possible designs for storage of Style information
    - First, due to my current Haskell ability, some of the proposals might not make sense/isn't optimal
    - **Option 1**: storing the style information directly inside the objects.  
        - For example, for a `Circ`, we add a couple fields such as `shape`, which is of type `SubShape`. I haven't verify this, but I think the pack/unpack process will not throw out fields inside objs?
        - Or we can have objects hold their own render functions?
    - **Option 2**: have a dictionary that does a similar job, which stores a mapping from `Obj`(or some string representation of it, since all `Obj`s are currently named) to some custom type that we can define. For instance, define `StyleInfo` to be a (algebraic?) type. Then I guess we have to ask if location and dimension a part of this type or not?
    - I don't know if this needs to be more complex if we have Style attributes that depends on multiple objects. Line break is a potential one. Then we might need to take that into account, too.
    - Of course, we will need a function that walks through the style AST and properly set up whatever data structure that we will be using.
- Katherine's suggestion:
    - Let's just start by generalizing `Circle` to `Square`. What parts of the system need to change then? The square datatype, for example, needs to contain a center, side length, and angle. Should it contain the endpoint coordinates? How do we label squares? What objective function should be synthesize for the combination of `Set A` and `Shape A Square`? What if one set is a circle and one set is a square? How do we write objective functions on circle-sets, square-sets, and dot-points? It could get very complicated.
    - So, maybe all our shapes should satisfy some kind of "optimization API"/typeclass. As Keenan suggested: maybe our optimization functions should in general only operate on objects that can return answers to "get thickness," "get distance," "get angle," etc. But we might need to know concretely where the borders of a square are in order to avoid putting labels on it. I don't know what the best solution is.
    - Some parts of Style are relevant to the optimization and can be changed by it (e.g. anything numerical, like angle and color), and some aren't (e.g. anything categorical, like whether the shape is a square or a circle). I suggest storing the former as fields in the record (e.g. radius of circle) and the latter as a list of style lines `StyLine`, which is then passed to the render function at the end. The list of style lines for each object will have to take into account the overrides.
    - I would not recommend having objects hold their own render functions. It's a good idea, but we can't easily inspect Haskell functions (meaning we can't compute on them, pattern-match on them...). I usually prefer storing things as data rather than as functions.
        - Is this true??


### Fix to the size problem with `Subset` constraints
- Problem: The initial implementation did not impose any constraint on the sizes of circles when the system samples the initial state. Therefore, we end up with contradictory scenarios where `Set A` is a subset of `Set B` but has a larger radius than `B`
- Solution: I added `[C.SubConstr]` to `State` so that whenever we resample the initial state, we use the constraints, specifically `C.Subset`, to force radius constraints on circles.
- TODO: the implementation is not at all elegant. I had to create another dictionary to store `[Obj]`, instead of the original `[Obj']`, and I do not know storing `C.SubConstr` inside of `State` make snese or not.
- Preview:
    - ![](assets/170606-subset-size.gif)
    - ![](assets/170606-subset-size-2.gif)
    - ![](assets/170606-subset-size-3.gif)

----------------------------
## [Week 1] Starter Project

### Adding Points

- Compiler:
    - adding `PointIn` and `PointNotIn` to the Substance language
        - Corresponding keywords are `In` and `NotIn`
    - (unlikely) adding `Shape` definition in the Style language?
- Runtime:
    - defining a `Pt` data type
        - Note that a point is not `Sized`
    - Change `Obj` typeclass to include it
- Optimization:
    - See if we can use the existing functions on `Pt`s
    - Need to design 3 functions:
        - `pointInExt`, `pointNotInExt`, `nearLabel`
        - `nearLabel` should be an ambient function that looks at other objects in the scene and make sure there is no overlapping. Here we might actually need the BBoxes.
    - I accidentally wrote the following program:
        ```
        Set A
        Set B
        Point C NoIntersect A B
        In C A
        In C B
        ```
        It does not make sense, but the compiler is absolutely okay with it. Therefore, some semantic checking is needed for things like that.
- Rendering:
    - A solid circle with `radius = 4`
    - How should label behave with points?? Definitely NOT centered, but as close as possible I guess.
        - A new energy function is needed for points' labels
        - Now just hardcoded labels to be `(10, 20)` away from the center
    - Added `inObj` for points and attempted to fix the BBox problem for labels. It now works, but when clicking from one character away, the label will still respond (consider actually rendering the bboxes?)
    - If a point is in a set, the objective function arbitrarily force the point to be `radius / 2` from the center of the circle. In the future, we could put this distance as an optimization parameter

- Preview:
    - ![pt](assets/170602-point.gif)
    - ![color](assets/170602-color-scheme.gif)
--------------------

### Color Support
- Enable solid circles: not naturally supported in gloss
    - All sets are now `ThickCircle`s with `radius = r/2` and `thickness = r`, which luckily gives us what we wanted
    - To deal with intersections, we could just set the alpha level to `0.5` all the time and have gloss deal with it. Otherwise, I don't know how we could render the intersected region differently.
- Randomize color upon start up:
    - `sampleCoord` is the most relevant function, where parameters for labels and circles are generated
    - TODO: BTW, is `crop` causing the program to hang when we only have one set?
    - After my change in `sampleCoord`. I had to change the `Circ'` definition, and a couple places where objects of `Circ` or `Circ'` types get instantiated.
    - As if now, the colors are completely randomized in `[0.0, 1.0]`. Most of the times they look pretty good.
    - The `Color` type we used is defined in Gloss, not by us.

![color1](assets/work-notes-90e10.png)
![color2](assets/work-notes-03694.png)

---------------------

### Centering the texts
- The font is some kind of vector font. According to StackOverFlow, gloss just uses whatever GLUT provides, which is possibly Helvetica Light?
    - According to one of the discussions, it is `GLUT.renderString GLUT.Roman str`, which is unfortunately not monospaced
    - "`Roman`: A proportionally spaced Roman Simplex font for ASCII characters 32 through 127. The maximum top character in the font is 119.05 units; the bottom descends 33.33 units."
    - "`MonoRoman`: A mono-spaced spaced Roman Simplex font (same characters as Roman) for ASCII characters 32 through 127. The maximum top character in the font is 119.05 units; the bottom descends 33.33 units. Each character is 104.76 units wide."
    - https://hackage.haskell.org/package/GLUT-2.7.0.12/docs/Graphics-UI-GLUT-Fonts.html
- It does NOT seem to be monospaced
- The centering offset is quite obvious. See pictures
- Reasons behind this:
    - "The ultimate problem is there are no portable font loader libraries for Haskell"
- Relevant discussions:
    - [Fonts in Gloss](https://groups.google.com/forum/#!searchin/haskell-gloss/text$20font%7Csort:relevance/haskell-gloss/xZGRTfPXfpA/wIRVnG01WzUJ)
- Quick hack: We just treat the width ~~as if it is monospaced~~, height as if it is the tallest. This will give okay adjustment when the label is just a wide character like `A` and `B`
    - I noticed that a half of a monospaced character would be more reasonable estimate, especially when the label gets longer, but this is a hack anyway.

![ctr1](assets/work-notes-d73d7.png)
![ctr2](assets/work-notes-21a42.png)
![ctr3](assets/work-notes-4ab73.png)

-------------------------
# Side project: generating random Penrose program
- The random generator: `genSub.hs` generates up to 26 objects and arbitrary number of constraints. For example:
- ![](assets/170607-many.gif)
- `rnd-1.sub`:
```
Set T
Set V
Set H
Point F
Point R
Point Y
NotIn F T
Subset T V
Subset T H
```
![image](https://user-images.githubusercontent.com/11740102/26896016-74d9300c-4b91-11e7-865c-8370f90fcf3b.png)
- `rnd-2.sub`:
```
Set E
Set P
Set L
Point A
Point G
Point Y
NotIn A E
Intersect E P
Intersect E L
```
![image](https://user-images.githubusercontent.com/11740102/26896125-c85b1984-4b91-11e7-80a3-7315782e620a.png)

- `rnd-3.sub`
```
Set T
Set C
Set Q
Point G
Point K
Point Y
In G T
NoIntersect T C
Subset T Q
```
![image](https://user-images.githubusercontent.com/11740102/26896188-f253f4ae-4b91-11e7-9f41-a8f7520021ae.png)

- Issues:
    - We only support single character label for now, that's why the number 26
    - When the number of constraints exceeds total possible permutations of pairs of point/set and set/set, we get duplicated constraints. Not sure if this will cause any problem
    - - I insisted on passing around the generator because I want to make sure we can still manually provide a seed and deterministically test the system. (and of course I do not know Monad well)
    - The program behaves correctly on a couple of the edge cases:
        - notation: (numSets, numPts, numSubsetConstrs, numPtConstrs)
        - (0, 5, 0, 5): `genSub: Prelude.!!: index too large`
        - (5, 0, 0, 5): `genSub: Prelude.!!: negative index`
        - (3, 3, 12, 0): starting to get repeated pairs, not sure if it will cause problems
        - (0, 0, 0, 0): printed nothing
        - Also a couple normal cases, seems to be fine.
- Constraints: implemented runtime checking for all constraints. Essentially, whenever the EP optimization converges, we run a round of checking. For each constraint in the Substance program, we check if the layout satisfies it. The result is then printed using `trace`
    - There is also a flag under "frequently used parameter" called `checkStateOn` that controls this behavior.
    Issues:
    - When two circles are tangent, I had to add or subtract by some very small offset to eliminate false positives. The offset I used is `0.1`.
    - The code base is getting a little redundant, which will make our next task, adding style language, even more tedious due to the boiler plate code.
    - Preview: two bad cases, and a big, consistent case. Notice in one inconsistent case the optimizer converged to violating different constraints, which is interesting.
    ![170608-check-constr-2](https://user-images.githubusercontent.com/11740102/26948032-f393e87a-4c62-11e7-95d5-b55ac7bc7035.gif)
- Katherine's Slack post
    * Some thoughts on procedurally generating set theory programs:
    * randomly sample a number of declarations for each kind of object (e.g. point, set, map)
    * assign each declaration a unique one-character name
    * for each subset of declared objects, if there exists a constraint that applies to objects of those types, for each applicable constraint, apply it with some probability.
    (e.g. a subset might be "Point p" and "Set A" and two constraints that might apply to those objects are "p in A" and "p NotIn A")
    * However, this process may produce programs with contradictory constraints, e.g. "p in A" and "p NotInA," or more complex ones like "p in A, A NotSubset B, p in B." We don't currently check for contradictory constraints; our optimizer just produces a diagram that sort-of satisfies the constraints.
    * (from my email to Daniel and Kevin, the NN diagram people)
- Tweakable parameters, e.g. number of decls, constraints, probability of constraint, probability it constrains already-used sets, etc

-------------------------
# Random quotes and notes

- "btw the way selection currently works is: if multiple objects overlap, it selects the first one that the cursor is inside in the list of objects."

- this is why the program is hanging if there's only one set. it's a combination of three mistakes on my part:
    1. the constraintflag is on, meaning it wants at least TWO SETS to overlap. first the constraint function should generated from the Substance program s.t. we find an initial state that violates the constraints. it should not hardcoded to be intersection. so i think you should just turn it off for now unless you want to tackle constraint function synthesis (which shouldn't be that hard)

    2. because the constraintflag is on, we call noneOverlap... but that function doesn't deal with labels. it assumes all objects are sets. it should be filtering out all labels (and all constraint functions should be fixed to ignore labels)

    3. noneOverlap calls noOverlapPair, which... returns true if any pair of objects is not two circles. but since there is only one set, the entire state of the world is just one circle and one label, so it always returns true. therefore the `not . noneOverlap` will always evaluate to false, so it will fail to ever find an initial state.

    best solution is to turn off `constraintFlag` and make minor fixes to `noneOverlap` and `noOverlapPair`

---------------
# Work log

- [07/25/17]
	- [x] infix function calls
- [07/24/17]
	- [x] (finally) tested on numerical argument, worked for `OnTop`
	- [x] Smarter cartesian product. Allows `In p S, In q S` to select points in the same set
	- [x] Labels for functions in the composition example
	- [x] Bug fix: check for duplicates before sending to Alloy, and make sure only when applying definition Alloy will be executed
	- [x] GC Slides
- [07/20/17] - [07/23/17]
	- [x] Alloy to Substance parser
	- [x] abstract style
	- [x] Composition example
- [07/19/17]
	- [x] Substance to Alloy translator
	- [x] Alloy Pretty-printer
- [07/14/17] - [07/18/17]
	- [x] First working version of the new Substance compiler
	- [x] Substance semantic checker
	- [x] Clean up for some Style functions
- [07/13/17]
	- [x] allow global selection
	- [x] made the function viz better!
- [07/12/17] - [07/14/17]
	- [x] Wiki-style viz for functions
	- [x] ellipse support
		- [x] containment detection for ellipse (only points in ellipse)
	- [x] Blog post on Alloy and function composition with alloy
	- [x] New font: Palentino. italic
- [07/11/17]
	- [x] continued reading on FOL, SOL, and inference algo
	- [x] found alloy and did a most basic example
	- [x] started adding numerical args into functions
- [07/10/17]
	- [x] initial research on FOL
- [07/08/17] - [07/09/17]
	- no record
- [07/07/17]
	- [x] fixed a couple cases of NaN, everything now works
	- [x] global namespace for all Substance ids and calibrated examples
- [07/06/17]
	- [x] Retrieve bboxes of labels from snap on start
	- [x] Making size variable
- [07/05/17]
	- [x] Make Arrow label optional (Hacky way)
	- [x] Simplified JSON decode
	- [x] Factored shapes into another module
- [07/04/17]
	- [x] fix the syntax for selector, delete underscores and add backticks
	- [x] fix `repel` function
	- [x] allow multiple selectors
		- [x] add in pairwise repel on all sets in `tree.sty`
		- [x] add in pairwise repel on labels/sets in `venn.sty`
- [07/03/17]
	- [x] Blog post on the new domain
- [06/27/17] - [07/02/17]
	- [x] new parser for the Style language
	- [x] tree layout
	- [x] arrow layout
	- [x] refactored objective functions
	- [x] wrote obj functions like `onTop`
- [06/26/17]
	- [x] (a little) Reading on Topology
	- [x] fixed small server bugs
- [06/25/17]
	- [x] Reading on Optimization and Topology
- [06/24/17]
	- [x] Meeting with Katherine
- [06/23/17]
	- [x] Better CSS for the web gui!
	- [x] Fixed dragging
- [06/22/17]
	- [x] implemented basically all functionalities of the web GUI
	- [x] resample, autostep, step controlled by buttons
- [06/21/17]
	- [x] Porting to the Web: now renders the initial state and allows basic dragging and minimal reorganization of the code.
- [06/20/17]
	- [x] meetings with GC and Keenan
	- [x] blog post on `tree.sty` and Keenan's complex number example
- [06/19/17]
	- [x] New parser achieved equivalent functionalities of the original one
- [06/18/17]
	- [x] Meeting with Katherine
- [06/17/17]
    - [x] set up for megaparsec
    - [x] first version of Style parser for the block-based design
- [06/16/17]
    - [x] Some googling about JS migration
    - [x] Attempted to setup GhcJS and failed
- [06/15/17]
    - [x] Fix dictionary, now mapping names to `[StyLine]`
    - [x] Refactored out a lot of the dictionary lookups
    - [x] A better label function
- [06/14/17]
    - [x] Fix Arrow related energy functions
    - [x] Created penroseDB, a database for counting lines of Penrose programs
    - [x] Initial investigation on porting to JS
- [06/13/17]
    - [x] Design document
    - [x] making some diagrams in TikZ or Asymptote
- [06/12/17]
    - [x] implemented rendering for arrows
    - [x] first working version of continuous map example!
- [06/11/17]
    - [x] Added `NoIntersect` and `Intersect` energy functions
    - [x] Meeting with Katherine and set up next week's goals
- [06/10/17]
    - [x] Build a key-value store from substance id to style info (`C.SubShape` for now)
    - [x] Added energy functions and `inObj` function
    - [x] thoughts on object representation in the system
- [06/09/17]
    - [x] Collect Style language related materials
    - [x] Language design writeup: Style design I
    - [x] Implement Square class and continue working on Style parsing
- [06/08/17]
    - [x] Random program generator
    - [x] Create and test checks for all constraints
- [06/07/17]
    - [x] Random generation for set and point constraints
    - [x] Attempted to place all labels at the top level. Graphically, they all worked okay, but the selection is a little off here.
- [06/06/17]
    - [x] Fix the problem with subsets having random sizes
    - [x] Random generation for set/point decls
- [06/05/17]
    - [x] Implemented `HollowDot` and `Cros` shapes and rendering functions for `Pt`
    - [x] Initial plan for the subset bug fix and implementation for different `Style` primitives
- [06/04/17]
    - Off
- [06/03/17]
    - Meeting with Katherine and laid out the plan for the upcoming week
- [06/02/17]
    - [x] Fixed parts commented in PR
    - [x] Added the first objective function `pointInExt`
    - [x] Design energy function for `PointIn` and `PointNotIn`
    - [x] Read Katherine's doc on obj fns and come up with questions
        - Read 2.5 times and no problem found, might need details on packing later
- [06/01/17]
    - [x] Randomize color on start-up
    - [x] Complete Compiler support for `Point`
    - [x] Make a plan for adding Point
    - [x] Test on rendering points in the system
    - [x] Complete the boiler plate section of the code (pack, unpack etc)
- [05/31/17]
    - [x] Came up with a quick fix to the label centering issue
    - [x] Solid color for circles

---------------------------------
# What abstractions do we want?

- An unified interface for objective function
    - Signed distance
    - At the same time, we should allow advanced users to provide their own custom implementations

------

# REUSE Schedule

- [x] week 5: design custom viz for set theory // tree diagram working
- [x] week 6: new style compiler? // learn topology
- [x] week 7: design sample topology sub/sty lang // learn topology
- [x] week 8: impl new viz // impl Alloy support // write DSLDI outline
- [x] week 9: impl topology? // DSLDI draft done
- [x] week 10: REU poster session, finish DSLDI paper // document / wrap-up
- REU ends aug 4th
- DSLDI due aug 7th
