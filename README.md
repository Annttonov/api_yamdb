## Проект API_YaMDB

> Проект YaMDb собирает отзывы пользователей на различные произведения.
> Проект был создан исключительно в учебных целях, и никакой практической пользы он не несет.

___
## Описание проекта:

Первая версия API для проекта API_YaMDB.
Документация в формате Redoc доступна по ссылке:

```
http://127.0.0.1:8000/redoc/
```

Позволяет:

* Реализовывать методы GET, POST, DELETE для моделей CATEGORIES, GENRES;
* Реализовывать методы GET, POST, PATCH, DELETE для моделей TITLES, REVIEWS, COMMENTS и USERS;
* Авторизовываться и аутентифицироваться через JWT-токен;
* Разграничивать доступ для неавторизованных, авторизованных пользователей и авторов;
* Разграничивать доступ для авторизованных пользователей и модератора/администратора;
* В данном проекте отсутствует какой-либо фронтенд или любая другая визуальная составляющая. Запросы к серверу и ответы от него приходят в формате ".JSON"
___

Стек технологий использованный в проекте:
* Python 3.9
* Django 3.2
* Django REST Framework 3.12.4
* Djoser 2.1.0
* Аутентификация по JWT-токену
___

### Как запустить проект:

Клонировать репозиторий и перейти в него в командной строке:
~~~bash
https://github.com/IvanKorch1289/api_yamdb.git
~~~
~~~bash
cd api_yamdb/
~~~

При необходимости создать и активировать виртуальное окружение:
~~~bash
python3 -m venv venv
~~~
~~~bash
source venv/bin/activate
~~~

Установить зависимости из файла requirements.txt:
~~~bash
python3 -m pip install --upgrade pip
~~~
~~~bash
pip install -r requirements.txt
~~~

Выполнить миграции:
~~~bash
python3 manage.py migrate
~~~

Запустить проект:
~~~bash
python3 manage.py runserver
~~~

___

### Как заполнить базу данных тестовыми данными:

* Проверить наличие тестовых данных в директории
"_api_yamdb\static\data_"

* Выполнить скрипт:
~~~bash
python3 api_yamdb/manage.py add_file_in_db
~~~
___

### Алгоритм регистрации пользователей.

* Пользователь отправляет POST-запрос на добавление нового пользователя с параметрами email и username на эндпоинт /api/v1/auth/signup/.
* YaMDB отправляет письмо с кодом подтверждения (confirmation_code) на адрес email.
* Пользователь отправляет POST-запрос с параметрами username и confirmation_code на эндпоинт /api/v1/auth/token/, в ответе на запрос ему приходит token (JWT-токен).
* При желании пользователь отправляет PATCH-запрос на эндпоинт /api/v1/users/me/ и заполняет поля в своём профайле (описание полей — в документации).

### Пользовательские роли.

* Аноним — может просматривать описания произведений, читать отзывы и комментарии.
* Аутентифицированный пользователь (user) — может, как и Аноним, читать всё, дополнительно он может публиковать отзывы и ставить оценку произведениям (фильмам/книгам/песенкам), может комментировать чужие отзывы; может редактировать и удалять свои отзывы и комментарии. * Эта роль присваивается по умолчанию каждому новому пользователю.
* Модератор (moderator) — те же права, что и у Аутентифицированного пользователя плюс право удалять любые отзывы и комментарии.
* Администратор (admin) — полные права на управление всем контентом проекта. Может создавать и удалять произведения, категории и жанры. Может назначать роли пользователям.
* Суперюзер Django — обладет правами администратора (admin)

### Регистрация нового пользователя.

> Получить код подтверждения на переданный email. Права доступа: Доступно без токена. Использовать имя 'me' в качестве username > запрещено. Поля email и username должны быть уникальными. Должна быть возможность повторного запроса кода подтверждения.

```
POST http://127.0.0.1:8000/api/v1/auth/signup/
```

### Получение JWT-токена.

> Получение JWT-токена в обмен на username и confirmation code. Права доступа: Доступно без токена.

```
POST http://127.0.0.1:8000/api/v1/auth/token/
```
___

## Примеры некоторых запросов к API:

### Для неавторизованных пользователей (доступ только в режиме чтения).

#### Получение всех категорий:

```
GET http://127.0.0.1:8000/api/v1/categories/
```
Права доступа: Доступно без токена

```
{
  "count": 0,
  "next": "string",
  "previous": "string",
  "results": [
    {
      "name": "string",
      "slug": "^-$"
    }
  ]
}
```

#### Получение списка всех жанров:

```
GET http://127.0.0.1:8000/api/v1/genres/
```
Права доступа: Доступно без токена

```
{
  "count": 0,
  "next": "string",
  "previous": "string",
  "results": [
    {
      "name": "string",
      "slug": "^-$"
    }
  ]
}
```

#### Получение списка всех произведений:

```
GET http://127.0.0.1:8000/api/v1/titles/
```
Права доступа: Доступно без токена

```
{
  "count": 0,
  "next": "string",
  "previous": "string",
  "results": [
    {
      "id": 0,
      "name": "string",
      "year": 0,
      "rating": 0,
      "description": "string",
      "genre": [
        {
          "name": "string",
          "slug": "^-$"
        }
      ],
      "category": {
        "name": "string",
        "slug": "^-$"
      }
    }
  ]
}
```

#### Получение информации о произведении:

```
GET http://127.0.0.1:8000/api/v1/titles/{titles_id}/
```
Права доступа: Доступно без токена

