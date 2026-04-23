# Pooling Layers

Pooling layers reduce spatial dimensions by aggregating values in local regions,
providing translation invariance and reducing computational cost in deeper layers.

- **MaxPool**: Takes the maximum value in each pooling window (preserves strong features)
- **AvgPool**: Takes the average value in each pooling window (smoother downsampling)
- **AdaptivePool**: Automatically calculates kernel size to produce a target output size
- **FractionalMaxPool**: Randomized pooling with fractional output size
- **MaxUnpool**: Computes the partial inverse of MaxPool using stored indices
- **LPPool**: Power-average pooling (generalization of avg/max pooling)

## MaxPool1d / MaxPool2d / MaxPool3d

class MaxPool1d : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<MaxPool1dImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `MaxPool1dImpl`.

See the documentation for `MaxPool1dImpl` class to learn what methods it provides, and examples of how to use `MaxPool1d` with `torch::nn::MaxPool1dOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = MaxPool1dImpl

class MaxPool1dImpl : public torch::nn::MaxPoolImpl<1, MaxPool1dImpl>

Applies maxpool over a 1-D input.

See [https://pytorch.org/docs/main/nn.html#torch.nn.MaxPool1d](https://pytorch.org/docs/main/nn.html#torch.nn.MaxPool1d) to learn about the exact behavior of this module.

See the documentation for `torch::nn::MaxPool1dOptions` class to learn what constructor arguments are supported for this module.

Example:

```
MaxPool1d model(MaxPool1dOptions(3).stride(2));
```

Public Functions

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input)

std::tuple<[Tensor](../aten/tensor.html#_CPPv46Tensorv), [Tensor](../aten/tensor.html#_CPPv46Tensorv)> forward_with_indices(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input)

Returns the outputs and the indices of the max values.

Useful for `torch::nn::MaxUnpool1d` later.

class MaxPool2d : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<MaxPool2dImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `MaxPool2dImpl`.

See the documentation for `MaxPool2dImpl` class to learn what methods it provides, and examples of how to use `MaxPool2d` with `torch::nn::MaxPool2dOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = MaxPool2dImpl

class MaxPool2dImpl : public torch::nn::MaxPoolImpl<2, MaxPool2dImpl>

Applies maxpool over a 2-D input.

See [https://pytorch.org/docs/main/nn.html#torch.nn.MaxPool2d](https://pytorch.org/docs/main/nn.html#torch.nn.MaxPool2d) to learn about the exact behavior of this module.

See the documentation for `torch::nn::MaxPool2dOptions` class to learn what constructor arguments are supported for this module.

Example:

```
MaxPool2d model(MaxPool2dOptions({3, 2}).stride({2, 2}));
```

Public Functions

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input)

