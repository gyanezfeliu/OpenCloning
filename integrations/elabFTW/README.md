

```
docker-compose up -d
docker exec elabftw bin/init db:install
docker exec -it elabftw bin/console db:update
```