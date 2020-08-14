# Докеризация проекта autoxml

# Требования
- [Docker](https://docs.docker.com/engine/install/) - Install Docker Engine
- [Docker Compose](https://docs.docker.com/compose/install/) - Install Docker Compose

# Используются следующие сервисы

  - php-fpm
  - mysql
  - nginx
  - redis
  - node

# Руководство по запуску:
  - Клонируем репозиторий в корневой каталог проекта
  - Копируем и переименовываем файл **env.example** в **.env**
  - Настраиваем переменные в файле **.env**
  - Копируем и переименовываем файл **nginx/example.conf** в нужное имя, задаем внутри нужные настройки
  - Дополнительно можно задать настройки для MySQL в файле **mysql/local.cnf**
  - Дополнительно можно задать настройки для PHP в файле **php/local.ini**
  - В локальном файле **/etc/hosts** добавляем `127.0.0.1 example.test`, **example.test** заменить на нужное
  - Переходим в папку c файлом ***docker-compose.yml***
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

- Запуск команд `php artisan`
```sh
function art() {
 ( cd path_to_project && docker-compose exec php-fpm php artisan "$@" )
}
```
- Запуск команд `npm`
```sh
function npm() {
 (cd path_to_project && docker-compose run --rm node npm "$@")
}
```

- Запуск команд `composer`
```sh
function comx() {
 (cd path_to_project && docker-compose exec php-fpm composer "$@")
}
```
