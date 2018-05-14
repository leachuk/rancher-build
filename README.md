# rancher-build
Setting up an environment build process with Rancher

## docker images
Build and run custom nginx container.

```
cd docker
docker build -t mynginx .
docker run -d -p 8081:80 mynginx
```
Then browse to `http://localhost:8081` to see the custom html page.
