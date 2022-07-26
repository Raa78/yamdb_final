# YaMDb

![YaMDb Workflow](https://github.com/Raa78/yamdb_final/actions/workflows/yamdb_workflow.yml/badge.svg)

Проект развернут по адресу - http://84.201.142.84/redoc/

## Описание:

Проект YaMDb собирает отзывы (Review) пользователей на произведения (Titles). Произведения делятся на категории: «Книги», «Фильмы», «Музыка». Список категорий (Category) может быть расширен администратором (например, можно добавить категорию «Изобразительное искусство» или «Ювелирка»).

## Стек технологий:
* Python 
* Django 
* DjangoRestFramework 
* PostgresSQL 
* Nginx
* Docker, Docker-compose, DockerHub

## Пример запроса к API:
*Получение списка всех произведений*

*Права доступа: Доступно без токена*
```
.../api/v1/titles (GET)
```

*Пример ответа*
```
[
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
            "slug": "string"
          }
        ],
        "category": {
          "name": "string",
          "slug": "string"
        }
      }
    ]
  }
]
```

## Запуск проекта:
1. Скачайте образ из репозитория DockerHub:
```
docker pull raa78/yamdb:v1.20.07.2022
```

2. Клонируйте репозиторий проекта с GitHub:
```
git clone https://github.com/Raa78/infra_sp2.git
```

3. В терминале, перейдите в каталог: 
```
cd infra_sp2/infra/
```

и создайте там файл .evn для хранения ключей:
```
SECRET_KEY = 'секретный ключ Django проекта'
DB_ENGINE=django.db.backends.postgresql # указываем, что используем postgresql
DB_NAME=postgres # указываем имя созданной базы данных
POSTGRES_USER=postgres # указываем имя своего пользователя для подключения к БД
POSTGRES_PASSWORD=postgres # устанавливаем свой пароль для подключения к БД
DB_HOST=db # указываем название сервиса (контейнера)
DB_PORT=5432 # указываем порт для подключения к БД 
```
Для генерация SECRET_KEY Django проекта воспользуйтесь сайтом https://djecrety.ir. 

4. Запустите окружение:

* Запустите docker-compose, развёртывание контейнеров выполниться в «фоновом режиме»
```
docker-compose up -d --build
```

* выполните миграции:
```
docker-compose exec web python manage.py migrate
```

* cоздайте суперпользователя, введите - логин,почту, пароль:
```
docker-compose exec web python manage.py createsuperuser
```

*  соберите статику:
```
docker-compose exec web python manage.py collectstatic --no-input
```

*  команда для остановки и удаления собранных контейнеров:
```
docker-compose down -v 
```


## Функционал проекта

### Пользовательские роли
**Аноним** — может просматривать описания произведений, читать отзывы и комментарии.

**Аутентифицированный пользователь (user)** — может, как и Аноним, читать всё, дополнительно он может публиковать отзывы и ставить оценку произведениям (фильмам/книгам/песенкам), может комментировать чужие отзывы; может редактировать и удалять свои отзывы и комментарии. Эта роль присваивается по умолчанию каждому новому пользователю.

**Модератор (moderator)** — те же права, что и у Аутентифицированного пользователя плюс право удалять любые отзывы и комментарии.

**Администратор (admin)** — полные права на управление всем контентом проекта. Может создавать и удалять произведения, категории и жанры. Может назначать роли пользователям.

**Суперюзер Django** — обладает правами администратора (admin)

Для аутентификации используйте JWT-токены.


### Самостоятельная регистрация

Для самостоятельной регистрации нужно отправить POST запрос на адрес .../api/v1/auth/signup/:

```
{
  "email": "string",
  "username": "string"
}
```

В ответе будет придёт **confirmation_code** он понадобиться для получения JWT токена.
Для того чтобы получить JWT токен нужно отправить POST запрос на адрес .../api/v1/auth/token/:
```
{
  "username": "string",
  "confirmation_code": "string"
}
```

## Полный функционал проекта можно найти после его запуска по ссылке:
```
http://localhost/redoc/
```
