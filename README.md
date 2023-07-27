# Проект Kittygram
___

### Проект доступен по адресу: https://bittykittygram.sytes.net/
## Описание
Kittygram — социальная сеть для обмена фотографиями любимых питомцев. Для доступа к сайту необходима регистрация. После авторизации можно добавить нового кота или изменить существующего, а также посмотреть записи других пользователей. При добавлении кота пользователь может указать имя, год рождения, выбрать цвет кота, добавить фото, выбрать достижения из списка, либо создать новое. 
Проект запускается в контейнерах Docker. Настроено автоматическое тестирование и развертывание на виртуальном удаленном сервере с ОС Ubuntu с помощью Github Actions. Сайт защищен шифрованием запросов по протоколу HTTPS.
___

## Технологии, использованные при разработке
- язык *Python*
- фреймворк *Django*
- контейнеризация - *Docker*
- автоматизация - *Github Actions*
- WSGI-сервер *Gunicorn*
- веб-сервер *Nginx*
- база данных *PostgreSQL*
___

## Установка
1. Клонировать репозиторий и перейти в него в командной строке:
```
git clone https://github.com/tetrapack55/kittygram_final
```
```
cd kittygram_final
```
2. В корне проекта создать файл .env, указываем следующие переменные со своими данными
```
POSTGRES_USER      #имя пользователя в базе PostgreSQL
POSTGRES_PASSWORD  #пароль пользователя в базе PostgreSQL
POSTGRES_DB        #имя базы данных
DB_HOST            #имя контейнера, где запущен сервер БД
DB_PORT            #порт, по которому Django будет обращаться к базе данных

SECRET_KEY         #ваш секретный код из settings.py
DEBUG              #статус режима отладки
ALLOWED_HOSTS      #адреса, по которым будет доступен проект

```
3. Создать Docker образы  (вместо username ваш логин на  DockerHub)
```
cd frontend
docker build -t username/kittygram_frontend .
cd ../backend
docker build -t username/kittygram_backend .
cd ../nginx
docker build -t username/kittygram_gateway . 
```
4. Загрузить образы на DockerHub
```
docker push username/kittygram_frontend
docker push username/kittygram_backend
docker push username/kittygram_gateway
```
5. Подключиться к своему удаленному серверу
```
ssh -i путь_до_файла_с_SSH_ключом/название_файла_с_SSH_ключом имя_пользователя@ip_адрес_сервера 
```
6. Установить docker compose на сервер
```
sudo apt update
sudo apt install curl
curl -fSL https://get.docker.com -o get-docker.sh
sudo sh ./get-docker.sh
sudo apt-get install docker-compose-plugin
```
7. Создать на сервере папку kittygram и скопировать в нее файлы docker-compose.yml и .env
8. Открыть на сервере конфиг файл Nginx
```
sudo nano /etc/nginx/sites-enabled/default
```
9. Изменить настройки location
```
location / {
    proxy_set_header Host $http_host;
    proxy_pass http://127.0.0.1:9000;
}
```
10. Перезапустить Nginx
```
sudo service nginx reload
```
11. Для адаптации workflow файла добавьте свои данные в секреты GitHub Actions
```
DOCKER_USERNAME                # имя пользователя в DockerHub
DOCKER_PASSWORD                # пароль пользователя в DockerHub
HOST                           # ip_address сервера
USER                           # имя пользователя
SSH_KEY                        # приватный ssh-ключ (cat ~/.ssh/id_rsa)
SSH_PASSPHRASE                 # кодовая фраза (пароль) для ssh-ключа
TELEGRAM_TO                    # id телеграм-аккаунта (можно узнать у @userinfobot, команда /start)
TELEGRAM_TOKEN                 # токен бота (получить токен можно у @BotFather, /token, имя бота)
```
12. Сохранить все изменения и запушить в GitHub. Если не будет ошибок проект развернется автоматически на сервере
___

## Примеры запросов к API

Получение и удаление токена

```
POST /api/token/login/
POST /api/token/logout/
```

Регистрация нового пользователя: 

```
POST /api/users/
```

Получение данных своей учетной записи:

```
GET /api/users/me/
```


Получение списка всех котов:

```
GET /api/cats/
```

Добавление нового кота:

```
POST /api/cats/
```

Добавление нового достижения:

```
POST /api/achievements/
```

## Автор проекта
Олег Кирьянов
```
https://github.com/tetrapack55
xagatgx@yandex.ru
```