## Лабораторная работа по работе с docker
### Матвеев Андрей. ИУ8-23

## Домашнее задание

В репозитории приведен код web-приложения, которое сохраняет в БД введенную информацию о задаче - ее имя.

## Часть I. Docker

1. Добавьте в код Dockerfile, который позволит запустить web-приложение с исходным кодом в каталоге app/ через docker.
   [Dockerfile](https://github.com/dvorfe30-io/lab07docker/blob/main/app/Dockerfile)
2. Выполните запуск контейнера с этим приложением.
   ```
   $ sudo docker build -t lab-docker-app .
   ```
   <details><summary>Вывод</summary>
   '''
[+] Building 42.0s (10/10) FINISHED                                        docker:default
 => [internal] load build definition from Dockerfile                                 0.0s
 => => transferring dockerfile: 186B                                                 0.0s
 => [internal] load metadata for docker.io/library/python:3.9-slim                   1.6s
 => [internal] load .dockerignore                                                    0.0s
 => => transferring context: 2B                                                      0.0s
 => [1/5] FROM docker.io/library/python:3.9-slim@sha256:2d97f6910b16bd338d3060f261f  0.0s
 => => resolve docker.io/library/python:3.9-slim@sha256:2d97f6910b16bd338d3060f261f  0.0s
 => [internal] load build context                                                    0.0s
 => => transferring context: 345B                                                    0.0s
 => CACHED [2/5] WORKDIR /app                                                        0.0s
 => [3/5] COPY requirements.txt .                                                    0.0s
 => [4/5] RUN pip install --no-cache-dir -r requirements.txt                        34.0s
 => [5/5] COPY . .                                                                   0.1s 
 => exporting to image                                                               6.1s 
 => => exporting layers                                                              5.2s 
 => => exporting manifest sha256:7050fd61d0f3f977000b700b86c6a30508f656eddf82a2642d  0.0s 
 => => exporting config sha256:52c1d1962a1d6d8aff33255a703bd42022dcb73a319322e3be0e  0.0s 
 => => exporting attestation manifest sha256:4e96c9f25982f526621a68868853042c47773b  0.0s 
 => => exporting manifest list sha256:3c19cb535af367f672e84624eb0d72c2e161bd8de5737  0.0s
 => => naming to docker.io/library/lab-docker-app:latest                             0.0s
 => => unpacking to docker.io/library/lab-docker-app:latest 
 '''
   </details>
```
sudo docker run -d -p 5000:5000 --name app-container lab-docker-app
244c6c00325a275e7c7cb555f03f0f0b7d4442f04046c1d7c03f5c1c4fa81877

sudo docker logs app-container
 * Serving Flask app 'app'
 * Debug mode: off
WARNING: This is a development server. Do not use it in a production deployment. Use a production WSGI server instead.
 * Running on all addresses (0.0.0.0)
 * Running on http://127.0.0.1:5000
 * Running on http://172.17.0.2:5000
```
   
3. Скопируйте из консоли в каталог /home/ контейнера файл README.md.
```
$ sudo docker cp ../README.md app-container:/home/README.md
Successfully copied 4.44kB (transferred 6.14kB) to app-container:/home/README.md
```
   
4. Подключитесь к терминалу контейнера с приложением в интерактивном режиме. Проверьте, что скопированный файл находится в нужном каталоге.
```
$ sudo docker exec -it app-container bash
root@244c6c00325a:/app# ls -la /home/
-rw-rw-r-- 1 1000 1000 4442 May 27 12:37 README.md
```
5. Выйдите из интерактивного режима.
```
root@244c6c00325a:/app# exit
```
6. Остановите контейнер с приложением.
```
sudo docker stop app-container
```


## Часть II. Docker compose
1. Создайте файл docker-compose.yml таким образом, чтобы совместно с описанным в части 1 контейнером работала бы база данных mysql. Файл инициализации БД в каталоге db/init.sql. Также пропишите порт подключения к приложению. Например 5000.
[docker-compose.yml](https://github.com/dvorfe30-io/lab07docker/blob/main/docker-compose.yml)
2. Запустите связку web-приложение - БД.
```
$ sudo docker compose up --build 
```
<details><summary>Вывод</summary>
[+] Building 22.2s (12/12) FINISHED                                                       
 => [internal] load local bake definitions                                           0.0s
 => => reading from stdin 504B                                                       0.0s
 => [internal] load build definition from Dockerfile                                 0.0s
 => => transferring dockerfile: 186B                                                 0.0s
 => [internal] load metadata for docker.io/library/python:3.9-slim                  21.8s
 => [internal] load .dockerignore                                                    0.0s
 => => transferring context: 2B                                                      0.0s
 => [1/5] FROM docker.io/library/python:3.9-slim@sha256:2d97f6910b16bd338d3060f261f  0.0s
 => => resolve docker.io/library/python:3.9-slim@sha256:2d97f6910b16bd338d3060f261f  0.0s
 => [internal] load build context                                                    0.0s
 => => transferring context: 191B                                                    0.0s
 => CACHED [2/5] WORKDIR /app                                                        0.0s
 => CACHED [3/5] COPY requirements.txt .                                             0.0s
 => CACHED [4/5] RUN pip install --no-cache-dir -r requirements.txt                  0.0s
 => CACHED [5/5] COPY . .                                                            0.0s
 => exporting to image                                                               0.1s
 => => exporting layers                                                              0.0s
 => => exporting manifest sha256:eb8b4f09cabbaaa7f8d9451b030dd9b088b2d9aa9d6b31fa72  0.0s
 => => exporting config sha256:a5a7b7c8cc09d835b37e0aabedf7b72d9bb9e7ea689e17025aed  0.0s
 => => exporting attestation manifest sha256:b83cd5b63babb6ad21fd267db7fea60e6fbc8f  0.0s
 => => exporting manifest list sha256:2388447c02151d2b158a2f088fab45bfb4b05f405cd6e  0.0s
 => => naming to docker.io/library/lab7docker-app:latest                             0.0s
 => => unpacking to docker.io/library/lab7docker-app:latest                          0.0s
 => resolving provenance for metadata file                                           0.0s
[+] up 5/5
 ✔ Image lab7docker-app       Built                                                  22.2s
 ✔ Network lab7docker_default Created                                                 0.1s
 ✔ Volume lab7docker_db_data  Created                                                 0.0s
 ✔ Container mysql_db         Created                                                 0.1s
 ✔ Container lab_dockerP2     Created                                                 0.1s
Attaching to lab_dockerP2, mysql_db
Container mysql_db Waiting 
mysql_db  | 2026-05-27 16:24:19+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 8.0.46-1.el9 started.
mysql_db  | 2026-05-27 16:24:20+00:00 [Note] [Entrypoint]: Switching to dedicated user 'mysql'
mysql_db  | 2026-05-27 16:24:20+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 8.0.46-1.el9 started.
mysql_db  | 2026-05-27 16:24:20+00:00 [Note] [Entrypoint]: Initializing database files
mysql_db  | 2026-05-27T16:24:20.436086Z 0 [Warning] [MY-011068] [Server] The syntax '--skip-host-cache' is deprecated and will be removed in a future release. Please use SET GLOBAL host_cache_size=0 instead.
mysql_db  | 2026-05-27T16:24:20.436232Z 0 [System] [MY-013169] [Server] /usr/sbin/mysqld (mysqld 8.0.46) initializing of server in progress as process 80
mysql_db  | 2026-05-27T16:24:20.447938Z 1 [System] [MY-013576] [InnoDB] InnoDB initialization has started.
mysql_db  | 2026-05-27T16:24:20.858060Z 1 [System] [MY-013577] [InnoDB] InnoDB initialization has ended.
mysql_db  | 2026-05-27T16:24:22.095947Z 6 [Warning] [MY-010453] [Server] root@localhost is created with an empty password ! Please consider switching off the --initialize-insecure option.
mysql_db  | 2026-05-27 16:24:25+00:00 [Note] [Entrypoint]: Database files initialized
mysql_db  | 2026-05-27 16:24:25+00:00 [Note] [Entrypoint]: Starting temporary server
mysql_db  | 2026-05-27T16:24:25.601666Z 0 [Warning] [MY-011068] [Server] The syntax '--skip-host-cache' is deprecated and will be removed in a future release. Please use SET GLOBAL host_cache_size=0 instead.
mysql_db  | 2026-05-27T16:24:25.603878Z 0 [System] [MY-010116] [Server] /usr/sbin/mysqld (mysqld 8.0.46) starting as process 124
mysql_db  | 2026-05-27T16:24:25.650502Z 1 [System] [MY-013576] [InnoDB] InnoDB initialization has started.
mysql_db  | 2026-05-27T16:24:25.822908Z 1 [System] [MY-013577] [InnoDB] InnoDB initialization has ended.
mysql_db  | 2026-05-27T16:24:26.050152Z 0 [Warning] [MY-010068] [Server] CA certificate ca.pem is self signed.
mysql_db  | 2026-05-27T16:24:26.050188Z 0 [System] [MY-013602] [Server] Channel mysql_main configured to support TLS. Encrypted connections are now supported for this channel.
mysql_db  | 2026-05-27T16:24:26.053134Z 0 [Warning] [MY-011810] [Server] Insecure configuration for --pid-file: Location '/var/run/mysqld' in the path is accessible to all OS users. Consider choosing a different directory.
mysql_db  | 2026-05-27T16:24:26.077504Z 0 [System] [MY-011323] [Server] X Plugin ready for connections. Socket: /var/run/mysqld/mysqlx.sock
mysql_db  | 2026-05-27T16:24:26.077600Z 0 [System] [MY-010931] [Server] /usr/sbin/mysqld: ready for connections. Version: '8.0.46'  socket: '/var/run/mysqld/mysqld.sock'  port: 0  MySQL Community Server - GPL.
mysql_db  | 2026-05-27 16:24:26+00:00 [Note] [Entrypoint]: Temporary server started.
mysql_db  | '/var/lib/mysql/mysql.sock' -> '/var/run/mysqld/mysqld.sock'
mysql_db  | Warning: Unable to load '/usr/share/zoneinfo/iso3166.tab' as time zone. Skipping it.
mysql_db  | Warning: Unable to load '/usr/share/zoneinfo/leap-seconds.list' as time zone. Skipping it.
mysql_db  | Warning: Unable to load '/usr/share/zoneinfo/leapseconds' as time zone. Skipping it.
mysql_db  | Warning: Unable to load '/usr/share/zoneinfo/tzdata.zi' as time zone. Skipping it.
mysql_db  | Warning: Unable to load '/usr/share/zoneinfo/zone.tab' as time zone. Skipping it.
mysql_db  | Warning: Unable to load '/usr/share/zoneinfo/zone1970.tab' as time zone. Skipping it.
mysql_db  | 2026-05-27 16:24:28+00:00 [Note] [Entrypoint]: Creating database maadb
mysql_db  | 2026-05-27 16:24:28+00:00 [Note] [Entrypoint]: Creating user maa
mysql_db  | 2026-05-27 16:24:28+00:00 [Note] [Entrypoint]: Giving user maa access to schema maadb
mysql_db  | 
mysql_db  | 2026-05-27 16:24:28+00:00 [Note] [Entrypoint]: /usr/local/bin/docker-entrypoint.sh: running /docker-entrypoint-initdb.d/init.sql
mysql_db  | 
mysql_db  | 
mysql_db  | 2026-05-27 16:24:28+00:00 [Note] [Entrypoint]: Stopping temporary server
mysql_db  | 2026-05-27T16:24:28.674721Z 14 [System] [MY-013172] [Server] Received SHUTDOWN from user root. Shutting down mysqld (Version: 8.0.46).
mysql_db  | 2026-05-27T16:24:29.800815Z 0 [System] [MY-010910] [Server] /usr/sbin/mysqld: Shutdown complete (mysqld 8.0.46)  MySQL Community Server - GPL.
mysql_db  | 2026-05-27 16:24:30+00:00 [Note] [Entrypoint]: Temporary server stopped
mysql_db  | 
mysql_db  | 2026-05-27 16:24:30+00:00 [Note] [Entrypoint]: MySQL init process done. Ready for start up.
mysql_db  | 
mysql_db  | 2026-05-27T16:24:30.923380Z 0 [Warning] [MY-011068] [Server] The syntax '--skip-host-cache' is deprecated and will be removed in a future release. Please use SET GLOBAL host_cache_size=0 instead.
mysql_db  | 2026-05-27T16:24:30.925811Z 0 [System] [MY-010116] [Server] /usr/sbin/mysqld (mysqld 8.0.46) starting as process 1
mysql_db  | 2026-05-27T16:24:30.935600Z 1 [System] [MY-013576] [InnoDB] InnoDB initialization has started.
mysql_db  | 2026-05-27T16:24:31.136484Z 1 [System] [MY-013577] [InnoDB] InnoDB initialization has ended.
mysql_db  | 2026-05-27T16:24:31.366691Z 0 [Warning] [MY-010068] [Server] CA certificate ca.pem is self signed.
mysql_db  | 2026-05-27T16:24:31.366731Z 0 [System] [MY-013602] [Server] Channel mysql_main configured to support TLS. Encrypted connections are now supported for this channel.
mysql_db  | 2026-05-27T16:24:31.369914Z 0 [Warning] [MY-011810] [Server] Insecure configuration for --pid-file: Location '/var/run/mysqld' in the path is accessible to all OS users. Consider choosing a different directory.
mysql_db  | 2026-05-27T16:24:31.394138Z 0 [System] [MY-011323] [Server] X Plugin ready for connections. Bind-address: '::' port: 33060, socket: /var/run/mysqld/mysqlx.sock
mysql_db  | 2026-05-27T16:24:31.394219Z 0 [System] [MY-010931] [Server] /usr/sbin/mysqld: ready for connections. Version: '8.0.46'  socket: '/var/run/mysqld/mysqld.sock'  port: 3306  MySQL Community Server - GPL.
Container mysql_db Healthy 
lab_dockerP2  |  * Serving Flask app 'app'
lab_dockerP2  |  * Debug mode: off
lab_dockerP2  | WARNING: This is a development server. Do not use it in a production deployment. Use a production WSGI server instead.
lab_dockerP2  |  * Running on all addresses (0.0.0.0)
lab_dockerP2  |  * Running on http://127.0.0.1:5000
lab_dockerP2  |  * Running on http://172.18.0.3:5000
lab_dockerP2  | Press CTRL+C to quit

</details>
3. Проверьте подключение к приложению через браузер. Сделайте снимок экрана.
[Снимок экрана](https://github.com/dvorfe30-io/lab07docker/blob/main/Screenshot.png)
4. Проверьте работу приложения через браузер.
```
$ curl http://localhost:5000
<!DOCTYPE html>
<html>
<head>
    <title>MVC App</title>
</head>
<body>
    <h1>Список из Базы Данных</h1>
    <ul>
        
            <li>ÐŸÑ€Ð¸Ð¼ÐµÑ€ 1</li>
        
            <li>ÐŸÑ€Ð¸Ð¼ÐµÑ€ 2</li>
        
    </ul>
</body>
</html>
```


