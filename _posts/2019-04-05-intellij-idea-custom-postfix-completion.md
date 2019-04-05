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

As you can surely guess, IDEA will replace the `$EXPR$` token with the expression being wrapped.
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
Assert.assertEquals(expected, actual);
```

Another shortcoming I have come across is, there appears to be no way to configure
the completions to automatically add the static method import so it would actually
result in my preferred style:

```
import static org.junit.Assert.assertEquals;

...

assertEquals(expected, actual);
```

## Plugin to the rescue

The good news are, there is a [Custom Postfix Templates plugin](https://plugins.jetbrains.com/plugin/9862-custom-postfix-templates)
that offers lot more:

- Plenty of predefined completions that are sorted into groups so one can
decide which to use and which to ignore. Not using Mockito? Not a problem, its
completions can be hidden easily.
- Custom completions are declared in a text format and fed in as a local file or URL.
The completions can be shared with the team and versioned. This is exactly how
predefined completions are declared and distributed so they can be customized and studied.
- And of course, the expressions are more powerful.

With no intention to transcribe the [project documentation](https://github.com/xylo/intellij-postfix-templates#kinds-of-template-files), here is how our completions evolved:

```
.at : Hamcrest assertThat
  NON_VOID → org.hamcrest.MatcherAssert.assertThat($expr$, $expected$); [USE_STATIC_IMPORTS]

.ae : JUnit assertEquals
  NON_VOID → org.junit.Assert.assertEquals($expected$, $expr$); [USE_STATIC_IMPORTS]
```

The two completions, both with human readable description displayed in suggestion
dropdown before use, are applicable in same context: `non-void` expression. With this plugin,
in both cases, static method import is added and `$expected$` parameter is where the caret
is placed after application. From here it is all similar to Live Templates in
supporting multiple variables and the smart navigation while entering them.
