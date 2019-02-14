## Common operators to overload

[refernce](https://stackoverflow.com/questions/4421706/what-are-the-basic-rules-and-idioms-for-operator-overloading)

Most of the work in overloading operators is boiler-plate code. That is little wonder, since operators are merely syntactic sugar, their actual work could be done by (and often is forwarded to) plain functions. But it is important that you get this boiler-plate code right. If you fail, either your operator’s code won’t compile or your users’ code won’t compile or your users’ code will behave surprisingly.

---
### Assignment Operator
There's a lot to be said about assignment. However, most of it has already been said in GMan's famous Copy-And-Swap FAQ, so I'll skip most of it here, only listing the perfect assignment operator for reference:
```cpp
X& X::operator=(X rhs)
{
  swap(rhs);
  return *this;
}
```
Bitshift Operators (used for Stream I/O)
The bitshift operators << and >>, although still used in hardware interfacing for the bit-manipulation functions they inherit from C, have become more prevalent as overloaded stream input and output operators in most applications. For guidance overloading as bit-manipulation operators, see the section below on Binary Arithmetic Operators. For implementing your own custom format and parsing logic when your object is used with iostreams, continue.  
The stream operators, among the most commonly overloaded operators, are binary infix operators for which the syntax specifies no restriction on whether they should be members or non-members. Since they change their left argument (they alter the stream’s state), they should, according to the rules of thumb, be implemented as members of their left operand’s type. However, their left operands are streams from the standard library, and while most of the stream output and input operators defined by the standard library are indeed defined as members of the stream classes, when you implement output and input operations for your own types, you cannot change the standard library’s stream types. That’s why you need to implement these operators for your own types as non-member functions. The canonical forms of the two are these:
```cpp
std::ostream& operator<<(std::ostream& os, const T& obj)
{
  // write obj to stream
  return os;
}

std::istream& operator>>(std::istream& is, T& obj)
{
  // read obj from stream

  if( /* no valid object of T found in stream */ )
    is.setstate(std::ios::failbit);
  return is;
}
```
When implementing operator>>, manually setting the stream’s state is only necessary when the reading itself succeeded, but the result is not what would be expected.

---
### Function call operator
The function call operator, used to create function objects, also known as functors, must be defined as a member function, so it always has the implicit this argument of member functions. Other than this it can be overloaded to take any number of additional arguments, including zero.

Here's an example of the syntax:
```cpp
class foo {
public:
    // Overloaded call operator
    int operator()(const std::string& y) {
        // ...
    }
};
Usage:

foo f;
int a = f("hello");
```
Throughout the C++ standard library, function objects are always copied. Your own function objects should therefore be cheap to copy. If a function object absolutely needs to use data which is expensive to copy, it is better to store that data elsewhere and have the function object refer to it.

---
### Comparison operators
The binary infix comparison operators should, according to the rules of thumb, be implemented as non-member functions[^1]. The unary prefix negation ! should (according to the same rules) be implemented as a member function. (but it is usually not a good idea to overload it.)

The standard library’s algorithms (e.g. std::sort()) and types (e.g. std::map) will always only expect operator< to be present. However, the users of your type will expect all the other operators to be present, too, so if you define operator<, be sure to follow the third fundamental rule of operator overloading and also define all the other boolean comparison operators. The canonical way to implement them is this:
```cpp
inline bool operator==(const X& lhs, const X& rhs){ /* do actual comparison */ }
inline bool operator!=(const X& lhs, const X& rhs){return !operator==(lhs,rhs);}
inline bool operator< (const X& lhs, const X& rhs){ /* do actual comparison */ }
inline bool operator> (const X& lhs, const X& rhs){return  operator< (rhs,lhs);}
inline bool operator<=(const X& lhs, const X& rhs){return !operator> (lhs,rhs);}
inline bool operator>=(const X& lhs, const X& rhs){return !operator< (lhs,rhs);}
```
The important thing to note here is that only two of these operators actually do anything, the others are just forwarding their arguments to either of these two to do the actual work.

The syntax for overloading the remaining binary boolean operators (||, &&) follows the rules of the comparison operators. However, it is very unlikely that you would find a reasonable use case for these[^2].

[^1]: As with all rules of thumb, sometimes there might be reasons to break this one, too. If so, do not forget that the left-hand operand of the binary comparison operators, which for member functions will be *this, needs to be const, too. So a comparison operator implemented as a member function would have to have this signature:
```cpp
bool operator<(const X& rhs) const { /* do actual comparison with *this */ }
```
(Note the const at the end.)

[^2]: It should be noted that the built-in version of || and && use shortcut semantics. While the user defined ones (because they are syntactic sugar for method calls) do not use shortcut semantics. User will expect these operators to have shortcut semantics, and their code may depend on it, Therefore it is highly advised NEVER to define them.

---
### Arithmetic Operators
Unary arithmetic operators
The unary increment and decrement operators come in both prefix and postfix flavor. To tell one from the other, the postfix variants take an additional dummy int argument. If you overload increment or decrement, be sure to always implement both prefix and postfix versions. Here is the canonical implementation of increment, decrement follows the same rules:
```cpp
class X {
  X& operator++()
  {
    // do actual increment
    return *this;
  }
  X operator++(int)
  {
    X tmp(*this);
    operator++();
    return tmp;
  }
};
```
Note that the postfix variant is implemented in terms of prefix. Also note that postfix does an extra copy.[^3]

Overloading unary minus and plus is not very common and probably best avoided. If needed, they should probably be overloaded as member functions.

[^3]: Also note that the postfix variant does more work and is therefore less efficient to use than the prefix variant. This is a good reason to generally prefer prefix increment over postfix increment. While compilers can usually optimize away the additional work of postfix increment for built-in types, they might not be able to do the same for user-defined types (which could be something as innocently looking as a list iterator). Once you got used to do i++, it becomes very hard to remember to do ++i instead when i is not of a built-in type (plus you'd have to change code when changing a type), so it is better to make a habit of always using prefix increment, unless postfix is explicitly needed.

---
### Binary arithmetic operators
For the binary arithmetic operators, do not forget to obey the third basic rule operator overloading: If you provide +, also provide +=, if you provide -, do not omit -=, etc. Andrew Koenig is said to have been the first to observe that the compound assignment operators can be used as a base for their non-compound counterparts. That is, operator + is implemented in terms of +=, - is implemented in terms of -= etc.

According to our rules of thumb, + and its companions should be non-members, while their compound assignment counterparts (+= etc.), changing their left argument, should be a member. Here is the exemplary code for += and +, the other binary arithmetic operators should be implemented in the same way:

```cpp
class X {
  X& operator+=(const X& rhs)
  {
    // actual addition of rhs to *this
    return *this;
  }
};
inline X operator+(X lhs, const X& rhs)
{
  lhs += rhs;
  return lhs;
}
```
operator+= returns its result per reference, while operator+ returns a copy of its result. Of course, returning a reference is usually more efficient than returning a copy, but in the case of operator+, there is no way around the copying. When you write a + b, you expect the result to be a new value, which is why operator+ has to return a new value.[^4] Also note that operator+ takes its left operand by copy rather than by const reference. The reason for this is the same as the reason giving for operator= taking its argument per copy.

The bit manipulation operators ~ & | ^ << >> should be implemented in the same way as the arithmetic operators. However, (except for overloading << and >> for output and input) there are very few reasonable use cases for overloading these.

[^4]: Again, the lesson to be taken from this is that a += b is, in general, more efficient than a + b and should be preferred if possible.

---
### Array Subscripting
The array subscript operator is a binary operator which must be implemented as a class member. It is used for container-like types that allow access to their data elements by a key. The canonical form of providing these is this:
```cpp
class X {
        value_type& operator[](index_type idx);
  const value_type& operator[](index_type idx) const;
  // ...
};
```
Unless you do not want users of your class to be able to change data elements returned by operator\[\] (in which case you can omit the non-const variant), you should always provide both variants of the operator.

If value_type is known to refer to a built-in type, the const variant of the operator should return a copy instead of a const reference.

---
### Operators for Pointer-like Types
For defining your own iterators or smart pointers, you have to overload the unary prefix dereference operator * and the binary infix pointer member access operator ->:
```cpp
class my_ptr {
        value_type& operator*();
  const value_type& operator*() const;
        value_type* operator->();
  const value_type* operator->() const;
};
```
Note that these, too, will almost always need both a const and a non-const version. For the -> operator, if value_type is of class (or struct or union) type, another operator->() is called recursively, until an operator->() returns a value of non-class type.

The unary address-of operator should never be overloaded.

For operator->*() see this question. It's rarely used and thus rarely ever overloaded. In fact, even iterators do not overload it.