# GIL

Global Interpreter Lock

### Why GIL?

> Python uses reference counting for memory management. It means that objects created in Python have a reference count variable that keeps track of the number of references that point to the object. When this count reaches zero, the memory occupied by the object is released.  
> ...  
> The problem was that this reference count variable needed protection from race conditions where two threads increase or decrease its value simultaneously.  
> ...  
> The GIL is a single lock on the interpreter itself which adds a rule that execution of any Python bytecode requires acquiring the interpreter lock. This prevents deadlocks (as there is only one lock) and doesn’t introduce much performance overhead. But it effectively makes any CPU-bound Python program single-threaded.  

### The impact on multi-threaded Python programs

> In the multi-threaded version the GIL prevented the CPU-bound threads from executing in parellel.  
> The GIL does not have much impact on the performance of I/O-bound multi-threaded programs as the lock is shared between threads while they are waiting for I/O.

## reference

https://realpython.com/python-gil/
