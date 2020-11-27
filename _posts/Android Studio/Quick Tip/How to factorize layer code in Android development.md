# How to factorize layer code in Android development

Hello ! I'm Xavier Jouvenot and in this small post, we are going to see how to factorize layer code in Android development.

_Self promotion_: You can find other articles on my [website](www.10xlearner.com) 😉

## Problematic

When developing an application, you may have some Activities, looking a lot alike, and this can, and will lead to a lot of duplicated layout description, or at least with layouts that are very similar from one Activity to the other.

Wouldn't be great if we had a way to factorize such layout to make it easily reusable, like we factorize code into functions ?

Well, this blog post would not be here if there was none, so let's take a look at the solution 😉

## Solution

In Android layout, there is a handy tag named [include](https://developer.android.com/training/improving-layouts/reusing-layouts), which does exactly what we want to.
It allows us to create a "generic" layout and to reuse it in another layout like that:

```xml
<LinearLayout>

  <include layout="@layout/layout_name"/>

</LinearLayout>
```

In this simple example, we are including the layout named `layout_name` inside another layout.
Every modification in the file describing `layout_name` will modify the elements everywhere it's included, like in the `LinearLayout` of our example.
That way, we can code our layout once and reuse it everywhere 🙂

----------

Thank you all for reading this article,
And until my next article, have a splendid day 😉

## Interesting links

- [Include tag documentation](https://developer.android.com/training/improving-layouts/reusing-layouts)

