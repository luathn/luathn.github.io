---
title: "Tuple are immutable but can contain mutable objects"
---
```py
>>> t = 1, 2, 3
>>> t[2] = 4
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'tuple' object does not support item assignment
```

```py
>>> t = ([1, 2, 3], [4, 5, 6])
>>> t[0][1] = 10
>>> t
([1, 10, 3], [4, 5, 6])
>>>
```
