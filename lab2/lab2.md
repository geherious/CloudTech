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

Сперва создадим окружение с помощью команды
```
python3 -m venv env
```
![Рисунок](https://github.com/geherious/CloudTech/blob/master/lab1/Images/img_1.jpg)

Также необходимо было открыть порты для входящих подключений в брандмауэре Windows и отключить антивирус, чтобы появилась возможность подключаться.
![Рисунок](https://github.com/geherious/CloudTech/blob/master/lab1/Images/img_2.jpg)

В Ubuntu необходимо настроить фаервол, прописав команды:
- sudo ufw allow ssh
- sudo ufw enable

## Подключение:
На компьютерах с Ubuntu, в основной операционной системе получаем ip адреса с помощью команды ipconfig (Windows). Затем с Mac подключаемся к первому компьютеру с помощью команды ssh, указывая порт, имя пользователя, ip, а затем и пароль.

![Рисунок](https://github.com/geherious/CloudTech/blob/master/lab1/Images/img_3.jpg)

Для передачи файла используется команда scp, которой мы передаем порт, начальную папку на компьютере 1, имя пользователя и его ip и конечную папку. Далее вводим пароль и созданный ранее myfile передается на второй компьютер.

![Рисунок](https://github.com/geherious/CloudTech/blob/master/lab1/Images/img_4.jpg)

## Вывод:
Мы установили ssh соединение между 3 компьютерами и передали файл с одного компьютера на другой, используя третий.

