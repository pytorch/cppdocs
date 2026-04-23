# Embedding Layers

Embedding layers map discrete tokens (words, categories, IDs) to dense vector
representations. They are the foundation of NLP models and recommendation systems.

- **Embedding**: Standard lookup table that maps indices to dense vectors
- **EmbeddingBag**: Computes sums or means of embeddings (efficient for sparse features)

**Key parameters:**

- `num_embeddings`: Size of the vocabulary (number of unique tokens)
- `embedding_dim`: Dimension of each embedding vector
- `padding_idx`: Index that outputs zeros (useful for padding tokens)

## Embedding

class Embedding : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<EmbeddingImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `EmbeddingImpl`.

See the documentation for `EmbeddingImpl` class to learn what methods it provides, and examples of how to use `Embedding` with `torch::nn::EmbeddingOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Static Functions

static inline Embedding from_pretrained(const torch::Tensor &embeddings, const EmbeddingFromPretrainedOptions &options = {})

See the documentation for `torch::nn::EmbeddingFromPretrainedOptions` class to learn what optional arguments are supported for this function.

class EmbeddingImpl : public torch::nn::[Cloneable](utilities.html#_CPPv4I0EN5torch2nn9CloneableE)<EmbeddingImpl>

Performs a lookup in a fixed size embedding table.

See [https://pytorch.org/docs/main/nn.html#torch.nn.Embedding](https://pytorch.org/docs/main/nn.html#torch.nn.Embedding) to learn about the exact behavior of this module.

See the documentation for `torch::nn::EmbeddingOptions` class to learn what constructor arguments are supported for this module.

Example:

```
Embedding model(EmbeddingOptions(10,
2).padding_idx(3).max_norm(2).norm_type(2.5).scale_grad_by_freq(true).sparse(true));
```

Public Functions

inline EmbeddingImpl(int64_t num_embeddings, int64_t embedding_dim)

explicit EmbeddingImpl(EmbeddingOptions options_)

virtual void reset() override

`reset()` must perform initialization of all members with reference semantics, most importantly parameters, buffers and submodules.

void reset_parameters()

virtual void pretty_print(std::ostream &stream) const override

Pretty prints the `Embedding` module into the given `stream`.

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &indices)

Performs a lookup on the embedding table stored in `weight` using the `indices` supplied and returns the result.

Public Members

EmbeddingOptions options

The `Options` used to configure this `Embedding` module.

Changes to `EmbeddingOptions` *after construction* have no effect.

[Tensor](../aten/tensor.html#_CPPv46Tensorv) weight

The embedding table.

**Example:**

```
auto embedding = torch::nn::Embedding(
 torch::nn::EmbeddingOptions(10000, 256) // num_embeddings, embedding_dim
 .padding_idx(0));

auto indices = torch::tensor({1, 2, 3, 4});
auto embedded = embedding->forward(indices); // [4, 256]
```

## EmbeddingBag

class EmbeddingBag : public torch::nn::[ModuleHolder](utilities.html#_CPPv4I0EN5torch2nn12ModuleHolderE)<EmbeddingBagImpl>

A `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` subclass for `EmbeddingBagImpl`.

See the documentation for `EmbeddingBagImpl` class to learn what methods it provides, and examples of how to use `EmbeddingBag` with `torch::nn::EmbeddingBagOptions`. See the documentation for `[ModuleHolder](utilities.html#PyTorchclasstorch_1_1nn_1_1_module_holder)` to learn about PyTorch's module storage semantics.

Public Static Functions

static inline EmbeddingBag from_pretrained(const torch::Tensor &embeddings, const EmbeddingBagFromPretrainedOptions &options = {})

See the documentation for `torch::nn::EmbeddingBagFromPretrainedOptions` class to learn what optional arguments are supported for this function.

class EmbeddingBagImpl : public torch::nn::[Cloneable](utilities.html#_CPPv4I0EN5torch2nn9CloneableE)<EmbeddingBagImpl>

Computes sums or means of 'bags' of embeddings, without instantiating the intermediate embeddings.

See [https://pytorch.org/docs/main/nn.html#torch.nn.EmbeddingBag](https://pytorch.org/docs/main/nn.html#torch.nn.EmbeddingBag) to learn about the exact behavior of this module.

See the documentation for `torch::nn::EmbeddingBagOptions` class to learn what constructor arguments are supported for this module.

Example:

```
EmbeddingBag model(EmbeddingBagOptions(10,
2).max_norm(2).norm_type(2.5).scale_grad_by_freq(true).sparse(true).mode(torch::kSum).padding_idx(1));
```

Public Functions

inline EmbeddingBagImpl(int64_t num_embeddings, int64_t embedding_dim)

explicit EmbeddingBagImpl(EmbeddingBagOptions options_)

virtual void reset() override

`reset()` must perform initialization of all members with reference semantics, most importantly parameters, buffers and submodules.

void reset_parameters()

virtual void pretty_print(std::ostream &stream) const override

Pretty prints the `EmbeddingBag` module into the given `stream`.

[Tensor](../aten/tensor.html#_CPPv46Tensorv) forward(const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &input, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &offsets = {}, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &per_sample_weights = {})

Public Members

EmbeddingBagOptions options

The `Options` used to configure this `EmbeddingBag` module.

[Tensor](../aten/tensor.html#_CPPv46Tensorv) weight

The embedding table.

Friends

*friend struct* torch::nn::AnyModuleHolder