# Что было сделано
* Подготовлен "кривой" манифест web-pod.yaml на основе файла kubernetes-intro/web-pod.yaml
* Добавлен readiness/liveness probe
* Создан deployment web-pod.yaml
* При maxUnavailable и maxSurge: 0 будет ошибка
```
The Deployment "web" is invalid: spec.strategy.rollingUpdate.maxUnavailable: Invalid value: intstr.IntOrString{Type:0, IntVal:0, StrVal:""}: may not be 0 when `maxSurge` is 0
```
* Создан `ClusterIp` и вполнена проверка, что он работает
* Настройка kube-proxy на работу с ipvs вместо iptables 
* Установлен MetalLB
* Добавлен маршрут до сети настроеной для MetalLB
* Установлен/настроен ingress-nginx