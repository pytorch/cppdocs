# CUDA Utility Functions

PyTorch provides utility functions for querying and managing CUDA devices,
streams, and library handles.

## Device Management

c10::DeviceIndex torch::cuda::device_count()

Returns the number of CUDA devices available.

int c10::cuda::current_device()

Returns the index of the current CUDA device.

**Example:**

```
#include <c10/cuda/CUDAFunctions.h>

// Check available devices
int num_devices = c10::cuda::device_count();

// Get current device
int current = c10::cuda::current_device();
```

## Device Properties

cudaDeviceProp *at::cuda::getCurrentDeviceProperties()

cudaDeviceProp *at::cuda::getDeviceProperties(c10::DeviceIndex device)

bool at::cuda::canDeviceAccessPeer(c10::DeviceIndex device, c10::DeviceIndex peer_device)

int at::cuda::warp_size()

**Example:**

```
#include <ATen/cuda/CUDAContext.h>

// Query properties of the current device
cudaDeviceProp* props = at::cuda::getCurrentDeviceProperties();
std::cout << "Device: " << props->name << std::endl;
std::cout << "Compute capability: " << props->major << "." << props->minor << std::endl;

// Query a specific device
cudaDeviceProp* dev1_props = at::cuda::getDeviceProperties(1);

// Check peer access
bool can_access = at::cuda::canDeviceAccessPeer(0, 1);
```

## Library Handles

These functions return handles for CUDA math libraries on the current device
and stream. They are primarily useful when writing custom CUDA kernels that
call cuBLAS, cuSPARSE, or cuSOLVER directly.

cublasHandle_t at::cuda::getCurrentCUDABlasHandle(bool setup = true)

cublasLtHandle_t at::cuda::getCurrentCUDABlasLtHandle()

cusparseHandle_t at::cuda::getCurrentCUDASparseHandle()

cusolverDnHandle_t at::cuda::getCurrentCUDASolverDnHandle()

**Example:**

```
#include <ATen/cuda/CUDAContext.h>

// Get cuBLAS handle for current device/stream
cublasHandle_t handle = at::cuda::getCurrentCUDABlasHandle();

// Get cuSPARSE handle
cusparseHandle_t sparse_handle = at::cuda::getCurrentCUDASparseHandle();
```

## cuDNN Descriptors

When writing custom kernels that call cuDNN directly, PyTorch provides RAII
wrapper classes for cuDNN descriptors. These are defined in
`ATen/cudnn/Descriptors.h`.

### Descriptor (Base Class)

template<typename T, cudnnStatus_t (*ctor)(T**), cudnnStatus_t (*dtor)(T*)>
class Descriptor

Public Functions

inline T *desc() const

inline T *desc()

inline T *mut_desc()

A generic RAII wrapper for cuDNN descriptor types. Descriptors default
construct to a `nullptr` and are initialized on first use via `mut_desc()`.
Use `desc()` for read-only access.

### TensorDescriptor

class TensorDescriptor : public at::native::Descriptor<cudnnTensorStruct, &cudnnCreateTensorDescriptor, &cudnnDestroyTensorDescriptor>

Public Functions

TensorDescriptor() = default

