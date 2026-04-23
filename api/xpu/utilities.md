# XPU Utility Functions

High-level utility functions for querying and managing XPU devices.

## Device Management

size_t torch::xpu::device_count()

Returns the number of XPU devices available.

bool torch::xpu::is_available()

Returns true if at least one XPU device is available.

void torch::xpu::synchronize(int64_t device_index)

Waits for all kernels in all streams on a XPU device to complete.

**Example:**

```
#include <torch/torch.h>

if (torch::xpu::is_available()) {
 size_t num_devices = torch::xpu::device_count();
 std::cout << "Found " << num_devices << " XPU device(s)" << std::endl;

 // Synchronize all streams on device 0
 torch::xpu::synchronize(0);
}
```

## Random Number Generation

void torch::xpu::manual_seed(uint64_t seed)

Sets the seed for the current GPU.

void torch::xpu::manual_seed_all(uint64_t seed)

Sets the seed for all available GPUs.

**Example:**

```
// Set seed for reproducibility on current XPU device
torch::xpu::manual_seed(42);

// Set seed for all XPU devices
torch::xpu::manual_seed_all(42);
```