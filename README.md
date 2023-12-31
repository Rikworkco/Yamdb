# Проект YamDB
![yamdb workflow](https://github.com/Rikworkco/yamdb_final/actions/workflows/yamdb_workflow.yml/badge.svg)![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)

Проект позволяет собирать и отзывы о различных произведениях (фильмы, книги, музыки)  
  
## Описание проекта  
Собранный проект Yamdb_API в docker-compose и размещен на [Doker Hub](https://hub.docker.com/repository/docker/rikworkco/yamdbfinal).

### Регистрация пользователя  
- Пользователь отправляет запрос с параметрами email и username на **/auth/email/**.  
- YaMDB отправляет письмо с кодом подтверждения (confirmation_code) на адрес email . 
- Пользователь отправляет запрос с параметрами email и confirmation_code на **/auth/token/**, в ответе на запрос ему приходит token (JWT-токен).  

### Отзывы и комментарии к ним 
Через эндпоинт **/api/v1/titles/{title_id}/reviews/**
Зарегистрированные пользователи могут оставить к произведениям отзывы с оценкой в диапазоне от одного до десяти. На одно произведение пользователь может оставить только один отзыв. С помощью пользовательских оценок формируется усреднённая оценка произведения. Читать отзывы могут все посетители. 

Через эндпоинт **/api/v1/titles/{title_id}/reviews/{review_id}/comments/**
Зарегистрированные пользователи на отзывы могут оставлять комментарии. Читать комментарии могут все посетители.
 

> Более подробная документация доступна по эндопинту  /redoc/

## Технологии  
  
- Python
- Django REST Framework
- Postgres
- Docker
- аутентификация реализована с помощью Simple JWT.  

## Запуск проекта
Склонируйте репозиторий.
Далее создайте в директории infra_sp2\infra .env файл с параметрами:

    DB_ENGINE=django.db.backends.postgresql  # указываем, что работаем с postgresql 
    DB_NAME=postgres  # имя базы данных 
    POSTGRES_USER=postgres  # логин для подключения к базе данных 
    POSTGRES_PASSWORD=postgres  # пароль для подключения к БД (установите свой)
    DB_HOST=db  # название сервиса (контейнера) 
    DB_PORT=5432  # порт для подключения к БД
    
    ALLOWED_HOSTS=localhost, 127.0.0.1, web, 127.0.0.1:8000, localhost:8000, web:8000 #Хосты
    SECRET_KEY=KEY # ваш ключ


Запуск проекта можно осуществить двумя способами.

### Через виртуальное окружение:

- Клонируйте репозиторий и перейти в него

- Установите и активируйте виртуальное окружение

- Установите зависимости из файла requirements.txt :
```
pip install -r requirements.txt
```
◾ В папке с файлом manage.py выполните миграции:
```
python manage.py migrate
```
◾ Запустите проект:
```
python manage.py runserver
```
### Через Doker
Установите и настройте [Doker](https://www.docker.com/products/docker-desktop/).
Собрать контейнеры можно автоматическом режиме для этого достаточно клонировать только папку infra и заменив в файле docker-compose.yaml:

    build:
      context: ../
      dockerfile: api_yamdb/Dockerfile
на

    image: rikworkco/yamdb_last

Или можно сделать колон всего проекта и собрать самостоятельно.

◾ Запустите docker-compose:
```
docker-compose up -d --build
```
Cоберите файлы статики, и запустите миграции командами:
```
docker-compose exec web python manage.py makemigrations reviews users
docker-compose exec web python manage.py migrate
docker-compose exec web python manage.py collectstatic --no-input
```

◾ Создать суперпользователя можно командой:
```
docker-compose exec web python manage.py createsuperuser
```
◾ Остановить:
```
docker-compose down -v
```

Также вместе с проектом лежат предварительно созданные фикстуры с тестовыми данными
Загрузить фикстуры можно командой:
```
docker-compose exec web python manage.py loaddata fixtures.json
```
Автор: Росманов Илья
