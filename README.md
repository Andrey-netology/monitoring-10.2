Push-модель – На сервера ставятся агенты, которые отправляют необходимую метрики на сервер. Отличается большим объёмом точек конфигурирования (как правило, по количеству агентов мониторинга), но в то же время дает возможность базе заниматься своей основной задачей – хранить метрики и управлять их жизненным циклом, пассивно ожидая, пока в неё что-нибудь положат.

Плюсы Push-модели:
-Упрощение репликации данных в разные системы мониторинга или их резервные копии (на клиенте настраивается конечная точка отправки или набор таких точек)
-Более гибкая настройка отправки пакетов данных с  метриками (на каждом клиенте задается объем данных и частоту отправки)
-UDP является менее затратным способом передачи данных, вследствии чего может вырости производительность сбора метрик(обратной стороной медали является гарантия доставки пакет

Pull-модель –  В этом случае сам сервер мониторинга ходит по пассивным экспортерам и забирает у них метрики. Плюс – единая точка конфигурирования, сам сервер, которому надо рассказать, что и откуда забирать.  

Плюсы Pull-модели:
-Легче контролировать подлинность данных (гарантия опроса только тех агентов, которые настроены в системе мониторинга)
-Можно настроить единый proxy-server до всех агентов с TLS (таким образом мы можем разнести систему мониторинга и агенты, с гарантией безопасности их взаимодействия)
-Упрощенная отладка получения данных с агентов (так как данные запрашиваются посредством HTTP, можно самостоятельно запрашивать эти данные, используя ПО вне системы мониторинга)


Prometheus	Гибридная : опрашивает определённые сервисы, так же может получать данные от агентов
TICK	PUSH : telegraph (агент) пересылает метрики и логи в хранилища
Zabbix	Гибридная : опрашивает определённые сервисы, так же может получать данные от агентов
VictoriaMetrics	БОльше подходит PUSH, так как метрики записываются в нее, но это (если правильно понял по описанию) система для хранения по большей части, и получает данные, которые к нейпишут другие системы
Nagios	PULL : Использует опрос агентов, которые собирают информацию


root@ubuntu-ansible:/home/admin/monitoring/sandbox# curl http://localhost:8086/ping -v
*   Trying 127.0.0.1:8086...
* TCP_NODELAY set
* Connected to localhost (127.0.0.1) port 8086 (#0)
> GET /ping HTTP/1.1
> Host: localhost:8086
> User-Agent: curl/7.68.0
> Accept: */*
>
* Mark bundle as not supporting multiuse
< HTTP/1.1 204 No Content
< Content-Type: application/json
< Request-Id: eaae3dcb-869a-11ec-8093-0242ac120002
< X-Influxdb-Build: OSS
< X-Influxdb-Version: 1.8.10
< X-Request-Id: eaae3dcb-869a-11ec-8093-0242ac120002
< Date: Sat, 05 Feb 2022 15:47:26 GMT
<
* Connection #0 to host localhost left intact

root@ubuntu-ansible:/home/admin/monitoring/sandbox# curl http://localhost:8888 -v
*   Trying 127.0.0.1:8888...
* TCP_NODELAY set
* Connected to localhost (127.0.0.1) port 8888 (#0)
> GET / HTTP/1.1
> Host: localhost:8888
> User-Agent: curl/7.68.0
> Accept: */*
>
* Mark bundle as not supporting multiuse
< HTTP/1.1 200 OK
< Accept-Ranges: bytes
< Cache-Control: public, max-age=3600
< Content-Length: 336
< Content-Security-Policy: script-src 'self'; object-src 'self'
< Content-Type: text/html; charset=utf-8
< Etag: "336820331"
< Last-Modified: Fri, 08 Oct 2021 20:33:01 GMT
< Vary: Accept-Encoding
< X-Chronograf-Version: 1.9.1
< X-Content-Type-Options: nosniff
< X-Frame-Options: SAMEORIGIN
< X-Xss-Protection: 1; mode=block
< Date: Sat, 05 Feb 2022 15:48:25 GMT
<
* Connection #0 to host localhost left intact
<!DOCTYPE html>
<html>
  <head>
   <meta http-equiv="Content-type" content="text/html; charset=utf-8">
   <title>Chronograf</title>
  <link rel="icon shortcut" href="/favicon.fa749080.ico"><link rel="stylesheet" href="/src.3dbae016.css"></head>
  <body> 
    <div id="react-root" data-basepath=""></div> 
  <script src="/src.fab22342.js"></script> </body></html>

root@ubuntu-ansible:/home/admin/monitoring/sandbox# curl http://localhost:9092/kapacitor/v1/ping -v
*   Trying 127.0.0.1:9092...
* TCP_NODELAY set
* Connected to localhost (127.0.0.1) port 9092 (#0)
> GET /kapacitor/v1/ping HTTP/1.1
> Host: localhost:9092
> User-Agent: curl/7.68.0
> Accept: */*
>
* Mark bundle as not supporting multiuse
< HTTP/1.1 204 No Content
< Content-Type: application/json; charset=utf-8
< Request-Id: b613e5c7-869b-11ec-80e8-000000000000
< X-Kapacitor-Version: 1.6.3
< Date: Sat, 05 Feb 2022 15:53:07 GMT
<
* Connection #0 to host localhost left intact
