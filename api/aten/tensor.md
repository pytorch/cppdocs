# Tensor Class

The `at::Tensor` class is the primary tensor class in ATen, representing
a multi-dimensional array with a specific data type and device.

## Tensor

class at::Tensor

The primary tensor class in ATen. Represents a multi-dimensional array
with a specific data type and device.

Tensor()

Default constructor. Creates an undefined tensor.

int64_t dim() const

Returns the number of dimensions of the tensor.

int64_t size(int64_t dim) const

Returns the size of the tensor at the given dimension.

IntArrayRef sizes() const

Returns the sizes of all dimensions.

IntArrayRef strides() const

Returns the strides of all dimensions.

ScalarType scalar_type() const

Returns the data type of the tensor.

Device device() const

Returns the device where the tensor is stored.

bool is_cuda() const

Returns true if the tensor is on a CUDA device.

bool is_cpu() const

Returns true if the tensor is on CPU.

bool requires_grad() const

Returns true if gradients need to be computed for this tensor.

Tensor &requires_grad_(bool requires_grad = true)

Sets whether gradients should be computed for this tensor.

Tensor to(Device device) const

Returns a tensor on the specified device.

Tensor to(ScalarType dtype) const

Returns a tensor with the specified data type.

Tensor contiguous() const

Returns a contiguous tensor with the same data.

void *data_ptr() const

Returns a pointer to the underlying data.

**Example:**

```
#include <ATen/ATen.h>

at::Tensor a = at::ones({2, 2}, at::kInt);
at::Tensor b = at::randn({2, 2});
auto c = a + b.to(at::kInt);
```

## TensorOptions

class at::TensorOptions

A class to specify options for tensor creation, including dtype, device,
layout, and requires_grad.

TensorOptions()

Default constructor.

TensorOptions dtype(ScalarType dtype) const

Returns options with the specified data type.

TensorOptions device(Device device) const

Returns options with the specified device.

TensorOptions layout(Layout layout) const

Returns options with the specified layout.

TensorOptions requires_grad(bool requires_grad) const

Returns options with the specified requires_grad setting.

**Example:**

```
auto options = at::TensorOptions()
 .dtype(at::kFloat)
 .device(at::kCUDA, 0)
 .requires_grad(true);

at::Tensor tensor = at::zeros({3, 4}, options);
```

## Scalar

class at::Scalar

Represents a scalar value that can be converted to various numeric types.

Scalar(int64_t v)

Construct from an integer.

Scalar(double v)

Construct from a double.

template<typename T>
T to() const

Convert to the specified type.

bool isIntegral(bool includeBool = false) const

Returns true if the scalar is an integral type.

bool isFloatingPoint() const

Returns true if the scalar is a floating point type.

## ScalarType

enum class at::ScalarType

Enumeration of data types supported by tensors.

enumerator Byte

8-bit unsigned integer (uint8_t)

enumerator Char

8-bit signed integer (int8_t)

enumerator Short

16-bit signed integer (int16_t)

enumerator Int

32-bit signed integer (int32_t)

enumerator Long

64-bit signed integer (int64_t)

enumerator Half

16-bit floating point (float16)

enumerator Float

32-bit floating point (float)

enumerator Double

64-bit floating point (double)

enumerator Bool

Boolean type

enumerator BFloat16

Brain floating point (bfloat16)

Convenience constants:

- `at::kByte`, `at::kChar`, `at::kShort`, `at::kInt`, `at::kLong`
- `at::kHalf`, `at::kFloat`, `at::kDouble`, `at::kBFloat16`
- `at::kBool`

## DeviceGuard

class DeviceGuard

RAII guard that sets a certain default device in its constructor, and changes it back to the device that was originally active upon destruction.

The device is always reset to the one that was active at the time of construction of the guard. Even if you `set_device` after construction, the destructor will still reset the device to the one that was active at construction time.

This device guard does NOT have an uninitialized state; it is guaranteed to reset a device on exit. If you are in a situation where you *might* want to setup a guard (i.e., are looking for the moral equivalent of std::optional<DeviceGuard>), see [OptionalDeviceGuard](../c10/guards.html#PyTorchclassc10_1_1_optional_device_guard).

Public Functions

explicit DeviceGuard() = delete

No default constructor; see Note [Omitted default constructor from RAII].

inline explicit DeviceGuard([Device](../c10/device.html#_CPPv4N3c106DeviceE) device)

Set the current device to the passed [Device](../c10/device.html#PyTorchstructc10_1_1_device).

inline explicit DeviceGuard([Device](../c10/device.html#_CPPv4N3c106DeviceE) device, const impl::DeviceGuardImplInterface *impl)

This constructor is for testing only.

~DeviceGuard() = default

DeviceGuard(const DeviceGuard&) = delete

Copy is disallowed.

DeviceGuard &operator=(const DeviceGuard&) = delete

DeviceGuard(DeviceGuard &&other) = delete

Move is disallowed, as [DeviceGuard](../c10/guards.html#PyTorchclassc10_1_1_device_guard) does not have an uninitialized state, which is required for moves on types with nontrivial destructors.

DeviceGuard &operator=(DeviceGuard &&other) = delete

inline void reset_device(at::Device device)

Sets the device to the given one.

The specified device must be consistent with the device type originally specified during guard construction.

TODO: The consistency check here is inconsistent with StreamGuard's behavior with set_stream, where a stream on a different device than the original one isn't an error; we just reset the stream and then switch devices.

inline void reset_device(at::Device device, const impl::DeviceGuardImplInterface *impl)

This method is for testing only.

inline void set_index(DeviceIndex index)

Sets the device index to the given one.

The device type is inferred from the original device type the guard was constructed with.

inline [Device](../c10/device.html#_CPPv4N3c106DeviceE) original_device() const

Returns the device that was set at the time the guard was constructed.

inline [Device](../c10/device.html#_CPPv4N3c106DeviceE) current_device() const

Returns the most recent device that was set using this device guard, either from construction, or via set_device.

**Example:**

```
{
 c10::DeviceGuard guard(at::Device(at::kCUDA, 1));
 // Operations here run on CUDA device 1
 auto tensor = at::zeros({2, 2});
}
// Previous device is restored
```

## Layout

enum class at::Layout

Enumeration of tensor memory layouts.

enumerator Strided

Dense tensor with strides.

enumerator Sparse

Sparse tensor (COO format).

enumerator SparseCsr

Sparse tensor in CSR format.

enumerator SparseCsc

Sparse tensor in CSC format.