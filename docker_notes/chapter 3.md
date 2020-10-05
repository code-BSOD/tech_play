# Personal Docker Commands Review Hub

## Chapter 3

1.  To check Docker version & see if engine can be accessed
```
$ docker version
```
2. To get more information & see configuration values
```
$ docker info
```
3. Pull Nginx webserver from Docker Hub
```
$ docker container run --publish 80:80 nginx
```
* Docker first checked if the image nginx is available.
* Since it's not, it downloaded an image from docker hub and then started a new container
* --publish 80:80, the first 80 is the port of the host, second 80 is the container port. So, when any request comes to host port 80, it sends the request to container with port 80.
* But this CMD will make nginx running on the foreground of the terminal
* Each time we use <code>run</code> CMD, it will create a new container and run it.
4. To run in in the background
```
$ docker container run --publish 80:80 --detach nginx
831c61a335ec8765e5f6c212b0826aff399f9751d8b1ed81a5e34d93411db288
```
* --detach makes the container run in the background
* It also returns the unique container id in the output

5. To list the running containers only
```
$ docker container ls
or
$ docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                NAMES
831c61a335ec        nginx               "/docker-entrypoint.…"   2 minutes ago       Up 2 minutes        0.0.0.0:80->80/tcp   affectionate_turing
```
6. To stop the docker container, we need to provide the container ID. Just a first few characters would be enough to stop the container.
```
$ docker container stop 831
831
```
7. To see all containers running or stopped, we add "-a"
```
$ docker container ls -a
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                     PORTS               NAMES
831c61a335ec        nginx               "/docker-entrypoint.…"   7 minutes ago       Exited (0) 2 minutes ago                       affectionate_turing
6200e8d1e686        nginx               "/docker-entrypoint.…"   14 minutes ago      Exited (0) 7 minutes ago                       wonderful_goldwasser
```
8. To give our container a name by ourself, we can run
```
$ sudo docker container run --publish 80:80 --detach --name webhost nginx
```
Now if we run ls,
```
$ docker container ls -a
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS                      PORTS                NAMES
5e0d43fb18cc        nginx               "/docker-entrypoint.…"   About a minute ago   Up About a minute           0.0.0.0:80->80/tcp   webhost
831c61a335ec        nginx               "/docker-entrypoint.…"   14 minutes ago       Exited (0) 9 minutes ago                         affectionate_turing
6200e8d1e686        nginx               "/docker-entrypoint.…"   21 minutes ago       Exited (0) 14 minutes ago                        wonderful_goldwasser
```

9. Since when detached, we can't see logs, to see the logs,
```
$ sudo docker container logs webhost
```
```
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
103.145.74.86 - - [27/Sep/2020:18:58:09 +0000] "GET / HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/85.0.4183.121 Safari/537.36" "-"
103.145.74.86 - - [27/Sep/2020:19:00:35 +0000] "GET / HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/85.0.4183.121 Safari/537.36" "-"
```

10. To see the processes running inside the docker container, we can run the command. <code>webhost</code> is the container name.
```
$ docker container top webhost
```
```
UID                 PID                 PPID                C                   STIME               TTY                 TIME                CMD
root                1849                1831                0                   18:57               ?                   00:00:00            nginx: master process nginx -g daemon off;
systemd+            1911                1849                0                   18:57               ?                   00:00:00            nginx: worker process
```

11. To remove stopped containers:
    First, checking the containers
```
$ docker container ls -a

CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                      PORTS                NAMES
5e0d43fb18cc        nginx               "/docker-entrypoint.…"   11 minutes ago      Up 11 minutes               0.0.0.0:80->80/tcp   webhost
831c61a335ec        nginx               "/docker-entrypoint.…"   24 minutes ago      Exited (0) 19 minutes ago                        affectionate_turing
6200e8d1e686        nginx               "/docker-entrypoint.…"   31 minutes ago      Exited (0) 24 minutes ago                        wonderful_goldwasser
```
Now to remove,
```
$ docker container rm 831 620
```
This will automatically remove the 2 containers. The 831, 620 are the container id.

12. To forcefully remove a container without stopping it first (otherswise, need to stop the container first before we can remove it)
```
$ docker container rm -f 5e0
5e0
```
This will forcefully remove the running container.

