# Custom Autograd Functions

PyTorch allows you to define custom autograd functions with custom forward
and backward implementations.

## Function Base Class

template<class T>
struct Function

To use custom autograd operations, implement a Function subclass with static forward and backward functions:

`forward` can take as many arguments as you want and should return either a variable list or a Variable. Use of any direct Variable arguments will be registered in the graph but no vectors/sets or any other data structures will be traversed. You can use std::optional<Tensor> as one of the arguments and it will be registered as a variable in the graph if the argument has a value. It should take a pointer to `torch::autograd::AutogradContext` as the first argument. Variables can be saved in the `ctx` using `ctx->save_for_backward` (see `torch::autograd::AutogradContext::save_for_backward`) and other data can be saved in the `ctx->saved_data` map (see `torch::autograd::AutogradContext::saved_data`) in the form of `<std::string, at::IValue>` pairs.

`backward` should take a pointer to `torch::autograd::AutogradContext` and a variable list containing as many Variables as there were outputs from `forward` as arguments. It should return as many Variables as there were inputs with each of them containing the gradient w.r.t. its corresponding input. Variables saved in `forward` can be accessed with `ctx->get_saved_variables` (see `torch::autograd::AutogradContext::get_saved_variables`) and other saved data can be accessed from `ctx->saved_data`. To enable compiled autograd support (torch.compile for backward) for your custom autograd operation, you can set MyFunction::is_traceable (see Function::istraceable notes below).

For example:

```
class MyFunction : public Function<MyFunction> {
 public:
 static constexpr bool is_traceable = true;

 static variable_list forward(AutogradContext *ctx, int n, Variable var) {
 // Save data for backward in context
 ctx->saved_data["n"] = n;
 var.mul_(n);
 // Mark var as modified by inplace operation
 ctx->mark_dirty({var});
 return {var};
 }

 static variable_list backward(AutogradContext *ctx, variable_list
 grad_output) {
 // Use data saved in forward
 auto n = ctx->saved_data["n"].toInt();
 return {grad_output[0]*n};
 }
};
```

To use `MyFunction`:

```
Variable x;
auto y = MyFunction::apply(6, x);
// Example backward call
y[0].sum().backward();
```

Public Static Functions

template<typename X = T, typename ...Args>
static auto apply(Args&&... args) -> std::enable_if_t<std::is_same_v<X, T>, forward_t<X, Args...>>

Public Static Attributes

static constexpr bool is_traceable = false

## AutogradContext

struct AutogradContext

Context to save information during `forward` that can be accessed in `backward` in custom autograd operations (see `torch::autograd::Function` for details).

Public Functions

AutogradContext() = default

AutogradContext(const AutogradContext &other) = delete

AutogradContext &operator=(const AutogradContext &other) = delete

AutogradContext(AutogradContext &&other) = delete

AutogradContext &operator=(AutogradContext &&other) = delete

~AutogradContext() = default

AutogradContext(PackedArgs &packed_args)

void save_for_backward(variable_list to_save)

Saves the list of variables for a future call to `backward`.

This should be called at most once from inside of `forward`.

void mark_dirty(const variable_list &inputs)

Marks variables in the list as modified in an in-place operation.

This should be called at most once from inside of `forward` and all arguments should be inputs.

void mark_non_differentiable(const variable_list &outputs)

Marks outputs in the list as not requiring gradients.

This should be called at most once from inside of `forward` and all arguments should be outputs.

void set_materialize_grads(bool value)

variable_list get_saved_variables() const

Get the list of variables that were saved in `forward` using `save_for_backward()`.

Before returning them to the user, a check is made to ensure that they were not modified by any in-place operations.

const std::unordered_set<at::TensorImpl*> &get_and_bump_dirty() const

const std::unordered_set<at::TensorImpl*> &get_non_differentiable() const

bool needs_input_grad(size_t output_edge_index) const

Expose the Node's `task_should_compute_output` method to the cpp custom autograd Function as `needs_input_grad`.

bool needs_input_grad(std::initializer_list<IndexRange> idxs) const

Public Members

ska::flat_hash_map<std::string, at::IValue> saved_data

Can be used to save non-variable data for `backward`.

## Creating Custom Functions

To create a custom autograd function, inherit from `torch::autograd::Function`
and implement the static `forward` and `backward` methods:

**Example:**

```
class MyReLU : public torch::autograd::Function<MyReLU> {
 public:
 static torch::Tensor forward(
 torch::autograd::AutogradContext* ctx,
 torch::Tensor input) {
 ctx->save_for_backward({input});
 return input.clamp_min(0);
 }

 static torch::autograd::variable_list backward(
 torch::autograd::AutogradContext* ctx,
 torch::autograd::variable_list grad_outputs) {
 auto saved = ctx->get_saved_variables();
 auto input = saved[0];
 auto grad_output = grad_outputs[0];
 auto grad_input = grad_output * (input > 0).to(grad_output.dtype());
 return {grad_input};
 }
};

// Usage
auto output = MyReLU::apply(input);
```

## Custom Kernels and AutoDispatchBelowADInplaceOrView

For users implementing custom kernels who want to redispatch below `Autograd` dispatch
keys, use `at::AutoDispatchBelowADInplaceOrView` instead of `InferenceMode`:

```
class ROIAlignFunction : public torch::autograd::Function<ROIAlignFunction> {
 public:
 static torch::autograd::variable_list forward(
 torch::autograd::AutogradContext* ctx,
 const torch::autograd::Variable& input,
 const torch::autograd::Variable& rois,
 double spatial_scale,
 int64_t pooled_height,
 int64_t pooled_width,
 int64_t sampling_ratio,
 bool aligned) {
 ctx->saved_data["spatial_scale"] = spatial_scale;
 ctx->saved_data["pooled_height"] = pooled_height;
 ctx->saved_data["pooled_width"] = pooled_width;
 ctx->saved_data["sampling_ratio"] = sampling_ratio;
 ctx->saved_data["aligned"] = aligned;
 ctx->saved_data["input_shape"] = input.sizes();
 ctx->save_for_backward({rois});

 at::AutoDispatchBelowADInplaceOrView guard;
 auto result = roi_align(
 input, rois, spatial_scale, pooled_height,
 pooled_width, sampling_ratio, aligned);
 return {result};
 }
};
```

For customized inplace and view kernels, see the
[custom kernel tutorial](https://pytorch.org/tutorials/advanced/cpp_extension.html#backward-pass)
for more details.