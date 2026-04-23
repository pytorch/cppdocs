# Custom Classes

PyTorch allows registering custom C++ classes that can be used from Python
and TorchScript.

Header: `torch/custom_class.h`

## class_ Template

template<class CurClass>
class class_ : public torch::detail::class_base

Entry point for custom C++ class registration.

To register a C++ class in PyTorch, instantiate `torch::class_` with the desired class as the template parameter. Typically, this instantiation should be done in the initialization of a global variable, so that the class will be made available on dynamic library loading without any additional API calls needed. For example, to register a class named Foo, you might create a global variable like so:

```
static auto register_foo = torch::class_<Foo>("myclasses", "Foo")
 .def("myMethod", &Foo::myMethod)
 .def("lambdaMethod", [](const c10::intrusive_ptr<Foo>& self) {
 // Do something with `self`
 });
```

 In addition to registering the class, this registration also chains `def()` calls to register methods. `myMethod()` is registered with a pointer to the Foo class's `myMethod()` method. `lambdaMethod()` is registered with a C++ lambda expression. 

Public Functions

inline explicit class_(const std::string &namespaceName, const std::string &className, std::string doc_string = "")

This constructor actually registers the class type.

String argument `namespaceName` is an identifier for the namespace you would like this class to appear in. String argument `className` is the name you would like to see this class exposed as in Python and TorchScript. For example, if you pass `foo` as the namespace name and `Bar` as the className, the class will appear as `torch.classes.foo.Bar` in Python and TorchScript

template<typename ...Types>
inline class_ &def(torch::detail::types<void, Types...>, std::string doc_string = "", std::initializer_list<arg> default_args = {})

def() can be used in conjunction with `torch::init()` to register a constructor for a given C++ class type.

For example, passing `torch::init<int, std::string>()` would register a two-argument constructor taking an `int` and a `std::string` as argument.

template<typename Func, typename ...ParameterTypes>
inline class_ &def(InitLambda<Func, c10::guts::typelist::typelist<ParameterTypes...>> init, std::string doc_string = "", std::initializer_list<arg> default_args = {})

template<typename Func>
inline class_ &def(std::string name, Func f, std::string doc_string = "", std::initializer_list<arg> default_args = {})

This is the normal method registration API.

`name` is the name that the method will be made accessible by in Python and TorchScript. `f` is a callable object that defines the method. Typically `f` will either be a pointer to a method on `CurClass`, or a lambda expression that takes a `c10::intrusive_ptr<CurClass>` as the first argument (emulating a `this` argument in a C++ method.)

Examples:

```
// Exposes method `foo` on C++ class `Foo` as `call_foo()` in
// Python and TorchScript
.def("call_foo", &Foo::foo)

// Exposes the given lambda expression as method `call_lambda()`
// in Python and TorchScript.
.def("call_lambda", [](const c10::intrusive_ptr<Foo>& self) {
 // do something
})
```

template<typename Func>
inline class_ &def_static(std::string name, Func func, std::string doc_string = "")

Method registration API for static methods.

template<typename GetterFunc, typename SetterFunc>
inline class_ &def_property(const std::string &name, GetterFunc getter_func, SetterFunc setter_func, std::string doc_string = "")

Property registration API for properties with both getter and setter functions.

template<typename GetterFunc>
inline class_ &def_property(const std::string &name, GetterFunc getter_func, std::string doc_string = "")

Property registration API for properties with only getter function.

template<typename T>
inline class_ &def_readwrite(const std::string &name, T CurClass::* field)

Property registration API for properties with read-write access.

template<typename T>
inline class_ &def_readonly(const std::string &name, T CurClass::* field)

Property registration API for properties with read-only access.

inline class_ &_def_unboxed(const std::string &name, std::function<void(jit::Stack&)> func, c10::FunctionSchema schema, std::string doc_string = "")

This is an unsafe method registration API added for adding custom JIT backend support via custom C++ classes.

It is not for general purpose use.

template<typename GetStateFn, typename SetStateFn>
inline class_ &def_pickle(GetStateFn &&get_state, SetStateFn &&set_state)

def_pickle() is used to define exactly what state gets serialized or deserialized for a given instance of a custom C++ class in Python or TorchScript.

This protocol is equivalent to the Pickle concept of `__getstate__` and `__setstate__` from Python ([https://docs.python.org/2/library/pickle.html#object.__getstate__](https://docs.python.org/2/library/pickle.html#object.__getstate__))

Currently, both the `get_state` and `set_state` callables must be C++ lambda expressions. They should have the following signatures, where `CurClass` is the class you're registering and `T1` is some object that encapsulates the state of the object.

```
__getstate__(intrusive_ptr<CurClass>) -> T1
__setstate__(T2) -> intrusive_ptr<CurClass>
```

`T1` must be an object that is convertible to IValue by the same rules for custom op/method registration.

For the common case, T1 == T2. T1 can also be a subtype of T2. An example where it makes sense for T1 and T2 to differ is if **setstate** handles legacy formats in a backwards compatible way.

Example:

```
.def_pickle(
 // __getstate__
 [](const c10::intrusive_ptr<MyStackClass<std::string>>& self) {
 return self->stack_;
 },
 [](std::vector<std::string> state) { // __setstate__
 return c10::make_intrusive<MyStackClass<std::string>>(
 std::vector<std::string>{"i", "was", "deserialized"});
 })
```

**Example:**

```
#include <torch/custom_class.h>

struct MyClass : torch::CustomClassHolder {
 int value;

 MyClass(int v) : value(v) {}

 int getValue() const { return value; }
 void setValue(int v) { value = v; }
};

TORCH_LIBRARY(my_classes, m) {
 m.class_<MyClass>("MyClass")
 .def(torch::init<int>())
 .def("getValue", &MyClass::getValue)
 .def("setValue", &MyClass::setValue)
 .def_readwrite("value", &MyClass::value);
}
```

## Registering Methods

**Constructor:**

```
m.class_<MyClass>("MyClass")
 .def(torch::init<int>()) // Constructor taking int
```

**Methods:**

```
m.class_<MyClass>("MyClass")
 .def("getValue", &MyClass::getValue)
 .def("setValue", &MyClass::setValue)
```

**Properties:**

```
m.class_<MyClass>("MyClass")
 .def_readwrite("value", &MyClass::value) // Read-write
 .def_readonly("const_value", &MyClass::const_value) // Read-only
```

## Using Custom Classes

**From C++:**

```
auto my_obj = c10::make_intrusive<MyClass>(42);
int val = my_obj->getValue();
```

**From Python:**

```
import torch
torch.classes.load_library("path/to/library.so")
obj = torch.classes.my_classes.MyClass(42)
print(obj.getValue())
```

**In TorchScript:**

```
@torch.jit.script
def use_my_class(x: torch.classes.my_classes.MyClass) -> int:
 return x.getValue()
```