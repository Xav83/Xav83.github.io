# Quick Tip - How to convert a Java jintArray to a C++ std::vector<jint>

Hello ! I'm Xavier Jouvenot and in this small post, I am going to explain how to convert a Java `jintArray` to a C++ `std::vector<jint>`.

_Self promotion_: You can find other articles on Android development on my [website](www.10xlearner.com) 😉

## Solution

For the people who only want the solution, to quickly copy-paste it in there code, here it is 😉

```C++
extern "C"
JNIEXPORT jstring JNICALL
Java_com_example_project_class_method(JNIEnv *env, jobject /* this */, jintArray arr) {
jsize size = env->GetArrayLength( arr );
std::vector<jint> input( size );
env->GetIntArrayRegion( arr, jsize{0}, size, &input[0] );
```

## Explanation

If you read this, it may be that you want to understand how the previous solution achieve the goal of transforming the `jintArray` into a `std::vector<jint>`, and this is what I am going to try to explain. 🙂 

First of all, we get the `jintArray` size by using the JNIEnv method `GetArrayLength`, and store this information in a variable.

Then, with this information, we can create a [std::vector](https://en.cppreference.com/w/cpp/container/vector) with the right size.

And finally, we copy the elements in from the `jintArray` to the `std::vector` using the JNIEnv method `GetIntArrayRegion`, by giving it all the informations that it need : the `jintArray`, the index where to start the copy, the size of the array, and the location where to copy the array.

--------------

Thank you all for reading this article,
And until my next article, have a splendid day 😉

## Interesting links

- [JNI documentation](https://docs.oracle.com/javase/7/docs/technotes/guides/jni/spec/functions.html)
- [JNI types documentation](https://docs.oracle.com/javase/7/docs/technotes/guides/jni/spec/types.html)
- [std::vector documentation](https://en.cppreference.com/w/cpp/container/vector)
- [10xlearner website](www.10xlearner.com)
