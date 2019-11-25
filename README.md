# Simbad Monorepo

Repo with all Simbad components added as submodules

[Project Documentation](https://simbad-docs.readthedocs.io)

## Updating submodules to latest
```
git submodule foreach git pull origin master
git add .
git commit -m "Update submodules"
git push
```

## Running simulation
Clone the project:
```
git clone https://github.com/JakubSokolowski/simbad-monorepo.git --recurse-submodules 
cd simbad-monorepo
```
### Running dev version (build Dockerfiles from source): 
In project root folder:
```
docker-compose -f docker/docker-compose.yml build
docker-compose -f docker/docker-compose.yml up
```
Go to http://localhost:8080/#/examples/simulation-pipeline
### Running prod version (build Dockerfiles docker hub): 
In project root folder:
```
docker-compose -f docker/prod-docker-compose.yml build
docker-compose -f docker/prod-docker-compose.yml up
```
Go to http://localhost:8080/#/examples/simulation-pipeline
