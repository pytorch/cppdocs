# Convolution Layers

Convolutional layers apply learnable filters to input data, extracting local features
through sliding window operations. They are fundamental to CNNs for image, audio, and
sequential data processing.

- **Conv1d/2d/3d**: Standard convolution for 1D sequences, 2D images, or 3D volumes
- **ConvTranspose1d/2d/3d**: Transposed convolution (deconvolution) for upsampling

**Key parameters:**

- `in_channels`: Number of input channels
- `out_channels`: Number of output channels (number of filters)
- `kernel_size`: Size of the convolving kernel
- `stride`: Stride of the convolution (default: 1)
- `padding`: Zero-padding added to input (default: 0)
- `dilation`: Spacing between kernel elements (default: 1)
- `groups`: Number of blocked connections (default: 1, use `in_channels` for depthwise)

## Conv1d

Applies 1D convolution over an input signal composed of several input planes.

class Conv1d : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<Conv1dImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `Conv1dImpl`.

See the documentation for `Conv1dImpl` class to learn what methods it provides, and examples of how to use `Conv1d` with `torch::nn::Conv1dOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = Conv1dImpl

class Conv1dImpl : public torch::nn::ConvNdImpl<1, Conv1dImpl>

Applies convolution over a 1-D input.

See [https://pytorch.org/docs/main/nn.html#torch.nn.Conv1d](https://pytorch.org/docs/main/nn.html#torch.nn.Conv1d) to learn about the exact behavior of this module.

See the documentation for `torch::nn::Conv1dOptions` class to learn what constructor arguments are supported for this module.

Example:

```
Conv1d model(Conv1dOptions(3, 2, 3).stride(1).bias(false));
```

Public Functions

inline Conv1dImpl(int64_t input_channels, int64_t output_channels, ExpandingArray<1> kernel_size)

explicit Conv1dImpl(Conv1dOptions options_)

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input)

## Conv2d

Applies 2D convolution over an input image. The most commonly used layer for
image processing tasks.

class Conv2d : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<Conv2dImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `Conv2dImpl`.

See the documentation for `Conv2dImpl` class to learn what methods it provides, and examples of how to use `Conv2d` with `torch::nn::Conv2dOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = Conv2dImpl

class Conv2dImpl : public torch::nn::ConvNdImpl<2, Conv2dImpl>

Applies convolution over a 2-D input.

See [https://pytorch.org/docs/main/nn.html#torch.nn.Conv2d](https://pytorch.org/docs/main/nn.html#torch.nn.Conv2d) to learn about the exact behavior of this module.

See the documentation for `torch::nn::Conv2dOptions` class to learn what constructor arguments are supported for this module.

Example:

```
Conv2d model(Conv2dOptions(3, 2, 3).stride(1).bias(false));
```

Public Functions

inline Conv2dImpl(int64_t input_channels, int64_t output_channels, ExpandingArray<2> kernel_size)

explicit Conv2dImpl(Conv2dOptions options_)

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input)

**Example:**

```
// Create Conv2d: 3 input channels, 64 output channels, 3x3 kernel
auto conv = torch::nn::Conv2d(
 torch::nn::Conv2dOptions(3, 64, 3)
 .stride(1)
 .padding(1)
 .bias(true));

auto output = conv->forward(input); // input: [N, 3, H, W]
```

## Conv3d

Applies 3D convolution over an input volume (e.g., video frames or 3D medical images).

class Conv3d : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<Conv3dImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `Conv3dImpl`.

See the documentation for `Conv3dImpl` class to learn what methods it provides, and examples of how to use `Conv3d` with `torch::nn::Conv3dOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = Conv3dImpl

class Conv3dImpl : public torch::nn::ConvNdImpl<3, Conv3dImpl>

Applies convolution over a 3-D input.

See [https://pytorch.org/docs/main/nn.html#torch.nn.Conv3d](https://pytorch.org/docs/main/nn.html#torch.nn.Conv3d) to learn about the exact behavior of this module.

See the documentation for `torch::nn::Conv3dOptions` class to learn what constructor arguments are supported for this module.

Example:

```
Conv3d model(Conv3dOptions(3, 2, 3).stride(1).bias(false));
```

Public Functions

inline Conv3dImpl(int64_t input_channels, int64_t output_channels, ExpandingArray<3> kernel_size)

explicit Conv3dImpl(Conv3dOptions options_)

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input)

## ConvTranspose1d

Applies 1D transposed convolution (fractionally-strided convolution) for upsampling.

class ConvTranspose1d : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<ConvTranspose1dImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `ConvTranspose1dImpl`.

See the documentation for `ConvTranspose1dImpl` class to learn what methods it provides, and examples of how to use `ConvTranspose1d` with `torch::nn::ConvTranspose1dOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = ConvTranspose1dImpl

