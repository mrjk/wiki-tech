# Docker


## Work with Docker

```
docker info --format '{{json .Swarm }}' | jq .
docker node ls  --format '{{json .}}'  | jq .
```
