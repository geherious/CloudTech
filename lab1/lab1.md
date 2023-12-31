# Лабораторная работа 1
## Цель работы:
Пользуясь терминалом на компьютере А перенести файл с компьютера Б на компьютер С, находящиеся в одной локальной сети

## Предварительная настройка:
Использовались 3 компьютера. 2 работали на Ubuntu на виртуальной машине, 3 - Mac.
Для доступа к виртуальным машинам внутри локальной сети в VirtualBox был настроен проброс портов, то есть запросы приходящие на основную систему в порт 2222 (в нашем случае) пробрасывались в VM в порт 22.

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
