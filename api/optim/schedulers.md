# Learning Rate Schedulers

Learning rate schedulers adjust the learning rate during training, which often
improves convergence and final accuracy. Common strategies include:

- **Step decay**: Reduce LR by a factor every N epochs
- **Exponential decay**: Multiply LR by gamma each epoch
- **Cosine annealing**: Smoothly decrease LR following a cosine curve
- **Warmup**: Gradually increase LR at the start of training

## LRScheduler Base Class

class LRScheduler

Subclassed by torch::optim::StepLR

Public Functions

LRScheduler(torch::optim::[Optimizer](index.html#_CPPv4N5torch5optim9OptimizerE) &optimizer)

virtual ~LRScheduler() = default

void step()

## StepLR

Decays the learning rate by `gamma` every `step_size` epochs. This is the
simplest and most commonly used scheduler.

class StepLR : public torch::optim::LRScheduler

Public Functions

StepLR(torch::optim::[Optimizer](index.html#_CPPv4N5torch5optim9OptimizerE) &optimizer, const unsigned step_size, const double gamma = 0.1)

**Example:**

```
auto optimizer = torch::optim::SGD(
 model->parameters(),
 torch::optim::SGDOptions(0.1));

// Reduce LR by 10x every 30 epochs
auto scheduler = torch::optim::StepLR(
 optimizer,
 /*step_size=*/30,
 /*gamma=*/0.1);

for (int epoch = 0; epoch < 90; ++epoch) {
 train_one_epoch(model, optimizer, data_loader);
 scheduler.step();
 // LR: 0.1 (epochs 0-29), 0.01 (30-59), 0.001 (60-89)
}
```

## ReduceLROnPlateau

Reduces the learning rate when a metric has stopped improving. Useful when
you want the scheduler to respond to validation loss rather than follow a
fixed schedule.

class ReduceLROnPlateauScheduler

Public Types

enum SchedulerMode

*Values:*

enumerator min

enumerator max

enum ThresholdMode

*Values:*

enumerator rel

enumerator abs

Public Functions

ReduceLROnPlateauScheduler([Optimizer](index.html#_CPPv4N5torch5optim9OptimizerE) &optimizer, SchedulerMode mode = min, float factor = 0.1, int patience = 10, double threshold = 1e-4, ThresholdMode threshold_mode = rel, int cooldown = 0, const std::vector<float> &min_lr = std::vector<float>(), double eps = 1e-8, bool verbose = false)

virtual ~ReduceLROnPlateauScheduler() = default

void step(float metric)

## ExponentialLR

Decays the learning rate by `gamma` every epoch. Provides smoother decay than
StepLR but may be slower to reduce the learning rate.

**Example:**

```
auto optimizer = torch::optim::Adam(
 model->parameters(),
 torch::optim::AdamOptions(1e-3));

// Reduce LR by 5% each epoch
auto scheduler = torch::optim::ExponentialLR(
 optimizer,
 /*gamma=*/0.95);

for (int epoch = 0; epoch < num_epochs; ++epoch) {
 train_one_epoch(model, optimizer, data_loader);
 scheduler.step();
}
```

## Complete Training Example

Here's a complete example showing optimizer usage with learning rate scheduling:

```
#include <torch/torch.h>

struct Net : torch::nn::Module {
 Net() {
 fc1 = register_module("fc1", torch::nn::Linear(784, 256));
 fc2 = register_module("fc2", torch::nn::Linear(256, 10));
 }

 torch::Tensor forward(torch::Tensor x) {
 x = torch::relu(fc1->forward(x.view({-1, 784})));
 return fc2->forward(x);
 }

 torch::nn::Linear fc1{nullptr}, fc2{nullptr};
};

int main() {
 // Create model
 auto model = std::make_shared<Net>();

 // Create optimizer with weight decay
 auto optimizer = torch::optim::AdamW(
 model->parameters(),
 torch::optim::AdamWOptions(1e-3)
 .weight_decay(0.01));

 // Learning rate scheduler
 auto scheduler = torch::optim::StepLR(optimizer, 10, 0.5);

 // Loss function
 auto loss_fn = torch::nn::CrossEntropyLoss();

 // Training loop
 for (int epoch = 0; epoch < 30; ++epoch) {
 model->train();
 double epoch_loss = 0.0;

 for (auto& batch : *train_loader) {
 optimizer.zero_grad();

 auto output = model->forward(batch.data);
 auto loss = loss_fn(output, batch.target);

 loss.backward();
 optimizer.step();

 epoch_loss += loss.item<double>();
 }

 scheduler.step();
 std::cout << "Epoch " << epoch
 << " Loss: " << epoch_loss
 << " LR: " << scheduler.get_last_lr()[0]
 << std::endl;
 }

 return 0;
}
```