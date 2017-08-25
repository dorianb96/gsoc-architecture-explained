# Application architecture

**note: because of a very low bandwidth limit provided by free version of gitlfs (git large file storage) I had to switch the repository of JAR files to a Dropbox files in which you can find the latest JAR files:   
[EEG_Analysis.jar](https://www.dropbox.com/s/p7tq3hs7j870p8g/EEG_Analysis.jar?dl=0)  
[remote-server-2.0.jar](https://www.dropbox.com/s/ojvkbnfqf1lynmh/remote-server-2.0.jar?dl=0)  
[spark_eeg-1.2-jar-with-dependencies.jar](https://www.dropbox.com/s/tu2o2coxa6yfm4q/spark_eeg-1.2-jar-with-dependencies.jar?dl=0)



## Architecture

The application consists of 3 parts:  

1)   **Client GUI**  

The client GUI is used to browse/manage data as well as build data analysis workflows using feature extraction and classification methods.   

Every data analysis job is then submitted to the Remote Server and executed there using the Data Analysis Package.
   
 
2)  **Remote Server**  
  
The Remote Server is the main communication point for the Client GUI with the remote Hadoop server.  

It listens for requests such as job submittals, checking of job status, fetching the results of a job, listing trained classifiers and etc.

It interacts with the Data Analysis Package to run the requested jobs.

3)  **Data Analysis Package**

The Data Analysis Package is a Java application made using Apache Spark, Apache Hadoop and Deep Learning for Java (dl4j) frameworks.

It's purpose is to provide a modular way of specifying data pipelines consisting of input sources, data processing, feature extraction and classification methods.

## Detailed architecture  

![1](https://github.com/dorianb96/gsoc-architecture-explained/blob/master/App_architecture.jpg?raw=true)


## Deployment

The application has 3 JAR files which need to be properly distributed.

1. Client GUI packaged in EEG_Analysis.jar runs on anywhere on any Operating system as a packaged JAR file  
2. Remote Server (packaged in remote-server-2.0.jar) and Data Analysis Package (packaged in spark_eeg-1.2-jar-with-dependencies.jar) need to be deployed on the remote server in the same folder   

The application is ran on a docker container using Cloudera Quickstart.  
The proper start-up command is: 

      docker run --hostname=quickstart.cloudera --privileged=true -t -i -m 14G -p 80:80 -p 8888:8888 -p 7180:7180 -p 8020:8020  -p 88:88 -p 88:88/udp -p 8990:8990 -p 50070:50070 [IMAGE ID] /usr/bin/docker-quickstart 
      
The docker container can then further be configured as in this tutorial: https://github.com/dorianb96/Cloudera_Quickstart_Install/blob/master/README.md


