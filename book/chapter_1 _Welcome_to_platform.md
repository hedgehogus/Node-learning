
## The Node.js philosophy

creator - *Ryan Dahl*

+ **Small core** - the smallest set of functionalities, leaving the rest to the so-called userland (or userspace),
the ecosystem of modules living outside the core
+ **Small modules** - one of the most evangelized principles is to design small modules, not only in terms of code 
size, but most importantly in terms of scope. “Make each program do one thing well.”
+ **Small surface area** - modules usually also have the characteristic of exposing a minimal set of functionalities.
Very common pattern for defining modules is to expose only one piece of functionality, such as a function or a constructor
Created to be used rather than extended.
+ **Simplicity and pragmatism** -  less and simpler functionality is a good design choice for software (Keep It Simple, Stupid (KISS)).
Designing simple, as opposed to perfect, fully-featured software.  we would probably have more success in trying to get
something working sooner and with reasonable complexity, instead of trying to create nearperfect software with huge effort 
and tons of code to maintain
