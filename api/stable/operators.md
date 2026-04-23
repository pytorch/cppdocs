# Stable Operators

The stable API provides tensor operations that maintain binary compatibility
across PyTorch versions.

## Tensor Class

class Tensor

An ABI stable wrapper around PyTorch tensors.

This class is modeled after TensorBase, as custom op kernels primarily need to interact with Tensor metadata (sizes, strides, device, dtype). Other tensor operations (like `empty_like`) exist as standalone functions outside of this struct.

Minimum compatible version: PyTorch 2.9.

Public Functions

inline Tensor()

Constructs a Tensor with an uninitialized AtenTensorHandle.

Creates a new stable::Tensor by allocating an uninitialized tensor handle. The ownership of the handle is managed internally via shared_ptr.

Minimum compatible version: PyTorch 2.9.

inline explicit Tensor(AtenTensorHandle ath)

Constructs a Tensor from an existing AtenTensorHandle.

Steals ownership of the provided AtenTensorHandle.

Minimum compatible version: PyTorch 2.9.

Parameters:

**ath** - The AtenTensorHandle to wrap. Ownership is transferred to this Tensor.

inline AtenTensorHandle get() const

Returns a borrowed reference to the underlying AtenTensorHandle.

Minimum compatible version: PyTorch 2.9.

Returns:

The underlying AtenTensorHandle.

inline void *data_ptr() const

Returns a pointer to the tensor's data.

Minimum compatible version: PyTorch 2.9.

Returns:

A void pointer to the tensor's data storage.

inline void *mutable_data_ptr() const

Returns a mutable pointer to the tensor's data.

Minimum compatible version: PyTorch 2.10.

Returns:

A mutable void pointer to the tensor's data storage.

inline const void *const_data_ptr() const

Returns a const pointer to the tensor's data.

Minimum compatible version: PyTorch 2.10.

Returns:

A const void pointer to the tensor's data storage.

template<typename T>
T *mutable_data_ptr() const

Returns a typed mutable pointer to the tensor's data.

Minimum compatible version: PyTorch 2.10.

Template Parameters:

**T** - The type to cast the data pointer to.

Returns:

A mutable pointer to the tensor's data cast to type T*.

template<typename T, std::enable_if_t<!std::is_const_v<T>, int> = 0>
const T *const_data_ptr() const

Returns a typed const pointer to the tensor's data.

Minimum compatible version: PyTorch 2.10.

Template Parameters:

**T** - The type to cast the data pointer to. Must not be const-qualified.

Returns:

A const pointer to the tensor's data cast to type const T*.

inline const Tensor &set_requires_grad(bool requires_grad) const

Sets whether this tensor requires gradient computation.

Minimum compatible version: PyTorch 2.10.

Parameters:

**requires_grad** - If true, gradients will be computed for this tensor during backpropagation.

Returns:

A reference to this Tensor.

inline int64_t dim() const

Returns the number of dimensions of the tensor.

Minimum compatible version: PyTorch 2.9.

Returns:

The number of dimensions (rank) of the tensor.

inline int64_t numel() const

Returns the total number of elements in the tensor.

Minimum compatible version: PyTorch 2.9.

Returns:

The total number of elements across all dimensions.

inline IntHeaderOnlyArrayRef sizes() const

Returns the sizes (shape) of the tensor.

Returns a borrowed reference of the dimension sizes of the tensor.

Minimum compatible version: PyTorch 2.9.

Returns:

An IntHeaderOnlyArrayRef containing the size of each dimension.

inline IntHeaderOnlyArrayRef strides() const

Returns the strides of the tensor.

Returns a borrowed reference of the strides of the tensor.

Minimum compatible version: PyTorch 2.9.

Returns:

An IntHeaderOnlyArrayRef containing the stride of each dimension.

inline bool is_contiguous() const

Checks if the tensor is contiguous in memory.

Minimum compatible version: PyTorch 2.9.

Note

This is a subset of the original TensorBase API. It takes no arguments whereas the original API takes a memory format argument. Here, we assume the default contiguous memory format.

Returns:

