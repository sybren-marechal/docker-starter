# Containers

## build your app

We are ready to build the app. Make sure you are still at the top level of your new directory. Now run the build command. This creates a Docker image, which we're going to tag using `-t` so it has a friendly name.

```text
docker build -t thermostat .
```

{% hint style="info" %}
Your image name is always in **smal case**.
{% endhint %}

Where is your built image? It's in your machine's local Docker image registry:

```text
$ docker image ls

REPOSITORY            TAG                 IMAGE ID
thermostat         latest              326387cea398
```

> Troubleshooting for Linux users
>
> _DNS settings_
>
> Proxy servers can block connections to your web app once it's up and running. If you are behind a proxy server, add the following lines to your Dockerfile, using the `ENV` command to specify the host and port for your proxy servers:
>
> ```text
> # Set proxy server, replace host:port with values for your servers
> ENV http_proxy host:port
> ENV https_proxy host:port
> ```
>
> _Proxy server settings_
>
> DNS misconfigurations can generate problems with `pip`. You need to set your own DNS server address to make `pip` work properly. You might want to change the DNS settings of the Docker daemon. You can edit \(or create\) the configuration file at `/etc/docker/daemon.json` with the `dns` key, as following:
>
> ```javascript
> {
>   "dns": ["your_dns_address", "8.8.8.8"]
> }
> ```
>
> In the example above, the first element of the list is the address of your DNS server. The second item is the Google's DNS which can be used when the first one is not available.
>
> Before proceeding, save `daemon.json` and restart the docker service.
>
> `sudo service docker restart`
>
> Once fixed, retry to run the `build` command.

## Run the app

Run the app, 

```text
docker run thermostat
```

Right now, you got your default output from what you have configured in your`Dockerfile.`

{% code-tabs %}
{% code-tabs-item title="Dockerfile" %}
```text
CMD ["ruby", "application.rb","23","C"]
```
{% endcode-tabs-item %}
{% endcode-tabs %}

```text
{
  "heating": "off",
  "airco": "on"
}
heating: false
cooling: true

```

To add a diffrent temperature you can simple do this

```text
docker run thermostat ruby application.rb 9 C
```

you can open you bash even in the thermostat project with

```text
docker run thermostat bash
```

## Share your image

To demonstrate the portability of what we just created, let's upload our built image and run it somewhere else. After all, you need to know how to push to registries when you want to deploy containers to production.

A registry is a collection of repositories, and a repository is a collection of images—sort of like a GitHub repository, except the code is already built. An account on a registry can create many repositories. The `docker` CLI uses Docker's public registry by default.

> **Note**: We use Docker's public registry here just because it's free and pre-configured, but there are many public ones to choose from, and you can even set up your own private registry using [Docker Trusted Registry](https://github.com/sybren-marechal/docker-starter/tree/86015b8f63a88424652425fd3d8071ca18d612a9/datacenter/dtr/2.2/guides/README.md).

### Log in with your Docker ID

If you don't have a Docker account, sign up for one at [cloud.docker.com](https://cloud.docker.com/){: target="_blank" class="_" }. Make note of your username.

Log in to the Docker public registry on your local machine.

```text
$ docker login
```

### Tag the image

The notation for associating a local image with a repository on a registry is `username/repository:tag`. The tag is optional, but recommended, since it is the mechanism that registries use to give Docker images a version. Give the repository and tag meaningful names for the context, such as `get-started:part2`. This puts the image in the `get-started` repository and tag it as `part2`.

Now, put it all together to tag the image. Run `docker tag image` with your username, repository, and tag names so that the image uploads to your desired destination. The syntax of the command is:

```text
docker tag image username/repository:tag
```

For example:

```text
docker tag thermostat sybrenmarechal/thermostat
```

Run [docker image ls](https://github.com/sybren-marechal/docker-starter/tree/86015b8f63a88424652425fd3d8071ca18d612a9/engine/reference/commandline/image_ls/README.md) to see your newly tagged image.

```text
$ docker image ls

REPOSITORY               TAG                 IMAGE ID            CREATED             SIZE
friendlyhello            latest              d9e555c53008        3 minutes ago       195MB
john/get-started         part2               d9e555c53008        3 minutes ago       195MB
python                   2.7-slim            1c7128a655f6        5 days ago          183MB
...
```

### Publish the image

Upload your tagged image to the repository:

```text
docker push username/repository:tag
```

Once complete, the results of this upload are publicly available. If you log in to [Docker Hub](https://hub.docker.com/), you see the new image there, with its pull command.

![](.gitbook/assets/screen-shot-2018-05-17-at-17.33.21.png)

### Pull and run the image from the remote repository

From now on, you can use `docker run` and run your app on any machine with this command:

```text
docker run username/repository:tag
```

example:

```text
docker run sybrenmarechal/thermostat:latest
```

If the image isn't available locally on the machine, Docker pulls it from the repository.

```text
$ docker run sybrenmarechal/thermostat:latest
Unable to find image 'sybrenmarechal/thermostat:latest' locally
latest: Pulling from sybrenmarechal/thermostat
Digest: sha256:7d18e4d997e93e359332b939d599bb06411c6bbb2145d47e8fcd23537ebc34b6
Status: Downloaded newer image for sybrenmarechal/thermostat:latest
{
  "heating": "off",
  "airco": "on"
}
heating: false
cooling: true
```

No matter where `docker run` executes, it pulls your image, along with Python and all the dependencies from `requirements.txt`, and runs your code. It all travels together in a neat little package, and you don't need to install anything on the host machine for Docker to run it.

## Recap and cheat sheet

Here's [a terminal recording of what was covered on this page](https://asciinema.org/a/blkah0l4ds33tbe06y4vkme6g):

Here is a list of the basic Docker commands from this page, and some related ones if you'd like to explore a bit before moving on.

```text
docker build -t friendlyhello .  # Create image using this directory's Dockerfile
docker run -p 4000:80 friendlyhello  # Run "friendlyname" mapping port 4000 to 80
docker run -d -p 4000:80 friendlyhello         # Same thing, but in detached mode
docker container ls                                # List all running containers
docker container ls -a             # List all containers, even those not running
docker container stop <hash>           # Gracefully stop the specified container
docker container kill <hash>         # Force shutdown of the specified container
docker container rm <hash>        # Remove specified container from this machine
docker container rm $(docker container ls -a -q)         # Remove all containers
docker image ls -a                             # List all images on this machine
docker image rm <image id>            # Remove specified image from this machine
docker image rm $(docker image ls -a -q)   # Remove all images from this machine
docker login             # Log in this CLI session using your Docker credentials
docker tag <image> username/repository:tag  # Tag <image> for upload to registry
docker push username/repository:tag            # Upload tagged image to registry
docker run username/repository:tag                   # Run image from a registry
```



