# Containers

Container modules hold other modules and define how they are composed together.
Use containers to build complex architectures from simpler building blocks.

- **Sequential**: Chain modules in order, output of one feeds into the next
- **ModuleList**: Store modules in a list for iteration (not auto-forwarded)
- **ModuleDict**: Store modules in a dictionary for named access
- **ParameterList/ParameterDict**: Store parameters directly without wrapping in modules

Note

PyTorch's C++ API uses the PIMPL (Pointer to Implementation) pattern. You create
modules using the public class name (e.g., `torch::nn::Sequential`), which
internally wraps an implementation class (`SequentialImpl`). The documentation
below shows the implementation classes, which contain all the actual methods.

## Sequential

`Sequential` is a container that chains modules together. Each module's output
becomes the next module's input. This is the simplest way to build feed-forward
networks.

class Sequential : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<SequentialImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `SequentialImpl`.

See the documentation for `SequentialImpl` class to learn what methods it provides, or the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Functions

Sequential() = default

inline Sequential(std::initializer_list<NamedAnyModule> named_modules)

Constructs the `Sequential` from a braced-init-list of named `[AnyModule](utilities.html#PyTorchclasstorch_1_1nn_1_1_any_module)`s.

It enables the following use case: `Sequential sequential({{"m1", M(1)}, {"m2", M(2)}})`

**Example:**

```
torch::nn::Sequential seq(
 torch::nn::Conv2d(torch::nn::Conv2dOptions(1, 32, 3)),
 torch::nn::ReLU(),
 torch::nn::Conv2d(torch::nn::Conv2dOptions(32, 64, 3)),
 torch::nn::ReLU()
);

auto output = seq->forward(input);
```

## ModuleList

`ModuleList` stores modules in a list for indexed or iterated access. Unlike
`Sequential`, it does not have a built-in `forward()` method--you control how
modules are called.

class ModuleList : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<ModuleListImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `ModuleListImpl`.

See the documentation for `ModuleListImpl` class to learn what methods it provides, or the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = ModuleListImpl

**Example:**

```
torch::nn::ModuleList layers;
layers->push_back(torch::nn::Linear(10, 20));
layers->push_back(torch::nn::Linear(20, 30));

torch::Tensor x = input;
for (const auto& layer : *layers) {
 x = layer->as<torch::nn::Linear>()->forward(x);
}
```

## ModuleDict

`ModuleDict` stores modules in a dictionary for named access. Useful when you
need to select modules by name at runtime.

class ModuleDict : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<ModuleDictImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `ModuleDictImpl`.

See the documentation for `ModuleDictImpl` class to learn what methods it provides, or the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = ModuleDictImpl

## ParameterList

`ParameterList` stores parameters directly without wrapping them in modules.

class ParameterList : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<ParameterListImpl>

Public Types

using Impl = ParameterListImpl

