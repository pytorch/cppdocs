# Recurrent Layers

Recurrent layers process sequential data by maintaining hidden state across time steps.
They are essential for tasks involving sequences: language modeling, speech recognition,
time series prediction, and more.

- **RNN**: Basic recurrent layer (simple but prone to vanishing gradients)
- **LSTM**: Long Short-Term Memory (gated architecture, handles long-range dependencies)
- **GRU**: Gated Recurrent Unit (simpler than LSTM, often similar performance)
- **Cell variants**: Single-step versions for custom loop implementations

**Key parameters:**

- `input_size`: Number of features in input
- `hidden_size`: Number of features in hidden state
- `num_layers`: Number of stacked recurrent layers
- `batch_first`: If true, input shape is `[batch, seq, features]`
- `bidirectional`: Process sequence in both directions

## RNN

class RNN : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<RNNImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `RNNImpl`.

See the documentation for `RNNImpl` class to learn what methods it provides, and examples of how to use `RNN` with `torch::nn::RNNOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = RNNImpl

class RNNImpl : public torch::nn::detail::RNNImplBase<RNNImpl>

A multi-layer Elman RNN module with [Tanh](activation.html#PyTorchclasstorch_1_1nn_1_1_tanh) or [ReLU](activation.html#PyTorchclasstorch_1_1nn_1_1_re_l_u) activation.

See [https://pytorch.org/docs/main/generated/torch.nn.RNN.html](https://pytorch.org/docs/main/generated/torch.nn.RNN.html) to learn about the exact behavior of this module.

See the documentation for `torch::nn::RNNOptions` class to learn what constructor arguments are supported for this module.

Example:

```
RNN model(RNNOptions(128,
64).num_layers(3).dropout(0.2).nonlinearity(torch::kTanh));
```

Public Functions

inline RNNImpl(int64_t input_size, int64_t hidden_size)

explicit RNNImpl(const RNNOptions &options_)

std::tuple<[Tensor](../aten/tensor.html#_CPPv46Tensorv), [Tensor](../aten/tensor.html#_CPPv46Tensorv)> forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, [Tensor](../aten/tensor.html#_CPPv46Tensorv) hx = {})

std::tuple<torch::nn::utils::rnn::[PackedSequence](utilities.html#_CPPv4N5torch2nn5utils3rnn14PackedSequenceE), [Tensor](../aten/tensor.html#_CPPv46Tensorv)> forward_with_packed_input(const torch::nn::utils::rnn::[PackedSequence](utilities.html#_CPPv4N5torch2nn5utils3rnn14PackedSequenceE) &packed_input, [Tensor](../aten/tensor.html#_CPPv46Tensorv) hx = {})

Public Members

RNNOptions options

Friends

*friend struct* torch::nn::AnyModuleHolder

**Example:**

```
auto rnn = torch::nn::RNN(
 torch::nn::RNNOptions(128, 256) // input_size, hidden_size
 .num_layers(2)
 .batch_first(true)
 .bidirectional(false));

auto input = torch::randn({32, 10, 128}); // [batch, seq_len, input_size]
auto [output, hidden] = rnn->forward(input);
```

## LSTM

class LSTM : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<LSTMImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `LSTMImpl`.

See the documentation for `LSTMImpl` class to learn what methods it provides, and examples of how to use `LSTM` with `torch::nn::LSTMOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = LSTMImpl

class LSTMImpl : public torch::nn::detail::RNNImplBase<LSTMImpl>

A multi-layer long-short-term-memory (LSTM) module.

