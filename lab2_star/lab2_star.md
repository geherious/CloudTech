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


![Рисунок](https://github.com/geherious/CloudTech/blob/master/lab2/images/img-5.jpg)
месте с ним при его смерти, поэтому надо использовать mount и volume.

## Вывод:
Мы разобрались в работе докер контейнеров, выявили плохие практики написания докер файлов и использовании контейнеров.
