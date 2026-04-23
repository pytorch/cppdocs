# Core Types

C10 provides fundamental types used throughout PyTorch.

## ArrayRef

template<typename T>
class ArrayRef : public HeaderOnlyArrayRef<T>

ArrayRef - Represent a constant reference to an array (0 or more elements consecutively in memory), i.e.

a start pointer and a length. It allows various APIs to take consecutive elements easily and conveniently.

This class does not own the underlying data, it is expected to be used in situations where the data resides in some other buffer, whose lifetime extends past that of the ArrayRef. For this reason, it is not in general safe to store an ArrayRef.

This is intended to be trivially copyable, so it should be passed by value.

NOTE: We have refactored out the headeronly parts of the ArrayRef struct into HeaderOnlyArrayRef. As adding `virtual` would change the performance of the underlying constexpr calls, we rely on apparent-type dispatch for inheritance. This should be fine because their memory format is the same, and it is never incorrect for ArrayRef to call HeaderOnlyArrayRef methods. However, you should prefer to use ArrayRef when possible, because its use of TORCH_CHECK will lead to better user-facing error messages.

Constructors, all inherited from HeaderOnlyArrayRef except for

SmallVector.

As inherited constructors won't work with class template argument deduction (CTAD) until C++23, we add deduction guides after the class definition to enable CTAD.

template<typename U>
inline ArrayRef(const SmallVectorTemplateCommon<T, U> &Vec)

Construct an ArrayRef from a SmallVector.

This is templated in order to avoid instantiating SmallVectorTemplateCommon<T> whenever we copy-construct an ArrayRef. NOTE: this is the only constructor that is not inherited from HeaderOnlyArrayRef.

Simple Operations, mostly inherited from HeaderOnlyArrayRef

inline constexpr const T &front() const

front - Get the first element.

We deviate from HeaderOnlyArrayRef by using TORCH_CHECK instead of STD_TORCH_CHECK

inline constexpr const T &back() const

back - Get the last element.

We deviate from HeaderOnlyArrayRef by using TORCH_CHECK instead of STD_TORCH_CHECK

inline constexpr ArrayRef<T> slice(size_t N, size_t M) const

slice(n, m) - Take M elements of the array starting at element N We deviate from HeaderOnlyArrayRef by using TORCH_CHECK instead of STD_TORCH_CHECK

inline constexpr ArrayRef<T> slice(size_t N) const

slice(n) - Chop off the first N elements of the array.

We deviate from HeaderOnlyArrayRef by using TORCH_CHECK instead of STD_TORCH_CHECK

Operator Overloads

inline constexpr const T &at(size_t Index) const

Vector compatibility We deviate from HeaderOnlyArrayRef by using TORCH_CHECK instead of STD_TORCH_CHECK.

template<typename U>
std::enable_if_t<std::is_same_v<U, T>, ArrayRef<T>> &operator=(U &&Temporary) = delete

Disallow accidental assignment from a temporary.

The declaration here is extra complicated so that "arrayRef = {}" continues to select the move assignment operator.

template<typename U>
std::enable_if_t<std::is_same_v<U, T>, ArrayRef<T>> &operator=(std::initializer_list<U>) = delete

Disallow accidental assignment from a temporary.

The declaration here is extra complicated so that "arrayRef = {}" continues to select the move assignment operator.

**Example:**

```
std::vector<int64_t> sizes = {3, 4, 5};
c10::ArrayRef<int64_t> sizes_ref(sizes);

// Can also use initializer list
auto tensor = at::zeros({3, 4, 5}); // implicitly converts
```

## OptionalArrayRef

template<typename T>
class OptionalArrayRef

Public Functions

constexpr OptionalArrayRef() noexcept = default

inline constexpr OptionalArrayRef(std::nullopt_t) noexcept

OptionalArrayRef(const OptionalArrayRef &other) = default

OptionalArrayRef(OptionalArrayRef &&other) noexcept = default

inline constexpr OptionalArrayRef(const std::optional<ArrayRef<T>> &other) noexcept

inline constexpr OptionalArrayRef(std::optional<ArrayRef<T>> &&other) noexcept

