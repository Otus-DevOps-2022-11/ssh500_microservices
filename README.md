# ssh500_microservices
ssh500 microservices repository HW (docker-2)

Сделал ветку docker-2 от ветки main в ssh500_microservices

Установил локально Docker и docker-compose. Запустил контейнер hello-world, посмотрел список всех контейнеров, images. Посмотрел разницу между docker run и docker start, попробовал docker attach, docker exec, docker -it, docker -dt. Сделал модифицированный локальный image из работающего контейнера: docker commit. Послал сигналы контейнеру docker kill, docker stop. Вывел статистику занятого дискового пространства docker system df.Удалил контейнер и образ с помощью docker rm -f, docker rmi. 

Создал VM YC в папке Default:
yc compute instance create --name docker-host --zone=ru-central1-a --network-interface subnet-name=first,nat-ip-version=ipv4 --create-boot-disk image-folder-id=standard-images,image-family=ubuntu-2004-lts,size=15 --ssh-key ~/.ssh/appuser.pub 

Cписок VM и ip для подключения можно вывести с помощью команды:  yc compute instance list

Установил Docker и docker-compose в VM YC. Скопировал файлы mongod.conf, db_config, start.sh. Написал Dockerfile, собрал image (docker build -t reddit:latest .), запустил контейнер из полученного image. Протестировал работоспособность приложения.

Зарегистрировался на https://hub.docker.com, изменил тег собранного локального image (docker tag reddit:latest ssh500/otus-reddit:1.0), запушил его в hub.docker.com  (docker push ssh500/otus-reddit:1.0)

Запустил у себя на локальном ПК образ скачанный из: https://hub.docker.com/repository/docker/ssh500/otus-reddit
(docker run --name reddit -d -p 9292:9292 ssh500/otus-reddit:1.0)  

Посмотрел логи запущенного контейнера (docker logs reddit -f), запустил bash в текущем контейнере "docker exec -it reddit bash" посмотрел запущенные процессы ps aux, остановил контейнер killall5 1. Посмотрел информацию об образе (docker inspect ssh500/otus-reddit:1.0). Запустил контейнер (docker run --name reddit -d -p 9292:9292 ssh500/otus-reddit:1.0), зашел в него (docker exec -it reddit bash) создал папку и файл, вышел из контейнера. Вывел изменения контейнера (docker diff reddit). Остановил и удалил контейнер docker stop reddit && docker rm reddit. Убедился что во вновь запущенном контейнере нет тестовой папки и файла.

Удалил VM в YC: yc compute instance delete docker-host && docker rm reddit


