---
layout: post
title: Bash strict mode and why you should care
author: olivergondza
tags: bash, productivity
---

It has been a while since I have stumbled upon a great post of Aaron Maxwell introducing
what he refers to as ["Unofficial Bash Strict Mode"](http://redsymbol.net/articles/unofficial-bash-strict-mode/)
and started taking advantage
of its benefits on everyday bases. It has even become part of my bash script
template so I never (well, almost) create a new file without a strict mode header.

## So, what is strict mode?

I am certain it is no easy task to design a language to serve the purpose of
interactive shell and well as the one of nontrivial system programing language. However,
over the years, one is constantly running into situations wishing bash would be more
strict, predictable and generally resemble more of a, well, more of a programming language.

Fortunately, there are ways to get a little closer to that to protect ones sanity
while working with what I (only half jokingly, though) call the most popular esoteric
programming language in the world. And to the best of my knowledge, Aaron was the
first one to popularize the concept of strict mode in bash.

So all in all, the strict mode is nothing more than a small bash snippet to put at
the beginning of the file to alter is behavior towards the safe side. Here is his
proposed strict mode declaration:

```bash
#!/bin/bash
set -euo pipefail
IFS=$'\n\t'
```

Let me have a brief look of what it attains (see the original post for a ton of
relevant details):

- `set -u`: Error out in case an undefined variable is substituted. While this is
not always an error as a fair deal of correct code can be written while substituting undefined
variables, typos or messed up refactorings manifests this way fairly often.
- `set -e`: Exit when any executed command return non-zero code. The obvious exception
here are commands executed inside loop conditions, conditionals or as arguments
to logic operators (`||`, `&&`).
- And its good friend `set -o pipefail` that aborts command [pipeline](https://www.gnu.org/software/bash/manual/html_node/Pipelines.html) as soon as some of its chained commands fails. This pair of settings attempts to address one of
the most frequent problems I have come across with bash scripting: scripts continuing execution
after some critical operation failed, with some of script invariants violated and
either failing much later or even completing with success exit code in the end.

So this is the way to ask for extra portion of errors and constrains to handle
while working with bash. Which, trust me, is a good thing. The original post gets
in some lengths on how to adapt common code patterns that does not
comply to work with the strict mode, and it only seldom makes the code less idiomatic
while making it more resilient most of the time.

The very last line adjusts the "Internal Field Separator" to behave more intuitively
around [word splitting](https://www.gnu.org/software/bash/manual/html_node/Word-Splitting.html)
strings that contain whitespace, where spaces often behaves as _undesired_
separators in buggy code.

## Is that it?

Aaron of course is not alone striving for seamless bash experience so more strict
modes has emerged. [Michael Daffin](https://disconnected.systems/blog/another-bash-strict-mode/)
has published one I really love:

```bash
#!/bin/bash
set -uo pipefail
trap 's=$?; echo "$0: Error on line "$LINENO": $BASH_COMMAND"; exit $s' ERR
IFS=$'\n\t'
```

It appears to be an alternation of the previous one (albeit it omits `set -e`),
but, what's with the `trap`? [Traps](http://tldp.org/LDP/Bash-Beginners-Guide/html/sect_12_02.html)
in bash registers a (string) code snippet to be executed when script exits with
declared error code, or as in the case of `ERR`, when a command returns non-zero
code (same exceptions apply as with `set -e`). I find this to be a true gem when used with `set -e` and `set -o pipefail` that
alone just abort the script, but does not tell you _where_ it failed. With this, it clearly reports that script
has failed (which you may miss unless you examine the exit code) stating the line number and the command
that failed! This makes it fail fast, _and_ loud.

## Are we saved?

Well, not quite. While all these contribute to the effort of maintaining developer's
sanity, they come to a cost. Apart from caveats covered in [Aaron's original post](http://redsymbol.net/articles/unofficial-bash-strict-mode/),
watch out for these:

- Composing advanced strict modes is not only making bash a language that is more sane, is also getting further
from the collective experience with the tool. Think of the team members surprised
by the cryptic noise at the beginning of the files, not even talking about their
reaction when they notice the change of the semantics of the code they though they
understand. This also impact all the documentation and reusable code snippets that
becomes less relevant as they silently imply default behavior. Who have ever heard of
a piece of code copied from Stack Overflow that would not work as intended?😱

- The `IFS` thing. The way I look at this, the other tweaks adds safety by refusing
to tolerate situations that can and arguably should be avoided (like using undefined
variable). But changing the `IFS` not to contain space character is merely changing
the semantics to compensate one of the frequent mistakes while working with
variable substitution. It does not force one to write _better_ code, it tolerates
_bad_ code instead and try to get correct(-ish) results.

- One hideous caveat of bash `trap` usage (the one people usually learn about the
hard way) is that multiple traps are not stacked and executed in sequence, but they
replace previously defined trap per given signal. Put differently, as soon
as one declare another trap bound to `ERR`, the diagnostics provided by the strict
mode declaration get replaced. Fortunately, there is not much motivation to introduce
`ERR` traps for other reason but diagnostics, so I do not find this very likely
to collide in production code

## Which one to use, then?

So, I ended up cultivating my [own strict mode](https://github.com/olivergondza/bash-strict-mode)
that, to this day, looks something like this:

```bash
#!/usr/bin/env bash
set -euo pipefail
trap 's=$?; echo >&2 "$0: Error on line "$LINENO": $BASH_COMMAND"; exit $s' ERR
```


- Always using `/usr/bin/env` to locate `bash` interpretter.
- `set -e` is not going anywhere.
- The trap prints to standard error.
- The `IFS` is not tempered with.

Note that `set -euo pipefail` part (and its subsets) has become quite notorious
and a lot of folks is using variations of that sporadically. One advantage of
using _consistent_ strict mode definition is all the scripts are behaving predictably.
It is enough we have chosen to deviate from bash defaults, let's at least be consistent in doing so.

Isn't it time to start thinking about bash strict mode in your life?
