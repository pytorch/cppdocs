# Linear Layers

Linear layers apply affine transformations to input data: `y = xW^T + b`.
They are the building blocks of fully-connected networks and are used for
feature transformation, classification heads, and projection layers.

- **Linear**: Standard fully-connected layer
- **Bilinear**: Bilinear transformation of two inputs
- **Identity**: Pass-through layer (useful for skip connections)
- **Flatten/Unflatten**: Reshape tensors between convolutional and linear layers

## Linear

class Linear : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<LinearImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `LinearImpl`.

See the documentation for `LinearImpl` class to learn what methods it provides, and examples of how to use `Linear` with `torch::nn::LinearOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = LinearImpl

class LinearImpl : public torch::nn::[Cloneable](utilities.html#_CPPv4I0EN5torch2nn9CloneableE)<LinearImpl>

Applies a linear transformation with optional bias.

See [https://pytorch.org/docs/main/generated/torch.nn.Linear.html](https://pytorch.org/docs/main/generated/torch.nn.Linear.html) to learn about the exact behavior of this module.

See the documentation for `torch::nn::LinearOptions` class to learn what constructor arguments are supported for this module.

Example:

```
Linear model(LinearOptions(5, 2).bias(false));
```

Public Functions

inline LinearImpl(int64_t in_features, int64_t out_features)

explicit LinearImpl(const LinearOptions &options_)

virtual void reset() override

`reset()` must perform initialization of all members with reference semantics, most importantly parameters, buffers and submodules.

void reset_parameters()

virtual void pretty_print(std::ostream &stream) const override

Pretty prints the `Linear` module into the given `stream`.

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input)

Transforms the `input` tensor by multiplying with the `weight` and optionally adding the `bias`, if `with_bias` is true in the options.

Public Members

LinearOptions options

The options used to configure this module.

[Tensor](../aten/tensor.html#_CPPv46Tensorv) weight

The learned weight.

[Tensor](../aten/tensor.html#_CPPv46Tensorv) bias

The learned bias.

If `bias` is false in the `options`, this tensor is undefined.

**Example:**

```
auto linear = torch::nn::Linear(torch::nn::LinearOptions(784, 256).bias(true));
auto output = linear->forward(input); // input: [N, 784]
```

## Bilinear

class Bilinear : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<BilinearImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `BilinearImpl`.

See the documentation for `BilinearImpl` class to learn what methods it provides, and examples of how to use `Bilinear` with `torch::nn::BilinearOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = BilinearImpl

class BilinearImpl : public torch::nn::[Cloneable](utilities.html#_CPPv4I0EN5torch2nn9CloneableE)<BilinearImpl>

Applies a billinear transformation with optional bias.

See [https://pytorch.org/docs/main/generated/torch.nn.Bilinear.html](https://pytorch.org/docs/main/generated/torch.nn.Bilinear.html) to learn about the exact behavior of this module.

See the documentation for `torch::nn::BilinearOptions` class to learn what constructor arguments are supported for this module.

Example:

```
Bilinear model(BilinearOptions(3, 2, 4).bias(false));
```

Public Functions

inline BilinearImpl(int64_t in1_features, int64_t in2_features, int64_t out_features)

explicit BilinearImpl(const BilinearOptions &options_)

virtual void reset() override

`reset()` must perform initialization of all members with reference semantics, most importantly parameters, buffers and submodules.

void reset_parameters()

virtual void pretty_print(std::ostream &stream) const override

Pretty prints the `Bilinear` module into the given `stream`.

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input1, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input2)

Applies a bilinear transform on the `input1` and `input2` tensor by multiplying with the `weight` and optionally adding the `bias`, if `with_bias` is true in the options.

Public Members

BilinearOptions options

The options used to configure this module.

[Tensor](../aten/tensor.html#_CPPv46Tensorv) weight

The learned weight.

[Tensor](../aten/tensor.html#_CPPv46Tensorv) bias

The learned bias.

If `with_bias` is false in the `options`, this tensor is undefined.

## Identity

class Identity : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<IdentityImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `IdentityImpl`.

See the documentation for `IdentityImpl` class to learn what methods it provides, or the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = IdentityImpl

class IdentityImpl : public torch::nn::[Cloneable](utilities.html#_CPPv4I0EN5torch2nn9CloneableE)<IdentityImpl>

A placeholder identity operator that is argument-insensitive.

See [https://pytorch.org/docs/main/generated/torch.nn.Identity.html](https://pytorch.org/docs/main/generated/torch.nn.Identity.html) to learn about the exact behavior of this module.

Public Functions

virtual void reset() override

`reset()` must perform initialization of all members with reference semantics, most importantly parameters, buffers and submodules.

virtual void pretty_print(std::ostream &stream) const override

Pretty prints the `Identity` module into the given `stream`.

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input)

## Flatten

class Flatten : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<FlattenImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `FlattenImpl`.

See the documentation for `FlattenImpl` class to learn what methods it provides, and examples of how to use `Flatten` with `torch::nn::FlattenOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = FlattenImpl

class FlattenImpl : public torch::nn::[Cloneable](utilities.html#_CPPv4I0EN5torch2nn9CloneableE)<FlattenImpl>

A placeholder for Flatten operator See [https://pytorch.org/docs/main/generated/torch.nn.Flatten.html](https://pytorch.org/docs/main/generated/torch.nn.Flatten.html) to learn about the exact behavior of this module.

See the documentation for `torch::nn::FlattenOptions` class to learn what constructor arguments are supported for this module.

Example:

```
Flatten model(FlattenOptions().start_dim(2).end_dim(4));
```

Public Functions

explicit FlattenImpl(const FlattenOptions &options_ = {})

virtual void reset() override

`reset()` must perform initialization of all members with reference semantics, most importantly parameters, buffers and submodules.

virtual void pretty_print(std::ostream &stream) const override

Pretty prints the `Flatten` module into the given `stream`.

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input)

Applies a flatten transform on the `input`.

Public Members

FlattenOptions options

The options used to configure this module.

## Unflatten

class Unflatten : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<UnflattenImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `UnflattenImpl`.

See the documentation for `UnflattenImpl` class to learn what methods it provides, and examples of how to use `Unflatten` with `torch::nn::UnflattenOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = UnflattenImpl

class UnflattenImpl : public torch::nn::[Cloneable](utilities.html#_CPPv4I0EN5torch2nn9CloneableE)<UnflattenImpl>

A placeholder for unflatten operator See [https://pytorch.org/docs/main/generated/torch.nn.Unflatten.html](https://pytorch.org/docs/main/generated/torch.nn.Unflatten.html) to learn about the exact behavior of this module.

See the documentation for `torch::nn::UnflattenOptions` class to learn what constructor arguments are supported for this module.

Example:

```
Unflatten model(UnflattenOptions(0, {2, 2}));
Unflatten model(UnflattenOptions("B", {{"B1", 2}, {"B2", 2}}));
```

Public Functions

inline UnflattenImpl(int64_t dim, std::vector<int64_t> sizes)

inline UnflattenImpl(std::string dimname, UnflattenOptions::namedshape_t namedshape)

explicit UnflattenImpl(UnflattenOptions options_)

virtual void reset() override

`reset()` must perform initialization of all members with reference semantics, most importantly parameters, buffers and submodules.

virtual void pretty_print(std::ostream &stream) const override

Pretty prints the `Unflatten` module into the given `stream`.

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input)

Applies an unflatten transform on the `input`.

Public Members

UnflattenOptions options

The options used to configure this module.