class ParameterListImpl : public torch::nn::[Cloneable](utilities.html#_CPPv4I0EN5torch2nn9CloneableE)<ParameterListImpl>

Public Types

using Iterator = std::vector<[OrderedDict](../library/registration.html#_CPPv4I00EN5torch11OrderedDictE)<std::string, torch::Tensor>::Item>::iterator

using ConstIterator = std::vector<[OrderedDict](../library/registration.html#_CPPv4I00EN5torch11OrderedDictE)<std::string, torch::Tensor>::Item>::const_iterator

Public Functions

ParameterListImpl() = default

template<typename ...Tensors>
inline explicit ParameterListImpl(Tensors&&... params)

Constructs the `ParameterList` from a variadic list of ParameterList.

template<typename ...Tensors>
inline explicit ParameterListImpl(const Tensors&... params)

inline virtual void reset() override

`reset()` is empty for `ParameterList`, since it does not have parameters of its own.

inline virtual void pretty_print(std::ostream &stream) const override

Pretty prints the `ParameterList` module into the given `stream`.

inline void append(torch::Tensor &¶m)

push the a given parameter at the end of the list

inline void append(const torch::Tensor ¶m)

push the a given parameter at the end of the list

inline void append(const [OrderedDict](../library/registration.html#_CPPv4I00EN5torch11OrderedDictE)<std::string, torch::Tensor>::Item &pair)

push the a given parameter at the end of the list And the key of the pair will be discarded, only the value will be added into the `ParameterList`

template<typename Container>
inline void extend(const Container &container)

extend parameters from a container to the end of the list

inline Iterator begin()

Returns an iterator to the start of the ParameterList the iterator returned will be type of `[OrderedDict](../library/registration.html#PyTorchclasstorch_1_1_ordered_dict)<std::string, torch::Tensor>::Item`

inline ConstIterator begin() const

Returns a const iterator to the start of the ParameterList the iterator returned will be type of `[OrderedDict](../library/registration.html#PyTorchclasstorch_1_1_ordered_dict)<std::string, torch::Tensor>::Item`

inline Iterator end()

Returns an iterator to the end of the ParameterList the iterator returned will be type of `[OrderedDict](../library/registration.html#PyTorchclasstorch_1_1_ordered_dict)<std::string, torch::Tensor>::Item`

inline ConstIterator end() const

Returns a const iterator to the end of the ParameterList the iterator returned will be type of `[OrderedDict](../library/registration.html#PyTorchclasstorch_1_1_ordered_dict)<std::string, torch::Tensor>::Item`

inline at::Tensor &at(size_t idx)

Returns the value associated with the given `key`.

Throws an exception if no such key is stored in the `ParameterList`. Check contains(key) before for a non-throwing way of access

inline const at::Tensor &at(size_t idx) const

Returns the value associated with the given `key`.

Throws an exception if no such key is stored in the `ParameterList`. Check contains(key) before for a non-throwing way of access

inline at::Tensor &operator[](size_t idx)

Returns the value associated with the given `key`.

Throws an exception if no such key is stored in the `ParameterList`. Check contains(key) before for a non-throwing way of access

inline const at::Tensor &operator[](size_t idx) const

Returns the value associated with the given `key`.

Throws an exception if no such key is stored in the `ParameterList`. Check contains(key) before for a non-throwing way of access

inline size_t size() const noexcept

Return the size of the ParameterList.

inline bool is_empty() const noexcept

True if the ParameterList is empty.

template<typename Container>
inline Container &operator+=(const Container &other)

Overload the +=, so that two ParameterList could be incrementally added.

## ParameterDict

`ParameterDict` stores parameters in a dictionary for named access.

class ParameterDict : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<ParameterDictImpl>

Public Types

using Impl = ParameterDictImpl

class ParameterDictImpl : public torch::nn::[Cloneable](utilities.html#_CPPv4I0EN5torch2nn9CloneableE)<ParameterDictImpl>

Public Types

using Iterator = [OrderedDict](../library/registration.html#_CPPv4I00EN5torch11OrderedDictE)<std::string, [Tensor](../aten/tensor.html#_CPPv46Tensorv)>::Iterator

using ConstIterator = [OrderedDict](../library/registration.html#_CPPv4I00EN5torch11OrderedDictE)<std::string, [Tensor](../aten/tensor.html#_CPPv46Tensorv)>::ConstIterator

Public Functions

ParameterDictImpl() = default

inline explicit ParameterDictImpl(const torch::[OrderedDict](../library/registration.html#_CPPv4I00EN5torch11OrderedDictE)<std::string, torch::Tensor> ¶ms)

inline virtual void reset() override

`reset()` is empty for `ParameterDict`, since it does not have parameters of its own.

inline virtual void pretty_print(std::ostream &stream) const override

Pretty prints the `ParameterDict` module into the given `stream`.

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) &insert(const std::string &key, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) ¶m)

Insert the parameter along with the key into ParameterDict The parameter is set to be require grad by default.

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) pop(const std::string &key)

Remove key from the ParameterDict and return its value, throw exception if the key is not contained.

Please check contains(key) before for a non-throwing access.

inline ::std::vector<std::string> keys() const

Return the keys in the dict.

inline ::std::vector<torch::Tensor> values() const

Return the Values in the dict.

inline Iterator begin()

Return an iterator to the start of ParameterDict.

inline ConstIterator begin() const

Return a const iterator to the start of ParameterDict.

inline Iterator end()

Return an iterator to the end of ParameterDict.

inline ConstIterator end() const

Return a const iterator to the end of ParameterDict.

inline size_t size() const noexcept

Return the number of items currently stored in the ParameterDict.

inline bool empty() const noexcept

Return true if the ParameterDict is empty, otherwise return false.

template<typename Container>
inline void update(const Container &container)

Update the ParameterDict with the key-value pairs from another ParameterDict, overwriting existing key.

inline void clear()

Remove all parameters in the ParameterDict.

inline bool contains(const std::string &key) const noexcept

Check if the certain parameter with the key in the ParameterDict.

inline const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &get(const std::string &key) const

Returns the value associated with the given `key`.

Throws an exception if no such key is stored in the `ParameterDict`. Check contains(key) before for a non-throwing way of access

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) &get(const std::string &key)

Returns the value associated with the given `key`.

Throws an exception if no such key is stored in the `ParameterDict`. Check contains(key) before for a non-throwing way of access

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) &operator[](const std::string &key)

Returns the value associated with the given `key`.

Throws an exception if no such key is stored in the `ParameterDict`. Check contains(key) before for a non-throwing way of access

inline const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &operator[](const std::string &key) const

Returns the value associated with the given `key`.

Throws an exception if no such key is stored in the `ParameterDict`. Check contains(key) before for a non-throwing way of access