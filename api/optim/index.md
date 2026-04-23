# Optimizers (torch::optim)

The `torch::optim` namespace provides optimization algorithms for
training neural networks. These optimizers update model parameters based
on computed gradients to minimize the loss function.

**When to use torch::optim:**

- When training neural networks with gradient descent
- When you need different optimization strategies (SGD, Adam, etc.)
- When implementing learning rate schedules

**Basic usage:**

```
#include <torch/torch.h>

// Create model and optimizer
auto model = std::make_shared<Net>();
auto optimizer = torch::optim::Adam(
 model->parameters(),
 torch::optim::AdamOptions(1e-3));

// Training loop
for (auto& batch : *data_loader) {
 optimizer.zero_grad(); // Clear gradients
 auto loss = loss_fn(model->forward(batch.data), batch.target);
 loss.backward(); // Compute gradients
 optimizer.step(); // Update parameters
}
```

## Header Files

- `torch/csrc/api/include/torch/optim.h` - Main optim header
- `torch/csrc/api/include/torch/optim/optimizer.h` - Optimizer base class
- `torch/csrc/api/include/torch/optim/sgd.h` - SGD optimizer
- `torch/csrc/api/include/torch/optim/adam.h` - Adam optimizer

## Optimizer Base Class

All optimizers inherit from the `Optimizer` base class, which provides common
functionality for parameter updates, gradient zeroing, and state management.

class Optimizer

