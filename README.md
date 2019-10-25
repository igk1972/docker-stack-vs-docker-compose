# prepair

- mkdir -p /mnt/hdd/mariadb



# run with docker stack

## run once only

- docker swarm init 
- docker network create --attachable --driver overlay common_backend 

## run deploy/update

- docker stack deploy --compose-file ./compose.yaml iot-broker

## test

- docker run -it --rm --net common_backend mariadb mysql -hmysql -uroot -p -e 'select Host, User from mysql.user;'



# run with docker-compose 

## run once only

- docker network create --attachable --driver bridge common_backend 

## run deploy/update

- rm -fr //mnt/hdd/mariadb/etc && cp -fr ./mysql  /mnt/hdd/mariadb/etc
- docker-compose -f ./compose.yaml -p iot-broker up -d
 
## test

- docker run -it --rm --net common_backend mariadb mysql -htasks.iot-broker_mysql -uroot -p -e 'select Host, User from mysql.user;'
