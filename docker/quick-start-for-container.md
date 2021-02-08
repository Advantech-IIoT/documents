# docker notes - quick start for ubuntu:bionic
---


## case 1

```bash=
$ docker create --name test1 --network host -v /wor:/work --privileged -i ubuntu:bionic bash 
```

```bash=
$ docker start test1
```

```bash=
$ docker exec -it test1 bash
```

## case 2

```bash=
$ docker create --name test1 --network host -v /wor:/work --privileged -it ubuntu:bionic bash 
```

```bash=
$ docker start test1
```

```bash=
$ docker attach test1
```

**The case 2 if you use exit to leave `container` bash, then the container will stop.**

---
###### tags: `docker` `ubuntu`
