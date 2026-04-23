# Neural Network Modules (torch::nn)

The `torch::nn` namespace provides neural network building blocks that mirror
Python's `torch.nn` module. It uses a PIMPL (Pointer to Implementation) pattern
where user-facing classes like `Conv2d` wrap internal `Conv2dImpl` classes.

**When to use torch::nn:**

- Building neural network models in C++
- Creating custom layers and modules
- Porting Python models to C++ for production inference
- Training models entirely in C++

**Basic usage:**

```
#include <torch/torch.h>

// Define a simple model
struct Net : torch::nn::Module {
 torch::nn::Conv2d conv1{nullptr};
 torch::nn::Linear fc1{nullptr};

 Net() {
 conv1 = register_module("conv1", torch::nn::Conv2d(
 torch::nn::Conv2dOptions(1, 32, 3).stride(1).padding(1)));
 fc1 = register_module("fc1", torch::nn::Linear(32 * 28 * 28, 10));
 }

 torch::Tensor forward(torch::Tensor x) {
 x = torch::relu(conv1->forward(x));
 x = x.view({-1, 32 * 28 * 28});
 return fc1->forward(x);
 }
};

// Create and use the model
auto model = std::make_shared<Net>();
auto input = torch::randn({1, 1, 28, 28});
auto output = model->forward(input);
```

## Header Files

- `torch/nn.h` - Main neural network header (includes all modules)
- `torch/nn/module.h` - Base Module class
- `torch/nn/modules.h` - All module implementations
- `torch/nn/options.h` - Options structs for modules
- `torch/nn/functional.h` - Functional API

## Module Base Class

All neural network modules inherit from `torch::nn::Module`, which provides
parameter management, serialization, device/dtype conversion, and hooks.

class Module : public std::enable_shared_from_this<Module>

The base class for all modules in PyTorch.

- .. note::
- The design and implementation of this class is largely based on the Python
- API. You may want to consult the python documentation for
- :py:class:`pytorch:torch.nn.Module` for further clarification on certain
- methods or behavior.
- 

A `Module` is an abstraction over the implementation of some function or algorithm, possibly associated with some persistent data. A `Module` may contain further `Module`s ("submodules"), each with their own implementation, persistent data and further submodules. `Module`s can thus be said to form a recursive tree structure. A `Module` is registered as a submodule to another `Module` by calling `register_module()`, typically from within a parent module's constructor.

A distinction is made between three kinds of persistent data that may be associated with a `Module`:

1. *Parameters*: tensors that record gradients, typically weights updated during the backward step (e.g. the `weight` of a `[Linear](linear.html#PyTorchclasstorch_1_1nn_1_1_linear)` module),
2. *Buffers*: tensors that do not record gradients, typically updated during the forward step, such as running statistics (e.g. `mean` and `variance` in the `BatchNorm` module),
3. Any additional state, not necessarily tensors, required for the implementation or configuration of a `Module`.

The first two kinds of state are special in that they may be registered with the `Module` system to allow convenient access and batch configuration. For example, registered parameters in any `Module` may be iterated over via the `parameters()` accessor. Further, changing the data type of a `Module`'s registered parameters can be done conveniently via `Module::to()`, e.g. `module->to(torch::kCUDA)` to move all parameters to GPU memory. Lastly, registered parameters and buffers are handled specially during a `clone()` operation, which performs a deepcopy of a cloneable `Module` hierarchy.

Parameters are registered with a `Module` via `register_parameter`. Buffers are registered separately via `register_buffer`. These methods are part of the public API of `Module` and are typically invoked from within a concrete `Module`s constructor.

