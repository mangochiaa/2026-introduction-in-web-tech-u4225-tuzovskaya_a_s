University: ITMO University
Faculty: FICT
Course: Введение в веб технологии
Year: 2025/2026
Group: u4225
Author: Тузовская Арина Сергеевна
Lab: Lab2
Date of create: 02.10.2026
Date of finished: 02.10.2026

## Цель работы

Научиться настраивать автоматизированные пайплайны для сборки Docker образов, их публикации в registry и автоматического деплоя при изменении кода.

## Ход работы

### 1. Создание нового репозитория

Был создан новый репозиторий `devops-lab2-tuzovskaya` на GitHub.

![Скриншот 1: Созданный репозиторий](screenshots/lab2-1.png)

Репозиторий склонирован на локальный компьютер:

~~~bash
git clone git@github.com:mangochiaa/devops-lab2-tuzovskaya.git
~~~

![Скриншот 2: Клонирование репозитория](screenshots/lab2-2.png)

### 2. Подготовка проекта

Из первой лабораторной работы были скопированы файлы `app.py`, `requirements.txt`, `Dockerfile`.

![Скриншот 3: Файлы проекта](screenshots/lab2-3.png)

### 3. Создание GitHub Actions workflow

Создана папка `.github/workflows/` и файл `docker-build.yml` со следующим содержимым:

~~~yaml
name: Build and Push Docker Image

on:
  push:
    branches:
      - main

jobs:
  build-and-push:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3

      - name: Log in to Docker Hub
        uses: docker/login-action@v3
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_PASSWORD }}

      - name: Build and push Docker image
        uses: docker/build-push-action@v5
        with:
          context: .
          push: true
          tags: ${{ secrets.DOCKER_USERNAME }}/my-flask-app:latest

      - name: Deploy
        run: echo "Deploying to production server..."
~~~

![Скриншот 4: Workflow файл (первая версия)](screenshots/lab2-4.png)

### 4. Первый коммит и push

~~~bash
git add .
git commit -m "Initial project setup with CI/CD pipeline"
git push origin main
~~~

![Скриншот 5: Первый коммит и push](screenshots/lab2-5.png)

### 5. Первый запуск пайплайна

Пайплайн запустился автоматически, но упал на шаге логина в Docker Hub, потому что секреты ещё не были настроены. Это ожидаемое поведение.

![Скриншот 6: Упавший пайплайн](screenshots/lab2-6.png)

### 6. Создание Access Token на Docker Hub

На Docker Hub был создан Personal Access Token с правами Read & Write.

![Скриншот 7: Access Token](screenshots/lab2-7.png)

### 7. Настройка секретов в GitHub

В настройках репозитория (`Settings → Secrets and variables → Actions`) были добавлены два секрета:

- `DOCKER_USERNAME` — логин на Docker Hub
- `DOCKER_PASSWORD` — Personal Access Token

![Скриншот 8: Секреты в GitHub](screenshots/lab2-8.png)

### 8. Повторный запуск пайплайна

После настройки секретов пайплайн был перезапущен. Он успешно завершился: образ собран и опубликован в Docker Hub.

![Скриншот 9: Успешный пайплайн](screenshots/lab2-9.png)

Все шаги пайплайна выполнены успешно:

![Скриншот 10: Детали успешного пайплайна](screenshots/lab2-10.png)

### 9. Проверка Docker Hub

Образ `mangochiaa/my-flask-app:latest` появился в Docker Hub.

![Скриншот 11: Образ в Docker Hub](screenshots/lab2-11.png)

### 10. Часть со звёздочкой: условный деплой

Workflow был обновлён — теперь он запускается для двух веток (`main` и `develop`) и выполняет разные шаги деплоя в зависимости от ветки:

~~~yaml
on:
  push:
    branches:
      - main
      - develop

# ...

      - name: Build and push Docker image
        uses: docker/build-push-action@v5
        with:
          context: .
          push: true
          tags: ${{ secrets.DOCKER_USERNAME }}/my-flask-app:${{ github.ref_name }}

      - name: Deploy to production
        if: github.ref == 'refs/heads/main'
        run: echo "Deploying to production server..."

      - name: Deploy to development
        if: github.ref == 'refs/heads/develop'
        run: echo "Deploying to development server..."
~~~

Тег образа теперь формируется динамически: для ветки `main` — `:main`, для ветки `develop` — `:develop`.

![Скриншот 12: Обновлённый workflow](screenshots/lab2-12.png)

Изменения закоммичены и запушены в `main`:

![Скриншот 13: Push обновлённого workflow](screenshots/lab2-13.png)

### 11. Создание ветки develop

Создана ветка `develop` для проверки условного деплоя:

~~~bash
git checkout -b develop
~~~

![Скриншот 14: Ветка develop](screenshots/lab2-14.png)

Сделан коммит и push в ветку `develop`:

![Скриншот 15: Push в develop](screenshots/lab2-15.png)

### 12. Проверка условного деплоя

**Для ветки develop:** шаг `Deploy to development` выполнен, а `Deploy to production` пропущен.

![Скриншот 16: Пайплайн для develop](screenshots/lab2-16.png)

**Для ветки main:** шаг `Deploy to production` выполнен, а `Deploy to development` пропущен.

![Скриншот 17: Пайплайн для main](screenshots/lab2-17.png)

### 13. Итог в Docker Hub

В Docker Hub появились 3 тега образа: `latest`, `main` и `develop` — по одному для каждой ветки/версии.

![Скриншот 18: Теги в Docker Hub](screenshots/lab2-18.png)

## Вывод

В ходе лабораторной работы я настроила CI/CD пайплайн с помощью GitHub Actions для автоматической сборки и публикации Docker-образа.

Были освоены:

- Создание GitHub Actions workflow (`.github/workflows/docker-build.yml`);
- Настройка триггеров на push в определённые ветки;
- Использование готовых actions (`actions/checkout`, `docker/setup-buildx-action`, `docker/login-action`, `docker/build-push-action`);
- Безопасная работа с секретами (`secrets.DOCKER_USERNAME`, `secrets.DOCKER_PASSWORD`);
- Создание Personal Access Token на Docker Hub вместо использования пароля;
- Автоматическая сборка и публикация образа в Docker Hub;
- Условный деплой с использованием `if:` — разные действия для веток `main` и `develop`;
- Динамическая подстановка тега через `${{ github.ref_name }}`.

**Ключевой вывод:** CI/CD пайплайн позволяет автоматизировать рутинные операции — сборку, тестирование и публикацию. При каждом пуше в репозиторий образ автоматически пересобирается, что исключает человеческий фактор и ускоряет доставку изменений. Использование секретов и Personal Access Token вместо паролей — стандарт безопасной работы с внешними registry. Условный деплой на основе ветки позволяет реализовать разные стратегии развёртывания: стабильная версия — в продакшн, промежуточная — в dev-окружение.