13. To STOP a container, we can pass the name or the unique container ID
```
$ docker container stop webhost
```
OR
```
$ docker container stop 5e0d
```

14. To START a stopped container
```
$ docker container start 5e0d
```
OR
```
$ docker container start webhost
```

15. To get HELP with CMDs,
<code>$ docker --help </code>
```
$ docker container start --help
```

16. To send environment variables, we can use the <code>--env</code> or <code>-e</code> to pass variable.
```
$ docker container run --publish 3306:3306 --name mysql -d --env MYSQL_RANDOM_ROOT_PASSWORD=yes mysql
```

# What's Going inside a container

17. To see metadata, how a container was build and it's configuration, we use <code>inspect</code> command.
```
$ docker container inspect apache
```
And it will return a JSON string with information about the container.
```
[
    {
        "Id": "cce79a8fa26cefdd33cb7fd1cd116d955e05ff85e665cc60c5acb5c99ed59052",
        "Created": "2020-09-28T02:57:40.534654135Z",
        "Path": "httpd-foreground",
        "Args": [],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 3915,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2020-09-28T02:57:41.246952221Z",
            "FinishedAt": "0001-01-01T00:00:00Z"
        },
        "Image": "sha256:417af7dc28bc66aa2cc4af18cbfa934cc55f46721b101beac68b9f5c33d7fcb2",
        "ResolvConfPath": "/var/lib/docker/containers/cce79a8fa26cefdd33cb7fd1cd116d955e05ff85e665cc60c5acb5c99ed59052/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/cce79a8fa26cefdd33cb7fd1cd116d955e05ff85e665cc60c5acb5c99ed59052/hostname",
        "HostsPath": "/var/lib/docker/containers/cce79a8fa26cefdd33cb7fd1cd116d955e05ff85e665cc60c5acb5c99ed59052/hosts",
        "LogPath": "/var/lib/docker/containers/cce79a8fa26cefdd33cb7fd1cd116d955e05ff85e665cc60c5acb5c99ed59052/cce79a8fa26cefdd33cb7fd1cd1
16d955e05ff85e665cc60c5acb5c99ed59052-json.log",
        "Name": "/apache",
        "RestartCount": 0,
        "Driver": "overlay2",
        "Platform": "linux",
        "MountLabel": "",
        "ProcessLabel": "",
        "AppArmorProfile": "docker-default",
        "ExecIDs": null,
        "HostConfig": {
            "Binds": null,
            "ContainerIDFile": "",
            "LogConfig": {
                "Type": "json-file",
                ..........
                (I truncated the output)
```

18. To see a live view of the performance and stats of a container, we can use the following <code>stats</code> command with either container name/id or just leave the container id/name blank and it will list down all the container information. This is live, so it will auto-refresh.

```
$ docker container stats apache

CONTAINER ID        NAME                CPU %               MEM USAGE / LIMIT    MEM %               NET I/O             BLOCK I/O           PIDS
cce79a8fa26c        apache              0.01%               8.02MiB / 3.843GiB   0.20%               4.7kB / 3.44kB      246kB / 0B          109
```
# Getting a Shell inside a container

19. When nginx container ran, it had a default command inside the image which stated it to start an nginx daemon. But we can actually override that command and start a bash shell instead. But for this, we need to pass additional parameter <code>-it</code> and pass the command at the end <code>bash</code>. <code>nginx</code> here is the image and <code>bash</code> is the command it will run after running the container instead of the default which is running the nginx daemon.

```
$ docker container run -it --name webserver nginx bash

root@734be56a25b4:/# whoami
root
root@734be56a25b4:/#
```
We can also use Ubuntu's docker image and it will run bash when container runs.
```
$ docker container run -it --name buntu ubuntu
root@b2856696194f:/# 
```

20. When we start an existing container and want to be in interactive mode, we don't need to use <code>-it</code> but only need to use <code>-ai</code> to specify interactive.

```
$ docker container start --help

Usage:  docker container start [OPTIONS] CONTAINER [CONTAINER...]

Start one or more stopped containers

Options:
  -a, --attach               Attach STDOUT/STDERR and forward signals
      --detach-keys string   Override the key sequence for detaching a container
  -i, --interactive          Attach container's STDIN

$ docker container start -ai buntu
root@b2856696194f:/# 
```