See [https://pytorch.org/docs/main/generated/torch.nn.LSTM.html](https://pytorch.org/docs/main/generated/torch.nn.LSTM.html) to learn about the exact behavior of this module.

See the documentation for `torch::nn::LSTMOptions` class to learn what constructor arguments are supported for this module.

Example:

```
LSTM model(LSTMOptions(2,
4).num_layers(3).batch_first(false).bidirectional(true));
```

Public Functions

inline LSTMImpl(int64_t input_size, int64_t hidden_size)

explicit LSTMImpl(const LSTMOptions &options_)

std::tuple<[Tensor](../aten/tensor.html#_CPPv46Tensorv), std::tuple<[Tensor](../aten/tensor.html#_CPPv46Tensorv), [Tensor](../aten/tensor.html#_CPPv46Tensorv)>> forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, std::optional<std::tuple<[Tensor](../aten/tensor.html#_CPPv46Tensorv), [Tensor](../aten/tensor.html#_CPPv46Tensorv)>> hx_opt = {})

std::tuple<torch::nn::utils::rnn::[PackedSequence](utilities.html#_CPPv4N5torch2nn5utils3rnn14PackedSequenceE), std::tuple<[Tensor](../aten/tensor.html#_CPPv46Tensorv), [Tensor](../aten/tensor.html#_CPPv46Tensorv)>> forward_with_packed_input(const torch::nn::utils::rnn::[PackedSequence](utilities.html#_CPPv4N5torch2nn5utils3rnn14PackedSequenceE) &packed_input, std::optional<std::tuple<[Tensor](../aten/tensor.html#_CPPv46Tensorv), [Tensor](../aten/tensor.html#_CPPv46Tensorv)>> hx_opt = {})

Public Members

LSTMOptions options

Friends

*friend struct* torch::nn::AnyModuleHolder

**Example:**

```
auto lstm = torch::nn::LSTM(
 torch::nn::LSTMOptions(128, 256)
 .num_layers(2)
 .batch_first(true)
 .dropout(0.1)
 .bidirectional(true));

auto input = torch::randn({32, 10, 128});
auto [output, state] = lstm->forward(input);
auto [h_n, c_n] = state; // hidden state, cell state
```

## GRU

class GRU : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<GRUImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `GRUImpl`.

See the documentation for `GRUImpl` class to learn what methods it provides, and examples of how to use `GRU` with `torch::nn::GRUOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = GRUImpl

class GRUImpl : public torch::nn::detail::RNNImplBase<GRUImpl>

A multi-layer gated recurrent unit (GRU) module.

See [https://pytorch.org/docs/main/generated/torch.nn.GRU.html](https://pytorch.org/docs/main/generated/torch.nn.GRU.html) to learn about the exact behavior of this module.

See the documentation for `torch::nn::GRUOptions` class to learn what constructor arguments are supported for this module.

Example:

```
GRU model(GRUOptions(2,
4).num_layers(3).batch_first(false).bidirectional(true));
```

Public Functions

inline GRUImpl(int64_t input_size, int64_t hidden_size)

explicit GRUImpl(const GRUOptions &options_)

std::tuple<[Tensor](../aten/tensor.html#_CPPv46Tensorv), [Tensor](../aten/tensor.html#_CPPv46Tensorv)> forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, [Tensor](../aten/tensor.html#_CPPv46Tensorv) hx = {})

std::tuple<torch::nn::utils::rnn::[PackedSequence](utilities.html#_CPPv4N5torch2nn5utils3rnn14PackedSequenceE), [Tensor](../aten/tensor.html#_CPPv46Tensorv)> forward_with_packed_input(const torch::nn::utils::rnn::[PackedSequence](utilities.html#_CPPv4N5torch2nn5utils3rnn14PackedSequenceE) &packed_input, [Tensor](../aten/tensor.html#_CPPv46Tensorv) hx = {})

Public Members

GRUOptions options

Friends

*friend struct* torch::nn::AnyModuleHolder

## RNNCell

class RNNCell : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<RNNCellImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `RNNCellImpl`.

See the documentation for `RNNCellImpl` class to learn what methods it provides, and examples of how to use `RNNCell` with `torch::nn::RNNCellOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = RNNCellImpl

class RNNCellImpl : public torch::nn::detail::RNNCellImplBase<RNNCellImpl>

An Elman RNN cell with tanh or [ReLU](activation.html#PyTorchclasstorch_1_1nn_1_1_re_l_u) non-linearity.

See [https://pytorch.org/docs/main/nn.html#torch.nn.RNNCell](https://pytorch.org/docs/main/nn.html#torch.nn.RNNCell) to learn about the exact behavior of this module.

See the documentation for `torch::nn::RNNCellOptions` class to learn what constructor arguments are supported for this module.

Example:

```
RNNCell model(RNNCellOptions(20,
10).bias(false).nonlinearity(torch::kReLU));
```

Public Functions

inline RNNCellImpl(int64_t input_size, int64_t hidden_size)

explicit RNNCellImpl(const RNNCellOptions &options_)

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &hx = {})

Public Members

RNNCellOptions options

Friends

*friend struct* torch::nn::AnyModuleHolder

## LSTMCell

class LSTMCell : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<LSTMCellImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `LSTMCellImpl`.

See the documentation for `LSTMCellImpl` class to learn what methods it provides, and examples of how to use `LSTMCell` with `torch::nn::LSTMCellOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = LSTMCellImpl

class LSTMCellImpl : public torch::nn::detail::RNNCellImplBase<LSTMCellImpl>

A long short-term memory (LSTM) cell.

See [https://pytorch.org/docs/main/nn.html#torch.nn.LSTMCell](https://pytorch.org/docs/main/nn.html#torch.nn.LSTMCell) to learn about the exact behavior of this module.

See the documentation for `torch::nn::LSTMCellOptions` class to learn what constructor arguments are supported for this module.

Example:

```
LSTMCell model(LSTMCellOptions(20, 10).bias(false));
```

Public Functions

inline LSTMCellImpl(int64_t input_size, int64_t hidden_size)

explicit LSTMCellImpl(const LSTMCellOptions &options_)

std::tuple<[Tensor](../aten/tensor.html#_CPPv46Tensorv), [Tensor](../aten/tensor.html#_CPPv46Tensorv)> forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, std::optional<std::tuple<[Tensor](../aten/tensor.html#_CPPv46Tensorv), [Tensor](../aten/tensor.html#_CPPv46Tensorv)>> hx_opt = {})

Public Members

LSTMCellOptions options

Friends

*friend struct* torch::nn::AnyModuleHolder

## GRUCell

class GRUCell : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<GRUCellImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `GRUCellImpl`.

See the documentation for `GRUCellImpl` class to learn what methods it provides, and examples of how to use `GRUCell` with `torch::nn::GRUCellOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = GRUCellImpl

class GRUCellImpl : public torch::nn::detail::RNNCellImplBase<GRUCellImpl>

A gated recurrent unit (GRU) cell.

See [https://pytorch.org/docs/main/nn.html#torch.nn.GRUCell](https://pytorch.org/docs/main/nn.html#torch.nn.GRUCell) to learn about the exact behavior of this module.

See the documentation for `torch::nn::GRUCellOptions` class to learn what constructor arguments are supported for this module.

Example:

```
GRUCell model(GRUCellOptions(20, 10).bias(false));
```

Public Functions

inline GRUCellImpl(int64_t input_size, int64_t hidden_size)

explicit GRUCellImpl(const GRUCellOptions &options_)

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &hx = {})

Public Members

GRUCellOptions options

Friends

*friend struct* torch::nn::AnyModuleHolder