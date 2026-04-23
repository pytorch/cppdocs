# Operator Registration

The library API provides macros and classes for registering custom operators
with PyTorch's dispatcher.

## Macros

### TORCH_LIBRARY

TORCH_LIBRARY(ns, m) static void TORCH_LIBRARY_init_##ns(torch::Library

&); \

static const

torch::detail::TorchLibraryInit

TORCH_LIBRARY_static_init_##ns( \

torch::Library::DEF, \

&TORCH_LIBRARY_init_##ns, \

C10_STRINGIZE(ns), \

std::nullopt, \

__FILE__, \

__LINE__); \

void TORCH_LIBRARY_init_##ns(

torch::Library& m)

Macro for defining a function that will be run at static initialization time to define a library of operators in the namespace `ns` (must be a valid C++ identifier, no quotes).

Use this macro when you want to define a new set of custom operators that do not already exist in PyTorch.

Example usage:

```
TORCH_LIBRARY(myops, m) {
 // m is a torch::Library; methods on it will define
 // operators in the myops namespace
 m.def("add", add_impl);
}
```

The `m` argument is bound to a torch::Library that is used to register operators. There may only be one TORCH_LIBRARY() for any given namespace.

**Example:**

```
TORCH_LIBRARY(myops, m) {
 m.def("add(Tensor self, Tensor other) -> Tensor", &add_impl);
 m.def("mul(Tensor self, Tensor other) -> Tensor");
 m.impl("mul", torch::kCPU, &mul_cpu_impl);
 m.impl("mul", torch::kCUDA, &mul_cuda_impl);
}
```

### TORCH_LIBRARY_IMPL

TORCH_LIBRARY_IMPL(ns, k, m) _TORCH_LIBRARY_IMPL(ns, k, m, C10_UID)

Macro for defining a function that will be run at static initialization time to define operator overrides for dispatch key `k` (must be an unqualified enum member of c10::DispatchKey) in namespace `ns` (must be a valid C++ identifier, no quotes).

Use this macro when you want to implement a preexisting set of custom operators on a new dispatch key (e.g., you want to provide CUDA implementations of already existing operators). One common usage pattern is to use TORCH_LIBRARY() to define schema for all new operators you want to define, and then use several TORCH_LIBRARY_IMPL() blocks to provide implementations of the operator for CPU, CUDA and Autograd.

In some cases, you need to define something that applies to all namespaces, not just one namespace (usually a fallback). In that case, use the reserved namespace _, e.g.,

```
TORCH_LIBRARY_IMPL(_, XLA, m) {
 m.fallback(xla_fallback);
}
```

Example usage:

```
TORCH_LIBRARY_IMPL(myops, CPU, m) {
 // m is a torch::Library; methods on it will define
 // CPU implementations of operators in the myops namespace.
 // It is NOT valid to call torch::Library::def()
 // in this context.
 m.impl("add", add_cpu_impl);
}
```

If `add_cpu_impl` is an overloaded function, use a `static_cast` to specify which overload you want (by providing the full type).

**Example:**

```
TORCH_LIBRARY_IMPL(myops, XLA, m) {
 m.impl("mul", &mul_xla_impl);
}
```

### TORCH_LIBRARY_FRAGMENT

TORCH_LIBRARY_FRAGMENT(ns, m) _TORCH_LIBRARY_FRAGMENT(ns, m, C10_UID)

This macro is a version of TORCH_LIBRARY() that doesn't enforce that there is only one library (it is a "fragment").

This is used inside the PerOpRegistration.cpp file, as well as in places where all op registrations within the same namespace cannot be easily put into one macro block (this is mostly the case for custom ops in fbcode that were ported from the old API)

**Example:**

```
// In file1.cpp
TORCH_LIBRARY(myops, m) {
 m.def("add(Tensor self, Tensor other) -> Tensor", &add_impl);
}

// In file2.cpp
TORCH_LIBRARY_FRAGMENT(myops, m) {
 m.def("mul(Tensor self, Tensor other) -> Tensor", &mul_impl);
}
```

## Classes

### Library

class Library

This object provides the API for defining operators and providing implementations at dispatch keys.

