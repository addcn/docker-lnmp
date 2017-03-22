# Introduction

Deploy lnmp(Linux, Nginx, MySQL, PHP7) using docker. [Read More...](http://www.jianshu.com/p/fcd0e542a6b2)

## How To Use?

#### Build Image

```shell
docker build --tag addcn/mysql -f mysql/Dockerfile .
docker build --tag addcn/php7 -f php7/Dockerfile .
docker build --tag addcn/nginx -f nginx/Dockerfile .
```

#### Run Container

```shell
docker run --name mysql -p 3306:3306 -v /root/bo/data/mysql:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 -it addcn/mysql
docker run --name php7 -p 9000:9000 -v /var/www/html:/usr/local/nginx/html --link mysql:mysql -it addcn/php7
docker run --name nginx -p 80:80 -v /var/www/html:/usr/local/nginx/html --link php7:php7 -it addcn/nginx
```

#### Test PHP & MySQL

> vi /var/www/html/test.php

```php
<?php
//date
echo date("Y-m-d H:i:s")."<br />\n";

//mysql
try {
    $conn = new PDO('mysql:host=mysql;port=3306;dbname=mysql;charset=utf8', 'root', '123456');
} catch (PDOException $e) {
    echo 'Connection failed: ' . $e->getMessage();
}
//$conn->exec('set names utf8');
$sql = "SELECT * FROM `user` WHERE 1";
$result = $conn->query($sql);
while($rows = $result->fetch(PDO::FETCH_ASSOC)) {
    echo $rows['Host'] . ' ' . $rows['User']."<br />\n";
}

//phpinfo
phpinfo();
?>
```

http://192.168.8.36/test.php


![docker-lnmp][1]

  [1]: docs/docker-lnmp.png


## About me

- 微博 [http://www.weibo.com/addcn](http://www.weibo.com/addcn)