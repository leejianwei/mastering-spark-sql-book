title: TruncateTableCommand

# TruncateTableCommand Logical Command

`TruncateTableCommand` is a spark-sql-LogicalPlan-RunnableCommand.md[logical command] that represents spark-sql-SparkSqlAstBuilder.md#visitTruncateTable[TRUNCATE TABLE] SQL statement.

=== [[creating-instance]] Creating TruncateTableCommand Instance

`TruncateTableCommand` takes the following to be created:

* [[tableName]] `TableIdentifier`
* [[partitionSpec]] Optional `TablePartitionSpec`

=== [[run]] Executing Logical Command -- `run` Method

[source, scala]
----
run(spark: SparkSession): Seq[Row]
----

NOTE: `run` is part of spark-sql-LogicalPlan-RunnableCommand.md#run[RunnableCommand Contract] to execute (_run_) a logical command.

`run`...FIXME

`run` throws an `AnalysisException` when executed on spark-sql-CatalogTable.md#tableType[external tables]:

```
Operation not allowed: TRUNCATE TABLE on external tables: [tableIdentWithDB]
```

`run` throws an `AnalysisException` when executed on spark-sql-CatalogTable.md#tableType[views]:

```
Operation not allowed: TRUNCATE TABLE on views: [tableIdentWithDB]
```

`run` throws an `AnalysisException` when executed with <<partitionSpec, TablePartitionSpec>> on spark-sql-CatalogTable.md#partitionColumnNames[non-partitioned tables]:

```
Operation not allowed: TRUNCATE TABLE ... PARTITION is not supported for tables that are not partitioned: [tableIdentWithDB]
```

`run` throws an `AnalysisException` when executed with <<partitionSpec, TablePartitionSpec>> with spark-sql-DDLUtils.md#verifyPartitionProviderIsHive[filesource partition disabled or partition metadata not in a Hive metastore].