std::tuple<[Tensor](../aten/tensor.html#_CPPv46Tensorv), [Tensor](../aten/tensor.html#_CPPv46Tensorv)> forward_with_indices(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input)

Returns the outputs and the indices of the max values.

Useful for `torch::nn::MaxUnpool2d` later.

class MaxPool3d : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<MaxPool3dImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `MaxPool3dImpl`.

See the documentation for `MaxPool3dImpl` class to learn what methods it provides, and examples of how to use `MaxPool3d` with `torch::nn::MaxPool3dOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = MaxPool3dImpl

class MaxPool3dImpl : public torch::nn::MaxPoolImpl<3, MaxPool3dImpl>

Applies maxpool over a 3-D input.

See [https://pytorch.org/docs/main/nn.html#torch.nn.MaxPool3d](https://pytorch.org/docs/main/nn.html#torch.nn.MaxPool3d) to learn about the exact behavior of this module.

See the documentation for `torch::nn::MaxPool3dOptions` class to learn what constructor arguments are supported for this module.

Example:

```
MaxPool3d model(MaxPool3dOptions(3).stride(2));
```

Public Functions

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input)

std::tuple<[Tensor](../aten/tensor.html#_CPPv46Tensorv), [Tensor](../aten/tensor.html#_CPPv46Tensorv)> forward_with_indices(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input)

Returns the outputs and the indices of the max values.

Useful for `torch::nn::MaxUnpool3d` later.

**Example:**

```
auto pool = torch::nn::MaxPool2d(
 torch::nn::MaxPool2dOptions(2).stride(2));
```

## AvgPool1d / AvgPool2d / AvgPool3d

class AvgPool1d : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<AvgPool1dImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `AvgPool1dImpl`.

See the documentation for `AvgPool1dImpl` class to learn what methods it provides, and examples of how to use `AvgPool1d` with `torch::nn::AvgPool1dOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = AvgPool1dImpl

class AvgPool1dImpl : public torch::nn::AvgPoolImpl<1, AvgPool1dImpl>

Applies avgpool over a 1-D input.

See [https://pytorch.org/docs/main/nn.html#torch.nn.AvgPool1d](https://pytorch.org/docs/main/nn.html#torch.nn.AvgPool1d) to learn about the exact behavior of this module.

See the documentation for `torch::nn::AvgPool1dOptions` class to learn what constructor arguments are supported for this module.

Example:

```
AvgPool1d model(AvgPool1dOptions(3).stride(2));
```

Public Functions

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input)

class AvgPool2d : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<AvgPool2dImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `AvgPool2dImpl`.

See the documentation for `AvgPool2dImpl` class to learn what methods it provides, and examples of how to use `AvgPool2d` with `torch::nn::AvgPool2dOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = AvgPool2dImpl

class AvgPool2dImpl : public torch::nn::AvgPoolImpl<2, AvgPool2dImpl>

Applies avgpool over a 2-D input.

See [https://pytorch.org/docs/main/nn.html#torch.nn.AvgPool2d](https://pytorch.org/docs/main/nn.html#torch.nn.AvgPool2d) to learn about the exact behavior of this module.

See the documentation for `torch::nn::AvgPool2dOptions` class to learn what constructor arguments are supported for this module.

Example:

```
AvgPool2d model(AvgPool2dOptions({3, 2}).stride({2, 2}));
```

Public Functions

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input)

class AvgPool3d : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<AvgPool3dImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `AvgPool3dImpl`.

See the documentation for `AvgPool3dImpl` class to learn what methods it provides, and examples of how to use `AvgPool3d` with `torch::nn::AvgPool3dOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = AvgPool3dImpl

class AvgPool3dImpl : public torch::nn::AvgPoolImpl<3, AvgPool3dImpl>

Applies avgpool over a 3-D input.

See [https://pytorch.org/docs/main/nn.html#torch.nn.AvgPool3d](https://pytorch.org/docs/main/nn.html#torch.nn.AvgPool3d) to learn about the exact behavior of this module.

See the documentation for `torch::nn::AvgPool3dOptions` class to learn what constructor arguments are supported for this module.

Example:

```
AvgPool3d model(AvgPool3dOptions(5).stride(2));
```

Public Functions

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input)

## AdaptiveAvgPool1d / AdaptiveAvgPool2d / AdaptiveAvgPool3d

class AdaptiveAvgPool1d : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<AdaptiveAvgPool1dImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `AdaptiveAvgPool1dImpl`.

See the documentation for `AdaptiveAvgPool1dImpl` class to learn what methods it provides, and examples of how to use `AdaptiveAvgPool1d` with `torch::nn::AdaptiveAvgPool1dOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = AdaptiveAvgPool1dImpl

class AdaptiveAvgPool1dImpl : public torch::nn::AdaptiveAvgPoolImpl<1, ExpandingArray<1>, AdaptiveAvgPool1dImpl>

Applies adaptive avgpool over a 1-D input.

See [https://pytorch.org/docs/main/nn.html#torch.nn.AdaptiveAvgPool1d](https://pytorch.org/docs/main/nn.html#torch.nn.AdaptiveAvgPool1d) to learn about the exact behavior of this module.

See the documentation for `torch::nn::AdaptiveAvgPool1dOptions` class to learn what constructor arguments are supported for this module.

Example:

```
AdaptiveAvgPool1d model(AdaptiveAvgPool1dOptions(5));
```

Public Functions

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input)

class AdaptiveAvgPool2d : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<AdaptiveAvgPool2dImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `AdaptiveAvgPool2dImpl`.

See the documentation for `AdaptiveAvgPool2dImpl` class to learn what methods it provides, and examples of how to use `AdaptiveAvgPool2d` with `torch::nn::AdaptiveAvgPool2dOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = AdaptiveAvgPool2dImpl

class AdaptiveAvgPool2dImpl : public torch::nn::AdaptiveAvgPoolImpl<2, ExpandingArrayWithOptionalElem<2>, AdaptiveAvgPool2dImpl>

Applies adaptive avgpool over a 2-D input.

See [https://pytorch.org/docs/main/nn.html#torch.nn.AdaptiveAvgPool2d](https://pytorch.org/docs/main/nn.html#torch.nn.AdaptiveAvgPool2d) to learn about the exact behavior of this module.

See the documentation for `torch::nn::AdaptiveAvgPool2dOptions` class to learn what constructor arguments are supported for this module.

Example:

```
AdaptiveAvgPool2d model(AdaptiveAvgPool2dOptions({3, 2}));
```

Public Functions

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input)

class AdaptiveAvgPool3d : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<AdaptiveAvgPool3dImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `AdaptiveAvgPool3dImpl`.

See the documentation for `AdaptiveAvgPool3dImpl` class to learn what methods it provides, and examples of how to use `AdaptiveAvgPool3d` with `torch::nn::AdaptiveAvgPool3dOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = AdaptiveAvgPool3dImpl

class AdaptiveAvgPool3dImpl : public torch::nn::AdaptiveAvgPoolImpl<3, ExpandingArrayWithOptionalElem<3>, AdaptiveAvgPool3dImpl>

Applies adaptive avgpool over a 3-D input.

See [https://pytorch.org/docs/main/nn.html#torch.nn.AdaptiveAvgPool3d](https://pytorch.org/docs/main/nn.html#torch.nn.AdaptiveAvgPool3d) to learn about the exact behavior of this module.

See the documentation for `torch::nn::AdaptiveAvgPool3dOptions` class to learn what constructor arguments are supported for this module.

Example:

```
AdaptiveAvgPool3d model(AdaptiveAvgPool3dOptions(3));
```

Public Functions

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input)

**Example:**

```
// Output will always be 7x7 regardless of input size
auto adaptive_pool = torch::nn::AdaptiveAvgPool2d(
 torch::nn::AdaptiveAvgPool2dOptions({7, 7}));
```

## AdaptiveMaxPool1d / AdaptiveMaxPool2d / AdaptiveMaxPool3d

class AdaptiveMaxPool1d : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<AdaptiveMaxPool1dImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `AdaptiveMaxPool1dImpl`.

See the documentation for `AdaptiveMaxPool1dImpl` class to learn what methods it provides, and examples of how to use `AdaptiveMaxPool1d` with `torch::nn::AdaptiveMaxPool1dOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = AdaptiveMaxPool1dImpl

class AdaptiveMaxPool1dImpl : public torch::nn::AdaptiveMaxPoolImpl<1, ExpandingArray<1>, AdaptiveMaxPool1dImpl>

Applies adaptive maxpool over a 1-D input.

See [https://pytorch.org/docs/main/nn.html#torch.nn.AdaptiveMaxPool1d](https://pytorch.org/docs/main/nn.html#torch.nn.AdaptiveMaxPool1d) to learn about the exact behavior of this module.

See the documentation for `torch::nn::AdaptiveMaxPool1dOptions` class to learn what constructor arguments are supported for this module.

Example:

```
AdaptiveMaxPool1d model(AdaptiveMaxPool1dOptions(3));
```

Public Functions

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input)

std::tuple<[Tensor](../aten/tensor.html#_CPPv46Tensorv), [Tensor](../aten/tensor.html#_CPPv46Tensorv)> forward_with_indices(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input)

Returns the indices along with the outputs.

Useful to pass to nn.MaxUnpool1d.

class AdaptiveMaxPool2d : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<AdaptiveMaxPool2dImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `AdaptiveMaxPool2dImpl`.

See the documentation for `AdaptiveMaxPool2dImpl` class to learn what methods it provides, and examples of how to use `AdaptiveMaxPool2d` with `torch::nn::AdaptiveMaxPool2dOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = AdaptiveMaxPool2dImpl

class AdaptiveMaxPool2dImpl : public torch::nn::AdaptiveMaxPoolImpl<2, ExpandingArrayWithOptionalElem<2>, AdaptiveMaxPool2dImpl>

Applies adaptive maxpool over a 2-D input.

See [https://pytorch.org/docs/main/nn.html#torch.nn.AdaptiveMaxPool2d](https://pytorch.org/docs/main/nn.html#torch.nn.AdaptiveMaxPool2d) to learn about the exact behavior of this module.

See the documentation for `torch::nn::AdaptiveMaxPool2dOptions` class to learn what constructor arguments are supported for this module.

Example:

```
AdaptiveMaxPool2d model(AdaptiveMaxPool2dOptions({3, 2}));
```

Public Functions

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input)

std::tuple<[Tensor](../aten/tensor.html#_CPPv46Tensorv), [Tensor](../aten/tensor.html#_CPPv46Tensorv)> forward_with_indices(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input)

Returns the indices along with the outputs.

Useful to pass to nn.MaxUnpool2d.

class AdaptiveMaxPool3d : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<AdaptiveMaxPool3dImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `AdaptiveMaxPool3dImpl`.

See the documentation for `AdaptiveMaxPool3dImpl` class to learn what methods it provides, and examples of how to use `AdaptiveMaxPool3d` with `torch::nn::AdaptiveMaxPool3dOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = AdaptiveMaxPool3dImpl

class AdaptiveMaxPool3dImpl : public torch::nn::AdaptiveMaxPoolImpl<3, ExpandingArrayWithOptionalElem<3>, AdaptiveMaxPool3dImpl>

Applies adaptive maxpool over a 3-D input.

See [https://pytorch.org/docs/main/nn.html#torch.nn.AdaptiveMaxPool3d](https://pytorch.org/docs/main/nn.html#torch.nn.AdaptiveMaxPool3d) to learn about the exact behavior of this module.

See the documentation for `torch::nn::AdaptiveMaxPool3dOptions` class to learn what constructor arguments are supported for this module.

Example:

```
AdaptiveMaxPool3d model(AdaptiveMaxPool3dOptions(3));
```

Public Functions

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input)

std::tuple<[Tensor](../aten/tensor.html#_CPPv46Tensorv), [Tensor](../aten/tensor.html#_CPPv46Tensorv)> forward_with_indices(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input)

Returns the indices along with the outputs.

Useful to pass to nn.MaxUnpool3d.

## FractionalMaxPool2d / FractionalMaxPool3d

class FractionalMaxPool2d : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<FractionalMaxPool2dImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `FractionalMaxPool2dImpl`.

See the documentation for `FractionalMaxPool2dImpl` class to learn what methods it provides, and examples of how to use `FractionalMaxPool2d` with `torch::nn::FractionalMaxPool2dOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = FractionalMaxPool2dImpl

class FractionalMaxPool2dImpl : public torch::nn::[Cloneable](utilities.html#_CPPv4I0EN5torch2nn9CloneableE)<FractionalMaxPool2dImpl>

Applies fractional maxpool over a 2-D input.

See [https://pytorch.org/docs/main/nn.html#torch.nn.FractionalMaxPool2d](https://pytorch.org/docs/main/nn.html#torch.nn.FractionalMaxPool2d) to learn about the exact behavior of this module.

See the documentation for `torch::nn::FractionalMaxPool2dOptions` class to learn what constructor arguments are supported for this module.

Example:

```
FractionalMaxPool2d model(FractionalMaxPool2dOptions(5).output_size(1));
```

Public Functions

inline FractionalMaxPool2dImpl(ExpandingArray<2> kernel_size)

explicit FractionalMaxPool2dImpl(FractionalMaxPool2dOptions options_)

virtual void reset() override

`reset()` must perform initialization of all members with reference semantics, most importantly parameters, buffers and submodules.

virtual void pretty_print(std::ostream &stream) const override

Pretty prints the `FractionalMaxPool2d` module into the given `stream`.

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input)

std::tuple<[Tensor](../aten/tensor.html#_CPPv46Tensorv), [Tensor](../aten/tensor.html#_CPPv46Tensorv)> forward_with_indices(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input)

Returns the outputs and the indices of the max values.

Useful for `torch::nn::MaxUnpool2d` later.

Public Members

FractionalMaxPool2dOptions options

The options with which this `[Module](index.html#PyTorchclasstorch_1_1nn_1_1_module)` was constructed.

[Tensor](../aten/tensor.html#_CPPv46Tensorv) _random_samples

class FractionalMaxPool3d : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<FractionalMaxPool3dImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `FractionalMaxPool3dImpl`.

See the documentation for `FractionalMaxPool3dImpl` class to learn what methods it provides, and examples of how to use `FractionalMaxPool3d` with `torch::nn::FractionalMaxPool3dOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = FractionalMaxPool3dImpl

class FractionalMaxPool3dImpl : public torch::nn::[Cloneable](utilities.html#_CPPv4I0EN5torch2nn9CloneableE)<FractionalMaxPool3dImpl>

Applies fractional maxpool over a 3-D input.

See [https://pytorch.org/docs/main/nn.html#torch.nn.FractionalMaxPool3d](https://pytorch.org/docs/main/nn.html#torch.nn.FractionalMaxPool3d) to learn about the exact behavior of this module.

See the documentation for `torch::nn::FractionalMaxPool3dOptions` class to learn what constructor arguments are supported for this module.

Example:

```
FractionalMaxPool3d model(FractionalMaxPool3dOptions(5).output_size(1));
```

Public Functions

inline FractionalMaxPool3dImpl(ExpandingArray<3> kernel_size)

explicit FractionalMaxPool3dImpl(FractionalMaxPool3dOptions options_)

virtual void reset() override

`reset()` must perform initialization of all members with reference semantics, most importantly parameters, buffers and submodules.

virtual void pretty_print(std::ostream &stream) const override

Pretty prints the `FractionalMaxPool3d` module into the given `stream`.

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input)

std::tuple<[Tensor](../aten/tensor.html#_CPPv46Tensorv), [Tensor](../aten/tensor.html#_CPPv46Tensorv)> forward_with_indices(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input)

Returns the outputs and the indices of the max values.

Useful for `torch::nn::MaxUnpool3d` later.

Public Members

FractionalMaxPool3dOptions options

The options with which this `[Module](index.html#PyTorchclasstorch_1_1nn_1_1_module)` was constructed.

[Tensor](../aten/tensor.html#_CPPv46Tensorv) _random_samples

## MaxUnpool1d / MaxUnpool2d / MaxUnpool3d

Computes a partial inverse of `MaxPool`, using the indices of the maximum
values computed during pooling to place values back into unpooled positions.

class MaxUnpool1d : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<MaxUnpool1dImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `MaxUnpool1dImpl`.

See the documentation for `MaxUnpool1dImpl` class to learn what methods it provides, and examples of how to use `MaxUnpool1d` with `torch::nn::MaxUnpool1dOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = MaxUnpool1dImpl

class MaxUnpool1dImpl : public torch::nn::MaxUnpoolImpl<1, MaxUnpool1dImpl>

Applies maxunpool over a 1-D input.

See [https://pytorch.org/docs/main/nn.html#torch.nn.MaxUnpool1d](https://pytorch.org/docs/main/nn.html#torch.nn.MaxUnpool1d) to learn about the exact behavior of this module.

See the documentation for `torch::nn::MaxUnpool1dOptions` class to learn what constructor arguments are supported for this module.

Example:

```
MaxUnpool1d model(MaxUnpool1dOptions(3).stride(2).padding(1));
```

Public Functions

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &indices, const std::optional<std::vector<int64_t>> &output_size = std::nullopt)

Friends

*friend struct* torch::nn::AnyModuleHolder

class MaxUnpool2d : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<MaxUnpool2dImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `MaxUnpool2dImpl`.

See the documentation for `MaxUnpool2dImpl` class to learn what methods it provides, and examples of how to use `MaxUnpool2d` with `torch::nn::MaxUnpool2dOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = MaxUnpool2dImpl

class MaxUnpool2dImpl : public torch::nn::MaxUnpoolImpl<2, MaxUnpool2dImpl>

Applies maxunpool over a 2-D input.

See [https://pytorch.org/docs/main/nn.html#torch.nn.MaxUnpool2d](https://pytorch.org/docs/main/nn.html#torch.nn.MaxUnpool2d) to learn about the exact behavior of this module.

See the documentation for `torch::nn::MaxUnpool2dOptions` class to learn what constructor arguments are supported for this module.

Example:

```
MaxUnpool2d model(MaxUnpool2dOptions(3).stride(2).padding(1));
```

Public Functions

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &indices, const std::optional<std::vector<int64_t>> &output_size = std::nullopt)

Friends

*friend struct* torch::nn::AnyModuleHolder

class MaxUnpool3d : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<MaxUnpool3dImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `MaxUnpool3dImpl`.

See the documentation for `MaxUnpool3dImpl` class to learn what methods it provides, and examples of how to use `MaxUnpool3d` with `torch::nn::MaxUnpool3dOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = MaxUnpool3dImpl

class MaxUnpool3dImpl : public torch::nn::MaxUnpoolImpl<3, MaxUnpool3dImpl>

Applies maxunpool over a 3-D input.

See [https://pytorch.org/docs/main/nn.html#torch.nn.MaxUnpool3d](https://pytorch.org/docs/main/nn.html#torch.nn.MaxUnpool3d) to learn about the exact behavior of this module.

See the documentation for `torch::nn::MaxUnpool3dOptions` class to learn what constructor arguments are supported for this module.

Example:

```
MaxUnpool3d model(MaxUnpool3dOptions(3).stride(2).padding(1));
```

Public Functions

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &indices, const std::optional<std::vector<int64_t>> &output_size = std::nullopt)

Friends

*friend struct* torch::nn::AnyModuleHolder

**Example:**

```
auto pool = torch::nn::MaxPool2d(
 torch::nn::MaxPool2dOptions(2).stride(2).return_indices(true));
auto unpool = torch::nn::MaxUnpool2d(
 torch::nn::MaxUnpoolOptions<2>(2).stride(2));

auto [output, indices] = pool->forward_with_indices(input);
auto reconstructed = unpool->forward(output, indices);
```

## LPPool1d / LPPool2d / LPPool3d

class LPPool1d : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<LPPool1dImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `LPPool1dImpl`.

See the documentation for `LPPool1dImpl` class to learn what methods it provides, and examples of how to use `LPPool1d` with `torch::nn::LPPool1dOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = LPPool1dImpl

class LPPool1dImpl : public torch::nn::LPPoolImpl<1, LPPool1dImpl>

Applies the LPPool1d function element-wise.

See [https://pytorch.org/docs/main/nn.html#torch.nn.LPPool1d](https://pytorch.org/docs/main/nn.html#torch.nn.LPPool1d) to learn about the exact behavior of this module.

See the documentation for `torch::nn::LPPool1dOptions` class to learn what constructor arguments are supported for this module.

Example:

```
LPPool1d model(LPPool1dOptions(1, 2).stride(5).ceil_mode(true));
```

Public Functions

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input)

class LPPool2d : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<LPPool2dImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `LPPool2dImpl`.

See the documentation for `LPPool2dImpl` class to learn what methods it provides, and examples of how to use `LPPool2d` with `torch::nn::LPPool2dOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = LPPool2dImpl

class LPPool2dImpl : public torch::nn::LPPoolImpl<2, LPPool2dImpl>

Applies the LPPool2d function element-wise.

See [https://pytorch.org/docs/main/nn.html#torch.nn.LPPool2d](https://pytorch.org/docs/main/nn.html#torch.nn.LPPool2d) to learn about the exact behavior of this module.

See the documentation for `torch::nn::LPPool2dOptions` class to learn what constructor arguments are supported for this module.

Example:

```
LPPool2d model(LPPool2dOptions(1, std::vector<int64_t>({3, 4})).stride({5,
6}).ceil_mode(true));
```

Public Functions

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input)

class LPPool3d : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<LPPool3dImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `LPPool3dImpl`.

See the documentation for `LPPool3dImpl` class to learn what methods it provides, and examples of how to use `LPPool3d` with `torch::nn::LPPool3dOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = LPPool3dImpl

class LPPool3dImpl : public torch::nn::LPPoolImpl<3, LPPool3dImpl>

Applies the LPPool3d function element-wise.

See [https://pytorch.org/docs/main/nn.html#torch.nn.LPPool3d](https://pytorch.org/docs/main/nn.html#torch.nn.LPPool3d) to learn about the exact behavior of this module.

See the documentation for `torch::nn::LPPool3dOptions` class to learn what constructor arguments are supported for this module.

Example:

```
LPPool3d model(LPPool3dOptions(1, std::vector<int64_t>({3, 4, 5})).stride(
{5, 6, 7}).ceil_mode(true));
```

Public Functions

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input)