# Acquaintance II.

This post is a continuation of the previous post:

- [Acquaintance I. About the author and idea.](11-01-acquaintance-i.md)


### About the project.


#### General principles.

The `voidc` project involves the creation and gradual joint development of two “components”:

- The "Void" programming language.
- The `voidc` compiler for this language.

That is:

- I don't know what the "ideal" language for such a system should be.
- I don't know what the "ideal" compiler for such a language should be.

I will try to "figure out" these things as I go.

I will need to start with a minimal language and the simplest “unroller” in C++.
Then gradually add extensibility to the compiler and develop the language itself.

At some stage, when the language and architecture of the compiler are “stabilized”,
it will be necessary to make a “bootstrap” and rewrite the “unroller” in Void itself.


Here are some thoughts on my "approaches" in the project:

- I would like the language to be similar to C.
But at the same time, the syntax regarding types should be more “mathematical”.
The example of the Rust language shows that this is quite achievable...

- I consciously try to avoid "homoiconic" solutions. For several reasons:

    - Any such solution has its limitations. And the main one is in your mind -
      “If you have a hammer in your hands, then all problems become like nails”...

    - We just don't need it. We can work with the internal representation of language structures directly,
      without the need to build a special "kitchen" for "reflection"...

- The main emphasis is on the simplest and most understandable solutions:

    - “You can’t optimize something that doesn’t exist.”
    - Nothing "esoteric". Minimum "tricks"...
    - At the beginning, an absolute minimum of diagnostics...

- In the future, the project should provide for the possibility of “seamless” integration with the C/C++ “ecosystem”.


#### "How it started."

As you can see in the initial commit, everything started out quite modestly:

- The language:

    - No any Unicode.
    - Expressions are NOT recursive.
    - The type system is purely LLVM (v11).

- The compiler:

    - Parser via [PackCC](https://github.com/arithy/packcc).
    - The simplest compilation in the [LLVM Tutorial](https://llvm.org/docs/tutorial/MyFirstLanguageFrontend/index.html) style.
    - The "Main Loop" on OrcJIT...

But at the same time:

- Quite advanced `v_import()` in Python style with saving compiled units.

- Ability to call arbitrary functions from libc, LLVM, etc.

- The technical ability to create your own functions using LLVM and call them via JIT.

Next, heading towards a full-fledged “Starter”:

- Using `pyclibrary`, a declaration of the main subset of the LLVM-C API is generated.

- `v_peg` is created - an extensible parsing subsystem based on PEG machinery:

    - Three components:

        - "Parsers" - a library of PEG "parser-combinators" (with "actions").

        - "Grammar" - a container of named parsers/actions etc.

        - "Context" - contains grammar, input stream, buffers, "memo", etc.

    - Allows you to dynamically rebuild the grammar (even during parsing).

    - Actions can influence parsing, and thus allow you to make parsing as complex and context-sensitive as you like.

    - Unicode (UTF-8) input stream support.

- `v_ast` is developed - an extensible abstract syntax subsystem:

    - Minimalistic class hierarchy supporting the Starter Language.

    - Special generic nodes in the AST hierarchy through which new types of nodes can be dynamically added...

    - Generic lists of AST elements (our greetings to "homoiconicity").

- `v_visitor` is created - an extensible AST handler:

    - Container of methods for processing AST nodes.

    - Allows you to dynamically rebuild the contents of the container.

    - Allows you to build a compiler for Starter Language.

- `v_target` is created - a subsystem of compilation contexts:

    - Global and local contexts.

    - `voidc` and `target` ...

    - Support for "identifier tables" ...

    - Support for the current compilation state.

    - Support for the stack of imports...

- `v_util` is created - a subsystem for working with `voidc` C++ objects...


All these tools have the necessary API to call from under Void...


#### "How it's going."

Main stages of project development to date:

- Development of basic constructions:

  - The `function_hack` - a relatively comfortable opportunity to create functions:

    - Allows you to syntactically construct a function “in two units”:

      - The first unit creates a function "header" and temporarily "overrides" the compiler of the next unit.

      - The second unit represents the completion of the function body...

    - This “hack” was used until it became possible to build functions “normally”...

  - Flow control statements: `if-then-else`, `block`, `loop`.

  - Minimalistic arithmetic ct-intrinsics: `v_binop`, `v_icmp`, `v_fcmp`.

  - The `grammar` statement - a DSL for grammar development.

  - `switch` statement, `v_malloc` intrinsic...

  - Creation of own Void's type system. This turned out to be absolutely necessary for constructing C-style expressions.

    - The "signedness" of integers must be "signaled" in the type system.

    - References as a new "kind of pointers" to support C-style assignments.

  - C-style expressions with some "twists":

    - `:=` as an assignment operator.

    - "Chained" relation operators, like in Python...

    - Support of LLVM's vectors "out of the box".

    - Expressions for types.

  - Basic `defer` statement...

  - Finally, some "normal" syntax for declarations/definitions...

- Creation of a system of language levels. Form "Level 0.0" and "Level 0.1".

- Level 0.2 - "C on steroids"...

- Level 0.3 - Kinda, objects...

...


#### Some theses about plans:

- First of all - documentation. It's like "breathing".

- Next - finish Level-0.3.

    - Coercions. This is what is in the works right now.

    - Namespaces. In C++ and/or Python style.

- This will allow us to move development towards two things:

    - Standard Library:

        - Strings like in C++
        - "Any" like in C++. (RTTI, etc.)
        - Smart pointers
        - Containers

    - Level-0.4:

        - Variants/matching (ADT)
        - Generic types
        - Type inference (maybe later?)

- Kind of satellite projects:

    - "C-fusion" and/or "Cxx-fusion" (Tools for "seamless" "integration" of third-party libraries with help of Clang "tooling").

    - "Fuse" some interesting libraries: GMP, GTK, Qt, Cairo, OpenGl, etc.


...


---

2023.11.16 - 2023.11.24

