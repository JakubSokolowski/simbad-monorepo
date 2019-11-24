### Build docker image with name
```console
docker build -t pipeline-base . -f docker/pipeline-base.Dockerfile 
```

### Publish docker image
```console
docker login
docker tag pipeline-base:latest jsokolowski/pipeline-base:latest
docker push jsokolowski/pipeline-base:latest 
```
### Spark
```console
spark-submit --master local --class analyzer.StreamLoader 
/jar/analyzer.jar /data/stream.csv.gz /data/stream.parquet --files /jar/log4j.properties --conf "spark.executor.extraJavaOptions='-Dlog4j.configuration=log4j.properties'"

```