inline constexpr OptionalArrayRef(const T &value) noexcept

template<typename U = ArrayRef<T>, std::enable_if_t<!std::is_same_v<std::decay_t<U>, OptionalArrayRef> && !std::is_same_v<std::decay_t<U>, std::in_place_t> && std::is_constructible_v<ArrayRef<T>, U&&> && std::is_convertible_v<U&&, ArrayRef<T>> && !std::is_convertible_v<U&&, T>, bool> = false>
inline constexpr OptionalArrayRef(U &&value) noexcept(std::is_nothrow_constructible_v<ArrayRef<T>, U&&>)

template<typename U = ArrayRef<T>, std::enable_if_t<!std::is_same_v<std::decay_t<U>, OptionalArrayRef> && !std::is_same_v<std::decay_t<U>, std::in_place_t> && std::is_constructible_v<ArrayRef<T>, U&&> && !std::is_convertible_v<U&&, ArrayRef<T>>, bool> = false>
inline explicit constexpr OptionalArrayRef(U &&value) noexcept(std::is_nothrow_constructible_v<ArrayRef<T>, U&&>)

template<typename ...Args>
inline explicit constexpr OptionalArrayRef(std::in_place_t ip, Args&&... args) noexcept

template<typename U, typename ...Args>
inline explicit constexpr OptionalArrayRef(std::in_place_t ip, std::initializer_list<U> il, Args&&... args)

inline constexpr OptionalArrayRef(const std::initializer_list<T> &Vec)

~OptionalArrayRef() = default

inline constexpr OptionalArrayRef &operator=(std::nullopt_t) noexcept

OptionalArrayRef &operator=(const OptionalArrayRef &other) = default

OptionalArrayRef &operator=(OptionalArrayRef &&other) noexcept = default

inline constexpr OptionalArrayRef &operator=(const std::optional<ArrayRef<T>> &other) noexcept

inline constexpr OptionalArrayRef &operator=(std::optional<ArrayRef<T>> &&other) noexcept

template<typename U = ArrayRef<T>, typename = std::enable_if_t<!std::is_same_v<std::decay_t<U>, OptionalArrayRef> && std::is_constructible_v<ArrayRef<T>, U&&> && std::is_assignable_v<ArrayRef<T>&, U&&>>>
inline constexpr OptionalArrayRef &operator=(U &&value) noexcept(std::is_nothrow_constructible_v<ArrayRef<T>, U&&> && std::is_nothrow_assignable_v<ArrayRef<T>&, U&&>)

inline constexpr ArrayRef<T> *operator->() noexcept

inline constexpr const ArrayRef<T> *operator->() const noexcept

inline constexpr ArrayRef<T> &operator*() & noexcept

inline constexpr const ArrayRef<T> &operator*() const & noexcept

inline constexpr ArrayRef<T> &&operator*() && noexcept

inline constexpr const ArrayRef<T> &&operator*() const && noexcept

inline explicit constexpr operator bool() const noexcept

inline constexpr bool has_value() const noexcept

inline constexpr ArrayRef<T> &value() &

inline constexpr const ArrayRef<T> &value() const &

inline constexpr ArrayRef<T> &&value() &&

inline constexpr const ArrayRef<T> &&value() const &&

template<typename U>
inline constexpr std::enable_if_t<std::is_convertible_v<U&&, ArrayRef<T>>, ArrayRef<T>> value_or(U &&default_value) const &

template<typename U>
inline constexpr std::enable_if_t<std::is_convertible_v<U&&, ArrayRef<T>>, ArrayRef<T>> value_or(U &&default_value) &&

inline constexpr void swap(OptionalArrayRef &other) noexcept

inline constexpr void reset() noexcept

template<typename ...Args>
inline constexpr std::enable_if_t<std::is_constructible_v<ArrayRef<T>, Args&&...>, ArrayRef<T>&> emplace(Args&&... args) noexcept(std::is_nothrow_constructible_v<ArrayRef<T>, Args&&...>)

template<typename U, typename ...Args>
inline constexpr ArrayRef<T> &emplace(std::initializer_list<U> il, Args&&... args) noexcept

