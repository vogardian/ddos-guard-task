### Система мониторинга виртуальной машины и СУБД MySQL

## Описание
Система мониторинга (СМ) состоит из группы open-sourse решений, развернутых в контейнерах Docker на одной виртуальной машине с СУБД PostgreSQL, которая так же развернута в контейнере Docker. СМ включает следующие компоненты:
prometheus (сбор и хранение метрик)
node-exporter — инструмент, позволяющий собирать метрики состояния различных компонентов и процессов виртуальных машин Linux
mysqld-exporter — инструмент, позволяющий собирать метрики СУБД MySQL
grafana — решение, собирающее, обрабатывающее и представляющее в виде графиков метрики, собранные prometheus, так же есть функционал рассылки алертов.

Для запуска необходимо:

На виртуальной машине:
1) Клонировать git-репозиторий https://github.com/vogardian/ddos-guard-task
git clone git@github.com:vogardian/ddos-guard-task.git
2) Указать IP адрес виртуальной машины в файле ddos-guard-task/configs/prometheus.yaml, 18 строка
3) Добавить файл ddos-guard-task/.env в корень директории с указанием переменной MYSQL_ROOT_PASSWORD= 
4) Перейти в директорию репозитория и запустить все контейнеры СМ

```
cd  ddos-guard-task
docker compose up --detach
```

На своем хосту:
1) Добавить в файл /etc/hosts запись, указывающую соответствие IP адрес виртуальной машины, где запускается СМ, и доменного имени metrics.local
```
echo '<YOUR VM IP ADDRESS>	metrics.local' >> /etc/hosts
```
2) Открыть в браузере Grafana по адресу metrics.local, ввести стандартный логин/пароль admin/admin после чего задать свой пароль по запросу Grafana
3) В Grafana указать источник данных Prometheus, пройдя по меню: в левом верхнем углу Toggle menu, далее  Connections;   Data Sourses;  + Add new data source; Prometheus; указать адрес подключения http://ddg-prometheus:9090; Save and test
4) Импортировать графики grafana-grafics/MySQL.json и OS General.json, при необходимости заново указать в каждом графике источник данных


