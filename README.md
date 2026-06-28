# Assignment-6-Spark-architecture
Advanced Architecture of Apache Spark and Data Engineering Pipeline (Week 6)
1. Purpose of The Week's Lesson
The goal of this week is to learn about the various components that make up Apache Spark's core cluster architecture and to review how Lazy Evaluation or "the mechanics of laziness" (based on DAG diagrams) performs as well as how to define an Optimized Processing Pipeline that uses the structural differences between Row-Based (CSV) and Columnar (Parquet) Storage Engines for storing data.

2. Fundamental Architecture Components
Cluster Topological Elements
Driver Node - The Driver Node runs the user application (code) and coordinates all execution through the Task Scheduler, creates the Logical Execution Lineage from which the DAG is built, and communicates with the Worker Nodes (through the Task Scheduler) as to how the application should be executed on the Worker Nodes.

Cluster Manager Responsibility: Allocate the application's physical cluster capacity (RAM/CPU core).

Executor Nodes Are agents for processing tasks; they reside on worker nodes, accept individual tasks from the Driver, execute memory-local transformations, and write data back to disk.

Lazy Evaluation and the Lineage Graph (DAG);
In contrast to executing data transformations one transformation at a time, Spark’s transformations create a Directed Acyclic Graph (DAG) of the transformations to be executed. The actual plan will only be executed when some ACTION happens (e.g., .show() or .write()) that causes the execution to take place; during this execution planning period, the engine will have an opportunity to not load unnecessary rows into memory and automatically combine multiple operations.

Disk Usage: CSV Files Vs. Parquet Files
CSV (Row-oriented design) -
All rows and columns have to be read from your file when calculating values for any one column. If you are only calculating values for one column in your output, it is very slow to calculate.
Parquet (Column-oriented Binary) -
It is designed specifically for analytical processes. Parquet stores each value in a separate column and therefore allows Spark to load only the exact variables requested. It provides built-in support for.
Predicate Pushdown, which will skip over record blocks in your files that do not contain relevant records before using network and memory resources.
3. Implementation Step Summary
Engine Deployment: Established an active analytical SparkSession array.
Column Projection Optimization: Announced to limit intermediate data traffic by only selecting a limited number of full-size columns to be projected out of the dataset (minimized shuffling footprint).
Schema Enforcement: Standardized naming convention by replacing any whitespace format control character with a clear underscore and explicitly converting all values to numeric (e.g., to integer or double).
Derived Metric Generation: Derived a KPI metric as a floating-point value (i.e., Profit_Margin) as an inline conditional KPI metric within the process of executing the batch job that created the above two derived metrics.
Multi-Format Ingestion Strategy: Saved the finished transformed matrices into two formats; (1) traditional flat-format CSV files; (2) very high-density column format (in comparison with CSV formats) Parquet files.
4. Engineering Insights and Best Practices for Execution:
Do not utilize .collect() in production: Avoid using .collect() to pull all partitions from a distributed cluster back to the driver node. Pulling large datasets will cause the driver (or master) process to run out of memory (OOM) and crash. Instead utilize containerized safe analytical display actions, such as .show(n).
Utilizing columnar formats: Transitioning analytic records from raw text formats (CSV) into production column formats (Parquet) will improve performance during scans because columnar data types are inherently more efficient. And columnar formats reduce file storage requirements using binary dictionary compression schema, which will allow hardware pushdown capabilities at runtime.
