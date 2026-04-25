# Utilities

Additional utilities for building neural networks: parameter initialization,
module cloning, type-erased containers, padding layers, and vision utilities.

## Parameter Initialization

The `torch::nn::init` namespace provides functions for initializing module parameters:

```
#include <torch/nn/init.h>

// Xavier/Glorot initialization
torch::nn::init::xavier_uniform_(linear->weight);
torch::nn::init::xavier_normal_(linear->weight);

// Kaiming/He initialization
torch::nn::init::kaiming_uniform_(conv->weight, /*a=*/0, torch::kFanIn, torch::kReLU);
torch::nn::init::kaiming_normal_(conv->weight);

// Other initializations
torch::nn::init::zeros_(linear->bias);
torch::nn::init::ones_(bn->weight);
torch::nn::init::constant_(linear->bias, 0.1);
torch::nn::init::normal_(linear->weight, /*mean=*/0, /*std=*/0.01);
torch::nn::init::uniform_(linear->weight, /*a=*/-0.1, /*b=*/0.1);
torch::nn::init::orthogonal_(rnn->weight_hh);
```

## Cloneable

template<typename Derived>
class Cloneable : public torch::nn::[Module](index.html#_CPPv4N5torch2nn6ModuleE)

The `clone()` method in the base `[Module](index.html#PyTorchclasstorch_1_1nn_1_1_module)` class does not have knowledge of the concrete runtime type of its subclasses.

Therefore, `clone()` must either be called from within the subclass, or from a base class that has knowledge of the concrete type. `Cloneable` uses the CRTP to gain knowledge of the subclass' static type and provide an implementation of the `clone()` method. We do not want to use this pattern in the base class, because then storing a module would always require templatizing it.

Public Functions

virtual void reset() = 0

`reset()` must perform initialization of all members with reference semantics, most importantly parameters, buffers and submodules.

inline virtual std::shared_ptr<Module> clone(const std::optional<Device> &device = std::nullopt) const override

Performs a recursive "deep copy" of the `[Module](index.html#PyTorchclasstorch_1_1nn_1_1_module)`, such that all parameters and submodules in the cloned module are different from those in the original module.

explicit Module(std::string name)

Tells the base `[Module](index.html#PyTorchclasstorch_1_1nn_1_1_module)` about the name of the submodule.

Module()

Constructs the module without immediate knowledge of the submodule's name.

The name of the submodule is inferred via RTTI (if possible) the first time `.[name()](index.html#PyTorchclasstorch_1_1nn_1_1_module_1a09407fc8e8eca674c18dd4436f1a7ad2)` is invoked.

Module(const Module&) = default

Module(Module&&) noexcept = default

All `torch::nn` modules inherit from `Cloneable`, enabling deep copies:

```
auto model = torch::nn::Linear(10, 5);
auto model_copy = std::dynamic_pointer_cast<torch::nn::LinearImpl>(model->clone());
```

## AnyModule

`AnyModule` provides type-erased storage for any module, allowing you to
store heterogeneous modules in a single container.

class AnyModule

Stores a type erased `[Module](index.html#PyTorchclasstorch_1_1nn_1_1_module)`.

The PyTorch C++ API does not impose an interface on the signature of `forward()` in `[Module](index.html#PyTorchclasstorch_1_1nn_1_1_module)` subclasses. This gives you complete freedom to design your `forward()` methods to your liking. However, this also means there is no unified base type you could store in order to call `forward()` polymorphically for any module. This is where the `AnyModule` comes in. Instead of inheritance, it relies on type erasure for polymorphism.

An `AnyModule` can store any `[nn::Module](index.html#PyTorchclasstorch_1_1nn_1_1_module)` subclass that provides a `forward()` method. This `forward()` may accept any types and return any type. Once stored in an `AnyModule`, you can invoke the underlying module's `forward()` by calling `AnyModule::forward()` with the arguments you would supply to the stored module (though see one important limitation below). Example:

- .. code-block:: cpp
- 
- struct GenericTrainer {
- ```
torch::nn::AnyModule module;
```
- 
- ```
void train(torch::Tensor input) {
```
- ```
module.forward(input);
```
- ```
}
```
- };
- 
- GenericTrainer trainer1{torch::nn::Linear(3, 4)};
- GenericTrainer trainer2{torch::nn::Conv2d(3, 4, 2)};
- 

As `AnyModule` erases the static type of the stored module (and its `forward()` method) to achieve polymorphism, type checking of arguments is moved to runtime. That is, passing an argument with an incorrect type to an `AnyModule` will compile, but throw an exception at runtime:

- .. code-block:: cpp
- 
- torch::nn::AnyModule module(torch::nn::Linear(3, 4));
- // Linear takes a tensor as input, but we are passing an integer.
- // This will compile, but throw a `torch::Error` exception at runtime.
- module.forward(123);
- 

- .. attention::
- One noteworthy limitation of `AnyModule` is that its `forward()` method
- does not support implicit conversion of argument types. For example, if
- the stored module's `forward()` method accepts a `float` and you call
- `any_module.forward(3.4)` (where `3.4` is a `double`), this will throw
- an exception.
- 

The return type of the `AnyModule`'s `forward()` method is controlled via the first template argument to `AnyModule::forward()`. It defaults to `torch::Tensor`. To change it, you can write `any_module.forward<int>()`, for example.

- .. code-block:: cpp
- 
- torch::nn::AnyModule module(torch::nn::Linear(3, 4));
- auto output = module.forward(torch::ones({2, 3}));
- 
- struct IntModule {
- ```
int forward(int x) { return x; }
```
- };
- torch::nn::AnyModule module(IntModule{});
- int output = module.forward(5);
- 

The only other method an `AnyModule` provides access to on the stored module is `clone()`. However, you may acquire a handle on the module via `.ptr()`, which returns a `shared_ptr<[nn::Module](index.html#PyTorchclasstorch_1_1nn_1_1_module)>`. Further, if you know the concrete type of the stored module, you can get a concrete handle to it using `.get<T>()` where `T` is the concrete module type.

- .. code-block:: cpp
- 
- torch::nn::AnyModule module(torch::nn::Linear(3, 4));
- std::shared_ptr[nn::Module](nn::Module) ptr = module.ptr();
- torch::nn::Linear linear(module.get[torch::nn::Linear](torch::nn::Linear)());
- 

Public Functions

AnyModule() = default

A default-constructed `AnyModule` is in an empty state.

template<typename ModuleType>
explicit AnyModule(std::shared_ptr<ModuleType> module)

Constructs an `AnyModule` from a `shared_ptr` to concrete module object.

template<typename ModuleType, typename = torch::detail::enable_if_module_t<ModuleType>>
explicit AnyModule(ModuleType &&module)

Constructs an `AnyModule` from a concrete module object.

template<typename ModuleType>
explicit AnyModule(const ModuleHolder<ModuleType> &module_holder)

Constructs an `AnyModule` from a module holder.

AnyModule(AnyModule&&) = default

Move construction and assignment is allowed, and follows the default behavior of move for `std::unique_ptr`.

AnyModule &operator=(AnyModule&&) = default

inline AnyModule(const AnyModule &other)

Creates a shallow copy of an `AnyModule`.

inline AnyModule &operator=(const AnyModule &other)

inline AnyModule clone(std::optional<Device> device = std::nullopt) const

Creates a deep copy of an `AnyModule` if it contains a module, else an empty `AnyModule` if it is empty.

template<typename ModuleType>
AnyModule &operator=(std::shared_ptr<ModuleType> module)

Assigns a module to the `AnyModule` (to circumvent the explicit constructor).

template<typename ...ArgumentTypes>
AnyValue any_forward(ArgumentTypes&&... arguments)

Invokes `forward()` on the contained module with the given arguments, and returns the return value as an `AnyValue`.

Use this method when chaining `AnyModule`s in a loop.

template<typename ReturnType = torch::Tensor, typename ...ArgumentTypes>
ReturnType forward(ArgumentTypes&&... arguments)

Invokes `forward()` on the contained module with the given arguments, and casts the returned `AnyValue` to the supplied `ReturnType` (which defaults to `torch::Tensor`).

template<typename T, typename = torch::detail::enable_if_module_t<T>>
T &get()

Attempts to cast the underlying module to the given module type.

Throws an exception if the types do not match.

template<typename T, typename = torch::detail::enable_if_module_t<T>>
const T &get() const

Attempts to cast the underlying module to the given module type.

Throws an exception if the types do not match.

template<typename T, typename ContainedType = typename T::ContainedType>
T get() const

Returns the contained module in a `nn::ModuleHolder` subclass if possible (i.e.

if `T` has a constructor for the underlying module type).

inline std::shared_ptr<[Module](index.html#_CPPv4N5torch2nn6ModuleE)> ptr() const

Returns a `std::shared_ptr` whose dynamic type is that of the underlying module.

template<typename T, typename = torch::detail::enable_if_module_t<T>>
std::shared_ptr<T> ptr() const

Like `ptr()`, but casts the pointer to the given type.

inline const std::type_info &type_info() const

Returns the `type_info` object of the contained value.

inline bool is_empty() const noexcept

Returns true if the `AnyModule` does not contain a module.

**Example:**

```
torch::nn::AnyModule any_module(torch::nn::Linear(10, 5));
auto output = any_module.forward(input);
```

## Functional

Wraps a function or callable as a module, useful for inserting arbitrary
functions into a `Sequential` container.

class FunctionalImpl : public torch::nn::Cloneable<FunctionalImpl>

Wraps a function in a `[Module](index.html#PyTorchclasstorch_1_1nn_1_1_module)`.

The `Functional` module allows wrapping an arbitrary function or function object in an `[nn::Module](index.html#PyTorchclasstorch_1_1nn_1_1_module)`. This is primarily handy for usage in `[Sequential](containers.html#PyTorchclasstorch_1_1nn_1_1_sequential)`.

- .. code-block:: cpp
- 
- Sequential sequential(
- ```
Linear(3, 4),
```
- ```
Functional(torch::relu),
```
- ```
BatchNorm1d(3),
```
- ```
Functional(torch::elu, /*alpha=*‍/1));
```
- 

While a `Functional` module only accepts a single `Tensor` as input, it is possible for the wrapped function to accept further arguments. However, these have to be bound *at construction time*. For example, if you want to wrap `torch::leaky_relu`, which accepts a `slope` scalar as its second argument, with a particular value for its `slope` in a `Functional` module, you could write

- .. code-block:: cpp
- 
- Functional(torch::leaky_relu, /*slope=*‍/0.5)
- 

The value of `0.5` is then stored within the `Functional` object and supplied to the function call at invocation time. Note that such bound values are evaluated eagerly and stored a single time. See the documentation of [std::bind](https://en.cppreference.com/w/cpp/utility/functional/bind) for more information on the semantics of argument binding.

- .. attention::
- After passing any bound arguments, the function must accept a single
- tensor and return a single tensor.
- 

Note that `Functional` overloads the call operator (`operator()`) such that you can invoke it with `my_func(...)`.

Public Types

using Function = std::function<[Tensor](../aten/tensor.html#_CPPv46Tensorv)([Tensor](../aten/tensor.html#_CPPv46Tensorv))>

Public Functions

explicit FunctionalImpl(Function function)

Constructs a `Functional` from a function object.

template<typename SomeFunction, typename ...Args, typename = std::enable_if_t<(sizeof...(Args) > 0)>>
inline explicit FunctionalImpl(SomeFunction original_function, Args&&... args)

virtual void reset() override

`reset()` must perform initialization of all members with reference semantics, most importantly parameters, buffers and submodules.

virtual void pretty_print(std::ostream &stream) const override

Pretty prints the `Functional` module into the given `stream`.

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward([Tensor](../aten/tensor.html#_CPPv46Tensorv) input)

Forwards the `input` tensor to the underlying (bound) function object.

[Tensor](../aten/tensor.html#_CPPv46Tensorv) operator()([Tensor](../aten/tensor.html#_CPPv46Tensorv) input)

Calls forward(input).

virtual bool is_serializable() const override

Returns whether the `[Module](index.html#PyTorchclasstorch_1_1nn_1_1_module)` is serializable.

## ModuleHolder

template<typename Contained>
class ModuleHolder : private torch::detail::ModuleHolderIndicator

A `ModuleHolder` is essentially a wrapper around `std::shared_ptr<M>` where `M` is an `[nn::Module](index.html#PyTorchclasstorch_1_1nn_1_1_module)` subclass, with convenient constructors defined for the kind of constructions we want to allow for our modules.

Public Types

using ContainedType = Contained

Public Functions

inline ModuleHolder()

Default constructs the contained module if it has a default constructor, else produces a static error.

NOTE: This uses the behavior of template classes in C++ that constructors (or any methods) are only compiled when actually used.

inline ModuleHolder(std::nullptr_t)

Constructs the `ModuleHolder` with an empty contained value.

Access to the underlying module is not permitted and will throw an exception, until a value is assigned.

template<typename Head, typename ...Tail, typename = std::enable_if_t<!(torch::detail::is_module_holder_of<Head, ContainedType>::value && (sizeof...(Tail) == 0))>>
inline explicit ModuleHolder(Head &&head, Tail&&... tail)

Constructs the `ModuleHolder` with a contained module, forwarding all arguments to its constructor.

inline ModuleHolder(std::shared_ptr<Contained> module)

Constructs the `ModuleHolder` from a pointer to the contained type.

Example: `[Linear](linear.html#PyTorchclasstorch_1_1nn_1_1_linear)(std::make_shared<[LinearImpl](linear.html#PyTorchclasstorch_1_1nn_1_1_linear_impl)>(...))`.

inline explicit operator bool() const noexcept

Returns true if the `ModuleHolder` contains a module, or false if it is `nullptr`.

inline Contained *operator->()

Forwards to the contained module.

inline const Contained *operator->() const

Forwards to the contained module.

inline Contained &operator*()

Returns a reference to the contained module.

inline const Contained &operator*() const

Returns a const reference to the contained module.

inline const std::shared_ptr<Contained> &ptr() const

Returns a shared pointer to the underlying module.

inline Contained *get()

Returns a pointer to the underlying module.

inline const Contained *get() const

Returns a const pointer to the underlying module.

template<typename ...Args>
inline auto operator()(Args&&... args) -> torch::detail::return_type_of_forward_t<Contained, Args...>

Calls the `forward()` method of the contained module.

template<typename Arg>
inline auto operator[](Arg &&arg)

Forwards to the subscript operator of the contained module.

NOTE: std::forward is qualified to prevent VS2017 emitting error C2872: 'std': ambiguous symbol

inline bool is_empty() const noexcept

Returns true if the `ModuleHolder` does not contain a module.

## CosineSimilarity

class CosineSimilarity : public torch::nn::ModuleHolder<CosineSimilarityImpl>

A `ModuleHolder` subclass for `CosineSimilarityImpl`.

See the documentation for `CosineSimilarityImpl` class to learn what methods it provides, and examples of how to use `CosineSimilarity` with `torch::nn::CosineSimilarityOptions`. See the documentation for `ModuleHolder` to learn about PyTorch's module storage semantics.

Public Types

using Impl = CosineSimilarityImpl

## PairwiseDistance

class PairwiseDistance : public torch::nn::ModuleHolder<PairwiseDistanceImpl>

A `ModuleHolder` subclass for `PairwiseDistanceImpl`.

See the documentation for `PairwiseDistanceImpl` class to learn what methods it provides, and examples of how to use `PairwiseDistance` with `torch::nn::PairwiseDistanceOptions`. See the documentation for `ModuleHolder` to learn about PyTorch's module storage semantics.

Public Types

using Impl = PairwiseDistanceImpl

## PackedSequence

class torch::nn::utils::rnn::PackedSequence

Holds the data and list of `batch_sizes` of a packed sequence.
All RNN modules accept packed sequences as inputs.

const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &data() const

Returns the packed tensor containing all sequence elements.

const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &batch_sizes() const

Returns a 1D tensor of batch sizes at each time step.

const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &sorted_indices() const

Returns indices used to sort sequences by length (descending).

const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &unsorted_indices() const

Returns indices to restore the original sequence order.

PackedSequence to(torch::Device device) const

Moves the packed sequence to the specified device.

See also: `torch::nn::utils::rnn::pack_padded_sequence` and
`torch::nn::utils::rnn::pad_packed_sequence`.

## Padding Layers

### ReflectionPad1d / ReflectionPad2d / ReflectionPad3d

class ReflectionPad1d : public torch::nn::ModuleHolder<ReflectionPad1dImpl>

A `ModuleHolder` subclass for `ReflectionPad1dImpl`.

See the documentation for `ReflectionPad1dImpl` class to learn what methods it provides, and examples of how to use `ReflectionPad1d` with `torch::nn::ReflectionPad1dOptions`. See the documentation for `ModuleHolder` to learn about PyTorch's module storage semantics.

Public Types

using Impl = ReflectionPad1dImpl

class ReflectionPad2d : public torch::nn::ModuleHolder<ReflectionPad2dImpl>

A `ModuleHolder` subclass for `ReflectionPad2dImpl`.

See the documentation for `ReflectionPad2dImpl` class to learn what methods it provides, and examples of how to use `ReflectionPad2d` with `torch::nn::ReflectionPad2dOptions`. See the documentation for `ModuleHolder` to learn about PyTorch's module storage semantics.

Public Types

using Impl = ReflectionPad2dImpl

class ReflectionPad3d : public torch::nn::ModuleHolder<ReflectionPad3dImpl>

A `ModuleHolder` subclass for `ReflectionPad3dImpl`.

See the documentation for `ReflectionPad3dImpl` class to learn what methods it provides, and examples of how to use `ReflectionPad3d` with `torch::nn::ReflectionPad3dOptions`. See the documentation for `ModuleHolder` to learn about PyTorch's module storage semantics.

Public Types

using Impl = ReflectionPad3dImpl

### ReplicationPad1d / ReplicationPad2d / ReplicationPad3d

class ReplicationPad1d : public torch::nn::ModuleHolder<ReplicationPad1dImpl>

A `ModuleHolder` subclass for `ReplicationPad1dImpl`.

See the documentation for `ReplicationPad1dImpl` class to learn what methods it provides, and examples of how to use `ReplicationPad1d` with `torch::nn::ReplicationPad1dOptions`. See the documentation for `ModuleHolder` to learn about PyTorch's module storage semantics.

Public Types

using Impl = ReplicationPad1dImpl

class ReplicationPad2d : public torch::nn::ModuleHolder<ReplicationPad2dImpl>

A `ModuleHolder` subclass for `ReplicationPad2dImpl`.

See the documentation for `ReplicationPad2dImpl` class to learn what methods it provides, and examples of how to use `ReplicationPad2d` with `torch::nn::ReplicationPad2dOptions`. See the documentation for `ModuleHolder` to learn about PyTorch's module storage semantics.

Public Types

using Impl = ReplicationPad2dImpl

class ReplicationPad3d : public torch::nn::ModuleHolder<ReplicationPad3dImpl>

A `ModuleHolder` subclass for `ReplicationPad3dImpl`.

See the documentation for `ReplicationPad3dImpl` class to learn what methods it provides, and examples of how to use `ReplicationPad3d` with `torch::nn::ReplicationPad3dOptions`. See the documentation for `ModuleHolder` to learn about PyTorch's module storage semantics.

Public Types

using Impl = ReplicationPad3dImpl

### ZeroPad1d / ZeroPad2d / ZeroPad3d

class ZeroPad1d : public torch::nn::ModuleHolder<ZeroPad1dImpl>

A `ModuleHolder` subclass for `ZeroPad1dImpl`.

See the documentation for `ZeroPad1dImpl` class to learn what methods it provides, and examples of how to use `ZeroPad1d` with `torch::nn::ZeroPad1dOptions`. See the documentation for `ModuleHolder` to learn about PyTorch's module storage semantics.

Public Types

using Impl = ZeroPad1dImpl

class ZeroPad2d : public torch::nn::ModuleHolder<ZeroPad2dImpl>

A `ModuleHolder` subclass for `ZeroPad2dImpl`.

See the documentation for `ZeroPad2dImpl` class to learn what methods it provides, and examples of how to use `ZeroPad2d` with `torch::nn::ZeroPad2dOptions`. See the documentation for `ModuleHolder` to learn about PyTorch's module storage semantics.

Public Types

using Impl = ZeroPad2dImpl

class ZeroPad3d : public torch::nn::ModuleHolder<ZeroPad3dImpl>

A `ModuleHolder` subclass for `ZeroPad3dImpl`.

See the documentation for `ZeroPad3dImpl` class to learn what methods it provides, and examples of how to use `ZeroPad3d` with `torch::nn::ZeroPad3dOptions`. See the documentation for `ModuleHolder` to learn about PyTorch's module storage semantics.

Public Types

using Impl = ZeroPad3dImpl

### ConstantPad1d / ConstantPad2d / ConstantPad3d

class ConstantPad1d : public torch::nn::ModuleHolder<ConstantPad1dImpl>

A `ModuleHolder` subclass for `ConstantPad1dImpl`.

See the documentation for `ConstantPad1dImpl` class to learn what methods it provides, and examples of how to use `ConstantPad1d` with `torch::nn::ConstantPad1dOptions`. See the documentation for `ModuleHolder` to learn about PyTorch's module storage semantics.

Public Types

using Impl = ConstantPad1dImpl

class ConstantPad2d : public torch::nn::ModuleHolder<ConstantPad2dImpl>

A `ModuleHolder` subclass for `ConstantPad2dImpl`.

See the documentation for `ConstantPad2dImpl` class to learn what methods it provides, and examples of how to use `ConstantPad2d` with `torch::nn::ConstantPad2dOptions`. See the documentation for `ModuleHolder` to learn about PyTorch's module storage semantics.

Public Types

using Impl = ConstantPad2dImpl

class ConstantPad3d : public torch::nn::ModuleHolder<ConstantPad3dImpl>

A `ModuleHolder` subclass for `ConstantPad3dImpl`.

See the documentation for `ConstantPad3dImpl` class to learn what methods it provides, and examples of how to use `ConstantPad3d` with `torch::nn::ConstantPad3dOptions`. See the documentation for `ModuleHolder` to learn about PyTorch's module storage semantics.

Public Types

using Impl = ConstantPad3dImpl

## Vision Layers

### PixelShuffle

class PixelShuffle : public torch::nn::ModuleHolder<PixelShuffleImpl>

A `ModuleHolder` subclass for `PixelShuffleImpl`.

See the documentation for `PixelShuffleImpl` class to learn what methods it provides, and examples of how to use `PixelShuffle` with `torch::nn::PixelShuffleOptions`. See the documentation for `ModuleHolder` to learn about PyTorch's module storage semantics.

Public Types

using Impl = PixelShuffleImpl

struct PixelShuffleOptions

Options for the `PixelShuffle` module.

Example:

```
PixelShuffle model(PixelShuffleOptions(5));
```

Public Functions

inline PixelShuffleOptions(int64_t upscale_factor)

inline auto upscale_factor(const int64_t &new_upscale_factor) -> decltype(*this)

Factor to increase spatial resolution by.

inline auto upscale_factor(int64_t &&new_upscale_factor) -> decltype(*this)

inline const int64_t &upscale_factor() const noexcept

inline int64_t &upscale_factor() noexcept

### PixelUnshuffle

class PixelUnshuffle : public torch::nn::ModuleHolder<PixelUnshuffleImpl>

A `ModuleHolder` subclass for `PixelUnshuffleImpl`.

See the documentation for `PixelUnshuffleImpl` class to learn what methods it provides, and examples of how to use `PixelUnshuffle` with `torch::nn::PixelUnshuffleOptions`. See the documentation for `ModuleHolder` to learn about PyTorch's module storage semantics.

Public Types

using Impl = PixelUnshuffleImpl

struct PixelUnshuffleOptions

Options for the `PixelUnshuffle` module.

Example:

```
PixelUnshuffle model(PixelUnshuffleOptions(5));
```

Public Functions

inline PixelUnshuffleOptions(int64_t downscale_factor)

inline auto downscale_factor(const int64_t &new_downscale_factor) -> decltype(*this)

Factor to decrease spatial resolution by.

inline auto downscale_factor(int64_t &&new_downscale_factor) -> decltype(*this)

inline const int64_t &downscale_factor() const noexcept

inline int64_t &downscale_factor() noexcept

### Upsample

class Upsample : public torch::nn::ModuleHolder<UpsampleImpl>

A `ModuleHolder` subclass for `UpsampleImpl`.

See the documentation for `UpsampleImpl` class to learn what methods it provides, and examples of how to use `Upsample` with `torch::nn::UpsampleOptions`. See the documentation for `ModuleHolder` to learn about PyTorch's module storage semantics.

Public Types

using Impl = UpsampleImpl

struct UpsampleOptions

Options for the `Upsample` module.

Example:

```
Upsample
model(UpsampleOptions().scale_factor(std::vector<double>({3})).mode(torch::kLinear).align_corners(false));
```

Public Functions

inline auto size(const std::optional<std::vector<int64_t>> &new_size) -> decltype(*this)

output spatial sizes.

inline auto size(std::optional<std::vector<int64_t>> &&new_size) -> decltype(*this)

inline const std::optional<std::vector<int64_t>> &size() const noexcept

inline std::optional<std::vector<int64_t>> &size() noexcept

inline auto scale_factor(const std::optional<std::vector<double>> &new_scale_factor) -> decltype(*this)

multiplier for spatial size.

inline auto scale_factor(std::optional<std::vector<double>> &&new_scale_factor) -> decltype(*this)

inline const std::optional<std::vector<double>> &scale_factor() const noexcept

inline std::optional<std::vector<double>> &scale_factor() noexcept

inline auto mode(const mode_t &new_mode) -> decltype(*this)

inline auto mode(mode_t &&new_mode) -> decltype(*this)

inline const mode_t &mode() const noexcept

inline mode_t &mode() noexcept

inline auto align_corners(const std::optional<bool> &new_align_corners) -> decltype(*this)

if "True", the corner pixels of the input and output tensors are aligned, and thus preserving the values at those pixels.

This only has effect when :attr:`mode` is "linear", "bilinear", "bicubic", or "trilinear". Default: "False"

inline auto align_corners(std::optional<bool> &&new_align_corners) -> decltype(*this)

inline const std::optional<bool> &align_corners() const noexcept

inline std::optional<bool> &align_corners() noexcept

### Fold / Unfold

class Fold : public torch::nn::ModuleHolder<FoldImpl>

A `ModuleHolder` subclass for `FoldImpl`.

See the documentation for `FoldImpl` class to learn what methods it provides, and examples of how to use `Fold` with `torch::nn::FoldOptions`. See the documentation for `ModuleHolder` to learn about PyTorch's module storage semantics.

Public Types

using Impl = FoldImpl

struct FoldOptions

Options for the `Fold` module.

Example:

```
Fold model(FoldOptions({8, 8}, {3, 3}).dilation(2).padding({2,
1}).stride(2));
```

Public Functions

inline FoldOptions(ExpandingArray<2> output_size, ExpandingArray<2> kernel_size)

inline auto output_size(const ExpandingArray<2> &new_output_size) -> decltype(*this)

describes the spatial shape of the large containing tensor of the sliding local blocks.

It is useful to resolve the ambiguity when multiple input shapes map to same number of sliding blocks, e.g., with stride > 0.

inline auto output_size(ExpandingArray<2> &&new_output_size) -> decltype(*this)

inline const ExpandingArray<2> &output_size() const noexcept

inline ExpandingArray<2> &output_size() noexcept

inline auto kernel_size(const ExpandingArray<2> &new_kernel_size) -> decltype(*this)

the size of the sliding blocks

inline auto kernel_size(ExpandingArray<2> &&new_kernel_size) -> decltype(*this)

inline const ExpandingArray<2> &kernel_size() const noexcept

inline ExpandingArray<2> &kernel_size() noexcept

inline auto dilation(const ExpandingArray<2> &new_dilation) -> decltype(*this)

controls the spacing between the kernel points; also known as the à trous algorithm.

inline auto dilation(ExpandingArray<2> &&new_dilation) -> decltype(*this)

inline const ExpandingArray<2> &dilation() const noexcept

inline ExpandingArray<2> &dilation() noexcept

inline auto padding(const ExpandingArray<2> &new_padding) -> decltype(*this)

controls the amount of implicit zero-paddings on both sides for padding number of points for each dimension before reshaping.

inline auto padding(ExpandingArray<2> &&new_padding) -> decltype(*this)

inline const ExpandingArray<2> &padding() const noexcept

inline ExpandingArray<2> &padding() noexcept

inline auto stride(const ExpandingArray<2> &new_stride) -> decltype(*this)

controls the stride for the sliding blocks.

inline auto stride(ExpandingArray<2> &&new_stride) -> decltype(*this)

inline const ExpandingArray<2> &stride() const noexcept

inline ExpandingArray<2> &stride() noexcept

class Unfold : public torch::nn::ModuleHolder<UnfoldImpl>

A `ModuleHolder` subclass for `UnfoldImpl`.

See the documentation for `UnfoldImpl` class to learn what methods it provides, and examples of how to use `Unfold` with `torch::nn::UnfoldOptions`. See the documentation for `ModuleHolder` to learn about PyTorch's module storage semantics.

Public Types

using Impl = UnfoldImpl

struct UnfoldOptions

Options for the `Unfold` module.

Example:

```
Unfold model(UnfoldOptions({2, 4}).dilation(2).padding({2, 1}).stride(2));
```

Public Functions

inline UnfoldOptions(ExpandingArray<2> kernel_size)

inline auto kernel_size(const ExpandingArray<2> &new_kernel_size) -> decltype(*this)

the size of the sliding blocks

inline auto kernel_size(ExpandingArray<2> &&new_kernel_size) -> decltype(*this)

inline const ExpandingArray<2> &kernel_size() const noexcept

inline ExpandingArray<2> &kernel_size() noexcept

inline auto dilation(const ExpandingArray<2> &new_dilation) -> decltype(*this)

controls the spacing between the kernel points; also known as the à trous algorithm.

inline auto dilation(ExpandingArray<2> &&new_dilation) -> decltype(*this)

inline const ExpandingArray<2> &dilation() const noexcept

inline ExpandingArray<2> &dilation() noexcept

inline auto padding(const ExpandingArray<2> &new_padding) -> decltype(*this)

controls the amount of implicit zero-paddings on both sides for padding number of points for each dimension before reshaping.

inline auto padding(ExpandingArray<2> &&new_padding) -> decltype(*this)

inline const ExpandingArray<2> &padding() const noexcept

inline ExpandingArray<2> &padding() noexcept

inline auto stride(const ExpandingArray<2> &new_stride) -> decltype(*this)

controls the stride for the sliding blocks.

inline auto stride(ExpandingArray<2> &&new_stride) -> decltype(*this)

inline const ExpandingArray<2> &stride() const noexcept

inline ExpandingArray<2> &stride() noexcept