University: [ITMO University](https://itmo.ru/ru/)
Faculty: [FICT](https://fict.itmo.ru)
Course: [Введение в веб технологии](https://itmo-ict-faculty.github.io/introduction-in-web-tech/)
Year: 2025/2026
Group: u4225
Author: Тузовская Арина Сергеевна
Lab: Lab0
Date of create: 01.10.2026
Date of finished: 01.10.2026

## Цель работы

Научиться создавать репозитории, настраивать рабочее окружение и изучить основы работы с Git и GitHub.

## Ход работы

### 1. Установка Git и настройка SSH-ключей

Git был установлен на macOS через инструменты командной строки Xcode. Затем настроены имя пользователя и email для подписи коммитов, а также сгенерирован SSH-ключ типа ed25519 для безопасной работы с GitHub.

Проверка установки Git:

~~~bash
git --version
~~~

Настройка пользователя:

~~~bash
git config --global user.name "Тузовская Арина"
git config --global user.email "mangochiaa@gmail.com"
~~~

Генерация SSH-ключа:

~~~bash
ssh-keygen -t ed25519 -C "mangochiaa@gmail.com"
~~~

Проверка связи с GitHub:

~~~bash
ssh -T git@github.com
~~~

Успешный вывод подтверждает корректную настройку: `Hi mangochiaa! You've successfully authenticated`.

![Скриншот 1: Проверка Git и SSH](screenshots/lab0-1.png)

SSH-ключ был добавлен в настройки аккаунта GitHub:

![Скриншот 2: SSH-ключ в GitHub](screenshots/lab0-2.png)

### 2. Создание репозитория на GitHub

Создан публичный репозиторий `devops-lab-tuzovskaya`. При создании не были выбраны автоматические файлы (README, .gitignore, LICENSE), чтобы настроить проект вручную.

![Скриншот 3: Созданный репозиторий](screenshots/lab0-3.png)

### 3. Клонирование репозитория

Репозиторий клонирован на локальный компьютер в папку `~/Projects/devops-lab-tuzovskaya`:

~~~bash
cd ~/Projects
git clone git@github.com:mangochiaa/devops-lab-tuzovskaya.git
cd devops-lab-tuzovskaya
git status
~~~

![Скриншот 4: Клонирование репозитория](screenshots/lab0-4.png)

### 4. Создание файла README.md

С помощью редактора `nano` создан файл `README.md` с описанием проекта, контактными данными и планом изучения DevOps.

Содержимое файла включает:
- Описание проекта;
- Контактные данные автора;
- План изучения DevOps (Git, CI/CD, Docker, Kubernetes, Terraform, мониторинг, облачные платформы).

![Скриншот 5: Создание README.md](screenshots/lab0-5.png)

### 5. Создание файла .gitignore

Создан файл `.gitignore` с исключениями для macOS (`.DS_Store`, `._*`, `**/.DS_Store`), IDE (`.idea/`, `.vscode/`, `*.swp`), логов, `.env`-файлов, а также для Node.js и Python.

![Скриншот 6: Создание .gitignore](screenshots/lab0-6.png)

### 6. Первый коммит в ветку main

Файлы `README.md` и `.gitignore` добавлены в индекс и закоммичены с сообщением `Initial project setup`, после чего отправлены в удалённый репозиторий:

~~~bash
git add .
git commit -m "Initial project setup"
git push origin main
~~~

![Скриншот 7: Первый коммит и push в main](screenshots/lab0-7.png)

### 7. Создание ветки develop

Для дальнейшей разработки создана и активирована ветка `develop`:

~~~bash
git checkout -b develop
git branch
~~~

Вывод команды `git branch` подтверждает переключение — активная ветка отмечена звёздочкой:

~~~
* develop
  main
~~~

![Скриншот 8: Создание ветки develop](screenshots/lab0-8.png)

### 8. Создание файла CONTRIBUTING.md

В ветке `develop` создан файл `CONTRIBUTING.md` с правилами участия в проекте. В нём описаны:

- Как внести изменения (создание feature-ветки, коммит, push, Pull Request);
- Стиль коммитов;
- Правила именования веток (`main`, `develop`, `feature/*`);
- Процесс код-ревью.

![Скриншот 9: Создание CONTRIBUTING.md](screenshots/lab0-9.png)

### 9. Коммит и push в develop

Изменения закоммичены и отправлены в удалённую ветку `develop`:

~~~bash
git add .
git commit -m "Add CONTRIBUTING.md"
git push -u origin develop
~~~

![Скриншот 10: Коммит и push в develop](screenshots/lab0-10.png)

### 10. Создание Pull Request

Через веб-интерфейс GitHub создан Pull Request из ветки `develop` в `main` с описанием внесённых изменений.

**Title:** `Initial project setup`
**Description:** Добавлен файл CONTRIBUTING.md с правилами участия в проекте. Настроена ветка develop, из которой будет вестись разработка.

![Скриншот 11: Создание Pull Request](screenshots/lab0-11.png)

### 11. Мерж Pull Request и удаление ветки

Pull Request успешно смержен в ветку `main` через кнопку **Merge pull request**. После мержа ветка `develop` удалена как из удалённого репозитория (через кнопку **Delete branch**), так и локально:

~~~bash
git checkout main
git pull
git branch -d develop
~~~

На странице Pull Request отображается фиолетовая метка **Merged** — значит, изменения вошли в основную ветку.

![Скриншот 12: Смерженный Pull Request](screenshots/lab0-12.png)

### 12. Итоговая структура репозитория

В репозитории `devops-lab-tuzovskaya` находятся файлы: `README.md`, `.gitignore`, `CONTRIBUTING.md`, а также история коммитов и веток.

![Скриншот 13: Финальная структура репозитория](screenshots/lab0-13.png)

## Вывод

В ходе лабораторной работы я настроила рабочее окружение для работы с Git и GitHub: установила Git, сгенерировала SSH-ключ, добавила его в аккаунт GitHub, настроила имя пользователя и email.

Были освоены базовые команды разработчика:

- Клонирование удалённого репозитория (`git clone`);
- Создание и редактирование файлов (`nano`);
- Работа с индексом Git (`git add`, `git status`);
- Создание коммитов с осмысленными сообщениями (`git commit`);
- Отправка изменений в удалённый репозиторий (`git push`);
- Создание и переключение между ветками (`git checkout -b`);
- Создание Pull Request и его мерж через GitHub;
- Удаление ветки после мержа (удалённо и локально).

**Ключевой вывод:** использование веток и Pull Request — это основа командной работы в DevOps. Ветка `develop` позволяет вести разработку изолированно от стабильной ветки `main`, а Pull Request даёт возможность провести код-ревью перед влитием изменений. Такой workflow предотвращает случайное попадание неоттестированного кода в стабильную версию проекта.

SSH-ключи обеспечивают безопасную аутентификацию без необходимости вводить пароль при каждом `push`. А файлы `README.md`, `.gitignore` и `CONTRIBUTING.md` — это базовый минимум для любого проекта, который делает его понятным для других разработчиков.
