# Transformer Layers

Transformer layers use self-attention mechanisms to process sequences in parallel,
enabling efficient training on long sequences. They are the foundation of modern
NLP models (BERT, GPT) and increasingly used in vision and other domains.

- **Transformer**: Complete encoder-decoder architecture
- **TransformerEncoder/Decoder**: Standalone encoder or decoder stacks
- **TransformerEncoderLayer/DecoderLayer**: Individual transformer blocks
- **MultiheadAttention**: Core attention mechanism used throughout

**Key parameters:**

- `d_model`: Dimension of the model (embedding dimension)
- `nhead`: Number of attention heads
- `num_encoder_layers/num_decoder_layers`: Number of stacked layers
- `dim_feedforward`: Dimension of feedforward network
- `dropout`: Dropout rate for regularization

## Transformer

Complete encoder-decoder transformer architecture.

class Transformer : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<TransformerImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `TransformerImpl`.

See the documentation for `TransformerImpl` class to learn what methods it provides, and examples of how to use `Transformer` with `torch::nn::TransformerOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = TransformerImpl

class TransformerImpl : public torch::nn::[Cloneable](utilities.html#_CPPv4I0EN5torch2nn9CloneableE)<TransformerImpl>

A transformer model.

User is able to modify the attributes as needed. The architecture is based on the paper "Attention Is All You Need". Ashish Vaswani, Noam Shazeer, Niki Parmar, Jakob Uszkoreit, Llion Jones, Aidan N Gomez, Lukasz Kaiser, and Illia Polosukhin. 2017. Attention is all you need. In Advances in Neural Information Processing Systems, pages 6000-6010.

See [https://pytorch.org/docs/stable/generated/torch.nn.Transformer.html](https://pytorch.org/docs/stable/generated/torch.nn.Transformer.html) to learn about the exact behavior of this transformer model

See the documentation for `torch::nn::Transformer` class to learn what constructor arguments are supported for this encoder layer model

Example:

```
Transformer trans(TransformerOptions(512, 8));
```

Public Functions

explicit TransformerImpl(TransformerOptions options_)

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &src, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &tgt, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &src_mask = {}, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &tgt_mask = {}, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &memory_mask = {}, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &src_key_padding_mask = {}, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &tgt_key_padding_mask = {}, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &memory_key_padding_mask = {})

forward function for Transformer [Module](index.html#PyTorchclasstorch_1_1nn_1_1_module) Args: src: the sequence to the encoder (required).

tgt: the sequence to the decoder (required). src_mask: the additive mask for the src sequence (optional). tgt_mask: the additive mask for the tgt sequence (optional). memory_mask: the additive mask for the encoder output (optional). src_key_padding_mask: the ByteTensor mask for src keys per batch (optional). tgt_key_padding_mask: the ByteTensor mask for tgt keys per batch (optional). memory_key_padding_mask: the ByteTensor mask for memory keys per batch (optional).

Shape: src: `(S, N, E)` tgt: `(T, N, E)` src_mask: `(S, S)` tgt_mask: `(T, T)` memory_mask: `(T, S)` src_key_padding_mask: `(N, S)` tgt_key_padding_mask: `(N, T)` memory_key_padding_mask: `(N, S)`

Note: [src/tgt/memory]_mask ensures that position i is allowed to attend the unmasked positions. If a ByteTensor is provided, the non-zero positions are not allowed to attend while the zero positions will be unchanged. If a BoolTensor is provided, positions with `True` are not allowed to attend while `False` values will be unchanged. If a FloatTensor is provided, it will be added to the attention weight.

[src/tgt/memory]_key_padding_mask provides specified elements in the key to be ignored by the attention. If a ByteTensor is provided, the non-zero positions will be ignored while the zero positions will be unchanged. If a BoolTensor is provided, the positions with the value of `True` will be ignored while the position with the value of `False` will be unchanged.

output: `(T, N, E)`

Note: Due to the multi-head attention architecture in the transformer model, the output sequence length of a transformer is same as the input sequence (i.e. target) length of the decode.

where S is the source sequence length, T is the target sequence length, N is the batch size, E is the feature number.

virtual void reset() override

`reset()` must perform initialization of all members with reference semantics, most importantly parameters, buffers and submodules.

void reset_parameters()

Public Members

TransformerOptions options

options with which this `Transformer` was constructed

[AnyModule](utilities.html#_CPPv4N5torch2nn9AnyModuleE) encoder

encoder module

[AnyModule](utilities.html#_CPPv4N5torch2nn9AnyModuleE) decoder

decoder module

Public Static Functions

static [Tensor](../aten/tensor.html#_CPPv46Tensorv) generate_square_subsequent_mask(int64_t sz)

Generate a square mask for the sequence.

The masked positions are filled with `-inf` in float type. Unmasked positions are filled with `0.0` in float type. Note:

1. This function will always return a CPU tensor.
2. This function requires the platform support IEEE754, since `-inf` is guaranteed to be valid only when IEEE754 is supported. If the platform doesn't support IEEE754, this function will fill the mask with the smallest float number instead of `-inf`, a one time warning will pop up as well.

Friends

*friend struct* torch::nn::AnyModuleHolder

**Example:**

```
auto transformer = torch::nn::Transformer(
 torch::nn::TransformerOptions()
 .d_model(512)
 .nhead(8)
 .num_encoder_layers(6)
 .num_decoder_layers(6)
 .dim_feedforward(2048)
 .dropout(0.1));
