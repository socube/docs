Glossary of Terms
================================================================================

Apache Apex
-----------

- **Apache Hadoop** -  [Apache Hadoop](http://hadoop.apache.org/) is a programming framework that supports the processing of large data sets in a distributed computing environment.
- **YARN** - [Apache Hadoop YARN](http://hadoop.apache.org/docs/current2/hadoop-yarn/hadoop-yarn-site/YARN.html) (Yet Another Resource Negotiator) is a cluster resource management technology, introduced with Hadoop 2.0.
- **Resource Manager** - YARN component that allocates and arbitrates the resources such as CPU, Memory and Network.
- **Container** - A physical resource with CPU and memory constraints allocated by YARN’s Resource Manager.
- **Application** - unified batch and real-time stream processing application running on Apache Apex platform.
- **Operator** - An entity that holds a computational logic to process the data tuples. It is part of a real-time stream processing application. The Operator computational logic gets executed inside a YARN Container.
- **Physical Operator** - Physical representation of the operator, which contains information such as the name of container and the Hadoop node where operator instance is running.
- **Port** - Each operator can have ports on which it can receive input data tuples and also output processed data tuples.
- **Stream** - A stream consists of data tuples that flow from one port of an operator to another.
- **STRAM** - Streaming Application Manager is the first process that is activated upon application launch and orchestrates the deployment, management, and monitoring of the Apache Apex applications throughout their lifecycle.
- **Logical Plan** - Logical representation of an Apache Apex application, where the computational nodes are called *Operators* and the data-flow edges are called *Streams*.
- **Physical Plan** - Physical representation of the Logical Plan of the application and is a blueprint of how the application will run inside YARN containers deployed across nodes in a Hadoop cluster.
- **DAG** - Directed Acyclic Graph, formed by a collection of vertices and directed edges without cycles.  Sometimes interchangeably used to describe Apache Apex applications, specifically referring to their *Logical* / *Physical* plans, composed of operators connected by streams.
- **Data Tuples Processed** - Number of data objects processed by the operators in an Apache Apex application.
- **Data Tuples Emitted** - Number of data objects emitted by the operators with an output port.
- **Current Window Id** - Sequentially increasing identifier for a specific computation time period within Apache Apex platform.
- **Recovery Window Id** - Identifier for the last computational window at which the operator state was check-pointed into HFDS.

DataTorrent RTS
---------------

- **dtGateway** - HTTP server used by dtManage to interact with STRAM, YARN Resource Manager, and HDFS
- **dtManage** - the web based interface to install, configure, manage & monitor Apache Apex applications running in a Hadoop Cluster
- **dtAssemble** - graphical application assembly tool used to develop applications.
- **dtDashboard** - graphical visualization tool to view and query system and application data.
