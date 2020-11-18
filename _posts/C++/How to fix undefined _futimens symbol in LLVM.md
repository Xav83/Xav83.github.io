# How to fix undefined _futimens symbol in LLVM on OSX

Hello ! I'm Xavier Jouvenot and in this blog post, we are going to see how to correctly fix an error with undefined "_futimens" symbol in LLVM on OSX.

_Self promotion_: You can find other articles on my [website](www.10xlearner.com)

## Problematic

When integrating the latest version of LLVM with your OSX project, you may have encountered an error like:
```bash
Undefined symbols for architecture x86_64:
  "_futimens", referenced from:
        llvm::sys::fs::setLastModificationAndAccessTime(int, std::__1::chrono::time_point<std::__1::chrono::system_clock, std::__1::chrono::duration<long long, std::__1::ratio<1l, 1000000000l> > >) in libLLVMSupport.a(Path.cpp.o)
```

This is a problem you will surely meet when trying to use a llvm imported via [brew](https://formulae.brew.sh/formula/llvm) or [macport](https://ports.macports.org/port/llvm-9.0/summary) or even from the [GitHub of the LLVM project](https://github.com/llvm/llvm-project/releases) in your program.

But have no worries, we are going to see how to solve this problem 😉

## Solutions

There are actually two solutions that you can implement to fix this problem.

#### OSX support and LLVM versions

The recent version of LLVM doesn't have a support for "old" version of OSX, but should be able to be integrated on the modern OSX version, officially supported by OSX (10.13 or later).

So a solution to the problem is to update the version of the LLVM and update the OSX SDK you use in your project to have your project working.
You should at least update your OSX Sdk to the version 10.13, since that, from this version of OSX, the "futimens error" is no longer present.

So, if you no longer need to support OSX 10.12 or older, this is the solution you should implement, as it will be the simplest to implement, and to maintain.

#### For older OSX SDK

If you need to support version of OSX older than the 10.13, then this is the solution you have to implement, as there is no other available to my knowledge.
And this solution consists in building your LLVM on your own.

This solution may seem a big task and overwhelming, but you can actually do it pretty simply.
Indeed, the LLVM project can be build using cmake command very easily with the following command

```bash
cd llvm-project/llvm/build
cmake .. -G "Unix Makefiles" -DCMAKE_INSTALL_PREFIX=../../../install -DCMAKE_BUILD_TYPE=Release -DLLVM_ENABLE_ASSERTIONS=OFF -DCMAKE_OSX_DEPLOYMENT_TARGET=10.12
cmake --build . --config Release --target install
```

Even on the latest LLVM release, you can support OSX 10.12 or older because of the variable [CMAKE_OSX_DEPLOYMENT_TARGET](https://cmake.org/cmake/help/v3.0/variable/CMAKE_OSX_DEPLOYMENT_TARGET.html).
The only inconvenient of this solution, is that generating all the LLVM libraries takes a loooooooong time 😜

But once done, you can use LLVM without problem on your project without any problem :wink:

-----------

Thank you all for reading this article,
And until my next article, have a splendid day ���

## Interesting links

- [CMAKE_OSX_DEPLOYMENT_TARGET documentation](https://cmake.org/cmake/help/v3.0/variable/CMAKE_OSX_DEPLOYMENT_TARGET.html)
- [LLVM Cmake generation](https://llvm.org/docs/CMake.html)
- [OSX Sdks GitHub](https://github.com/phracker/MacOSX-SDKs/releases)