```

## TransformerEncoder

Stack of encoder layers for processing source sequences.

class TransformerEncoder : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<TransformerEncoderImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `TransformerEncoderImpl`.

See the documentation for `TransformerEncoderImpl` class to learn what methods it provides, and examples of how to use `TransformerEncoder` with `torch::nn::TransformerEncoderOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = TransformerEncoderImpl

class TransformerEncoderImpl : public torch::nn::[Cloneable](utilities.html#_CPPv4I0EN5torch2nn9CloneableE)<TransformerEncoderImpl>

TransformerEncoder module.

See [https://pytorch.org/docs/main/generated/torch.nn.TransformerEncoder.html](https://pytorch.org/docs/main/generated/torch.nn.TransformerEncoder.html) to learn abouut the exact behavior of this encoder layer module.

See the documentation for `torch::nn::TransformerEncoder` class to learn what constructor arguments are supported for this encoder module.

Example:

```
TransformerEncoderLayer encoderLayer(TransformerEncoderLayerOptions(512,
8).dropout(0.1)); TransformerEncoder
encoder(TransformerEncoderOptions(encoderLayer,
6).norm(LayerNorm(LayerNormOptions({2}))));
```

Public Functions

inline TransformerEncoderImpl(TransformerEncoderLayer encoder_layer, int64_t num_layers)

explicit TransformerEncoderImpl(TransformerEncoderOptions options_)

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &src, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &src_mask = {}, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &src_key_padding_mask = {})

virtual void reset() override

`reset()` must perform initialization of all members with reference semantics, most importantly parameters, buffers and submodules.

void reset_parameters()

Public Members

TransformerEncoderOptions options

options with which this `TransformerEncoder` was constructed

[ModuleList](containers.html#_CPPv4N5torch2nn10ModuleListE) layers = nullptr

module list that contains all the encoder layers

[AnyModule](utilities.html#_CPPv4N5torch2nn9AnyModuleE) norm

optional normalization module

Friends

*friend struct* torch::nn::AnyModuleHolder

## TransformerDecoder

Stack of decoder layers for generating target sequences.

class TransformerDecoder : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<TransformerDecoderImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `TransformerDecoderImpl`.

See the documentation for `TransformerDecoderImpl` class to learn what methods it provides, and examples of how to use `TransformerDecoder` with `torch::nn::TransformerDecoderOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = TransformerDecoderImpl

class TransformerDecoderImpl : public torch::nn::[Cloneable](utilities.html#_CPPv4I0EN5torch2nn9CloneableE)<TransformerDecoderImpl>

TransformerDecoder is a stack of N decoder layers.

See [https://pytorch.org/docs/main/generated/torch.nn.TransformerDecoder.html](https://pytorch.org/docs/main/generated/torch.nn.TransformerDecoder.html) to learn abouut the exact behavior of this decoder module

See the documentation for `torch::nn::TransformerDecoderOptions` class to learn what constructor arguments are supported for this decoder module

Example:

```
TransformerDecoderLayer decoder_layer(TransformerDecoderLayerOptions(512,
8).dropout(0.1)); TransformerDecoder
transformer_decoder(TransformerDecoderOptions(decoder_layer,
6).norm(LayerNorm(LayerNormOptions({2})))); const auto memory =
torch::rand({10, 32, 512}); const auto tgt = torch::rand({20, 32, 512});
auto out = transformer_decoder(tgt, memory);
```

Public Functions

inline TransformerDecoderImpl(TransformerDecoderLayer decoder_layer, int64_t num_layers)

explicit TransformerDecoderImpl(TransformerDecoderOptions options_)

virtual void reset() override

`reset()` must perform initialization of all members with reference semantics, most importantly parameters, buffers and submodules.

void reset_parameters()

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &tgt, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &memory, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &tgt_mask = {}, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &memory_mask = {}, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &tgt_key_padding_mask = {}, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &memory_key_padding_mask = {})

Pass the inputs (and mask) through the decoder layer in turn.

Args: tgt: the sequence to the decoder layer (required). memory: the sequence from the last layer of the encoder (required). tgt_mask: the mask for the tgt sequence (optional). memory_mask: the mask for the memory sequence (optional). tgt_key_padding_mask: the mask for the tgt keys per batch (optional). memory_key_padding_mask: the mask for the memory keys per batch (optional).

Public Members

TransformerDecoderOptions options

The options used to configure this module.

[ModuleList](containers.html#_CPPv4N5torch2nn10ModuleListE) layers = {nullptr}

Cloned layers of decoder layers.

[AnyModule](utilities.html#_CPPv4N5torch2nn9AnyModuleE) norm

optional layer normalization module

Friends

*friend struct* torch::nn::AnyModuleHolder

## TransformerEncoderLayer

Single encoder layer with self-attention and feedforward network.

class TransformerEncoderLayerImpl : public torch::nn::[Cloneable](utilities.html#_CPPv4I0EN5torch2nn9CloneableE)<TransformerEncoderLayerImpl>

TransformerEncoderLayer module.

See [https://pytorch.org/docs/main/generated/torch.nn.TransformerEncoderLayer.html](https://pytorch.org/docs/main/generated/torch.nn.TransformerEncoderLayer.html) to learn abouut the exact behavior of this encoder layer model

See the documentation for `torch::nn::TransformerEncoderLayer` class to learn what constructor arguments are supported for this encoder layer model

Example:

```
TransformerEncoderLayer encoderLayer(TransformerEncoderLayerOptions(512,
8).dropout(0.1));
```

Public Functions

inline TransformerEncoderLayerImpl(int64_t d_model, int64_t nhead)

explicit TransformerEncoderLayerImpl(TransformerEncoderLayerOptions options_)

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &src, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &src_mask = {}, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &src_key_padding_mask = {})

virtual void reset() override

`reset()` must perform initialization of all members with reference semantics, most importantly parameters, buffers and submodules.

void reset_parameters()

Public Members

TransformerEncoderLayerOptions options

options with which this `TransformerEncoderLayer` was constructed

MultiheadAttention self_attn = nullptr

self attention

[Linear](linear.html#_CPPv4N5torch2nn6LinearE) linear1 = nullptr

feedforward first linear layer

[Dropout](dropout.html#_CPPv4N5torch2nn7DropoutE) dropout = nullptr

feedforward dropout layer

[Linear](linear.html#_CPPv4N5torch2nn6LinearE) linear2 = nullptr

feedforward second linear layer

[LayerNorm](normalization.html#_CPPv4N5torch2nn9LayerNormE) norm1 = nullptr

pre feedforward, normalization layer

[LayerNorm](normalization.html#_CPPv4N5torch2nn9LayerNormE) norm2 = nullptr

post feedfastward, normalization layer

[Dropout](dropout.html#_CPPv4N5torch2nn7DropoutE) dropout1 = nullptr

pre feedfastward, dropout layer

[Dropout](dropout.html#_CPPv4N5torch2nn7DropoutE) dropout2 = nullptr

post feedfastward, dropout layer

Friends

*friend struct* torch::nn::AnyModuleHolder

## TransformerDecoderLayer

Single decoder layer with self-attention, cross-attention, and feedforward network.

Warning

doxygenclass: Cannot find class "TransformerDecoderLayerImpl" in doxygen xml output for project "PyTorch" from directory: ../build/xml

## MultiheadAttention

Scaled dot-product attention with multiple parallel heads.

class MultiheadAttention : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<MultiheadAttentionImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `MultiheadAttentionImpl`.

See the documentation for `MultiheadAttentionImpl` class to learn what methods it provides, and examples of how to use `MultiheadAttention` with `torch::nn::MultiheadAttentionOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Types

using Impl = MultiheadAttentionImpl

class MultiheadAttentionImpl : public torch::nn::[Cloneable](utilities.html#_CPPv4I0EN5torch2nn9CloneableE)<MultiheadAttentionImpl>

Applies the MultiheadAttention function element-wise.

See [https://pytorch.org/docs/main/nn.html#torch.nn.MultiheadAttention](https://pytorch.org/docs/main/nn.html#torch.nn.MultiheadAttention) to learn about the exact behavior of this module.

See the documentation for `torch::nn::MultiheadAttentionOptions` class to learn what constructor arguments are supported for this module.

Example:

```
MultiheadAttention model(MultiheadAttentionOptions(20, 10).bias(false));
```

Public Functions

inline MultiheadAttentionImpl(int64_t embed_dim, int64_t num_heads)

explicit MultiheadAttentionImpl(const MultiheadAttentionOptions &options_)

std::tuple<[Tensor](../aten/tensor.html#_CPPv46Tensorv), [Tensor](../aten/tensor.html#_CPPv46Tensorv)> forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &query, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &key, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &value, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &key_padding_mask = {}, bool need_weights = true, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &attn_mask = {}, bool average_attn_weights = true)

virtual void reset() override

`reset()` must perform initialization of all members with reference semantics, most importantly parameters, buffers and submodules.

void _reset_parameters()

Public Members

MultiheadAttentionOptions options

The options with which this `[Module](index.html#PyTorchclasstorch_1_1nn_1_1_module)` was constructed.

bool _qkv_same_embed_dim = {}

[Tensor](../aten/tensor.html#_CPPv46Tensorv) in_proj_weight

[Tensor](../aten/tensor.html#_CPPv46Tensorv) in_proj_bias

[Tensor](../aten/tensor.html#_CPPv46Tensorv) bias_k

[Tensor](../aten/tensor.html#_CPPv46Tensorv) bias_v

[Linear](linear.html#_CPPv4N5torch2nn6LinearE) out_proj = nullptr

[Tensor](../aten/tensor.html#_CPPv46Tensorv) q_proj_weight

[Tensor](../aten/tensor.html#_CPPv46Tensorv) k_proj_weight

[Tensor](../aten/tensor.html#_CPPv46Tensorv) v_proj_weight

int64_t head_dim = {}

Friends

*friend struct* torch::nn::AnyModuleHolder