21. To run additional commands on a container that is already running something (e.g MySQL, nginx etc), we can use the `exec` command to run additional commands.
    * Unlike other commands which stops the container when we exit, this one does NOT do that. As it's running an additional command on top of what was already running. So, if we have a MySQL instance running in detached mode, we can enter into bash shell inside that container and when we exit, it won't stop the container.
```
$ docker container exec --help

Usage:  docker container exec [OPTIONS] CONTAINER COMMAND [ARG...]

Run a command in a running container

Options:
  -d, --detach               Detached mode: run command in the background
      --detach-keys string   Override the key sequence for detaching a container
  -e, --env list             Set environment variables
  -i, --interactive          Keep STDIN open even if not attached
      --privileged           Give extended privileges to the command
  -t, --tty                  Allocate a pseudo-TTY
  -u, --user string          Username or UID (format: <name|uid>[:<group|gid>])
  -w, --workdir string       Working directory inside the container
```

So if a container named 'MySQL' is already running a MySQL instance, we can access it's terminal using,
```
$ docker container exec -it mysql bash
```
So, essentially it's running the bash command in the `mysql` container.
Now to see the process inside the mysql container, we need to install `ps` command first inside the container.

```
$ apt-get update && apt-get install -y procps

root@fb805eea0617:/# ps aux
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
mysql          1  1.3  8.9 1730540 360784 ?      Ssl  04:50   0:02 mysqld
root         196  0.0  0.0   3864  3208 pts/0    Ss   04:51   0:00 bash
root         602  0.0  0.0   7636  2656 pts/0    R+   04:53   0:00 ps aux
```
We can see, it's running bash, ps aux along with the mysqld daemon.
After we exit, we can see the container is still running.
```
root@fb805eea0617:/# exit
exit

asif_mahmud1574@docker-test:~$ sudo docker container ls
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                               NAMES
fb805eea0617        mysql               "docker-entrypoint.s…"   4 minutes ago       Up 4 minutes        0.0.0.0:3306->3306/tcp, 33060/tcp   sql
```

22. To see the current images in the Docker engine,
```
$ docker image ls

REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
mongo               latest              ba0c2ff8d362        2 days ago          492MB
ubuntu              latest              9140108b62dc        2 days ago          72.9MB
httpd               latest              417af7dc28bc        12 days ago         138MB
nginx               latest              7e4d58f0e5f3        2 weeks ago         133MB
mysql               latest              e1d7dc9731da        2 weeks ago         544MB
```

23. To PULL an image from Docker hub
```
$ docker pull alpine
```
Here, alpine is the image name. Alpine linux.

# Docker Network Concepts
## Private and Public communications for containers

* All containers can talk to each other without `-p`
* Best practice is to create virtual network for each app
* For example, if we create network `my_web_server` for a MySQL and Nginx container, they can talk to each other without opening up their ports to the rest of the internet.
* Network `my_api` can have MongoDB & NodeJS container running and they can talk to each other. But they can't talk directly to container running on network `my_web_server` which has MySQL and Nginx container.

24. To find the PORT of a container
```
$ docker container port webserver

80/tcp -> 0.0.0.0:80
```

25. To get the IP Address of a container, we can use the same `inspect` command. We can search through the result or we can use `--format` option to get the IP address directly.

```
$ sudo docker container inspect --format '{{ .NetworkSettings.IPAddress }}' webserver
172.17.0.2
```

* Each container is attached to a Network Bridge in Docker and that Network Bridge is actually physically connected to the host's network. So we can communicate via internal IP Address.
* For example, we have an Nginx server running. And the ip address is:
```
$ sudo docker container inspect --format '{{ .NetworkSettings.IPAddress }}' webserver

172.17.0.2

$ docker container start -ia ubuntu

root@2ffbd119ca8f:/# curl 172.17.0.2
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
```
So, we can see, data can be retrieved to Ubuntu container from the webserver container.
* So when we use `--publish 80:80`, the Network Bridge is actually opening up Port 80 on the host, and forwards traffic from host's port 80 to containers port 80.

## Docker Network CLI Management Commands

26. To see the network bridges and network ids,

```
$ docker network ls

NETWORK ID          NAME                DRIVER              SCOPE
3ebdc7cea4e3        bridge              bridge              local
8df96ede698f        host                host                local
aaf0a0e81826        none                null                local
```