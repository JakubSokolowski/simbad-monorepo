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

Jest sukces ze sparkiem, udało się rozbudować dockera o api http i uruchomić proces symulacji przez te api
 z poziomu przeglądarki. W Dockerfilu jest mały błąd, bo jest stara nazwa klasy `StreamLoader` zamiast `StreamReader`.
 Kilka ostrzeżeń mi się pojawiło:
- Przy uruchamianiu dostaje ostrzeżenie od Hadoopa: `Unable to load native-hadoop library for your platform`
- WARN CSVDataSource: CSV header does not conform to the schema. Expected: next_event_time but found: next_event.time
- Domyślna konfiguracja która jest generowana przez edytor pada na etapie Analyzera z błędem:
```dockerfile
simbad-analyzer-server | Exception in thread "main" java.lang.IllegalArgumentException: More than Int.MaxValue elements.
simbad-analyzer-server |        at scala.collection.immutable.NumericRange$.check$1(NumericRange.scala:309)
simbad-analyzer-server |        at scala.collection.immutable.NumericRange$.count(NumericRange.scala:319)
simbad-analyzer-server |        at scala.collection.immutable.NumericRange.numRangeElements$lzycompute(NumericRange.scala:52)
simbad-analyzer-server |        at scala.collection.immutable.NumericRange.numRangeElements(NumericRange.scala:51)
simbad-analyzer-server |        at scala.collection.immutable.NumericRange.length(NumericRange.scala:54)
simbad-analyzer-server |        at scala.collection.immutable.NumericRange.foreach(NumericRange.scala:72)
simbad-analyzer-server |        at scala.collection.generic.Growable$class.$plus$plus$eq(Growable.scala:59)
simbad-analyzer-server |        at scala.collection.immutable.VectorBuilder.$plus$plus$eq(Vector.scala:732)
simbad-analyzer-server |        at scala.collection.immutable.VectorBuilder.$plus$plus$eq(Vector.scala:708)
simbad-analyzer-server |        at scala.collection.SeqLike$class.$colon$plus(SeqLike.scala:557)
simbad-analyzer-server |        at scala.collection.AbstractSeq.$colon$plus(Seq.scala:41)
simbad-analyzer-server |        at analyzer.Analyzer$.readOrComputeTimePoints(Analyzer.scala:34)
simbad-analyzer-server |        at analyzer.Analyzer$.main(Analyzer.scala:132)
simbad-analyzer-server |        at analyzer.Analyzer.main(Analyzer.scala)
simbad-analyzer-server |        at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
simbad-analyzer-server |        at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
simbad-analyzer-server |        at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
simbad-analyzer-server |        at java.lang.reflect.Method.invoke(Method.java:498)
simbad-analyzer-server |        at org.apache.spark.deploy.JavaMainApplication.start(SparkApplication.scala:52)
simbad-analyzer-server |        at org.apache.spark.deploy.SparkSubmit.org$apache$spark$deploy$SparkSubmit$$runMain(SparkSubmit.scala:849)
simbad-analyzer-server |        at org.apache.spark.deploy.SparkSubmit.doRunMain$1(SparkSubmit.scala:167)
simbad-analyzer-server |        at org.apache.spark.deploy.SparkSubmit.submit(SparkSubmit.scala:195)
simbad-analyzer-server |        at org.apache.spark.deploy.SparkSubmit.doSubmit(SparkSubmit.scala:86)
simbad-analyzer-server |        at org.apache.spark.deploy.SparkSubmit$$anon$2.doSubmit(SparkSubmit.scala:924)
simbad-analyzer-server |        at org.apache.spark.deploy.SparkSubmit$.main(SparkSubmit.scala:933)
simbad-analyzer-server |        at org.apache.spark.deploy.SparkSubmit.main(SparkSubmit.scala)
```
Z tego etapu została właściwie tylko praca fizyczna czyli zbieranie informacji o dostępie i dodanie tych informacji do interfejsu.
Przykładową konfiguracja z repo działa, otrzymuje jakieś wyniki więc mogę zająć się teraz etapem tworzenia wykresów:
- Jakie pliki i inne argumenty należy podać do jakich skryptów żeby te wykresy utworzyć?