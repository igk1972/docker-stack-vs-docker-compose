Step-by-step comparison of two deployment methods in docker

Conclusion: the methods are almost comparable, but the stack is more convenient and functional, regardless of the dependence on the swarm.



# prepair

- curl -L https://github.com/docker/compose/releases/download/1.24.1/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
- chmod a+x /usr/local/bin/docker-compose
- mkdir -p /mnt/hdd/mariadb




# A. run with docker stack

## run once only

- docker swarm init 
- docker network create --attachable --driver overlay common_backend 

## run deploy/update

- docker stack deploy --compose-file ./compose.yaml iot-broker

## test

- docker run -it --rm --net common_backend mariadb:10.4 mysql -htasks.iot-broker_mysql -uroot -p -e 'select Host, User from mysql.user;'
 
## output

+-----------+------+
| Host      | User |
+-----------+------+
| %         | root |
| localhost | root |
+-----------+------+




# B. run with docker-compose 

## run once only

- docker network create --attachable --driver bridge common_backend 

## run deploy/update

- rm -fr //mnt/hdd/mariadb/etc && cp -fr ./mysql  /mnt/hdd/mariadb/etc
- docker-compose -f ./compose.yaml -p iot-broker up -d
 
## test

- docker run -it --rm --net common_backend mariadb:10.4 mysql -hdatabase -uroot -p -e 'select Host, User from mysql.user;'

## output

+-----------+------+
| Host      | User |
+-----------+------+
| %         | root |
| localhost | root |
+-----------+------+

