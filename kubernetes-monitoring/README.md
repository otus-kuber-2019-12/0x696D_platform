### Как запустить проект:
 - Создаем namespace `prom-oper` для prometheus-operator
 - Ставим nginx в namespace `default`
 - Ставим сервис для `nginx` c эндпоинтами `http` и `metrics`
 - Устанавливаем prometheus-operator в созданый `namespace`
```bash
kubectl apply -f ns-prom-oper.yaml
kubectl apply -f deploymanet.yaml
kubectl apply -f service.yaml
helm upgrade --install prometheus stable/prometheus-operator --namespace=prom-oper -f values.yaml
```
#### После установки, вкатываем service-monitor
```bash
kubectl apply -f servicemonitor.yaml
```

#### Проверка:
 - проверяем, что nginx отдает метрики:
```bash
kubectl port-forward <one-of-nginx-pod-name> 9113:9113
curl localhost:9113/metrics
```
 - проверяем, что prometheus-operator увидел service-monitor и добавил в таргеты nginx
```bash
kubectl port-forward -n monitoring prometheus-prometheus-prometheus-oper-prometheus-0 9090:9090
```
 - идем в браузер на `http://localhost:9090/targets` и ищем nginx в default неймспейсе