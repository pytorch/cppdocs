# Saving and Loading

The primary interface for serialization uses the `torch::save` and
`torch::load` functions, which can save and load tensors, modules,
and optimizers.

## Save Functions

template<typename Value, typename ...SaveToArgs>
void torch::save(const Value &value, SaveToArgs&&... args)

Serializes the given `value`.

There must be an overload of `operator<<` between `[serialize::OutputArchive](archives.html#PyTorchclasstorch_1_1serialize_1_1_output_archive)` and `Value` for this method to be well-formed. Currently, such an overload is provided for (subclasses of):

- `[torch::nn::Module](../nn/index.html#PyTorchclasstorch_1_1nn_1_1_module)`,
- `[torch::optim::Optimizer](../optim/index.html#PyTorchclasstorch_1_1optim_1_1_optimizer)`
- `torch::Tensor`

To perform the serialization, a `[serialize::OutputArchive](archives.html#PyTorchclasstorch_1_1serialize_1_1_output_archive)` is constructed, and all arguments after the `value` are forwarded to its `save_to` method. For example, you can pass a filename, or an `ostream`.

- .. code-block:: cpp
- 
- torch::nn::Linear model(3, 4);
- torch::save(model, "model.pt");
- 
- torch::optim::SGD sgd(model->parameters(), 0.9); // 0.9 is learning rate
- std::ostringstream stream;
- // Note that the same stream cannot be used in multiple torch::save(...)
- // invocations, otherwise the header will be corrupted.
- torch::save(sgd, stream);
- 
- auto tensor = torch::ones({3, 4});
- torch::save(tensor, "my_tensor.pt");
- 

template<typename ...SaveToArgs>
void torch::save(const std::vector<torch::Tensor> &tensor_vec, SaveToArgs&&... args)

Serializes the given `tensor_vec` of type `std::vector<torch::Tensor>`.

To perform the serialization, a `[serialize::OutputArchive](archives.html#PyTorchclasstorch_1_1serialize_1_1_output_archive)` is constructed, and all arguments after the `tensor_vec` are forwarded to its `save_to` method. For example, you can pass a filename, or an `ostream`.

- .. code-block:: cpp
- 
- std::vector[torch::Tensor](torch::Tensor) tensor_vec = { torch::randn({1, 2}),
- torch::randn({3, 4}) }; torch::save(tensor_vec, "my_tensor_vec.pt");
- 
- std::vector[torch::Tensor](torch::Tensor) tensor_vec = { torch::randn({5, 6}),
- torch::randn({7, 8}) }; std::ostringstream stream;
- // Note that the same stream cannot be used in multiple torch::save(...)
- // invocations, otherwise the header will be corrupted.
- torch::save(tensor_vec, stream);
- 

## Load Functions

template<typename Value, typename ...LoadFromArgs>
void torch::load(Value &value, LoadFromArgs&&... args)

Deserializes the given `value`.

There must be an overload of `operator>>` between `[serialize::InputArchive](archives.html#PyTorchclasstorch_1_1serialize_1_1_input_archive)` and `Value` for this method to be well-formed. Currently, such an overload is provided for (subclasses of):

- `[torch::nn::Module](../nn/index.html#PyTorchclasstorch_1_1nn_1_1_module)`,
- `[torch::optim::Optimizer](../optim/index.html#PyTorchclasstorch_1_1optim_1_1_optimizer)`
- `torch::Tensor`

To perform the serialization, a `[serialize::InputArchive](archives.html#PyTorchclasstorch_1_1serialize_1_1_input_archive)` is constructed, and all arguments after the `value` are forwarded to its `load_from` method. For example, you can pass a filename, or an `istream`.

- .. code-block:: cpp
- 
- torch::nn::Linear model(3, 4);
- torch::load(model, "model.pt");
- 
- torch::optim::SGD sgd(model->parameters(), 0.9); // 0.9 is learning rate
- std::istringstream stream("...");
- torch::load(sgd, stream);
- 
- auto tensor = torch::ones({3, 4});
- torch::load(tensor, "my_tensor.pt");
- 

template<typename ...LoadFromArgs>
void torch::load(std::vector<torch::Tensor> &tensor_vec, LoadFromArgs&&... args)

Deserializes the given `tensor_vec` of type `std::vector<torch::Tensor>`.

To perform the serialization, a `[serialize::InputArchive](archives.html#PyTorchclasstorch_1_1serialize_1_1_input_archive)` is constructed, and all arguments after the `value` are forwarded to its `load_from` method. For example, you can pass a filename, or an `istream`.

- .. code-block:: cpp
- 
- std::vector[torch::Tensor](torch::Tensor) tensor_vec;
- torch::load(tensor_vec, "my_tensor_vec.pt");
- 
- std::vector[torch::Tensor](torch::Tensor) tensor_vec;
- std::istringstream stream("...");
- torch::load(tensor_vec, stream);
- 

## Saving and Loading Tensors

```
// Save a tensor
torch::Tensor tensor = torch::randn({2, 3});
torch::save(tensor, "tensor.pt");

// Load a tensor
torch::Tensor loaded;
torch::load(loaded, "tensor.pt");
```

## Saving and Loading Modules

```
// Define a model
struct Net : torch::nn::Module {
 Net() {
 fc1 = register_module("fc1", torch::nn::Linear(784, 64));
 fc2 = register_module("fc2", torch::nn::Linear(64, 10));
 }

 torch::Tensor forward(torch::Tensor x) {
 x = torch::relu(fc1->forward(x));
 return fc2->forward(x);
 }

 torch::nn::Linear fc1{nullptr}, fc2{nullptr};
};

// Save model
auto model = std::make_shared<Net>();
torch::save(model, "model.pt");

// Load model
auto loaded_model = std::make_shared<Net>();
torch::load(loaded_model, "model.pt");
```

## Saving Optimizer State

```
auto model = std::make_shared<Net>();
auto optimizer = torch::optim::Adam(model->parameters(), 0.001);

// Train...

// Save both model and optimizer
torch::save(model, "model.pt");
torch::save(optimizer, "optimizer.pt");

// Load both
torch::load(model, "model.pt");
torch::load(optimizer, "optimizer.pt");
```