*Big Data with Hadoop*

##### Table of Contents
- [Hadoop](#Hadoop)
  * [Installation](#Installation)
  * [Overview of Hadoop Ecosystem](#overview-of-hadoop-ecosystem)
  * [HDFS](#hdfs)
    + [Login using Ambari](#login-using-ambari)
    + [Login using Putty](#login-using-putty)
    + [Admin access for Ambari](#admin-access-for-ambari)
   
## Hadoop
* Hadoop is an **open source** software platform for **distributed storage** and **distributed processing** of **very large datasets** on **computer clusters** built from commodity hardware
* Why Hadoop?
  * Data is too big
  * Vertical scaling isn't an option
    * Disk seek times
    * Hardware failures
    * Processing times
  * Horizontal scaling is linear
  * You can do much more instead of just batch processing

## Installation
* Download Virtual Box from https://www.virtualbox.org/
* Download image of Hadoop to run on Virtual Box
  * (Horton Works Data Platform) **HDP 2.5 Sandbox** is preffered because it boots up faster than new versions
    * Download from https://hortonworks.com/downloads/#sandbox
* Import the image into Virtual Box
* Once you bootup, you will have CentOS instance that has Hadoop up and running
* We can use CLI, it also has browser interface
  * **Ambari** is available to easily navigate and manage different systems on Hadoop
  * Goto http://localhost:8888
* Launch Dashboard and login to Ambari
  * Username: maria_dev
  * Password: maria_dev
* #### Trouble shooting
  * Enable **virtualization** in your BIOS
  * Disable **Hyper-V acceleration** in Windows
    * this option is in the “Turn Windows Features On and Off” control panel
  * Make sure you have atleast 8+ GB of RAM
  
## Overview of Hadoop Ecosystem
* Core Hadoop Ecosytem could be visualized as:
* <p align="center"><img src="https://i.imgur.com/Dqf8wEz.png"></p>
* For external Datastorage, we have
  * MySQL
  * MongoDB
  * Cassandra
* Query Engines which run on top of Hadoop clusters are:
  * Apache Drill
  * Hue
  * Apache Phoenix
  * Presto
  * Apache Zeppelin
  
## HDFS
* The Hadoop Distributed File System
* It is mostly for handling very large files
  * could be data from sensors, or logs from web servers etc
* It breaks them into many blocks
  * one block is 128 MB by default
* These blocks can be stored across several computers
* HDFS Architecture consists of
  * **Name Node**
    * It maintains logs of different blocks where they reside and their state
  * **Data  Node**
    * It stores each block of data
* For reading a file
  * Client Node talks with Name Node and checks for file location
  * Name Node informs the Client Node of the position of blocks of this file on Data Nodes
  * Then Client Node retrieves data from Data Nodes
* <p align="center"><img src="https://i.imgur.com/q5OBRlF.png"></p>
* For writing a file
  * Client Node talks with Name Node so that it could keep track of this new file
  * Name Node then creates an entry of this file and Client Node writes it to Data Nodes
  * Data Nodes sent acknowledgement to Client Node
  * Client Node then updates Name Node to add entry of this new file
* <p align="center"><img src="https://i.imgur.com/VLQwHXc.png"></p>
* There are multiple Data Nodes so the data can be retrieved even if a single Data Node fails
* But what if a Name Node fails, that would be a single point of failure
  * It creates a backup of Metadata
    * Namenode writes to local disk and NFS
  * There can be a secondary Namenode
    * It contains a merged copy of edit logs to restore from
  * Each Name node manages a specific namespace volume
* HDFS has high availability
  * Hot standby Namenode using shared edit log
  * Zookeper tracks active Namenode
  * Uses extreme measures to ensure only one namenode is used at a time
* We can use HDFS through:
  * UI (Ambari)
  * CLI
  * HTTP/HDFS Proxy
  * Java interface
  * NFS Gateway

#### Login using Ambari
* To manipulate files with GUI, we can use **Ambari** through HTTP interface
  * Goto http://localhost:8080
  * Login with maria_dev
  * Click on **HDFS** from different options available
  * Goto grid icon and click on **Files View**
  * It shows you the HDFS thats running on your Hadoop cluster
    * You can now perform operations on the files
      * Upload, rename, make directories, concatenate, download files etc
#### Login using Putty
* To manipulate files using CLI, we need to download Putty Client
  * Download from https://putty.org/
* In the Hostname field, type
  * maria_dev@127.0.0.1
* In the Port field, type
  * 2222 (default for Hortonworks sandbox)
* Select connection type as
  * SSH
* Click on open and type password
  * maria_dev
* You can now write commands to manipulate files on HDFS
* Command syntax is **hadoop fs -[command]**
  * hadoop fs -ls
  * hadoop fs -mkdir abc
  * wget *url*
    * To download files into your Virtual Box
  * hadoop fs -copyFromLocal *source* *Destination*
* To check all the commands, type
  * hadoop fs
  
#### Admin access for Ambari
* Sometimes, we need admin access to Ambari instead of maria_dev
  * For example, starting or stopping some services
  * or installing new services on Hadoop
  * these actions require administrative privileges
* In order to have the Admin access
  * first [login using Putty](#login-using-putty) using maria_dev credentials
  * ```su root```
  * ```ambari-admin-password-reset```
    * now set the new password for the ```admin``` account
* Now goto http://localhost:8080
  * enter username as ```admin```
  * password is what you just entered in the previous command
