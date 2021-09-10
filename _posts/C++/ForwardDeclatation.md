# What are Forward declarations and why you should use them whenever possible !

Hello ! I'm Xavier Jouvenot and in this small post, we are going to se what forward declarations are, and why you probably want to use them in your projects.

_Self promotion_: You can find other articles on my [website](www.10xlearner.com) 😉

## What is a forward declaration ?

In C++, you may have encounter such situations, a line in a header file looking like `class ClassName`.
This is a form of C++ Forward Declaration.

As wonderfully defined in the great [cppreference site](https://en.cppreference.com/cpp/language/class):
> Forward Declaration declares a class type which will be defined later in this scope.

So it is basically a way to tell the computer: "This is a class type, don't worry for now, you will know about it later 😉"

## Why I should use those ?

There are several use cases for Forward Declarations:

1. The first one and probably the reason why this feature exists, is to offer a solution to double inclusion in the header files. When 2 classes need on another to be defined, you can forward them in the header file of the other to avoid compilation errors. It looks something like that:
```c++
class A; // Forward declaring the class type "A" as the class B need to know about it.

class B {
  A var;
};

class A {
  B var;
};
```

2. Another use for forward declaration can be to improve the compilation time by reducing the number of include in header files. This is something that I personnally use pretty often ! When a class "b" has only references or pointer to a specific type "A", you can forward declare "A" in the header file of "B", and remove the `include` instruction associated with "A" from the header file of the class "B" and move the include in the implementation file of the class "B" which probably need it to compile. With the file include of the class "A" removed from your header file of "B", the compiler won't have to include the file "A" in every compilation unit where the header file of "B" is included.

```c++
// in the header file of the class B
class A;

class B {
  A& a1;
};

// in the cpp file of the class B
#include "A.h"
```

3. There are also other use case for forward declaration but they are, for me less interesting and not common enough to be mentionned here, as you probably encounter those. But if you are interested, you can look them up [here, on the cppreference website](https://en.cppreference.com/cpp/language/class)

## Other forward declaration examples

In the previous part, I have shown you some forms of Forward Declarations, but they can vary a little bit depending on which type you want to forward declare and in which scope they are.

In the following examples, I am going to only give you the lines which should replace the `class ClassName;` instructions for each use cases.

#### Forward declaration for classes in specific namespace

```
namespace NamespaceName {
class ClassName;
}
```

#### Forward declaration for template classes

```
templace <typename> // or <typename, typename> : you can add as much typename keywords as the template need
class ClassName;
```

#### Forward declaration for inner class (class inside another class)

This is not possible outside of the class, but you can use a forward declaration inside a class definition if you need it, for between the inner classes.

```
class A {
  class C;

  class B{
   // Use of class C
  };

  class C{
  };
};
```

#### Add forward declaration for struct

```
struct StructName;
```

#### Add forward declaration for free functions

In this case, all you have to do is to use the function prototy as a forward declaration

```
void functionName();
```

#### Add forward declaration for free functions in a specific namespace

```
namespace NamespaceName{
void functionName();
}
```

#### Add forward declaration for enum class

```
enum class EnumName;
```

#### Anymore ?

To my knowledge, there is not other use case where you can forward declare a type or a function.
But if you know any other, I will be very happy to hear about it, and make this article more accurate. 😉

-------------------

Thank you all for reading this article,
And until my next article, have a splendid day 😉

## Interesting links

- [Class declaration - C++ standard documentation](https://en.cppreference.com/cpp/language/class)
- [Forward declaration wikipedia definition](https://en.wikipedia.org/wiki/Forward_declaration)
- [Microsoft - Declaration and definitions (C++)](https://docs.microsoft.com/en-us/cpp/cpp/declarations-and-definitions-cpp)
