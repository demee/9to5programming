---
tags:
- bash
- " tests"
- " unit"
layout: post
title: Unit testing bash scripts
date: 2019-10-15 23:00:00 +0000

---
Today I had to write some more complicated than usual scripts in bash. For a moment I might do them in ruby as bash is hard to test, but I found out that is not!

I found [assert.sh](https://github.com/lehmannro/assert.sh "assert.sh") script that allowed me to test my bash scripts effectively. 

Here is an example. I wrote a 
Function in bash that will fail my pipeline if coverage drops. I pass two values to it, one with the current value of coverage and second with value after my changes: 

```bash
#!/usr/bin/env bash

. functions.sh
. assert.sh

# check_coverage
assert_raises "check_coverage 30 20" "0"
assert_raises "check_coverage 30 30" "0"
assert_raises "check_coverage 30.1 40.2" "1"
assert_end
```

Running the tests is very simple: 

```bash 
➜  simple-coverage git:(master) ./tests.sh 
all 3 tests passed in 0.000s.
```

Now I can write more complex bash scripts and make sure they work as I expect them too. It's great! 

