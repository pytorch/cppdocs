# Samplers

Samplers control the order in which samples are accessed from a dataset.
They determine the indices that the DataLoader uses to fetch data.

## Sampler Base Class

template<typename BatchRequest = std::vector<size_t>>
class Sampler

A `Sampler` is an object that yields an index with which to access a dataset.

Subclassed by torch::data::samplers::RandomSampler, torch::data::samplers::SequentialSampler

Public Types

using BatchRequestType = BatchRequest

Public Functions

virtual ~Sampler() = default

virtual void reset(std::optional<size_t> new_size) = 0

Resets the `Sampler`'s internal state.

Typically called before a new epoch. Optionally, accepts a new size when resetting the sampler.

virtual std::optional<BatchRequest> next(size_t batch_size) = 0

Returns the next index if possible, or an empty optional if the sampler is exhausted for this epoch.

virtual void save(serialize::[OutputArchive](../serialize/archives.html#_CPPv4N5torch9serialize13OutputArchiveE) &archive) const = 0

Serializes the `Sampler` to the `archive`.

virtual void load(serialize::[InputArchive](../serialize/archives.html#_CPPv4N5torch9serialize12InputArchiveE) &archive) = 0

Deserializes the `Sampler` from the `archive`.

## Sequential Sampler

Accesses samples in order from 0 to N-1. Use this for evaluation or when
order matters.

class SequentialSampler : public torch::data::samplers::Sampler<>

A `Sampler` that returns indices sequentially.

Public Functions

explicit SequentialSampler(size_t size)

Creates a `SequentialSampler` that will return indices in the range `0...size - 1`.

virtual void reset(std::optional<size_t> new_size = std::nullopt) override

Resets the `SequentialSampler` to zero.

virtual std::optional<std::vector<size_t>> next(size_t batch_size) override

Returns the next batch of indices.

virtual void save(serialize::[OutputArchive](../serialize/archives.html#_CPPv4N5torch9serialize13OutputArchiveE) &archive) const override

Serializes the `SequentialSampler` to the `archive`.

virtual void load(serialize::[InputArchive](../serialize/archives.html#_CPPv4N5torch9serialize12InputArchiveE) &archive) override

Deserializes the `SequentialSampler` from the `archive`.

size_t index() const noexcept

Returns the current index of the `SequentialSampler`.

## Random Sampler

Accesses samples in random order. Use this for training to ensure the model
sees samples in different orders each epoch.

class RandomSampler : public torch::data::samplers::Sampler<>

A `Sampler` that returns random indices.

Public Functions

explicit RandomSampler(int64_t size, Dtype index_dtype = torch::kInt64)

Constructs a `RandomSampler` with a size and dtype for the stored indices.

The constructor will eagerly allocate all required indices, which is the sequence `0 ... size - 1`. `index_dtype` is the data type of the stored indices. You can change it to influence memory usage.

~RandomSampler() override

virtual void reset(std::optional<size_t> new_size = std::nullopt) override

Resets the `RandomSampler` to a new set of indices.

virtual std::optional<std::vector<size_t>> next(size_t batch_size) override

Returns the next batch of indices.

virtual void save(serialize::[OutputArchive](../serialize/archives.html#_CPPv4N5torch9serialize13OutputArchiveE) &archive) const override

Serializes the `RandomSampler` to the `archive`.

virtual void load(serialize::[InputArchive](../serialize/archives.html#_CPPv4N5torch9serialize12InputArchiveE) &archive) override

Deserializes the `RandomSampler` from the `archive`.

size_t index() const noexcept

Returns the current index of the `RandomSampler`.

## Distributed Random Sampler

For distributed training, ensures each process gets a different subset of
the data without overlap.

class DistributedRandomSampler : public torch::data::samplers::DistributedSampler<>

Select samples randomly.

The sampling order is shuffled at each `reset()` call.

Public Functions

DistributedRandomSampler(size_t size, size_t num_replicas = 1, size_t rank = 0, bool allow_duplicates = true)

virtual void reset(std::optional<size_t> new_size = std::nullopt) override

Resets the `DistributedRandomSampler` to a new set of indices.

virtual std::optional<std::vector<size_t>> next(size_t batch_size) override

Returns the next batch of indices.

virtual void save(serialize::[OutputArchive](../serialize/archives.html#_CPPv4N5torch9serialize13OutputArchiveE) &archive) const override

Serializes the `DistributedRandomSampler` to the `archive`.

virtual void load(serialize::[InputArchive](../serialize/archives.html#_CPPv4N5torch9serialize12InputArchiveE) &archive) override

Deserializes the `DistributedRandomSampler` from the `archive`.

size_t index() const noexcept

Returns the current index of the `DistributedRandomSampler`.

## Distributed Sampler (Base)

template<typename BatchRequest = std::vector<size_t>>
class DistributedSampler : public torch::data::samplers::Sampler<std::vector<size_t>>

A `Sampler` that selects a subset of indices to sample from and defines a sampling behavior.

In a distributed setting, this selects a subset of the indices depending on the provided num_replicas and rank parameters. The `Sampler` performs a rounding operation based on the `allow_duplicates` parameter to decide the local sample count.

Subclassed by torch::data::samplers::DistributedRandomSampler, torch::data::samplers::DistributedSequentialSampler

Public Functions

inline DistributedSampler(size_t size, size_t num_replicas = 1, size_t rank = 0, bool allow_duplicates = true)

inline void set_epoch(size_t epoch)

Set the epoch for the current enumeration.

This can be used to alter the sample selection and shuffling behavior.

inline size_t epoch() const

## Distributed Sequential Sampler

class DistributedSequentialSampler : public torch::data::samplers::DistributedSampler<>

Select samples sequentially.

Public Functions

DistributedSequentialSampler(size_t size, size_t num_replicas = 1, size_t rank = 0, bool allow_duplicates = true)

virtual void reset(std::optional<size_t> new_size = std::nullopt) override

Resets the `DistributedSequentialSampler` to a new set of indices.

virtual std::optional<std::vector<size_t>> next(size_t batch_size) override

Returns the next batch of indices.

virtual void save(serialize::[OutputArchive](../serialize/archives.html#_CPPv4N5torch9serialize13OutputArchiveE) &archive) const override

Serializes the `DistributedSequentialSampler` to the `archive`.

virtual void load(serialize::[InputArchive](../serialize/archives.html#_CPPv4N5torch9serialize12InputArchiveE) &archive) override

Deserializes the `DistributedSequentialSampler` from the `archive`.

size_t index() const noexcept

Returns the current index of the `DistributedSequentialSampler`.

## Stream Sampler

class StreamSampler : public torch::data::samplers::Sampler<BatchSize>

A sampler for (potentially infinite) streams of data.

The major feature of the `StreamSampler` is that it does not return particular indices, but instead only the number of elements to fetch from the dataset. The dataset has to decide how to produce those elements.

Public Functions

explicit StreamSampler(size_t epoch_size)

Constructs the `StreamSampler` with the number of individual examples that should be fetched until the sampler is exhausted.

virtual void reset(std::optional<size_t> new_size = std::nullopt) override

Resets the internal state of the sampler.

virtual std::optional<BatchSize> next(size_t batch_size) override

Returns a `BatchSize` object with the number of elements to fetch in the next batch.

This number is the minimum of the supplied `batch_size` and the difference between the `epoch_size` and the current index. If the `epoch_size` has been reached, returns an empty optional.

virtual void save(serialize::[OutputArchive](../serialize/archives.html#_CPPv4N5torch9serialize13OutputArchiveE) &archive) const override

Serializes the `StreamSampler` to the `archive`.

virtual void load(serialize::[InputArchive](../serialize/archives.html#_CPPv4N5torch9serialize12InputArchiveE) &archive) override

Deserializes the `StreamSampler` from the `archive`.