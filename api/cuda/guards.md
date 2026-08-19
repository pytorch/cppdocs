# CUDA Guards

CUDA guards are RAII wrappers that set a CUDA device or stream as the current
context and automatically restore the previous context when the guard goes
out of scope.

## CUDAGuard

struct CUDAGuard

A variant of [DeviceGuard](../c10/guards.html#PyTorchclassc10_1_1_device_guard) that is specialized for CUDA.

It accepts integer indices (interpreting them as CUDA devices) and is a little more efficient than [DeviceGuard](../c10/guards.html#PyTorchclassc10_1_1_device_guard) (it compiles to straight line cudaSetDevice/cudaGetDevice calls); however, it can only be used from code that links against CUDA directly.

Public Functions

explicit CUDAGuard() = delete

No default constructor; see Note [Omitted default constructor from RAII].

inline explicit CUDAGuard(DeviceIndex device_index)

Set the current CUDA device to the passed device index.

inline explicit CUDAGuard([Device](../c10/device.html#_CPPv4N3c106DeviceE) device)

Sets the current CUDA device to the passed device.

Errors if the passed device is not a CUDA device.

CUDAGuard(const CUDAGuard&) = delete

CUDAGuard &operator=(const CUDAGuard&) = delete

CUDAGuard(CUDAGuard &&other) = delete

CUDAGuard &operator=(CUDAGuard &&other) = delete

~CUDAGuard() = default

inline void set_device([Device](../c10/device.html#_CPPv4N3c106DeviceE) device)

Sets the CUDA device to the given device.

Errors if the given device is not a CUDA device.

inline void reset_device([Device](../c10/device.html#_CPPv4N3c106DeviceE) device)

Sets the CUDA device to the given device.

Errors if the given device is not a CUDA device. (This method is provided for uniformity with [DeviceGuard](../c10/guards.html#PyTorchclassc10_1_1_device_guard)).

inline void set_index(DeviceIndex device_index)

Sets the CUDA device to the given device index.

inline [Device](../c10/device.html#_CPPv4N3c106DeviceE) original_device() const

Returns the device that was set upon construction of the guard.

inline [Device](../c10/device.html#_CPPv4N3c106DeviceE) current_device() const

Returns the last device that was set via `set_device`, if any, otherwise the device passed during construction.

**Example:**

```
#include <c10/cuda/CUDAGuard.h>

{
 c10::cuda::CUDAGuard guard(1); // Switch to device 1
 // All CUDA operations here run on device 1
 auto tensor = torch::zeros({2, 2}, torch::device(torch::kCUDA));
}
// Previous device is restored
```

## CUDAStreamGuard

struct CUDAStreamGuard

A variant of StreamGuard that is specialized for CUDA.

See CUDAGuard for when you can use this.

Public Functions

explicit CUDAStreamGuard() = delete

No default constructor, see Note [Omitted default constructor from RAII].

inline explicit CUDAStreamGuard([Stream](../c10/streams.html#_CPPv4N3c106StreamE) stream)

Set the current CUDA device to the device associated with the passed stream, and set the current CUDA stream on that device to the passed stream.

Errors if the [Stream](../c10/streams.html#PyTorchclassc10_1_1_stream) is not a CUDA stream.

~CUDAStreamGuard() = default

CUDAStreamGuard(const CUDAStreamGuard&) = delete

Copy is disallowed.

CUDAStreamGuard &operator=(const CUDAStreamGuard&) = delete

CUDAStreamGuard(CUDAStreamGuard &&other) = delete

Move is disallowed, as CUDAStreamGuard does not have an uninitialized state, which is required for moves on types with nontrivial destructors.

CUDAStreamGuard &operator=(CUDAStreamGuard &&other) = delete

inline void reset_stream([Stream](../c10/streams.html#_CPPv4N3c106StreamE) stream)

Resets the currently set stream to the original stream and the currently set device to the original device.

Then, set the current device to the device associated with the passed stream, and set the current stream on that device to the passed stream. Errors if the stream passed is not a CUDA stream.

NOTE: this implementation may skip some stream/device setting if it can prove that it is unnecessary.

WARNING: reset_stream does NOT preserve previously set streams on different devices. If you need to set streams on multiple devices on CUDA, use CUDAMultiStreamGuard instead.

inline [CUDAStream](streams.html#_CPPv4N3c104cuda10CUDAStreamE) original_stream() const

Returns the CUDA stream that was set at the time the guard was constructed.

inline [CUDAStream](streams.html#_CPPv4N3c104cuda10CUDAStreamE) current_stream() const

Returns the most recent CUDA stream that was set using this device guard, either from construction, or via set_stream.

inline [Device](../c10/device.html#_CPPv4N3c106DeviceE) current_device() const

Returns the most recent CUDA device that was set using this device guard, either from construction, or via set_device/reset_device/set_index.

inline [Device](../c10/device.html#_CPPv4N3c106DeviceE) original_device() const

Returns the CUDA device that was set at the most recent reset_stream(), or otherwise the device at construction time.

**Example:**

```
#include <c10/cuda/CUDAGuard.h>

auto stream = c10::cuda::getStreamFromPool();
{
 c10::cuda::CUDAStreamGuard guard(stream);
 // Operations here use the specified stream
}
// Previous stream is restored
```

## OptionalCUDAGuard

struct OptionalCUDAGuard

A variant of [OptionalDeviceGuard](../c10/guards.html#PyTorchclassc10_1_1_optional_device_guard) that is specialized for CUDA.

See CUDAGuard for when you can use this.

Public Functions

explicit OptionalCUDAGuard() = default

Create an uninitialized OptionalCUDAGuard.

inline explicit OptionalCUDAGuard(std::optional<[Device](../c10/device.html#_CPPv4N3c106DeviceE)> device_opt)

Set the current CUDA device to the passed [Device](../c10/device.html#PyTorchstructc10_1_1_device), if it is not nullopt.

inline explicit OptionalCUDAGuard(std::optional<DeviceIndex> device_index_opt)

Set the current CUDA device to the passed device index, if it is not nullopt.

OptionalCUDAGuard(const OptionalCUDAGuard&) = delete

OptionalCUDAGuard &operator=(const OptionalCUDAGuard&) = delete

OptionalCUDAGuard(OptionalCUDAGuard &&other) = delete

OptionalCUDAGuard &operator=(OptionalCUDAGuard &&other) = delete

~OptionalCUDAGuard() = default

inline void set_device([Device](../c10/device.html#_CPPv4N3c106DeviceE) device)

Sets the CUDA device to the given device, initializing the guard if it is not already initialized.

Errors if the given device is not a CUDA device.

inline void reset_device([Device](../c10/device.html#_CPPv4N3c106DeviceE) device)

Sets the CUDA device to the given device, initializing the guard if it is not already initialized.

Errors if the given device is not a CUDA device. (This method is provided for uniformity with [OptionalDeviceGuard](../c10/guards.html#PyTorchclassc10_1_1_optional_device_guard)).

inline void set_index(DeviceIndex device_index)

Sets the CUDA device to the given device index, initializing the guard if it is not already initialized.

inline std::optional<[Device](../c10/device.html#_CPPv4N3c106DeviceE)> original_device() const

Returns the device that was set immediately prior to initialization of the guard, or nullopt if the guard is uninitialized.

inline std::optional<[Device](../c10/device.html#_CPPv4N3c106DeviceE)> current_device() const

Returns the most recent device that was set using this device guard, either from construction, or via set_device, if the guard is initialized, or nullopt if the guard is uninitialized.

inline void reset()

Restore the original CUDA device, resetting this guard to uninitialized state.

**Example:**

```
c10::cuda::OptionalCUDAGuard guard;
if (use_cuda) {
 guard.set_device(0);
}
// Guard only switches device if set_device was called
```

## OptionalCUDAStreamGuard

struct OptionalCUDAStreamGuard

A variant of OptionalStreamGuard that is specialized for CUDA.

See CUDAGuard for when you can use this.

Public Functions

explicit OptionalCUDAStreamGuard() = default

Create an uninitialized guard.

inline explicit OptionalCUDAStreamGuard([Stream](../c10/streams.html#_CPPv4N3c106StreamE) stream)

Set the current CUDA device to the device associated with the passed stream, and set the current CUDA stream on that device to the passed stream.

Errors if the [Stream](../c10/streams.html#PyTorchclassc10_1_1_stream) is not a CUDA stream.

inline explicit OptionalCUDAStreamGuard(std::optional<[Stream](../c10/streams.html#_CPPv4N3c106StreamE)> stream_opt)

Set the current device to the device associated with the passed stream, and set the current stream on that device to the passed stream, if the passed stream is not nullopt.

OptionalCUDAStreamGuard(const OptionalCUDAStreamGuard&) = delete

Copy is disallowed.

OptionalCUDAStreamGuard &operator=(const OptionalCUDAStreamGuard&) = delete

OptionalCUDAStreamGuard(OptionalCUDAStreamGuard &&other) = delete

OptionalCUDAStreamGuard &operator=(OptionalCUDAStreamGuard &&other) = delete

~OptionalCUDAStreamGuard() = default

inline void reset_stream([Stream](../c10/streams.html#_CPPv4N3c106StreamE) stream)

Resets the currently set CUDA stream to the original stream and the currently set device to the original device.

Then, set the current device to the device associated with the passed stream, and set the current stream on that device to the passed stream. Initializes the guard if it was not previously initialized.

inline std::optional<[CUDAStream](streams.html#_CPPv4N3c104cuda10CUDAStreamE)> original_stream() const

Returns the CUDA stream that was set at the time the guard was most recently initialized, or nullopt if the guard is uninitialized.

inline std::optional<[CUDAStream](streams.html#_CPPv4N3c104cuda10CUDAStreamE)> current_stream() const

Returns the most recent CUDA stream that was set using this stream guard, either from construction, or via reset_stream, if the guard is initialized, or nullopt if the guard is uninitialized.

inline void reset()

Restore the original CUDA device and stream, resetting this guard to uninitialized state.

## CUDAMultiStreamGuard

struct CUDAMultiStreamGuard

A variant of MultiStreamGuard that is specialized for CUDA.

Public Functions

inline explicit CUDAMultiStreamGuard([ArrayRef](../c10/types.html#_CPPv4I0EN3c108ArrayRefE)<[CUDAStream](streams.html#_CPPv4N3c104cuda10CUDAStreamE)> streams)

CUDAMultiStreamGuard(const CUDAMultiStreamGuard&) = delete

Copy is disallowed.

CUDAMultiStreamGuard &operator=(const CUDAMultiStreamGuard&) = delete

CUDAMultiStreamGuard(CUDAMultiStreamGuard &&other) = delete

CUDAMultiStreamGuard &operator=(CUDAMultiStreamGuard &&other) = delete

~CUDAMultiStreamGuard() = default

**Example:**

```
at::cuda::CUDAStream stream0 = at::cuda::getStreamFromPool(false, 0);
at::cuda::CUDAStream stream1 = at::cuda::getStreamFromPool(false, 1);

{
 at::cuda::CUDAMultiStreamGuard multi_guard({stream0, stream1});
 // stream0 is current on device 0, stream1 on device 1
}
// Both streams restored
```