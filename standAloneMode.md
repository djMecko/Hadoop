# Hadoop installation in Stand-Alone mode

## Step1: Install Java

```
$ sudo apt-get update
```
```
$ sudo apt-get install default-jdk
```
```
$ java -version
```
```
Output
openjdk version "1.8.0_91"
OpenJDK Runtime Environment (build 1.8.0_91-8u91-b14-3ubuntu1~16.04.1-b14)
OpenJDK 64-Bit Server VM (build 25.91-b14, mixed mode)
```

## Step2: Installing Hadoop


```
$ wget http://apache.mirrors.tds.net/hadoop/common/hadoop-2.7.3/hadoop-2.7.3.tar.gz
```
```
$ tar -xzvf hadoop-2.7.3.tar.gz
```
```
$ sudo mv hadoop-2.7.3 /usr/local/hadoop
```

## Step3: Configuring Hadoop's Java Home

```
$ readlink -f /usr/bin/java | sed "s:bin/java::"
```
```
Output
/usr/lib/jvm/java-8-openjdk-amd64/jre/
```

To begin, open hadoop-env.sh:

```
$ sudo nano /usr/local/hadoop/etc/hadoop/hadoop-env.sh
```

### Option 1: Set a Static Value

```
#export JAVA_HOME=${JAVA_HOME}
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64/jre/
```

### Option 2: Use Readlink to Set the Value Dynamically
```
#export JAVA_HOME=${JAVA_HOME}
export JAVA_HOME=$(readlink -f /usr/bin/java | sed "s:bin/java::")
```
## Step 4: Running Hadoop

```
$ /usr/local/hadoop/bin/hadoop
```
The help means we've successfully configured Hadoop to run in stand-alone mode. 
```
Output
Usage: hadoop [--config confdir] [COMMAND | CLASSNAME]
  CLASSNAME            run the class named CLASSNAME
 or
  where COMMAND is one of:
  fs                   run a generic filesystem user client
  version              print the version
  jar <jar>            run a jar file
                       note: please use "yarn jar" to launch
                             YARN applications, not this command.
  checknative [-a|-h]  check native hadoop and compression libraries availability
  distcp <srcurl> <desturl> copy file or directories recursively
  archive -archiveName NAME -p <parent path> <src>* <dest> create a hadoop archive
  classpath            prints the class path needed to get the
  credential           interact with credential providers
                       Hadoop jar and the required libraries
  daemonlog            get/set the log level for each daemon
```
```
mkdir ~/input
```

```
cp /usr/local/hadoop/etc/hadoop/*.xml ~/input
```
Next, we can use the following command to run the MapReduce hadoop-mapreduce-examples program, a Java archive with several options. We'll invoke its grep program, one of many examples included in hadoop-mapreduce-examples, followed by the input directory, input and the output directory grep_example.

```
/usr/local/hadoop/bin/hadoop jar /usr/local/hadoop/share/hadoop/mapreduce/hadoop-mapreduce-examples-2.7.3.jar grep ~/input ~/grep_example 'principal[.]*'
```

```
Output
 . . .
        File System Counters
                FILE: Number of bytes read=1247674
                FILE: Number of bytes written=2324248
                FILE: Number of read operations=0
                FILE: Number of large read operations=0
                FILE: Number of write operations=0
        Map-Reduce Framework
                Map input records=2
                Map output records=2
                Map output bytes=37
                Map output materialized bytes=47
                Input split bytes=114
                Combine input records=0
                Combine output records=0
                Reduce input groups=2
                Reduce shuffle bytes=47
                Reduce input records=2
                Reduce output records=2
                Spilled Records=4
                Shuffled Maps =1
                Failed Shuffles=0
                Merged Map outputs=1
                GC time elapsed (ms)=61
                Total committed heap usage (bytes)=263520256
        Shuffle Errors
                BAD_ID=0
                CONNECTION=0
                IO_ERROR=0
                WRONG_LENGTH=0
                WRONG_MAP=0
                WRONG_REDUCE=0
        File Input Format Counters
                Bytes Read=151
        File Output Format Counters
                Bytes Written=37
```
Results are stored in the output directory and can be checked by running cat on the output directory:

```
$ cat ~/grep_example/*
```
```
Output
6       principal
1       principal.

```


