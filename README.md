# Докеризация проекта autoxml

# Требования
- [Docker](https://docs.docker.com/engine/install/) - Install Docker Engine
- [Docker Compose](https://docs.docker.com/compose/install/) - Install Docker Compose

# Используются следующие сервисы

  - php:7.2-fpm
  - mysql:5.7
  - nginx:alpine
  - redis:latest
  - node:10.21.0

# Руководство по запуску:
  - Клонируем репозиторий в корневой каталог проекта
  - Переименовываем файл **env.example** в **.env**
  - Задаем пути для **APP_HOST** и **APP_CONTAINER**
  - Переимновываем файл **nginx/sites/example.conf** в нужное имя, задаем внутри нужные настройки
  - Дополнительно можно задать настройки для MySQL в файле **mysql/conf/local.cnf**
  - Дополнительно можно изменить название БД в MySQL в файле **init.slq** в папке **mysql/dumps**, что создать БД
  - Дополнительно можно задать настройки для PHP в файле **php/local.ini**
  - Переходим в папку c файлом ***docker-compose.yml***
  - В локальном файле **/etc/hosts** добавляем `127.0.0.1 example.test`, **example.test** заменить на нужное
  - Запускаем докер командой `$ docker-compose up -d`
  - После готовности, импортируем БД
  - Запускем в браузере **http://example.test**
  
# Команды bash для удобного запуска
Добавляем в конец файла ***~/.bashrc***. 
Название команд можно поменять на удобное

**path_to_project** - пусть к папке с докером в проекте, например /home/user/www/example.test/docker

- Запуск контейнеров
```sh
function d_start() {
 ( cd path_to_project && docker-compose up -d )
}
```
- Остановка контейнеров
```sh
function d_stop() {
   ( cd path_to_project && docker-compose stop )
}
```

- Запуск команд ARTISAN
```sh
function art() {
 ( cd path_to_project && docker-compose exec php-fpm php artisan "$@" )
}
```
- Запуск команд NPM
```sh
function npm() {
 (cd path_to_project && docker-compose run node npm "$@")
}
```
