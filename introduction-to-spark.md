# Introduction to Spark

## What is Apache Spark?

Apache Spark is an _**open-source distributed general-purpose cluster-computing**_ framework. It is a _**fast, in-memory data**_ processing engine, which allows _**data workers**_ to efficiently execute _**SQL, Machine learning and Streaming**_ workload which require fast _**iterative access**_ to data.



## What is the difference between Hadoop and Spark?

**All major differences can be explained with how an Iterative Data Algorithms is used on the Data**

**Spark** is an _**in-memory data processing engine**_, hence spark to _**cache data**_ in the memory for further processing of the data. This enables spark to be highly efficient when processing data with _**iterative algorithms**_.

**Hadoop** application are traditional Map-Reduce jobs where data is passed via _**HDFS**_ across iterations. And hence the data cannot be cached in-memory.

**Spark** can process algorithms with multiple passes on data, where the result of the previous execution is used as input for the next pass.

**Hadoop** is limited by the means which the data can be shared between `(Map -> Reduce) => (Map -> Reduce)` jobs. `'=>'` the data flow is via HDFS.

**Spark** executor can be reused when performing iterative processing as executor cores will be replaced with new threads tasked in performing the new data processing. Spark creates a DAG of tasks and the data is in Memory and not written to Disk in between different tasks of the DAG.

**Hadoop** the JVM can be reused between MR tasks spawned on the same worker, however this is subtly different and needs knowledge of the Hadoop configuration parameter `"mapred.job.reuse.jvm.num.tasks"`.

## **What is the high level overview of Execution Model of Spark?**

* **Driver** This is the **main** program of the Spark Application. It **instantiates a SparkContext**. Based on the Spark Application code in the driver, A **logical plan** is created which is used to **create a DAG** which translates to an **Execution Plan** which is divided into **stages** consisting of **tasks** which will be **executed on the Workers**. There is no data processing happening in the driver.
* **Spark Context** This serves as an link or bridge which enables the Driver to communicate with the Cluster Manger. Some possible examples of SparkContext's use are **Negotiate Resources**, **Change memory, cores requested in executors**, **create RDDs on cluster, Create Accumulators, Create Broadcast variables**.
* **Cluster Manger** Is a resource manager which is aware of the distributed workers in the cluster and its resources. It also serves as a resource negotiator for the Spark Application. The cluster manger decides which tasks are scheduled on which worker nodes based on the data available on the node.
* **Workers** Are distributed nodes in the cluster, the workers have a certain capacity based on the Memory/CPU avaible on the machines.
* **Executors** Is an Java process, For example an JVM running in an Yarn container. This is where the data processing happens.
* **Executor Cores** Number of threads which each container can run to process the tasks