**Example:**

```
void my_function(c10::OptionalArrayRef<int64_t> sizes = c10::nullopt) {
 if (sizes.has_value()) {
 for (auto s : sizes.value()) {
 // process sizes
 }
 }
}
```

## Optional

class c10::optional

A wrapper type that may or may not contain a value.
Similar to `std::optional`.

bool has_value() const

Returns true if a value is present.

T &value()

Returns the contained value. Throws if empty.

T value_or(T default_value) const

Returns the value if present, otherwise returns the default.

**Example:**

```
c10::optional<int64_t> maybe_dim = c10::nullopt;

if (maybe_dim.has_value()) {
 std::cout << "Dim: " << maybe_dim.value() << std::endl;
}

int64_t dim = maybe_dim.value_or(-1); // Returns -1 if empty
```

## Half

class c10::Half

16-bit floating point type (IEEE 754 half-precision).

Half(float value)

Construct from a float.

operator float() const

Convert to float.

**Example:**

```
c10::Half h = 3.14f;
float f = static_cast<float>(h);
```

## Containers

C10 provides container types that store `IValue` elements internally. These
are pointer types: copies share the same underlying storage.

### Dict

An ordered hash map from `Key` to `Value`. Valid key types are `int64_t`,
`double`, `bool`, `std::string`, and `at::Tensor`.

template<class Key, class Value>
class Dict

**Example:**

```
#include <ATen/core/Dict.h>

c10::Dict<std::string, at::Tensor> named_tensors;
named_tensors.insert("weight", torch::randn({3, 3}));
named_tensors.insert("bias", torch::zeros({3}));

if (named_tensors.contains("weight")) {
 at::Tensor w = named_tensors.at("weight");
}

for (const auto& entry : named_tensors) {
 std::cout << entry.key() << ": " << entry.value().sizes() << std::endl;
}
```

### List

A type-safe list container backed by `IValue` elements.

template<class T>
class List

**Example:**

```
#include <ATen/core/List.h>

c10::List<at::Tensor> tensor_list;
tensor_list.push_back(torch::randn({2, 3}));
tensor_list.push_back(torch::zeros({2, 3}));

at::Tensor first = tensor_list.get(0);
std::cout << "List size: " << tensor_list.size() << std::endl;

c10::List<int64_t> int_list;
int_list.push_back(1);
int_list.push_back(2);
int_list.push_back(3);
```

### IListRef

`c10::IListRef<T>` is a lightweight reference type that provides a unified
interface over different list-like types (`List<T>`, `ArrayRef<T>`,
`std::vector<T>`). It avoids copying when passing list arguments to operators.

template<class T>
class IListRef

**Example:**

```
#include <ATen/core/IListRef.h>

// IListRef can wrap different underlying types
std::vector<at::Tensor> vec = {torch::randn({2}), torch::randn({3})};
c10::IListRef<at::Tensor> ref(vec);

for (const auto& t : ref) {
 std::cout << t.sizes() << std::endl;
}
```

## IValue

`c10::IValue` (Interpreter Value) is a type-erased container used extensively
for storing values of different types. It can hold tensors,
scalars, lists, dictionaries, and other types.

Note

The full API documentation for IValue is complex due to its many type
conversion methods. See the header file `ATen/core/ivalue.h` for complete
details.

**Common methods:**

- `isTensor()` / `toTensor()` - Check if tensor / convert to tensor
- `isInt()` / `toInt()` - Check if int / convert to int
- `isDouble()` / `toDouble()` - Check if double / convert to double
- `isBool()` / `toBool()` - Check if bool / convert to bool
- `isString()` / `toString()` - Check if string / convert to string
- `isList()` / `toList()` - Check if list / convert to list
- `isGenericDict()` / `toGenericDict()` - Check if dict / convert to dict
- `isTuple()` / `toTuple()` - Check if tuple / convert to tuple
- `isNone()` - Check if None/null

**Example:**

```
c10::IValue val = at::ones({2, 2});

if (val.isTensor()) {
 at::Tensor t = val.toTensor();
}
```