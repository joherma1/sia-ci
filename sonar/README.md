# How to run a Sonar in a container
##### 1. Config the database
  * Internally uses a volume (/var/lib/postgresql/data)
```
docker run --name postgres-sonar -p 5432:5432 \
    -e POSTGRES_PASSWORD=sonar -e POSTGRES_USER=sonar -d postgres
```

  * Open a client creating a new container (remove when finished)
```
docker run -it --link postgres-sonar:postgres --rm postgres \
    sh -c 'exec psql -h "$POSTGRES_PORT_5432_TCP_ADDR" -p "$POSTGRES_PORT_5432_TCP_PORT" -U sonar'

```

##### 2. Start sonar
```
docker run -d --name sonar-sia \
    --link postgres-sonar:postgres \
    -p 9000:9000 -p 9092:9092 \
    -e SONARQUBE_JDBC_USERNAME=sonar \
    -e SONARQUBE_JDBC_PASSWORD=sonar \
    -e SONARQUBE_JDBC_URL=jdbc:postgresql://"$(docker inspect --format '{{ .NetworkSettings.IPAddress }}' postgres-sonar)":5432/sonar \
    sonarqube:5.1
```

##### 3. Analyse code
