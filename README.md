# Hadoop
Hadoop installation and code examples

## Hadoop installation
Download hadoop binary from web site: http://hadoop.apache.org/releases.html

Unzip the file and copy it to a directory on our computer.

```
sudo tar xzf hadoop-2.2.0.tar.gz
mv hadoop-2.2.0 /usr/local/
mv /usr/local/hadoop-2.2.0 /usr/local/hadoop
```
Create a user, assign the password and add the user to the sudoers file, etc.
```
useradd -d /home/hadoop -m hadoop
passwd hadoop
usermod -a -G sudo hadoop
usermod -s /bin/bash hadoop
```
Log in as hadoop user.

SSH configuration and key generation
```
sudo apt-get install ssh
ssh-keygen -t rsa -f ~/.ssh/id_rsa
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
```
Add the following commands in the file ~/.bashrc
```
export HADOOP_HOME=/usr/local/hadoop 
export PATH=$PATH:$HADOOP_HOME/bin
export PATH=$PATH:$HADOOP_HOME/sbin
export HADOOP_MAPRED_HOME=${HADOOP_HOME}
export HADOOP_COMMON_HOME=${HADOOP_HOME}
export HADOOP_HDFS_HOME=${HADOOP_HOME}
export YARN_HOME=${HADOOP_HOME}
```
Set enviroment variables in hadoop-env.sh
```
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64/
export HADOOP_OPTS="$HADOOP_OPTS -Djava.net.preferIPv4Stack=true"
```
## Example: WordCount in local mode

```
cp $HADOOP_HOME/*.txt input 
ls -l input 
hadoop jar /usr/local/hadoop/share/hadoop/mapreduce/hadoop-mapreduce-examples-3.0.0.jar wordcount input output
```
Check result
```
$cat output/* 
```

## Example: Runing Hadoop in local as straming mode

Note: For find hadoop streaming jar you can use: find / -name 'hadoop-streaming*.jar'

```
yarn jar $HADOOP_STREAMING_JAR -mapper 'wc -l' -numReduceTasks 0 -input input -output wc_mr
yarn jar $HADOOP_STREAMING_JAR -mapper 'wc -l' -reducer './reducer.sh' -file reducer.sh -numReduceTasks 1 -input input -output wc_m
yarn jar $HADOOP_STREAMING_JAR -mapper 'wc -l' -reducer "awk '{line_count += \$1} END { print line_count }'" -numReduceTasks 1 -input input -output wc_m
```

## Hadoop in pseudo distributed mode

Change conf/core-site.xml to
```
<configuration>
<property>
<name>fs.default.name</name>
<value>hdfs://localhost:9000</value>
</property>
</configuration>
```
Change conf/hdfs-site.xml to
```
<configuration>
<property>
<name>dfs.replication</name>
<value>1</value>
</property>
</configuration>
```
Change conf/mapred-site.xml to
```
<configuration>
<property>
<name>mapred.job.tracker</name>
<value>localhost:9001</value>
</property>
</configuration>
```
Format the name node:
```
bin/hadoop namenode –format
```
Start the all the demons:
```
bin/start–all.sh
```
Web Information

http://localhost:8088/cluster/nodes

http://localhost:50070/explorer.html#/

Guide for this Configuration: https://www.codementor.io/lakshaynagpal/how-to-setup-hadoop-pseudo-distributed-mode-single-cluster-du10831te

# Hadoop from eclipse

https://www.youtube.com/watch?v=D0uv8fRrutY

Plug in: https://github.com/Ravi-Shekhar/hadoop-eclipse-plugin/projects

MR port 8088 

DFS port 8020

```
hadoop jar /home/max/eclipse-hadoop/WordCount.jar /user /output
```

# References

https://www.tutorialspoint.com/es/hadoop/hadoop_enviornment_setup.htm

https://www.adictosaltrabajo.com/tutoriales/hadoop-first-steps/#03
