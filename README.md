# simplilearn-dockercoins

```
git clone https://github.com/academiaonline/simplilearn-dockercoins 
cd simplilearn-dockercoins/
git checkout 2021-09

alias docker='sudo docker'
```
```
docker image build --file Dockerfile-hasher --tag local/simplilearn-dockercoins:test-hasher /mnt/
docker image build --file Dockerfile-rng --tag local/simplilearn-dockercoins:test-rng /mnt/
docker image build --file Dockerfile-webui --tag local/simplilearn-dockercoins:test-webui /mnt/
docker image build --file Dockerfile-worker --tag local/simplilearn-dockercoins:test-worker /mnt/ 

docker network create hasher
docker network create redis
docker network create rng

docker volume create redis

docker container run --detach --name redis --network redis --volume redis:/data library/redis:6.2.5-alpine3.14@sha256:649d5317016d601ac7d6a7b7ef56b6d96196fb7df433d10143189084d52ee6f7
docker container run --detach --name hasher --network hasher local/simplilearn-dockercoins:test-hasher
docker container run --detach --name rng --network rng local/simplilearn-dockercoins:test-rng

docker container run --detach --name worker --network redis local/simplilearn-dockercoins:test-worker

docker network connect hasher worker
docker network connect rng worker

docker container run --detach --name webui --network redis --publish 80:8080 local/simplilearn-dockercoins:test-webui
```
```
docker container top hasher
docker container top redis
docker container top rng
docker container top webui
docker container top worker
```
