docker pull postgres
docker build  -t joherma1/sia .

docker run -p 5432:5432 -e POSTGRES_PASSWORD=agricultura.1 -e POSTGRES_USER=sia --name postgres-sia -d postgres

docker run  -p 8080:8080 -e POSTGRES_PASSWORD=agricultura.1 -e POSTGRES_USER=sia --name sia --link postgres-sia:postgres -d joherma1/sia

//TODO
org.apache.commons.dbcp.SQLNestedException: Cannot create PoolableConnectionFactory (FATAL: role "root" does not exist)