# Normalization Layers

Normalization layers stabilize and accelerate training by normalizing intermediate
activations. They help with gradient flow and allow higher learning rates.

- **BatchNorm**: Normalizes across batch dimension; most common in CNNs
- **InstanceNorm**: Normalizes each sample independently; popular in style transfer
- **LayerNorm**: Normalizes across feature dimension; standard in transformers
- **GroupNorm**: Normalizes within groups of channels; works with small batches
- **LocalResponseNorm**: Lateral inhibition inspired by neuroscience (less common today)

## BatchNorm1d / BatchNorm2d / BatchNorm3d

class BatchNorm1d : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<BatchNorm1dImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `BatchNorm1dImpl`.

See the documentation for `BatchNorm1dImpl` class to learn what methods it provides, and examples of how to use `BatchNorm1d` with `torch::nn::BatchNorm1dOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = BatchNorm1dImpl

class BatchNorm1dImpl : public torch::nn::BatchNormImplBase<1, BatchNorm1dImpl>

Applies the BatchNorm1d function.

See [https://pytorch.org/docs/main/nn.html#torch.nn.BatchNorm1d](https://pytorch.org/docs/main/nn.html#torch.nn.BatchNorm1d) to learn about the exact behavior of this module.

See the documentation for `torch::nn::BatchNorm1dOptions` class to learn what constructor arguments are supported for this module.

Example:

```
BatchNorm1d
model(BatchNorm1dOptions(4).eps(0.5).momentum(0.1).affine(false).track_running_stats(true));
```

class BatchNorm2d : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<BatchNorm2dImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `BatchNorm2dImpl`.

See the documentation for `BatchNorm2dImpl` class to learn what methods it provides, and examples of how to use `BatchNorm2d` with `torch::nn::BatchNorm2dOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = BatchNorm2dImpl

class BatchNorm2dImpl : public torch::nn::BatchNormImplBase<2, BatchNorm2dImpl>

Applies the BatchNorm2d function.

