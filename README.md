Installation:

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

Usage:
        firt we need start the data continer because we need that for the other continers

            sudo docker run --name "data_hub" -v /var/www/:/var/www/ -dti data_hub

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
            sudo docker exec -ti mysql bash
```
        and in continainer:
```
            cat ~/.my
```
        Next we start php
```
            sudo docker run --name "php_54" -dti --volumes-from data_hub php:5.4
```
        And stop him because we want to start 5.3 aswell
```
            sudo docker stop php_54
```
        Next we start php 5.3
```
             sudo docker run --name "php_53" -dti --volumes-from data_hub php:5.3
```
        And as last we start apache:
```
            sudo docker run --name "apache" \
                            -dti \
                            -p 80:80 \
                            --volumes-from data_hub \
                            --link mysql:mysql \
                            apache
```

        so if you want to switch between php instances just simply stop php_53 and start php_54

        so for example:
```
            sudo docker stop  php_53 && sudo docker start php_54
```