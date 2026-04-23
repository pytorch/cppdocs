# ATen: Tensor Library

ATen (A Tensor Library) is the foundational tensor and mathematical operation
library on which all of PyTorch is built. It provides the core `Tensor` class
and hundreds of mathematical operations that work on tensors.

**When to use ATen directly:**

- When writing low-level tensor operations or custom kernels
- When you need direct access to tensor data and metadata
- When working with the PyTorch internals or extending PyTorch

**Basic usage:**

```
#include <ATen/ATen.h>

// Create tensors
at::Tensor a = at::ones({2, 3});
at::Tensor b = at::randn({2, 3});

// Perform operations
at::Tensor c = a + b;
at::Tensor d = at::matmul(a.t(), b);

// Move to GPU
if (at::cuda::is_available()) {
 at::Tensor gpu_tensor = c.to(at::kCUDA);
}
```

For most applications, prefer using the higher-level `torch::` namespace
(see [Neural Network Modules (torch::nn)](../nn/index.html), [Optimizers (torch::optim)](../optim/index.html)) which provides a more user-friendly API.

## Header Files

The following headers are part of the ATen public API:

- `ATen/ATen.h` - Main ATen header
- `ATen/Backend.h` - Backend enumeration
- `ATen/core/Tensor.h` - Tensor class
- `ATen/core/ivalue.h` - IValue type (see [C10: Core Utilities](../c10/index.html))
- `ATen/core/ScalarType.h` - Data type definitions
- `ATen/TensorOptions.h` - Tensor creation options
- `ATen/Scalar.h` - Scalar type
- `ATen/Layout.h` - Tensor layout
- `ATen/DeviceGuard.h` - Device context management
- `ATen/native/TensorShape.h` - Tensor shape operations
- `ATen/cuda/CUDAContext.h` - CUDA context (see [CUDA Support](../cuda/index.html))
- `ATen/cudnn/Descriptors.h` - cuDNN descriptors
- `ATen/mkl/Descriptors.h` - MKL descriptors

Note

The core `at::Tensor` class is defined in a generated header file
(`TensorBody.h`) that only exists after building PyTorch. The documentation
below describes the API manually.

## ATen Categories

- [Tensor Class](tensor.html)
- [Tensor Creation](creation.html)
- [Tensor Indexing](indexing.html)
- [Tensor Accessors](accessors.html)