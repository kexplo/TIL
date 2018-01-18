## Do I have to do StringIO.close() ?

reference: https://stackoverflow.com/questions/9718950/do-i-have-to-do-stringio-close

```
`StringIO.close()`: Free the memory buffer. Attempting to do further operations with a closed StringIO object will raise a ValueError.
```

```python
import StringIO, weakref

def handler(ref):
    print 'Buffer died!'

def f():
    buffer = StringIO.StringIO()
    ref = weakref.ref(buffer, handler)
    buffer.write('something')
    return buffer.getvalue()

print 'before f()'
f()
print 'after f()'
```

result:

```bash
$ python test.py 
before f()
Buffer died!
after f()
$
```

Or, use it in a `with` statement.


```python
# python 2
with contextlib.closing(StringIO()) as buffer:
    buffer.write('hello')
```

```python
# python 3
with StringIO() as buffer:
    buffer.write('hello')
```