# Проделанная работа:
  - Создан тестовый кластер в GKE
  - Установлен helm3 
  - С помощью helm установлен nginx-ingress
  - С помощью helm установлен cert-manager
  - Установлен let's-encrypt `ClusterIssuer` с челленжем `http01`
  - Установлен chartmuseum и для него автоматом выпущен сертификат LE
  - Установлен `hipster-shop` в одноименный `namespace`
  - Сервис `frontend` выделен и шаблонизирован под `helm3`
  