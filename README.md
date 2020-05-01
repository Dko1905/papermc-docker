# Paper Minecraft Server Docker Image

This is a simple docker image for running a Paper Minecraft server.
Thanks to [mtoensing](https://github.com/mtoensing/) for his original Dockerfile, which is [here](https://github.com/mtoensing/Docker-Minecraft-PaperMC-Server).
I have added support for multiple architectures, and made the code cleaner and simpler to use.

## Quick Start

You can either building it yourself or use the image from docker hub. 

### Using Docker Hub

You can pull the image by using `docker pull dako1905/papermc`

Here is a minimal example with persistant storage
```
docker run --rm \
	--name papermc \
	-v /home/joe/paperserver/:/data:rw \
	-p 25565:25565 \
	-e JVM_MEMORY=1G
```
Here is a docker-compose example
```yml
version: '3'
services:
	minecraft_server:
		restart: "no"
		image: dako1905/papermc
		ports:
			- 25565:25565
		volumes:
			- /home/joe/paperserver/:/data:rw
		environment:
			- JVM_MEMORY=1G

```
(I have not tested the `docker-compose.yml` file, but it should work, create an issue if it doesn't)

### Building it youself

You start by cloning the repository, for that you need to have `git` installed.
```
git clone https://github.com/mtoensing/Docker-Minecraft-PaperMC-Server
```

After cloning you can build it using docker, you need to specify the architecture when building
```
docker build --tag dako1905/papermc:<arch>-latest ./<arch>
```

## Configuration

### Volume

You can use volumes to store data persistantly, for example:
```
docker run --rm \
	-p 25565:25565 \
	-v <full path to folder where you want to store the server files>:/data:rw \
	dako1905/papermc
```

### Port Settings

The default mc port is 25565.
You can change the port in the `server.properties` file and then when launching it.
```
docker run --rm \
	-p <port here>:<port here> \
	dako1905/papermc
```

### Environment variables

There are two kinds of enviroment variables, the build ones and the runtime ones.
Some of the variables work on both.

| **Name** | **type** | **Description** | **Extra info** |
| -------- |:--------:|:---------------:|:--------------:|
| PAPERMC_CI_URL | build | The url for paperclip | This should not be changed |
| JVM_MEMORY | build & runtime | It specifies the amount of memory that the jvm max can use | 1G is fine |
| CUSTOM_JVM_ARGS | build & runtime | Custom arguments to the jvm | For advanced users |

## License
It is licensed under the MIT license.