```
{
  "id": 0,
  "name": "string",
  "year": 0,
  "rating": 0,
  "description": "string",
  "genre": [
    {
      "name": "string",
      "slug": "^-$"
    }
  ],
  "category": {
    "name": "string",
    "slug": "^-$"
  }
}
```
#### Получение списка всех отзывов:

```
GET http://127.0.0.1:8000/api/v1/titles/{title_id}/reviews/
```
Права доступа: Доступно без токена

```
{
  "count": 0,
  "next": "string",
  "previous": "string",
  "results": [
    {
      "id": 0,
      "text": "string",
      "author": "string",
      "score": 1,
      "pub_date": "2019-08-24T14:15:22Z"
    }
  ]
}
```

#### Полуение отзыва по id:

```
GET http://127.0.0.1:8000/api/v1/titles/{title_id}/reviews/{review_id}/
```
Права доступа: Доступно без токена

```
{
  "id": 0,
  "text": "string",
  "author": "string",
  "score": 1,
  "pub_date": "2019-08-24T14:15:22Z"
}
```


#### Получение списка всех комментариев к отзыву:

```
GET http://127.0.0.1:8000/api/v1/titles/{title_id}/reviews/{review_id}/comments/
```
Права доступа: Доступно без токена

```
{
  "count": 0,
  "next": "string",
  "previous": "string",
  "results": [
    {
      "id": 0,
      "text": "string",
      "author": "string",
      "pub_date": "2019-08-24T14:15:22Z"
    }
  ]
}
```
___

### Для авторизованных пользователей.

#### Добавление новой категории:

```
POST http://127.0.0.1:8000/api/v1/categories/
```
```
{
  "name": "string",
  "slug": "^-$"
}
```
Права доступа: Администратор. Поле slug каждой категории должно быть уникальным.

#### Удаление категории:

```
DELETE http://127.0.0.1:8000/api/v1/categories/{slug}/
```
Права доступа: Администратор.

#### Добавление жанра:

```
POST http://127.0.0.1:8000/api/v1/genres/
```
```
{
  "name": "string",
  "slug": "^-$"
}
```
Права доступа: Администратор. Поле slug каждого жанра должно быть уникальным.

#### Удаление жанра:

```
DELETE http://127.0.0.1:8000/api/v1/genres/{slug}/
```
Права доступа: Администратор.

#### Добавление произведения:

```
POST http://127.0.0.1:8000/api/v1/titles/
```
```
{
  "name": "string",
  "year": 0,
  "description": "string",
  "genre": [
    "string"
  ],
  "category": "string"
}
```
Права доступа: Администратор. Нельзя добавлять произведения, которые еще не вышли (год выпуска не может быть больше текущего). При добавлении нового произведения требуется указать уже существующие категорию и жанр.

#### Удаление произведения:

```
DELETE http://127.0.0.1:8000/api/v1/titles/{titles_id}/
```
Права доступа: Администратор.

#### Добавление нового отзыва:

```
POST http://127.0.0.1:8000/api/v1/titles/{title_id}/reviews/
```
```
{
  "text": "string",
  "score": 1
}
```
Пользователь может оставить только один отзыв на произведение. Права доступа: Аутентифицированные пользователи.

#### Частичное обновление отзыва по id:

```
PATCH http://127.0.0.1:8000/api/v1/titles/{title_id}/reviews/{review_id}/
```
```
{
  "text": "string",
  "score": 1
}
```
Права доступа: Автор отзыва, модератор или администратор.

#### Добавление комментария к отзыву:

```
POST http://127.0.0.1:8000/api/v1/titles/{title_id}/reviews/{review_id}/comments/
```
```
{
  "text": "string"
}
```
Права доступа: Аутентифицированные пользователи.

#### Удаление комментария к отзыву:

```
DELETE http://127.0.0.1:8000/api/v1/titles/{title_id}/reviews/{review_id}/comments/{comment_id}/
```
Права доступа: Автор комментария, модератор или администратор.

#### Получение списка всех пользователей:

```
GET http://127.0.0.1:8000/api/v1/users/
```
Права доступа: Администратор

```
{
  "count": 0,
  "next": "string",
  "previous": "string",
  "results": [
    {
      "username": "^w\\Z",
      "email": "user@example.com",
      "first_name": "string",
      "last_name": "string",
      "bio": "string",
      "role": "user"
    }
  ]
}
```

#### Изменение данных пользователя по username:

```
PATCH http://127.0.0.1:8000/api/v1/users/{username}/
```
Права доступа: Администратор. Поля email и username должны быть уникальными.

```
{
  "username": "^w\\Z",
  "email": "user@example.com",
  "first_name": "string",
  "last_name": "string",
  "bio": "string",
  "role": "user"
}
```

#### Получение данных своей учетной записи:

```
GET http://127.0.0.1:8000/api/v1/users/me/
```
Права доступа: Любой авторизованный пользователь

```
{
  "username": "^w\\Z",
  "email": "user@example.com",
  "first_name": "string",
  "last_name": "string",
  "bio": "string",
  "role": "user"
}
```

#### Получение данных своей учетной записи:

```
PATCH http://127.0.0.1:8000/api/v1/users/me/
```
Права доступа: Любой авторизованный пользователь Поля email и username должны быть уникальными.

```
{
  "username": "^w\\Z",
  "email": "user@example.com",
  "first_name": "string",
  "last_name": "string",
  "bio": "string"
}
```
#### Проект разработали:

* Корч И.И., teamlead - https://github.com/IvanKorch1289
* Антонов Д.С. - https://github.com/Annttonov
* Карпова Е.М. - https://github.com/karpova-el-m

