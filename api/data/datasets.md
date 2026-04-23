# Datasets

The dataset abstraction defines how to access individual samples in your data.
All datasets inherit from `Dataset` and must implement `get()` and `size()`.

## Dataset Base Class

template<typename Self, typename SingleExample = Example<>>
class Dataset : public torch::data::datasets::BatchDataset<Self, std::vector<Example<>>>

A dataset that can yield data in batches, or as individual examples.

A `Dataset` is a `BatchDataset`, because it supports random access and therefore batched access is implemented (by default) by calling the random access indexing function for each index in the requested batch of indices. This can be customized.

Public Types

using ExampleType = SingleExample

Public Functions

virtual ExampleType get(size_t index) = 0

Returns the example at the given index.

inline virtual std::vector<ExampleType> get_batch(ArrayRef<size_t> indices) override

Returns a batch of data.

The default implementation calls `get()` for every requested index in the batch.

template<typename Self, typename Batch = std::vector<Example<>>, typename BatchRequest = ArrayRef<size_t>>
class BatchDataset

A dataset that can yield data only in batches.

Subclassed by torch::data::datasets::Dataset< MNIST >, torch::data::datasets::Dataset< TensorDataset, TensorExample >, torch::data::datasets::StatefulDataset< ChunkDataset< ChunkReader, samplers::RandomSampler, samplers::RandomSampler >, ChunkReader::BatchType, size_t >

Public Types

using SelfType = Self

using BatchType = Batch

using BatchRequestType = BatchRequest

Public Functions

virtual ~BatchDataset() = default

virtual Batch get_batch(BatchRequest request) = 0

Returns a batch of data given an index.

virtual std::optional<size_t> size() const = 0

Returns the size of the dataset, or an empty std::optional if it is unsized.

template<typename TransformType>
inline MapDataset<Self, TransformType> map(TransformType transform) &

Creates a `MapDataset` that applies the given `transform` to this dataset.

template<typename TransformType>
inline MapDataset<Self, TransformType> map(TransformType transform) &&

Creates a `MapDataset` that applies the given `transform` to this dataset.

Public Static Attributes

static constexpr bool is_stateful = detail::is_optional<BatchType>::value

## StatefulDataset

A dataset that manages its own state across batches (e.g., position in a stream).
Unlike `Dataset`, it produces batches directly without external samplers.

template<typename Self, typename Batch = std::vector<Example<>>, typename BatchRequest = size_t>
class StatefulDataset : public BatchDataset<Self, std::optional<std::vector<Example<>>>, size_t>

A stateful dataset is a dataset that maintains some internal state, which will be `reset()` at the beginning of each epoch.

Subclasses can override the `reset()` method to configure this behavior. Further, the return type of a stateful dataset's `get_batch()` method is always an `optional`. When the stateful dataset wants to indicate to the dataloader that its epoch has ended, it should return an empty optional. The dataloader knows to modify its implementation based on whether the dataset is stateless or stateful.

