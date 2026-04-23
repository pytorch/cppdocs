# Gradient Descent Optimizers

These optimizers use gradient descent with optional enhancements like momentum.
They are the foundation of neural network training and work well when you can
afford careful hyperparameter tuning.

## SGD (Stochastic Gradient Descent)

The classic optimization algorithm. SGD with momentum is often the best choice
for convolutional neural networks when properly tuned. While requiring more
careful learning rate selection than adaptive methods, it frequently achieves
the best final accuracy.

**When to use:**

- Training CNNs (ResNet, VGG, etc.) where you want maximum accuracy
- When you have time for hyperparameter tuning
- When combined with learning rate schedules (warmup, cosine annealing)

**Key parameters:**

- `lr`: Learning rate (typical: 0.01-0.1 for CNNs)
- `momentum`: Accelerates convergence (typical: 0.9)
- `weight_decay`: L2 regularization coefficient
- `nesterov`: Use Nesterov momentum (often improves convergence)

class SGD : public torch::optim::[Optimizer](index.html#_CPPv4N5torch5optim9OptimizerE)

Public Functions

inline explicit SGD(const std::vector<[OptimizerParamGroup](index.html#_CPPv4N5torch5optim19OptimizerParamGroupE)> ¶m_groups, SGDOptions defaults)

inline explicit SGD(std::vector<[Tensor](../aten/tensor.html#_CPPv46Tensorv)> params, SGDOptions defaults)

virtual torch::Tensor step(LossClosure closure = nullptr) override

A loss function closure, which is expected to return the loss value.

virtual void save(serialize::[OutputArchive](../serialize/archives.html#_CPPv4N5torch9serialize13OutputArchiveE) &archive) const override

Serializes the optimizer state into the given `archive`.

virtual void load(serialize::[InputArchive](../serialize/archives.html#_CPPv4N5torch9serialize12InputArchiveE) &archive) override

Deserializes the optimizer state from the given `archive`.

**Example:**

```
// Standard SGD with momentum - good for CNNs
auto optimizer = torch::optim::SGD(
 model->parameters(),
 torch::optim::SGDOptions(0.01) // learning rate
 .momentum(0.9) // momentum factor
 .weight_decay(1e-4) // L2 regularization
 .nesterov(true)); // Nesterov momentum
```