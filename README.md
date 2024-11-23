# Приложение Kittigram 🐾
### Описание 
**Kittygram** — это социальная сеть, созданная для обмена фотографиями ваших любимых питомцев.  
В приложении пользователи могут:
- Зарегистрироваться и авторизоваться.
- Добавлять карточки своих домашних питомцев.
- Редактировать карточки.
- Просматривать карточки других участников сообщества.

---
### Технологии: 
- **Backend**:
  - Python 3.9
  - Django 3.2
  - Django REST Framework (DRF) 3.12
  - JWT для авторизации
- **Frontend**:
  - HTML/CSS
  - JavaScript
- **Инфраструктура**:
  - Gunicorn
  - Nginx
  - Docker / Docker Compose
  - Certbot для SSL-сертификатов
  - GitHub Actions для CI/CD

---

### Разворачивание сервиса на сервере 
Деплой проекта осуществляется автоматически при внесении изменений и пуше 
в ветку **main** репозитория на GitHub.
В корне проекта создайте файл `.env` и добавьте в него следующие переменные окружения:
```env
SECRET_KEY=<ваш_секретный_ключ>
DEBUG=False
ALLOWED_HOSTS=<разрешенные_хосты>
POSTGRES_DB=<название_базы_данных>
POSTGRES_USER=<пользователь_базы>
POSTGRES_PASSWORD=<пароль_базы>
DB_HOST=db
DB_PORT=5432
```

1. **При пуше в ветку `main`:**
    - Запускаются тесты и линтеры для проверки качества кода.
    - Собираются Docker-образы приложения.
    - Образы пушатся на Docker Hub.
  
2. **После успешной сборки:**
    - Подключается сервер.
    - Копируется и запускается файл `docker-compose.production.yml`.
    - Выполняются миграции и сборка статики.
  
3. **Уведомление:**
    - После успешного деплоя отправляется уведомление в Telegram-бот о завершении процесса.


### Разворачивание сервиса на сервере вручную

1. Подготовьте файл `.env` в корне проекта с переменными.
2. Запустите проект, используя файл `docker-compose.production.yml`:
 ```bash
    docker compose -f docker-compose.production.yml up -d
 ```
3. Выполните миграции базы данных:
 ```bash
    docker compose -f docker-compose.production.yml exec backend python manage.py migrate
 ```
4. Соберите и скопируйте статику:
 ```bash
    docker compose -f docker-compose.production.yml exec backend python manage.py collectstatic
    docker compose -f docker-compose.production.yml exec backend cp -r /app/collected_static/. /backend_static/static/
 ```

---
### Автор

[skhfh](https://github.com/skhfh)
