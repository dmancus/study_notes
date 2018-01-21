# Zookeeper

This is a pre-requisite for Kafka  
Java 1.6+ is a pre-requisite for Zookeeper  
https://zookeeper.apache.org/doc/r3.3.3/zookeeperStarted.html  

# Get Up And Running
1. Install Java JDK
2. Set Java heap size
3. Download ZooKeeper
```
    http://ftp.jaist.ac.jp/pub/apache/zookeeper/
    3.4.10 is stable release as of 2018-01-20
    ```
    ```
    tar -xvf zookeeper-3.4.10.tar.gz
    cd zookeeper-3.4.10
    ```
4. Config file with ticketTime, dataDir, clientPort, initLimit, syncLimit, and server list
```
    tickTime=2000
    dataDir=/var/zookeeper
    clientPort=2181
```
5. myid file that tells the id of the zookeeper on current host - not needed for single host dev instance
6. Start Zookeeper
```
$ bin/zkServer.sh start
ZooKeeper JMX enabled by default
Using config: /home/david/Downloads/software/zookeeper-3.4.10/bin/../conf/zoo.cfg
Starting zookeeper ... STARTED
```
7. Test connection?


# Now it's up and running, let's use it
1. Connect to the zookeeper server
```
$ bin/zkCli.sh -server 127.0.0.1:2181
$ help
$ ls /
```
2. Create a znode
```
$ create /zk_test my_data
$ ls /
$ get /zk_test
```
3. Modify data associated with a znode
```
$ set /zk_test new_text
$ get /zk_test
```
4. Delete zk_test
```
$ delete /zk_test
$ ls /
```

# Modifications for running as a cluster or Ensemble
```
tickTime=2000
dataDir=/var/zookeeper
clientPort=2181
initLimit=5
syncLimit=2
server.1=zoo1:2888:3888
server.2=zoo2:2888:3888
server.3=zoo3:2888:3888
```
- tickTime - unit of time in milliseconds, so 2 seconds here
- initLimit - number of tickTime units allowed for establishing connections between peers
- syncLimit - number of tickTime units allowed to get out of syncLimit
- server.x - "x" is the id of the zookeeper setup in the ensemble.  This is identified by putting the *myid* file in the root of dataDir, containing the ascii id, so server.1 should have a myid file with "1" in it
- 2 port numbers - one is for making connections to peers, second one is needed for leader election
