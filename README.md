# OrbStack demo

## Cleanup

Commands:

```
sudo dscacheutil -flushcache; sudo killall -HUP mDNSResponder
docker system prune --all --volumes
docker volume prune --all
```

## Warmup

Docker desktop:

```
docker context use desktop-linux
docker run -d -p 5432:5432 \
	--name postgres \
	-e POSTGRES_PASSWORD=postgres \
	postgres
```

OrbStack:

```
docker context use orbstack
docker pull mariadb:10.6.4-focal
docker pull wordpress:latest
docker pull nginx:latest
docker pull --platform=linux/amd64 mysql:8.0.27
```

## Demo 01

Commands:

```
cd demo-01-wordpress
docker-compose up -d
```

## Demo 02

Commands:

```
orb migrate docker -f
```

## Demo 03

URLs:
* http://demo-01-wordpress-wordpress-1.orb.local/
* http://wordpress.demo-01-wordpress.orb.local/wp-admin/install.php
* http://nginx.demo-03-nginx.orb.local
* https://orb.local/
* https://nginx.custom.local/

Commands:

```
cd demo-03-wordpress
docker-compose up -d
```

## Demo 04

Commands:

```
cd demo-04-wordpress-mysql
docker-compose up -d
```

## Demo 05

Commands:

```
cd demo-05-build-multi-platforms
git clone https://github.com/docker/getting-started-app.git
cd getting-started-app
```

Create Dockerfile:

```Dockerfile
FROM node:18-alpine
WORKDIR /app
COPY . .
RUN yarn install --production && yarn cache clean
CMD ["node", "src/index.js"]
EXPOSE 3000
```

Commands:

```
# Create a parallel multi-platform builder
docker buildx create --name builder --use
# Make "buildx" the default
docker buildx install
# Build for multiple platforms
docker build --platform linux/amd64,linux/arm64 -t varrocen0/getting-started-app --push .
```

## Demo 06

Commands:

```
cd demo-06-kubernetes
orb start k8s
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.8.1/deploy/static/provider/cloud/deploy.yaml
kubectl create deployment nginx --image nginx
kubectl expose deploy/nginx --port=80
orb stop k8s
orb delete k8s
```

URLs:
* http://k8s.orb.local/

## Demo 07

Commands:

```
orb create ubuntu:jammy
orb
ls /mnt/mac
uname -a
mac uname -a
sudo apt install nginx
sudo systemctl start nginx
```

URL : h

demo-05-build-multi-platforms/getting-started-app
