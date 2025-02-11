docker help
docker build
docker run
docker ps -a // lists all containers
docker run -d -p 80:80 docker/getting-started:latest
docker volume create ghost-vol
docker volume inspect ghost-vol
docker volume ls
docker stop CONTAINER_ID
docker kill CONTAINER_ID
docker exec CONTAINER_ID ls
docker exec CONTAINER_ID netstat -ltnp
docker exec -it CONTAINER_ID /bin/sh // shell session
docker stats
docker run -d -e NODE_ENV=development -e url=http://localhost:3001 -p 3001:2368 -v ghost-vol:/var/lib/ghost/content ghost
docker restart CONTAINER_ID
ping google.com
docker run -d --network none docker/getting-started
docker pull caddy
docker network create caddytest
docker network ls
(docker run --network caddytest --name caddy1 -v $PWD/index1.html:/usr/share/caddy/index.html caddy)
docker build . -t helloworld:latest
docker run helloworld
docker push USERNAME/goserver
docker pull USERNAME/goserver
docker image rm USERNAME/goserver

```bash
docker build -t username/imagename:0.0.0 -t username/imagename:latest .
docker push username/imagename --all-tags
```

## Building for linux (bonus)
GOOS=linux GOARCH=amd64 go build