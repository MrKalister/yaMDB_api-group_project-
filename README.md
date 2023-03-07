## Проект API для Yamdb
Изначальный проект https://github.com/suslyak/api_yamdb
### Описание:

Проект представляет собой бэк-енд для мини-социальной сети, позволяющую размещать отзывы к произведениям, добавляемым на сайт админом, разбитыми на категори и жанры, а так же коментировать эти отзывы. Проект имеет API в REST-парадигме для работы с базой данных проекта через интерфейсы, программные модули, консоли.
Аутентификация на проекте осуществляется с помощью JWT-токенов.

### Технологический стек:
* python 3.7
* Django
* Django Rest Framework
* Redoc

### Установка пректа:
Установить python:
Если в операционной системе не установлен python версии не ниже 3.7, то перед работой с проектом его потребуется установить:
##### Для Windows
Пройдите по ссылке и выберите подходящий для вашей операционной системы вариант.

https://www.python.org/downloads/release/python-379/

##### Для Linux
Сначала обновить пакеты:

```
sudo apt update && sudo apt upgrade
```
###### Вариант №1
Затем установить зависимости:
```
sudo apt-get install wget build-essential checkinstall
sudo apt-get install libreadline-gplv2-dev libncursesw5-dev libssl-dev libsqlite3-dev tk-dev libgdbm-dev libc6-dev libbz2-dev libffi-dev zlib1g-dev
```
Скачать python:
```
cd /usr/src
sudo wget https://www.python.org/ftp/python/3.7.9/Python-3.7.9.tgz
```
Разархивировать и установать пакет:
```
sudo tar xzf Python-3.7.9.tgz
cd Python-3.7.9
sudo ./configure --enable-optimizations
sudo make altinstall
```
###### Вариант №2 (Для Ubuntu и подобных)
Установка из репозитория deadsnakes ppa
```
sudo apt install software-properties-common
sudo add-apt-repository ppa:deadsnakes/ppa
sudo apt install python3.7
```
##### Установка проекта из репозитория
Клонировать репозиторий и перейти в него в командной строке:

```
git clone https://github.com/suslyak/api_yamdb.git
```

```
cd api_yamdb
```

Cоздать и активировать виртуальное окружение:

В зависимости от переменных окружения в операционной системе, вместо python3 в коммандах может быть python или что-то другое.

```
python3 -m venv env
```
##### для Linux

```
source env/bin/activate
```

##### для Windows

```
source env/Scripts/activate.bat
```
Далее:
```
python3 -m pip install --upgrade pip
```

Установить зависимости из файла requirements.txt:

```
pip install -r requirements.txt
```

Из директории проекта api_yamdb
```
cd .\api_yamdb\
```
Выполнить миграции:

```
python3 manage.py migrate
```

Запустить проект:

```
python3 manage.py runserver
```

### Особенности пректа:

Авторизация на проекте осуществляется при помощи JWT-токенов.
#### Регистрация
1. Пользователь отправляет POST-запрос с параметрами email и username на эндпоинт /api/v1/auth/signup/.
2. Бэкенд отправляет письмо с кодом подтверждения на указанный адрес email.
3. Пользователь отправляет POST-запрос с параметрами username и confirmation_code с этим кодом на эндпоинт /api/v1/auth/token/, в ответе на запрос ему приходит token (JWT-токен). В ответе приходит только access token (без refresh token'a) c временем жизни в сутки, по истечении которого, пользователю необходимо снова отправить запрс на эндпоинт /api/v1/auth/token/ и получить новый токен.

Так же пользователя может создать и админ, в этом случае, поле confirmation_code у пользователя будет пустое, при отправке POST-запроса с параметрами email и username этого пользователя на эндпоинт /api/v1/auth/signup/, бэкенд создаст код потверждения (confirmation_code) и также отправит письмо с кодом подтверждения (confirmation_code) на указанный адрес email.

### Примеры запросов:

Получение списка всех произведений:
GET http://127.0.0.1:8000/api/v1/titles/

Получение произведения c id 1 в базе данных:
GET http://127.0.0.1:8000/api/v1/titles/1/

Получение списка всех отзывов о произведении c id 1 в базе данных:
GET http://127.0.0.1:8000/api/v1/titles/1/reviews/

Создание отзыва о произведении c id 1 в базе данных:
POST http://127.0.0.1:8000/api/v1/titles/1/reviews/
с телом запроса 
```
{
"text": "string",
"score": 1
}
```

Частичное обновление отзыва c id 1 о произведении c id 1 в базе данных:
PATCH http://127.0.0.1:8000/api/v1/titles/1/reviews/1/
с телом запроса 
```
{
"score": 2
}
```

Удаление отзыва c id 1 о произведении c id 1 в базе данных:
DELETE http://127.0.0.1:8000/api/v1/titles/1/reviews/1/

Получение списка всех комментариев к отзыву c id 1 о произведении c id 1 в базе данных:
GET http://127.0.0.1:8000/api/v1/titles/1/reviews/comments/

Создание комментария к отзыву c id 1 о произведении c id 1 в базе данных:
POST http://127.0.0.1:8000/api/v1/titles/1/reviews/1/comments/
с телом запроса 
```
{
"text": "string"
}
```

Частичное обновление комментария c id 1 к отзыву c id 1 о произведении c id 1 в базе данных:
PATCH http://127.0.0.1:8000/api/v1/titles/1/reviews/1/comments/1/
с телом запроса 
```
{
"text": "string"
}
```

Удаление комментария c id 1 к отзыву c id 1 о произведении c id 1 в базе данных:
DELETE http://127.0.0.1:8000/api/v1/titles/1/reviews/1/comments/1/

#### Остальные примеры запросов и подробное описание доступны в документации по пути
http://127.0.0.1:8000/redoc/

#### Контакты разработчиков
email: 
* sergey.suslyak@mail.ru
* bosacheva98@mail.ru
* maxon.nowik@yandex.ru
