## Task01:
 - Создан service account `bob` с ролью `admin` в кластере
 - Создан service account `dave` с ролью `wo-access` без прав в кластере

## Task02:
 - Создан namespace `prometheus`
 - Создан service account `carol` в namespace `prometheus`
 - Создана роль `pod-reader` с verbs ["get", "list", "watch"] для `pod`
 - Создан role binding `group-prometheus-pod-reader` с ссылкой на роль `pod-reader`

## Task03:
 - Создан namespace `dev`
 - Создан service account `jane` и `ken` в namespace `dev`
 - Создан role binding в `dev` `jane-admin` связывающий service account `jane` и кластерную роль  `admin`
 - Создан role binding в `dev` `ken-view` связывающий service account `ken` и кластерную роль  `view`

## Как запустить проект:
 - Task01: `kubectl apply -f kubernetes-security/task01/`
 - Task02: `kubectl apply -f kubernetes-security/task02/`
 - Task03: `kubectl apply -f kubernetes-security/task03/`

## Как проверить: 
Task01: 
```sh
kubectl auth can-i get deployments --as system:serviceaccount:default:bob -n default
yes
kubectl auth can-i get pods --as system:serviceaccount:default:dave
no
```
Task02: 
```sh
kubectl auth can-i get deployments --as system:serviceaccount:prometheus:carol
no
kubectl auth can-i list pods --as system:serviceaccount:prometheus:carol -n prometheus
yes
kubectl auth can-i list pods --as system:serviceaccount:prometheus:carol
yes
```
Task03: 
```sh
kubectl auth can-i get deployments --as system:serviceaccount:dev:jane
no
kubectl auth can-i create deployments --as system:serviceaccount:dev:jane -n dev
yes
kubectl auth can-i get deployments --as system:serviceaccount:dev:ken
no
kubectl auth can-i get deployments --as system:serviceaccount:dev:ken -n dev
yes
kubectl auth can-i create deployments --as system:serviceaccount:dev:ken -n dev
no
```