Typically, a torch::Library is not allocated directly; instead it is created by the TORCH_LIBRARY() or TORCH_LIBRARY_IMPL() macros.

Most methods on torch::Library return a reference to itself, supporting method chaining.

```
// Examples:

TORCH_LIBRARY(torchvision, m) {
 // m is a torch::Library
 m.def("roi_align", ...);
 ...
}

TORCH_LIBRARY_IMPL(aten, XLA, m) {
 // m is a torch::Library
 m.impl("add", ...);
 ...
}
```

Public Functions

Library(const Library&) = delete

Library &operator=(const Library&) = delete

Library(Library&&) = default

Library &operator=(Library&&) = default

~Library() = default

inline Library &def(c10::FunctionSchema &&s, const std::vector<at::Tag> &tags = {}, _RegisterOrVerify rv = _RegisterOrVerify::REGISTER) &

Declare an operator with a schema, but don't provide any implementations for it.

You're expected to then provide implementations using the impl() method. All template arguments are inferred.

```
// Example:
TORCH_LIBRARY(myops, m) {
 m.def("add(Tensor self, Tensor other) -> Tensor");
}
```

Parameters:

**raw_schema** - The schema of the operator to be defined. Typically, this is a `const char*` string literal, but any type accepted by torch::schema() is accepted here.

inline Library &def(const char *raw_schema, const std::vector<at::Tag> &tags = {}, _RegisterOrVerify rv = _RegisterOrVerify::REGISTER) &

inline Library &set_python_module(const char *pymodule, const char *context = "")

Declares that for all operators that are subsequently def'ed, their fake impls may be found in the given Python module (pymodule).

This registers some help text that is used if the fake impl cannot be found.

Args:

- pymodule: the python module
- context: We may include this in the error message.

inline Library &impl_abstract_pystub(const char *pymodule, const char *context = "")

Deprecated; use set_python_module instead.

template<typename NameOrSchema, typename Func>
inline Library &def(NameOrSchema &&raw_name_or_schema, Func &&raw_f, const std::vector<at::Tag> &tags = {}) &

Define an operator for a schema and then register an implementation for it.

This is typically what you would use if you aren't planning on making use of the dispatcher to structure your operator implementation. It's roughly equivalent to calling def() and then impl(), but if you omit the schema of the operator, we will infer it from the type of your C++ function. All template arguments are inferred.

```
// Example:
TORCH_LIBRARY(myops, m) {
 m.def("add", add_fn);
}
```

Parameters:

- **raw_name_or_schema** - The schema of the operator to be defined, or just the name of the operator if the schema is to be inferred from `raw_f`. Typically a `const char*` literal.
- **raw_f** - The C++ function that implements this operator. Any valid constructor of torch::CppFunction is accepted here; typically you provide a function pointer or lambda.

template<typename Name, typename Func>
inline Library &impl(Name name, Func &&raw_f, _RegisterOrVerify rv = _RegisterOrVerify::REGISTER) &

Register an implementation for an operator.

You may register multiple implementations for a single operator at different dispatch keys (see torch::dispatch()). Implementations must have a corresponding declaration (from def()), otherwise they are invalid. If you plan to register multiple implementations, DO NOT provide a function implementation when you def() the operator.

```
// Example:
TORCH_LIBRARY_IMPL(myops, CUDA, m) {
 m.impl("add", add_cuda);
}
```

Parameters:

- **name** - The name of the operator to implement. Do NOT provide schema here.
- **raw_f** - The C++ function that implements this operator. Any valid constructor of torch::CppFunction is accepted here; typically you provide a function pointer or lambda.

c10::OperatorName _resolve(const char *name) const

template<typename Name, typename Func>
inline Library &impl_UNBOXED(Name, Func*) &

inline Library &def(detail::SelectiveStr<false>, const std::vector<at::Tag> &tags[`[maybe_unused]`] = {}) &

inline Library &def(detail::SelectiveStr<true> raw_schema, const std::vector<at::Tag> &tags = {}) &

template<typename Func>
inline Library &def(detail::SelectiveStr<false>, Func&&, const std::vector<at::Tag> &tags[`[maybe_unused]`] = {}) &

