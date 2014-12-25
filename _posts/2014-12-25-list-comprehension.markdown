---
layout: post
title: "List comprehension"
date: 2014-12-25
comments: true
categories: programming
tags: programming python erlang list comprehension
---

Python provides an intersting syntax sugar for generating lists.  
Recently learning Erlang syntax, I found the same feature in different syntax.  
Put them here helps me remember the name of this feature and syntaxes in different languages.  

Python
------
```python
In [1]: origin = [1,2,3,4,5]

In [2]: origin
Out[2]: [1, 2, 3, 4, 5]

In [3]: [n*2 for n in origin if n > 1 and n < 5]
Out[3]: [4, 6, 8]
```

Erlang
------
```erlang
1> Origin = [1,2,3,4,5].
[1,2,3,4,5]

2> Origin.
[1,2,3,4,5]

3> [L*2 || L <- Origin, L > 1, L < 5].
[4,6,8]
```
