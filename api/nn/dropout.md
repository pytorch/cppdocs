# Dropout Layers

Dropout randomly zeros elements during training as a regularization technique,
preventing overfitting by forcing the network to learn redundant representations.
During evaluation, dropout is disabled and outputs are scaled appropriately.

- **Dropout**: Standard dropout for fully-connected layers
- **Dropout2d/3d**: Spatial dropout that zeros entire channels (better for CNNs)
- **AlphaDropout**: Maintains self-normalizing property (use with SELU activation)

Note

Remember to call `model->train()` during training and `model->eval()` during
inference to properly enable/disable dropout behavior.

## Dropout

class Dropout : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<DropoutImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `DropoutImpl`.

See the documentation for `DropoutImpl` class to learn what methods it provides, and examples of how to use `Dropout` with `torch::nn::DropoutOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = DropoutImpl

class DropoutImpl : public torch::nn::detail::_DropoutNd<DropoutImpl>

Applies dropout over a 1-D input.

See [https://pytorch.org/docs/main/nn.html#torch.nn.Dropout](https://pytorch.org/docs/main/nn.html#torch.nn.Dropout) to learn about the exact behavior of this module.

See the documentation for `torch::nn::DropoutOptions` class to learn what constructor arguments are supported for this module.

Example:

```
Dropout model(DropoutOptions().p(0.42).inplace(true));
```

Public Functions

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward([Tensor](../aten/tensor.html#_CPPv46Tensorv) input)

virtual void pretty_print(std::ostream &stream) const override

Pretty prints the `Dropout` module into the given `stream`.

**Example:**

```
auto dropout = torch::nn::Dropout(torch::nn::DropoutOptions(0.5));
```

## Dropout2d / Dropout3d

class Dropout2d : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<Dropout2dImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `Dropout2dImpl`.

See the documentation for `Dropout2dImpl` class to learn what methods it provides, and examples of how to use `Dropout2d` with `torch::nn::Dropout2dOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = Dropout2dImpl

class Dropout2dImpl : public torch::nn::detail::_DropoutNd<Dropout2dImpl>

Applies dropout over a 2-D input.

See [https://pytorch.org/docs/main/nn.html#torch.nn.Dropout2d](https://pytorch.org/docs/main/nn.html#torch.nn.Dropout2d) to learn about the exact behavior of this module.

See the documentation for `torch::nn::Dropout2dOptions` class to learn what constructor arguments are supported for this module.

Example:

```
Dropout2d model(Dropout2dOptions().p(0.42).inplace(true));
```

Public Functions

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward([Tensor](../aten/tensor.html#_CPPv46Tensorv) input)

virtual void pretty_print(std::ostream &stream) const override

Pretty prints the `Dropout2d` module into the given `stream`.

class Dropout3d : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<Dropout3dImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `Dropout3dImpl`.

See the documentation for `Dropout3dImpl` class to learn what methods it provides, and examples of how to use `Dropout3d` with `torch::nn::Dropout3dOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = Dropout3dImpl

class Dropout3dImpl : public torch::nn::detail::_DropoutNd<Dropout3dImpl>

Applies dropout over a 3-D input.

See [https://pytorch.org/docs/main/nn.html#torch.nn.Dropout3d](https://pytorch.org/docs/main/nn.html#torch.nn.Dropout3d) to learn about the exact behavior of this module.

See the documentation for `torch::nn::Dropout3dOptions` class to learn what constructor arguments are supported for this module.

Example:

```
Dropout3d model(Dropout3dOptions().p(0.42).inplace(true));
```

Public Functions

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward([Tensor](../aten/tensor.html#_CPPv46Tensorv) input)

virtual void pretty_print(std::ostream &stream) const override

Pretty prints the `Dropout3d` module into the given `stream`.

## AlphaDropout

class AlphaDropout : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<AlphaDropoutImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `AlphaDropoutImpl`.

See the documentation for `AlphaDropoutImpl` class to learn what methods it provides, and examples of how to use `AlphaDropout` with `torch::nn::AlphaDropoutOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = AlphaDropoutImpl

class AlphaDropoutImpl : public torch::nn::detail::_DropoutNd<AlphaDropoutImpl>

Applies Alpha Dropout over the input.

See [https://pytorch.org/docs/main/nn.html#torch.nn.AlphaDropout](https://pytorch.org/docs/main/nn.html#torch.nn.AlphaDropout) to learn about the exact behavior of this module.

See the documentation for `torch::nn::AlphaDropoutOptions` class to learn what constructor arguments are supported for this module.

Example:

```
AlphaDropout model(AlphaDropoutOptions(0.2).inplace(true));
```

Public Functions

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input)

virtual void pretty_print(std::ostream &stream) const override

Pretty prints the `AlphaDropout` module into the given `stream`.

## FeatureAlphaDropout

class FeatureAlphaDropout : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<FeatureAlphaDropoutImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `FeatureAlphaDropoutImpl`.

See the documentation for `FeatureAlphaDropoutImpl` class to learn what methods it provides, and examples of how to use `FeatureAlphaDropout` with `torch::nn::FeatureAlphaDropoutOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = FeatureAlphaDropoutImpl

class FeatureAlphaDropoutImpl : public torch::nn::detail::_DropoutNd<FeatureAlphaDropoutImpl>

See the documentation for `torch::nn::FeatureAlphaDropoutOptions` class to learn what constructor arguments are supported for this module.

Example:

```
FeatureAlphaDropout model(FeatureAlphaDropoutOptions(0.2).inplace(true));
```

Public Functions

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input)

virtual void pretty_print(std::ostream &stream) const override

Pretty prints the `FeatureAlphaDropout` module into the given `stream`.