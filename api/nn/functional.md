# Functional API

The `torch::nn::functional` namespace provides stateless versions of neural
network operations. Unlike module classes, functional operations do not hold
learnable parameters -- you pass weights explicitly.

**When to use functional vs modules:**

- Use **modules** (`torch::nn::Conv2d`) when you need learnable parameters
managed automatically (training, saving, loading).
- Use **functional** (`torch::nn::functional::conv2d`) when you already have
weights as tensors, or for operations without parameters (e.g., `relu`).

```
#include <torch/nn/functional.h>
namespace F = torch::nn::functional;

// Stateless activation -- no module needed
auto output = F::relu(input);

// Convolution with explicit weight tensor
auto output = F::conv2d(input, weight, F::Conv2dFuncOptions().stride(1).padding(1));

// Softmax along a dimension
auto probs = F::softmax(logits, F::SoftmaxFuncOptions(/*dim=*/1));
```

## Activation Functions

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::elu([Tensor](../aten/tensor.html#_CPPv46Tensorv) input, const ELUFuncOptions &options = {})

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.elu](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.elu) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::ELUFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::elu(x, F::ELUFuncOptions().alpha(0.42).inplace(true));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::selu([Tensor](../aten/tensor.html#_CPPv46Tensorv) input, const SELUFuncOptions &options = {})

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.selu](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.selu) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::SELUFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::selu(input, F::SELUFuncOptions(false));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::hardshrink(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const HardshrinkFuncOptions &options = {})

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.hardshrink](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.hardshrink) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::HardshrinkFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::hardshrink(x, F::HardshrinkFuncOptions().lambda(0.42));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::hardtanh([Tensor](../aten/tensor.html#_CPPv46Tensorv) input, const HardtanhFuncOptions &options = {})

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.hardtanh](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.hardtanh) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::HardtanhFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::hardtanh(x,
F::HardtanhFuncOptions().min_val(-1.0).max_val(1.0).inplace(true));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::leaky_relu([Tensor](../aten/tensor.html#_CPPv46Tensorv) input, const LeakyReLUFuncOptions &options = {})

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.leaky_relu](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.leaky_relu) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::LeakyReLUFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::leaky_relu(x,
F::LeakyReLUFuncOptions().negative_slope(0.42).inplace(true));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::logsigmoid(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input)

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::glu(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const GLUFuncOptions &options = {})

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.glu](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.glu) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::GLUFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::glu(input, GLUFuncOptions(1));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::gelu(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const GELUFuncOptions &options = {})

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::silu(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input)

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::mish(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input)

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::prelu(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &weight)

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::relu([Tensor](../aten/tensor.html#_CPPv46Tensorv) input, const ReLUFuncOptions &options = {})

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.relu](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.relu) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::ReLUFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::relu(x, F::ReLUFuncOptions().inplace(true));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::relu6([Tensor](../aten/tensor.html#_CPPv46Tensorv) input, const ReLU6FuncOptions &options = {})

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.relu6](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.relu6) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::ReLU6FuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::relu6(x, F::ReLU6FuncOptions().inplace(true));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::rrelu([Tensor](../aten/tensor.html#_CPPv46Tensorv) input, const RReLUFuncOptions &options = {})

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.rrelu](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.rrelu) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::RReLUFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::rrelu(x, F::RReLUFuncOptions().lower(0.1).upper(0.4).inplace(true));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::celu([Tensor](../aten/tensor.html#_CPPv46Tensorv) input, const CELUFuncOptions &options = {})

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.celu](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.celu) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::CELUFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::celu(x, F::CELUFuncOptions().alpha(0.42).inplace(true));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::softplus(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const SoftplusFuncOptions &options = {})

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.softplus](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.softplus) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::SoftplusFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::softplus(x, F::SoftplusFuncOptions().beta(0.5).threshold(3.0));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::softshrink(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const SoftshrinkFuncOptions &options = {})

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.softshrink](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.softshrink) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::SoftshrinkFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::softshrink(x, F::SoftshrinkFuncOptions(0.42));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::softsign(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input)

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::tanhshrink(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input)

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::threshold([Tensor](../aten/tensor.html#_CPPv46Tensorv) input, const ThresholdFuncOptions &options)

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.threshold](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.threshold) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::ThresholdFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::threshold(x, F::ThresholdFuncOptions(0.5, 0.5).inplace(true));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::softmax(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const SoftmaxFuncOptions &options)

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.softmax](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.softmax) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::SoftmaxFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::softmax(input, F::SoftmaxFuncOptions(1));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::softmin(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const SoftminFuncOptions &options)

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.softmin](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.softmin) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::SoftminFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::softmin(input, F::SoftminFuncOptions(1));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::log_softmax(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const LogSoftmaxFuncOptions &options)

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.log_softmax](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.log_softmax) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::LogSoftmaxFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::log_softmax(input, LogSoftmaxFuncOptions(1));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::gumbel_softmax(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &logits, const GumbelSoftmaxFuncOptions &options = {})

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.gumbel_softmax](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.gumbel_softmax) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::GumbelSoftmaxFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::gumbel_softmax(logits, F::GumbelSoftmaxFuncOptions().hard(true).dim(-1));
```

## Convolution Functions

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::conv1d(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &weight, const Conv1dFuncOptions &options = {})

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.conv1d](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.conv1d) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::Conv1dFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::conv1d(x, weight, F::Conv1dFuncOptions().stride(1));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::conv2d(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &weight, const Conv2dFuncOptions &options = {})

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.conv2d](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.conv2d) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::Conv2dFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::conv2d(x, weight, F::Conv2dFuncOptions().stride(1));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::conv3d(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &weight, const Conv3dFuncOptions &options = {})

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.conv3d](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.conv3d) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::Conv3dFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::conv3d(x, weight, F::Conv3dFuncOptions().stride(1));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::conv_transpose1d(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &weight, const ConvTranspose1dFuncOptions &options = {})

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.conv_transpose1d](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.conv_transpose1d) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::ConvTranspose1dFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::conv_transpose1d(x, weight, F::ConvTranspose1dFuncOptions().stride(1));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::conv_transpose2d(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &weight, const ConvTranspose2dFuncOptions &options = {})

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.conv_transpose2d](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.conv_transpose2d) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::ConvTranspose2dFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::conv_transpose2d(x, weight, F::ConvTranspose2dFuncOptions().stride(1));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::conv_transpose3d(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &weight, const ConvTranspose3dFuncOptions &options = {})

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.conv_transpose3d](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.conv_transpose3d) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::ConvTranspose3dFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::conv_transpose3d(x, weight, F::ConvTranspose3dFuncOptions().stride(1));
```

## Pooling Functions

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::avg_pool1d(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const AvgPool1dFuncOptions &options)

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.avg_pool1d](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.avg_pool1d) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::AvgPool1dFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::avg_pool1d(x, F::AvgPool1dFuncOptions(3).stride(2));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::avg_pool2d(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const AvgPool2dFuncOptions &options)

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.avg_pool2d](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.avg_pool2d) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::AvgPool2dFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::avg_pool2d(x, F::AvgPool2dFuncOptions(3).stride(2));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::avg_pool3d(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const AvgPool3dFuncOptions &options)

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.avg_pool3d](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.avg_pool3d) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::AvgPool3dFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::avg_pool3d(x, F::AvgPool3dFuncOptions(3).stride(2));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::max_pool1d(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const MaxPool1dFuncOptions &options)

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.max_pool1d](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.max_pool1d) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::MaxPool1dFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::max_pool1d(x, F::MaxPool1dFuncOptions(3).stride(2));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::max_pool2d(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const MaxPool2dFuncOptions &options)

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.max_pool2d](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.max_pool2d) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::MaxPool2dFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::max_pool2d(x, F::MaxPool2dFuncOptions(3).stride(2));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::max_pool3d(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const MaxPool3dFuncOptions &options)

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.max_pool3d](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.max_pool3d) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::MaxPool3dFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::max_pool3d(x, F::MaxPool3dFuncOptions(3).stride(2));
```

inline std::tuple<[Tensor](../aten/tensor.html#_CPPv46Tensorv), [Tensor](../aten/tensor.html#_CPPv46Tensorv)> torch::nn::functional::max_pool1d_with_indices(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const MaxPool1dFuncOptions &options)

See the documentation for `torch::nn::functional::MaxPool1dFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::max_pool1d_with_indices(x, F::MaxPool1dFuncOptions(3).stride(2));
```

inline std::tuple<[Tensor](../aten/tensor.html#_CPPv46Tensorv), [Tensor](../aten/tensor.html#_CPPv46Tensorv)> torch::nn::functional::max_pool2d_with_indices(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const MaxPool2dFuncOptions &options)

See the documentation for `torch::nn::functional::MaxPool2dFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::max_pool2d_with_indices(x, F::MaxPool2dFuncOptions(3).stride(2));
```

inline std::tuple<[Tensor](../aten/tensor.html#_CPPv46Tensorv), [Tensor](../aten/tensor.html#_CPPv46Tensorv)> torch::nn::functional::max_pool3d_with_indices(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const MaxPool3dFuncOptions &options)

See the documentation for `torch::nn::functional::MaxPool3dFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::max_pool3d_with_indices(x, F::MaxPool3dFuncOptions(3).stride(2));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::adaptive_max_pool1d(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const AdaptiveMaxPool1dFuncOptions &options)

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.adaptive_max_pool1d](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.adaptive_max_pool1d) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::AdaptiveMaxPool1dFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::adaptive_max_pool1d(x, F::AdaptiveMaxPool1dFuncOptions(3));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::adaptive_max_pool2d(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const AdaptiveMaxPool2dFuncOptions &options)

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.adaptive_max_pool2d](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.adaptive_max_pool2d) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::AdaptiveMaxPool2dFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::adaptive_max_pool2d(x, F::AdaptiveMaxPool2dFuncOptions(3));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::adaptive_max_pool3d(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const AdaptiveMaxPool3dFuncOptions &options)

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.adaptive_max_pool3d](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.adaptive_max_pool3d) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::AdaptiveMaxPool3dFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::adaptive_max_pool3d(x, F::AdaptiveMaxPool3dFuncOptions(3));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::adaptive_avg_pool1d(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const AdaptiveAvgPool1dFuncOptions &options)

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.adaptive_avg_pool1d](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.adaptive_avg_pool1d) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::AdaptiveAvgPool1dFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::adaptive_avg_pool1d(x, F::AdaptiveAvgPool1dFuncOptions(3));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::adaptive_avg_pool2d(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const AdaptiveAvgPool2dFuncOptions &options)

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.adaptive_avg_pool2d](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.adaptive_avg_pool2d) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::AdaptiveAvgPool2dFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::adaptive_avg_pool2d(x, F::AdaptiveAvgPool2dFuncOptions(3));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::adaptive_avg_pool3d(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const AdaptiveAvgPool3dFuncOptions &options)

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.adaptive_avg_pool3d](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.adaptive_avg_pool3d) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::AdaptiveAvgPool3dFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::adaptive_avg_pool3d(x, F::AdaptiveAvgPool3dFuncOptions(3));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::max_unpool1d(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &indices, const MaxUnpool1dFuncOptions &options)

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.max_unpool1d](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.max_unpool1d) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::MaxUnpool1dFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::max_unpool1d(x, indices,
F::MaxUnpool1dFuncOptions(3).stride(2).padding(1));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::max_unpool2d(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &indices, const MaxUnpool2dFuncOptions &options)

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.max_unpool2d](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.max_unpool2d) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::MaxUnpool2dFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::max_unpool2d(x, indices,
F::MaxUnpool2dFuncOptions(3).stride(2).padding(1));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::max_unpool3d(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &indices, const MaxUnpool3dFuncOptions &options)

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.max_unpool3d](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.max_unpool3d) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::MaxUnpool3dFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::max_unpool3d(x, indices, F::MaxUnpool3dFuncOptions(3));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::fractional_max_pool2d(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const FractionalMaxPool2dFuncOptions &options)

