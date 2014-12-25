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

Livescript
----------
```livescript
origin = [1,2,3,4,5]
console.log(origin)

result = [n*2 for n in origin when n > 1 and n < 5]
console.log(result)
```
```
$ livescript list-comprehension.livescript
[ 1, 2, 3, 4, 5 ]
[ 4, 6, 8 ]
```  
- - -
These following languages don't have this similar syntax sugar, but we can achieve it in other simple forms.

Groovy
------
```groovy
groovy:000> origin = [1,2,3,4,5]
===> [1, 2, 3, 4, 5]

groovy:000> origin
===> [1, 2, 3, 4, 5]

groovy:000> origin.findAll({it > 1}).findAll({it < 5}).collect {it*2}
===> [4, 6, 8]
```  

Scala
-----
```scala
scala> var list = List(1,2,3,4,5)
list: List[Int] = List(1, 2, 3, 4, 5)

scala> list
res0: List[Int] = List(1, 2, 3, 4, 5)

scala> for(i <- list if i > 1 && i < 5) yield i*2
res1: List[Int] = List(4, 6, 8)
```  

Ruby
----
```ruby
2.1.2 :001 > origin = [1,2,3,4,5]
 => [1, 2, 3, 4, 5]
 
2.1.2 :002 > origin
 => [1, 2, 3, 4, 5]
 
2.1.2 :003 > origin.select {|x| x > 1 and x < 5}.collect {|x| x*2}
 => [4, 6, 8]
```  
