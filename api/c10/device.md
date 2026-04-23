# Device and DeviceType

PyTorch provides device abstractions for writing code that works across
CPU, CUDA, and other backends.

## Device

struct Device

Represents a compute device on which a tensor is located.

A device is uniquely identified by a type, which specifies the type of machine it is (e.g. CPU or CUDA GPU), and a device index or ordinal, which identifies the specific compute device when there is more than one of a certain type. The device index is optional, and in its defaulted state represents (abstractly) "the current device". Further, there are two constraints on the value of the device index, if one is explicitly stored:

1. A negative index represents the current device, a non-negative index represents a specific, concrete device,
2. When the device type is CPU, the device index must be zero.

Public Types

using Type = DeviceType

Public Functions

inline Device(DeviceType type, DeviceIndex index = -1)

Constructs a new `Device` from a `DeviceType` and an optional device index.

Device(const std::string &device_string)

Constructs a `Device` from a string description, for convenience.

The string supplied must follow the following schema: `(cpu|cuda)[:<device-index>]` where `cpu` or `cuda` specifies the device type, and `:<device-index>` optionally specifies a device index.

inline bool operator==(const Device &other) const noexcept

Returns true if the type and index of this `Device` matches that of `other`.

inline bool operator!=(const Device &other) const noexcept

Returns true if the type or index of this `Device` differs from that of `other`.

inline void set_index(DeviceIndex index)

Sets the device index.

inline DeviceType type() const noexcept

Returns the type of device this is.

inline DeviceIndex index() const noexcept

Returns the optional index.

inline bool has_index() const noexcept

Returns true if the device has a non-default index.

inline bool is_cuda() const noexcept

Return true if the device is of CUDA type.

inline bool is_privateuseone() const noexcept

Return true if the device is of PrivateUse1 type.

inline bool is_mps() const noexcept

Return true if the device is of MPS type.

inline bool is_hip() const noexcept

Return true if the device is of HIP type.

inline bool is_ve() const noexcept

Return true if the device is of VE type.

inline bool is_xpu() const noexcept

Return true if the device is of XPU type.

inline bool is_ipu() const noexcept

Return true if the device is of IPU type.

inline bool is_xla() const noexcept

Return true if the device is of XLA type.

inline bool is_mtia() const noexcept

Return true if the device is of MTIA type.

inline bool is_hpu() const noexcept

Return true if the device is of HPU type.

inline bool is_lazy() const noexcept

Return true if the device is of Lazy type.

inline bool is_vulkan() const noexcept

Return true if the device is of Vulkan type.

inline bool is_metal() const noexcept

Return true if the device is of Metal type.

inline bool is_maia() const noexcept

Return true if the device is of MAIA type.

inline bool is_meta() const noexcept

Return true if the device is of META type.

inline bool is_cpu() const noexcept

Return true if the device is of CPU type.

inline bool supports_as_strided() const noexcept

Return true if the device supports arbitrary strides.

std::string str() const

Same string as returned from operator<<.

**Example:**

```
c10::Device cpu_device(c10::kCPU);
c10::Device cuda_device(c10::kCUDA, 0); // CUDA device 0

if (cuda_device.is_cuda()) {
 std::cout << "Using CUDA device " << cuda_device.index() << std::endl;
}
```

## DeviceType

enum class c10::DeviceType

Enumeration of supported device types.

enumerator CPU = 0

CPU device.

enumerator CUDA = 1

NVIDIA CUDA GPU.

enumerator HIP = 6

AMD HIP GPU.

enumerator XLA = 9

XLA / TPU.

enumerator Vulkan = 10

Vulkan GPU.

enumerator Metal = 11

Apple Metal GPU.

enumerator XPU = 12

Intel XPU GPU.

enumerator MPS = 13

Apple Metal Performance Shaders.

enumerator Meta = 14

Meta tensors (shape only, no data).

enumerator HPU = 15

Habana HPU.

enumerator Lazy = 17

Lazy tensors.

enumerator IPU = 18

Graphcore IPU.

enumerator MTIA = 19

Meta training and inference accelerator.

enumerator PrivateUse1 = 20

Custom backend registered via `c10::register_privateuse1_backend()`.

Convenience constants:

- `c10::kCPU`, `c10::kCUDA`, `c10::kHIP`
- `c10::kXLA`, `c10::kVulkan`, `c10::kMetal`
- `c10::kXPU`, `c10::kMPS`, `c10::kMeta`
- `c10::kHPU`, `c10::kLazy`, `c10::kIPU`, `c10::kMTIA`
- `c10::kPrivateUse1`