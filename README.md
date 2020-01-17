# ЛегкоОценка Жильё (Backend)

Develop
![Build report](https://gitlab.com/f-case/lo.back/badges/develop/build.svg)
![Coverage report](https://gitlab.com/f-case/lo.back/badges/develop/coverage.svg)

## Зависимости:
* Docker
* Docker-Compose

## Установка
* Скопировать **.env.example** в **.env**
* Установить переменные окружения в **.env**
    * **PUID** должен совпадать с результатом команды `id -u` 
    * **GUID** должен совпадать с результатом команды `id -g`
* Скопировать **code/.env.example** в **code/.env**
* Скопировать **docker-compose.example.yml** в **docker-compose.yml**
* Установить переменные окружения в **code/.env**
* Запустить docker `docker-compose up -d`
* В контейнере **workspace** от имени пользователя **commander** выполнить следующее
	* Установить зависимости `composer install`
    * Сгенерировать ключ шифрования: `php artisan key:generate`
    * Выполнить миграции базы данных: `php artisan migrate`
* В контейнере **node** выполнить следующее
	* Установить зависимости `npm install`

Чтобы запустить контейнер <CONTAINER> от имени пользователя <USERNAME> необходимо выполнить 
`docker-compose exec --user <USERNAME> <CONTAINER> bash`

## Backup
`sudo docker-compose exec -T postgres pg_dump --dbname=postgresql://admin:password@localhost:5432/lo >  /tmp/db.dump`

`rsync -avz -e "ssh -p 20027" kmax@loliving.f-case.ru:/tmp/db.dump /tmp/db.dump`

`docker container exec -i loback_postgres_1 psql -U admin -d lo < /tmp/db.dump`

`rsync -avz -e "ssh -p 20027" kmax@loliving.f-case.ru:/mnt/vol2/loback/project/code/storage/app/ /var/projects/lo.back/code/storage/app/`