Subclassed by [torch::optim::Adagrad](adaptive.html#PyTorchclasstorch_1_1optim_1_1_adagrad), [torch::optim::Adam](adaptive.html#PyTorchclasstorch_1_1optim_1_1_adam), [torch::optim::AdamW](adaptive.html#PyTorchclasstorch_1_1optim_1_1_adam_w), [torch::optim::LBFGS](second_order.html#PyTorchclasstorch_1_1optim_1_1_l_b_f_g_s), [torch::optim::RMSprop](adaptive.html#PyTorchclasstorch_1_1optim_1_1_r_m_sprop), [torch::optim::SGD](gradient_descent.html#PyTorchclasstorch_1_1optim_1_1_s_g_d)

Public Types

using LossClosure = std::function<[Tensor](../aten/tensor.html#_CPPv46Tensorv)()>

Public Functions

Optimizer(const Optimizer &optimizer) = delete

Optimizer(Optimizer &&optimizer) = default

Optimizer &operator=(const Optimizer &optimizer) = delete

Optimizer &operator=(Optimizer &&optimizer) = default

inline explicit Optimizer(const std::vector<OptimizerParamGroup> ¶m_groups, std::unique_ptr<OptimizerOptions> defaults)

inline explicit Optimizer(std::vector<[Tensor](../aten/tensor.html#_CPPv46Tensorv)> parameters, std::unique_ptr<OptimizerOptions> defaults)

Constructs the `Optimizer` from a vector of parameters.

void add_param_group(const OptimizerParamGroup ¶m_group)

Adds the given param_group to the optimizer's param_group list.

virtual ~Optimizer() = default

virtual [Tensor](../aten/tensor.html#_CPPv46Tensorv) step(LossClosure closure = nullptr) = 0

A loss function closure, which is expected to return the loss value.

void add_parameters(const std::vector<[Tensor](../aten/tensor.html#_CPPv46Tensorv)> ¶meters)

Adds the given vector of parameters to the optimizer's parameter list.

void zero_grad(bool set_to_none = true)

Zeros out the gradients of all parameters.

const std::vector<[Tensor](../aten/tensor.html#_CPPv46Tensorv)> ¶meters() const noexcept

Provides a const reference to the parameters in the first param_group this optimizer holds.

std::vector<[Tensor](../aten/tensor.html#_CPPv46Tensorv)> ¶meters() noexcept

Provides a reference to the parameters in the first param_group this optimizer holds.

size_t size() const noexcept

Returns the number of parameters referenced by the optimizer.

OptimizerOptions &defaults() noexcept

const OptimizerOptions &defaults() const noexcept

std::vector<OptimizerParamGroup> ¶m_groups() noexcept

Provides a reference to the param_groups this optimizer holds.

const std::vector<OptimizerParamGroup> ¶m_groups() const noexcept

Provides a const reference to the param_groups this optimizer holds.

ska::flat_hash_map<void*, std::unique_ptr<OptimizerParamState>> &state() noexcept

Provides a reference to the state this optimizer holds.

const ska::flat_hash_map<void*, std::unique_ptr<OptimizerParamState>> &state() const noexcept

Provides a const reference to the state this optimizer holds.

virtual void save(serialize::[OutputArchive](../serialize/archives.html#_CPPv4N5torch9serialize13OutputArchiveE) &archive) const

Serializes the optimizer state into the given `archive`.

virtual void load(serialize::[InputArchive](../serialize/archives.html#_CPPv4N5torch9serialize12InputArchiveE) &archive)

Deserializes the optimizer state from the given `archive`.

### OptimizerOptions

class OptimizerOptions

Public Functions

OptimizerOptions() = default

OptimizerOptions(const OptimizerOptions&) = default

OptimizerOptions &operator=(const OptimizerOptions&) = default

OptimizerOptions(OptimizerOptions&&) noexcept = default

OptimizerOptions &operator=(OptimizerOptions&&) noexcept = default

virtual std::unique_ptr<OptimizerOptions> clone() const

virtual void serialize(torch::serialize::[InputArchive](../serialize/archives.html#_CPPv4N5torch9serialize12InputArchiveE) &archive)

virtual void serialize(torch::serialize::[OutputArchive](../serialize/archives.html#_CPPv4N5torch9serialize13OutputArchiveE) &archive) const

virtual ~OptimizerOptions() = default

virtual double get_lr() const

virtual void set_lr(const double lr)

### OptimizerParamGroup

class OptimizerParamGroup

Stores parameters in the param_group and stores a pointer to the OptimizerOptions.

Public Functions

inline OptimizerParamGroup(const OptimizerParamGroup ¶m_group)

OptimizerParamGroup(OptimizerParamGroup &¶m_group) = default

inline OptimizerParamGroup(std::vector<[Tensor](../aten/tensor.html#_CPPv46Tensorv)> params)

inline OptimizerParamGroup(std::vector<[Tensor](../aten/tensor.html#_CPPv46Tensorv)> params, std::unique_ptr<OptimizerOptions> options)

OptimizerParamGroup &operator=(const OptimizerParamGroup ¶m_group) = delete

OptimizerParamGroup &operator=(OptimizerParamGroup &¶m_group) noexcept = default

~OptimizerParamGroup() = default

bool has_options() const

OptimizerOptions &options()

const OptimizerOptions &options() const

void set_options(std::unique_ptr<OptimizerOptions> options)

std::vector<[Tensor](../aten/tensor.html#_CPPv46Tensorv)> ¶ms()

const std::vector<[Tensor](../aten/tensor.html#_CPPv46Tensorv)> ¶ms() const

### OptimizerParamState

class OptimizerParamState

Public Functions

OptimizerParamState() = default

OptimizerParamState(const OptimizerParamState&) = default

OptimizerParamState &operator=(const OptimizerParamState&) = default

OptimizerParamState(OptimizerParamState&&) noexcept = default

OptimizerParamState &operator=(OptimizerParamState&&) noexcept = default

virtual std::unique_ptr<OptimizerParamState> clone() const

virtual void serialize(torch::serialize::[InputArchive](../serialize/archives.html#_CPPv4N5torch9serialize12InputArchiveE) &archive)

virtual void serialize(torch::serialize::[OutputArchive](../serialize/archives.html#_CPPv4N5torch9serialize13OutputArchiveE) &archive) const

virtual ~OptimizerParamState() = default

## Choosing an Optimizer

Selecting the right optimizer depends on your model architecture, dataset, and
training requirements:

| Optimizer | Best For | Trade-offs |
| --- | --- | --- |
| **SGD + Momentum** | CNNs, well-understood problems, when you can tune hyperparameters | Requires careful learning rate tuning; often achieves best final accuracy |
| **Adam/AdamW** | General-purpose, transformers, quick prototyping | Works well out-of-the-box; AdamW preferred with weight decay |
| **RMSprop** | RNNs, non-stationary objectives | Good for recurrent architectures; handles varying gradient scales |
| **Adagrad** | Sparse data (NLP, embeddings) | Learning rate decreases over time; good for infrequent features |
| **LBFGS** | Small models, fine-tuning, convex problems | Memory-intensive; requires closure function |

## Optimizer Categories

- [Gradient Descent Optimizers](gradient_descent.html)
- [Adaptive Learning Rate Optimizers](adaptive.html)
- [Second-Order Optimizers](second_order.html)
- [Learning Rate Schedulers](schedulers.html)