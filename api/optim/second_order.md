# Second-Order Optimizers

Second-order methods use curvature information (Hessian or its approximations)
to make better optimization steps. They can converge faster but are more
computationally expensive and memory-intensive.

## LBFGS (Limited-memory Broyden-Fletcher-Goldfarb-Shanno)

LBFGS is a quasi-Newton method that approximates the inverse Hessian using
gradient history. It can converge much faster than first-order methods for
smooth, convex-like loss surfaces.

**When to use:**

- Small models where memory isn't a concern
- Fine-tuning pre-trained models
- Convex or near-convex optimization problems
- Full-batch training (not mini-batch)

**Key parameters:**

- `lr`: Learning rate (often 1.0 for LBFGS)
- `max_iter`: Maximum iterations per step
- `history_size`: Number of past gradients to store

**Important:** LBFGS requires a closure function that recomputes the loss.

class LBFGS : public torch::optim::[Optimizer](index.html#_CPPv4N5torch5optim9OptimizerE)

Public Functions

inline explicit LBFGS(const std::vector<[OptimizerParamGroup](index.html#_CPPv4N5torch5optim19OptimizerParamGroupE)> ¶m_groups, LBFGSOptions defaults = {})

inline explicit LBFGS(std::vector<[Tensor](../aten/tensor.html#_CPPv46Tensorv)> params, LBFGSOptions defaults = {})

virtual [Tensor](../aten/tensor.html#_CPPv46Tensorv) step(LossClosure closure) override

A loss function closure, which is expected to return the loss value.

virtual void save(serialize::[OutputArchive](../serialize/archives.html#_CPPv4N5torch9serialize13OutputArchiveE) &archive) const override

Serializes the optimizer state into the given `archive`.

virtual void load(serialize::[InputArchive](../serialize/archives.html#_CPPv4N5torch9serialize12InputArchiveE) &archive) override

Deserializes the optimizer state from the given `archive`.

**Example:**

```
auto optimizer = torch::optim::LBFGS(
 model->parameters(),
 torch::optim::LBFGSOptions(1.0)
 .max_iter(20)
 .history_size(10));

// LBFGS requires a closure that recomputes the model
for (int epoch = 0; epoch < num_epochs; ++epoch) {
 auto closure = [&]() {
 optimizer.zero_grad();
 auto output = model->forward(data);
 auto loss = loss_fn(output, target);
 loss.backward();
 return loss;
 };
 optimizer.step(closure);
}
```