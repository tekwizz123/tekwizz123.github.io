---
title: '  Practical Reverse Engineering - Chapter 1 pg 35 - Exercise #3'
date: 2018-11-21T15:10:00.003-08:00
draft: false
url: /2018/11/practical-reverse-engineering-chapter-1_21.html
---

Question: In some of the assembly listings, the function name has a @ prefix followed by a number. Explain when and why this decoration exists.


So first off I should clarify that technically the @ symbol isn't a prefix as described in the book but is rather a suffix which is part of the function's name. With that out of the way, I did some digging into the way this is used. An example of a function within the assembly listings referenced by this question was _\_DllMain@12_. According to [https://en.wikipedia.org/wiki/Name\_mangling](https://en.wikipedia.org/wiki/Name_mangling), this is a common way to perform name mangling on functions (all the following information is taken from this page so please refer to it if you wish to gain a more complete understanding. I'm just simplifying what they say and adding some examples).

So what exactly is name mangling? Simply put, when compiling a program the linker needs to know what functions are being used or declared by a program, their calling convention, and the size and types of the respective parameters. Additionally if any functions are overloaded (aka multiple declarations of the same function but with different signatures (ex making two declarations such as _int aFunc(b)_ and _int aFunc(b,c)_. Both have the same function name but different signatures as the expected parameters are different), or two functions with the same name but different namespaces (_internalFunc::theFunc(a,b)_ and _externalFunc::theFunc(a,b)_ would be an example), then mangling needs to occur.

In C the convention is as follows:

*   If the calling convention is CDECL, append _\__ before the function name.
*   If the calling convention is STDCALL, append _\__ before the function name. Then append _@_ after the function name, followed by the total number of bytes that all the parameters take up.
*   If the calling convention is FASTCALL, then add _@_ before the function name. Then append @ after the function name, followed by the total number of bytes that all the parameters take up.

If you think about it this makes sense though. In CDECL, the _caller_ is responsible for cleaning up the stack, so there is no need for the linker to really care about how many bytes are used in the _callee_, this is handled by the _caller_. In the case of STDCALL and FASTCALL though, the linker needs to be aware of how many bytes these functions use for their arguments so that it can generate code that appropriately cleans up the stack for these functions.

So from this we can break down the definition of _\_DllMain@12_. Since it starts with \_ and ends with @ followed by a number, it is a STDCALL function. Additionally based on the fact that it follows the conventions mentioned above we can also conclude it is a C function and not a C++ function (which will we show shortly). The _DllMain_ part tells the linker that the function name is _DllMain_ whist the _@12_ part tells the linker to reserve 12 bytes on the stack for the arguments. This makes sense since _DllMain_ has the definition _DllMain(HINSTANCE hinstDLL, DWORD fdwReason, LPVOID lpReserved),_ which means each argument takes 4 bytes each. 4 \* 3 arguments gives us the 12 bytes mentioned in the mangled function definition.

Interestingly according to the Wikipedia page mentioned earlier, unlike C there appears to be no clear mangling method for C++ functions. For example the Borland compiler has one way of mangling function names whilst the Microsoft MSVC compiler has a very different way of mangling names (refer to [https://en.wikipedia.org/wiki/Name\_mangling](https://en.wikipedia.org/wiki/Name_mangling) for some examples).

Perhaps the best tutorial I've seen on actually demangling C++ names by hand was this tutorial though: [https://www.int0x80.gr/papers/name\_mangling.pdf](https://www.int0x80.gr/papers/name_mangling.pdf). Worth a read if you have the time.

Also interesting note that was mentioned in the [https://www.int0x80.gr/papers/name\_mangling.pdf](https://www.int0x80.gr/papers/name_mangling.pdf) paper but if your using IDA you can change the way IDA displays mangled symbols by choosing Options->Demangled Names.... and then clicking on the option buttons to either set the demangled name as a comment, not demangle names at all, or change the function name to the demangled version. Should help if you want to clean things up a bit in your view.