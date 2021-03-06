# Working through the tutorial from https://docs.docker.com/get-started

## part-1 Orientation commands:
### List Docker CLI commands
docker
docker container --help

### Display Docker version and info
docker --version
docker version
docker info

### Execute Docker image
docker run hello-world

### List Docker images
docker image ls

### List Docker containers (running, all, all in quiet mode)
docker container ls
docker container ls --all
docker container ls -aq

## part-2 containers commands:
### Create image using this directory's Dockerfile
docker build -t friendlyhello .
### Run "friendlyhello" mapping port 4000 to 80
docker run -p 4000:80 friendlyhello
### Same thing, but in detached mode
docker run -d -p 4000:80 friendlyhello
### List all running containers
docker container ls
### List all containers, even those not running
docker container ls -a
### Gracefully stop the specified container
docker container stop <hash>
### Force shutdown of the specified container
docker container kill <hash>
### Remove specified container from this machine
docker container rm <hash>
### Remove all containers
docker container rm $(docker container ls -a -q)
### List all images on this machine
docker image ls -a
### Remove specified image from this machine
docker image rm <image id>
### Remove all images from this machine
docker image rm $(docker image ls -a -q)
### Log in this CLI session using your Docker credentials
docker login
### Tag <image> for upload to registry
docker tag <image> username/repository:tag
### Upload tagged image to registry
docker push username/repository:tag
### Run image from a registry
docker run username/repository:tag

## part-3 services commands:
### List stacks or apps
docker stack ls
### Run the specified Compose file
docker stack deploy -c <composefile> <appname>
### List running services associated with an app
docker service ls
### List tasks associated with an app
docker service ps <service>
### Inspect task or container
docker inspect <task or container>
### List container IDs
docker container ls -q
### Tear down an application
docker stack rm <appname>
### Take down a single node swarm from the manager
docker swarm leave --force

## part-4 swarms commands:
### Create a VM (Mac, Win7, Linux)
docker-machine create --driver virtualbox myvm1
### Win10
docker-machine create -d hyperv --hyperv-virtual-switch "myswitch" myvm1
### initialize myvm1 as a swarm manager
docker-machine ssh myvm1 "docker swarm init --advertise-addr <myvm1 ip>"
### join myvm2 to the swarm
docker-machine ssh myvm2 "docker swarm join --token <work-token> <myvm2 ip>"
### View basic information about your node
docker-machine env myvm1
### List the nodes in your swarm
docker-machine ssh myvm1 "docker node ls"
### Inspect a node
docker-machine ssh myvm1 "docker node inspect <node ID>"
### View join token
docker-machine ssh myvm1 "docker swarm join-token -q worker"
### Open an SSH session with the VM; type "exit" to end
docker-machine ssh myvm1
### View nodes in swarm (while logged on to manager)
docker node ls
### Make the worker leave the swarm
docker-machine ssh myvm2 "docker swarm leave"
### Make master leave, kill swarm
docker-machine ssh myvm1 "docker swarm leave -f"
### list VMs, asterisk shows which VM this shell is talking to
docker-machine ls
### Start a VM that is currently not running
docker-machine start myvm1
### show environment variables and command for myvm1
docker-machine env myvm1
### Mac command to connect shell to myvm1
eval $(docker-machine env myvm1)
### Windows command to connect shell to myvm1
& "C:\Program Files\Docker\Docker\Resources\bin\docker-machine.exe" env myvm1 | Invoke-Expression
### Deploy an app; command shell must be set to talk to manager (myvm1), uses local Compose file
docker stack deploy -c <file> <app>
### Copy file to node's home dir (only required if you use ssh to connect to manager and deploy the app)
docker-machine scp docker-compose.yml myvm1:~ 
### Deploy an app using ssh (you must have first copied the Compose file to myvm1)
docker-machine ssh myvm1 "docker stack deploy -c <file> <app>"
### Disconnect shell from VMs, use native docker
eval $(docker-machine env -u)
### Stop all running VMs
docker-machine stop $(docker-machine ls -q)
### Delete all VMs and their disk images
docker-machine rm $(docker-machine ls -q)