See the documentation for `torch::nn::functional::FractionalMaxPool2dFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::fractional_max_pool2d(x,
F::FractionalMaxPool2dFuncOptions(3).output_size(2));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::fractional_max_pool3d(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const FractionalMaxPool3dFuncOptions &options)

See the documentation for `torch::nn::functional::FractionalMaxPool3dFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::fractional_max_pool3d(x,
F::FractionalMaxPool3dFuncOptions(3).output_size(2));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::lp_pool1d(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const LPPool1dFuncOptions &options)

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.lp_pool1d](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.lp_pool1d) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::LPPool1dFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::lp_pool1d(x, F::LPPool1dFuncOptions(2, 3).stride(2));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::lp_pool2d(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const LPPool2dFuncOptions &options)

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.lp_pool2d](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.lp_pool2d) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::LPPool2dFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::lp_pool2d(x, F::LPPool2dFuncOptions(2, {2, 3}).stride(2));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::lp_pool3d(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const LPPool3dFuncOptions &options)

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.lp_pool3d](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.lp_pool3d) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::LPPool3dFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::lp_pool3d(x, F::LPPool3dFuncOptions(3, {3, 3, 5}).stride(3));
```

## Linear Functions

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::linear(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &weight, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &bias = {})

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::bilinear(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input1, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input2, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &weight, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &bias = [Tensor](../aten/tensor.html#_CPPv46Tensorv)())

## Dropout Functions

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::dropout([Tensor](../aten/tensor.html#_CPPv46Tensorv) input, const DropoutFuncOptions &options = {})

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.dropout](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.dropout) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::DropoutFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::dropout(input, F::DropoutFuncOptions().p(0.5));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::dropout2d([Tensor](../aten/tensor.html#_CPPv46Tensorv) input, const Dropout2dFuncOptions &options = {})

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.dropout2d](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.dropout2d) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::Dropout2dFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::dropout2d(input, F::Dropout2dFuncOptions().p(0.5));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::dropout3d([Tensor](../aten/tensor.html#_CPPv46Tensorv) input, const Dropout3dFuncOptions &options = {})

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.dropout3d](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.dropout3d) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::Dropout3dFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::dropout3d(input, F::Dropout3dFuncOptions().p(0.5));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::alpha_dropout([Tensor](../aten/tensor.html#_CPPv46Tensorv) input, const AlphaDropoutFuncOptions &options = {})

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.alpha_dropout](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.alpha_dropout) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::AlphaDropoutFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::alpha_dropout(input,
F::AlphaDropoutFuncOptions().p(0.5).training(false));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::feature_alpha_dropout([Tensor](../aten/tensor.html#_CPPv46Tensorv) input, const FeatureAlphaDropoutFuncOptions &options = {})

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.feature_alpha_dropout](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.feature_alpha_dropout) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::FeatureAlphaDropoutFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::feature_alpha_dropout(input,
F::FeatureAlphaDropoutFuncOptions().p(0.5).training(false));
```

