# docker

---

## view running containers

```
docker ps
```

```
docker ps -a
```

## view images

```
docker images
```

## run container

```
docker run -h level1 --name level1 -dit -p 2222:22 baseq
```

* `--name level1` - run a docker container with name "level1"
* `-d` - detatch container
* `-i` - keep STDIN open
* `-t` - allocate pseudo-tty
* `-p 2222:22` map port 2222 on host machine to 22 on docker container
* `baseq` the image that we want to create the container from

### level2

```bash
docker run -h level2 --name level2 --net ping-net --ip 172.20.0.2 -it -p 2222:22 level2
```

## execute commands to a container

```
docker exec -dit level1 service ssh start
```

## docker commit

```
docker commit <container-name> <image-name>:latest
```

## tag

```
docker tag base-image:latest level4:latest
```

```
docker run -it level4
```

