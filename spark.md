---
layout: default
---

# Deeplearning4j on Spark

Given that deep learning is computationally intensive, if you are working with large datasets, you should think about how to train deep neural networks in parallel. You can run Deeplearning4j multi-threaded on your local machine using Spark standalone; i.e. you don't need a cluster or the cloud. 

## Install Spark

* First, see whether you have Spark installed by entering `which spark` in the command line. 

* If you don't have it, you can easily install Spark on OSX with `brew`:

        brew install apache-spark

* For other OS, see the instructions on [this page](https://spark.apache.org/downloads.html). We recommend a pre-built download rather than building from source. Below, we'll be working with  Hadoop 2.4 and Spark 1.4.1, but you may have slightly different versions. (Spark dataframes require 1.4.*)

![Alt text](../img/spark_download.png)

* You'll need to set the environmental variable SPARK_HOME. To figure out what the file path should be, search for the spark command you'll need later, `spark-submit`:

        sudo find / -name "spark-submit"

* Take the results (here's what mine look like)

        /Users/cvn/Desktop/spark-1.4.1-bin-hadoop2.4/bin/spark-submit

* Remove `/bin/spark-submit` and feed the rest of the file path into your environment variable SPARK_HOME like so:

        export SPARK_HOME=/users/cvn/Desktop/spark-1.4.1-bin-hadoop2.4

## Build the Examples

Now `git clone` the Deeplearning4j Spark ML examples repo from Github:

       git clone https://github.com/deeplearning4j/dl4j-spark-ml-examples

Compile the project with Maven using whichever Spark and Hadoop versions you need. 

       mvn clean package -Dspark.version=1.4.1 -Dhadoop.version=2.4.0

## Run the Examples

Then make sure you're in the dl4j-spark-ml-examples directory and run

        bin/run-example ml.JavaIrisClassification

The output, amid a river of other log info, should look like this:

![Alt text](../img/dl4j_iris_dataframe.png)

You can run other examples with the `bin/run-example` command:

    bin/run-example
    Usage: ./bin/run-example <example-class> [example-args]
        - set env variable MASTER to use a specific master; e.g. export MASTER=local[*]
        - can use abbreviated example class name relative to org.deeplearning4j
        (e.g. ml.JavaIrisClassification,ml.JavaLfwClassification)

They can be run on your local machine by setting `master` to `local[YourNumberOfCores]` or `local[*]` for all cores. For example: 

    MASTER=local[*] bin/run-example ml.JavaIrisClassification 
    //or
    export MASTER=local[*]
    bin/run-example ml.JavaIrisClassification

You can read more about Spark standalone, which is just Spark without a cluster, [here](http://spark.apache.org/docs/latest/spark-standalone.html). Ultimately, training neural networks with Deeplearning4j is just another Spark job. 