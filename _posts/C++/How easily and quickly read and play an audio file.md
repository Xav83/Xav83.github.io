# How to easily and quickly read/play an audio file in C++

Hello! I'm Xavier Jouvenot and in this small post, we are going to see how to easily and quickly read/play an audio file in C++.

_Self promotion_: You can find other articles on my [website](www.10xlearner.com) ���

## Problematic

In C++, there are sadly some operations that require a lot of boilerplate code to do some simple operations.
Playing a audio file is on of those operation that can be difficult to do natively in C++, whereas in some other language such as Python or Javascript.

But the more the C++ language goes forward with its new releases and the more the C++ tools progress, the more those operations become simpler to do.
And with that in mind, when I tried to play a sound file in a simple terminal application and didn't find some valid article or online solution, I decided to write about it, and this is what this article is about, and what we are going to see right now.

## Solution

First of all, let's talk about the tools we are going to use in this solution:
- [Conan](https://conan.io/), an open-source package manager
- [CMake](https://cmake.org/), an open-source system managing build system and processes
- [SFML](https://www.sfml-dev.org/index-fr.php), a Simple and Fast Multimedia Library

with those tools, we can, with two files, generates a program able to play an audio file.

First, we need to specify to `conan` that we want to integrate the sfml library, and more specifically its audio module, via the `conanfile.txt` file:

```txt
[requires]
sfml/2.5.0@bincrafters/stable

[generators]
cmake_multi
```

Then, with the `CMakeLists.txt` file, we are going to bind all those tools:

```cmake
cmake_minimum_required(VERSION 3.18)

project(Project)

# Get the Conan executable
find_program(
  CONAN_EXE
  NAMES conan conan.exe
  DOC "Conan executable" REQUIRED)

# Install the conan packages and specify the audio module of the sfml library installation
execute_process(
  COMMAND ${CONAN_EXE} install . --install-folder=build --build=missing -s build_type=Debug -o sfml:audio=True
  WORKING_DIRECTORY ${PROJECT_SOURCE_DIR})
execute_process(
  COMMAND ${CONAN_EXE} install . --install-folder=build --build=missing -s build_type=Release -o sfml:audio=True
  WORKING_DIRECTORY ${PROJECT_SOURCE_DIR})

include(${PROJECT_SOURCE_DIR}/build/conanbuildinfo_multi.cmake)
conan_basic_setup(TARGETS)

# Create the target
add_executable(program_name src/main.cpp)
set_property(TARGET program_name PROPERTY CXX_STANDARD 17)

# Links the target and the sfml library
target_link_libraries(program_name PUBLIC CONAN_PKG::sfml)
```

And finally, in a few C++ lines, we can play a specific audio file:

```c++
#include <SFML/Audio.hpp>
#include <cassert>
#include <filesystem>
#include <iostream>
#include <thread>

int main()
{
  sf::Music music;

  std::filesystem::path p ("C:/absolute/path/to/the/audio/file.wav");
  assert(std::filesystem::exists(p));

  music.openFromFile(p.string());
  music.play();

  // Wait for the music to finish, or the program would finish before hearing any sound.
  std::this_thread::sleep_for(std::chrono::seconds(5));
  return 0;
}
```

This is, at the time I am writing this article, the smallest project I have found to be able to play an audio file in a cross platform executable.
If you have any other solution or improvement, please share it with me, I will be more than happy to read it 🙂

-----------

Thank you all for reading this article,
And until my next article, have a splendid day 😉

## Interesting links

- [std::filesystem::path documentation](https://en.cppreference.com/w/cpp/filesystem/path)
- [std::filesystem::exists documentation](https://en.cppreference.com/w/cpp/filesystem/exists)
- [sf::Music class documentation](https://www.sfml-dev.org/documentation/2.5.1/classsf_1_1Music.php)
- [Soundimage website](https://soundimage.org/)
- [SFML website](https://www.sfml-dev.org/)
- [CMake website](https://cmake.org/)
- [Conan website](https://conan.io/)
- [Play an audio file in Python](https://realpython.com/playing-and-recording-sound-python/)
- [Play an audio file in Javascript](https://stackoverflow.com/questions/9419263/how-to-play-audio)

