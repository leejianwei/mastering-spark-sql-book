# ProjectEstimation

`ProjectEstimation` is...FIXME

=== [[estimate]] Estimating Statistics and Query Hints of Project Logical Operator -- `estimate` Method

[source, scala]
----
estimate(project: Project): Option[Statistics]
----

`estimate`...FIXME

NOTE: `estimate` is used exclusively when `BasicStatsPlanVisitor` is requested to spark-sql-BasicStatsPlanVisitor.md#visitProject[estimate statistics and query hints of a Project logical operator].
