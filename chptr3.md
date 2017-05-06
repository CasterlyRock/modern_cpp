## Item 7 - Distinguish between `()` and `{}` when creating objects
### braced initialization
- _braced initialization_ is also called _uniform initialization_: a single initialization syntax that can be used anywhere and express everything.
```
class Widget {
  private:
    int x{ 0 };  // fine
    int y = 0; // fine
    int z(0);    // error!
};
```

```
//initialization of uncopyable objects
std::atomic<int> a1{ 0 };     //fine
std::atomic<int> a2(0);       //fine
std::atomic<int> a3 = 0;      //error!
```

- _braced initialization_ also prohibits implicit _narrowing conversions_ among built-in types.
```
double x, y, z;
...
int sum { x + y + z};   //error! sum of double may not expressed as int
int sum2 = x + y + z;   //fine
int sum3(x + y +z);     //fine
```
- __most vexing parse__: treat any construct that could be a declaration as a declaration
```
struct S {
  int a;
};
double a1 = 1.2;
S x(int(a1));   //will treat this as a function decalration
```
- But _braced initialization_ is immune to _most vexing parse_
- _braced initialization_ also has drawbacks: `std::initializer_list<T>`, `auto`

> Braced initialization is the most widely usable initialization syntax, it prevents narrowing conversions, and it’s immune to C++’s most vexing parse.

> During constructor overload resolution, braced initializers are matched to std::initializer_list parameters if at all possible, even if other constructors offer seemingly better matches.

> An example of where the choice between parentheses and braces can make a significant difference is creating a std::vector<numeric type> with two arguments.

## Item 8 - Prefer `nullptr` to `0` and `NULL`
- Main reason not use `0` and `NULL` as null pointer in initialzation is that `0` and `NULL` is not pointer type, if the fucntion has overload of int type, use `0` will call that overload function
- `nullptr` does not have int type, so it can avoid overload resolution surprise.

> Avoid overloading on integral and pointer types

## Item 9 - Prefer alias declarations to `typedef`s
- `typedef`s are sooooo C++98, use alias in C++11
```
using UPtrMapSS = 
    std::unique_ptr<std::unordered_map<std::string, std::string> > >;
```

- alias is easier the swallow than `typedef`
```
typedef void (*FP)(int, const std::string&);
using FP = void(*)(int, const std::string&);
```

- `typedef`s don't support templatization, but alias declarations do.
- C++11 provides some tools to perform tansformations in the form of _type traits_
> C++14 offers alias templates for all the C++11 type traits transformations.

```
// C++11
std::reomve_const<T>::type      //yield T from const T
std::remove_reference<T>::type  //yiled T from T& and T&&
std::reomve_lvalue_reference<t>::type  //yield T& from T
//C++14
std::remove_const_t<T>
std::remove_reference_t<T>
std::remove_lvalue_reference_t<T>
```

## Item 10 - Prefer scoped `enum`s to unscoped `enum`s
- using `enum class` for _scoped enum_
- 


## Item 11 - Prefer deleted functions to private undefined ones
- C++ can define copy constructor and copy assignment for you. But sometimes you do not want user to use these function.
- In C++98, the way to prevent this is to make these functions __private__ and not implement them.
- In C++11, the way to prevent this is to `delete` these functions
- By convention, deleted functions are declared `public`, so that user who want to use this function will find the compilation error as _deleted_ instead of _private_ (better error message)
- An advantage of delete function is that __any__ function can be deleted, not just member function (while only member function can be `private`)
- Another advantage is that we can disable certain type for template functions.

```
template<typename T>
void processPointer(T *ptr);
// pointer has two special cases: 
// 1. void pointer: it cannot be dereferenced, cannot increment or decrement. 
// 2. char* : it represents pointer to C-style strings, not pointers to individual characters
// so we want to disable processPointer for these two types
template<>
void processPointer<void>(void *ptr);
template<>
void processPointer<char>(char *ptr);
```

## Item 12 - Declare overriding functions `override`
- use keyword `override` at the end of function declaration to avoid wrong derived class overriding.
- Member function reference qualifiers make it possible to treat lvalue and rvalue objects (*this) differently

## Item 13 - Prefer `const_iterator`s to `iterator`s

## Item 14 - Declare functions `noexcept` if they won't emit exceptions
```
int f(int x) throw();  // no exception from f: C++98 style, less optimizable
int f(int x) noexcept();  // no exception from f: C++11 style, most optimizable
```

## Item 15 - Use `constexpr` whenever possible
- It is known that `constexpr` is a `const` and is determined during compilation time (technically translation time)
- So the value of `constexpr` will be stored in read-only memory.
- `const` objects need not be initialized with values known during compilation.
- __All__ `constexpr` __objects__ are `const`, not all `const` objects are `constexpr`
- 


