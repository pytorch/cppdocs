# DataLoader

The DataLoader batches samples from a dataset and optionally shuffles and
parallelizes the loading process. It is the main interface for iterating
over training data.

## DataLoader Classes

template<typename Dataset, typename Batch, typename BatchRequest>
class DataLoaderBase

Public Types

using BatchType = Batch

using BatchRequestType = BatchRequest

Public Functions

inline DataLoaderBase(DataLoaderOptions options, std::unique_ptr<Dataset> main_thread_dataset = nullptr)

Constructs a new DataLoader from a `dataset` to sample from, `options` to configure the DataLoader with, and a `sampler` that specifies the sampling strategy.

DataLoaderBase(const DataLoaderBase&) = delete

DataLoaderBase(DataLoaderBase&&) = delete

DataLoaderBase &operator=(const DataLoaderBase&) = delete

DataLoaderBase &operator=(DataLoaderBase&&) = delete

inline virtual ~DataLoaderBase()

inline Iterator<Batch> begin()

Returns an iterator into the DataLoader.

The lifetime of the iterator is bound to the DataLoader. In C++ standards language, the category of the iterator is `OutputIterator`. See [https://en.cppreference.com/w/cpp/named_req/OutputIterator](https://en.cppreference.com/w/cpp/named_req/OutputIterator) for what this means. In short: you may increment the iterator and dereference it, but cannot go back, or step forward more than one position at a time. When the DataLoader is exhausted, it will compare equal with the special "sentinel" iterator returned by `DataLoader::end()`. Most of the time, you should only use range-for loops to loop over the DataLoader, but standard algorithms like `std::copy(dataloader.begin(), dataloader.end(), output_iterator)` are supported too.

inline Iterator<Batch> end()

Returns a special "sentinel" iterator that compares equal with a non-sentinel iterator once the DataLoader is exhausted.

inline void join()

Joins the DataLoader's worker threads and drains internal queues.

This function may only be invoked from the main thread (in which the DataLoader lives).

inline const FullDataLoaderOptions &options() const noexcept

Returns the options with which the DataLoader was configured.

## DataLoaderOptions

struct DataLoaderOptions

Options to configure a `DataLoader`.

Public Functions

DataLoaderOptions() = default

inline DataLoaderOptions(size_t batch_size)

inline auto batch_size(const size_t &new_batch_size) -> decltype(*this)

The size of each batch to fetch.

inline auto batch_size(size_t &&new_batch_size) -> decltype(*this)

inline const size_t &batch_size() const noexcept

inline size_t &batch_size() noexcept

inline auto workers(const size_t &new_workers) -> decltype(*this)

The number of worker threads to launch.

If zero, the main thread will synchronously perform the data loading.

inline auto workers(size_t &&new_workers) -> decltype(*this)

inline const size_t &workers() const noexcept

inline size_t &workers() noexcept

inline auto max_jobs(const std::optional<size_t> &new_max_jobs) -> decltype(*this)

The maximum number of jobs to enqueue for fetching by worker threads.

Defaults to two times the number of worker threads.

inline auto max_jobs(std::optional<size_t> &&new_max_jobs) -> decltype(*this)

inline const std::optional<size_t> &max_jobs() const noexcept

inline std::optional<size_t> &max_jobs() noexcept

inline auto timeout(const std::optional<std::chrono::milliseconds> &new_timeout) -> decltype(*this)

An optional limit on the time to wait for the next batch.

inline auto timeout(std::optional<std::chrono::milliseconds> &&new_timeout) -> decltype(*this)

inline const std::optional<std::chrono::milliseconds> &timeout() const noexcept

inline std::optional<std::chrono::milliseconds> &timeout() noexcept

inline auto enforce_ordering(const bool &new_enforce_ordering) -> decltype(*this)

Whether to enforce ordering of batches when multiple are loaded asynchronously by worker threads.

Set to `false` for better performance if you do not care about determinism.

inline auto enforce_ordering(bool &&new_enforce_ordering) -> decltype(*this)

inline const bool &enforce_ordering() const noexcept

inline bool &enforce_ordering() noexcept

inline auto drop_last(const bool &new_drop_last) -> decltype(*this)

Whether to omit the last batch if it contains less than `batch_size` examples.

inline auto drop_last(bool &&new_drop_last) -> decltype(*this)

inline const bool &drop_last() const noexcept

inline bool &drop_last() noexcept

## StatefulDataLoader

A DataLoader for `StatefulDataset` types that manage their own batching logic
internally.

template<typename Dataset>
class StatefulDataLoader : public torch::data::DataLoaderBase<Dataset, Dataset::BatchType::value_type, Dataset::BatchRequestType>

A dataloader for stateful datasets.

A dataloader for stateful datatasets differs from one for stateless datasets one in that the dataset is shared among worker threads, and that this dataset is itself responsible for producing batches rather than depending on a sampler. The statefulness here actually refers to the dataset. The StatefulDataLoader simply alters the data loading algorithm to accommodate the stateful, shared nature of the dataset. Note that the dataset must be thread safe if more than one worker thread is used.

A stateful dataloader is created by calling `make_data_loader` with a stateful dataset.

Public Types

using super = DataLoaderBase<Dataset, typename Dataset::BatchType::value_type, typename Dataset::BatchRequestType>

using BatchRequestType = BatchRequest

Public Functions

inline StatefulDataLoader(Dataset dataset, DataLoaderOptions options)

Constructs the `StatefulDataLoader` from a `dataset` and some `options`.

## StatelessDataLoader

A DataLoader for `Dataset` types that use external samplers for batching.

template<typename Dataset, typename Sampler>
class StatelessDataLoader : public torch::data::DataLoaderBase<Dataset, Dataset::BatchType, Sampler::BatchRequestType>

A dataloader for stateless datasets.

This dataloader follows the traditional PyTorch dataloader design, whereby a (possibly) stateful sampler produces *batch requests* for a stateless dataset, which acts as a simple batch request to batch mapping. The batch request will often be an array of indices, and if the dataset is a simple image dataset, the dataset would produce the images at those indices.

Public Types

using super = DataLoaderBase<Dataset, typename Dataset::BatchType, typename Sampler::BatchRequestType>

using BatchRequestType = BatchRequest

Public Functions

inline StatelessDataLoader(Dataset dataset, Sampler sampler, DataLoaderOptions options)

Constructs the `StatelessDataLoader` from a `dataset`, a `sampler` and some `options`.

## Iterator

template<typename Batch>
class Iterator

Public Types

using difference_type = std::ptrdiff_t

using value_type = Batch

using pointer = Batch*

using reference = Batch&

using iterator_category = std::input_iterator_tag

Public Functions

inline explicit Iterator(std::unique_ptr<detail::IteratorImpl<Batch>> impl)

inline Iterator &operator++()

Increments the iterator.

Only permitted for valid iterators (not past the end).

inline Batch &operator*()

Returns the current batch.

Only permitted for valid iterators (not past the end).

inline Batch *operator->()

Returns a pointer to the current batch.

Only permitted for valid iterators (not past the end).

inline bool operator==(const Iterator &other) const

Compares two iterators for equality.

inline bool operator!=(const Iterator &other) const

Compares two iterators for inequality.

## Creating a DataLoader

Use `make_data_loader` to create a DataLoader from a dataset:

```
auto data_loader = torch::data::make_data_loader(
 std::move(dataset),
 torch::data::DataLoaderOptions()
 .batch_size(64)
 .workers(4));

for (auto& batch : *data_loader) {
 auto data = batch.data;
 auto target = batch.target;
 // Train on batch
}
```

## Complete Training Example

```
#include <torch/torch.h>

int main() {
 // Load dataset
 auto dataset = torch::data::datasets::MNIST("./data")
 .map(torch::data::transforms::Normalize<>(0.1307, 0.3081))
 .map(torch::data::transforms::Stack<>());

 // Create data loader
 auto data_loader = torch::data::make_data_loader(
 std::move(dataset),
 torch::data::DataLoaderOptions().batch_size(64).workers(2));

 // Create model and optimizer
 auto model = std::make_shared<Net>();
 auto optimizer = torch::optim::Adam(model->parameters(), 0.001);

 // Training loop
 for (size_t epoch = 1; epoch <= 10; ++epoch) {
 for (auto& batch : *data_loader) {
 optimizer.zero_grad();
 auto output = model->forward(batch.data);
 auto loss = torch::nll_loss(output, batch.target);
 loss.backward();
 optimizer.step();
 }
 }
}
```