template<typename Func>
inline Library &def(detail::SelectiveStr<true> raw_name_or_schema, Func &&raw_f, const std::vector<at::Tag> &tags = {}) &

template<typename Func>
inline Library &impl(detail::SelectiveStr<false>, Func&&) &

template<typename Dispatch, typename Func>
inline Library &impl(detail::SelectiveStr<false>, Dispatch&&, Func&&) &

template<typename Func>
inline Library &impl_UNBOXED(detail::SelectiveStr<false>, Func*) &

template<typename Func>
inline Library &impl(detail::SelectiveStr<true> name, Func &&raw_f) &

template<typename Dispatch, typename Func>
inline Library &impl(detail::SelectiveStr<true> name, Dispatch &&key, Func &&raw_f) &

template<typename Func>
inline Library &impl_UNBOXED(detail::SelectiveStr<true>, Func*) &

template<typename Func>
inline Library &fallback(Func &&raw_f) &

Register a fallback implementation for all operators which will be used if there is not a specific implementation for an operator available.

There MUST be a DispatchKey associated with a fallback; e.g., only call this from TORCH_LIBRARY_IMPL() with namespace `_`.

```
// Example:

TORCH_LIBRARY_IMPL(_, AutogradXLA, m) {
 // If there is not a kernel explicitly registered
 // for AutogradXLA, fallthrough to the next
 // available kernel
 m.fallback(torch::CppFunction::makeFallthrough());
}

// See aten/src/ATen/core/dispatch/backend_fallback_test.cpp
// for a full example of boxed fallback
```

Parameters:

**raw_f** - The function that implements the fallback. Unboxed functions typically do not work as fallback functions, as fallback functions must work for every operator (even though they have varying type signatures). Typical arguments are CppFunction::makeFallthrough() or CppFunction::makeFromBoxedFunction()

template<class CurClass>
inline torch::[class_](custom_classes.html#_CPPv4I0EN5torch6class_E)<CurClass> class_(const std::string &className)

template<class CurClass>
inline torch::[class_](custom_classes.html#_CPPv4I0EN5torch6class_E)<CurClass> class_(detail::SelectiveStr<true> className)

template<class CurClass>
inline detail::ClassNotSelected class_(detail::SelectiveStr<false> className)

void reset()

template<class CurClass>
inline class_<CurClass> class_(const std::string &className)

template<class CurClass>
inline class_<CurClass> class_(detail::SelectiveStr<true> className)

Friends

*friend class* detail::TorchLibraryInit

**Example:**

```
TORCH_LIBRARY(myops, m) {
 // Define with implementation
 m.def("add(Tensor self, Tensor other) -> Tensor", &add_impl);

 // Define schema only
 m.def("mul(Tensor self, Tensor other) -> Tensor");

 // Provide backend-specific implementations
 m.impl("mul", torch::kCPU, &mul_cpu_impl);
 m.impl("mul", torch::kCUDA, &mul_cuda_impl);
}
```

### CppFunction

class CppFunction

Represents a C++ function that implements an operator.

Most users won't interact directly with this class, except via error messages: the constructors this function define the set of permissible "function"-like things you can bind via the interface.

This class erases the type of the passed in function, but durably records the type via an inferred schema for the function.

Public Functions

template<typename Func>
inline explicit CppFunction(Func *f, std::enable_if_t<c10::guts::is_function_type<Func>::value, std::nullptr_t> = nullptr)

This overload accepts function pointers, e.g., `CppFunction(&add_impl)`

template<typename FuncPtr>
inline explicit CppFunction(FuncPtr f, std::enable_if_t<c10::is_compile_time_function_pointer<FuncPtr>::value, std::nullptr_t> = nullptr)

This overload accepts compile time function pointers, e.g., `CppFunction(TORCH_FN(add_impl))`

template<typename Lambda>
inline explicit CppFunction(Lambda &&f, std::enable_if_t<c10::guts::is_functor<std::decay_t<Lambda>>::value, std::nullptr_t> = nullptr)

This overload accepts lambdas, e.g., `CppFunction([](const Tensor& self) { ...`

})

