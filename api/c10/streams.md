# Streams

`c10::Stream` is the device-agnostic base stream class. It provides a
common interface for working with streams across different backends
(CUDA, XPU, etc.).

For backend-specific stream APIs, see [CUDA Streams](../cuda/streams.html) and [XPU Support](../xpu/index.html).

## Stream

class Stream

A stream is a software mechanism used to synchronize launched kernels without requiring explicit synchronizations between kernels.

The basic model is that every kernel launch is associated with a stream: every kernel on the same stream is implicitly synchronized so that if I launch kernels A and B on the same stream, A is guaranteed to finish before B launches. If I want B to run concurrently with A, I must schedule it on a different stream.

The Stream class is a backend agnostic value class representing a stream which I may schedule a kernel on. Every stream is associated with a device, which is recorded in stream, which is used to avoid confusion about which device a stream refers to.

Streams are explicitly thread-safe, in the sense that it is OK to pass a Stream from one thread to another, and kernels queued from two different threads will still get serialized appropriately. (Of course, the time when the kernels get queued is undetermined unless you synchronize host side ;)

Stream does NOT have a default constructor. Streams are for expert users; if you want to use Streams, we're going to assume you know how to deal with C++ template error messages if you try to resize() a vector of Streams.

Known instances of streams in backends:

- cudaStream_t (CUDA)
- hipStream_t (HIP)
- cl_command_queue (OpenCL) (NB: Caffe2's existing OpenCL integration does NOT support command queues.)

Because this class is device agnostic, it cannot provide backend-specific functionality (e.g., get the cudaStream_t of a CUDA stream.) There are wrapper classes which provide this functionality, e.g., CUDAStream.

Public Types

enum Unsafe

*Values:*

enumerator UNSAFE

enum Default

*Values:*

enumerator DEFAULT

Public Functions

inline explicit Stream(Unsafe, [Device](device.html#_CPPv4N3c106DeviceE) device, StreamId id)

Unsafely construct a stream from a [Device](device.html#PyTorchstructc10_1_1_device) and a StreamId.

In general, only specific implementations of streams for a backend should manufacture Stream directly in this way; other users should use the provided APIs to get a stream. In particular, we don't require backends to give any guarantees about non-zero StreamIds; they are welcome to allocate in whatever way they like.

inline explicit Stream(Default, [Device](device.html#_CPPv4N3c106DeviceE) device)

Construct the default stream of a [Device](device.html#PyTorchstructc10_1_1_device).

The default stream is NOT the same as the current stream; default stream is a fixed stream that never changes, whereas the current stream may be changed by StreamGuard.

inline bool operator==(const Stream &other) const noexcept

inline bool operator!=(const Stream &other) const noexcept

inline [Device](device.html#_CPPv4N3c106DeviceE) device() const noexcept

inline [DeviceType](device.html#_CPPv4N3c1010DeviceTypeE) device_type() const noexcept

inline DeviceIndex device_index() const noexcept

inline StreamId id() const noexcept

void *native_handle() const

template<typename T>
inline void wait(const T &event) const

bool query() const

void synchronize() const

bool is_capturing() const

inline uint64_t hash() const noexcept

inline struct StreamData3 pack3() const

Public Static Functions

static inline Stream unpack3(StreamId stream_id, DeviceIndex device_index, [DeviceType](device.html#_CPPv4N3c1010DeviceTypeE) device_type)

**Example:**

```
#include <c10/core/Stream.h>

// Streams are typically obtained from backend-specific APIs
auto cuda_stream = c10::cuda::getCurrentCUDAStream();

// c10::Stream provides the common interface
c10::Device device = cuda_stream.device();
c10::DeviceType type = cuda_stream.device_type();
```