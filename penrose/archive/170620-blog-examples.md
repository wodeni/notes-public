This post includes my attempt to implement, using our new Style language design, (1) the tree diagram example for set theory, and (2) Keenan's complex number example from the previous blog post.

### (1) Tree representation of subsets
A tree-like representation of subset relationship is our immediate next target to visualize. Here is my attempt:

```
-- Global blocks can be omitted when not needed
Set {
    -- "None" is predefined, the renderer skips Nones
    shape = None
    -- Referring to a "Subset" constraint, which takes 2 arguments
    Subset A, B:
        -- Binding an arrow to an id, which is visible globally
        arr =  Arrow {
            start   = A.shape
            end     = B.shape
            -- "near" is a function built into Arrow objects, adding a
             constraint to the system by calling "near(A.shape, start)" and
             "near(B.shape, near)"
            spacing = near
        }
        -- A keyword "show" that signals the renderer
        show arr
}
constraints {
    forall Subset A, B {
        repel(A, B)
    }
    -- we can also specify constraints on the arrow here, using its id:
    -- near(A.shape, arr.start)
    -- near(B.shape, arr.end)
}
```

The most important aspect of this example is that the arrows in the diagram are "triggered" by a constraint declared in the Substance program. This implies that we must have a way to refer to these constraints. My naive approach here is to use the exact syntax from the Substance program. Moreover, the arrows are associated with multiple objects, potentially from different types. For example, if we were to add a point in the Substance program and try to draw a red arrow from a point to a set whenever a point is in a set, where should we put this `show arrow` statement then? This constraint is not technically owned by one single object. Does it make sense to put it in any particular block?

### (2) Keenan's complex number example

I have another version of the DSL for complex numbers, which is very similar to Keenan's version, with a little scoping added to reduce redundancy. I haven't taken the time to think about the grammar for this language, so as always, I just used the math notation here.

```
-- MyComplex.dsl
-- define a new type called "Complex"
Type Complex {

    -- raw attributes
    re :: double
    im :: double

    -- derived attributes: can refer to the raw attributes
    -- the angle with the real axis
    arg :: double -- might be able to omit this line if we have type inference
    arg = atan2(re, im)

    norm :: double
    norm = sqrt(re * re + im * im)  # the length

    isReal, isImag :: Bool
    isReal = im == 0
    isImag = re == 0
}
```

The substance program is trivial:

```
-- Complex.sub
-- The grammar here is undefined. We should either force a unified grammar
-- or let the users specify

Complex z
z = 1.5 + i
```

Here is the Style file for the polar representation of complex numbers:

```
-- Complex_polar.sty
global {
    -- Is this the right place to draw x,y axis?
}

Complex : z {
    shape = Arrow {
        start = (0, 0)
        end   = (z.re, z.im)
    }
    -- How do we add more shapes to the same object?
    -- Maybe attach it as a new data member of the object
    arc :: Arc
    arc = Arc {
        start_ang = 0
        end_eng   = z.arg
        radius    = Random
        center    = (0, 0)
    }
    -- Using the "show" keyword here to render an arc for every complex number
    show arc
}
```

I omitted the code for drawing x, y axises. I imagine it would be something like a library call here (or specified n a `View` file somewhere)?
Since the arc is associated with one number, so I declared a data member/field for every complex number. I gave it a name so that we can refer to it in the `constraints` block later. Say we want to make sure the radii of the arcs are not larger then a half of the magnitude of the vector:

```
constraints {
    forall Complex z {
        z.arc < 0.5 * z.norm
        -- (<) just happened to be a constraint function defined using an operator
    }
}
```

We then want to change the Style file just a little bit to get to rectangular coordinates:

```
-- Complex_rect.sty
global {
    -- Is this the right place to draw x,y axis?
}

Complex : z {
    shape = Arrow {
        start = (0, 0)
        end   = (z.re, z.im)
    }
    
    -- New features: a bracket and two dashed lines

    brac :: Bracket
    brac = Bracket {
        start = z.shape.start
        end   = z.shape.end
    }
    show brac

    vert_l, hori_l :: Line
    vert_l = Line {
        start = (z.re, z.im)
        end   = (z.re, 0)
        style = dotted -- or DottedLine as a different type, idk
    }
    hori_l = Line {
        start = (z.re, z.im)
        end   = (0, z.im)
        style = dotted -- or DottedLine as a different type, idk
    }
    show vert_l
    show hori_l
}
```

A lot of design questions come up by just doing a simple example. Now I start seeing the importance of examples in programming language design, which is something that we really lacked when taking the Complier course later semester : )
