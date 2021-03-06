Introduction
============

PureScript is a small strongly, statically typed compile-to-JS language with a number of interesting features, such as:

- Type Inference
- Higher Kinded Polymorphism
- Support for basic Javascript types
- Extensible records
- Extensible effects
- Optimizer rules for generation of efficient Javascript
- Pattern matching
- Simple FFI
- Modules
- Rank N Types
- Do Notation
- Tail-call elimination
- Type Classes

Motivation
----------

I was looking for a simple functional language which would compile to JavaScript and have the following characteristics:

- Generate simple readable Javascript
- Provide the ability to compile to tight loops if necessary
- Reasonable type system
- Ideally, written in a programming language I would enjoy working in
- Provides a simple interface to existing Javascript code

I didn't find exactly what I was looking for, so I wrote PureScript. It doesn't have everything right now, but it should serve as a simple core on which to develop new ideas.

PureScript was originally designed to implement purely functional core logic. However, recent additions to the compiler and libraries now also make it a good option for general-purpose programming.

PureScript can also be seen as a trade-off between a theoretically ideal language and one which generates reasonably high performance code.

Hello, PureScript!
------------------

As an introductory example, here is the usual "Hello World" written in PureScript::

  module Main where
  
  import Debug.Trace
  
  main = trace "Hello, World!"

which compiles to the following Javascript, ignoring the Prelude::

  var Main;
  (function (Main) {
      var main = trace("Hello, World!");
      Main.main = main;
  })(Main = Main || {});

The following command will compile and execute the PureScript code above::

  psc input.purs --main | node

Another Example
---------------

The following code defines a ``Person`` data type and a function to generate a string representation for a ``Person``::

  data Person = Person { name :: String, age :: Number }
  
  showPerson :: Person -> String
  showPerson (Person o) = o.name ++ ", aged " ++ show o.age
  
  examplePerson :: Person
  examplePerson = Person { name: "Bonnie", age: 26 }

Line by line, this reads as follows:

- ``Person`` is a data type with one constructor, also called ``Person``
- The ``Person`` constructor takes an object with two properties, ``name`` which is a ``String``, and ``age`` which is a ``Number``
- The ``showPerson`` function takes a ``Person`` and returns a ``String``
- ``showPerson`` works by case analysis on its argument, first matching the constructor ``Person`` and then using string concatenation and object accessors to return its result.
- ``examplePerson`` is a Person object, made with the ``Person`` constructor and given the String "Bonnie" for the name value and the Number 26 for the age value.

The generated Javascript looks like this::

  var Person = function (value) { 
      return { ctor: 'Person', values: [value] }; 
  };
  
  function showPerson(_1) {
      return _1.values[0].name + ", aged " + numberToString(_1.values[0].age); 
  };
  
  var examplePerson = Person({
    name: "Bonnie", 
    age: 26
  });

Related Projects
----------------

PureScript might be compared to other AltJS projects such as Roy, Haste, Fay, Elm and GHCJS. Certainly, there is a lot of overlap in terms of syntax, but the goals of PureScript listed above separate it in one or more ways from each of these languages.

Roy is probably the most similar language on the list, and was a large influence on the development of PureScript. There are however, key differences in the foreign function interface, the type system and the choice of development language (Haskell vs. Javascript)

Projects such as Haste, Fay and GHCJS aim to use some combination of the GHC compiler itself and/or its intermediate representation, Core, to perform some of the tasks involved in compilation such as parsing and type checking. This usually gives the advantage that tools and libraries can be shared with Haskell, but often at the cost of the size of the generated Javascript. This is the main practical difference between PureScript and these projects.

Elm also shares a lot in terms of functionality with PureScript. Elm is designed for functional reactive programming, and focusses on tools and language features suitable for that domain, while PureScript focusses on the development of purely functional core application logic. Another difference between PureScript and Elm is PureScript's lack of a runtime system.

