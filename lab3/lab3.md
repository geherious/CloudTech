# Лабораторная работа 3
## Цель работы:
Научиться работать с github actions.

## Предварительная настройка:
Будем использовать хороший dockerfile из предыдущей лабораторной работы:
```
FROM python:3.10

RUN apt-get update && apt-get install -y curl

COPY server.py ./
 
EXPOSE 8000
 
CMD ["python", "server.py"]

```

## Создание github action
```
name: ci

on:
  push:
    branches:
      - "master"

jobs:
  build:
    runs-on: ubuntu-22.04
    steps:
      -
        name: Checkout
        uses: actions/checkout@v4
      -
        name: Login to Docker Hub
        uses: docker/login-action@v3
        with:
          username: ${{ secrets.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}
      -
        name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3
      -
        name: Build and push
        uses: docker/build-push-action@v5
        with:
          context: ./lab3/good/
          file: ./lab3/good/Dockerfile
          push: true
          tags: ${{ secrets.DOCKERHUB_USERNAME }}/cloud-tech:latest
```

- В начале мы указываем название и при каких действия срабатывает этот action.
- Далее указываем, что нужно сделать (jobs).
- У каждого задания есть платформа, на которой оно выполняется и шаги
- У каждого шага есть имя (необязательно) и "uses", в котором указывается другой github action, функционал которого мы используем. Этим действиям можно передавать аргументы с помощью "with".

Разберем созданный action
Он срабатывает при пуше в мастер и собирается на убунте 22 версии. Содержит одно задание, состоящее из нескольких шагов:
1. Checkout. Мы используем action checkout, который при стандартных настройках извлекает последний commit и дает к нему доступ остальным процессами.
2. Login to Docker Hub. Использует login-action, чтобы произвести вход в аккаунт на докерхабе. Нужно указать логин и пароль.
3. Set up Docker Buildx. Использует docker/setup-buildx-action, чтобы создать builder для работы с докером.
4. Build and push. Использует docker/build-push-action. Используя предыдушую зависимость, строит образ и пушит его в докерхаб. Нужно указать несколько параметров: контекст, путь к докер файлу, нужно ли пушить образ и теги образа

Также в этом action мы использем github secrets для хранения секретов, а точнее логина и пароля от докерхаба.

## Тестирование
Сделаем пуш в мастер и посмотрим на результат. Action успешно выполняется
![Рисунок](https://github.com/geherious/CloudTech/blob/master/lab3/images/img-1.png)

Образ появился на докерхабе
![Рисунок](https://github.com/geherious/CloudTech/blob/master/lab3/images/img-2.png)
## Вывод:
Мы разобрались в работе github actions и создали базовый action для работы с докером