Note that when subclassing a from `StatefulDataset<Self, T>`, the return type of `get_batch()`, which the subclass must override, will be `optional<T>` (i.e. the type specified in the `StatefulDataset` specialization is automatically boxed into an `optional` for the dataset's `BatchType`).

Public Functions

virtual void reset() = 0

Resets internal state of the dataset.

virtual void save(serialize::[OutputArchive](../serialize/archives.html#_CPPv4N5torch9serialize13OutputArchiveE) &archive) const = 0

Saves the statefulDataset's state to OutputArchive.

virtual void load(serialize::[InputArchive](../serialize/archives.html#_CPPv4N5torch9serialize12InputArchiveE) &archive) = 0

Deserializes the statefulDataset's state from the `archive`.

## ChunkDataReader

Interface for reading chunks of data from a data source. Used with
`ChunkDataset` for large-scale data loading.

template<typename ExampleType_, typename ChunkType_ = std::vector<ExampleType_>>
class ChunkDataReader

Interface for chunk reader, which performs data chunking and reading of entire chunks.

A chunk could be an entire file, such as an audio data file or an image, or part of a file in the case of a large text-file split based on seek positions.

Public Types

using ChunkType = ChunkType_

using ExampleType = ExampleType_

Public Functions

virtual ~ChunkDataReader() = default

virtual ChunkType read_chunk(size_t chunk_index) = 0

Read an entire chunk.

virtual size_t chunk_count() = 0

Returns the number of chunks available in this reader.

virtual void reset() = 0

This will clear any internal state associate with this reader.

## Custom Dataset Example

```
class CustomDataset : public torch::data::datasets::Dataset<CustomDataset> {
 public:
 explicit CustomDataset(const std::string& root) {
 // Load data from root directory
 }

 torch::data::Example<> get(size_t index) override {
 return {images_[index], labels_[index]};
 }

 torch::optional<size_t> size() const override {
 return images_.size(0);
 }

 private:
 torch::Tensor images_, labels_;
};
```

## MapDataset

template<typename SourceDataset, typename AppliedTransform>
class MapDataset : public torch::data::datasets::BatchDataset<MapDataset<SourceDataset, AppliedTransform>, detail::optional_if_t<SourceDataset::is_stateful, AppliedTransform::OutputBatchType>, SourceDataset::BatchRequestType>

A `MapDataset` is a dataset that applies a transform to a source dataset.

Public Types

using DatasetType = SourceDataset

using TransformType = AppliedTransform

using BatchRequestType = typename SourceDataset::BatchRequestType

using OutputBatchType = detail::optional_if_t<SourceDataset::is_stateful, typename AppliedTransform::OutputBatchType>

Public Functions

inline MapDataset(DatasetType dataset, TransformType transform)

inline virtual OutputBatchType get_batch(BatchRequestType indices) override

Gets a batch from the source dataset and applies the transform to it, returning the result.

inline virtual std::optional<size_t> size() const noexcept override

Returns the size of the source dataset.

inline void reset()

Calls `reset()` on the underlying dataset.

NOTE: Stateless datasets do not have a reset() method, so a call to this method will only compile for stateful datasets (which have a reset() method).

inline const SourceDataset &dataset() noexcept

Returns the underlying dataset.

inline const AppliedTransform &transform() noexcept

Returns the transform being applied.

## ChunkDataset

template<typename ChunkReader, typename ChunkSampler = samplers::[RandomSampler](samplers.html#_CPPv4N5torch4data8samplers13RandomSamplerE), typename ExampleSampler = samplers::[RandomSampler](samplers.html#_CPPv4N5torch4data8samplers13RandomSamplerE)>
class ChunkDataset : public torch::data::datasets::StatefulDataset<ChunkDataset<ChunkReader, samplers::[RandomSampler](samplers.html#_CPPv4N5torch4data8samplers13RandomSamplerE), samplers::[RandomSampler](samplers.html#_CPPv4N5torch4data8samplers13RandomSamplerE)>, ChunkReader::BatchType, size_t>

A stateful dataset that support hierarchical sampling and prefetching of entre chunks.

Unlike regular dataset, chunk dataset require two samplers to operate and keeps an internal state. `ChunkSampler` selects, which chunk to load next, while the `ExampleSampler` determines the order of Examples that are returned in each `get_batch` call. The hierarchical sampling approach used here is inspired by this paper [http://martin.zinkevich.org/publications/nips2010.pdf](http://martin.zinkevich.org/publications/nips2010.pdf)

Public Types

using BatchType = std::optional<typename ChunkReader::BatchType>

using UnwrappedBatchType = typename ChunkReader::BatchType

using BatchRequestType = size_t

using ChunkSamplerType = ChunkSampler

using ExampleSamplerType = ExampleSampler

Public Functions

inline ChunkDataset(ChunkReader chunk_reader, ChunkSampler chunk_sampler, ExampleSampler example_sampler, ChunkDatasetOptions options, std::function<void(UnwrappedBatchType&)> preprocessing_policy = std::function<void(UnwrappedBatchType&)>())

inline ~ChunkDataset() override

inline BatchType get_batch(size_t batch_size) override

Default get_batch method of BatchDataset.

This method returns Example batches created from the preloaded chunks. The implementation is dataset agnostic and does not need overriding in different chunk datasets.

inline BatchType get_batch()

Helper method around get_batch as `batch_size` is not strictly necessary.

inline virtual void reset() override

This will clear any internal state and starts the internal prefetching mechanism for the chunk dataset.

inline virtual std::optional<size_t> size() const override

size is not used for chunk dataset.

inline ChunkSamplerType &chunk_sampler()

inline virtual void save(serialize::[OutputArchive](../serialize/archives.html#_CPPv4N5torch9serialize13OutputArchiveE) &archive) const override

Saves the statefulDataset's state to OutputArchive.

inline virtual void load(serialize::[InputArchive](../serialize/archives.html#_CPPv4N5torch9serialize12InputArchiveE) &archive) override

Deserializes the statefulDataset's state from the `archive`.

## SharedBatchDataset

template<typename UnderlyingDataset>
class SharedBatchDataset : public torch::data::datasets::BatchDataset<SharedBatchDataset<UnderlyingDataset>, UnderlyingDataset::BatchType, UnderlyingDataset::BatchRequestType>

A dataset that wraps another dataset in a shared pointer and implements the `BatchDataset` API, delegating all calls to the shared instance.

This is useful when you want all worker threads in the dataloader to access the same dataset instance. The dataset must take care of synchronization and thread-safe access itself.

Use `torch::data::datasets::make_shared_dataset()` to create a new `SharedBatchDataset` like you would a `std::shared_ptr`.

Public Types

using BatchType = typename UnderlyingDataset::BatchType

using BatchRequestType = typename UnderlyingDataset::BatchRequestType

Public Functions

inline SharedBatchDataset(std::shared_ptr<UnderlyingDataset> shared_dataset)

Constructs a new `SharedBatchDataset` from a `shared_ptr` to the `UnderlyingDataset`.

inline virtual BatchType get_batch(BatchRequestType request) override

Calls `get_batch` on the underlying dataset.

inline virtual std::optional<size_t> size() const override

Returns the `size` from the underlying dataset.

inline UnderlyingDataset &operator*()

Accesses the underlying dataset.

inline const UnderlyingDataset &operator*() const

Accesses the underlying dataset.

inline UnderlyingDataset *operator->()

Accesses the underlying dataset.

inline const UnderlyingDataset *operator->() const

Accesses the underlying dataset.

inline void reset()

Calls `reset()` on the underlying dataset.

## Built-in Datasets

### MNIST

class MNIST : public torch::data::datasets::Dataset<MNIST>

The MNIST dataset.

Public Types

enum class Mode

The mode in which the dataset is loaded.

*Values:*

enumerator kTrain

enumerator kTest

Public Functions

explicit MNIST(const std::string &root, Mode mode = Mode::kTrain)

Loads the MNIST dataset from the `root` path.

The supplied `root` path should contain the *content* of the unzipped MNIST dataset, available from [http://yann.lecun.com/exdb/mnist](http://yann.lecun.com/exdb/mnist).

virtual Example get(size_t index) override

Returns the `Example` at the given `index`.

virtual std::optional<size_t> size() const override

Returns the size of the dataset.

bool is_train() const noexcept

Returns true if this is the training subset of MNIST.

const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &images() const

Returns all images stacked into a single tensor.

const [Tensor](../aten/tensor.html#_CPPv46Tensorv) &targets() const

Returns all targets stacked into a single tensor.

**Example:**

```
auto dataset = torch::data::datasets::MNIST("./data")
 .map(torch::data::transforms::Normalize<>(0.1307, 0.3081))
 .map(torch::data::transforms::Stack<>());
```

## Example Struct

template<typename Data = at::[Tensor](../aten/tensor.html#_CPPv4N2at6TensorE), typename Target = at::[Tensor](../aten/tensor.html#_CPPv4N2at6TensorE)>
struct Example

An `Example` from a dataset.

A dataset consists of data and an associated target (label).

Public Types

using DataType = Data

using TargetType = Target

Public Functions

Example() = default

inline Example(Data data, Target target)

Public Members

Data data

Target target