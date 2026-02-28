---
layout: page_with_comments
title: "Mocking the Standard Template Library: Step 2"
permalink: /TBCI/Step2.html
---

On to our clean-up.

The compiler errors fall into several categories:
1. First, when the compiler sees ```std::uniform_real_distribution<> U01;``` it needs to know that it's a template type inside our ```std``` struct.
   When it's a dependent name (that is, depending on what ```Base``` is),
   the compiler can't know whether it's a type template, a static data member, a function, an enum or a value, unless we tell it.
   So that's what we've got to do, and we do it like this:  ```typename std::template uniform_real_distribution<>```.
   In other words, anywhere there's a ```std::``` templated type (like ```std::array<double,D>```), it needs to be fixed up with ```typename```, ```template``` or both.

3. Second, there are just plain missing types, for example, ```std::string```.
   We need to add a definition of that to our ```std``` struct, which is easily done with ```using string = ::std::string;```.
   But then the same problem as above kicks in:  the compiler can't know that ```std::string``` is a type unless we tell it.
   (MSVC's compiler is very helpful here with its compiler message: ```error C7510: 'string': use of dependent type name must be prefixed with 'typename'```.
   The fix is again simple: put a ```typename``` in front.
   
4. Third, we have missing functions, like ```std::to_string()```. You can either write a forwarding function by hand (good for function templates), or use the MOCKABLE_FUNCTION macro (good for C APIs in the global namespace), or use the [```Callable```](https://github.com/MiddleRaster/tbci/blob/main/MockableFunction.h) template.
   In this case, since it's an overloaded function, you can use the last option, like this:  ```inline static Callable<static_cast<std::string(*)(int)>(&::std::to_string)> to_string; // an overload requires cast```.
   Note that the last option creates a data-member that is a callable object, and it needs to be ```static``` (so that we don't need an instance of ```std``` to call it), and ```inline``` so that we don't get ODR violations.

At this point, everything compiles and the unit tests pass.

The next step is to write our actual mock. And we'll need at least one unit test, too.  
If you'd like to give that a try yourself, go ahead; when you're ready, click [Next](/TBCI/Step3.html).
