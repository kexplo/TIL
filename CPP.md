# Modern C++

Since C++11

## Inheriting constructors

```cpp
// Old way
class A {
public:
    A() {}
    A(int var) {}
}

class B : public A {
public:
    B() : A() {}
    B(int var) : A(var) {}
}


// Since C++11
class B : public A {
public:
    using A::A; // inherit all parent's constructors
}
```

See also: https://en.cppreference.com/w/cpp/language/using_declaration

## Read a whole binary file

```cpp
std::ifstream ifs(path, std::fstream::in | std::fstream::binary);
ifs.seekg(0, ifs.end);
std::streampos length = ifs.tellg();
ifs.seekg(0, ifs.beg);

std::string buffer;
buffer.reserve(length);
buffer.assign(
    (std::istreambuf_iterator<char>(ifs)),
    (std::istreambuf_iterator<char>()));
```