class ConvTranspose1dImpl : public torch::nn::ConvTransposeNdImpl<1, ConvTranspose1dImpl>

Applies the ConvTranspose1d function.

See [https://pytorch.org/docs/main/nn.html#torch.nn.ConvTranspose1d](https://pytorch.org/docs/main/nn.html#torch.nn.ConvTranspose1d) to learn about the exact behavior of this module.

See the documentation for `torch::nn::ConvTranspose1dOptions` class to learn what constructor arguments are supported for this module.

Example:

```
ConvTranspose1d model(ConvTranspose1dOptions(3, 2,
3).stride(1).bias(false));
```

Public Functions

inline ConvTranspose1dImpl(int64_t input_channels, int64_t output_channels, ExpandingArray<1> kernel_size)

explicit ConvTranspose1dImpl(ConvTranspose1dOptions options_)

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const std::optional<at::IntArrayRef> &output_size = std::nullopt)

Friends

*friend struct* torch::nn::AnyModuleHolder

## ConvTranspose2d

Applies 2D transposed convolution for upsampling. Commonly used in decoder
networks and generative models.

class ConvTranspose2d : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<ConvTranspose2dImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `ConvTranspose2dImpl`.

See the documentation for `ConvTranspose2dImpl` class to learn what methods it provides, and examples of how to use `ConvTranspose2d` with `torch::nn::ConvTranspose2dOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = ConvTranspose2dImpl

class ConvTranspose2dImpl : public torch::nn::ConvTransposeNdImpl<2, ConvTranspose2dImpl>

Applies the ConvTranspose2d function.

See [https://pytorch.org/docs/main/nn.html#torch.nn.ConvTranspose2d](https://pytorch.org/docs/main/nn.html#torch.nn.ConvTranspose2d) to learn about the exact behavior of this module.

See the documentation for `torch::nn::ConvTranspose2dOptions` class to learn what constructor arguments are supported for this module.

Example:

```
ConvTranspose2d model(ConvTranspose2dOptions(3, 2,
3).stride(1).bias(false));
```

Public Functions

inline ConvTranspose2dImpl(int64_t input_channels, int64_t output_channels, ExpandingArray<2> kernel_size)

explicit ConvTranspose2dImpl(ConvTranspose2dOptions options_)

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const std::optional<at::IntArrayRef> &output_size = std::nullopt)

Friends

*friend struct* torch::nn::AnyModuleHolder

**Example:**

```
// Create ConvTranspose2d for upsampling
auto conv_transpose = torch::nn::ConvTranspose2d(
 torch::nn::ConvTranspose2dOptions(64, 32, 4)
 .stride(2)
 .padding(1));
```

## ConvTranspose3d

Applies 3D transposed convolution for upsampling volumetric data.

class ConvTranspose3d : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<ConvTranspose3dImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `ConvTranspose3dImpl`.

See the documentation for `ConvTranspose3dImpl` class to learn what methods it provides, and examples of how to use `ConvTranspose3d` with `torch::nn::ConvTranspose3dOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = ConvTranspose3dImpl

class ConvTranspose3dImpl : public torch::nn::ConvTransposeNdImpl<3, ConvTranspose3dImpl>

Applies the ConvTranspose3d function.

See [https://pytorch.org/docs/main/nn.html#torch.nn.ConvTranspose3d](https://pytorch.org/docs/main/nn.html#torch.nn.ConvTranspose3d) to learn about the exact behavior of this module.

See the documentation for `torch::nn::ConvTranspose3dOptions` class to learn what constructor arguments are supported for this module.

Example:

```
ConvTranspose3d model(ConvTranspose3dOptions(2, 2,
2).stride(1).bias(false));
```

Public Functions

inline ConvTranspose3dImpl(int64_t input_channels, int64_t output_channels, ExpandingArray<3> kernel_size)

explicit ConvTranspose3dImpl(ConvTranspose3dOptions options_)

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const std::optional<at::IntArrayRef> &output_size = std::nullopt)

Friends

*friend struct* torch::nn::AnyModuleHolder