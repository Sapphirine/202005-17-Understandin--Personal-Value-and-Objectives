# How to use

## Preprocessing
In the preprocessing directory is python code used for preparing the data for 
processing. This code is well documented.

## Processing
There are two Scala applications that use Spark for processing: DataEmulator and
ComplexNetwork. First, you must build the mvn project and copy the JAR to your
Google bucket. For example, from the e6895-mvn directory one might run the following:
```
mvn clean install && gsutil cp target/personality-project_0.1-0.0.4.jar gs://eecs-e6895-bucket/jars
```

To execute these run the following:

```
CLUSTER=<<<PUT YOU CLUSTER HERE>>>
CLASS=<<<ComplexNetwork OR DataEmulator>>>
JAR="gs://eecs-e6895-bucket/jars/personality-project_0.1-0.0.4.jar" # make sure to copy the jar to your bucket
SIZE=15
RANK=4
TOL=0.002
MAXITER=25
OUTPUT="demo"

gcloud dataproc jobs submit spark \
    --cluster=$CLUSTER \
    --class=$CLASS \
    --region=us-west1 \
    --jars="$JAR,gs://spark-lib/bigquery/spark-bigquery-latest.jar" \
    --properties="cloud.profiler.enable=true,cloud.profiler.name=ComplexNetwork,cloud.profiler.service.version=demo" \
    --properties="spark.jars.packages=graphframes:graphframes:0.8.0-spark2.4-s_2.11" \
    -- $SIZE $RANK $TOL $MAXITER $OUTPUT
```

## Postprocessing
For post processing the data, run the MultilayerAnalysis python script, 
identifying the input path to the CSV files that were stored during the 
processing step prior.

## Visualization
1. In the /visualization directory there is a README that directs you to the correct static HTML page
2. For viewing the detection results, run the MultilayerAnalysis Python script in the /postprocessing directory.

## More Information
http://www.ee.columbia.edu/~cylin/course/bigdata/projects/
