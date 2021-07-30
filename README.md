//If you want to use this code, please tell me via email:2261928980@qq.com
//Do not use them for your assignment/homework/practic/exam
//Run the test
//finished by Jiuhong Du a1762017

## compile：

//Go to the project root directory(assignment2)

```
## Compiling in Terminal（Use the following command）：

javac -classpath ./src -d ./build ./src/aggregation/AggregationServer.java ./src/content/ResourceServer.java ./src/get/GetClient.java ./src/test/*
```

## Run the aggregation server（Use the following command）：

```
java -classpath ./build aggregation.AggregationServer port=4567 backup=backup.txt
```

//Open another new Terminal and Go to the project root directory(Assignment2). Do not Shut down the terminal 1(aggregation server).(!importent!)

## Run the resource server（Use the following command）：

```
java -classpath ./build content.ResourceServer port=4567 host=localhost data=data.txt
```

//Please finish running the content server before conducting further tests using the following commands
## Run client（Use the following command）：

```
java -classpath ./build get.GetClient port=4567 host=localhost
```

## test PUT（Use the following command）：

```
java -classpath ./build test.PutTest
```

## test GET（Use the following command）：

```
java -classpath ./build test.GetTest
```

## test OTHER（Use the following command）：

```
java -classpath ./build test.OtherTest
```

## test ERROR（Use the following command）：

```
java -classpath ./build test.ErrorTest
```



# Design ideas

```
	A request to come in, the underlying using HTTP, will enter into the ReceiveThread thread, obtain agreement, enter the corresponding request, invoke the put request, lamport clock from the request header extraction, if lamport is the latest, parse the data, persisted in memory, so that you can avoid frequent access the hard disk, and can be updated persistence, put in the process of using locks, avoid threads that do not have a thread-safe map crash, finally lamport set this put request, let get request again after 12 seconds feedback new data, If a PUT request goes awry, the data is restored before the backup. Get requests through the Java operation returns the XML dom, packaging good tools, and check the clock, whether for 12 seconds, than to the latest data, collapse and backup to the backup file to restart the read the backup files, persisted in memory, realize the crash recovery, and handle the fault tolerance, let the request error status code is 500.
```

## Resource server resource file format

```
title:Fishing Reports
subtitle:The latest reports from fishinhole.com
link:http://www.fishinhole.com/reports
updated:2015-07-03T16:19:54-05:00
author:NameOfYourBoss
id:tag:fishinhole.com,2008:http://www.fishinhole.com/reports
```

## Return XML format

```
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<feed>
    <entry>
        <title>Fishing Reports</title>
        <id>tag:fishinhole.com,2008:http://www.fishinhole.com/reports</id>
        <author>NameOfYourBoss</author>
        <link>http://www.fishinhole.com/reports</link>
        <subtitle>The latest reports from fishinhole.com</subtitle>
        <updated>Wed Sep 23 14:29:02 CST 2020</updated>
    </entry>
</feed>
```

## Backup format

```
title:Fishing Reports
subtitle:The latest reports from fishinhole.com
link:http://www.fishinhole.com/reports
updated:2015-07-03T16:19:54-05:00
author:NameOfYourBoss
id:tag:fishinhole.com,2008:http://www.fishinhole.com/reports
```

