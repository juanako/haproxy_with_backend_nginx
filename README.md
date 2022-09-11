# Description
This docker-compose file allow to build and run haproxy with two backend servers. By default HA Proxy is mapped locally on 8080 port and traffic is proxied to one of two nginx servers.

# Configuration
Configuration is stored locally in conf directories. To run the same images across multiple environments and decrese number of tags in the registry all configuration is mounted per enviornment.

# Docker registry
Each push to master is triggering github action which build and push images to [docker hub public registry](https://hub.docker.com/u/lukaszdurak). All the images are stored into separate repository even there is no changes into dockerfile, that was done to cache image in specific version that we requires.

# Docker compose useful commands
Build:
```
docker-compose build
```
Run:
```
docker-compose up
```
Stop:
```
docker-compose stop
```
Remove containers
```
docker-compose rm
```
Restart
```
docker-compose restart
```
Logs
```
docker-compose logs <container>
```
Build/Up/Stop/Rm etc. specific container
```
docker-compose <command> <container_name>
```
# HA Proxy stats and ssl
## Stats
Backend statisics are avaiable under 8404 port, you can find default login and password in the configuration file.
## SSL
It is possible to run and test ssl connection on HA Proxy. To do that follow these steps:
1. Put your SSL pem file under haproxy/ssl path with name cert.pem
2. Uncomment the lines 
    ```
    frontend https
    bind *:443 ssl crt /etc/cert.pem 

    default_backend webui
    ```
    in haproxy.cfg file
3. Uncomment volume bind with certificate in docker-compose.yml file
    ```
    - type: bind
      source: ./haproxy/ssl/cert.pem
      target: /etc/cert.pem
    ```
