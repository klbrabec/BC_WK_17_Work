# BC_WK_17_Work

17.0.2 Module Roadmap 
What constitutes big data and how its handled 
Hadoop
MapReduce
PySpark
Natural Language Processing
Cloud Services / AWS

Define big data and describe the challenges associated with it
Define Hadoop and name the main elements of its ecosystem
Explain how MapReduce processes data 
Define Spark and how it processes data
Describe NLP colleciton and analysis
Explain how to use AWS Simple Storage Service (S3) and relational databases for basic cloud storage
Complete an analysis of Amazon customer reviews 

17.0.3 - Getting Ready for Virtual Class 

17.1.1 - What is Big Data? 
Data is considered 'big data' when it exceeds the capacity of operational databases 

The four V's of Big Data 
Volume - the size of the data - (ex terabytes of records)

Velocity - How quickly data comes in.  

Variety - the different forms of data. 

Veracity - the uncertainty of data. 

These 4v's will help you determine when to migrate from regular data to big data solutions. 

Big data problems - working with datasets of size creates unique challenges. 
    How can it be stored?
    How can it be accessed?
    How does it get backed up? 

17.0.4 - Welcome to Big Data 

17.1.2- Big Data Technologies 
Apache Hadoop - one of the most popular open source frameworks - with numerous technologies for big data. 
Developed by Google - to process large amounts of data by splitting it across a distributed file system. 

Hadoop Distributed File System - a file system used to store data across server clusters (groups of computers) 
    Scalable - handles influxes of data. 
    Fault tolerant - handles hardware failures 
    Distributed - spread across multiple servers connected by a common core. 
Map Reduce - programming model and processing technique for big data. 
    Enables the processing of large amounts of data across the cluster by performing the same task for each system. 
YARN - Yeta nother resource Negotiator - manages and allocates resources across clusters. 

Hadoop distributes for the storage and processing of data through a cluster (a group of connected computers that work together to store and perform tasks on a dataset)

YOu need to set up all three main components across multiple machines as well as make sure each one has sufficient resources and is configured for optimal performance. 

Hadoop official website - https://hadoop.apache.org/

17.2.1 - MapReduce Process 
A common tool for splitting up large data sets 
Built on the process of mapping - assigning the same job to each of the computers in a cluster  and reducing - when you come back together to combine the results. 
Splitting up the data reduces the time needed to analyze it. 
Works on divided datasets in smaller batches to work faster. 
The mapping stage - when the funciton is applied to each computer - takes a small piece of the input and coverts the data into key/value pairs. 

Ex - word count process
input - entire file is fed into the word counter
splitting - each line of text is separated
mapping - each word in every line is assigned a value of 1 
Shuffling - the words are combined and organized alphabetically, creating a list of values
Reducing - the list of values summed for each word
Final result - complete list of words and value counts returned. 

17.2.2 - mrjob Library 
MapReduce JOb 
Helps us practice map reduce outside of the Hadoop ecosystem. 
Very little setup needed 
Can be run from any computer with Python installed 

Code Sample: 
from mrjob.job import MRJob //imports library dependency 
class Bacon_count(MRJob): //establishes a class 
   def mapper(self, _, line): //Creates a function that assigns input to key value pairsand allows methods to be mapped together - the line parameter allows the line of text to be taken from the raw input file 
       for word in line.split(): //calls the split method on each line to break the text into a list of words 
           if word.lower() == "bacon"://converts all words to lowercase 
               yield "bacon", 1 //if words match 'bacon' a key value pair will show as yeild bacon 1 
               //this returns a generator object - like an interator - however it is not stored in memory.  Will not return a value until next() is called.  
               In this example - each time the word bacon appears, mrJobs returns bacon,1 
               //the shuffle step - inherits from the MrJob library - organizes the key value pairs so that there is only one key for each unique key and combines values into a list. 
   def reducer(self, key, values):
    //self parameter - used in Python to represent the instance of the class being used. 
    //Key parameter - represents the key of the key/value pair created in the mapper function 
    //Values - list of values created in the mapper function - produces the key and sum of all of the values assigned to it. 
       yield key, sum(values)
if __name__ == "__main__":
   Bacon_count.run()


CODE LOOKS LIKE THIS: 
from mrjob.job import MRJob
class Bacon_count(MRJob):
   def mapper(self, _, line):
       for word in line.split():
           if word.lower() == "bacon":
               yield "bacon", 1

   def reducer(self, key, values):
       yield key, sum(values)
if __name__ == "__main__":
   Bacon_count.run()

Run with this : 
$ python bacon_counter.py input.txt

Creates a temporary folder,
Outputs the results to that folder
reports back
Removes the folder. 

the mrJob library will not be used in production anywhere (there are better libraries for that purpose)

17.3.1 - Key Features of Spark 
With the growinginterest in big data and ease of access to cloud technology - hadoop is no longer required. 

Spark (apache spark) a unified analytics engine for large scale data processing. 
    Lets you write applications in code that can run on hadoop 
    spark does not have to run in Hadoop 
        Can also run in stand alone mode or the cloud 
        Can be much faster than hadoop 
        Like MapReduce - spark works with data across a cluster 
    Spark uses in memory computation instead of a disk based solution. 
        Does not need to talk to the HDFS each time
        Can retain as much as HDFS can in memory 
        Uses lazy evaluation - delays the evaluation of an expression or command until needed. 

17.3.2 - Spark Architecture 
Driver - the heart of the application - responsible for maintaining application information, responding to code or input, analyzing, distributing and scheduling work 
Executors - perform the code assigned by the driver and report the state of computation to the driver
Cluster manager - controls the driver and executors and allocates resources to the machines 
    External service for acquiring resources on the cluster. 

Driver - manager that assings work to employees that then perform the task and report back when it is finished or not.  Cluster manager is a budge manager who allocates specific resources to employees to perform tasks. 

17.3.3 Spark Parallelism 
Parallelism - running work through a cluster concurrently, instead of performing all work on a single machine. 
Partitions - a chunk of data will sit on a physical machine in a cluster.  (in each machine in the cluster)
If there is only one partition, but many executors, the parallelism is only one. (One machine can only work with one executor)
If there are many partitions but only one exectutor, the parallelism is still only one - (one executoy is assigned to one machine) 
Each executor need to work on each partition for perfect parallellism 

17.3.4 - Spark API 
Very accessible through different languages 
There is an API that translates code into spark code and executes 
    Java/Python/SQL/R 

Data APIs 
    supports two different sets of APIs 
        Low level unstructured APIs - 
            deals with resilient distributed datasets - immutable collection of records that can be operated in parallel 
        High Level APIs - Consist of structured data
            Datasets
            DataFrames
            SQL Tables and views 

17.5.1 - Natural Language Processing 






Spark Vs Hadoop 
    Stores data in memory - Spark 
    Contains a whole ecosystem - Hadoop 
    Used for processing large amounts of data - both 
    Uses lazy evaluation - spark 
    WOrks with data across clusters - both
    Has its own distributed file system - Hadoop 



