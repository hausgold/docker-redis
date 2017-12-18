![mDNS enabled official/redis](https://raw.githubusercontent.com/hausgold/docker-redis/master/docs/assets/project.png)

This Docker images provides the [official/redis](https://hub.docker.com/_/redis/) image as base
with the mDNS/ZeroConf stack on top. So you can enjoy [redis](https://redis.io/) while
it is accessible by default as *redis.local*. (Port 6379)

## Requirements

* Host enabled Avahi daemon
* Host enabled mDNS NSS lookup

## Getting starting

You just need to run it like that, to get a working redis:

```bash
$ docker run --rm hausgold/redis
```

The port 6379 is untouched.

## docker-compose usage example

```yaml
redis:
  image: hausgold/redis
  environment:
    # Mind the .local suffix
    - MDNS_HOSTNAME=redis.test.local
  ports:
    # The ports are just for you to know when configure your
    # container links, on depended containers
    - "6379"
```

## Host configs

Install the nss-mdns package, enable and start the avahi-daemon.service. Then,
edit the file /etc/nsswitch.conf and change the hosts line like this:

```bash
hosts: ... mdns4_minimal [NOTFOUND=return] resolve [!UNAVAIL=return] dns ...
```

## Configure a different mDNS hostname

The magic environment variable is *MDNS_HOSTNAME*. Just pass it like that to
your docker run command:

```bash
$ docker run --rm -e MDNS_HOSTNAME=something.else.local hausgold/redis
```

This will result in *something.else.local*.

## Other top level domains

By default *.local* is the default mDNS top level domain. This images does not
force you to use it. But if you do not use the default *.local* top level
domain, you need to [configure your host avahi][custom_mdns] to accept it.

## Further reading

* Docker/mDNS demo: https://github.com/Jack12816/docker-mdns
* Archlinux howto: https://wiki.archlinux.org/index.php/avahi
* Ubuntu/Debian howto: https://wiki.ubuntuusers.de/Avahi/

[custom_mdns]: https://wiki.archlinux.org/index.php/avahi#Configuring_mDNS_for_custom_TLD