Subclassed by [torch::nn::Cloneable< SoftshrinkImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< PReLUImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< LogSoftmaxImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< L1LossImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< SequentialImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< HardshrinkImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< GLUImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< RReLUImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< ParameterDictImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< IdentityImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< FoldImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< EmbeddingBagImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< BilinearImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< TripletMarginWithDistanceLossImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< SoftminImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< SmoothL1LossImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< MultiLabelMarginLossImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< LeakyReLUImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< FunctionalImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< ELUImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< TanhshrinkImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< PairwiseDistanceImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< LogSigmoidImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< HardtanhImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< FractionalMaxPool2dImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< FlattenImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< CrossMapLRN2dImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< TransformerEncoderLayerImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< ThresholdImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< SoftsignImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< MultiMarginLossImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< FractionalMaxPool3dImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< CTCLossImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< UnfoldImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< SiLUImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< ParameterListImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< MultiheadAttentionImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< CELUImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< UpsampleImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< TransformerImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< SELUImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< PixelUnshuffleImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< LinearImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< HingeEmbeddingLossImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< EmbeddingImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< MultiLabelSoftMarginLossImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< CrossEntropyLossImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< TripletMarginLossImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< TransformerDecoderLayerImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< SoftMarginLossImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< LocalResponseNormImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< BCELossImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< LayerNormImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< AdaptiveLogSoftmaxWithLossImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< ReLUImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< ModuleListImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< HuberLossImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< GELUImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< SoftmaxImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< Softmax2dImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< SoftplusImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< SigmoidImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< PoissonNLLLossImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< ModuleDictImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< MishImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< UnflattenImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< ReLU6Impl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< MSELossImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< CosineSimilarityImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< CosineEmbeddingLossImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< TransformerDecoderImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< TanhImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< NLLLossImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< MarginRankingLossImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< BCEWithLogitsLossImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< TransformerEncoderImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< PixelShuffleImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< KLDivLossImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< GroupNormImpl >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable), [torch::nn::Cloneable< Derived >](utilities.html#PyTorchclasstorch_1_1nn_1_1_cloneable)

Public Types

using ModuleApplyFunction = std::function<void(Module&)>

using ConstModuleApplyFunction = std::function<void(const Module&)>

using NamedModuleApplyFunction = std::function<void(const std::string&, Module&)>

using ConstNamedModuleApplyFunction = std::function<void(const std::string&, const Module&)>

using ModulePointerApplyFunction = std::function<void(const std::shared_ptr<Module>&)>

using NamedModulePointerApplyFunction = std::function<void(const std::string&, const std::shared_ptr<Module>&)>

Public Functions

explicit Module(std::string name)

Tells the base `Module` about the name of the submodule.

Module()

Constructs the module without immediate knowledge of the submodule's name.

The name of the submodule is inferred via RTTI (if possible) the first time `.name()` is invoked.

Module(const Module&) = default

Module &operator=(const Module&) = default

Module(Module&&) noexcept = default

Module &operator=(Module&&) noexcept = default

virtual ~Module() = default

const std::string &name() const noexcept

Returns the name of the `Module`.

A `Module` has an associated `name`, which is a string representation of the kind of concrete `Module` it represents, such as `"Linear"` for the `[Linear](linear.html#PyTorchclasstorch_1_1nn_1_1_linear)` module. Under most circumstances, this name is automatically inferred via runtime type information (RTTI). In the unusual circumstance that you have this feature disabled, you may want to manually name your `Module`s by passing the string name to the `Module` base class' constructor.

virtual std::shared_ptr<Module> clone(const std::optional<Device> &device = std::nullopt) const

Performs a recursive deep copy of the module and all its registered parameters, buffers and submodules.

Optionally, this method sets the current device to the one supplied before cloning. If no device is given, each parameter and buffer will be moved to the device of its source.

- .. attention::
- Attempting to call the `clone()` method inherited from the base `Module`
- class (the one documented here) will fail. To inherit an actual
- implementation of `clone()`, you must subclass `Cloneable`. `Cloneable`
- is templatized on the concrete module type, and can thus properly copy a
- `Module`. This method is provided on the base class' API solely for an
- easier-to-use polymorphic interface.
- 

void apply(const ModuleApplyFunction &function)

Applies the `function` to the `Module` and recursively to every submodule.

The function must accept a `Module&`.

- .. code-block:: cpp
- 
- ```
MyModule module;
```
- ```
module->apply([](nn::Module& module) {
```
- ```
std::cout << module.name() << std::endl;
```
- ```
});
```
- 
- 

void apply(const ConstModuleApplyFunction &function) const

Applies the `function` to the `Module` and recursively to every submodule.

The function must accept a `const Module&`.

- .. code-block:: cpp
- 
- ```
MyModule module;
```
- ```
module->apply([](const nn::Module& module) {
```
- ```
std::cout << module.name() << std::endl;
```
- ```
});
```
- 
- 

void apply(const NamedModuleApplyFunction &function, const std::string &name_prefix = std::string())

Applies the `function` to the `Module` and recursively to every submodule.

The function must accept a `const std::string&` for the key of the module, and a `Module&`. The key of the module itself is the empty string. If `name_prefix` is given, it is prepended to every key as `<name_prefix>.<key>` (and just `name_prefix` for the module itself).

- .. code-block:: cpp
- 
- ```
MyModule module;
```
- ```
module->apply([](const std::string& key, nn::Module& module) {
```
- ```
std::cout << key << ": " << module.name() << std::endl;
```
- ```
});
```
- 
- 

void apply(const ConstNamedModuleApplyFunction &function, const std::string &name_prefix = std::string()) const

Applies the `function` to the `Module` and recursively to every submodule.

The function must accept a `const std::string&` for the key of the module, and a `const Module&`. The key of the module itself is the empty string. If `name_prefix` is given, it is prepended to every key as `<name_prefix>.<key>` (and just `name_prefix` for the module itself).

- .. code-block:: cpp
- 
- ```
MyModule module;
```
- ```
module->apply([](const std::string& key, const nn::Module& module) {
```
- ```
std::cout << key << ": " << module.name() << std::endl;
```
- ```
});
```
- 
- 

void apply(const ModulePointerApplyFunction &function) const

Applies the `function` to the `Module` and recursively to every submodule.

The function must accept a `const std::shared_ptr<Module>&`.

- .. code-block:: cpp
- 
- ```
MyModule module;
```
- ```
module->apply([](const std::shared_ptr<nn::Module>& module) {
```
- ```
std::cout << module->name() << std::endl;
```
- ```
});
```
- 
- 

void apply(const NamedModulePointerApplyFunction &function, const std::string &name_prefix = std::string()) const

Applies the `function` to the `Module` and recursively to every submodule.

The function must accept a `const std::string&` for the key of the module, and a `const std::shared_ptr<Module>&`. The key of the module itself is the empty string. If `name_prefix` is given, it is prepended to every key as `<name_prefix>.<key>` (and just `name_prefix` for the module itself).

- .. code-block:: cpp
- 
- ```
MyModule module;
```
- ```
module->apply([](const std::string& key,
```
- ```
const std::shared_ptr<nn::Module>& module) {
```
- ```
std::cout << key << ": " << module->name() << std::endl;
```
- ```
});
```
- 
- 

std::vector<[Tensor](../aten/tensor.html#_CPPv46Tensorv)> parameters(bool recurse = true) const

Returns the parameters of this `Module` and if `recurse` is true, also recursively of every submodule.

[OrderedDict](../library/registration.html#_CPPv4I00EN5torch11OrderedDictE)<std::string, [Tensor](../aten/tensor.html#_CPPv46Tensorv)> named_parameters(bool recurse = true) const

Returns an `[OrderedDict](../library/registration.html#PyTorchclasstorch_1_1_ordered_dict)` with the parameters of this `Module` along with their keys, and if `recurse` is true also recursively of every submodule.

std::vector<[Tensor](../aten/tensor.html#_CPPv46Tensorv)> buffers(bool recurse = true) const

Returns the buffers of this `Module` and if `recurse` is true, also recursively of every submodule.

[OrderedDict](../library/registration.html#_CPPv4I00EN5torch11OrderedDictE)<std::string, [Tensor](../aten/tensor.html#_CPPv46Tensorv)> named_buffers(bool recurse = true) const

Returns an `[OrderedDict](../library/registration.html#PyTorchclasstorch_1_1_ordered_dict)` with the buffers of this `Module` along with their keys, and if `recurse` is true also recursively of every submodule.

std::vector<std::shared_ptr<Module>> modules(bool include_self = true) const

Returns the submodules of this `Module` (the entire submodule hierarchy) and if `include_self` is true, also inserts a `shared_ptr` to this module in the first position.

- .. warning::
- Only pass `include_self` as `true` if this `Module` is stored in a
- `shared_ptr`! Otherwise an exception will be thrown. You may still call
- this method with `include_self` set to false if your `Module` is not
- stored in a `shared_ptr`.
- 

[OrderedDict](../library/registration.html#_CPPv4I00EN5torch11OrderedDictE)<std::string, std::shared_ptr<Module>> named_modules(const std::string &name_prefix = std::string(), bool include_self = true) const

Returns an `[OrderedDict](../library/registration.html#PyTorchclasstorch_1_1_ordered_dict)` of the submodules of this `Module` (the entire submodule hierarchy) and their keys, and if `include_self` is true, also inserts a `shared_ptr` to this module in the first position.

If `name_prefix` is given, it is prepended to every key as `<name_prefix>.<key>` (and just `name_prefix` for the module itself).

- .. warning::
- Only pass `include_self` as `true` if this `Module` is stored in a
- `shared_ptr`! Otherwise an exception will be thrown. You may still call
- this method with `include_self` set to false if your `Module` is not
- stored in a `shared_ptr`.
- 

std::vector<std::shared_ptr<Module>> children() const

Returns the direct submodules of this `Module`.

[OrderedDict](../library/registration.html#_CPPv4I00EN5torch11OrderedDictE)<std::string, std::shared_ptr<Module>> named_children() const

Returns an `[OrderedDict](../library/registration.html#PyTorchclasstorch_1_1_ordered_dict)` of the direct submodules of this `Module` and their keys.

virtual void train(bool on = true)

Enables "training" mode.

void eval()

Calls train(false) to enable "eval" mode.

Do not override this method, override `train()` instead.

virtual bool is_training() const noexcept

True if the module is in training mode.

Every `Module` has a boolean associated with it that determines whether the `Module` is currently in *training* mode (set via `.train()`) or in *evaluation* (inference) mode (set via `.eval()`). This property is exposed via `is_training()`, and may be used by the implementation of a concrete module to modify its runtime behavior. See the `BatchNorm` or `[Dropout](dropout.html#PyTorchclasstorch_1_1nn_1_1_dropout)` modules for examples of `Module`s that use different code paths depending on this property.

virtual void to(torch::Device device, torch::Dtype dtype, bool non_blocking = false)

Recursively casts all parameters to the given `dtype` and `device`.

If `non_blocking` is true and the source is in pinned memory and destination is on the GPU or vice versa, the copy is performed asynchronously with respect to the host. Otherwise, the argument has no effect.

virtual void to(torch::Dtype dtype, bool non_blocking = false)

Recursively casts all parameters to the given dtype.

If `non_blocking` is true and the source is in pinned memory and destination is on the GPU or vice versa, the copy is performed asynchronously with respect to the host. Otherwise, the argument has no effect.

virtual void to(torch::Device device, bool non_blocking = false)

Recursively moves all parameters to the given device.

If `non_blocking` is true and the source is in pinned memory and destination is on the GPU or vice versa, the copy is performed asynchronously with respect to the host. Otherwise, the argument has no effect.

virtual void zero_grad(bool set_to_none = true)

Recursively zeros out the `grad` value of each registered parameter.

template<typename ModuleType>
ModuleType::ContainedType *as() noexcept

Attempts to cast this `Module` to the given `ModuleType`.

This method is useful when calling `apply()`.

- .. code-block:: cpp
- 
- ```
void initialize_weights(nn::Module& module) {
```
- ```
torch::NoGradGuard no_grad;
```
- ```
if (auto* linear = module.as<nn::Linear>()) {
```
- ```
linear->weight.normal_(0.0, 0.02);
```
- ```
}
```
- ```
}
```
- 
- ```
MyModule module;
```
- ```
module->apply(initialize_weights);
```
- 
- 

template<typename ModuleType>
const ModuleType::ContainedType *as() const noexcept

Attempts to cast this `Module` to the given `ModuleType`.

This method is useful when calling `apply()`.

- .. code-block:: cpp
- 
- ```
void initialize_weights(nn::Module& module) {
```
- ```
torch::NoGradGuard no_grad;
```
- ```
if (auto* linear = module.as<nn::Linear>()) {
```
- ```
linear->weight.normal_(0.0, 0.02);
```
- ```
}
```
- ```
}
```
- 
- ```
MyModule module;
```
- ```
module->apply(initialize_weights);
```
- 
- 

template<typename ModuleType, typename = torch::detail::disable_if_module_holder_t<ModuleType>>
ModuleType *as() noexcept

Attempts to cast this `Module` to the given `ModuleType`.

This method is useful when calling `apply()`.

- .. code-block:: cpp
- 
- ```
void initialize_weights(nn::Module& module) {
```
- ```
torch::NoGradGuard no_grad;
```
- ```
if (auto* linear = module.as<nn::Linear>()) {
```
- ```
linear->weight.normal_(0.0, 0.02);
```
- ```
}
```
- ```
}
```
- 
- ```
MyModule module;
```
- ```
module.apply(initialize_weights);
```
- 
- 

template<typename ModuleType, typename = torch::detail::disable_if_module_holder_t<ModuleType>>
const ModuleType *as() const noexcept

Attempts to cast this `Module` to the given `ModuleType`.

This method is useful when calling `apply()`.

- .. code-block:: cpp
- 
- ```
void initialize_weights(nn::Module& module) {
```
- ```
torch::NoGradGuard no_grad;
```
- ```
if (auto* linear = module.as<nn::Linear>()) {
```
- ```
linear->weight.normal_(0.0, 0.02);
```
- ```
}
```
- ```
}
```
- 
- ```
MyModule module;
```
- ```
module.apply(initialize_weights);
```
- 
- 

virtual void save(serialize::[OutputArchive](../serialize/archives.html#_CPPv4N5torch9serialize13OutputArchiveE) &archive) const

Serializes the `Module` into the given `OutputArchive`.

If the `Module` contains unserializable submodules (e.g. `nn::Functional`), those submodules are skipped when serializing.

virtual void load(serialize::[InputArchive](../serialize/archives.html#_CPPv4N5torch9serialize12InputArchiveE) &archive)

Deserializes the `Module` from the given `InputArchive`.

If the `Module` contains unserializable submodules (e.g. `nn::Functional`), we don't check the existence of those submodules in the `InputArchive` when deserializing.

virtual void pretty_print(std::ostream &stream) const

Streams a pretty representation of the `Module` into the given `stream`.

By default, this representation will be the name of the module (taken from `name()`), followed by a recursive pretty print of all of the `Module`'s submodules.

Override this method to change the pretty print. The input `stream` should be returned from the method, to allow easy chaining.

virtual bool is_serializable() const

Returns whether the `Module` is serializable.

[Tensor](../aten/tensor.html#_CPPv46Tensorv) ®ister_parameter(std::string name, [Tensor](../aten/tensor.html#_CPPv46Tensorv) tensor, bool requires_grad = true)

Registers a parameter with this `Module`.

A parameter should be any gradient-recording tensor used in the implementation of your `Module`. Registering it makes it available to methods such as `parameters()`, `clone()` or `to().`

Note that registering an undefined Tensor (e.g. `module.register_parameter("param", Tensor())`) is allowed, and is equivalent to `module.register_parameter("param", None)` in Python API.

- .. code-block:: cpp
- 
- MyModule::MyModule() {
- ```
weight_ = register_parameter("weight", torch::randn({A, B}));
```
- }
- 

[Tensor](../aten/tensor.html#_CPPv46Tensorv) ®ister_buffer(std::string name, [Tensor](../aten/tensor.html#_CPPv46Tensorv) tensor)

Registers a buffer with this `Module`.

A buffer is intended to be state in your module that does not record gradients, such as running statistics. Registering it makes it available to methods such as `buffers()`, `clone()` or `to()`.

- .. code-block:: cpp
- 
- MyModule::MyModule() {
- ```
mean_ = register_buffer("mean", torch::empty({num_features_}));
```
- }
- 

template<typename ModuleType>
std::shared_ptr<ModuleType> register_module(std::string name, std::shared_ptr<ModuleType> module)

Registers a submodule with this `Module`.

Registering a module makes it available to methods such as `modules()`, `clone()` or `to()`.

- .. code-block:: cpp
- 
- MyModule::MyModule() {
- ```
submodule_ = register_module("linear", torch::nn::Linear(3, 4));
```
- }
- 

template<typename ModuleType>
std::shared_ptr<ModuleType> register_module(std::string name, [ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<ModuleType> module_holder)

Registers a submodule with this `Module`.

This method deals with `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)`s.

Registering a module makes it available to methods such as `modules()`, `clone()` or `to()`.

- .. code-block:: cpp
- 
- MyModule::MyModule() {
- ```
submodule_ = register_module("linear", torch::nn::Linear(3, 4));
```
- }
- 

template<typename ModuleType>
std::shared_ptr<ModuleType> replace_module(const std::string &name, std::shared_ptr<ModuleType> module)

Replaces a registered submodule with this `Module`.

This takes care of the registration, if you used submodule members, you should assign the submodule as well, i.e. use as module->submodule_ = module->replace_module("linear", [torch::nn::Linear(3, 4)](linear.html#PyTorchclasstorch_1_1nn_1_1_linear)); It only works when a module of the name is already registered.

This is useful for replacing a module after initialization, e.g. for finetuning.

template<typename ModuleType>
std::shared_ptr<ModuleType> replace_module(const std::string &name, [ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<ModuleType> module_holder)

Replaces a registered submodule with this `Module`.

This method deals with `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)`s.

This takes care of the registration, if you used submodule members, you should assign the submodule as well, i.e. use as module->submodule_ = module->replace_module("linear", linear_holder); It only works when a module of the name is already registered.

This is useful for replacing a module after initialization, e.g. for finetuning.

void unregister_module(const std::string &name)

Unregisters a submodule from this `Module`.

If there is no such module with `name` an exception is thrown.

**Key features:**

- `register_module()`: Register submodules for parameter tracking
- `register_parameter()`: Register learnable parameters
- `register_buffer()`: Register non-learnable state (e.g., running mean)
- `parameters()` / `named_parameters()`: Iterate over all parameters
- `to()`: Move module to a device or convert dtype
- `train()` / `eval()`: Toggle training/evaluation mode
- `save()` / `load()`: Serialize and deserialize module state

## Module Categories

- [Containers](containers.html)
- [Convolution Layers](convolution.html)
- [Pooling Layers](pooling.html)
- [Linear Layers](linear.html)
- [Activation Functions](activation.html)
- [Normalization Layers](normalization.html)
- [Dropout Layers](dropout.html)
- [Embedding Layers](embedding.html)
- [Recurrent Layers](recurrent.html)
- [Transformer Layers](transformer.html)
- [Loss Functions](loss.html)
- [Functional API](functional.html)
- [Utilities](utilities.html)