See [https://pytorch.org/docs/main/nn.html#torch.nn.BatchNorm2d](https://pytorch.org/docs/main/nn.html#torch.nn.BatchNorm2d) to learn about the exact behavior of this module.

See the documentation for `torch::nn::BatchNorm2dOptions` class to learn what constructor arguments are supported for this module.

Example:

```
BatchNorm2d
model(BatchNorm2dOptions(4).eps(0.5).momentum(0.1).affine(false).track_running_stats(true));
```

class BatchNorm3d : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<BatchNorm3dImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `BatchNorm3dImpl`.

See the documentation for `BatchNorm3dImpl` class to learn what methods it provides, and examples of how to use `BatchNorm3d` with `torch::nn::BatchNorm3dOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = BatchNorm3dImpl

class BatchNorm3dImpl : public torch::nn::BatchNormImplBase<3, BatchNorm3dImpl>

Applies the BatchNorm3d function.

See [https://pytorch.org/docs/main/nn.html#torch.nn.BatchNorm3d](https://pytorch.org/docs/main/nn.html#torch.nn.BatchNorm3d) to learn about the exact behavior of this module.

See the documentation for `torch::nn::BatchNorm3dOptions` class to learn what constructor arguments are supported for this module.

Example:

```
BatchNorm3d
model(BatchNorm3dOptions(4).eps(0.5).momentum(0.1).affine(false).track_running_stats(true));
```

**Example:**

```
auto bn = torch::nn::BatchNorm2d(
 torch::nn::BatchNorm2dOptions(64) // num_features
 .eps(1e-5)
 .momentum(0.1)
 .affine(true)
 .track_running_stats(true));
```

## InstanceNorm1d / InstanceNorm2d / InstanceNorm3d

class InstanceNorm1d : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<InstanceNorm1dImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `InstanceNorm1dImpl`.

See the documentation for `InstanceNorm1dImpl` class to learn what methods it provides, and examples of how to use `InstanceNorm1d` with `torch::nn::InstanceNorm1dOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = InstanceNorm1dImpl

class InstanceNorm1dImpl : public torch::nn::InstanceNormImpl<1, InstanceNorm1dImpl>

Applies the InstanceNorm1d function.

See [https://pytorch.org/docs/main/nn.html#torch.nn.InstanceNorm1d](https://pytorch.org/docs/main/nn.html#torch.nn.InstanceNorm1d) to learn about the exact behavior of this module.

See the documentation for `torch::nn::InstanceNorm1dOptions` class to learn what constructor arguments are supported for this module.

Example:

```
InstanceNorm1d
model(InstanceNorm1dOptions(4).eps(0.5).momentum(0.1).affine(false).track_running_stats(true));
```

class InstanceNorm2d : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<InstanceNorm2dImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `InstanceNorm2dImpl`.

See the documentation for `InstanceNorm2dImpl` class to learn what methods it provides, and examples of how to use `InstanceNorm2d` with `torch::nn::InstanceNorm2dOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = InstanceNorm2dImpl

class InstanceNorm2dImpl : public torch::nn::InstanceNormImpl<2, InstanceNorm2dImpl>

Applies the InstanceNorm2d function.

See [https://pytorch.org/docs/main/nn.html#torch.nn.InstanceNorm2d](https://pytorch.org/docs/main/nn.html#torch.nn.InstanceNorm2d) to learn about the exact behavior of this module.

See the documentation for `torch::nn::InstanceNorm2dOptions` class to learn what constructor arguments are supported for this module.

Example:

```
InstanceNorm2d
model(InstanceNorm2dOptions(4).eps(0.5).momentum(0.1).affine(false).track_running_stats(true));
```

class InstanceNorm3d : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<InstanceNorm3dImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `InstanceNorm3dImpl`.

See the documentation for `InstanceNorm3dImpl` class to learn what methods it provides, and examples of how to use `InstanceNorm3d` with `torch::nn::InstanceNorm3dOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = InstanceNorm3dImpl

class InstanceNorm3dImpl : public torch::nn::InstanceNormImpl<3, InstanceNorm3dImpl>

Applies the InstanceNorm3d function.

See [https://pytorch.org/docs/main/nn.html#torch.nn.InstanceNorm3d](https://pytorch.org/docs/main/nn.html#torch.nn.InstanceNorm3d) to learn about the exact behavior of this module.

See the documentation for `torch::nn::InstanceNorm3dOptions` class to learn what constructor arguments are supported for this module.

Example:

```
InstanceNorm3d
model(InstanceNorm3dOptions(4).eps(0.5).momentum(0.1).affine(false).track_running_stats(true));
```

## LayerNorm

class LayerNorm : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<LayerNormImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `LayerNormImpl`.

See the documentation for `LayerNormImpl` class to learn what methods it provides, and examples of how to use `LayerNorm` with `torch::nn::LayerNormOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = LayerNormImpl

class LayerNormImpl : public torch::nn::[Cloneable](utilities.html#_CPPv4I0EN5torch2nn9CloneableE)<LayerNormImpl>

Applies Layer Normalization over a mini-batch of inputs as described in the paper `Layer Normalization`_ .

See [https://pytorch.org/docs/main/nn.html#torch.nn.LayerNorm](https://pytorch.org/docs/main/nn.html#torch.nn.LayerNorm) to learn about the exact behavior of this module.

See the documentation for `torch::nn::LayerNormOptions` class to learn what constructor arguments are supported for this module.

Example:

```
LayerNorm model(LayerNormOptions({2,
2}).elementwise_affine(false).eps(2e-5));
```

Public Functions

inline LayerNormImpl(std::vector<int64_t> normalized_shape)

explicit LayerNormImpl(LayerNormOptions options_)

virtual void reset() override

`reset()` must perform initialization of all members with reference semantics, most importantly parameters, buffers and submodules.

void reset_parameters()

virtual void pretty_print(std::ostream &stream) const override

Pretty prints the `LayerNorm` module into the given `stream`.

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input)

Applies layer normalization over a mini-batch of inputs as described in the paper `Layer Normalization`_ .

The mean and standard-deviation are calculated separately over the last certain number dimensions which have to be of the shape specified by input `normalized_shape`.

`Layer Normalization`: [https://arxiv.org/abs/1607.06450](https://arxiv.org/abs/1607.06450)

Public Members

LayerNormOptions options

The options with which this module was constructed.

[Tensor](../aten/tensor.html#_CPPv46Tensorv) weight

The learned weight.

Initialized to ones if the `elementwise_affine` option is set to `true` upon construction.

[Tensor](../aten/tensor.html#_CPPv46Tensorv) bias

The learned bias.

Initialized to zeros `elementwise_affine` option is set to `true` upon construction.

**Example:**

```
auto ln = torch::nn::LayerNorm(
 torch::nn::LayerNormOptions({768})); // normalized_shape
```

## GroupNorm

class GroupNorm : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<GroupNormImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `GroupNormImpl`.

See the documentation for `GroupNormImpl` class to learn what methods it provides, and examples of how to use `GroupNorm` with `torch::nn::GroupNormOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = GroupNormImpl

class GroupNormImpl : public torch::nn::[Cloneable](utilities.html#_CPPv4I0EN5torch2nn9CloneableE)<GroupNormImpl>

Applies Group Normalization over a mini-batch of inputs as described in the paper `Group Normalization`_ .

See [https://pytorch.org/docs/main/nn.html#torch.nn.GroupNorm](https://pytorch.org/docs/main/nn.html#torch.nn.GroupNorm) to learn about the exact behavior of this module.

See the documentation for `torch::nn::GroupNormOptions` class to learn what constructor arguments are supported for this module.

Example:

```
GroupNorm model(GroupNormOptions(2, 2).eps(2e-5).affine(false));
```

Public Functions

inline GroupNormImpl(int64_t num_groups, int64_t num_channels)

explicit GroupNormImpl(const GroupNormOptions &options_)

virtual void reset() override

`reset()` must perform initialization of all members with reference semantics, most importantly parameters, buffers and submodules.

void reset_parameters()

virtual void pretty_print(std::ostream &stream) const override

Pretty prints the `GroupNorm` module into the given `stream`.

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input)

Public Members

GroupNormOptions options

The options with which this module was constructed.

[Tensor](../aten/tensor.html#_CPPv46Tensorv) weight

The learned weight.

[Tensor](../aten/tensor.html#_CPPv46Tensorv) bias

The learned bias.

**Example:**

```
auto gn = torch::nn::GroupNorm(
 torch::nn::GroupNormOptions(32, 256)); // num_groups, num_channels
```

## LocalResponseNorm

class LocalResponseNorm : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<LocalResponseNormImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `LocalResponseNormImpl`.

See the documentation for `LocalResponseNormImpl` class to learn what methods it provides, and examples of how to use `LocalResponseNorm` with `torch::nn::LocalResponseNormOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = LocalResponseNormImpl

class LocalResponseNormImpl : public torch::nn::[Cloneable](utilities.html#_CPPv4I0EN5torch2nn9CloneableE)<LocalResponseNormImpl>

Applies local response normalization over an input signal composed of several input planes, where channels occupy the second dimension.

Applies normalization across channels. See [https://pytorch.org/docs/main/nn.html#torch.nn.LocalResponseNorm](https://pytorch.org/docs/main/nn.html#torch.nn.LocalResponseNorm) to learn about the exact behavior of this module.

See the documentation for `torch::nn::LocalResponseNormOptions` class to learn what constructor arguments are supported for this module.

Example:

```
LocalResponseNorm
model(LocalResponseNormOptions(2).alpha(0.0002).beta(0.85).k(2.));
```

Public Functions

inline LocalResponseNormImpl(int64_t size)

explicit LocalResponseNormImpl(const LocalResponseNormOptions &options_)

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input)

virtual void reset() override

`reset()` must perform initialization of all members with reference semantics, most importantly parameters, buffers and submodules.

virtual void pretty_print(std::ostream &stream) const override

Pretty prints the `LocalResponseNormImpl` module into the given `stream`.

Public Members

LocalResponseNormOptions options

The options with which this `[Module](index.html#PyTorchclasstorch_1_1nn_1_1_module)` was constructed.