# Лабораторная работа 2
## Цель работы:
Выявить плохие практики при написании Dockerfile и работе с контейнерами.

## Предварительная настройка:
Установим докер на VM с Ubuntu, использую официальную документацию:
```
sudo apt-get update
sudo apt-get install ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

# Add the repository to Apt sources:
echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```
В роли содержимого контейнера будет выступать простой http server на python. Для начала мы создадим две папки для 2 образов: хорошего и плохого. Они будут различаться только Dockerfile.

Сперва создадим и подключим окружение с помощью команды
```
python3 -m venv env
source myenv/bin/activate
```
Создадим server.py
```
import http.server
import socketserver
 
PORT = 8000
 
Handler = http.server.SimpleHTTPRequestHandler
 
with socketserver.TCPServer(("", PORT), Handler) as httpd:
    print("Сервер запущен на порту", PORT)
    httpd.serve_forever()
```
Теперь созадим плохой Dockerfile
```
FROM python:latest
 
RUN apt-get update
RUN apt-get install -y curl
RUN apt-get install python3-pip -y

COPY server.py ./
 
EXPOSE 8000
 
CMD python server.py

```
И хороший
```
FROM python:3.10

RUN apt-get update && apt-get install -y curl

COPY server.py ./
 
EXPOSE 8000
 
CMD ["python", "server.py"]

```
Создадим image для плохого и хорошего докер файла, с помощью команд, находясь в соответствующих названию папках
```
docker build -t bad .
docker build -t good .
```

![Рисунок](https://github.com/geherious/CloudTech/blob/master/lab2/images/img-1.jpg)
![Рисунок](https://github.com/geherious/CloudTech/blob/master/lab2/images/img-2.jpg)

Список образов:

![Рисунок](https://github.com/geherious/CloudTech/blob/master/lab2/images/img-3.jpg)

Запустим например хороший докер файл

![Рисунок](https://github.com/geherious/CloudTech/blob/master/lab2/images/img-4.jpg)

Тогда перейдя по ссылке localhost:8000 в браузере, можно увидеть файловую систему

![Рисунок](https://github.com/geherious/CloudTech/blob/master/lab2/images/img-5.jpg)

## Сравнение докер файлов
1. ### Использование конкретных версий
   В плохом докер файле используется базовый образ версии latest, что может привести к ошибкам и несовместимости компонентов при выходе новых версий.
   В хорошоем докер файле используется конкретная версия.
   ```
   # плохо
   FROM python:latest
   # хорошо
   FROM python:3.10
   ```
   

## Вывод:
Мы установили ssh соединение между 3 компьютерами и передали файл с одного компьютера на другой, используя третий.

