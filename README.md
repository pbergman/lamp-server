##Intro

i made this LAMP with different contianers so it is easy controllable/scalable.

The all work independently but work with each other i used the data container.

so the apache and php-fpm talk over sockets through the data continer and that
gives them teh possibility to use different pools for virtual hosts.

if you use this installation/usage than the apache/php config can mostely be
configured in the data container (this is busybox image with bash and nano)
en just restart the php, apache container to apply the changes

this als give the possibility to easely sitch between php 5.3 and 5.4 just
by stopping on contianer and start the othe container.

##Installation:

The data container:
```
    ~$ cd data/
    ~$ sudo docker build -t "data_hub" .
```
Mysql:
```
    ~$ cd ../mysql/
    ~$ sudo docker build -t "mysql" .
```
Apache:
```
    ~$ cd ../apache/
    ~$ sudo docker build -t "apache" .
```

next build fpm container:
```
    ~$ cd ../php54/
    ~$ sudo docker build -t "php:5.4" .
```
and for php 5.3
```
    ~$ cd ../php53/
    ~$ sudo docker build -t "php:5.3" .
```
and for php 5.5
```
    ~$ cd ../php55/
    ~$ sudo docker build -t "php:5.5" .
```

##Usage:
firt we need start the data continer because we need that for the other continers
```
    sudo docker run --name "data_hub" -v /var/www/:/var/www/ -dti data_hub
```
Next we need to start mysql:
```
    sudo docker run --name "mysql" \
                    -dti \
                    -p 3306:3306 \
                    --volumes-from data_hub \
                    mysql
```
to see root password:
```
    sudo docker logs mysql
```
or
```
    sudo docker exec -ti mysql cat /root/.my.cnf | grep password | awk '{ print $NF}'
```
Next we start php
```
    sudo docker run --name "php_54" \
                    -dti \
                    --volumes-from data_hub \
                    --link mysql:mysql \
                    php:5.4
```
And stop him because we want to start 5.3 as well
```
    sudo docker stop php_54
```
Next we start php 5.3
```
    sudo docker run --name "php_53" \
                    -dti \
                    --volumes-from data_hub \
                    --link mysql:mysql \
                    php:5.3
```
And do the same for php 5.5
```
    sudo docker stop php_53
    sudo docker run --name "php_55" \
                    -dti \
                    --volumes-from data_hub \
                    --link mysql:mysql \
                    php:5.5

```
And as last we start apache:
```
    sudo docker run --name "apache" \
                    -dti \
                    -p 80:80 \
                    --volumes-from data_hub \
                    apache
```

so if you want to switch between php instances just simply stop php_53 and start php_54

so for example:
```
    sudo docker stop  php_53 && sudo docker start php_54
```
you php should be able to connect with mysql as long you linked the php container with mysql conatiner,
through the given alias. So with this eample you should be to connect with:

    database_host:     mysql
    database_port:     3306
    database_user:     root

for password see logs or the .my.cnf file in the mysql/data container
