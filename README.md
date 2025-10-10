# KratosEngine
Game engine written in C and C++.

## Coding Standards
Coding Standards ***v1.0.0*** <br>
Defined below are the coding standards and best practices for KratosEngine.

### Indentation
Use 1 tab per indentation level. Only use spaces for alignment.

### Naming
<a id="camel-case"></a>[<ins>**Camel Case**</ins>](https://developer.mozilla.org/en-US/docs/Glossary/Camel_case): Write phrases without spaces, where the first letter of each word is capitalized, except for the first letter of the entire compound word. Example: ```camelCase```. <br>
<a id="pascal-case"></a>[<ins>**Pascal Case**</ins>](https://developer.mozilla.org/en-US/docs/Glossary/Camel_case): Write phrases without spaces, where the first letter of each word is capitalized. Example: ```PascalCase```. <br>
<a id="snake-case"></a>[<ins>**Snake Case**</ins>](https://developer.mozilla.org/en-US/docs/Glossary/Snake_case): Write phrases without spaces, where spaces are replaced with underscores ```_```, and the words are typically all lower case. Example: ```snake_case```. <br>
<a id="screaming-snake-case"></a>[<ins>**Screaming Snake Case**</ins>](https://developer.mozilla.org/en-US/docs/Glossary/Snake_case): Write phrases without spaces, where spaces are replaced with underscores ```_```, and the words are typically all upper case. Example: ```SCREAMING_SNAKE_CASE```. <br>

#### Namespaces
In C++, namespaces are to be written in [**Pascal Case**](#pascal-case). Example:
```cpp
This::Is::AN::Example::Namespace
```
In C, namespaces are to be represented by name prefix's. These prefix's are to be written in [**Snake Case**](#snake-case) or [**Screaming Snake Case**](#screaming-snake-case). Example: 
```c
This_Is_AN_Example_Namespace
```
In Python, namespaces are to be represented by modules. These modules name's are to be written in [**Pascal Case**](#pascal-case). Example:
```py
import This.Is.AN.Example.Namespace
```

#### Types
Types and type-names are to be written in [**Camel Case**](#camel-case). <br>
C/C++ example:
```cpp
struct ExampleStruct;
class ExampleClass;
enum ExampleEnum;
enum class ExampleEnumClass;
typedef struct ExampleStructType;
typedef class ExampleClassType;
```
Python example:
```py
class ExampleClass: ...
type ExampleType = int
```

#### Constants
Constants are to be written in [**Screaming Snake Case**](#screaming-snake-case).
Constant variables or members are to be written like normal [**Variables and Members**](#variables-and-members) but in [**Screaming Snake Case**](#screaming-snake-case). <br> 
C/C++ example:
```cpp
const void* THIS_IS_AN_EXAMPLE_CONSTANT;
struct { 
    const void* THIS_IS_AN_EXAMPLE_CONSTANT; 
}
class { 
    public: 
    const void* THIS_IS_AN_EXAMPLE_CONSTANT; 
    private: 
    const void* m_THIS_IS_AN_EXAMPLE_CONSTANT ;
}
```
Python example:
```py
THIS_IS_AN_EXAMPLE_CONSTANT: int = 0
class:
    THIS_IS_AN_EXAMPLE_CONSTANT: int = 0
    m_THIS_IS_AN_EXAMPLE_CONSTANT: int = 0
```

#### Variables and Members
Variables are to be written in [**Camel Case**](#Camel-case). Private member variables are to be prefixed with m, meaning the name is written in [**Pascal Case**](#pascal-case) and prefixed with lowercase m. <br>
C/C++ example :
```cpp
void* thisIsAnExampleVariable;
struct { 
    void* thisIsAnExamplePublicMember; 
};
class { 
    public: 
    void* thisIsAnExamplePublicMember; 
    private:
    void* mThisIsAnExamplePrivateMember
};
```
Python example:
```py
thisIsAnExampleVariable: int = 0
class:
    thisIsAnExamplePublicMember: int = 0
    mThisIsAnExamplePublicMember: int = 0
```

#### Functions
In C++ and Python, functions are to be written in [**Camel Case**](#camel-case). Private member functions are to be prefixed with m, meaning the name is written in [**Pascal Case**](#pascal-case) and prefixed with lowercase m. <br>
C++ example:
```cpp
void thisIsAnExampleFunction();
struct { 
    void thisIsAnExamplePublicFunction(); 
}
class { 
    public: 
    void thisIsAnExamplePublicFunction(); 
    private:
    void mThisIsAnExamplePrivateFunction();
}
```
Python example:
```py
def thisIsAnExampleFunction() -> int: ...
class:
    def thisIsAnExamplePublicFunction() -> int: ...
    def mThisIsAnExamplePrivateFunction() -> int: ...
```
In C, functions are to be written in [**Camel Case**](#camel-case). Member functions are to be prefixed with their belonging type-name. <br>
C example:
```c
void thisIsAnExampleFunction();
void ExampleStruct_thisIsAnExampleMemberFunction();
```

#### Function Parameters
Function parameters are to be written in [**Camel Case**](#camel-case). Function parameter names must describe **exactly** what they expect. A parameter should be marked const if applicable. <br>
C/C++ example:
```cpp
void rotate(const Vec2& vector, float angleX, float angleY);
```
Python example:
```py
def rotate(vector: Vec2, angleX: float, angleY: float) -> None: ...
```

#### Templates
Templates are to be written in [**Pascal Case**](#pascal-case) or [**Screaming Snake Case**](#screaming-snake-case). If applicable, use single letters. Example:
```cpp
template<typename T, typename U, typename ExampleTypenameTemplate>
```

### Formatting
#### Types
C/C++ example:
```cpp
struct ExampleStruct {
    void exampleMember;
}
class ExampleClass {
    public:
    void exampleMember;
    void exampleFunction();
    private:
    void mExampleMember;
}
```
Python example:
```py
class ExampleClass:
    exampleMember: SomeType = ...
    def exampleFunction() -> SomeType: ...
```

#### If Statements
C/C++ example:
```cpp
if (exampleCondition) {
    // Do something...
} else if (otherExampleCondition) {
    // Do something else...
} else {
    // Do something else...
}
```
Python example:
```py
if exampleCondition:
    # Do something...
    pass
elif otherExampleCondition:
    # Do something else...
    pass
else:
    # Do something else...
    pass
```

### Best Practices
#### Pointers
All persistent-data or data outside of function scope (except for the main function) should be stored as a pointer for compatibility with future custom allocators. <br>
C/C++ example:
```cpp
struct {
    void* examplePersistentData;
}

void someFunc() {
    void exampleNonPersistentData;
}

int main() {
    void* examplePersistentData;
}
```