## Embedding Functions

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::one_hot(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &tensor, int64_t num_classes = -1)

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::embedding(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &weight, const EmbeddingFuncOptions &options = {})

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.embedding](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.embedding) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::EmbeddingFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::embedding(input, weight,
F::EmbeddingFuncOptions().norm_type(2.5).scale_grad_by_freq(true).sparse(true));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::embedding_bag(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &weight, const EmbeddingBagFuncOptions &options = {})

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.embedding_bag](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.embedding_bag) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::EmbeddingBagFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::embedding_bag(input, weight,
F::EmbeddingBagFuncOptions().mode(torch::kSum).offsets(offsets));
```

## Normalization Functions

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::batch_norm(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &running_mean, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &running_var, const BatchNormFuncOptions &options = {})

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.batch_norm](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.batch_norm) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::BatchNormFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::batch_norm(input, mean, variance,
F::BatchNormFuncOptions().weight(weight).bias(bias).momentum(0.1).eps(1e-05).training(false));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::instance_norm(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const InstanceNormFuncOptions &options = {})

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.instance_norm](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.instance_norm) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::InstanceNormFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::instance_norm(input,
F::InstanceNormFuncOptions().running_mean(mean).running_var(variance).weight(weight).bias(bias).momentum(0.1).eps(1e-5));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::layer_norm(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const LayerNormFuncOptions &options)

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.layer_norm](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.layer_norm) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::LayerNormFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::layer_norm(input, F::LayerNormFuncOptions({2, 2}).eps(2e-5));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::group_norm(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const GroupNormFuncOptions &options)

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.group_norm](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.group_norm) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::GroupNormFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::group_norm(input, F::GroupNormFuncOptions(2).eps(2e-5));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::local_response_norm(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const LocalResponseNormFuncOptions &options)

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.local_response_norm](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.local_response_norm) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::LocalResponseNormFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::local_response_norm(x, F::LocalResponseNormFuncOptions(2));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::normalize(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, NormalizeFuncOptions options = {})

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.normalize](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.normalize) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::NormalizeFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::normalize(input, F::NormalizeFuncOptions().p(1).dim(-1));
```

