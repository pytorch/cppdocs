# Activation Functions

Activation functions introduce non-linearity into neural networks, allowing them
to learn complex patterns. Without activations, stacked linear layers would collapse
into a single linear transformation.

**Common choices:**

- **ReLU family** (ReLU, LeakyReLU, PReLU, RReLU): Fast, widely used, good default choice
- **ELU family** (ELU, SELU, CELU): Smoother than ReLU, can produce negative outputs
- **GELU/SiLU/Mish**: Modern activations popular in transformers and advanced architectures
- **Sigmoid/Tanh**: Classic activations, useful for output layers (probabilities, bounded outputs)
- **Softmax**: Converts logits to probability distribution (classification output)

## ReLU

class ReLU : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<ReLUImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `ReLUImpl`.

See the documentation for `ReLUImpl` class to learn what methods it provides, and examples of how to use `ReLU` with `torch::nn::ReLUOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = ReLUImpl

class ReLUImpl : public torch::nn::[Cloneable](utilities.html#_CPPv4I0EN5torch2nn9CloneableE)<ReLUImpl>

Applies the ReLU function element-wise.

See [https://pytorch.org/docs/main/nn.html#torch.nn.ReLU](https://pytorch.org/docs/main/nn.html#torch.nn.ReLU) to learn about the exact behavior of this module.

See the documentation for `torch::nn::ReLUOptions` class to learn what constructor arguments are supported for this module.

Example:

```
ReLU model(ReLUOptions().inplace(true));
```

Public Functions

explicit ReLUImpl(const ReLUOptions &options_ = {})

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward([Tensor](../aten/tensor.html#_CPPv46Tensorv) input)

virtual void reset() override

`reset()` must perform initialization of all members with reference semantics, most importantly parameters, buffers and submodules.

virtual void pretty_print(std::ostream &stream) const override

Pretty prints the `ReLU` module into the given `stream`.

Public Members

ReLUOptions options

The options with which this `[Module](index.html#PyTorchclasstorch_1_1nn_1_1_module)` was constructed.

**Example:**

```
auto relu = torch::nn::ReLU(torch::nn::ReLUOptions().inplace(true));
```

## LeakyReLU

class LeakyReLU : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<LeakyReLUImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `LeakyReLUImpl`.

See the documentation for `LeakyReLUImpl` class to learn what methods it provides, and examples of how to use `LeakyReLU` with `torch::nn::LeakyReLUOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = LeakyReLUImpl

class LeakyReLUImpl : public torch::nn::[Cloneable](utilities.html#_CPPv4I0EN5torch2nn9CloneableE)<LeakyReLUImpl>

Applies the LeakyReLU function element-wise.

See [https://pytorch.org/docs/main/nn.html#torch.nn.LeakyReLU](https://pytorch.org/docs/main/nn.html#torch.nn.LeakyReLU) to learn about the exact behavior of this module.

See the documentation for `torch::nn::LeakyReLUOptions` class to learn what constructor arguments are supported for this module.

Example:

```
LeakyReLU model(LeakyReLUOptions().negative_slope(0.42).inplace(true));
```

Public Functions

explicit LeakyReLUImpl(const LeakyReLUOptions &options_ = {})

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward([Tensor](../aten/tensor.html#_CPPv46Tensorv) input)

virtual void reset() override

`reset()` must perform initialization of all members with reference semantics, most importantly parameters, buffers and submodules.

virtual void pretty_print(std::ostream &stream) const override

Pretty prints the `LeakyReLU` module into the given `stream`.

Public Members

LeakyReLUOptions options

The options with which this `[Module](index.html#PyTorchclasstorch_1_1nn_1_1_module)` was constructed.

## PReLU

class PReLU : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<PReLUImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `PReLUImpl`.

See the documentation for `PReLUImpl` class to learn what methods it provides, and examples of how to use `PReLU` with `torch::nn::PReLUOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = PReLUImpl

class PReLUImpl : public torch::nn::[Cloneable](utilities.html#_CPPv4I0EN5torch2nn9CloneableE)<PReLUImpl>

Applies the PReLU function element-wise.

See [https://pytorch.org/docs/main/nn.html#torch.nn.PReLU](https://pytorch.org/docs/main/nn.html#torch.nn.PReLU) to learn about the exact behavior of this module.

See the documentation for `torch::nn::PReLUOptions` class to learn what constructor arguments are supported for this module.

Example:

```
PReLU model(PReLUOptions().num_parameters(42));
```

Public Functions

explicit PReLUImpl(const PReLUOptions &options_ = {})

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input)

virtual void reset() override

`reset()` must perform initialization of all members with reference semantics, most importantly parameters, buffers and submodules.

virtual void pretty_print(std::ostream &stream) const override

Pretty prints the `PReLU` module into the given `stream`.

Public Members

PReLUOptions options

The options with which this `[Module](index.html#PyTorchclasstorch_1_1nn_1_1_module)` was constructed.

[Tensor](../aten/tensor.html#_CPPv46Tensorv) weight

The learned weight.

## RReLU

class RReLU : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<RReLUImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `RReLUImpl`.

See the documentation for `RReLUImpl` class to learn what methods it provides, and examples of how to use `RReLU` with `torch::nn::RReLUOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = RReLUImpl

class RReLUImpl : public torch::nn::[Cloneable](utilities.html#_CPPv4I0EN5torch2nn9CloneableE)<RReLUImpl>

Applies the RReLU function element-wise.

See [https://pytorch.org/docs/main/nn.html#torch.nn.RReLU](https://pytorch.org/docs/main/nn.html#torch.nn.RReLU) to learn about the exact behavior of this module.

See the documentation for `torch::nn::RReLUOptions` class to learn what constructor arguments are supported for this module.

Example:

```
RReLU model(RReLUOptions().lower(0.24).upper(0.42).inplace(true));
```

Public Functions

explicit RReLUImpl(const RReLUOptions &options_ = {})

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward([Tensor](../aten/tensor.html#_CPPv46Tensorv) input)

virtual void reset() override

`reset()` must perform initialization of all members with reference semantics, most importantly parameters, buffers and submodules.

virtual void pretty_print(std::ostream &stream) const override

Pretty prints the `RReLU` module into the given `stream`.

Public Members

RReLUOptions options

The options with which this `[Module](index.html#PyTorchclasstorch_1_1nn_1_1_module)` was constructed.

## ReLU6

Like ReLU but caps the output at 6: `min(max(0, x), 6)`. Commonly used in
mobile architectures (MobileNet).

class ReLU6 : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<ReLU6Impl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `ReLU6Impl`.

See the documentation for `ReLU6Impl` class to learn what methods it provides, and examples of how to use `ReLU6` with `torch::nn::ReLU6Options`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = ReLU6Impl

class ReLU6Impl : public torch::nn::[Cloneable](utilities.html#_CPPv4I0EN5torch2nn9CloneableE)<ReLU6Impl>

Applies the ReLU6 function element-wise.

See [https://pytorch.org/docs/main/nn.html#torch.nn.ReLU6](https://pytorch.org/docs/main/nn.html#torch.nn.ReLU6) to learn about the exact behavior of this module.

See the documentation for `torch::nn::ReLU6Options` class to learn what constructor arguments are supported for this module.

Example:

```
ReLU6 model(ReLU6Options().inplace(true));
```

Public Functions

explicit ReLU6Impl(const ReLU6Options &options_ = {})

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward([Tensor](../aten/tensor.html#_CPPv46Tensorv) input)

virtual void reset() override

`reset()` must perform initialization of all members with reference semantics, most importantly parameters, buffers and submodules.

virtual void pretty_print(std::ostream &stream) const override

Pretty prints the `ReLU6` module into the given `stream`.

Public Members

ReLU6Options options

The options with which this `[Module](index.html#PyTorchclasstorch_1_1nn_1_1_module)` was constructed.

## GLU

Gated Linear Unit. Splits the input tensor in half along a dimension,
then applies `a * sigmoid(b)`.

class GLU : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<GLUImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `GLUImpl`.

See the documentation for `GLUImpl` class to learn what methods it provides, and examples of how to use `GLU` with `torch::nn::GLUOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = GLUImpl

class GLUImpl : public torch::nn::[Cloneable](utilities.html#_CPPv4I0EN5torch2nn9CloneableE)<GLUImpl>

Applies glu over a given input.

See [https://pytorch.org/docs/main/nn.html#torch.nn.GLU](https://pytorch.org/docs/main/nn.html#torch.nn.GLU) to learn about the exact behavior of this module.

See the documentation for `torch::nn::GLUOptions` class to learn what constructor arguments are supported for this module.

Example:

```
GLU model(GLUOptions(1));
```

Public Functions

explicit GLUImpl(const GLUOptions &options_ = {})

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input)

virtual void reset() override

`reset()` must perform initialization of all members with reference semantics, most importantly parameters, buffers and submodules.

virtual void pretty_print(std::ostream &stream) const override

Pretty prints the `GLU` module into the given `stream`.

Public Members

GLUOptions options

The options with which this `[Module](index.html#PyTorchclasstorch_1_1nn_1_1_module)` was constructed.

## LogSigmoid

Applies element-wise `log(sigmoid(x))`. Numerically more stable than
computing `log` and `sigmoid` separately.

class LogSigmoid : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<LogSigmoidImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `LogSigmoidImpl`.

See the documentation for `LogSigmoidImpl` class to learn what methods it provides, or the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = LogSigmoidImpl

class LogSigmoidImpl : public torch::nn::[Cloneable](utilities.html#_CPPv4I0EN5torch2nn9CloneableE)<LogSigmoidImpl>

Applies the LogSigmoid function element-wise.

See [https://pytorch.org/docs/main/nn.html#torch.nn.LogSigmoid](https://pytorch.org/docs/main/nn.html#torch.nn.LogSigmoid) to learn about the exact behavior of this module.

Public Functions

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input)

virtual void reset() override

`reset()` must perform initialization of all members with reference semantics, most importantly parameters, buffers and submodules.

virtual void pretty_print(std::ostream &stream) const override

Pretty prints the `LogSigmoid` module into the given `stream`.

## ELU

class ELU : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<ELUImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `ELUImpl`.

See the documentation for `ELUImpl` class to learn what methods it provides, and examples of how to use `ELU` with `torch::nn::ELUOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = ELUImpl

class ELUImpl : public torch::nn::[Cloneable](utilities.html#_CPPv4I0EN5torch2nn9CloneableE)<ELUImpl>

Applies elu over a given input.

See [https://pytorch.org/docs/main/nn.html#torch.nn.ELU](https://pytorch.org/docs/main/nn.html#torch.nn.ELU) to learn about the exact behavior of this module.

See the documentation for `torch::nn::ELUOptions` class to learn what constructor arguments are supported for this module.

Example:

```
ELU model(ELUOptions().alpha(42.42).inplace(true));
```

Public Functions

explicit ELUImpl(const ELUOptions &options_ = {})

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward([Tensor](../aten/tensor.html#_CPPv46Tensorv) input)

virtual void reset() override

`reset()` must perform initialization of all members with reference semantics, most importantly parameters, buffers and submodules.

virtual void pretty_print(std::ostream &stream) const override

Pretty prints the `ELU` module into the given `stream`.

Public Members

ELUOptions options

The options with which this `[Module](index.html#PyTorchclasstorch_1_1nn_1_1_module)` was constructed.

## SELU

class SELU : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<SELUImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `SELUImpl`.

See the documentation for `SELUImpl` class to learn what methods it provides, and examples of how to use `SELU` with `torch::nn::SELUOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = SELUImpl

class SELUImpl : public torch::nn::[Cloneable](utilities.html#_CPPv4I0EN5torch2nn9CloneableE)<SELUImpl>

Applies the selu function element-wise.

See [https://pytorch.org/docs/main/nn.html#torch.nn.SELU](https://pytorch.org/docs/main/nn.html#torch.nn.SELU) to learn about the exact behavior of this module.

See the documentation for `torch::nn::SELUOptions` class to learn what constructor arguments are supported for this module.

Example:

```
SELU model(SELUOptions().inplace(true));
```

Public Functions

explicit SELUImpl(const SELUOptions &options_ = {})

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward([Tensor](../aten/tensor.html#_CPPv46Tensorv) input)

virtual void reset() override

`reset()` must perform initialization of all members with reference semantics, most importantly parameters, buffers and submodules.

virtual void pretty_print(std::ostream &stream) const override

Pretty prints the `SELU` module into the given `stream`.

Public Members

SELUOptions options

The options with which this `[Module](index.html#PyTorchclasstorch_1_1nn_1_1_module)` was constructed.

## CELU

class CELU : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<CELUImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `CELUImpl`.

See the documentation for `CELUImpl` class to learn what methods it provides, and examples of how to use `CELU` with `torch::nn::CELUOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = CELUImpl

class CELUImpl : public torch::nn::[Cloneable](utilities.html#_CPPv4I0EN5torch2nn9CloneableE)<CELUImpl>

Applies celu over a given input.

See [https://pytorch.org/docs/main/nn.html#torch.nn.CELU](https://pytorch.org/docs/main/nn.html#torch.nn.CELU) to learn about the exact behavior of this module.

See the documentation for `torch::nn::CELUOptions` class to learn what constructor arguments are supported for this module.

Example:

```
CELU model(CELUOptions().alpha(42.42).inplace(true));
```

Public Functions

explicit CELUImpl(const CELUOptions &options_ = {})

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward([Tensor](../aten/tensor.html#_CPPv46Tensorv) input)

virtual void reset() override

`reset()` must perform initialization of all members with reference semantics, most importantly parameters, buffers and submodules.

virtual void pretty_print(std::ostream &stream) const override

Pretty prints the `CELU` module into the given `stream`.

Public Members

CELUOptions options

The options with which this `[Module](index.html#PyTorchclasstorch_1_1nn_1_1_module)` was constructed.

## GELU

class GELU : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<GELUImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `GELUImpl`.

See the documentation for `GELUImpl` class to learn what methods it provides, or the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = GELUImpl

class GELUImpl : public torch::nn::[Cloneable](utilities.html#_CPPv4I0EN5torch2nn9CloneableE)<GELUImpl>

Applies gelu over a given input.

See [https://pytorch.org/docs/main/nn.html#torch.nn.GELU](https://pytorch.org/docs/main/nn.html#torch.nn.GELU) to learn about the exact behavior of this module.

Public Functions

explicit GELUImpl(GELUOptions options_ = {})

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input)

virtual void reset() override

`reset()` must perform initialization of all members with reference semantics, most importantly parameters, buffers and submodules.

virtual void pretty_print(std::ostream &stream) const override

Pretty prints the `GELU` module into the given `stream`.

Public Members

GELUOptions options

The options with which this `[Module](index.html#PyTorchclasstorch_1_1nn_1_1_module)` was constructed.

## SiLU (Swish)

class SiLU : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<SiLUImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `SiLUImpl`.

See the documentation for `SiLUImpl` class to learn what methods it provides, or the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = SiLUImpl

class SiLUImpl : public torch::nn::[Cloneable](utilities.html#_CPPv4I0EN5torch2nn9CloneableE)<SiLUImpl>

Applies silu over a given input.

See [https://pytorch.org/docs/main/nn.html#torch.nn.SiLU](https://pytorch.org/docs/main/nn.html#torch.nn.SiLU) to learn about the exact behavior of this module.

Public Functions

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input)

virtual void reset() override

`reset()` must perform initialization of all members with reference semantics, most importantly parameters, buffers and submodules.

virtual void pretty_print(std::ostream &stream) const override

Pretty prints the `SiLU` module into the given `stream`.

## Mish

class Mish : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<MishImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `MishImpl`.

See the documentation for `MishImpl` class to learn what methods it provides, or the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = MishImpl

class MishImpl : public torch::nn::[Cloneable](utilities.html#_CPPv4I0EN5torch2nn9CloneableE)<MishImpl>

Applies mish over a given input.

See [https://pytorch.org/docs/main/nn.html#torch.nn.Mish](https://pytorch.org/docs/main/nn.html#torch.nn.Mish) to learn about the exact behavior of this module.

Public Functions

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input)

virtual void reset() override

`reset()` must perform initialization of all members with reference semantics, most importantly parameters, buffers and submodules.

virtual void pretty_print(std::ostream &stream) const override

Pretty prints the `Mish` module into the given `stream`.

## Sigmoid

class Sigmoid : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<SigmoidImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `SigmoidImpl`.

See the documentation for `SigmoidImpl` class to learn what methods it provides, or the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = SigmoidImpl

class SigmoidImpl : public torch::nn::[Cloneable](utilities.html#_CPPv4I0EN5torch2nn9CloneableE)<SigmoidImpl>

Applies sigmoid over a given input.

See [https://pytorch.org/docs/main/nn.html#torch.nn.Sigmoid](https://pytorch.org/docs/main/nn.html#torch.nn.Sigmoid) to learn about the exact behavior of this module.

Public Functions

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input)

virtual void reset() override

`reset()` must perform initialization of all members with reference semantics, most importantly parameters, buffers and submodules.

virtual void pretty_print(std::ostream &stream) const override

Pretty prints the `Sigmoid` module into the given `stream`.

## Tanh

class Tanh : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<TanhImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `TanhImpl`.

See the documentation for `TanhImpl` class to learn what methods it provides, or the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = TanhImpl

class TanhImpl : public torch::nn::[Cloneable](utilities.html#_CPPv4I0EN5torch2nn9CloneableE)<TanhImpl>

Applies Tanh over a given input.

See [https://pytorch.org/docs/main/nn.html#torch.nn.Tanh](https://pytorch.org/docs/main/nn.html#torch.nn.Tanh) to learn about the exact behavior of this module.

Public Functions

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input)

virtual void reset() override

`reset()` must perform initialization of all members with reference semantics, most importantly parameters, buffers and submodules.

virtual void pretty_print(std::ostream &stream) const override

Pretty prints the `Tanh` module into the given `stream`.

## Softmax

class Softmax : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<SoftmaxImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `SoftmaxImpl`.

See the documentation for `SoftmaxImpl` class to learn what methods it provides, and examples of how to use `Softmax` with `torch::nn::SoftmaxOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = SoftmaxImpl

class SoftmaxImpl : public torch::nn::[Cloneable](utilities.html#_CPPv4I0EN5torch2nn9CloneableE)<SoftmaxImpl>

Applies the Softmax function.

See [https://pytorch.org/docs/main/nn.html#torch.nn.Softmax](https://pytorch.org/docs/main/nn.html#torch.nn.Softmax) to learn about the exact behavior of this module.

See the documentation for `torch::nn::SoftmaxOptions` class to learn what constructor arguments are supported for this module.

Example:

```
Softmax model(SoftmaxOptions(1));
```

Public Functions

inline explicit SoftmaxImpl(int64_t dim)

explicit SoftmaxImpl(const SoftmaxOptions &options_)

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input)

virtual void reset() override

`reset()` must perform initialization of all members with reference semantics, most importantly parameters, buffers and submodules.

virtual void pretty_print(std::ostream &stream) const override

Pretty prints the `Softmax` module into the given `stream`.

Public Members

SoftmaxOptions options

**Example:**

```
auto softmax = torch::nn::Softmax(torch::nn::SoftmaxOptions(/*dim=*/1));
```

## Softmax2d

Applies `Softmax` over features to each spatial location in a 4D input
tensor of shape `(N, C, H, W)`.

class Softmax2d : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<Softmax2dImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `Softmax2dImpl`.

See the documentation for `Softmax2dImpl` class to learn what methods it provides, or the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = Softmax2dImpl

class Softmax2dImpl : public torch::nn::[Cloneable](utilities.html#_CPPv4I0EN5torch2nn9CloneableE)<Softmax2dImpl>

Applies the Softmax2d function element-wise.

See [https://pytorch.org/docs/main/nn.html#torch.nn.Softmax2d](https://pytorch.org/docs/main/nn.html#torch.nn.Softmax2d) to learn about the exact behavior of this module.

Public Functions

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input)

virtual void reset() override

`reset()` must perform initialization of all members with reference semantics, most importantly parameters, buffers and submodules.

virtual void pretty_print(std::ostream &stream) const override

Pretty prints the `Softmax2d` module into the given `stream`.

## LogSoftmax

class LogSoftmax : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<LogSoftmaxImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `LogSoftmaxImpl`.

See the documentation for `LogSoftmaxImpl` class to learn what methods it provides, and examples of how to use `LogSoftmax` with `torch::nn::LogSoftmaxOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = LogSoftmaxImpl

class LogSoftmaxImpl : public torch::nn::[Cloneable](utilities.html#_CPPv4I0EN5torch2nn9CloneableE)<LogSoftmaxImpl>

Applies the LogSoftmax function element-wise.

See [https://pytorch.org/docs/main/nn.html#torch.nn.LogSoftmax](https://pytorch.org/docs/main/nn.html#torch.nn.LogSoftmax) to learn about the exact behavior of this module.

See the documentation for `torch::nn::LogSoftmaxOptions` class to learn what constructor arguments are supported for this module.

Example:

```
LogSoftmax model(LogSoftmaxOptions(1));
```

Public Functions

inline explicit LogSoftmaxImpl(int64_t dim)

explicit LogSoftmaxImpl(const LogSoftmaxOptions &options_)

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input)

virtual void reset() override

`reset()` must perform initialization of all members with reference semantics, most importantly parameters, buffers and submodules.

virtual void pretty_print(std::ostream &stream) const override

Pretty prints the `LogSoftmax` module into the given `stream`.

Public Members

LogSoftmaxOptions options

## Softmin

class Softmin : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<SoftminImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `SoftminImpl`.

See the documentation for `SoftminImpl` class to learn what methods it provides, and examples of how to use `Softmin` with `torch::nn::SoftminOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = SoftminImpl

class SoftminImpl : public torch::nn::[Cloneable](utilities.html#_CPPv4I0EN5torch2nn9CloneableE)<SoftminImpl>

Applies the Softmin function element-wise.

See [https://pytorch.org/docs/main/nn.html#torch.nn.Softmin](https://pytorch.org/docs/main/nn.html#torch.nn.Softmin) to learn about the exact behavior of this module.

See the documentation for `torch::nn::SoftminOptions` class to learn what constructor arguments are supported for this module.

Example:

```
Softmin model(SoftminOptions(1));
```

Public Functions

inline explicit SoftminImpl(int64_t dim)

explicit SoftminImpl(const SoftminOptions &options_)

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input)

virtual void reset() override

`reset()` must perform initialization of all members with reference semantics, most importantly parameters, buffers and submodules.

virtual void pretty_print(std::ostream &stream) const override

Pretty prints the `Softmin` module into the given `stream`.

Public Members

SoftminOptions options

## Softplus

class Softplus : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<SoftplusImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `SoftplusImpl`.

See the documentation for `SoftplusImpl` class to learn what methods it provides, and examples of how to use `Softplus` with `torch::nn::SoftplusOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = SoftplusImpl

class SoftplusImpl : public torch::nn::[Cloneable](utilities.html#_CPPv4I0EN5torch2nn9CloneableE)<SoftplusImpl>

Applies softplus over a given input.

See [https://pytorch.org/docs/main/nn.html#torch.nn.Softplus](https://pytorch.org/docs/main/nn.html#torch.nn.Softplus) to learn about the exact behavior of this module.

See the documentation for `torch::nn::SoftplusOptions` class to learn what constructor arguments are supported for this module.

Example:

```
Softplus model(SoftplusOptions().beta(0.24).threshold(42.42));
```

Public Functions

explicit SoftplusImpl(const SoftplusOptions &options_ = {})

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input)

virtual void reset() override

`reset()` must perform initialization of all members with reference semantics, most importantly parameters, buffers and submodules.

virtual void pretty_print(std::ostream &stream) const override

Pretty prints the `Softplus` module into the given `stream`.

Public Members

SoftplusOptions options

The options with which this `[Module](index.html#PyTorchclasstorch_1_1nn_1_1_module)` was constructed.

## Softshrink

class Softshrink : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<SoftshrinkImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `SoftshrinkImpl`.

See the documentation for `SoftshrinkImpl` class to learn what methods it provides, and examples of how to use `Softshrink` with `torch::nn::SoftshrinkOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = SoftshrinkImpl

class SoftshrinkImpl : public torch::nn::[Cloneable](utilities.html#_CPPv4I0EN5torch2nn9CloneableE)<SoftshrinkImpl>

Applies the soft shrinkage function element-wise.

See [https://pytorch.org/docs/main/nn.html#torch.nn.Softshrink](https://pytorch.org/docs/main/nn.html#torch.nn.Softshrink) to learn about the exact behavior of this module.

See the documentation for `torch::nn::SoftshrinkOptions` class to learn what constructor arguments are supported for this module.

Example:

```
Softshrink model(SoftshrinkOptions(42.42));
```

Public Functions

explicit SoftshrinkImpl(const SoftshrinkOptions &options_ = {})

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input)

virtual void reset() override

`reset()` must perform initialization of all members with reference semantics, most importantly parameters, buffers and submodules.

virtual void pretty_print(std::ostream &stream) const override

Pretty prints the `Softshrink` module into the given `stream`.

Public Members

SoftshrinkOptions options

The options with which this `[Module](index.html#PyTorchclasstorch_1_1nn_1_1_module)` was constructed.

## Softsign

class Softsign : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<SoftsignImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `SoftsignImpl`.

See the documentation for `SoftsignImpl` class to learn what methods it provides, or the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = SoftsignImpl

class SoftsignImpl : public torch::nn::[Cloneable](utilities.html#_CPPv4I0EN5torch2nn9CloneableE)<SoftsignImpl>

Applies Softsign over a given input.

See [https://pytorch.org/docs/main/nn.html#torch.nn.Softsign](https://pytorch.org/docs/main/nn.html#torch.nn.Softsign) to learn about the exact behavior of this module.

Public Functions

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input)

virtual void reset() override

`reset()` must perform initialization of all members with reference semantics, most importantly parameters, buffers and submodules.

virtual void pretty_print(std::ostream &stream) const override

Pretty prints the `Softsign` module into the given `stream`.

## Hardshrink

class Hardshrink : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<HardshrinkImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `HardshrinkImpl`.

See the documentation for `HardshrinkImpl` class to learn what methods it provides, and examples of how to use `Hardshrink` with `torch::nn::HardshrinkOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = HardshrinkImpl

class HardshrinkImpl : public torch::nn::[Cloneable](utilities.html#_CPPv4I0EN5torch2nn9CloneableE)<HardshrinkImpl>

Applies the hard shrinkage function element-wise.

See [https://pytorch.org/docs/main/nn.html#torch.nn.Hardshrink](https://pytorch.org/docs/main/nn.html#torch.nn.Hardshrink) to learn about the exact behavior of this module.

See the documentation for `torch::nn::HardshrinkOptions` class to learn what constructor arguments are supported for this module.

Example:

```
Hardshrink model(HardshrinkOptions().lambda(42.42));
```

Public Functions

explicit HardshrinkImpl(const HardshrinkOptions &options_ = {})

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input)

virtual void reset() override

`reset()` must perform initialization of all members with reference semantics, most importantly parameters, buffers and submodules.

virtual void pretty_print(std::ostream &stream) const override

Pretty prints the `Hardshrink` module into the given `stream`.

Public Members

HardshrinkOptions options

The options with which this `[Module](index.html#PyTorchclasstorch_1_1nn_1_1_module)` was constructed.

## Hardtanh

class Hardtanh : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<HardtanhImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `HardtanhImpl`.

See the documentation for `HardtanhImpl` class to learn what methods it provides, and examples of how to use `Hardtanh` with `torch::nn::HardtanhOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = HardtanhImpl

class HardtanhImpl : public torch::nn::[Cloneable](utilities.html#_CPPv4I0EN5torch2nn9CloneableE)<HardtanhImpl>

Applies the HardTanh function element-wise.

See [https://pytorch.org/docs/main/nn.html#torch.nn.Hardtanh](https://pytorch.org/docs/main/nn.html#torch.nn.Hardtanh) to learn about the exact behavior of this module.

See the documentation for `torch::nn::HardtanhOptions` class to learn what constructor arguments are supported for this module.

Example:

```
Hardtanh
model(HardtanhOptions().min_val(-42.42).max_val(0.42).inplace(true));
```

Public Functions

explicit HardtanhImpl(const HardtanhOptions &options_ = {})

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward([Tensor](../aten/tensor.html#_CPPv46Tensorv) input)

virtual void reset() override

`reset()` must perform initialization of all members with reference semantics, most importantly parameters, buffers and submodules.

virtual void pretty_print(std::ostream &stream) const override

Pretty prints the `Hardtanh` module into the given `stream`.

Public Members

HardtanhOptions options

The options with which this `[Module](index.html#PyTorchclasstorch_1_1nn_1_1_module)` was constructed.

## Tanhshrink

class Tanhshrink : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<TanhshrinkImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `TanhshrinkImpl`.

See the documentation for `TanhshrinkImpl` class to learn what methods it provides, or the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = TanhshrinkImpl

class TanhshrinkImpl : public torch::nn::[Cloneable](utilities.html#_CPPv4I0EN5torch2nn9CloneableE)<TanhshrinkImpl>

Applies Tanhshrink over a given input.

See [https://pytorch.org/docs/main/nn.html#torch.nn.Tanhshrink](https://pytorch.org/docs/main/nn.html#torch.nn.Tanhshrink) to learn about the exact behavior of this module.

Public Functions

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input)

virtual void reset() override

`reset()` must perform initialization of all members with reference semantics, most importantly parameters, buffers and submodules.

virtual void pretty_print(std::ostream &stream) const override

Pretty prints the `Tanhshrink` module into the given `stream`.

## Threshold

class Threshold : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<ThresholdImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `ThresholdImpl`.

See the documentation for `ThresholdImpl` class to learn what methods it provides, and examples of how to use `Threshold` with `torch::nn::ThresholdOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = ThresholdImpl

class ThresholdImpl : public torch::nn::[Cloneable](utilities.html#_CPPv4I0EN5torch2nn9CloneableE)<ThresholdImpl>

Applies the Threshold function element-wise.

See [https://pytorch.org/docs/main/nn.html#torch.nn.Threshold](https://pytorch.org/docs/main/nn.html#torch.nn.Threshold) to learn about the exact behavior of this module.

See the documentation for `torch::nn::ThresholdOptions` class to learn what constructor arguments are supported for this module.

Example:

```
Threshold model(ThresholdOptions(42.42, 24.24).inplace(true));
```

Public Functions

inline ThresholdImpl(double threshold, double value)

explicit ThresholdImpl(const ThresholdOptions &options_)

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward([Tensor](../aten/tensor.html#_CPPv46Tensorv) input)

virtual void reset() override

`reset()` must perform initialization of all members with reference semantics, most importantly parameters, buffers and submodules.

virtual void pretty_print(std::ostream &stream) const override

Pretty prints the `Threshold` module into the given `stream`.

Public Members

ThresholdOptions options

The options with which this `[Module](index.html#PyTorchclasstorch_1_1nn_1_1_module)` was constructed.