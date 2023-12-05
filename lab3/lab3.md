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
![Рисунок](https://github.com/geherious/CloudTech/blob/master/lab2/images/img-5.jpg)

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
          context: .
          file: ./good/Dockerfile
          push: true
          tags: ${{ secrets.DOCKERHUB_USERNAME }}/cloud-tech:latest
```
- В начале мы указываем название и при каких действия срабатывает этот action.
- Далее указываем, что нужно сделать (jobs).
## Вывод:
Мы разобрались в работе докер контейнеров, выявили плохие практики написания докер файлов и использовании контейнеров.
