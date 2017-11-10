# Traefik-Docker-Sample

This repo is based on this [HAProxy-Docker-Sample](https://github.com/Franklin89/HAProxy-Docker-Sample) but uses Traefik.

After following the instructions you should be able to call in your browser:

```text
site1.org     -> Routed to site1    container
site1.org/sub -> Routed to site1sub container
site2.org     -> Routed to site2    container
```

## Proxy Network

A overlay network has to be created first using the following command:

```bash
docker network create --driver overlay --scope swarm proxy
```

## Deploy Proxy

```bash
docker stack deploy -c proxy\docker-compose.treafik.yml proxy
```

## Deploy visualizer

```bash
docker stack deploy -c viz\docker-compose.viz.yml viz
```

## Deploy Site 1

```bash
docker stack deploy -c site1\docker-compose.web.yml site1
```

## Deploy Site 2

```bash
docker stack deploy -c site2\docker-compose.web.yml site2
```

## Deploy Site 1 Sub

```bash
docker stack deploy -c site1sub\docker-compose.web.yml site1sub
```

## (local dev only) Update Hosts file

Add the host `site1.org` and `site2.org` to your hosts file.

On windows 10 the hosts files is under: `C:\Windows\System32\drivers\etc`

On a mac the hosts file is under: `/private/etc`

Add the following lines.

```text
127.0.0.1       site1.org
127.0.0.1       site2.org
127.0.0.1       viz.org
```

## Stats page

Traefik offers a nice stats page. Open your browser and navigate to `localhost:8080`.

## Scaling

It is now possible to scale your webservice to as many containers as needed. Traefik will take care of load balancing all of them.

```bash
docker service scale site1_site1=2
docker service scale site2_site2=3
docker service scale site1sub_site1sub=5
```