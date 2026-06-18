# Kittygram

Проект **Kittygram** — это социальная сеть для любителей котиков. Пользователи могут создавать карточки своих питомцев, указывая имя, окрас, год рождения, достижения и загружая фотографии. Проект позволяет делиться информацией о котиках и просматривать карточки других пользователей.

## Использованные технологии

| Технология | Назначение | Документация |
|---|---|---|
| **Django 5.1.1** | Backend-фреймворк | https://docs.djangoproject.com/ |
| **Django REST Framework 3.15.2** | Создание REST API | https://www.django-rest-framework.org/ |
| **Djoser 2.3.1** | Библиотека для аутентификации (регистрация, логин, токены) | https://djoser.readthedocs.io/ |
| **PostgreSQL** | База данных | https://www.postgresql.org/docs/ |
| **React** | Frontend-фреймворк | https://react.dev/ |
| **Nginx** | Веб-сервер и reverse-proxy | https://nginx.org/ |
| **Docker** | Контейнеризация приложения | https://docs.docker.com/ |
| **Docker Compose** | Оркестрация контейнеров | https://docs.docker.com/compose/ |
| **Gunicorn** | WSGI-сервер для Django | https://gunicorn.org/ |
| **GitHub Actions** | CI/CD (непрерывная интеграция и деплой) | https://docs.github.com/actions |
| **pytest** | Тестирование | https://docs.pytest.org/ |
| **webcolors 1.11.1** | Библиотека для работы с цветами | https://github.com/ubernostrum/webcolors |
| **Pillow 11.0.0** | Обработка изображений | https://python-pillow.org/ |

## Установка и запуск проекта

### Требования

- Установленные Docker и Docker Compose
- Клонированный репозиторий проекта

### Локальный запуск с помощью Docker

1. Клонируйте репозиторий:

```bash
git clone git@github.com:Maksim-1995/kittygram_final.git
cd kittygram_final
```

2. Создайте файл `.env` в корне проекта со следующими переменными окружения (пример в `.env.example`):

```
SECRET_KEY=django-insecure-ваш-секретный-ключ
DEBUG=True
POSTGRES_DB=kittygram
POSTGRES_USER=kittygram_user
POSTGRES_PASSWORD=password
DB_HOST=db
DB_PORT=5432
```

3. Запустите контейнеры:

```bash
docker compose up -d
```

4. Выполните миграции и сбор статики:

```bash
docker compose exec backend python manage.py migrate
docker compose exec backend python manage.py collectstatic
docker compose exec backend cp -r /app/collected_static /app/static_backend
```

5. Проект будет доступен по адресу: [http://localhost:9000](http://localhost:9000)

### Запуск тестов

```bash
# Создайте виртуальное окружение
python3 -m venv venv
source venv/bin/activate

# Установите зависимости
pip install -r backend/requirements.txt

# Запустите тесты
pytest
```

### Развёртывание на сервере

1. На сервере должны быть установлены Docker и Docker Compose.
2. Скопируйте файлы `docker-compose.production.yml` и `.env` на сервер.
3. Запустите:

```bash
docker compose -f docker-compose.production.yml up -d
```

4. Настройте CI/CD через GitHub Actions (см. файл `.github/workflows/main.yml`).

## Примеры запросов к API

### Регистрация пользователя

```bash
POST /api/users/
Content-Type: application/json

{
  "username": "catlover",
  "password": "strongpassword123"
}
```

**Ответ:** `201 Created`

```json
{
  "id": 1,
  "username": "catlover"
}
```

### Получение токена (авторизация)

```bash
POST /api/auth/token/login/
Content-Type: application/json

{
  "username": "catlover",
  "password": "strongpassword123"
}
```

**Ответ:** `200 OK`

```json
{
  "auth_token": "9944b09199c62bcf9418ad846dd0e4bbdfc6ee4b"
}
```

### Получение списка котиков

```bash
GET /api/cats/
```

**Ответ:** `200 OK`

```json
{
  "count": 2,
  "next": null,
  "previous": null,
  "results": [
    {
      "id": 1,
      "name": "Барсик",
      "color": "black",
      "birth_year": 2020,
      "achievements": [
        {
          "id": 1,
          "achievement_name": "Лучший охотник"
        }
      ],
      "owner": 1,
      "age": 4,
      "image": null,
      "image_url": null
    }
  ]
}
```

### Создание карточки котика (требуется токен)

```bash
POST /api/cats/
Content-Type: application/json
Authorization: Token 9944b09199c62bcf9418ad846dd0e4bbdfc6ee4b

{
  "name": "Мурка",
  "color": "white",
  "birth_year": 2021,
  "achievements": [
    {"achievement_name": "Самый пушистый"}
  ],
  "image": "data:image/jpeg;base64,/9j/4AAQ..."
}
```

**Ответ:** `201 Created`

### Получение списка достижений

```bash
GET /api/achievements/
```

**Ответ:** `200 OK`

```json
[
  {
    "id": 1,
    "achievement_name": "Лучший охотник"
  },
  {
    "id": 2,
    "achievement_name": "Самый пушистый"
  }
]
```

## Чек-лист для проверки перед отправкой задания

- [ ] Проект Taski доступен по доменному имени, указанному в `tests.yml`.
- [ ] Проект Kittygram доступен по доменному имени, указанному в `tests.yml`.
- [ ] Пуш в ветку main запускает тестирование и деплой Kittygram, а после успешного деплоя вам приходит сообщение в телеграм.
- [ ] В корне проекта есть файл `kittygram_workflow.yml`.

## Автор

**Максим** — [GitHub](https://github.com/Maksim-1995)