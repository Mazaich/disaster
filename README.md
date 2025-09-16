Домашнее задание по теме «Кластеризация и балансировка нагрузки» - Машаев Роман


Задание 1 

# Конфигурационный файл haproxy:
global
    log /dev/log local0
    log /dev/log local1 notice
    chroot /var/lib/haproxy
    stats socket /run/haproxy/admin.sock mode 660 level admin expose-fd listeners
    stats timeout 30s
    user haproxy
    group haproxy
    daemon

defaults
    log global
    mode tcp
    option tcplog
    option dontlognull
    timeout connect 5000
    timeout client 50000
    timeout server 50000

frontend http_front
    bind *:80
    default_backend http_back

backend http_back
    balance roundrobin
    server server1 127.0.0.1:8000 check
    server server2 127.0.0.1:8001 check

listen stats
    bind *:1936
    mode http
    stats enable
    stats uri /
    stats hide-version
    stats auth admin:password

____________________________________________________________________________________________________________________

1. ![Скриншот на порядок выполнения](https://github.com/Mazaich/disaster/blob/main/Снимок%20экрана%20от%202025-09-15%2016-52-31.png?raw=true)  
2. ![Скриншот на порядок выполнения](https://github.com/Mazaich/disaster/blob/main/Снимок%20экрана%20от%202025-09-15%2017-00-21.png?raw=true)
3. ![Скриншот на порядок выполнения](https://github.com/Mazaich/disaster/blob/main/Снимок%20экрана%20от%202025-09-15%2017-09-41.png?raw=true)
4. ![Скриншот на порядок выполнения](https://github.com/Mazaich/disaster/blob/main/Снимок%20экрана%20от%202025-09-15%2017-17-20.png?raw=true)
5. ![Скриншот на статистику](https://github.com/Mazaich/disaster/blob/main/Снимок%20экрана%20от%202025-09-16%2010-19-22.png?raw=true)
6. [Скриншот на статистику](https://github.com/Mazaich/disaster/blob/main/Снимок%20экрана%20от%202025-09-16%2010-29-40.png?raw=true)


Задание 2 

# Конфигурационный файл haproxy для 2 задания:
global
    log /dev/log    local0
    log /dev/log    local1 notice
    chroot /var/lib/haproxy
    stats socket /run/haproxy/admin.sock mode 660 level admin expose-fd listeners
    stats timeout 30s
    user haproxy
    group haproxy
    daemon

defaults
    log     global
    mode    http
    option  httplog
    option  dontlognull
    timeout connect 5000
    timeout client  50000
    timeout server  50000
    errorfile 400 /etc/haproxy/errors/400.http
    errorfile 403 /etc/haproxy/errors/403.http
    errorfile 408 /etc/haproxy/errors/408.http
    errorfile 500 /etc/haproxy/errors/500.http
    errorfile 502 /etc/haproxy/errors/502.http
    errorfile 503 /etc/haproxy/errors/503.http
    errorfile 504 /etc/haproxy/errors/504.http

frontend http_front
    bind *:80
    acl is_example_local hdr(host) -i example.local
    use_backend example_backend if is_example_local
    default_backend default_backend

backend example_backend
    balance roundrobin
    server server1 127.0.0.1:8001 weight 2
    server server2 127.0.0.1:8002 weight 3
    server server3 127.0.0.1:8003 weight 4

backend default_backend
    mode http
    http-request deny status 403

_____________________________________________________________________________________________________________________


1. ![Скриншот на порядок выполнения](https://github.com/Mazaich/disaster/blob/main/Снимок%20экрана%20от%202025-09-16%2013-04-18.png?raw=true)
2. ![Скриншот на порядок выполнения](https://github.com/Mazaich/disaster/blob/main/Снимок%20экрана%20от%202025-09-16%2013-05-44.png?raw=true)
3. ![Скриншот на порядок выполнения](https://github.com/Mazaich/disaster/blob/main/Снимок%20экрана%20от%202025-09-16%2013-08-35.png?raw=true)
