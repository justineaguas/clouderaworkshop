# 01 - Ingest csv files using Flume
### Create needed directories
Project directory:
```
mkdir /home/cloudera/presentation
```
Landing directory where the CSV file will be dumped to be read by Flume
```
mkdir /home/cloudera/presentation/dump
```
HDFS directory where Flume will load the data
```
/user/cloudera/flumecsv
```

### Create a .conf file that uses the Flume spoolDir

```
cat > flumetocsv.conf
```
Paste this to be written in flumetocsv.conf
```
#Flume Configuration Starts
# Define a file channel called fileChannel on agent1
agent1.channels.fileChannel1_1.type = file
# on linux FS
agent1.channels.fileChannel1_1.capacity = 200000
agent1.channels.fileChannel1_1.transactionCapacity = 1000
# Define a source for agent1
agent1.sources.source1_1.type = spooldir
# on linux FS
#Spooldir in my case is /home/cloudera/presentation/dump
agent1.sources.source1_1.spoolDir = /home/cloudera/presentation/dump
agent1.sources.source1_1.fileHeader = false
agent1.sources.source1_1.fileSuffix = .COMPLETED
agent1.sinks.hdfs-sink1_1.type = hdfs

#Sink is /flume_import under hdfs

agent1.sinks.hdfs-sink1_1.hdfs.path = /user/cloudera/flumecsv
agent1.sinks.hdfs-sink1_1.hdfs.batchSize = 10
agent1.sinks.hdfs-sink1_1.hdfs.rollSize = 0
agent1.sinks.hdfs-sink1_1.hdfs.rollInterval = 0
agent1.sinks.hdfs-sink1_1.hdfs.rollCount = 5
agent1.sinks.hdfs-sink1_1.hdfs.writeFormat=Text

agent1.sinks.hdfs-sink1_1.hdfs.fileType = DataStream
agent1.sources.source1_1.channels = fileChannel1_1
agent1.sinks.hdfs-sink1_1.channel = fileChannel1_1

agent1.sinks =  hdfs-sink1_1
agent1.sources = source1_1
agent1.channels = fileChannel1_1
```
### Start the Flume agent using the .conf file
```
flume-ng agent -n agent1 -f flumecsv.conf
```
Note: As long as Flume agent is running, it will just wait for new files to be picked up and will automatically tag completed files

### Download sample files into the landing directory
```
cd /home/cloudera/presentation/dump
wget https://data.sfgov.org/resource/rjid-nys3.csv
```
### Flume agent should be able to pick up the file and load it into HDFS
![alt text](https://github.com/justineaguas/clouderaworkshop/blob/master/flumelogs.JPG)
### Check file in HDFS
```
[cloudera@quickstart presentation]$ hdfs dfs -ls /user/cloudera/flumecsv
Found 1 items
-rw-r--r--   1 cloudera cloudera     301311 2018-11-22 03:48 /user/cloudera/flumecsv/FlumeData.1542887286229
```
Proceed to next steps to read the data in Hive
