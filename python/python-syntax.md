# Syntax in Python
A note of some python syntax which I found it's very useful.

### List operation
* zip: This function returns a list of tuples, where the i-th tuple contains the i-th element from each of the argument sequences or iterables. The returned list is truncated in length to the length of the shortest argument sequence. When there are multiple arguments which are all of the same length, zip() is similar to map() with an initial argument of None. With a single sequence argument, it returns a list of 1-tuples. With no arguments, it returns an empty list.
```python
x = [1,2,3]
y = [4,5,6,7]
zipped = zip(x,y)
"""print zipped: [(1,4),(2,5),(3,6)]"""
for a,b in zip(x,y):
  if a == b:
    print "same"
```

* join list:
```python
a = [1,2,3]
b = [4,5,6]
print a+b # [1,2,3,4,5,6]
```

### Object Oriented
* call super class method
```python
# Where m() is a method from the base class.
class Derived(Base):
   def m(self):
       return super(Derived, self).m()
```

* represent object: similar to Java toString()
```python
def __repr__(self):
  return "(%d, %d, %d)" % (self.x, self.y, self.z)    
```