true if the tensor is contiguous, false otherwise.

inline int64_t stride(int64_t dim) const

Returns the stride of a specific dimension.

Minimum compatible version: PyTorch 2.9.

Parameters:

**dim** - The dimension index to query.

Returns:

The stride of the specified dimension.

inline DeviceIndex get_device_index() const

Returns the device index of the tensor.

Minimum compatible version: PyTorch 2.9.

Returns:

The device index as DeviceIndex (int32_t).

inline bool is_cuda() const

Checks if the tensor is on a CUDA device.

Minimum compatible version: PyTorch 2.9.

Returns:

true if the tensor is on a CUDA device, false otherwise.

inline bool is_cpu() const

Checks if the tensor is on the CPU.

Minimum compatible version: PyTorch 2.9.

Returns:

true if the tensor is on the CPU, false otherwise.

inline int64_t size(int64_t dim) const

Returns the size of a specific dimension.

Minimum compatible version: PyTorch 2.9.

Parameters:

**dim** - The dimension index to query.

Returns:

The size of the specified dimension.

inline bool defined() const

Checks if the tensor is defined (not null).

Minimum compatible version: PyTorch 2.9.

Returns:

true if the tensor is defined, false otherwise.

inline int64_t storage_offset() const

Returns the storage offset of the tensor.

The storage offset is the number of elements from the beginning of the underlying storage to the first element of the tensor.

Minimum compatible version: PyTorch 2.9.

Returns:

The storage offset in number of elements.

inline size_t element_size() const

Returns the size in bytes of each element in the tensor.

Minimum compatible version: PyTorch 2.9.

Returns:

The element size in bytes.

ScalarType scalar_type() const

Returns the scalar type (dtype) of the tensor.

Minimum compatible version: PyTorch 2.9.

Returns:

The ScalarType of the tensor.

Device device() const

Returns the device of the tensor.

Minimum compatible version: PyTorch 2.9.

Returns:

The Device on which the tensor resides.

Layout layout() const

Returns the layout of the tensor.

Minimum compatible version: PyTorch 2.9.

Returns:

The Layout of the tensor (e.g., Strided, Sparse).

**Example:**

```
torch::stable::Tensor tensor = torch::stable::empty({3, 4}, ...);
float* data = tensor.data_ptr<float>();
auto shape = tensor.sizes();
```

## Device Class

class Device