~CppFunction()

CppFunction(const CppFunction&) = delete

CppFunction &operator=(const CppFunction&) = delete

CppFunction(CppFunction&&) noexcept = default

CppFunction &operator=(CppFunction&&) = default

inline CppFunction &&debug(std::string d) &&

Public Static Functions

static inline CppFunction makeFallthrough()

This creates a fallthrough function.

Fallthrough functions immediately redispatch to the next available dispatch key, but are implemented more efficiently than a hand written function done in the same way.

template<c10::BoxedKernel::BoxedKernelFunction *func>
static inline CppFunction makeFromBoxedFunction()

Create a function from a boxed kernel function with signature `void(const OperatorHandle&, Stack*)`; i.e., they receive a stack of arguments in a boxed calling convention, rather than in the native C++ calling convention.

Boxed functions are typically only used to register backend fallbacks via torch::Library::fallback().

template<c10::BoxedKernel::BoxedKernelFunction_withDispatchKeys *func>
static inline CppFunction makeFromBoxedFunction()

template<class KernelFunctor>
static inline CppFunction makeFromBoxedFunctor(std::unique_ptr<KernelFunctor> kernelFunctor)

Create a function from a boxed kernel functor which defines `operator()(const OperatorHandle&, DispatchKeySet, Stack*)` (receiving arguments from boxed calling convention) and inherits from `c10::OperatorKernel`.

Unlike makeFromBoxedFunction, functions registered in this way can also carry additional state which is managed by the functor; this is useful if you're writing an adapter to some other implementation, e.g., a Python callable, which is dynamically associated with the registered kernel.

template<typename FuncPtr, std::enable_if_t<c10::guts::is_function_type<FuncPtr>::value, std::nullptr_t> = nullptr>
static inline CppFunction makeFromUnboxedFunction(FuncPtr *f)

Create a function from an unboxed kernel function.

This is typically used to register common operators.

template<typename FuncPtr, std::enable_if_t<c10::is_compile_time_function_pointer<FuncPtr>::value, std::nullptr_t> = nullptr>
static inline CppFunction makeFromUnboxedFunction(FuncPtr f)

Create a function from a compile time unboxed kernel function pointer.

This is typically used to register common operators. Compile time function pointers can be used to allow the compiler to optimize (e.g. inline) calls to it.

### OrderedDict

template<typename Key, typename Value>
class OrderedDict

An ordered dictionary implementation, akin to Python's `OrderedDict`.

Public Types

using Iterator = typename std::vector<Item>::iterator

using ConstIterator = typename std::vector<Item>::const_iterator

Public Functions

explicit OrderedDict(std::string key_description = "Key")

Constructs the `OrderedDict` with a short description of the kinds of keys stored in the `OrderedDict`.

This description is used in error messages thrown by the `OrderedDict`.

OrderedDict(const OrderedDict &other)

Copy constructs this `OrderedDict` from `other`.

OrderedDict &operator=(const OrderedDict &other)

Assigns items from `other` to this `OrderedDict`.

OrderedDict(OrderedDict &&other) noexcept = default

OrderedDict &operator=(OrderedDict &&other) noexcept = default

~OrderedDict() = default

OrderedDict(std::initializer_list<Item> initializer_list)

Constructs a new `OrderedDict` and pre-populates it with the given `Item`s.

const std::string &key_description() const noexcept

Returns the key description string the `OrderedDict` was constructed with.

Item &front()

Returns the very first item in the `OrderedDict` and throws an exception if it is empty.

const Item &front() const

Returns the very first item in the `OrderedDict` and throws an exception if it is empty.

Item &back()

Returns the very last item in the `OrderedDict` and throws an exception if it is empty.

const Item &back() const

Returns the very last item in the `OrderedDict` and throws an exception if it is empty.

Item &operator[](size_t index)

Returns the item at the `index`-th position in the `OrderedDict`.

Throws an exception if the index is out of bounds.

const Item &operator[](size_t index) const

Returns the item at the `index`-th position in the `OrderedDict`.

Throws an exception if the index is out of bounds.

Value &operator[](const Key &key)

Returns the value associated with the given `key`.

