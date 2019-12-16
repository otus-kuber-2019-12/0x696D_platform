# 0x696D_platform

## Задание 1
Почему все pod в namespace kube-system восстановились после удаления? 

## Ответ
Все поды кроме coredns это [static pod](https://kubernetes.io/docs/tasks/configure-pod-container/static-pod/)'ы управляемые напрямую kubelet'ом. 
Описания подов хранятся тут: 
```sh
# ls -l  /etc/kubernetes/manifests
total 20
-rw-r----- 1 root root 1532 Jan  1  0001 addon-manager.yaml.tmpl
-rw------- 1 root root 1812 Dec 15 11:11 etcd.yaml
-rw------- 1 root root 2889 Dec 15 11:11 kube-apiserver.yaml
-rw------- 1 root root 2492 Dec 15 11:11 kube-controller-manager.yaml
-rw------- 1 root root 1120 Dec 15 11:11 kube-scheduler.yaml
```
coredns описан в деплойменте `kubectl describe deployments coredns -n kube-system`: 
```sh
...
Replicas:               2 desired | 2 updated | 2 total | 2 available | 0 unavailable
...
```
В данном случае выбран реплика сет 2. [ReplicationController](https://kubernetes.io/docs/concepts/workloads/controllers/replicationcontroller/#what-is-a-replicationcontroller) автоматически восстанавливает pod, если он зафейлится/удалится/завершится. 

## Задание 2
Создать Dockerfile, в котором будет описан образ:
* Запускающий web-сервер на порту 8000 (можно использовать любой способ)
* Отдающий содержимое директории /app внутри контейнера (например, если в директории /app лежит файл homework.html, то при запуске контейнера данный файл должен быть доступен по URL http://localhost:8000/homework.html)
* Работающий с UID 1001

## Ответ 2
За основу был взят образ alpine, в который установлен пакет nginx. 
Образ запушен в публичный репозиторий [0x696d/kubernetes-intro](https://hub.docker.com/repository/docker/0x696d/kubernetes-intro) 
В манифест добавлен инит-контейнер с общей шарой `/app` в которую копируется файлик `index.html`. 
Применив манифест `web-pod.yaml`, можно проверить выполнив команду: 
```sh
kubectl port-forward --address 0.0.0.0 pod/web 8000:8000
```
И открыть в браузере http://localhost:8000