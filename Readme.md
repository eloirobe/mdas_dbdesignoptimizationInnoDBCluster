# MDAS Diseño y Optimización Base de Datos MySQL InnoDB Cluster :tada:

Bienvenidos a la imagen de MySQL InnoDB Cluster para a la asignatura de Bases de datos no estructuradas del master [Máster en Desarrollo y Arquitectura de Software (MDAS)](https://www.salleurl.edu/es/estudios/master-en-desarrollo-y-arquitectura-software)

Para levantar un cluster de InnoDB Cluster utilizaremos el docker-compose.yml de este repositorio y los comandos que funcionan con la MySQL Shell:

:wave: *Requisitos previo tener instalado docker*

1) Clonar el reposiorio
```bash
git clone
```
2) Levantar los contenedores (recomendad sin la opción -d para ver el log)
```bash
docker compose up
```
3) Entramos en primer contenedor 
```bash
docker compose exec db1 bash
```
4) Una vez dentro del contenedor db1 entramos en el MySQL Shell
```bash
mysqlsh
```
5) Configuramos la instancia db1 para que permita crear un cluster. (contraseña example)
```bash
dba.configureInstance('root@db1')
```
6) Nos conectamos al MySQL del db1
```
mysqlsh 
\c root@db1
```
7) Creamos el cluster con un único nodo creado
```bash
dba.createCluster('testCluster')
```
8) Una vez creado el cluster con un nodo añadimos el segundo nodo
```bash
dba.configureInstance('root@db2')
cluster.addInstance('root@db2')
```
9) Una vez creado el cluster con un nodo añadimos el tercer nodo
```bash
dba.configureInstance('root@db3')
cluster.addInstance('root@db3')
```

10) Finalmente con este comando podremos ver cual es el estado del cluster
```bash
cluster.status()
#Desde una consola de mysql también se puede ver esta información con
SELECT * FROM performance_schema.replication_group_members;
```

*) En el caso de salir de la consola y volver a entrar tenemos que asignar a una variable el cluster para poder utilizar los comandos del cluster.
```
var cluster = dba.getCluster('testCluster')
```


## MySQL Router


```bash
docker run -e MYSQL_HOST=db1 -e MYSQL_PORT=3306 -e MYSQL_USER=root -e MYSQL_PASSWORD=example -e MYSQL_INNODB_CLUSTER_MEMBERS=3 --network=mdas_dbdesignoptimizationinnodbcluster_mdasnet -p 6446:6446 -p 6447:6447 -p 6448:6448 -p 6449:6449 -p 8443:8443  -ti mysql/mysql-router 
```