A stable version of [c10::Device](../c10/device.html#PyTorchstructc10_1_1_device).

Minimum compatible version: PyTorch 2.9.

Public Functions

inline Device(DeviceType type, DeviceIndex index = -1)

Constructs a Device from a DeviceType and optional device index.

Minimum compatible version: PyTorch 2.9.

Parameters:

- **type** - The type of device (e.g., DeviceType::CPU, DeviceType::CUDA).
- **index** - The device index. Default is -1 (current device).

Device(const std::string &device_string)

Constructs a stable::Device from a string description.

The string must follow the schema: (cpu|cuda|...)[:<device-index>]

Minimum compatible version: PyTorch 2.10.

Parameters:

**device_string** - A string describing the device (e.g., "cuda:0", "cpu").

inline bool operator==(const Device &other) const noexcept

Checks if two devices are equal.

Minimum compatible version: PyTorch 2.9.

Parameters:

**other** - The device to compare with.

Returns:

true if both type and index match, false otherwise.

inline bool operator!=(const Device &other) const noexcept

Checks if two devices are not equal.

Minimum compatible version: PyTorch 2.9.

Parameters:

**other** - The device to compare with.

Returns:

true if type or index differ, false otherwise.

inline void set_index(DeviceIndex index)

Sets the device index.

Minimum compatible version: PyTorch 2.9.

Parameters:

**index** - The new device index.

inline DeviceType type() const noexcept

Returns the device type.

Minimum compatible version: PyTorch 2.9.

Returns:

The DeviceType of this device.

inline DeviceIndex index() const noexcept

Returns the device index.

Minimum compatible version: PyTorch 2.9.

Returns:

The device index, or -1 if no specific index is set.

inline bool has_index() const noexcept

Checks if this device has a specific index.

Minimum compatible version: PyTorch 2.9.

Returns:

true if index is not -1, false otherwise.

inline bool is_cuda() const noexcept

Checks if this is a CUDA device.

Minimum compatible version: PyTorch 2.9.

Returns:

true if the device type is CUDA, false otherwise.

inline bool is_cpu() const noexcept

Checks if this is a CPU device.

Minimum compatible version: PyTorch 2.9.

Returns:

true if the device type is CPU, false otherwise.

**Example:**

```
torch::stable::Device cpu_device(torch::headeronly::DeviceType::CPU);
torch::stable::Device cuda_device(torch::headeronly::DeviceType::CUDA, 0);
```

## Tensor Creation

inline torch::stable::Tensor torch::stable::empty(torch::headeronly::IntHeaderOnlyArrayRef size, std::optional<torch::headeronly::ScalarType> dtype = std::nullopt, std::optional<torch::headeronly::Layout> layout = std::nullopt, std::optional<torch::stable::Device> device = std::nullopt, std::optional<bool> pin_memory = std::nullopt, std::optional<torch::headeronly::MemoryFormat> memory_format = std::nullopt)

Stable version of the empty.memory_format op.

Creates a new uninitialized tensor with the specified size and options. This function supports full tensor creation options including device, dtype, layout, and memory format.

Minimum compatible version: PyTorch 2.10.

Parameters:

- **size** - The desired size of the output tensor.
- **dtype** - Optional scalar type for the tensor elements.
- **layout** - Optional memory layout (e.g., strided, sparse).
- **device** - Optional device to place the tensor on.
- **pin_memory** - Optional flag to use pinned memory (for CUDA tensors).
- **memory_format** - Optional memory format for the tensor.

Returns:

A new uninitialized tensor with the specified properties.

inline torch::stable::Tensor torch::stable::empty_like(const torch::stable::Tensor &self)

Stable version of the empty_like op.

Creates a new uninitialized tensor with the same size, dtype, layout, and device as the input tensor. This version does not support kwargs (device, dtype, layout, memory_format) - kwargs support may be added in the future.

Minimum compatible version: PyTorch 2.9.

Parameters:

**self** - The input tensor whose properties will be used for the new tensor.

Returns:

A new uninitialized tensor with the same properties as self.

inline torch::stable::Tensor torch::stable::new_empty(const torch::stable::Tensor &self, torch::headeronly::IntHeaderOnlyArrayRef size, std::optional<torch::headeronly::ScalarType> dtype = std::nullopt, std::optional<torch::headeronly::Layout> layout = std::nullopt, std::optional<torch::stable::Device> device = std::nullopt, std::optional<bool> pin_memory = std::nullopt)

Stable version of the new_empty op (2.10 version with full kwargs).

Creates a new uninitialized tensor with the specified size and options. This version supports all tensor creation kwargs. For versions < 2.10, a simpler overload that only takes dtype is available.

Minimum compatible version: PyTorch 2.10.

Parameters:

- **self** - The input tensor whose properties may be inherited if kwargs are not provided.
- **size** - The desired size of the output tensor.
- **dtype** - Optional scalar type for the tensor elements.
- **layout** - Optional memory layout (e.g., strided, sparse).
- **device** - Optional device to place the tensor on.
- **pin_memory** - Optional flag to use pinned memory (for CUDA tensors).

Returns:

A new uninitialized tensor with the specified properties.

inline torch::stable::Tensor torch::stable::new_zeros(const torch::stable::Tensor &self, torch::headeronly::IntHeaderOnlyArrayRef size, std::optional<torch::headeronly::ScalarType> dtype = std::nullopt, std::optional<torch::headeronly::Layout> layout = std::nullopt, std::optional<torch::stable::Device> device = std::nullopt, std::optional<bool> pin_memory = std::nullopt)

Stable version of the new_zeros op (2.10 version with full kwargs).

Creates a new zero-filled tensor with the specified size and options. This version supports all tensor creation kwargs. For versions < 2.10, a simpler overload that only takes dtype is available.

Minimum compatible version: PyTorch 2.10.

Parameters:

- **self** - The input tensor whose properties may be inherited if kwargs are not provided.
- **size** - The desired size of the output tensor.
- **dtype** - Optional scalar type for the tensor elements.
- **layout** - Optional memory layout (e.g., strided, sparse).
- **device** - Optional device to place the tensor on.
- **pin_memory** - Optional flag to use pinned memory (for CUDA tensors).

Returns:

A new zero-filled tensor with the specified properties.

inline torch::stable::Tensor torch::stable::full(torch::headeronly::IntHeaderOnlyArrayRef size, double fill_value, std::optional<torch::headeronly::ScalarType> dtype = std::nullopt, std::optional<torch::headeronly::Layout> layout = std::nullopt, std::optional<torch::stable::Device> device = std::nullopt, std::optional<bool> pin_memory = std::nullopt)

Stable version of the full.default op.

Creates a tensor of the specified size filled with the given value.

Minimum compatible version: PyTorch 2.10.

Note

The fill_value parameter is typed C shim API uses double for the Scalar parameter.

Parameters:

- **size** - The desired size of the output tensor.
- **fill_value** - The value to fill the tensor with.
- **dtype** - Optional scalar type for the tensor elements.
- **layout** - Optional memory layout.
- **device** - Optional device to place the tensor on.
- **pin_memory** - Optional flag to use pinned memory.

Returns:

A new tensor filled with the specified value.

inline torch::stable::Tensor torch::stable::from_blob(void *data, torch::headeronly::IntHeaderOnlyArrayRef sizes, torch::headeronly::IntHeaderOnlyArrayRef strides, torch::stable::Device device, torch::headeronly::ScalarType dtype, int64_t storage_offset = 0, torch::headeronly::Layout layout = torch::headeronly::Layout::Strided)

Creates a tensor from an existing data blob.

Creates a tensor that uses the provided data pointer as its storage. The tensor does not own the data, so the caller must ensure the data remains valid for the lifetime of the tensor.

Minimum compatible version: PyTorch 2.10.

Parameters:

- **data** - Pointer to the data buffer.
- **sizes** - The size of each dimension of the tensor.
- **strides** - The stride for each dimension.
- **device** - The device where the data resides.
- **dtype** - The scalar type of the data.
- **storage_offset** - The offset into the data buffer. Defaults to 0.
- **layout** - The memory layout. Defaults to Strided.

Returns:

A tensor backed by the provided data.

**Example:**

```
auto tensor = torch::stable::empty(
 {3, 4},
 torch::headeronly::ScalarType::Float,
 torch::headeronly::Layout::Strided,
 torch::stable::Device(torch::headeronly::DeviceType::CUDA, 0),
 false,
 torch::headeronly::MemoryFormat::Contiguous);
```

## Tensor Manipulation

inline torch::stable::Tensor torch::stable::clone(const torch::stable::Tensor &self)

Stable version of the clone op.

Returns a copy of the input tensor. The returned tensor has the same data and type as the input, but is stored in a new memory location.

Minimum compatible version: PyTorch 2.9.

Note

Optional memory_format kwarg support

Parameters:

**self** - The input tensor to clone.

Returns:

A new tensor with copied data.

inline torch::stable::Tensor torch::stable::contiguous(const torch::stable::Tensor &self, torch::headeronly::MemoryFormat memory_format = torch::headeronly::MemoryFormat::Contiguous)

Stable version of the contiguous op.

Returns a contiguous in memory tensor containing the same data as the input tensor. If the input tensor is already contiguous in the specified memory format, the input tensor is returned.

Minimum compatible version: PyTorch 2.10.

Parameters:

- **self** - The input tensor.
- **memory_format** - The desired memory format.

Returns:

A contiguous tensor.

inline torch::stable::Tensor torch::stable::reshape(const torch::stable::Tensor &self, torch::headeronly::IntHeaderOnlyArrayRef shape)

Stable version of the reshape op.

Returns a tensor with the same data and number of elements as the input, but with the specified shape. When possible, the returned tensor will be a view of the input.

Minimum compatible version: PyTorch 2.10.

Parameters:

- **self** - The input tensor.
- **shape** - The desired output shape.

Returns:

A tensor with the specified shape.

inline torch::stable::Tensor torch::stable::view(const torch::stable::Tensor &self, torch::headeronly::IntHeaderOnlyArrayRef size)

Stable version of the view op.

Returns a new tensor with the same data as the input tensor but with a different shape. The returned tensor shares the same data and must have the same number of elements.

Minimum compatible version: PyTorch 2.10.

Parameters:

- **self** - The input tensor.
- **size** - The desired output shape.

Returns:

A view tensor with the specified shape.

inline torch::stable::Tensor torch::stable::flatten(const torch::stable::Tensor &self, int64_t start_dim = 0, int64_t end_dim = -1)

Stable version of the flatten.using_ints op.

Flattens the input tensor by reshaping it into a one-dimensional tensor. If start_dim or end_dim are specified, only dimensions starting from start_dim to end_dim are flattened.

Minimum compatible version: PyTorch 2.9.

Parameters:

- **self** - The input tensor to flatten.
- **start_dim** - The first dimension to flatten. Defaults to 0.
- **end_dim** - The last dimension to flatten. Defaults to -1 (last dim).

Returns:

A flattened tensor.

inline torch::stable::Tensor torch::stable::squeeze(const torch::stable::Tensor &self, int64_t dim)

Stable version of the squeeze.dim op.

Returns a tensor with the dimension of size one at the specified position removed. The returned tensor shares the same underlying data with the input tensor.

Minimum compatible version: PyTorch 2.9.

Parameters:

- **self** - The input tensor.
- **dim** - The dimension to squeeze. the tensor is returned unchanged.

Returns:

A tensor with the specified dimension removed (if size was 1).

inline torch::stable::Tensor torch::stable::unsqueeze(const torch::stable::Tensor &self, int64_t dim)

Stable version of the unsqueeze op.

Returns a new tensor with a dimension of size one inserted at the specified position. The returned tensor shares the same underlying data with the input tensor.

Minimum compatible version: PyTorch 2.9.

Parameters:

- **self** - The input tensor.
- **dim** - The index at which to insert values are supported.

Returns:

A tensor with an additional dimension.

inline torch::stable::Tensor torch::stable::transpose(const torch::stable::Tensor &self, int64_t dim0, int64_t dim1)

Stable version of the transpose.int op.

Returns a tensor that is a transposed version of the input, with dimensions dim0 and dim1 swapped. The returned tensor shares storage with the input.

Minimum compatible version: PyTorch 2.9.

Parameters:

- **self** - The input tensor.
- **dim0** - The first dimension to transpose.
- **dim1** - The second dimension to transpose.

Returns:

A transposed view of the input tensor.

inline torch::stable::Tensor torch::stable::select(const torch::stable::Tensor &self, int64_t dim, int64_t index)

Stable version of the select.int op.

Slices the input tensor along the specified dimension at the given index. This function returns a view of the original tensor with the given dimension removed.

Minimum compatible version: PyTorch 2.9.

Note

The index parameter is typed header-only.

Parameters:

- **self** - The input tensor.
- **dim** - The dimension to slice.
- **index** - The index to select along the dimension.

Returns:

A tensor with one fewer dimension.

inline torch::stable::Tensor torch::stable::narrow(torch::stable::Tensor &self, int64_t dim, int64_t start, int64_t length)

Stable version of the narrow.default op.

Returns a new tensor that is a narrowed version of the input tensor. The dimension dim is narrowed from start to start + length.

Minimum compatible version: PyTorch 2.9.

Note

The start and length parameters is not yet header-only.

Parameters:

- **self** - The input tensor to narrow.
- **dim** - The dimension along which to narrow.
- **start** - The starting index for the narrowed dimension.
- **length** - The length of the narrowed dimension.

Returns:

A new tensor that is a narrowed view of the input.

inline torch::stable::Tensor torch::stable::pad(const torch::stable::Tensor &self, torch::headeronly::IntHeaderOnlyArrayRef pad, const std::string &mode = "constant", double value = 0.0)

Stable version of the pad.default op.

Pads the input tensor according to the specified padding sizes. The padding is applied symmetrically to each dimension, with the padding sizes specified in reverse order (last dimension first).

Minimum compatible version: PyTorch 2.9.

Note

The pad parameter is typed not yet header-only.

Parameters:

- **self** - The input tensor to pad.
- **pad** - The padding sizes for each dimension (in pairs, starting from the last dimension).
- **mode** - The padding mode: "constant", "reflect", "replicate", or "circular". Defaults to "constant".
- **value** - The fill value for constant padding. Defaults to 0.0.

Returns:

A new padded tensor.

## Device and Type Conversion

inline torch::stable::Tensor torch::stable::to(const torch::stable::Tensor &self, std::optional<torch::headeronly::ScalarType> dtype = std::nullopt, std::optional<torch::headeronly::Layout> layout = std::nullopt, std::optional<torch::stable::Device> device = std::nullopt, std::optional<bool> pin_memory = std::nullopt, bool non_blocking = false, bool copy = false, std::optional<torch::headeronly::MemoryFormat> memory_format = std::nullopt)

Stable version of the to.dtype_layout op.

Converts a tensor to the specified dtype, layout, device, and/or memory format. Returns a new tensor with the specified properties.

Minimum compatible version: PyTorch 2.10.

Parameters:

- **self** - The input tensor.
- **dtype** - Optional target scalar type.
- **layout** - Optional target memory layout.
- **device** - Optional target device.
- **pin_memory** - Optional flag to use pinned memory.
- **non_blocking** - If true, the operation may be asynchronous. Defaults to false.
- **copy** - If true, always create a copy. Defaults to false.
- **memory_format** - Optional target memory format.

Returns:

A tensor with the specified properties.

inline torch::stable::Tensor torch::stable::to(const torch::stable::Tensor &self, torch::stable::Device device, bool non_blocking = false, bool copy = false)

Convenience overload for moving a tensor to a device.

Moves the tensor to the specified device. This is a convenience wrapper around the full to() function.

Minimum compatible version: PyTorch 2.10.

Parameters:

- **self** - The input tensor.
- **device** - The target device.
- **non_blocking** - If true, the operation may be asynchronous. Defaults to false.
- **copy** - If true, always create a copy. Defaults to false.

Returns:

A tensor on the specified device.

## In-place Operations

inline torch::stable::Tensor torch::stable::fill_(const torch::stable::Tensor &self, double value)

Stable version of the fill_.Scalar op.

Fills the input tensor with the specified scalar value in-place and returns it. This has identical semantics to the existing fill_.Scalar op.

Minimum compatible version: PyTorch 2.9.

Note

The value parameter is typed as double This is because Scalar.h is currently not header-only.

Parameters:

- **self** - The tensor to fill.
- **value** - The scalar value to fill the tensor with.

Returns:

The input tensor, now filled with the specified value.

inline torch::stable::Tensor torch::stable::zero_(torch::stable::Tensor &self)

Stable version of the zero_ op.

Fills the input tensor with zeros in-place and returns it. Unlike the tensor method version (t.zero_()), this is called as a function: zero_(t).

Minimum compatible version: PyTorch 2.9.

Parameters:

**self** - The tensor to fill with zeros.

Returns:

The input tensor, now filled with zeros.

inline torch::stable::Tensor torch::stable::copy_(torch::stable::Tensor &self, const torch::stable::Tensor &src, std::optional<bool> non_blocking = std::nullopt)

Stable version of the copy_ op.

Copies the elements from the source tensor into the destination tensor in-place and returns the destination tensor. The tensors must be broadcastable.

Minimum compatible version: PyTorch 2.9.

Parameters:

- **self** - The destination tensor (modified in-place).
- **src** - The source tensor to copy from.
- **non_blocking** - If true, the copy may occur asynchronously with respect to the host. Defaults to false.

Returns:

The destination tensor with copied values.

## Mathematical Operations

inline torch::stable::Tensor torch::stable::matmul(const torch::stable::Tensor &self, const torch::stable::Tensor &other)

Stable version of the matmul op.

Performs matrix multiplication between two tensors. The behavior depends on the dimensionality of the tensors (see PyTorch documentation for details on broadcasting rules for matmul).

Minimum compatible version: PyTorch 2.9.

Parameters:

- **self** - The first input tensor.
- **other** - The second input tensor.

Returns:

The result of matrix multiplication.

inline torch::stable::Tensor torch::stable::amax(const torch::stable::Tensor &self, int64_t dim, bool keepdim = false)

Stable version of the amax.default op (single dimension).

Computes the maximum value along the specified dimension. If keepdim is true, the output tensor has the same number of dimensions as the input, with the reduced dimension having size 1. Otherwise, the reduced dimension is removed.

Minimum compatible version: PyTorch 2.9.

Parameters:

- **self** - The input tensor.
- **dim** - The dimension along which to compute the maximum.
- **keepdim** - Whether to retain

Returns:

A tensor containing the maximum values along the specified dimension.

inline torch::stable::Tensor torch::stable::amax(const torch::stable::Tensor &self, torch::headeronly::IntHeaderOnlyArrayRef dims, bool keepdim = false)

Stable version of the amax.default op (multiple dimensions).

Computes the maximum value reducing over all the specified dimensions. If keepdim is true, the output tensor has the same number of dimensions as the input, with the reduced dimensions having size 1. Otherwise, the reduced dimensions are removed.

Minimum compatible version: PyTorch 2.9.

Note

The dims parameter is typed is not yet header-only.

Parameters:

- **self** - The input tensor.
- **dims** - The dimensions along which to compute the maximum.
- **keepdim** - Whether to retain the reduced dimensions. Defaults to false.

Returns:

A tensor containing the maximum values.

inline torch::stable::Tensor torch::stable::sum(const torch::stable::Tensor &self, std::optional<torch::headeronly::IntHeaderOnlyArrayRef> dim = std::nullopt, bool keepdim = false, std::optional<torch::headeronly::ScalarType> dtype = std::nullopt)

Stable version of the sum.dim_IntList op.

Computes the sum of the input tensor along the specified dimensions. If dim is not provided, sums over all dimensions.

Minimum compatible version: PyTorch 2.10.

Parameters:

- **self** - The input tensor.
- **dim** - Optional dimensions to reduce. If not provided, reduces all dimensions.
- **keepdim** - Whether to retain the reduced dimensions. Defaults to false.
- **dtype** - Optional output dtype. If not provided, uses the input dtype.

Returns:

A tensor containing the sum.

inline torch::stable::Tensor &torch::stable::sum_out(torch::stable::Tensor &out, const torch::stable::Tensor &self, std::optional<torch::headeronly::IntHeaderOnlyArrayRef> dim = std::nullopt, bool keepdim = false, std::optional<torch::headeronly::ScalarType> dtype = std::nullopt)

Stable version of the sum.IntList_out op.

Computes the sum of the input tensor along the specified dimensions, storing the result in the provided output tensor. Following C++ convention, the out parameter comes first.

Minimum compatible version: PyTorch 2.10.

Parameters:

- **out** - The output tensor (modified in-place).
- **self** - The input tensor.
- **dim** - Optional dimensions to reduce.
- **keepdim** - Whether to retain the reduced dimensions. Defaults to false.
- **dtype** - Optional output dtype.

Returns:

Reference to the output tensor.

inline torch::stable::Tensor torch::stable::subtract(const torch::stable::Tensor &self, const torch::stable::Tensor &other, double alpha = 1.0)

Stable version of the subtract.Tensor op.

Subtracts the other tensor from self, with an optional scaling factor alpha. Computes: self - alpha * other.

Minimum compatible version: PyTorch 2.10.

Note

The alpha parameter is typed as double API uses double for the Scalar parameter.

Parameters:

- **self** - The input tensor.
- **other** - The tensor to subtract.
- **alpha** - The scaling factor for other. Defaults to 1.0.

Returns:

The result of self - alpha * other.