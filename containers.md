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



