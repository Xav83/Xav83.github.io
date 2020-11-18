# How to use alternative operators in C++ with Visual Studio

I'm Xavier Jouvenot and in this small post, we are going to see how to use alternative operators in C++ with Visual Studio.

_Self promotion_: You can find other articles on my [website](www.10xlearner.com) :wink:
😄
## Problematic

In C++, as you may already know, there are primary operators available to us since the beginning of C++, as they are inherited from the C language.
For example, the operator `&&` for an **and** condition, or the operator `||` for an **or** condition. 

But there also a set of alternative operators that is available to us, and which can be much more friendly that the primary operators.
Indeed, instead of using two times the character `&` to have an operator **and**, the C++ language allows us, with those alternative operators, to directly type the word `and`.
Same goes for the primary operator `||` which can be replaced by the the word `or`.

If you want to find the whole list of alternative operators, you can follow this [link](https://en.cppreference.com/w/cpp/language/operator_alternative) to the C++ documentation

As cool as it can seem to have a C++ looking more like english, and being more expressive in its intention, those alternative operators are sadly not as much used as they should.
Indeed, if you go follow any course on C++, you will find everyone still teaching `&&` and `||` operators when they could directly introduce `and` and `or` operators! :angry:
Well... I digress, this is not the subject of this blog post :laughing:

To come back to the alternative operators, they are sadly not automatically integrated on every main computer :cry:
With gcc or clang, no problem, the alternative operators are directly recognized, but sadly, in Visual Studio (with the msvc compiler), they are not automatically recognized.
You can see on this [godbolt example](https://godbolt.org/z/TPK9zG) the compilation error triggered in Visual Studio.

But we are going to make that possible :wink:

## Solution

In order to Visual Studio have interpreting C++ correctly the alternatives operators, we need to specify an particular option named [permissive](https://docs.microsoft.com/fr-fr/cpp/build/reference/permissive-standards-conformance?view=msvc-160). To pass it to a Visual Studio project, you have to it like that:

```
/permissive-
```

And with that option, you can finally have our [godbolt example](https://godbolt.org/z/ebefaz) working ! :smile:

You can note that this option works since Visual Studio 2017 and since "Visual Studio 2017 version 15.5", it is added in all project by default accordingly to the documentation.

Juste like that such code will be understood by all the main compiler :wink:
```
if(question == "The Ultimate Question of Life, the Universe, and Everything" and answer == 42)
{
    return "H2G2";
}
```

-----------

Thank you all for reading this article,
And until my next article, have a splendid day :wink:

## Interesting links

- [Alternative operators documentation](https://en.cppreference.com/w/cpp/language/operator_alternative)
- [Permissive flag documentation][https://docs.microsoft.com/fr-fr/cpp/build/reference/permissive-standards-conformance?view=msvc-160]
- [C++ keywords](https://en.cppreference.com/w/cpp/keyword)
- [First](https://godbolt.org/z/TPK9zG) and [second](https://godbolt.org/z/ebefaz) godbolt examples
- [H2G2](https://amzn.to/34GEFV4)

