# Advent Of Code - The Ideal Stocking Stuffer - Puzzle 4

Hello ! I'm Xavier Jouvenot and here is the third part of a long series on [Advent Of Code](https://adventofcode.com). You can find the previous part [here](2019-06-14-AdventOfCode2015-Day3-Perfectly-Spherical-Houses-in-A-Vacuum.md)

For this new post, we are going to solve the second problem from the 4th December 2015, named "The Ideal Stocking Stuffer".
The solution I will propose in C++, but the reasoning can be applied to other languages.

## Part 1

### The Problem

The full version of this problem can be found directly on the [Advent of Code website](https://adventofcode.com/2015/day/4), I will only describe the essence of the problem here:

Santa needs help mining AdventCoins, a wonderful crypto currency, to offer as preset to people.

For that, he needs to find the `MD5` hashes which, in hexadecimal, start with **at least five zeroes**. The input to the MD5 hash is some secret key followed by a decimal number. To help Santa, we must to find the lowest positive number (no leading zeroes: 1, 2, 3, ...) that produces such a hash.

For example:

If your secret key is `abcdef`, the answer is `609043`, because the MD5 hash of `abcdef609043` starts with five zeroes (`000001dbbfa...`), and it is the lowest such number to do so.

### Solution

I don't know for you, but, when I read the problem the first time, I was like : "What is happening here ? 😨". Then, I started to google some stuff, to make some sense about that.

First of all, I've looked at what [MD5](https://en.wikipedia.org/wiki/MD5) is, which, as Wikipedia explains very well, is a **widely used message-digest algorithm producing 128-bit hash value**.
After taking a look at the pseudo code of the algorithm in the Wikipedia, it comes to my mind that the **widely used** of the definition means that other people may have already implemented this algorithm. And after a bit of research, I've found that the library OpenSSL has an implementation of MD5. So, I included the OpenSSL library using Cmake and the package Manager [Conan](https://conan.io/) (I will explain how I did it in another post). And voilà, now, i can do `#include <openssl/md5.h>` 😃.

Now that we have the function md5 available, we have two things to do.
First, we have to make it easy to use. Since this algorithm is C code, we have to adapt it to use it with `std::string` more easily and to make it more readable.
So, I wrote a function `std::string getMD5(std::string key)` which does exactly that. You can look at my implementaion [here](TODO), but this is not an interesting part, for me. I will describe it, if some people reading this post wants more information about it.

Finally, we have to use this function, in order to solve the problem.

```c++
    size_t result = 0;
    while (true)
    {
        const auto str = secretKey + std::to_string(result);
        const auto md5Result = getMD5(str);
        if(md5Result.substr(0, 5) == "00000")
        {
            break;
        }
        ++result;
    }
    std::cout << result;
```

And here we go. As you can see in this solution, we look at the 5 first characters of the md5 hash, and check if there are only `0`. If this is not the case, then we try with the next number, but if this is the case, we can help Santa with the right answer 🎅.

## Part 2

### The Problem

Surprise, this is the same problem as the part 1 except to one "little" detail, we have to found the number which allow us to find the first hash starting with 6 zeros.

### Solution

To find the solution, all we have to do, is to replace one line in our code:
```c++
    // Old line
    if(md5Result.substr(0, 5) == "00000")
    // New line
    if(md5Result.substr(0, 6) == "000000")
```

And voilà, now we can compile, run our program and wait to have our solution. And we wait and wait again. Because yes, crypto currency is not fast to mine. As an example, in debug mode, it took `4.12` seconds on my machine to find the result of the first problem, but `146.18` seconds, more than 2 minutes to find the solution of the part 2 !
And in release mode, it took `3.47` seconds for the first problem and `34.24` seconds for the second one.

Of course, there are ways to improve this solution I just described, and running some benchmarks on this solution could help to mine faster, but, this is not the purpose of this post.
And even if it took more than 2 minutes, it is very satisfying to have helped Santa 😉.

## Conclusion

You can note that the solutions written in this post, don't include all the sources to make running programs, but only the interesting part of the sources to solve this problem.
If you want to see the programs from end to end, you can go on my [GitHub account]https://github.com/Xav83/AdventOfCode/tree/2015.04/2015/Day4), explore the full solution, add comments or ask questions if you want to.

Here is the list of std methods and containers that we have used, I can't encourage you enough to look at their definitions :

- [std::string::substr](https://en.cppreference.com/w/cpp/container/vector)
- [std::to_string](https://en.cppreference.com/w/cpp/algorithm/sort)
- [openssl/md.h](https://www.openssl.org/docs/man1.0.2/man3/MD5.html)

Thanks for you reading, hope you liked it 😃

And until next part, have fun learning and growing.