## Loss Functions

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::l1_loss(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &target, const L1LossFuncOptions &options = {})

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.l1_loss](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.l1_loss) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::L1LossFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::l1_loss(input, target, F::L1LossFuncOptions(torch::kNone));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::mse_loss(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &target, const MSELossFuncOptions &options = {})

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.mse_loss](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.mse_loss) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::MSELossFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::mse_loss(input, target, F::MSELossFuncOptions(torch::kNone));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::binary_cross_entropy(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &target, const BinaryCrossEntropyFuncOptions &options = {})

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.binary_cross_entropy](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.binary_cross_entropy) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::BinaryCrossEntropyFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::binary_cross_entropy(input, target,
F::BinaryCrossEntropyFuncOptions().weight(weight));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::binary_cross_entropy_with_logits(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &target, const BinaryCrossEntropyWithLogitsFuncOptions &options = {})

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.binary_cross_entropy_with_logits](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.binary_cross_entropy_with_logits) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::BinaryCrossEntropyWithLogitsFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::binary_cross_entropy_with_logits(input, target,
F::BinaryCrossEntropyWithLogitsFuncOptions().pos_weight(pos_weight).reduction(torch::kSum));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::cross_entropy(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &target, const CrossEntropyFuncOptions &options = {})

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.cross_entropy](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.cross_entropy) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::CrossEntropyFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::cross_entropy(input, target,
F::CrossEntropyFuncOptions().ignore_index(-100).reduction(torch::kMean));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::nll_loss(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &target, const NLLLossFuncOptions &options = {})

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.nll_loss](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.nll_loss) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::NLLLossFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::nll_loss(input, target,
F::NLLLossFuncOptions().ignore_index(-100).reduction(torch::kMean));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::kl_div(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &target, const KLDivFuncOptions &options = {})

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.kl_div](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.kl_div) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::KLDivFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::kl_div(input, target,
F::KLDivFuncOptions.reduction(torch::kNone).log_target(false));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::smooth_l1_loss(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &target, const SmoothL1LossFuncOptions &options = {})

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.smooth_l1_loss](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.smooth_l1_loss) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::SmoothL1LossFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::smooth_l1_loss(input, target, F::SmoothL1LossFuncOptions(torch::kNone));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::huber_loss(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &target, const HuberLossFuncOptions &options = {})

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.huber_loss](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.huber_loss) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::HuberLossFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::huber_loss(input, target,
F::HuberLossFuncOptions().reduction(torch::kNone).delta(0.5));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::hinge_embedding_loss(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &target, const HingeEmbeddingLossFuncOptions &options = {})

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.hinge_embedding_loss](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.hinge_embedding_loss) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::HingeEmbeddingLossFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::hinge_embedding_loss(input, target,
F::HingeEmbeddingLossFuncOptions().margin(2));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::multi_margin_loss(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &target, const MultiMarginLossFuncOptions &options = {})

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.multi_margin_loss](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.multi_margin_loss) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::MultiMarginLossFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::multi_margin_loss(input, target,
F::MultiMarginLossFuncOptions().margin(2).weight(weight));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::cosine_embedding_loss(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input1, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input2, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &target, const CosineEmbeddingLossFuncOptions &options = {})

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.cosine_embedding_loss](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.cosine_embedding_loss) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::CosineEmbeddingLossFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::cosine_embedding_loss(input1, input2, target,
F::CosineEmbeddingLossFuncOptions().margin(0.5));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::margin_ranking_loss(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input1, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input2, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &target, const MarginRankingLossFuncOptions &options = {})

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.margin_ranking_loss](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.margin_ranking_loss) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::MarginRankingLossFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::margin_ranking_loss(input1, input2, target,
F::MarginRankingLossFuncOptions().margin(0.5).reduction(torch::kSum));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::multilabel_margin_loss(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &target, const MultilabelMarginLossFuncOptions &options = {})

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.multilabel_margin_loss](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.multilabel_margin_loss) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::MultilabelMarginLossFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::multilabel_margin_loss(input, target,
F::MultilabelMarginLossFuncOptions(torch::kNone));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::soft_margin_loss(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &target, const SoftMarginLossFuncOptions &options = {})

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.soft_margin_loss](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.soft_margin_loss) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::SoftMarginLossFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::soft_margin_loss(input, target,
F::SoftMarginLossFuncOptions(torch::kNone));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::multilabel_soft_margin_loss(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &target, const MultilabelSoftMarginLossFuncOptions &options = {})

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.multilabel_soft_margin_loss](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.multilabel_soft_margin_loss) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::MultilabelSoftMarginLossFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::multilabel_soft_margin_loss(input, target,
F::MultilabelSoftMarginLossFuncOptions().reduction(torch::kNone).weight(weight));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::triplet_margin_loss(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &anchor, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &positive, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &negative, const TripletMarginLossFuncOptions &options = {})

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.triplet_margin_loss](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.triplet_margin_loss) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::TripletMarginLossFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::triplet_margin_loss(anchor, positive, negative,
F::TripletMarginLossFuncOptions().margin(1.0));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::triplet_margin_with_distance_loss(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &anchor, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &positive, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &negative, const TripletMarginWithDistanceLossFuncOptions &options = {})

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.triplet_margin_with_distance_loss](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.triplet_margin_with_distance_loss) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::TripletMarginWithDistanceLossFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::triplet_margin_with_distance_loss(anchor, positive, negative,
F::TripletMarginWithDistanceLossFuncOptions().margin(1.0));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::ctc_loss(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &log_probs, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &targets, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input_lengths, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &target_lengths, const CTCLossFuncOptions &options = {})

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.ctc_loss](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.ctc_loss) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::CTCLossFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::ctc_loss(log_probs, targets, input_lengths, target_lengths,
F::CTCLossFuncOptions().reduction(torch::kNone));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::poisson_nll_loss(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &target, const PoissonNLLLossFuncOptions &options = {})

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.poisson_nll_loss](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.poisson_nll_loss) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::PoissonNLLLossFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::poisson_nll_loss(input, target,
F::PoissonNLLLossFuncOptions().reduction(torch::kNone));
```

## Distance Functions

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::cosine_similarity(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &x1, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &x2, const CosineSimilarityFuncOptions &options = {})

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.cosine_similarity](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.cosine_similarity) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::CosineSimilarityFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::cosine_similarity(input1, input2,
F::CosineSimilarityFuncOptions().dim(1));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::pairwise_distance(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &x1, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &x2, const PairwiseDistanceFuncOptions &options = {})

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.pairwise_distance](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.pairwise_distance) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::PairwiseDistanceFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::pairwise_distance(input1, input2, F::PairwiseDistanceFuncOptions().p(1));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::pdist(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, double p = 2.0)

Computes the p-norm distance between every pair of row vectors in the input.

This function will be faster if the rows are contiguous.

## Vision Functions

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::interpolate(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const InterpolateFuncOptions &options = {})

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.interpolate](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.interpolate) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::InterpolateFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::interpolate(input,
F::InterpolateFuncOptions().size({4}).mode(torch::kNearest));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::affine_grid(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &theta, const IntArrayRef &size, bool align_corners = false)

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::grid_sample(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &grid, const GridSampleFuncOptions &options = {})

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.grid_sample](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.grid_sample) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::GridSampleFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::grid_sample(input, grid,
F::GridSampleFuncOptions().mode(torch::kBilinear).padding_mode(torch::kZeros).align_corners(true));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::pad(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const PadFuncOptions &options)

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.pad](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.pad) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::PadFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::pad(input, F::PadFuncOptions({1, 2, 2, 1, 1,
2}).mode(torch::kReplicate));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::pixel_shuffle(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const PixelShuffleFuncOptions &options)

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.pixel_shuffle](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.pixel_shuffle) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::PixelShuffleFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::pixel_shuffle(x, F::PixelShuffleFuncOptions(2));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::pixel_unshuffle(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const PixelUnshuffleFuncOptions &options)

## Fold/Unfold

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::fold(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const FoldFuncOptions &options)

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.fold](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.fold) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::FoldFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::fold(input, F::FoldFuncOptions({3, 2}, {2, 2}));
```

