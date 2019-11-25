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