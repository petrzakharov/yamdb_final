# Проект yamdb_final

------

![status workflow](https://github.com/petrzakharov/yamdb_final/actions/workflows/yamdb_workflow.yml/badge.svg)

## Описание

В данном проекте реализован API функционал для проекта YAMDB и настроен ci/cd с помощью Github Actions.
>После того, как сделан пуш в основную ветку:

* Разворачивается окружение
* Прогоняются тесты
* Cобирается docker образ и пушится в DockerHub
* На удаленном сервере запускаются команды из файла docker-compose.yaml которые поднимают 3 контейнера (db, web, nginx). Эти команды описаны в файле: yamdb_workflow.yml.

## Перед запуском на удаленном сервере

1. Установить Docker и Docker-compose

    `sudo apt install docker.io`
2. Добавить env переменные в Github Actions

    ```python
    DB_ENGINE
    DB_HOST
    DB_NAME
    DB_PORT
    DOCKER_PASSWORD
    DOCKER_USERNAME
    HOST
    PASSPHRASE
    POSTGRES_PASSWORD
    POSTGRES_USER
    SSH_KEY
    TELEGRAM_TO
    TELEGRAM_TOKEN
    USER
    ```

3. Скопировать файлы docker-compose.yaml и nginx/default.conf из репозитория на сервер в home/<ваш_username>/docker-compose.yaml и home/<ваш_username>/nginx/default.conf

## После деплоя на сервер

1. Создать и применить миграции

   `sudo docker-compose exec web python manage.py makemigrations --noinput`

   `sudo docker-compose exec web python manage.py migrate --noinput`

2. Создать супер пользователя

    `sudo docker-compose exec web python manage.py createsuperuser`

3. Собрать статику

    `sudo docker-compose exec web python manage.py collectstatic --no-input`

> Готово, можно перейти по <http://84.201.154.175/admin/>
