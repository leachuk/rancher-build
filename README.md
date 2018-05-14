# rancher-build
Setting up an environment build process with Rancher

## docker images
Build and run custom nginx container.

```
cd docker/nginx
docker build -t mynginx .
docker run -d -p 8081:80 mynginx
```
Then browse to `http://localhost:8081` to see the custom html page.

## local docker registry for local dev use by rancher
### create local docker registry
Create a local VM to act as the docker registry.
This will be based off rancher os.
```
docker-machine create -d virtualbox --virtualbox-boot2docker-url iso/rancheros-1.4.0.iso --virtualbox-cpu-count 1 --virtualbox-memory 2048 --virtualbox-disk-size 20000 docker-registry
```

Install docker registry image onto the `docker-registry` vm box.
```
docker-machine ssh docker-registry
docker run -d \
  -p 5000:5000 \
  --restart=always \
  --name registry \
  registry:2 
```
### tag custom nginx docker image and place into registry
```
docker tag mynginx 192.168.99.101:5000/mynginx
docker push 192.168.99.101:5000/mynginx
```
### pull custom nginx from registry and run
```
docker pull 192.168.99.101:5000/mynginx
docker run -d -p 8081:80 192.168.99.101:5000/mynginx
```
