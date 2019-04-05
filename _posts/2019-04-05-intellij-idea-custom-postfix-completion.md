---
layout: post
title: IntelliJ IDEA custom postfix completion
author: olivergondza
tags: productivity java IDEA
---

I have no idea how could I possibly miss that one of my favorite features of IntelliJ
IDEA is actually user-configurable. The postfix completion itself permits one to
transform expression on the left side from the caret into different code block
without jumping back and forth. One of the predefined completions is turning

```java
co.nonnull<TAB>
```

into

```java
if (co != null) {
    |
}
```

While this is pretty neat with plenty of predefined completions, adding custom
is just an incredible productivity boost. Navigate to _File_ > _Settings_ > _Editor_ >
_General_ > _Postfix Completion_ to see what is available, and scratch the itch
that has been bugging you for so long. It is a pity one can not explore how predefined
completions are implemented, but it is fairly easy to get started without that.

Here is a couple of must-haves I added instantly:

```
$EXPR$.at -> org.hamcrest.MatcherAssert.assertThat($EXPR$, |);
$EXPR$.ae -> org.junit.Assert.assertEquals(|, $EXPR$);
```

As you can surely guess, IDEA will add the static import (presumably according to
IDE preferences) and replaces the `$EXPR$` token with the expression being wrapped.
None of this is really a discovery except for the fact, the official [documentation](https://www.jetbrains.com/help/idea/settings-postfix-completion.html)
does not mention how to declare desired caret position _after_ the template has been
applied. If left unspecified, the caret is placed behind the resulting declaration which is
rarely the desired thing. Fortunately, the cure is mentioned in the announcement blog
post for IDEA sibling project: [PhpStorm](https://blog.jetbrains.com/phpstorm/2018/07/custom-postfix-completion-templates/).

So these are the declarations, applicable on `non-void` expressions, that will
permit user to fill in the missing arguments right away:

```
org.hamcrest.MatcherAssert.assertThat($EXPR$, $END$);
org.junit.Assert.assertEquals($END$, $EXPR$);
```
So typing `actual.ae<TAB>expected` results into:

```
import static org.junit.Assert.assertEquals;

assertEquals(expected, actual);
```

Btw, there is a [plugin](https://plugins.jetbrains.com/plugin/9862-custom-postfix-templates)
that adds more powers we know from live templates to custom postfix completions.
Can't wait to give that a try!