Throws an exception if no such key is stored in the `OrderedDict`. Use `find()` for a non-throwing way of accessing a value if it is present.

const Value &operator[](const Key &key) const

Returns the value associated with the given `key`.

Throws an exception if no such key is stored in the `OrderedDict`. Use `find()` for a non-throwing way of accessing a value if it is present.

Value *find(const Key &key) noexcept

Returns a pointer to the value associated with the given key, or a `nullptr` if no such key is stored in the `OrderedDict`.

const Value *find(const Key &key) const noexcept

Returns a pointer to the value associated with the given key, or a `nullptr` if no such key is stored in the `OrderedDict`.

bool contains(const Key &key) const noexcept

Returns true if the key is present in the `OrderedDict`.

Iterator begin()

Returns an iterator to the first item in the `OrderedDict`.

Iteration is ordered.

ConstIterator begin() const

Returns an iterator to the first item in the `OrderedDict`.

Iteration is ordered.

Iterator end()

Returns an iterator one past the last item in the `OrderedDict`.

ConstIterator end() const

Returns an iterator one past the last item in the `OrderedDict`.

size_t size() const noexcept

Returns the number of items currently stored in the `OrderedDict`.

bool is_empty() const noexcept

Returns true if the `OrderedDict` contains no elements.

void reserve(size_t requested_capacity)

Resizes internal storage to fit at least `requested_capacity` items without requiring reallocation.

template<typename K, typename V>
Value &insert(K &&key, V &&value)

Inserts a new `(key, value)` pair into the `OrderedDict`.

Throws an exception if the key is already present. If insertion is successful, immediately returns a reference to the inserted value.

Value &insert(Key key, Value &&value)

Inserts a new `(key, value)` pair into the `OrderedDict`.

Throws an exception if the key is already present. If insertion is successful, immediately returns a reference to the inserted value.

void update(OrderedDict &&other)

Inserts all items from `other` into this `OrderedDict`.

If any key from `other` is already present in this `OrderedDict`, an exception is thrown.

void update(const OrderedDict &other)

Inserts all items from `other` into this `OrderedDict`.

If any key from `other` is already present in this `OrderedDict`, an exception is thrown.

void erase(const Key &key)

Removes the item that has `key` from this `OrderedDict` if exists and if it doesn't an exception is thrown.

void clear()

Removes all items from this `OrderedDict`.

const std::vector<Item> &items() const noexcept

Returns the items stored in the `OrderedDict`.

::std::vector<Key> keys() const

Returns a newly allocated vector and copies all keys from this `OrderedDict` into the vector.

::std::vector<Value> values() const

Returns a newly allocated vector and copies all values from this `OrderedDict` into the vector.

::std::vector<std::pair<Key, Value>> pairs() const

Returns a newly allocated vector and copies all keys and values from this `OrderedDict` into a vector of `std::pair<Key, Value>`.

Friends

template<typename K, typename V>
friend bool operator==(const OrderedDict<K, V> &a, const OrderedDict<K, V> &b)

Returns true if both dicts contain the same keys and values, in the same order.

class Item

Public Functions

inline Item(Key key, Value value)

Constructs a new item.

inline Value &operator*()

Returns a reference to the value.

inline const Value &operator*() const

Returns a reference to the value.

inline Value *operator->()

Allows access to the value using the arrow operator.

inline const Value *operator->() const

Allows access to the value using the arrow operator.

inline const Key &key() const noexcept

Returns a reference to the key.

inline Value &value() noexcept

Returns a reference to the value.

inline const Value &value() const noexcept

Returns a reference to the value.

inline const std::pair<Key, Value> &pair() const noexcept

Returns a `(key, value)` pair.

## Functions

The library API provides builder methods on the `Library` class for registering
operators. See the `Library` class documentation above for the full API including
`def()`, `impl()`, and `fallback()` methods.

## Dispatch Keys

Common dispatch keys used with `torch::dispatch()`:

- `torch::kCPU` - CPU backend
- `torch::kCUDA` - CUDA backend
- `torch::kAutograd` - Autograd backend
- `torch::kMeta` - Meta tensor backend