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

## execute commands to a container

```
docker exec -dit level1 service ssh start
```

