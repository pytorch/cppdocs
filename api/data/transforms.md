# Transforms

Transforms apply preprocessing to data samples, such as normalization or
augmentation. They can be chained using the `.map()` method on datasets.

## Transform (Base Class)

The base class for all transforms. Subclass this to create custom transforms.

template<typename Input, typename Output>
class Transform : public torch::data::transforms::BatchTransform<std::vector<Input>, std::vector<Output>>

A transformation of individual input examples to individual output examples.

Just like a `Dataset` is a `BatchDataset`, a `Transform` is a `BatchTransform` that can operate on the level of individual examples rather than entire batches. The batch-level transform is implemented (by default) in terms of the example-level transform, though this can be customized.

Public Types

using InputType = Input

using OutputType = Output

Public Functions

virtual OutputType apply(InputType input) = 0

Applies the transformation to the given `input`.

inline virtual std::vector<Output> apply_batch(std::vector<Input> input_batch) override

Applies the `transformation` over the entire `input_batch`.

## BatchTransform (Base Class)

Base class for transforms that operate on entire batches.

template<typename InputBatch, typename OutputBatch>
class BatchTransform

A transformation of a batch to a new batch.

Subclassed by torch::data::transforms::Transform< Example< Tensor, Tensor >, Example< Tensor, Tensor > >, torch::data::transforms::Transform< Input, Input >, torch::data::transforms::Stack< Example<> >, torch::data::transforms::Stack< TensorExample >

Public Types

using InputBatchType = InputBatch

using OutputBatchType = OutputBatch

Public Functions

virtual ~BatchTransform() = default

virtual OutputBatch apply_batch(InputBatch input_batch) = 0

Applies the transformation to the given `input_batch`.

## TensorTransform

Base class for transforms that operate on tensors specifically.

template<typename Target = [Tensor](../aten/tensor.html#_CPPv46Tensorv)>
class TensorTransform : public torch::data::transforms::Transform<[Example](datasets.html#_CPPv4I00EN5torch4data7ExampleE)<[Tensor](../aten/tensor.html#_CPPv46Tensorv), [Tensor](../aten/tensor.html#_CPPv46Tensorv)>, [Example](datasets.html#_CPPv4I00EN5torch4data7ExampleE)<[Tensor](../aten/tensor.html#_CPPv46Tensorv), [Tensor](../aten/tensor.html#_CPPv46Tensorv)>>

A `Transform` that is specialized for the typical `[Example](datasets.html#PyTorchstructtorch_1_1data_1_1_example)<Tensor, Tensor>` combination.

It exposes a single `operator()` interface hook (for subclasses), and calls this function on input `[Example](datasets.html#PyTorchstructtorch_1_1data_1_1_example)` objects.

Public Types

using E = [Example](datasets.html#_CPPv4I00EN5torch4data7ExampleE)<[Tensor](../aten/tensor.html#_CPPv46Tensorv), Target>

Public Functions

virtual [Tensor](../aten/tensor.html#_CPPv46Tensorv) operator()([Tensor](../aten/tensor.html#_CPPv46Tensorv) input) = 0

Transforms a single input tensor to an output tensor.

inline virtual OutputType apply(InputType input) override

Implementation of `Transform::apply` that calls `operator()`.

## Normalize

Normalizes tensors with a given mean and standard deviation.

template<typename Target = [Tensor](../aten/tensor.html#_CPPv46Tensorv)>
struct Normalize : public torch::data::transforms::TensorTransform<[Tensor](../aten/tensor.html#_CPPv46Tensorv)>

Normalizes input tensors by subtracting the supplied mean and dividing by the given standard deviation.

Public Functions

inline Normalize(ArrayRef<double> mean, ArrayRef<double> stddev)

Constructs a `Normalize` transform.

The mean and standard deviation can be anything that is broadcastable over the input tensors (like single scalars).

inline virtual torch::Tensor operator()([Tensor](../aten/tensor.html#_CPPv46Tensorv) input) override

Transforms a single input tensor to an output tensor.

Public Members

torch::Tensor mean

torch::Tensor stddev

## Stack

Stacks a batch of tensors into a single tensor.

template<typename T = [Example](datasets.html#_CPPv4I00EN5torch4data7ExampleE)<>>
struct Stack

**Example:**

```
auto dataset = torch::data::datasets::MNIST("./data")
 .map(torch::data::transforms::Normalize<>(0.5, 0.5))
 .map(torch::data::transforms::Stack<>());
```

## Lambda

template<typename Input, typename Output = Input>
class Lambda : public torch::data::transforms::Transform<Input, Input>

Public Types

using FunctionType = std::function<Output(Input)>

Public Functions

inline explicit Lambda(FunctionType function)

Constructs the `Lambda` from the given `function` object.

inline virtual OutputType apply(InputType input) override

Applies the user-provided function object to the `input`.

## TensorLambda

template<typename Target = [Tensor](../aten/tensor.html#_CPPv46Tensorv)>
class TensorLambda : public torch::data::transforms::TensorTransform<[Tensor](../aten/tensor.html#_CPPv46Tensorv)>

A `Lambda` specialized for the typical `[Example](datasets.html#PyTorchstructtorch_1_1data_1_1_example)<Tensor, Tensor>` input type.

Public Types

using FunctionType = std::function<[Tensor](../aten/tensor.html#_CPPv46Tensorv)([Tensor](../aten/tensor.html#_CPPv46Tensorv))>

Public Functions

inline explicit TensorLambda(FunctionType function)

Creates a `TensorLambda` from the given `function`.

inline virtual [Tensor](../aten/tensor.html#_CPPv46Tensorv) operator()([Tensor](../aten/tensor.html#_CPPv46Tensorv) input) override

Applies the user-provided functor to the input tensor.

## BatchLambda

template<typename Input, typename Output = Input>
class BatchLambda : public torch::data::transforms::BatchTransform<Input, Input>

A `BatchTransform` that applies a user-provided functor to a batch.

Public Types

using FunctionType = std::function<OutputBatchType(InputBatchType)>

Public Functions

inline explicit BatchLambda(FunctionType function)

Constructs the `BatchLambda` from the given `function` object.

inline virtual OutputBatchType apply_batch(InputBatchType input_batch) override

Applies the user-provided function object to the `input_batch`.

## Chaining Transforms

Transforms can be chained together using `.map()`:

```
auto dataset = torch::data::datasets::MNIST("./data")
 .map(torch::data::transforms::Normalize<>(0.1307, 0.3081))
 .map(torch::data::transforms::Stack<>());
```