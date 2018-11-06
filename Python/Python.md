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


## Create Cython lib (*.so) in the current directory

```bash
$ python setup.py build_ext --inplace
```


from python [document](https://docs.python.org/2/distutils/configfile.html)

```bash
> python setup.py --help build_ext
[...]
Options for 'build_ext' command:
  --build-lib (-b)     directory for compiled extension modules
  --build-temp (-t)    directory for temporary files (build by-products)
  --inplace (-i)       ignore build-lib and put compiled extensions into the
                       source directory alongside your pure Python modules
  --include-dirs (-I)  list of directories to search for header files
  --define (-D)        C preprocessor macros to define
  --undef (-U)         C preprocessor macros to undefine
  --swig-opts          list of SWIG command line options
[...]
```