# Loss Functions

Loss functions measure how well the model's predictions match the targets.
The choice of loss function depends on your task type and data characteristics.

**Regression losses:**

- **L1Loss/MSELoss**: Basic regression losses (MAE vs MSE)
- **SmoothL1Loss/HuberLoss**: Robust to outliers

**Classification losses:**

- **CrossEntropyLoss**: Multi-class classification (combines LogSoftmax + NLLLoss)
- **NLLLoss**: Negative log likelihood (use with LogSoftmax output)
- **BCELoss/BCEWithLogitsLoss**: Binary classification

**Specialized losses:**

- **CTCLoss**: Sequence-to-sequence without alignment (speech recognition)
- **TripletMarginLoss**: Metric learning (similarity/embedding tasks)
- **CosineEmbeddingLoss**: Similarity learning with cosine distance

## L1Loss

class L1Loss : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<L1LossImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `L1LossImpl`.

See the documentation for `L1LossImpl` class to learn what methods it provides, and examples of how to use `L1Loss` with `torch::nn::L1LossOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = L1LossImpl

struct L1LossImpl : public torch::nn::[Cloneable](utilities.html#_CPPv4I0EN5torch2nn9CloneableE)<L1LossImpl>

Creates a criterion that measures the mean absolute error (MAE) between each element in the input : math :`x` and target : `y`.

See [https://pytorch.org/docs/main/nn.html#torch.nn.L1Loss](https://pytorch.org/docs/main/nn.html#torch.nn.L1Loss) to learn about the exact behavior of this module.

See the documentation for `torch::nn::L1LossOptions` class to learn what constructor arguments are supported for this module.

Example:

```
L1Loss model(L1LossOptions(torch::kNone));
```

Public Functions

explicit L1LossImpl(L1LossOptions options_ = {})

virtual void reset() override

`reset()` must perform initialization of all members with reference semantics, most importantly parameters, buffers and submodules.

virtual void pretty_print(std::ostream &stream) const override

Pretty prints the `L1Loss` module into the given `stream`.

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &target)

Public Members

L1LossOptions options

The options with which this `[Module](index.html#PyTorchclasstorch_1_1nn_1_1_module)` was constructed.

## MSELoss

class MSELoss : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<MSELossImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `MSELossImpl`.

See the documentation for `MSELossImpl` class to learn what methods it provides, and examples of how to use `MSELoss` with `torch::nn::MSELossOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = MSELossImpl

struct MSELossImpl : public torch::nn::[Cloneable](utilities.html#_CPPv4I0EN5torch2nn9CloneableE)<MSELossImpl>

Creates a criterion that measures the mean squared error (squared L2 norm) between each element in the input :math:`x` and target :math:`y`.

See [https://pytorch.org/docs/main/nn.html#torch.nn.MSELoss](https://pytorch.org/docs/main/nn.html#torch.nn.MSELoss) to learn about the exact behavior of this module.

See the documentation for `torch::nn::MSELossOptions` class to learn what constructor arguments are supported for this module.

Example:

```
MSELoss model(MSELossOptions(torch::kNone));
```

Public Functions

explicit MSELossImpl(MSELossOptions options_ = {})

virtual void reset() override

`reset()` must perform initialization of all members with reference semantics, most importantly parameters, buffers and submodules.

virtual void pretty_print(std::ostream &stream) const override

Pretty prints the `MSELoss` module into the given `stream`.

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &target)

Public Members

MSELossOptions options

The options with which this `[Module](index.html#PyTorchclasstorch_1_1nn_1_1_module)` was constructed.

**Example:**

```
auto loss_fn = torch::nn::MSELoss();
auto loss = loss_fn->forward(predictions, targets);
```

## CrossEntropyLoss

class CrossEntropyLoss : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<CrossEntropyLossImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `CrossEntropyLossImpl`.

See the documentation for `CrossEntropyLossImpl` class to learn what methods it provides, and examples of how to use `CrossEntropyLoss` with `torch::nn::CrossEntropyLossOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = CrossEntropyLossImpl

struct CrossEntropyLossImpl : public torch::nn::[Cloneable](utilities.html#_CPPv4I0EN5torch2nn9CloneableE)<CrossEntropyLossImpl>

Creates a criterion that computes cross entropy loss between input and target.

See [https://pytorch.org/docs/main/nn.html#torch.nn.CrossEntropyLoss](https://pytorch.org/docs/main/nn.html#torch.nn.CrossEntropyLoss) to learn about the exact behavior of this module.

See the documentation for `torch::nn::CrossEntropyLossOptions` class to learn what constructor arguments are supported for this module.

Example:

```
CrossEntropyLoss
model(CrossEntropyLossOptions().ignore_index(-100).reduction(torch::kMean));
```

Public Functions

explicit CrossEntropyLossImpl(CrossEntropyLossOptions options_ = {})

virtual void reset() override

`reset()` must perform initialization of all members with reference semantics, most importantly parameters, buffers and submodules.

virtual void pretty_print(std::ostream &stream) const override

Pretty prints the `CrossEntropyLoss` module into the given `stream`.

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &target)

Public Members

CrossEntropyLossOptions options

The options with which this `[Module](index.html#PyTorchclasstorch_1_1nn_1_1_module)` was constructed.

[Tensor](../aten/tensor.html#_CPPv46Tensorv) weight

A manual rescaling weight given to each class.

**Example:**

```
auto loss_fn = torch::nn::CrossEntropyLoss();
auto logits = torch::randn({32, 10}); // [batch, num_classes]
auto targets = torch::randint(0, 10, {32}); // [batch]
auto loss = loss_fn->forward(logits, targets);
```

## NLLLoss

class NLLLoss : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<NLLLossImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `NLLLossImpl`.

See the documentation for `NLLLossImpl` class to learn what methods it provides, and examples of how to use `NLLLoss` with `torch::nn::NLLLossOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = NLLLossImpl

struct NLLLossImpl : public torch::nn::[Cloneable](utilities.html#_CPPv4I0EN5torch2nn9CloneableE)<NLLLossImpl>

The negative log likelihood loss.

It is useful to train a classification problem with `C` classes. See [https://pytorch.org/docs/main/nn.html#torch.nn.NLLLoss](https://pytorch.org/docs/main/nn.html#torch.nn.NLLLoss) to learn about the exact behavior of this module.

See the documentation for `torch::nn::NLLLossOptions` class to learn what constructor arguments are supported for this module.

Example:

```
NLLLoss model(NLLLossOptions().ignore_index(-100).reduction(torch::kMean));
```

Public Functions

explicit NLLLossImpl(NLLLossOptions options_ = {})

virtual void pretty_print(std::ostream &stream) const override

Pretty prints the `NLLLoss` module into the given `stream`.

virtual void reset() override

`reset()` must perform initialization of all members with reference semantics, most importantly parameters, buffers and submodules.

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &target)

Public Members

NLLLossOptions options

The options with which this `[Module](index.html#PyTorchclasstorch_1_1nn_1_1_module)` was constructed.

[Tensor](../aten/tensor.html#_CPPv46Tensorv) weight

A manual rescaling weight given to each class.

## BCELoss

class BCELoss : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<BCELossImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `BCELossImpl`.

See the documentation for `BCELossImpl` class to learn what methods it provides, and examples of how to use `BCELoss` with `torch::nn::BCELossOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = BCELossImpl

struct BCELossImpl : public torch::nn::[Cloneable](utilities.html#_CPPv4I0EN5torch2nn9CloneableE)<BCELossImpl>

Creates a criterion that measures the Binary Cross Entropy between the target and the output.

See [https://pytorch.org/docs/main/nn.html#torch.nn.BCELoss](https://pytorch.org/docs/main/nn.html#torch.nn.BCELoss) to learn about the exact behavior of this module.

See the documentation for `torch::nn::BCELossOptions` class to learn what constructor arguments are supported for this module.

Example:

```
BCELoss model(BCELossOptions().reduction(torch::kNone).weight(weight));
```

Public Functions

explicit BCELossImpl(BCELossOptions options_ = {})

virtual void reset() override

`reset()` must perform initialization of all members with reference semantics, most importantly parameters, buffers and submodules.

virtual void pretty_print(std::ostream &stream) const override

Pretty prints the `BCELoss` module into the given `stream`.

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &target)

Public Members

BCELossOptions options

The options with which this `[Module](index.html#PyTorchclasstorch_1_1nn_1_1_module)` was constructed.

## BCEWithLogitsLoss

class BCEWithLogitsLoss : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<BCEWithLogitsLossImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `BCEWithLogitsLossImpl`.

See the documentation for `BCEWithLogitsLossImpl` class to learn what methods it provides, and examples of how to use `BCEWithLogitsLoss` with `torch::nn::BCEWithLogitsLossOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = BCEWithLogitsLossImpl

struct BCEWithLogitsLossImpl : public torch::nn::[Cloneable](utilities.html#_CPPv4I0EN5torch2nn9CloneableE)<BCEWithLogitsLossImpl>

This loss combines a `[Sigmoid](activation.html#PyTorchclasstorch_1_1nn_1_1_sigmoid)` layer and the `BCELoss` in one single class.

This version is more numerically stable than using a plain `[Sigmoid](activation.html#PyTorchclasstorch_1_1nn_1_1_sigmoid)` followed by a `BCELoss` as, by combining the operations into one layer, we take advantage of the log-sum-exp trick for numerical stability. See [https://pytorch.org/docs/main/nn.html#torch.nn.BCEWithLogitsLoss](https://pytorch.org/docs/main/nn.html#torch.nn.BCEWithLogitsLoss) to learn about the exact behavior of this module.

See the documentation for `torch::nn::BCEWithLogitsLossOptions` class to learn what constructor arguments are supported for this module.

Example:

```
BCEWithLogitsLoss
model(BCEWithLogitsLossOptions().reduction(torch::kNone).weight(weight));
```

Public Functions

explicit BCEWithLogitsLossImpl(BCEWithLogitsLossOptions options_ = {})

virtual void reset() override

`reset()` must perform initialization of all members with reference semantics, most importantly parameters, buffers and submodules.

virtual void pretty_print(std::ostream &stream) const override

Pretty prints the `BCEWithLogitsLoss` module into the given `stream`.

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &target)

Public Members

BCEWithLogitsLossOptions options

The options with which this `[Module](index.html#PyTorchclasstorch_1_1nn_1_1_module)` was constructed.

[Tensor](../aten/tensor.html#_CPPv46Tensorv) weight

A manual rescaling weight given to the loss of each batch element.

[Tensor](../aten/tensor.html#_CPPv46Tensorv) pos_weight

A weight of positive examples.

## HuberLoss

class HuberLoss : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<HuberLossImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `HuberLossImpl`.

See the documentation for `HuberLossImpl` class to learn what methods it provides, and examples of how to use `HuberLoss` with `torch::nn::HuberLossOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = HuberLossImpl

struct HuberLossImpl : public torch::nn::[Cloneable](utilities.html#_CPPv4I0EN5torch2nn9CloneableE)<HuberLossImpl>

Creates a criterion that uses a squared term if the absolute element-wise error falls below delta and a delta-scaled L1 term otherwise.

See [https://pytorch.org/docs/main/nn.html#torch.nn.HuberLoss](https://pytorch.org/docs/main/nn.html#torch.nn.HuberLoss) to learn about the exact behavior of this module.

See the documentation for `torch::nn::HuberLossOptions` class to learn what constructor arguments are supported for this module.

Example:

```
HuberLoss model(HuberLossOptions().reduction(torch::kNone).delta(0.5));
```

Public Functions

explicit HuberLossImpl(HuberLossOptions options_ = {})

virtual void reset() override

`reset()` must perform initialization of all members with reference semantics, most importantly parameters, buffers and submodules.

virtual void pretty_print(std::ostream &stream) const override

Pretty prints the `HuberLoss` module into the given `stream`.

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &target)

Public Members

HuberLossOptions options

The options with which this `[Module](index.html#PyTorchclasstorch_1_1nn_1_1_module)` was constructed.

## SmoothL1Loss

class SmoothL1Loss : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<SmoothL1LossImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `SmoothL1LossImpl`.

See the documentation for `SmoothL1LossImpl` class to learn what methods it provides, and examples of how to use `SmoothL1Loss` with `torch::nn::SmoothL1LossOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = SmoothL1LossImpl

struct SmoothL1LossImpl : public torch::nn::[Cloneable](utilities.html#_CPPv4I0EN5torch2nn9CloneableE)<SmoothL1LossImpl>

Creates a criterion that uses a squared term if the absolute element-wise error falls below beta and an L1 term otherwise.

It is less sensitive to outliers than the `MSELoss` and in some cases prevents exploding gradients (e.g. see the paper `Fast R-CNN` by Ross Girshick). See [https://pytorch.org/docs/main/nn.html#torch.nn.SmoothL1Loss](https://pytorch.org/docs/main/nn.html#torch.nn.SmoothL1Loss) to learn about the exact behavior of this module.

See the documentation for `torch::nn::SmoothL1LossOptions` class to learn what constructor arguments are supported for this module.

Example:

```
SmoothL1Loss model(SmoothL1LossOptions().reduction(torch::kNone).beta(0.5));
```

Public Functions

explicit SmoothL1LossImpl(SmoothL1LossOptions options = {})

virtual void reset() override

`reset()` must perform initialization of all members with reference semantics, most importantly parameters, buffers and submodules.

virtual void pretty_print(std::ostream &stream) const override

Pretty prints the `L1Loss` module into the given `stream`.

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &target)

Public Members

SmoothL1LossOptions options

The options with which this `[Module](index.html#PyTorchclasstorch_1_1nn_1_1_module)` was constructed.

## KLDivLoss

class KLDivLoss : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<KLDivLossImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `KLDivLossImpl`.

See the documentation for `KLDivLossImpl` class to learn what methods it provides, and examples of how to use `KLDivLoss` with `torch::nn::KLDivLossOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = KLDivLossImpl

struct KLDivLossImpl : public torch::nn::[Cloneable](utilities.html#_CPPv4I0EN5torch2nn9CloneableE)<KLDivLossImpl>

The Kullback-Leibler divergence loss measure See [https://pytorch.org/docs/main/nn.html#torch.nn.KLDivLoss](https://pytorch.org/docs/main/nn.html#torch.nn.KLDivLoss) to learn about the exact behavior of this module.

See the documentation for `torch::nn::KLDivLossOptions` class to learn what constructor arguments are supported for this module.

Example:

```
KLDivLoss model(KLDivLossOptions().reduction(torch::kNone));
```

Public Functions

explicit KLDivLossImpl(KLDivLossOptions options_ = {})

virtual void reset() override

`reset()` must perform initialization of all members with reference semantics, most importantly parameters, buffers and submodules.

virtual void pretty_print(std::ostream &stream) const override

Pretty prints the `KLDivLoss` module into the given `stream`.

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &target)

Public Members

KLDivLossOptions options

The options with which this `[Module](index.html#PyTorchclasstorch_1_1nn_1_1_module)` was constructed.

## CTCLoss

class CTCLoss : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<CTCLossImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `CTCLossImpl`.

See the documentation for `CTCLossImpl` class to learn what methods it provides, and examples of how to use `CTCLoss` with `torch::nn::CTCLossOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = CTCLossImpl

struct CTCLossImpl : public torch::nn::[Cloneable](utilities.html#_CPPv4I0EN5torch2nn9CloneableE)<CTCLossImpl>

The Connectionist Temporal Classification loss.

See [https://pytorch.org/docs/main/nn.html#torch.nn.CTCLoss](https://pytorch.org/docs/main/nn.html#torch.nn.CTCLoss) to learn about the exact behavior of this module.

See the documentation for `torch::nn::CTCLossOptions` class to learn what constructor arguments are supported for this module.

Example:

```
CTCLoss
model(CTCLossOptions().blank(42).zero_infinity(false).reduction(torch::kSum));
```

Public Functions

explicit CTCLossImpl(CTCLossOptions options_ = {})

virtual void reset() override

`reset()` must perform initialization of all members with reference semantics, most importantly parameters, buffers and submodules.

virtual void pretty_print(std::ostream &stream) const override

Pretty prints the `CTCLoss` module into the given `stream`.

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &log_probs, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &targets, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input_lengths, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &target_lengths)

Public Members

CTCLossOptions options

The options with which this `[Module](index.html#PyTorchclasstorch_1_1nn_1_1_module)` was constructed.

## PoissonNLLLoss

class PoissonNLLLoss : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<PoissonNLLLossImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `PoissonNLLLossImpl`.

See the documentation for `PoissonNLLLossImpl` class to learn what methods it provides, and examples of how to use `PoissonNLLLoss` with `torch::nn::PoissonNLLLossOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = PoissonNLLLossImpl

struct PoissonNLLLossImpl : public torch::nn::[Cloneable](utilities.html#_CPPv4I0EN5torch2nn9CloneableE)<PoissonNLLLossImpl>

Negative log likelihood loss with Poisson distribution of target.

See [https://pytorch.org/docs/main/nn.html#torch.nn.PoissonNLLLoss](https://pytorch.org/docs/main/nn.html#torch.nn.PoissonNLLLoss) to learn about the exact behavior of this module.

See the documentation for `torch::nn::PoissonNLLLossOptions` class to learn what constructor arguments are supported for this module.

Example:

```
PoissonNLLLoss
model(PoissonNLLLossOptions().log_input(false).full(true).eps(0.42).reduction(torch::kSum));
```

Public Functions

explicit PoissonNLLLossImpl(PoissonNLLLossOptions options_ = {})

virtual void reset() override

`reset()` must perform initialization of all members with reference semantics, most importantly parameters, buffers and submodules.

virtual void pretty_print(std::ostream &stream) const override

Pretty prints the `PoissonNLLLoss` module into the given `stream`.

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &log_input, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &targets)

Public Members

PoissonNLLLossOptions options

The options with which this `[Module](index.html#PyTorchclasstorch_1_1nn_1_1_module)` was constructed.

## MarginRankingLoss

class MarginRankingLoss : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<MarginRankingLossImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `MarginRankingLossImpl`.

See the documentation for `MarginRankingLossImpl` class to learn what methods it provides, and examples of how to use `MarginRankingLoss` with `torch::nn::MarginRankingLossOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = MarginRankingLossImpl

struct MarginRankingLossImpl : public torch::nn::[Cloneable](utilities.html#_CPPv4I0EN5torch2nn9CloneableE)<MarginRankingLossImpl>

Creates a criterion that measures the loss given inputs :math:`x1`, :math:`x2`, two 1D mini-batch `Tensors`, and a label 1D mini-batch tensor :math:`y` (containing 1 or -1).

See [https://pytorch.org/docs/main/nn.html#torch.nn.MarginRankingLoss](https://pytorch.org/docs/main/nn.html#torch.nn.MarginRankingLoss) to learn about the exact behavior of this module.

See the documentation for `torch::nn::MarginRankingLossOptions` class to learn what constructor arguments are supported for this module.

Example:

```
MarginRankingLoss
model(MarginRankingLossOptions().margin(0.5).reduction(torch::kSum));
```

Public Functions

explicit MarginRankingLossImpl(MarginRankingLossOptions options_ = {})

virtual void reset() override

`reset()` must perform initialization of all members with reference semantics, most importantly parameters, buffers and submodules.

virtual void pretty_print(std::ostream &stream) const override

Pretty prints the `MarginRankingLoss` module into the given `stream`.

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input1, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input2, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &targets)

Public Members

MarginRankingLossOptions options

The options with which this `[Module](index.html#PyTorchclasstorch_1_1nn_1_1_module)` was constructed.

## HingeEmbeddingLoss

class HingeEmbeddingLoss : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<HingeEmbeddingLossImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `HingeEmbeddingLossImpl`.

See the documentation for `HingeEmbeddingLossImpl` class to learn what methods it provides, and examples of how to use `HingeEmbeddingLoss` with `torch::nn::HingeEmbeddingLossOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = HingeEmbeddingLossImpl

struct HingeEmbeddingLossImpl : public torch::nn::[Cloneable](utilities.html#_CPPv4I0EN5torch2nn9CloneableE)<HingeEmbeddingLossImpl>

Creates a criterion that measures the loss given an input tensor :math:`x` and a labels tensor :math:`y` (containing 1 or -1).

See [https://pytorch.org/docs/main/nn.html#torch.nn.HingeEmbeddingLoss](https://pytorch.org/docs/main/nn.html#torch.nn.HingeEmbeddingLoss) to learn about the exact behavior of this module.

See the documentation for `torch::nn::HingeEmbeddingLossOptions` class to learn what constructor arguments are supported for this module.

Example:

```
HingeEmbeddingLoss
model(HingeEmbeddingLossOptions().margin(4).reduction(torch::kNone));
```

Public Functions

explicit HingeEmbeddingLossImpl(HingeEmbeddingLossOptions options_ = {})

virtual void reset() override

`reset()` must perform initialization of all members with reference semantics, most importantly parameters, buffers and submodules.

virtual void pretty_print(std::ostream &stream) const override

Pretty prints the `HingeEmbeddingLoss` module into the given `stream`.

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &target)

Public Members

HingeEmbeddingLossOptions options

The options with which this `[Module](index.html#PyTorchclasstorch_1_1nn_1_1_module)` was constructed.

## CosineEmbeddingLoss

class CosineEmbeddingLoss : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<CosineEmbeddingLossImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `CosineEmbeddingLossImpl`.

See the documentation for `CosineEmbeddingLossImpl` class to learn what methods it provides, and examples of how to use `CosineEmbeddingLoss` with `torch::nn::CosineEmbeddingLossOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = CosineEmbeddingLossImpl

struct CosineEmbeddingLossImpl : public torch::nn::[Cloneable](utilities.html#_CPPv4I0EN5torch2nn9CloneableE)<CosineEmbeddingLossImpl>

Creates a criterion that measures the loss given input tensors `input1`, `input2`, and a `Tensor` label `target` with values 1 or -1.

This is used for measuring whether two inputs are similar or dissimilar, using the cosine distance, and is typically used for learning nonlinear embeddings or semi-supervised learning. See [https://pytorch.org/docs/main/nn.html#torch.nn.CosineEmbeddingLoss](https://pytorch.org/docs/main/nn.html#torch.nn.CosineEmbeddingLoss) to learn about the exact behavior of this module.

See the documentation for `torch::nn::CosineEmbeddingLossOptions` class to learn what constructor arguments are supported for this module.

Example:

```
CosineEmbeddingLoss model(CosineEmbeddingLossOptions().margin(0.5));
```

Public Functions

explicit CosineEmbeddingLossImpl(CosineEmbeddingLossOptions options_ = {})

virtual void reset() override

`reset()` must perform initialization of all members with reference semantics, most importantly parameters, buffers and submodules.

virtual void pretty_print(std::ostream &stream) const override

Pretty prints the `CosineEmbeddingLoss` module into the given `stream`.

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input1, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input2, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &target)

Public Members

CosineEmbeddingLossOptions options

The options with which this `[Module](index.html#PyTorchclasstorch_1_1nn_1_1_module)` was constructed.

## MultiMarginLoss

class MultiMarginLoss : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<MultiMarginLossImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `MultiMarginLossImpl`.

See the documentation for `MultiMarginLossImpl` class to learn what methods it provides, and examples of how to use `MultiMarginLoss` with `torch::nn::MultiMarginLossOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = MultiMarginLossImpl

struct MultiMarginLossImpl : public torch::nn::[Cloneable](utilities.html#_CPPv4I0EN5torch2nn9CloneableE)<MultiMarginLossImpl>

Creates a criterion that optimizes a multi-class classification hinge loss (margin-based loss) between input :math:`x` (a 2D mini-batch `Tensor`) and output :math:`y` (which is a 1D tensor of target class indices, :math:`0 \leq y \leq \text{x.size}(1)-1`).

See [https://pytorch.org/docs/main/nn.html#torch.nn.MultiMarginLoss](https://pytorch.org/docs/main/nn.html#torch.nn.MultiMarginLoss) to learn about the exact behavior of this module.

See the documentation for `torch::nn::MultiMarginLossOptions` class to learn what constructor arguments are supported for this module.

Example:

```
MultiMarginLoss model(MultiMarginLossOptions().margin(2).weight(weight));
```

Public Functions

explicit MultiMarginLossImpl(MultiMarginLossOptions options_ = {})

virtual void reset() override

`reset()` must perform initialization of all members with reference semantics, most importantly parameters, buffers and submodules.

virtual void pretty_print(std::ostream &stream) const override

Pretty prints the `MultiMarginLoss` module into the given `stream`.

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &target)

Public Members

MultiMarginLossOptions options

The options with which this `[Module](index.html#PyTorchclasstorch_1_1nn_1_1_module)` was constructed.

## MultiLabelMarginLoss

class MultiLabelMarginLoss : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<MultiLabelMarginLossImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `MultiLabelMarginLossImpl`.

See the documentation for `MultiLabelMarginLossImpl` class to learn what methods it provides, and examples of how to use `MultiLabelMarginLoss` with `torch::nn::MultiLabelMarginLossOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = MultiLabelMarginLossImpl

struct MultiLabelMarginLossImpl : public torch::nn::[Cloneable](utilities.html#_CPPv4I0EN5torch2nn9CloneableE)<MultiLabelMarginLossImpl>

Creates a criterion that optimizes a multi-class multi-classification hinge loss (margin-based loss) between input :math:`x` (a 2D mini-batch `Tensor`) and output :math:`y` (which is a 2D `Tensor` of target class indices).

See [https://pytorch.org/docs/main/nn.html#torch.nn.MultiLabelMarginLoss](https://pytorch.org/docs/main/nn.html#torch.nn.MultiLabelMarginLoss) to learn about the exact behavior of this module.

See the documentation for `torch::nn::MultiLabelMarginLossOptions` class to learn what constructor arguments are supported for this module.

Example:

```
MultiLabelMarginLoss model(MultiLabelMarginLossOptions(torch::kNone));
```

Public Functions

explicit MultiLabelMarginLossImpl(MultiLabelMarginLossOptions options_ = {})

virtual void reset() override

`reset()` must perform initialization of all members with reference semantics, most importantly parameters, buffers and submodules.

virtual void pretty_print(std::ostream &stream) const override

Pretty prints the `L1Loss` module into the given `stream`.

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &target)

Public Members

MultiLabelMarginLossOptions options

The options with which this `[Module](index.html#PyTorchclasstorch_1_1nn_1_1_module)` was constructed.

## MultiLabelSoftMarginLoss

class MultiLabelSoftMarginLoss : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<MultiLabelSoftMarginLossImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `MultiLabelSoftMarginLossImpl`.

See the documentation for `MultiLabelSoftMarginLossImpl` class to learn what methods it provides, and examples of how to use `MultiLabelSoftMarginLoss` with `torch::nn::MultiLabelSoftMarginLossOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = MultiLabelSoftMarginLossImpl

struct MultiLabelSoftMarginLossImpl : public torch::nn::[Cloneable](utilities.html#_CPPv4I0EN5torch2nn9CloneableE)<MultiLabelSoftMarginLossImpl>

Creates a criterion that optimizes a multi-label one-versus-all loss based on max-entropy, between input :math:`x` and target :math:`y` of size :math:`(N, C)`.

See [https://pytorch.org/docs/main/nn.html#torch.nn.MultiLabelSoftMarginLoss](https://pytorch.org/docs/main/nn.html#torch.nn.MultiLabelSoftMarginLoss) to learn about the exact behavior of this module.

See the documentation for `torch::nn::MultiLabelSoftMarginLossOptions` class to learn what constructor arguments are supported for this module.

Example:

```
MultiLabelSoftMarginLoss
model(MultiLabelSoftMarginLossOptions().reduction(torch::kNone).weight(weight));
```

Public Functions

explicit MultiLabelSoftMarginLossImpl(MultiLabelSoftMarginLossOptions options_ = {})

virtual void pretty_print(std::ostream &stream) const override

Pretty prints the `MultiLabelSoftMarginLoss` module into the given `stream`.

virtual void reset() override

`reset()` must perform initialization of all members with reference semantics, most importantly parameters, buffers and submodules.

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &target)

Public Members

MultiLabelSoftMarginLossOptions options

The options with which this `[Module](index.html#PyTorchclasstorch_1_1nn_1_1_module)` was constructed.

## SoftMarginLoss

class SoftMarginLoss : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<SoftMarginLossImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `SoftMarginLossImpl`.

See the documentation for `SoftMarginLossImpl` class to learn what methods it provides, and examples of how to use `SoftMarginLoss` with `torch::nn::SoftMarginLossOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = SoftMarginLossImpl

struct SoftMarginLossImpl : public torch::nn::[Cloneable](utilities.html#_CPPv4I0EN5torch2nn9CloneableE)<SoftMarginLossImpl>

Creates a criterion that optimizes a two-class classification logistic loss between input tensor :math:`x` and target tensor :math:`y` (containing 1 or -1).

See [https://pytorch.org/docs/main/nn.html#torch.nn.SoftMarginLoss](https://pytorch.org/docs/main/nn.html#torch.nn.SoftMarginLoss) to learn about the exact behavior of this module.

See the documentation for `torch::nn::SoftMarginLossOptions` class to learn what constructor arguments are supported for this module.

Example:

```
SoftMarginLoss model(SoftMarginLossOptions(torch::kNone));
```

Public Functions

explicit SoftMarginLossImpl(SoftMarginLossOptions options_ = {})

virtual void pretty_print(std::ostream &stream) const override

Pretty prints the `SoftMarginLoss` module into the given `stream`.

virtual void reset() override

`reset()` must perform initialization of all members with reference semantics, most importantly parameters, buffers and submodules.

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &target)

Public Members

SoftMarginLossOptions options

The options with which this `[Module](index.html#PyTorchclasstorch_1_1nn_1_1_module)` was constructed.

## TripletMarginLoss

class TripletMarginLoss : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<TripletMarginLossImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `TripletMarginLossImpl`.

See the documentation for `TripletMarginLossImpl` class to learn what methods it provides, and examples of how to use `TripletMarginLoss` with `torch::nn::TripletMarginLossOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = TripletMarginLossImpl

struct TripletMarginLossImpl : public torch::nn::[Cloneable](utilities.html#_CPPv4I0EN5torch2nn9CloneableE)<TripletMarginLossImpl>

Creates a criterion that measures the triplet loss given an input tensors :math:`x1`, :math:`x2`, :math:`x3` and a margin with a value greater than :math:`0`.

This is used for measuring a relative similarity between samples. A triplet is composed by `a`, `p` and `n` (i.e., `anchor`, `positive examples` and `negative examples` respectively). The shapes of all input tensors should be :math:`(N, D)`. See [https://pytorch.org/docs/main/nn.html#torch.nn.TripletMarginLoss](https://pytorch.org/docs/main/nn.html#torch.nn.TripletMarginLoss) to learn about the exact behavior of this module.

See the documentation for `torch::nn::TripletMarginLossOptions` class to learn what constructor arguments are supported for this module.

Example:

```
TripletMarginLoss
model(TripletMarginLossOptions().margin(3).p(2).eps(1e-06).swap(false));
```

Public Functions

explicit TripletMarginLossImpl(TripletMarginLossOptions options_ = {})

virtual void reset() override

`reset()` must perform initialization of all members with reference semantics, most importantly parameters, buffers and submodules.

virtual void pretty_print(std::ostream &stream) const override

Pretty prints the `TripletMarginLoss` module into the given `stream`.

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &anchor, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &positive, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &negative)

Public Members

TripletMarginLossOptions options

The options with which this `[Module](index.html#PyTorchclasstorch_1_1nn_1_1_module)` was constructed.

## TripletMarginWithDistanceLoss

class TripletMarginWithDistanceLoss : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<TripletMarginWithDistanceLossImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `TripletMarginWithDistanceLossImpl`.

See the documentation for `TripletMarginWithDistanceLossImpl` class to learn what methods it provides, and examples of how to use `TripletMarginWithDistanceLoss` with `torch::nn::TripletMarginWithDistanceLossOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = TripletMarginWithDistanceLossImpl

struct TripletMarginWithDistanceLossImpl : public torch::nn::[Cloneable](utilities.html#_CPPv4I0EN5torch2nn9CloneableE)<TripletMarginWithDistanceLossImpl>

Creates a criterion that measures the triplet loss given input tensors :math:`a`, :math:`p`, and :math:`n` (representing anchor, positive, and negative examples, respectively); and a nonnegative, real-valued function ("distance function") used to compute the relationships between the anchor and positive example ("positive distance") and the anchor and negative example ("negative distance").

See [https://pytorch.org/docs/main/nn.html#torch.nn.TripletMarginWithDistanceLoss](https://pytorch.org/docs/main/nn.html#torch.nn.TripletMarginWithDistanceLoss) to learn about the exact behavior of this module.

See the documentation for `torch::nn::TripletMarginWithDistanceLossOptions` class to learn what constructor arguments are supported for this module.

Example:

```
TripletMarginWithDistanceLoss
model(TripletMarginWithDistanceLossOptions().margin(3).swap(false));
```

Public Functions

explicit TripletMarginWithDistanceLossImpl(TripletMarginWithDistanceLossOptions options_ = {})

virtual void reset() override

`reset()` must perform initialization of all members with reference semantics, most importantly parameters, buffers and submodules.

virtual void pretty_print(std::ostream &stream) const override

Pretty prints the `TripletMarginWithDistanceLoss` module into the given `stream`.

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &anchor, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &positive, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &negative)

Public Members

TripletMarginWithDistanceLossOptions options

The options with which this `[Module](index.html#PyTorchclasstorch_1_1nn_1_1_module)` was constructed.