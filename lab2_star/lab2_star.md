# Лабораторная работа 2*
## Цель работы:
Запустить Kubernetes кластер. Запустить контейнер, созданный во второй лабораторной работе, внутри этого кластера.
## Установка зависимостей:
### Установка kubectl:
```
curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/kubectl
```
### Установка cri-dockerd:
```
VER=$(curl -s https://api.github.com/repos/Mirantis/cri-dockerd/releases/latest|grep tag_name | cut -d '"' -f 4|sed 's/v//g')
echo $VER

wget https://github.com/Mirantis/cri-dockerd/releases/download/v${VER}/cri-dockerd-${VER}.amd64.tgz
tar xvf cri-dockerd-${VER}.amd64.tgz

sudo mv cri-dockerd/cri-dockerd /usr/local/bin/
```
### Установка conntrack пакета:
```
sudo apt-get install -y conntrack
```
### Установка crictl пакета:
```
VERSION="v1.28.0"
wget https://github.com/kubernetes-sigs/cri-tools/releases/download/$VERSION/crictl-$VERSION-linux-amd64.tar.gz
sudo tar zxvf crictl-$VERSION-linux-amd64.tar.gz -C /usr/local/bin
rm -f crictl-$VERSION-linux-amd64.tar.gz
```
### Установка minikube:
```
curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
chmod +x minikube
sudo mv minikube /usr/local/bin/
```
### Запустим minikube:
```
minikube start --driver=docker
```
## Работа с кластером
### Docker:
В качестве докер образа будем использовать http сервер из предыдущей лабораторной работы
```
FROM python:3.10

RUN apt-get update && apt-get install -y curl

COPY server.py ./
 
EXPOSE 8000
 
CMD ["python", "server.py"]
```

Создадим image и запушим его в Docker Hub, предварительно создав удаленный репозиторий.
```
sudo docker build -t good .
sudo docker login
<Вводим логин и пароль>
sudo docker tag good geherious/cloud-tech:1.0
sudo docker push 752fd55140bb geherious/cloud-tech:1.0
```
![Рисунок](https://github.com/geherious/CloudTech/blob/master/lab2_star/images/img-1.png)
### Deployment:
Создадим deployment.yaml
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: kuber-training
  labels:
    app: data
spec:
  replicas: 1
  selector:
    matchLabels:
      app: data
  template:
    metadata:
      labels:
        app: data
    spec:
      containers:
      - name: data
        image: geherious/cloud-tech:1.0
        ports:
        - containerPort: 8000
          protocol: TCP
          name: http
```
Создадим deployment на основе этого файла и выведем его:
```
kubectl create -f deployment.yaml
kubectl get pod -o wide
```
![Рисунок](https://github.com/geherious/CloudTech/blob/master/lab2_star/images/img-2.png)
### Service:
Создадим service.yaml:
```
apiVersion: v1
kind: Service
metadata:
  labels:
    app: data
  name: data-service
spec:
  ports:
  - port: 8000
    protocol: TCP
    targetPort: 8000
  selector:
    app: data
  type: NodePort
```
Создадим service на основе файла и выведем его:
```
kubectl create -f service.yaml
kubectl get svc -o wide
```
![Рисунок](https://github.com/geherious/CloudTech/blob/master/lab2_star/images/img-3.png)
Для доступа к сервису используем команду, которая выведет url для доступа к сервису:
```
minikube service data-service --url
```
![Рисунок](https://github.com/geherious/CloudTech/blob/master/lab2_star/images/img-4.png)

## Вывод:
Мы разобрались в работе kubernetes, создали простейшие deployment и service.