inline explicit TensorDescriptor(const at::[Tensor](../aten/tensor.html#_CPPv4N2at6TensorE) &t, size_t pad = 0)

void set(const at::[Tensor](../aten/tensor.html#_CPPv4N2at6TensorE) &t, size_t pad = 0)

void set(const at::[Tensor](../aten/tensor.html#_CPPv4N2at6TensorE) &t, at::MemoryFormat memory_format, size_t pad = 0)

void set(cudnnDataType_t dataType, IntArrayRef sizes, IntArrayRef strides, size_t pad = 0)

void print()

Wraps `cudnnTensorDescriptor_t`. Supports padding lower-dimensional tensors
to meet cuDNN broadcasting requirements (see `pad` parameter).

**Example:**

```
#include <ATen/cudnn/Descriptors.h>

at::Tensor input = torch::randn({32, 3, 224, 224}, torch::kCUDA);
at::native::TensorDescriptor desc(input);
cudnnTensorDescriptor_t raw = desc.desc();
```

### FilterDescriptor

class FilterDescriptor : public at::native::Descriptor<cudnnFilterStruct, &cudnnCreateFilterDescriptor, &cudnnDestroyFilterDescriptor>

Public Functions

inline void set(const at::[Tensor](../aten/tensor.html#_CPPv4N2at6TensorE) &t, int64_t pad = 0)

void set(const at::[Tensor](../aten/tensor.html#_CPPv4N2at6TensorE) &t, const at::MemoryFormat memory_format, int64_t pad = 0)

void print()

Wraps `cudnnFilterDescriptor_t` for convolution filter weights.

### ConvolutionDescriptor

struct ConvolutionDescriptor : public at::native::Descriptor<cudnnConvolutionStruct, &cudnnCreateConvolutionDescriptor, &cudnnDestroyConvolutionDescriptor>

Public Functions

inline void set(cudnnDataType_t dataType, int dim, int *pad, int *stride, int *upscale, int groups, bool allow_tf32)

Wraps `cudnnConvolutionDescriptor_t`. Configures padding, stride, dilation,
groups, and math type (TF32, tensor ops) for convolution operations.

### RNNDataDescriptor

class RNNDataDescriptor : public at::native::Descriptor<cudnnRNNDataStruct, &cudnnCreateRNNDataDescriptor, &cudnnDestroyRNNDataDescriptor>

Public Functions

void set(const at::[Tensor](../aten/tensor.html#_CPPv4N2at6TensorE) &t, cudnnRNNDataLayout_t layout, int maxSeqLength, int batchSize, int vectorSize, const int *seqLengthArray)

Wraps `cudnnRNNDataDescriptor_t` for variable-length sequence data.

### DropoutDescriptor

struct DropoutDescriptor : public at::native::Descriptor<cudnnDropoutStruct, &cudnnCreateDropoutDescriptor, &cudnnDestroyDropoutDescriptor>

Public Functions

inline void initialize_rng(cudnnHandle_t handle, float dropout, long long int seed, const [TensorOptions](../aten/tensor.html#_CPPv4N2at13TensorOptionsE) &options)

inline void set(cudnnHandle_t handle, float dropout, const at::[Tensor](../aten/tensor.html#_CPPv4N2at6TensorE) &state)

inline void set_no_dropout(cudnnHandle_t handle)

Public Members

at::[Tensor](../aten/tensor.html#_CPPv4N2at6TensorE) state

Wraps `cudnnDropoutDescriptor_t`. Manages RNG state for cuDNN dropout.

### ActivationDescriptor

struct ActivationDescriptor : public at::native::Descriptor<cudnnActivationStruct, &cudnnCreateActivationDescriptor, &cudnnDestroyActivationDescriptor>

Public Functions

inline void set(cudnnActivationMode_t mode)

Wraps `cudnnActivationDescriptor_t`.

### SpatialTransformerDescriptor

struct SpatialTransformerDescriptor : public at::native::Descriptor<cudnnSpatialTransformerStruct, &cudnnCreateSpatialTransformerDescriptor, &cudnnDestroySpatialTransformerDescriptor>

Public Functions

inline void set(cudnnDataType_t dataType, int dim, int *size)

### CTCLossDescriptor

struct CTCLossDescriptor : public at::native::Descriptor<cudnnCTCLossStruct, &cudnnCreateCTCLossDescriptor, &cudnnDestroyCTCLossDescriptor>

Public Functions

inline void set(cudnnDataType_t datatype)

inline void setEx(cudnnDataType_t datatype, cudnnLossNormalizationMode_t normMode, cudnnNanPropagation_t gradMode)

inline void set_v8_v9(cudnnDataType_t datatype, cudnnLossNormalizationMode_t normMode, cudnnNanPropagation_t gradMode, int maxLabelLength)

## Stream Management

[CUDAStream](streams.html#_CPPv4N3c104cuda10CUDAStreamE) c10::cuda::getDefaultCUDAStream(DeviceIndex device_index = -1)

Get the default CUDA stream, for the passed CUDA device, or for the current device if no device index is passed.

The default stream is where most computation occurs when you aren't explicitly using streams.

[CUDAStream](streams.html#_CPPv4N3c104cuda10CUDAStreamE) c10::cuda::getCurrentCUDAStream(DeviceIndex device_index = -1)

Get the current CUDA stream, for the passed CUDA device, or for the current device if no device index is passed.

The current CUDA stream will usually be the default CUDA stream for the device, but it may be different if someone called 'setCurrentCUDAStream' or used 'StreamGuard' or '[CUDAStreamGuard](guards.html#PyTorchstructc10_1_1cuda_1_1_c_u_d_a_stream_guard)'.

void c10::cuda::setCurrentCUDAStream([CUDAStream](streams.html#_CPPv4N3c104cuda10CUDAStreamE) stream)

Set the current stream on the device of the passed in stream to be the passed in stream.

Yes, you read that right: this function has *nothing* to do with the current device: it toggles the current stream of the device of the passed stream.

Confused? Avoid using this function; prefer using '[CUDAStreamGuard](guards.html#PyTorchstructc10_1_1cuda_1_1_c_u_d_a_stream_guard)' instead (which will switch both your current device and current stream in the way you expect, and reset it back to its original state afterwards).

[CUDAStream](streams.html#_CPPv4N3c104cuda10CUDAStreamE) c10::cuda::getStreamFromPool(const bool isHighPriority = false, DeviceIndex device = -1)

Get a new stream from the CUDA stream pool.

You can think of this as "creating" a new stream, but no such creation actually happens; instead, streams are preallocated from the pool and returned in a round-robin fashion.

You can request a stream from the high priority pool by setting isHighPriority to true, or a stream for a specific device by setting device (defaulting to the current CUDA stream.)

[CUDAStream](streams.html#_CPPv4N3c104cuda10CUDAStreamE) c10::cuda::getStreamFromExternal(cudaStream_t ext_stream, DeviceIndex device_index)

Get a [CUDAStream](streams.html#PyTorchclassc10_1_1cuda_1_1_c_u_d_a_stream) from a externally allocated one.

This is mainly for interoperability with different libraries where we want to operate on a non-torch allocated stream for data exchange or similar purposes

**Example:**

```
#include <c10/cuda/CUDAStream.h>

// Create and set custom stream
auto stream = c10::cuda::getStreamFromPool();
c10::cuda::setCurrentCUDAStream(stream);

// Get default stream
auto default_stream = c10::cuda::getDefaultCUDAStream();

// Wrap an externally created CUDA stream
cudaStream_t ext_stream;
cudaStreamCreate(&ext_stream);
auto wrapped = c10::cuda::getStreamFromExternal(ext_stream, /*device_index=*/0);
```