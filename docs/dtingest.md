# dtIngest Tutorial

"dtIngest" is a datatorrent application that can read data from various
sources and store the processed data to various sinks. The data movement
happens at scale and in parallel. To know more about dtIngest please
refer the [dtIngest
blog](https://www.datatorrent.com/dtingest-unified-streaming-batch-data-ingestion-hadoop/).

This tutorial refers to dtIngest version 1.0.0

## Pre-requisites

-   Datatorrent RTS should be installed on your hadoop cluster. Please
    refer [Installation
    guide](https://www.datatorrent.com/docs/README.html) for
    installation steps.

-   Source and destination file systems should be accessible from all
    nodes of the cluster running DataTorrent RTS. It can be any of HDFS,
    NFS, S3, FTP. For sandbox image or single node hadoop cluster you can also use local
    files as source. But, for multi-node cluster local files cannot be used as source.

-   In case your source or destination file system is NFS; then NFS
    should be mounted on all the nodes within the hadoop cluster at a
    common mount point and should have read/write permission to the user
    running dtIngest application.

## Launching ingestion application

dtIngest application can be configured and launched from [Datatorrent
Management
Console](https://www.datatorrent.com/docs/guides/ConsoleGuide.html).

1.  Navigate to 'Develop' tab.
    ![Screenshot from 2015-09-24 16:33:45.png](images/dtingest/image45.png)

1.  The dtIngest application package is already uploaded and available
    to use under 'Application Packages' section. 

1.  Select 'Ingestion Application' from list of App packages. And click
    on ‘launch application’ button.
    ![Screenshot from 2015-09-24 16:37:23.png](images/dtingest/image13.png)

1.  Configuration page for dtingest is displayed after the 'launch'.
    Just provide desired values for the configuration and click
    'Launch' to ingest your data.

## Configuring dtingest instance properties

1.  In the 'Name this application' textbox; enter suitable name of
    your choice for this ingestion instance. For example, 'Ingestion
    test'
    ![Screenshot from 2015-10-15 13:58:10.png](images/dtingest/image33.png)

1.  Under 'Specify a queue' option; keep the 'Specify a queue'
    checkbox unselected if you want to use default queue.

    If you want to specify some specific queue to launch this application; then
    select the 'Specify a queue' checkbox and select desired queue from the
    dropdown. To know more about this read [Hadoop Capacity Scheduler Docs](https://hadoop.apache.org/docs/r2.4.1/hadoop-yarn/hadoop-yarn-site/CapacityScheduler.html)
    ![Screenshot from 2015-10-15 15:20:19.png](images/dtingest/image10.png)

1.  Under 'Use config a file' option; select the checkbox if you wish
    to use some configuration which is already saved. In the drop down
    select the desired configuration.
    After selection, all the input options will load the values saved in the configuration.
    ![Screenshot from 2015-10-15 16:10:03.png](images/dtingest/image32.png)

    After values are loaded you can modify the values of any option to the
    desired value. You can also save this new combination of values as a
    configuration under the same name or different name.

    If you want to specify all the options from scratch without using any pre-saved configuration; then unselect a checkbox 'Use a config file'

1.  Configure input source, refer to [Configuring input
    source](#configuring-input-source) section for details.

1.  Configure output destination, refer to [Configuring output
    destination](#configuring-output-destination) section for details.

1.  Configure processing steps, refer to [Configuring processing
    steps](#configuring-processing-steps) section for details.

1.  Under 'Save Configuration file' give name for configuration; if
    you wish to save this combination of values for future use.
    You may keep this blank if you do not want to save this for future use. 

## Configuring input source

### Configuring HDFS input

1.  For 'Input data source' field; select 'HDFS' option from the
    drop-down.  
    ![](images/dtingest/image26.png)

2.  Under 'Source directories'; specify complete URL for the file path
    to be ingested.  
    For example, if the namenode is 'namenode1.cluster.company.org' and
    port is '8020' and file path is ''/user/john/data' then complete URL in
    this case will be
    `hdfs://namenode1.cluster.company.org:8020/user/john/data`.  
    Where,
    -   `hdfs://` indicates HDFS protocol
    -   `namenode1.cluster.company.org` indicates fully qualified domain
       name for the namenode of source HDFS.
    -    `8020` indicates port number for HDFS namenode service
    -  `/user/john/data` indicates full path for destination directory  

    ![](images/dtingest/image14.png)

    If there are more than one directories/file to be ingested; click on
    'Add directory' button. Then specify complete URL for the file path to be
    ingested as specified above

1.  In the 'Filtering criteria' field; specify regular expression for
    files to be copied.  For regular expression syntax, please refer to
    [Java regular expression  documentation](http://docs.oracle.com/javase/7/docs/api/java/util/regex/Pattern.html).
    For example, if only `.log` files need to be ingested; then use `.*\.log` as regular expression.  

    ![Screenshot from 2015-09-24 19:36:18.png](images/dtingest/image02.png)

    Where,
    -   `.*` indicates any character zero or more times
    -   `\.` indicates dot escaped with backslash
    -   `log` indicates desired extension which is 'log'

    In this case, dtingest ingests only '.log' files from the source
directories.

1.  Under 'Runs' field, select 'Single run' if you expect
    application to shutdown after ingesting files currently present in
    the directory.

    Select 'Polling' if you expect application to periodically poll the
    directory/file for the changes. Change of file is detected based on
    'modification timestamp'. Entire file will be ingested again in case of
    any change.

    If 'Polling' mode is selected; then 'Polling interval' should be specified.
    This is the time interval between sub-sequent scans for detecting
    new/modified files.  

    ![Screenshot from 2015-09-24 20:03:57.png](images/dtingest/image17.png)

### Configuring NFS input

1.  For 'Input data source' field; select 'File/NFS' option from the
    drop-down. ![Screenshot from 2015-09-24 15:23:24.png](images/dtingest/image31.png)

1.  Under 'Source directories'; specify complete URL for the file path
    to be ingested.

    For example, if the NFS mount is located at '/disk5/nfsmount' and
    'path/to/data/directory' is the directory under this mount which needs
    to be ingested; then complete URL in this case will be `file:///disk5/nfsmount/path/to/data/directory`.
    Where,
    -   `file://` indicates that it is some file system mounted on the node.
    -   `/disk5/nfsmount/` indicates the mount point. Note that, this has
    to be uniform across all the nodes in the cluster
    -   `path/to/data/directory` is the directory to be ingested

    ![Screenshot from 2015-09-24 16:41:29.png](images/dtingest/image37.png)

    Note that, for the above example, there should be `///` (triple slash)
    after `file:`.

1.  If there are more than one directories to be ingested; click on
    'Add directory' button. Then specify complete URL for the file
    path to be ingested as specified in point 3.

1.  If there are specific files to be ingested; then specify complete
    URL for the file path to be ingested.

    For example,  if some other NFS mount is located at '/disk6/nfsmount2' and 'path/to/file/to/copy/datafile.txt' is a file under this mount which needs
    to be ingested; then complete URL in this case will be `file:///disk6/nfsmount2/path/to/file/to/copy/datafile.txt`.

    ![Screenshot from 2015-09-24 17:36:06.png](images/dtingest/image39.png)

1.  In the 'Filtering criteria' field; specify regular expression for
    files to be copied.
    For example, if only `.log` files need to be ingested; then use `.*\.log` as regular expression.

    ![Screenshot from 2015-09-24 19:36:18.png](images/dtingest/image02.png)

    Where,
    -   `.\*` indicates any character zero or more times
    -   `\\.` indicates dot escaped with backslash '\\'
    -   `log` indicates desired extension which is 'log'

    In this case, dtingest ingests only `.log` files from the source directories.

1.  Under 'Runs' field, select 'Single run' if you expect application
    to shutdown after ingesting files currently present in the directory.

    Select 'Polling' if you expect application to periodically poll the
    directory/file for the changes. Change of file is detected based
    on 'modification timestamp'. Entire file will be ingested again in case
    of any change.

    If 'Polling' mode is selected; then 'Polling interval' should be specified.
    This is the time interval between sub-sequent scans for detecting
    new/modified files.

    ![Screenshot from 2015-09-24 20:03:57.png](images/dtingest/image17.png)

### Configuring FTP input

FTP is most widely used transfer protocol, used for transfer files from
one host to another. Use dtIngest, to copy files/directory from ftp
source location to some destination. This section gives details about
how to ingest files/directories from FTP using dtIngest.  

1.  Select  FTP as input type
    ![FTPInput.png](images/dtingest/image19.png)

1.  After selecting the FTP as input type, snapshot of UI as below:     ![FTP1.png](images/dtingest/image15.png)

1.  Configure FTP input url
    Input url for FTP needs to be provided in following format,  `ftp://username:password@host:port/path`
    where,
    - `ftp` :  protocol name
    - `username` :  username for ftp server
    - `password` : password
    - `host` : FTP host
    - `port` : port number
    - `path` : path to either file / directory

    ![FTP2.png](images/dtingest/image05.png)

    If you want to copy multiple files/directories, click on (+) button. If
you are copying multiple files, then UI would be as follows:
    ![ftp2.png](images/dtingest/image03.png)

    If you are copying multiple directories, then UI would be as follows:
    ![ftp3.png](images/dtingest/image24.png)

### Configuring Amazon S3 input

S3 is clustered storage service of Amazon. Amazon s3 provides a web
services interface that can be used to store and retrieve any amount of
data, at any time, from anywhere on web. For more details about Amazon
S3, please refer to [Amazon S3
Documentation](https://aws.amazon.com/documentation/s3/).

Use dtIngest, you can copy files/directory efficiently from S3 source
location to some destination. This section gives details about how to
ingest files/directories from S3 using dtIngest.  

1.  Select  S3 as input type
![S3Input.png](images/dtingest/image30.png)

1.  After selecting the S3 as source type then UI looks like as below:
![S3\_1.png](images/dtingest/image16.png)

1.  Configure S3 input url.

    Input url for S3 needs to be provided in following format,  
    `s3n://ukey:upass@bucketName/path`

    where,
    - `s3n`:  protocol name
    - `ukey`: access key
    - `upass`: secret access key
    - `bucketName` : bucketName
    - `path` : path to either file / directory

  ![S3Input1.png](images/dtingest/image41.png)
  If you want to copy multiple directories, then click on (+) button and
specify the url’s, UI would be as below:
  ![s3\_2.png](images/dtingest/image48.png)

### Configuring Kafka input

Kafka is a distributed publish subscribe messaging system. Since kafka
is distributed system, topics are partitioned and replicated across
nodes. Messages in kafka are expired after some time. For more details
about Kafka, please refer to [Apache Kafka
Documentation](http://kafka.apache.org/documentation.html).

We need to copy messages from multiple partitions to one destination
before expiry. This section gives details about how to ingest messages
from kafka using dtIngest.

1.  Select kafka as input type
    ![kafkaInput1.png](images/dtingest/image46.png)

1.  After selecting kafka as input type then UI looks like as below:
    ![Kafka1.png](images/dtingest/image23.png)

1.  Configure topic name and zookeeper quorum.
    zookeeper quorum  is a string in the form of
    `hostname1:port1,hostname2:port2,hostname3:port3`

    where,
    
    - `hostname1,hostname2,hostname3` are hosts
    - `port1,port2,port3` are ports of zookeeper server

    e.g. localhost:2181,localhost:2182
    ![Kafka.png](images/dtingest/image44.png)

1.  Select the offset type. By default, “Latest” offset is enabled. If
    you want to consume messages from beginning of kafka queue, then
    select “Earliest” offset option.

1.  If the topic name is same across the kafka clusters and want to
    ingest data from these clusters, then configure the zookeeper quorum
    as follows:

    `c1::hs1:p1,hs2:p2,hs3:p3;c2::hs4:p4,hs5:p5,c3::hs6:p6`

    where,
    - `c1,c2,c3` indicates the cluster names,
    - `hs1,hs2,hs3,hs4,hs5,hs6` are zookeeper host names
    - `p1,p2,p3,p4,p5,p6` are corresponding ports.

    For example, ClusterA and ClusterB are 2 kafka clusters as below, then
    zookeeper quorum would be as `ClusterA::node3.example.com:2181,node4.example.com:2181;ClusterB::node8.example.com:2181`

    ![KafkaCluster.png](images/dtingest/image04.png)

### Configuring JMS input

JMS provides the facility to create, send and read messages. You can
copy messages from JMS source location to some destination using
dtIngest. This section gives details about how to ingest messages from
JMS using dtIngest.  

1.  Select JMS as input type.
    ![JMSInput.png](images/dtingest/image18.png)

1.  After selecting the JMS as source type then UI looks like as below:
    ![jms1.png](images/dtingest/image25.png)

1.  Configure Broker URL and topic name.
    Broker url would be in the form of tcp://hostName:port
    ![JMS2.png](images/dtingest/image38.png)

## Configuring output destination

### Configuring HDFS output

1.  For 'Output Location' field; select 'HDFS' option from the
    drop-down.

    ![Screenshot from 2015-09-24 20:08:49.png](images/dtingest/image08.png)

1.  Under 'Target directory' specify complete URL for the HDFS path
    for the destination directory. For example, URL should be like
`hdfs://namenode1.cluster.company.org:8020/user/username/path/to/destination/directory`

    ![Screenshot from 2015-09-24 20:51:55.png](images/dtingest/image47.png)

    Where,
    - `hdfs://` indicates HDFS protocol
    - `namenode1.cluster.company.org` indicates fully qualified domain name for
    the namenode of destination HDFS.
    - `:8020` indicates port number for HDFS namenode service
    - `/user/username/path/to/destination/directory` indicates full path
    for destination directory.

1.  Under 'Recursive copy' option, select 'Yes' if you wish to copy
    entire directory structure under source directory to the
    destination. Select 'No' if you want non-recursive copy.

1.  Under 'Overwrite conflicting files' option, select 'Yes' if you
    wish to overwrite the file at the destination if file with the same
    name is discovered under input source.

#### Compact files

If you want to copy the data to the HDFS and partition the data into
partitions of fixed size; use 'Compact files' feature to do this. This
can be used to combine large number of small files into partitions of
manageable size. Additionally, you may also use this to breakdown very
large file into partitions of manageable size.

  ![Screenshot from 2015-10-16 11:04:39.png](images/dtingest/image28.png)

1.  Select 'yes' for radio button under 'Compact files' option. This
    will display other additional options for compaction. If you do not
    want to compact files but copy them as they are; then select 'no'
    for 'Compact files' option. If you select 'no' ; additional
    options for compaction will be hidden.

2.  Select delimiter to be used for separating contents of the files.
    This will be useful if you decide to use some custom logic for
    parsing partition files. Default value for 'delimiter' option is
    'none'. You can use new line or any other custom delimiter based
    on your requirement. Note that, special characters in the custom
    delimiter should be escaped with `\`. For example, tab character
    `\t` should be specified as `\\t`.

3.  Specify the size for each partition under 'Max compacted file
    size'. You can specify partition size in bytes, MB, GB. Data will
    spill over to the next partition once this size is reached.

Note that, partition will be of exact sizes in case of continuous
incoming data. If there is no incoming data for consecutive 600 windows
then that partition will be committed to the HDFS. In this case, new
incoming data will be spilled to the next partition.

### Configuring NFS output

1.  For ‘Output Location’ field; select ‘File/NFS’ option from the
    drop-down  

    ![](images/dtingest/image00.png)

1.  Under ‘Target directory’ specify complete URL for NFS path for
    destination directory.  
    For example,  if the NFS mount is located at '/disk5/nfsmount' and
    'path/to/data/directory' is the directory under this mount which
    needs to be ingested; then complete URL in this case will be
    `file:///disk5/nfsmount/path/to/data/directory`

    Where,

    - `file://` indicates that it is some file system mounted on the node.
    - `/disk5/nfsmount/` indicates the mount point.
    Note that, this has to be uniform across all the nodes in cluster.
    - `path/to/data/directory` is the directory to be ingested

    ![](images/dtingest/image11.png)

    Note that, for the above example, there should be `///` (triple slash)
    after `file:`.

### Configuring FTP output

1.  Select FTP as output type.
    ![FTPOutput1.png](images/dtingest/image34.png)

1.  After selecting FTP as output type then UI looks like as below:   ![FTPOUtput3.png](images/dtingest/image42.png)

1.  Specify the destination url below the “Output directory” label.
    Output url for FTP needs to be provided in following format,  `ftp://username:password@host:port/path`

    Where,
    - `ftp` :  protocol name
    - `username` : username for ftp server
    - `password` : password
    - `host` : FTP host
    - `port` : port number
    - `path` : Directory path to ingested

    ![HFTP2.png](images/dtingest/image01.png)

### Configuring Amazon S3 output

1.  Select S3 as output type.
    ![S3Output1.png](images/dtingest/image09.png)

1.  After selecting S3 as output then UI looks like as below:
    ![H2S3\_12.png](images/dtingest/image36.png)

1.  Specify the destination url below the 'Output directory' label.
output url for S3 needs to be provided in following format,  
`s3n://ukey:upass@bucketName/path`
    Where,
    - `s3n` : protocol name
    - `ukey` : access key
    - `upass` : secret access key
    - `bucketName` :  bucketName
    - `path` : Directory path

### Configuring Kafka output

1.  Select kafka as output type.
    ![KafkaOutput1.png](images/dtingest/image35.png)

1.  After Selecting kafka as output then UI looks like as below:
    ![K2K2.png](images/dtingest/image22.png)

1.  Configure broker list and topic name.

### Configuring JMS output

1.  Select JMS as output type.
![JMSOutput1.png](images/dtingest/image20.png)

1.  After selecting JMS as output type then UI looks like as below:
![K2J1.png](images/dtingest/image21.png)

1.  Configure Broker URL and topic name. Broker URL would be in the form of tcp://host:port
![K2J2.png](images/dtingest/image40.png)

## Configuring processing steps

### Configuring compression

Select compression type on configuration page         
![](images/dtingest/image07.png)

1. Select LZO radio button to apply LZO compression  
![](images/dtingest/image43.png)

  Lzo compression is not directly supported. To use lzo compression provide
  plugin to ingestion app which provides lzo implementation and extends from java FilterOutputStream class. Copy plugin to \~/.dt/plugins folder
  (i.e. HOME\_DIR/.dt/plugins) of the user who launches ingestion app.
  We do ship default lzo plugin, and is available to download on maven repository at
https://oss.sonatype.org/content/repositories/releases/com/datatorrent/dtIngest-lzo/1.0.0/

1. Select GZIP radio button to apply GZIP compression
![](images/dtingest/image12.png)

### Configuring encryption

Select type of encryption on configuration page.
![](images/dtingest/image27.png)

1.  Apply AES encryption:
  1. Select AES radio button to apply AES encryption

  2. Provide AES symmetric encryption key in “AES key” text box  
     Note: AES symmetric key should be of size 128, 192 or 256 bits.
![](images/dtingest/image06.png)

1.  Apply PKI encryption:  
    1. Select PKI encryption button to apply PKI encryption  
    1. Provide Asymmetric public key to be used for PKI encryption  

    ![](images/dtingest/image29.png)
