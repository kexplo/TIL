## StringIO

```python
# python3
with StringIO() as buffer:
    buffer.write('hello')
```

```python
# python2
with contextlib.closing(StringIO()) as buffer:
    buffer.write('hello')
```
