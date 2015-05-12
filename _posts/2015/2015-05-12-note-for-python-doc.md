---
---

+ Access a dictionary element

```python
d = {'hello': 'world'}
print d.get('hello', 'default_value') # prints 'world'
print d.get('thingy', 'default_value') # prints 'default_value'

# Or
if 'hello' in d:
   print d['hello']
```