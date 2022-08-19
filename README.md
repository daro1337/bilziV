# Simple Web App using GHA & docker

# Docker Compose

Simple web app (simple index.html) using Nginx with HAProxy as Load balancer for at least two backend serves

## Generate self-signed certs for HAproxy:


```bash
openssl req -x509 -newkey rsa:4096 -sha256 \
                  -days 3650 -nodes \
                  -keyout server.key \
                  -out server.crt \
                  -subj "/CN=localhost" \
                  -addext "subjectAltName=DNS:localhost,IP:127.0.0.1"
```

Save them as ssl.pem

```bash
cat server.crt server.key  > haproxy/ssl/ssl.pem
```

## Start env:
```bash
docker-compose up -d
```

## Check env:

Example:
```bash
docker ps
CONTAINER ID   IMAGE                            COMMAND                  CREATED          STATUS          PORTS                                   NAMES
7e84c16fb380   dariusza/bilziv-haproxy:latest   "docker-entrypoint.s…"   15 minutes ago   Up 15 minutes   0.0.0.0:443->443/tcp, :::443->443/tcp   bilziv_haproxy_1
73974e42953a   dariusza/bilziv-www:latest       "/docker-entrypoint.…"   19 minutes ago   Up 19 minutes   80/tcp                                  bilziv_web1_1
c068c870d617   dariusza/bilziv-www:latest       "/docker-entrypoint.…"   19 minutes ago   Up 19 minutes   80/tcp                                  bilziv_web2_1
```

## TEST

```bash
curl -vk https://localhost:443/
```

Output:

```json
{ "msg": " Hello World!" }
```
