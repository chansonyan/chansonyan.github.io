---
layout: post
title: Debug：Unresolved external symbol _sprintf and _sscanf in Visual Studio 2015
categories: Debug_Windows
tags: Visual_Studio_2015 Debug
---

Recently, I was following the tutorial [OpenGL Essentials LiveLessons by Paul Varcholik](https://www.safaribooksonline.com/library/view/opengl-essentials-livelessons/9780133824360/). When I donwload [the sample source codes](https://bitbucket.org/pvarcholik/opengl-essentials-livelessons/overview), and played, no luck. I got errors like:
```c++
1>  Generating Code...
1>glfw3d.lib(context.obj) : error LNK2019: unresolved external symbol _sscanf referenced in function _parseGLVersion
1>glfw3d.lib(init.obj) : error LNK2019: unresolved external symbol _vsnprintf referenced in function __glfwInputError
1>LIBCMTD.lib(vsnprintf.obj) : error LNK2001: unresolved external symbol _vsnprintf
1>LIBCMTD.lib(vsnprintf.obj) : error LNK2001: unresolved external symbol __vsnprintf
1>F:\OpenGL\pvarcholik-opengl-essentials-livelessons-a0365eec59d9\source\Lesson1.2\bin\Debug\Lesson1_2.exe : fatal error LNK1120: 3 unresolved externals
```

Answer: [legacy_stdio_definitions.lib](http://stackoverflow.com/questions/32418766/c-unresolved-external-symbol-sprintf-and-sscanf-in-visual-studio-2015)

Add the following library to the linker input files:
```c++
legacy_stdio_definitions.lib
```

VS 2015 now uses inline definitions that call internal functions for many of the <mark> stdio.h </mark> functions. If an object file (or library member) depends on one of those functions, then the <mark>legacy_stdio_definitions.lib</mark> provides an externally linkable version of the function that can be linked to.

Your other option is to recompile the unit that depends on those functions with VS 2015 (this is probably the preferred option).

总结：下载的代码是用VS2013编译的，我是在VS2015上玩的，有些改变。
