# Docker

[Docker Documentation](https://docs.docker.com/)

## Resource

<https://philipzheng.gitbooks.io/docker_practice/content/dockerfile/instructions.html>

[How to Build and Run Your Own Container Images](https://rancher.com/learning-paths/how-to-build-and-run-your-own-container-images/)

## Commands

[Cheet sheet](https://github.com/wsargent/docker-cheat-sheet)  
[Docker CLIs](https://docs.docker.com/engine/reference/commandline/cli/)

```bash
docker run -it ubuntu:18.04 /bin/bash
```

|cmd|description|
|--|--|
| docker ps| List containers注意如果沒container沒在執行中不會顯示，請下參數’--all’或’-a’|
| docker start/docker stop| Start one or more stopped containers / Stop one or more running containers|
| docker logs| Fetch the logs of a container|
| docker logs -f —tail| 這邊是紀錄stdout的logs|
| docker exec| Run a command in a running container|
| docker exec -it ubuntu_bash bash| This will create a new Bash session in the container `ubuntu_bash`. Next, set an environment variable in the current bash session.`exit` **to quit**|
| docker exec -it [container] /bin/bash | 執行container中的bash，如同進入container中的shell|
| docker inspect| Return low-level information on Docker objects可以檢查進入點、config等等資訊running中的container才可以看|
| docker info| Display system-wide information|
| service docker restart| restart docker service|
| docker create|creates a container but does not start it.|
| docker start|starts a container so it is running.|
| docker run|creates and starts a container in one operation.|
| docker run -d|Run container in background and print container ID|
| docker rm|remove container|
| docker rmi|remove image|
| docker network ls|List networks|

## Dockerfile

### RUN vs CMD

#### In a nutshell

- RUN executes command(s) in a new layer and creates a new image. E.g., it is often used for installing software packages.
- CMD sets default command and/or parameters, which can be overwritten from command line when docker container runs.
- ENTRYPOINT configures a container that will run as an executable.

### 為什麼先COPY package.json再COPY source code

為了cache，Dockerfile每一行都是一個cache

### CMD為什麼在build之前

CMD只是指示container啟動時要執行的指令，他在哪裡都可以

## Container’s config

```bash
/var/lib/docker/containers/[container-id]/config.json
```

這是container的config位置，只要知道container id就可以找到他進行修改，注意這不是官方建議作法，不是所有參數都能修改，Evn好像還可以，修改之後重啟container是沒效的，必須重啟docker: `service docker restart`

## How to debug

```bash
docker info
```

看你的container裝在哪邊，裡面會有這一行 `Docker Root Dir:`，可以到這路徑底下找container，裡面應有盡有
   