inline [Tensor](../aten/tensor.html#_CPPv46Tensorv) torch::nn::functional::unfold(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const UnfoldFuncOptions &options)

See [https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.unfold](https://pytorch.org/docs/main/nn.functional.html#torch.nn.functional.unfold) about the exact behavior of this functional.

See the documentation for `torch::nn::functional::UnfoldFuncOptions` class to learn what optional arguments are supported for this functional.

Example:

```
namespace F = torch::nn::functional;
F::unfold(input, F::UnfoldFuncOptions({2, 2}).padding(1).stride(2));
```

## Functional Options Structs

Each functional operation that takes configuration uses a corresponding options
struct. The naming convention is `<Operation>FuncOptions`.

**Activation Options:**

using torch::nn::functional::ELUFuncOptions = ELUOptions

Options for `torch::nn::functional::elu`.

See the documentation for `torch::nn::ELUOptions` class to learn what arguments are supported.

Example:

```
namespace F = torch::nn::functional;
F::elu(x, F::ELUFuncOptions().alpha(0.42).inplace(true));
```

using torch::nn::functional::SELUFuncOptions = SELUOptions

Options for `torch::nn::functional::selu`.

See the documentation for `torch::nn::SELUOptions` class to learn what arguments are supported.

Example:

```
namespace F = torch::nn::functional;
F::selu(input, F::SELUFuncOptions(false));
```

using torch::nn::functional::GLUFuncOptions = GLUOptions

Options for `torch::nn::functional::glu`.

See the documentation for `torch::nn::GLUOptions` class to learn what arguments are supported.

Example:

```
namespace F = torch::nn::functional;
F::glu(input, GLUFuncOptions(1));
```

using torch::nn::functional::GELUFuncOptions = GELUOptions

Options for `torch::nn::functional::gelu`.

See the documentation for `torch::nn::GELUOptions` class to learn what arguments are supported.

Example:

```
namespace F = torch::nn::functional;
F::gelu(input, F::GELUFuncOptions().approximate("none"));
```

using torch::nn::functional::HardshrinkFuncOptions = HardshrinkOptions

Options for `torch::nn::functional::hardshrink`.

See the documentation for `torch::nn::HardshrinkOptions` class to learn what arguments are supported.

Example:

```
namespace F = torch::nn::functional;
F::hardshrink(x, F::HardshrinkFuncOptions().lambda(0.42));
```

using torch::nn::functional::HardtanhFuncOptions = HardtanhOptions

Options for `torch::nn::functional::hardtanh`.

See the documentation for `torch::nn::HardtanhOptions` class to learn what arguments are supported.

Example:

```
namespace F = torch::nn::functional;
F::hardtanh(x,
F::HardtanhFuncOptions().min_val(-1.0).max_val(1.0).inplace(true));
```

using torch::nn::functional::LeakyReLUFuncOptions = LeakyReLUOptions

Options for `torch::nn::functional::leaky_relu`.

See the documentation for `torch::nn::LeakyReLUOptions` class to learn what arguments are supported.

Example:

```
namespace F = torch::nn::functional;
F::leaky_relu(x,
F::LeakyReLUFuncOptions().negative_slope(0.42).inplace(true));
```

using torch::nn::functional::ReLUFuncOptions = ReLUOptions

Options for `torch::nn::functional::relu`.

See the documentation for `torch::nn::ReLUOptions` class to learn what arguments are supported.

Example:

```
namespace F = torch::nn::functional;
F::relu(x, F::ReLUFuncOptions().inplace(true));
```

using torch::nn::functional::ReLU6FuncOptions = ReLU6Options

Options for `torch::nn::functional::relu6`.

See the documentation for `torch::nn::ReLU6Options` class to learn what arguments are supported.

Example:

```
namespace F = torch::nn::functional;
F::relu6(x, F::ReLU6FuncOptions().inplace(true));
```

using torch::nn::functional::CELUFuncOptions = CELUOptions

Options for `torch::nn::functional::celu`.

See the documentation for `torch::nn::CELUOptions` class to learn what arguments are supported.

Example:

```
namespace F = torch::nn::functional;
F::celu(x, F::CELUFuncOptions().alpha(0.42).inplace(true));
```

using torch::nn::functional::SoftplusFuncOptions = SoftplusOptions

Options for `torch::nn::functional::softplus`.

See the documentation for `torch::nn::SoftplusOptions` class to learn what arguments are supported.

Example:

```
namespace F = torch::nn::functional;
F::softplus(x, F::SoftplusFuncOptions().beta(0.5).threshold(3.0));
```

using torch::nn::functional::SoftshrinkFuncOptions = SoftshrinkOptions

Options for `torch::nn::functional::softshrink`.

See the documentation for `torch::nn::SoftshrinkOptions` class to learn what arguments are supported.

Example:

```
namespace F = torch::nn::functional;
F::softshrink(x, F::SoftshrinkFuncOptions(0.42));
```

using torch::nn::functional::ThresholdFuncOptions = ThresholdOptions

Options for `torch::nn::functional::threshold`.

See the documentation for `torch::nn::ThresholdOptions` class to learn what arguments are supported.

Example:

```
namespace F = torch::nn::functional;
F::threshold(x, F::ThresholdFuncOptions(0.5, 0.5).inplace(true));
```

**Convolution Options:**

using torch::nn::functional::Conv1dFuncOptions = ConvFuncOptions<1>

`ConvFuncOptions` specialized for `torch::nn::functional::conv1d`.

Example:

```
namespace F = torch::nn::functional;
F::conv1d(x, weight, F::Conv1dFuncOptions().stride(1));
```

using torch::nn::functional::Conv2dFuncOptions = ConvFuncOptions<2>

`ConvFuncOptions` specialized for `torch::nn::functional::conv2d`.

Example:

```
namespace F = torch::nn::functional;
F::conv2d(x, weight, F::Conv2dFuncOptions().stride(1));
```

using torch::nn::functional::Conv3dFuncOptions = ConvFuncOptions<3>

`ConvFuncOptions` specialized for `torch::nn::functional::conv3d`.

Example:

```
namespace F = torch::nn::functional;
F::conv3d(x, weight, F::Conv3dFuncOptions().stride(1));
```

using torch::nn::functional::ConvTranspose1dFuncOptions = ConvTransposeFuncOptions<1>

`ConvTransposeFuncOptions` specialized for `torch::nn::functional::conv_transpose1d`.

Example:

```
namespace F = torch::nn::functional;
F::conv_transpose1d(x, weight, F::ConvTranspose1dFuncOptions().stride(1));
```

using torch::nn::functional::ConvTranspose2dFuncOptions = ConvTransposeFuncOptions<2>

`ConvTransposeFuncOptions` specialized for `torch::nn::functional::conv_transpose2d`.

Example:

```
namespace F = torch::nn::functional;
F::conv_transpose2d(x, weight, F::ConvTranspose2dFuncOptions().stride(1));
```

using torch::nn::functional::ConvTranspose3dFuncOptions = ConvTransposeFuncOptions<3>

`ConvTransposeFuncOptions` specialized for `torch::nn::functional::conv_transpose3d`.

Example:

```
namespace F = torch::nn::functional;
F::conv_transpose3d(x, weight, F::ConvTranspose3dFuncOptions().stride(1));
```

**Pooling Options:**

using torch::nn::functional::AvgPool1dFuncOptions = AvgPool1dOptions

Options for `torch::nn::functional::avg_pool1d`.

See the documentation for `torch::nn::AvgPool1dOptions` class to learn what arguments are supported.

Example:

```
namespace F = torch::nn::functional;
F::avg_pool1d(x, F::AvgPool1dFuncOptions(3).stride(2));
```

using torch::nn::functional::AvgPool2dFuncOptions = AvgPool2dOptions

Options for `torch::nn::functional::avg_pool2d`.

See the documentation for `torch::nn::AvgPool2dOptions` class to learn what arguments are supported.

Example:

```
namespace F = torch::nn::functional;
F::avg_pool2d(x, F::AvgPool2dFuncOptions(3).stride(2));
```

using torch::nn::functional::AvgPool3dFuncOptions = AvgPool3dOptions

Options for `torch::nn::functional::avg_pool3d`.

See the documentation for `torch::nn::AvgPool3dOptions` class to learn what arguments are supported.

Example:

```
namespace F = torch::nn::functional;
F::avg_pool3d(x, F::AvgPool3dFuncOptions(3).stride(2));
```

using torch::nn::functional::MaxPool1dFuncOptions = MaxPool1dOptions

Options for `torch::nn::functional::max_pool1d` and `torch::nn::functional::max_pool1d_with_indices`.

Example:

```
namespace F = torch::nn::functional;
F::max_pool1d(x, F::MaxPool1dFuncOptions(3).stride(2));
```

using torch::nn::functional::MaxPool2dFuncOptions = MaxPool2dOptions

Options for `torch::nn::functional::max_pool2d` and `torch::nn::functional::max_pool2d_with_indices`.

Example:

```
namespace F = torch::nn::functional;
F::max_pool2d(x, F::MaxPool2dFuncOptions(3).stride(2));
```

using torch::nn::functional::MaxPool3dFuncOptions = MaxPool3dOptions

Options for `torch::nn::functional::max_pool3d` and `torch::nn::functional::max_pool3d_with_indices`.

Example:

```
namespace F = torch::nn::functional;
F::max_pool3d(x, F::MaxPool3dFuncOptions(3).stride(2));
```

using torch::nn::functional::AdaptiveMaxPool1dFuncOptions = AdaptiveMaxPool1dOptions

Options for `torch::nn::functional::adaptive_max_pool1d` and `torch::nn::functional::adaptive_max_pool1d_with_indices`

Example:

```
namespace F = torch::nn::functional;
F::adaptive_max_pool1d(x, F::AdaptiveMaxPool1dFuncOptions(3));
```

using torch::nn::functional::AdaptiveMaxPool2dFuncOptions = AdaptiveMaxPool2dOptions

Options for `torch::nn::functional::adaptive_max_pool2d` and `torch::nn::functional::adaptive_max_pool2d_with_indices`

Example:

```
namespace F = torch::nn::functional;
F::adaptive_max_pool2d(x, F::AdaptiveMaxPool2dFuncOptions(3));
```

using torch::nn::functional::AdaptiveMaxPool3dFuncOptions = AdaptiveMaxPool3dOptions

Options for `torch::nn::functional::adaptive_max_pool3d` and `torch::nn::functional::adaptive_max_pool3d_with_indices`

Example:

```
namespace F = torch::nn::functional;
F::adaptive_max_pool3d(x, F::AdaptiveMaxPool3dFuncOptions(3));
```

using torch::nn::functional::AdaptiveAvgPool1dFuncOptions = AdaptiveAvgPool1dOptions

Options for `torch::nn::functional::adaptive_avg_pool1d`.

See the documentation for `torch::nn::AdaptiveAvgPool1dOptions` class to learn what arguments are supported.

Example:

```
namespace F = torch::nn::functional;
F::adaptive_avg_pool1d(x, F::AdaptiveAvgPool1dFuncOptions(3));
```

using torch::nn::functional::AdaptiveAvgPool2dFuncOptions = AdaptiveAvgPool2dOptions

Options for `torch::nn::functional::adaptive_avg_pool2d`.

See the documentation for `torch::nn::AdaptiveAvgPool2dOptions` class to learn what arguments are supported.

Example:

```
namespace F = torch::nn::functional;
F::adaptive_avg_pool2d(x, F::AdaptiveAvgPool2dFuncOptions(3));
```

using torch::nn::functional::AdaptiveAvgPool3dFuncOptions = AdaptiveAvgPool3dOptions

Options for `torch::nn::functional::adaptive_avg_pool3d`.

See the documentation for `torch::nn::AdaptiveAvgPool3dOptions` class to learn what arguments are supported.

Example:

```
namespace F = torch::nn::functional;
F::adaptive_avg_pool3d(x, F::AdaptiveAvgPool3dFuncOptions(3));
```

**Other Options:**

using torch::nn::functional::CosineSimilarityFuncOptions = CosineSimilarityOptions

Options for `torch::nn::functional::cosine_similarity`.

See the documentation for `torch::nn::CosineSimilarityOptions` class to learn what arguments are supported.

Example:

```
namespace F = torch::nn::functional;
F::cosine_similarity(input1, input2,
F::CosineSimilarityFuncOptions().dim(1));
```

using torch::nn::functional::PairwiseDistanceFuncOptions = PairwiseDistanceOptions

Options for `torch::nn::functional::pairwise_distance`.

See the documentation for `torch::nn::PairwiseDistanceOptions` class to learn what arguments are supported.

Example:

```
namespace F = torch::nn::functional;
F::pairwise_distance(input1, input2, F::PairwiseDistanceFuncOptions().p(1));
```

using torch::nn::functional::Dropout2dFuncOptions = DropoutFuncOptions

Options for `torch::nn::functional::dropout2d`.

Example:

```
namespace F = torch::nn::functional;
F::dropout2d(input, F::Dropout2dFuncOptions().p(0.5));
```

using torch::nn::functional::Dropout3dFuncOptions = DropoutFuncOptions

Options for `torch::nn::functional::dropout3d`.

Example:

```
namespace F = torch::nn::functional;
F::dropout3d(input, F::Dropout3dFuncOptions().p(0.5));
```

using torch::nn::functional::L1LossFuncOptions = L1LossOptions

Options for `torch::nn::functional::l1_loss`.

See the documentation for `torch::nn::L1LossOptions` class to learn what arguments are supported.

Example:

```
namespace F = torch::nn::functional;
F::l1_loss(input, target, F::L1LossFuncOptions(torch::kNone));
```

using torch::nn::functional::FoldFuncOptions = [FoldOptions](utilities.html#_CPPv4N5torch2nn11FoldOptionsE)

Options for `torch::nn::functional::fold`.

See the documentation for `[torch::nn::FoldOptions](utilities.html#PyTorchstructtorch_1_1nn_1_1_fold_options)` class to learn what arguments are supported.

Example:

```
namespace F = torch::nn::functional;
F::fold(input, F::FoldFuncOptions({3, 2}, {2, 2}));
```

using torch::nn::functional::UnfoldFuncOptions = [UnfoldOptions](utilities.html#_CPPv4N5torch2nn13UnfoldOptionsE)

Options for `torch::nn::functional::unfold`.

See the documentation for `[torch::nn::UnfoldOptions](utilities.html#PyTorchstructtorch_1_1nn_1_1_unfold_options)` class to learn what arguments are supported.

Example:

```
namespace F = torch::nn::functional;
F::unfold(input, F::UnfoldFuncOptions({2, 2}).padding(1).stride(2));
```

using torch::nn::functional::PixelShuffleFuncOptions = [PixelShuffleOptions](utilities.html#_CPPv4N5torch2nn19PixelShuffleOptionsE)

Options for `torch::nn::functional::pixel_shuffle`.

See the documentation for `[torch::nn::PixelShuffleOptions](utilities.html#PyTorchstructtorch_1_1nn_1_1_pixel_shuffle_options)` class to learn what arguments are supported.

Example:

```
namespace F = torch::nn::functional;
F::pixel_shuffle(x, F::PixelShuffleFuncOptions(2));
```

using torch::nn::functional::PixelUnshuffleFuncOptions = [PixelUnshuffleOptions](utilities.html#_CPPv4N5torch2nn21PixelUnshuffleOptionsE)

Options for `torch::nn::functional::pixel_unshuffle`.

See the documentation for `[torch::nn::PixelUnshuffleOptions](utilities.html#PyTorchstructtorch_1_1nn_1_1_pixel_unshuffle_options)` class to learn what arguments are supported.

Example:

```
namespace F = torch::nn::functional;
F::pixel_unshuffle(x, F::PixelUnshuffleFuncOptions(2));
```