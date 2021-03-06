# DataSourceRDD

`DataSourceRDD` is a `RDD[InternalRow]` that acts as a thin adapter between Spark SQL's [Data Source API V2](spark-sql-data-source-api-v2.md) and Spark Core's RDD API.

`DataSourceRDD` uses [DataSourceRDDPartition](spark-sql-DataSourceRDDPartition.md) for the [partitions](#getPartitions) (that is a mere wrapper of the [InputPartitions](#inputPartitions)).

## Creating Instance

`DataSourceRDD` takes the following to be created:

* <span id="sc"> `SparkContext`
* <span id="inputPartitions"> [InputPartition](InputPartition.md)s
* <span id="partitionReaderFactory"> [PartitionReaderFactory](connector/PartitionReaderFactory.md)
* <span id="columnarReads"> `columnarReads` flag

`DataSourceRDD` is created when:

* `BatchScanExec` physical operator is requested for an [input RDD](physical-operators/BatchScanExec.md#inputRDD)

* `MicroBatchScanExec` physical operator is requested for an `inputRDD`

## <span id="getPreferredLocations"> Preferred Locations For Partition

```scala
getPreferredLocations(
    split: Partition): Seq[String]
```

`getPreferredLocations` simply requests the given `split` <<spark-sql-DataSourceRDDPartition.md#, DataSourceRDDPartition>> for the <<spark-sql-DataSourceRDDPartition.md#inputPartition, InputPartition>> that in turn is requested for the [preferred locations](InputPartition.md#preferredLocations).

`getPreferredLocations` is part of Spark Core's `RDD` abstraction.

## <span id="getPartitions"> RDD Partitions

```scala
getPartitions: Array[Partition]
```

`getPartitions` simply creates a <<spark-sql-DataSourceRDDPartition.md#, DataSourceRDDPartition>> for every <<inputPartitions, InputPartition>>.

`getPartitions` is part of Spark Core's `RDD` abstraction.

## <span id="compute"> Computing Partition (in TaskContext)

```scala
compute(
    split: Partition,
    context: TaskContext): Iterator[T]
```

`compute` requests the input <<spark-sql-DataSourceRDDPartition.md#, DataSourceRDDPartition>> (the `split` partition) for the <<spark-sql-DataSourceRDDPartition.md#inputPartition, InputPartition>> that in turn is requested to [create an InputPartitionReader](InputPartition.md#createPartitionReader).

`compute` registers a Spark Core `TaskCompletionListener` that requests the `InputPartitionReader` to close when a task completes.

`compute` returns a Spark Core `InterruptibleIterator` that requests the `InputPartitionReader` to <<spark-sql-InputPartitionReader.md#next, proceed to the next record>> (when requested to `hasNext`) and <<spark-sql-InputPartitionReader.md#get, return the current record>> (when `next`).

`compute` is part of Spark Core's `RDD` abstraction.
