# Archives

Archives provide a lower-level interface for serialization, allowing you to
save multiple values to a single file with named keys.

## OutputArchive

class OutputArchive

Public Functions

explicit OutputArchive(std::shared_ptr<jit::CompilationUnit> cu)

inline explicit OutputArchive()

OutputArchive(OutputArchive&&) = default

OutputArchive &operator=(OutputArchive&&) = default

OutputArchive(OutputArchive&) = delete

OutputArchive &operator=(OutputArchive&) = delete

inline std::shared_ptr<jit::CompilationUnit> compilation_unit() const

void write(const std::string &key, const c10::IValue &ivalue)

Writes an `IValue` to the `OutputArchive`.

void write(const std::string &key, const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &tensor, bool is_buffer = false)

Writes a `(key, tensor)` pair to the `OutputArchive`, and marks it as being or not being a buffer (non-differentiable tensor).

void write(const std::string &key, OutputArchive &nested_archive)

Writes a nested `OutputArchive` under the given `key` to this `OutputArchive`.

void save_to(const std::string &filename)

Saves the `OutputArchive` into a serialized representation in a file at `filename`.

void save_to(std::ostream &stream)

Saves the `OutputArchive` into a serialized representation into the given `stream`.

void save_to(const std::function<size_t(const void*, size_t)> &func)

Saves the `OutputArchive` into a serialized representation using the given writer function.

template<typename ...Ts>
inline void operator()(Ts&&... ts)

Forwards all arguments to `write()`.

Useful for generic code that can be reused for both `OutputArchive` and `InputArchive` (where `operator()` forwards to `read()`).

**Example:**

```
torch::serialize::OutputArchive archive;
archive.write("tensor1", tensor1);
archive.write("tensor2", tensor2);
archive.save_to("multi_tensor.pt");
```

## InputArchive

class InputArchive

A recursive representation of tensors that can be deserialized from a file or stream.

In most cases, users should not have to interact with this class, and should instead use `[torch::load](save_load.html#PyTorchnamespacetorch_1ad98de93d4a74dd9a91161f64758f1a76)`.

Public Functions

InputArchive()

Default-constructs the `InputArchive`.

InputArchive(InputArchive&&) = default

InputArchive &operator=(InputArchive&&) = default

InputArchive(InputArchive&) = delete

InputArchive &operator=(InputArchive&) = delete

~InputArchive() = default

void read(const std::string &key, c10::IValue &ivalue)

Reads an `IValue` associated with a given `key`.

bool try_read(const std::string &key, c10::IValue &ivalue)

Reads an `IValue` associated with a given `key`.

If there is no `IValue` associated with the `key`, this returns false, otherwise it returns true.

bool try_read(const std::string &key, [Tensor](../aten/tensor.html#_CPPv46Tensorv) &tensor, bool is_buffer = false)

Reads a `tensor` associated with a given `key`.

If there is no `tensor` associated with the `key`, this returns false, otherwise it returns true. If the tensor is expected to be a buffer (not differentiable), `is_buffer` must be `true`.

void read(const std::string &key, [Tensor](../aten/tensor.html#_CPPv46Tensorv) &tensor, bool is_buffer = false)

Reads a `tensor` associated with a given `key`.

If the tensor is expected to be a buffer (not differentiable), `is_buffer` must be `true`.

bool try_read(const std::string &key, InputArchive &archive)

Reads a `InputArchive` associated with a given `key`.

If there is no `InputArchive` associated with the `key`, this returns false, otherwise it returns true.

void read(const std::string &key, InputArchive &archive)

Reads an `InputArchive` associated with a given `key`.

The archive can thereafter be used for further deserialization of the nested data.

void load_from(const std::string &filename, std::optional<torch::Device> device = std::nullopt)

Loads the `InputArchive` from a serialized representation stored in the file at `filename`.

Storage are remapped using device option. If device is not specified, the module is loaded to the original device.

void load_from(std::istream &stream, std::optional<torch::Device> device = std::nullopt)

Loads the `InputArchive` from a serialized representation stored in the given `stream`.

Storage are remapped using device option. If device is not specified, the module is loaded to the original device.

void load_from(const char *data, size_t size, std::optional<torch::Device> device = std::nullopt)

void load_from(const std::function<size_t(uint64_t pos, void *buf, size_t nbytes)> &read_func, const std::function<size_t(void)> &size_func, std::optional<torch::Device> device = std::nullopt)

std::vector<std::string> keys()

template<typename ...Ts>
inline void operator()(Ts&&... ts)

Forwards all arguments to `read()`.

Useful for generic code that can be reused for both `InputArchive` and `OutputArchive` (where `operator()` forwards to `write()`).

**Example:**

```
torch::serialize::InputArchive archive;
archive.load_from("multi_tensor.pt");

torch::Tensor tensor1, tensor2;
archive.read("tensor1", tensor1);
archive.read("tensor2", tensor2);
```

## Saving Multiple Values

Archives are useful when you need to save multiple related values together:

```
// Save multiple tensors and metadata
torch::serialize::OutputArchive out_archive;
out_archive.write("weights", model_weights);
out_archive.write("biases", model_biases);
out_archive.write("epoch", torch::tensor(current_epoch));
out_archive.write("loss", torch::tensor(best_loss));
out_archive.save_to("training_state.pt");

// Load them back
torch::serialize::InputArchive in_archive;
in_archive.load_from("training_state.pt");

torch::Tensor weights, biases, epoch_tensor, loss_tensor;
in_archive.read("weights", weights);
in_archive.read("biases", biases);
in_archive.read("epoch", epoch_tensor);
in_archive.read("loss", loss_tensor);

int epoch = epoch_tensor.item<int>();
float loss = loss_tensor.item<float>();
```