University: [ITMO University](https://itmo.ru/ru/)
Faculty: [FICT](https://fict.itmo.ru)
Course: [Введение в веб технологии](https://itmo-ict-faculty.github.io/introduction-in-web-tech/)
Year: 2025/2026
Group: u4225
Author: Тузовская Арина Сергеевна
Lab: Lab1
Date of create: 01.10.2026
Date of finished: 1.10.2026

Цель работы
Научиться работать с Docker: устанавливать Docker, создавать Dockerfile, собирать образы, запускать контейнеры и управлять ими.

Ход работы
1. Установка Docker Desktop
Docker Desktop был установлен для macOS. После запуска приложения проверена его работа.

https://screenshots/lab1-1.png

2. Проверка установки и запуск тестового контейнера
Проверена версия Docker командой docker --version. Запущен тестовый контейнер hello-world.

bash
docker --version
docker run hello-world
https://screenshots/lab1-2.png

3. Базовые команды
Изучены базовые команды для работы с образами и контейнерами:

bash
docker images       # список локальных образов
docker ps           # запущенные контейнеры
docker ps -a        # все контейнеры, включая остановленные
https://screenshots/lab1-3.png

4. Работа с готовым образом Ubuntu
Скачан образ Ubuntu, запущен интерактивный контейнер, внутри установлен пакет curl:

bash
docker pull ubuntu:latest
docker run -it ubuntu bash
apt update && apt install -y curl
curl --version
exit
https://screenshots/lab1-4.png

5. Запуск веб-сервера nginx
Запущен контейнер с веб-сервером nginx:

bash
docker run -d -p 8080:80 --name web-server nginx:alpine
https://screenshots/lab1-5.png

Сервер был проверен через браузер по адресу http://localhost:8080 — открылась стандартная страница «Welcome to nginx!».

Логи контейнера показывают успешный запуск nginx и запросы из браузера (код ответа 200):

https://screenshots/lab1-6.png

Также был выполнен вход внутрь контейнера и просмотрено содержимое файла index.html:

bash
docker exec -it web-server sh
ls /usr/share/nginx/html
cat /usr/share/nginx/html/index.html
exit
https://screenshots/lab1-7.png

6. Управление контейнерами
Изучены команды остановки, запуска и удаления контейнера:

bash
docker ps
docker stop web-server
docker ps
docker ps -a
docker start web-server
docker ps
https://screenshots/lab1-8.png

7. Удаление контейнера и образа
bash
docker stop web-server
docker rm web-server
docker rmi nginx:alpine
docker ps -a
docker images
https://screenshots/lab1-9.png

8. Работа с томами (volumes)
Создан том my-volume, к нему подключён контейнер, внутри создан файл. Затем контейнер был удалён, и создан новый контейнер с тем же томом — файл сохранился, что подтверждает персистентность данных в томах.

bash
docker volume create my-volume
docker run -it --name volume-test -d -v my-volume:/data ubuntu bash
docker exec -it volume-test bash
echo "Hello from volume" > /data/test.txt
cat /data/test.txt
exit

docker rm -f volume-test
docker run -it --name volume-test-2 -d -v my-volume:/data ubuntu bash
docker exec -it volume-test-2 bash
cat /data/test.txt
exit
После создания нового контейнера файл /data/test.txt по-прежнему содержит Hello from volume.

https://screenshots/lab1-10.png

9. Очистка ресурсов
bash
docker rm -f volume-test-2
docker volume rm my-volume
docker rm $(docker ps -aq)
docker rmi $(docker images -q)
docker volume prune -f
https://screenshots/lab1-11.png

10. Создание собственного Dockerfile (часть со звёздочкой)
Создано Flask-приложение: файлы app.py, requirements.txt и Dockerfile.

app.py:

python
from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello():
    return "Hello from Docker!"

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
requirements.txt:

text
Flask==2.0.1
Werkzeug==2.0.3
Примечание: версия Werkzeug была зафиксирована на 2.0.3, потому что Flask 2.0.1 несовместим с новыми версиями Werkzeug (в них удалён модуль werkzeug.urls.url_quote). Без явного указания версии приложение падало с ошибкой ImportError.

Dockerfile:

dockerfile
FROM python:3.9-slim

WORKDIR /app

RUN apt-get update && apt-get install -y curl vim && rm -rf /var/lib/apt/lists/*

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY app.py .

RUN useradd -u 1000 -m appuser
USER appuser

EXPOSE 5000

ENV FLASK_ENV=production

CMD ["python", "app.py"]
https://screenshots/lab1-12.png

Образ собран командой:

bash
docker build -t my-flask-app .
https://screenshots/lab1-13.png

Контейнер запущен:

bash
docker run -d -p 5001:5000 --name flask-container my-flask-app
docker logs flask-container
curl http://localhost:5001
Порт 5001 вместо 5000 использован, потому что порт 5000 на macOS занят системным сервисом AirPlay Receiver (Control Center).

Приложение успешно отвечает:

https://screenshots/lab1-14.png

Также проверено в браузере:

https://screenshots/lab1-15.png

11. Финальная очистка
bash
docker rm -f flask-container
docker rmi my-flask-app
docker ps -a
docker images
Оба списка пусты — все ресурсы удалены.

https://screenshots/lab1-16.png

Вывод
В ходе лабораторной работы я изучила основы работы с Docker. Были освоены:

Установка Docker Desktop и проверка его работы;

Основные команды: docker images, docker ps, docker ps -a, docker run, docker stop, docker start, docker rm, docker rmi;

Скачивание готовых образов (docker pull) и запуск контейнеров из них;

Работа внутри контейнера (установка пакетов через apt, работа с файлами);

Запуск веб-сервера nginx в контейнере с пробросом портов (-p 8080:80);

Просмотр логов контейнера (docker logs);

Подключение к работающему контейнеру (docker exec);

Работа с томами (docker volume) — данные в томе сохраняются при удалении контейнера и доступны в новом контейнере;

Создание собственного образа через Dockerfile — в нём были использованы: базовый образ, WORKDIR, установка системных и Python-пакетов, копирование файлов, создание непривилегированного пользователя (appuser с UID 1000), открытие порта (EXPOSE), переменные окружения (ENV) и команда запуска (CMD).

Ключевой вывод: Docker позволяет упаковать приложение вместе со всеми его зависимостями в изолированный контейнер, который можно запустить на любом компьютере с Docker — независимо от операционной системы и установленных библиотек. Контейнеры создаются из образов, а данные, которые должны сохраняться между запусками, хранятся в томах. Создание собственного Dockerfile даёт полный контроль над окружением приложения и является стандартной практикой при разработке и развёртывании современных приложений.
