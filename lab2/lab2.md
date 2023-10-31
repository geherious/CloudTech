# Лабораторная работа 2# Лабораторная работа 1
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


![Рисунок](https://github.com/geherious/CloudTech/blob/master/lab1/Images/img_4.jpg)

## Вывод:
Мы установили ssh соединение между 3 компьютерами и передали файл с одного компьютера на другой, используя третий.

