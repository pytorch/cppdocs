# Device Guards

C10 provides device-agnostic RAII guards for managing the current device
context. These guards work across all backends (CUDA, XPU, etc.) and
automatically restore the previous device when they go out of scope.

For backend-specific guards, see [CUDA Guards](../cuda/guards.html) and [XPU Support](../xpu/index.html).

## DeviceGuard

class DeviceGuard

RAII guard that sets a certain default device in its constructor, and changes it back to the device that was originally active upon destruction.

The device is always reset to the one that was active at the time of construction of the guard. Even if you `set_device` after construction, the destructor will still reset the device to the one that was active at construction time.

This device guard does NOT have an uninitialized state; it is guaranteed to reset a device on exit. If you are in a situation where you *might* want to setup a guard (i.e., are looking for the moral equivalent of std::optional<DeviceGuard>), see OptionalDeviceGuard.

Public Functions

explicit DeviceGuard() = delete

No default constructor; see Note [Omitted default constructor from RAII].

inline explicit DeviceGuard([Device](device.html#_CPPv4N3c106DeviceE) device)

Set the current device to the passed [Device](device.html#PyTorchstructc10_1_1_device).

inline explicit DeviceGuard([Device](device.html#_CPPv4N3c106DeviceE) device, const impl::DeviceGuardImplInterface *impl)

This constructor is for testing only.

~DeviceGuard() = default

DeviceGuard(const DeviceGuard&) = delete

Copy is disallowed.

DeviceGuard &operator=(const DeviceGuard&) = delete

DeviceGuard(DeviceGuard &&other) = delete

Move is disallowed, as [DeviceGuard](../aten/tensor.html#PyTorchclassc10_1_1_device_guard) does not have an uninitialized state, which is required for moves on types with nontrivial destructors.

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

inline [Device](device.html#_CPPv4N3c106DeviceE) original_device() const

Returns the device that was set at the time the guard was constructed.

inline [Device](device.html#_CPPv4N3c106DeviceE) current_device() const

Returns the most recent device that was set using this device guard, either from construction, or via set_device.

**Example:**

```
#include <c10/core/DeviceGuard.h>

{
 c10::DeviceGuard guard(c10::Device(c10::kCUDA, 1));
 // All operations here run on CUDA device 1
}
// Previous device is restored
```

## OptionalDeviceGuard

class OptionalDeviceGuard

A OptionalDeviceGuard is an RAII class that sets a device to some value on initialization, and resets the device to its original value on destruction.

Morally, a OptionalDeviceGuard is equivalent to std::optional<DeviceGuard>, but with extra constructors and methods as appropriate.

Besides its obvious use (optionally applying a [DeviceGuard](../aten/tensor.html#PyTorchclassc10_1_1_device_guard)), OptionalDeviceGuard is often also used for the following idiom:

OptionalDeviceGuard g; for (const auto& t : tensors) { g.set_device(t.device()); do_something_with(t); }

This usage is marginally more efficient than constructing a [DeviceGuard](../aten/tensor.html#PyTorchclassc10_1_1_device_guard) every iteration of the for loop, as it avoids an unnecessary device reset.

Unlike [DeviceGuard](../aten/tensor.html#PyTorchclassc10_1_1_device_guard), a OptionalDeviceGuard may be uninitialized. This occurs when you use the nullary constructor, or pass a nullopt to the constructor. Uninitialized OptionalDeviceGuards do *nothing*; they do not know what the original device was and they do not reset on destruction. This is why original_device() and current_device() return std::optional<Device> rather than [Device](device.html#PyTorchstructc10_1_1_device) (as they do in [DeviceGuard](../aten/tensor.html#PyTorchclassc10_1_1_device_guard)), and also is why we didn't just provide OptionalDeviceGuard by default and hide [DeviceGuard](../aten/tensor.html#PyTorchclassc10_1_1_device_guard) from users.

The semantics of an OptionalDeviceGuard are exactly explained by thinking of it as an std::optional<DeviceGuard>. In particular, an initialized OptionalDeviceGuard doesn't restore device to its value at construction; it restores device to its value *at initialization*. So if you have the program:

```
setDevice(1);
OptionalDeviceGuard g;
setDevice(2);
g.reset_device(Device(DeviceType::CUDA, 3)); // initializes!
```

 On destruction, g will reset device to 2, rather than 1.

An uninitialized OptionalDeviceGuard is distinct from a (initialized) [DeviceGuard](../aten/tensor.html#PyTorchclassc10_1_1_device_guard) whose original_device_ and current_device_ match, since the [DeviceGuard](../aten/tensor.html#PyTorchclassc10_1_1_device_guard) will still reset the device to original_device_.

Public Functions

explicit OptionalDeviceGuard() = default

Create an uninitialized guard. Set the guard later using reset_device.

inline explicit OptionalDeviceGuard([Device](device.html#_CPPv4N3c106DeviceE) device)

Initialize the guard, setting the current device to the passed [Device](device.html#PyTorchstructc10_1_1_device).

inline explicit OptionalDeviceGuard(std::optional<[Device](device.html#_CPPv4N3c106DeviceE)> device)

Initialize the guard if a [Device](device.html#PyTorchstructc10_1_1_device) is passed; otherwise leave the guard uninitialized.

inline explicit OptionalDeviceGuard([Device](device.html#_CPPv4N3c106DeviceE) device, const impl::DeviceGuardImplInterface *impl)

Constructor for testing only.

~OptionalDeviceGuard() = default

OptionalDeviceGuard(const OptionalDeviceGuard&) = delete

Copy is disallowed.

OptionalDeviceGuard &operator=(const OptionalDeviceGuard&) = delete

OptionalDeviceGuard(OptionalDeviceGuard &&other) = delete

Move is disallowed See Note [Explicit initialization of optional fields] and // Note [Move construction for RAII guards is tricky] for rationale.

OptionalDeviceGuard &operator=(OptionalDeviceGuard &&other) = delete

inline void reset_device(at::Device device)

Sets the device to the given one.

The specified device must be consistent with the device type originally specified during guard construction.

inline void reset_device(at::Device device, const impl::DeviceGuardImplInterface *impl)

For testing only.

inline std::optional<[Device](device.html#_CPPv4N3c106DeviceE)> original_device() const

Returns the device that was set at the time the guard was constructed.

inline std::optional<[Device](device.html#_CPPv4N3c106DeviceE)> current_device() const

Returns the most recent device that was set using this device guard, either from construction, or via reset_device.

**Example:**

```
#include <c10/core/DeviceGuard.h>

c10::OptionalDeviceGuard guard;
if (use_gpu) {
 guard.reset_device(c10::Device(c10::kCUDA, 0));
}
// Guard only restores device if it was set
```