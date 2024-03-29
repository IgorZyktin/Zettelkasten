# Docker

Docker это инструмент виртуализации, позволяющий упаковывать приложения в
удобные для развёртывания контейнеры.



- [{{ компьютеры }}](../../__tags/kompytery.md)
- [{{ программное обеспечение }}](../../__tags/programmnoe_obespechenie.md)
- [{{ Программы }}](../../__tags/programmy.md)
- [{{ полезные рецепты }}](../../__tags/poleznye_retsepty.md)
- [{{ системное администрирование }}](../../__tags/sistemnoe_administrirovanie.md)
- [{{ Виртуализация }}](../../__tags/virtualizatsiya.md)
- [{{ linux }}](../../__tags/linux.md)
- [{{ Windows }}](../../__tags/windows.md)
- [{{ Docker }}](../../__tags/docker.md)


## Описание

Docker не является полномасштабной виртуализацией т.к. во многом полагается на
хостовую операционную систему. Построен на linux инструментах, таких как
cgroups и namespaces. Все контейнеры зависят от ядра операционной системы хоста
и способны дополнять его функционал. Фактически они выступают обёртками над
ядром, это позволяет формировать окружения практически без потерь в
производительности, но работает это только для родственных linux систем. В
отличие от настоящей виртуальной машины, запустить например Windows XP в
контейнере не получится. Написан на языке GO.

[Официальный сайт](https://docs.docker.com/engine/install/)

## Установка Docker на CentOS

```shell
$ sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```

```shell
$ sudo yum install -y yum-utils
```

```shell
$ sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
```

```shell
$ sudo yum install -y docker-ce docker-ce-cli containerd.io
$ sudo systemctl start docker
$ sudo docker run hello-world
```

## Диагностирование проблем с сетью внутри контейнера

При работе через VPN возможна ситуация, когда внутри контейнера не будут
доступны некоторые сетевые ресурсы.

Выполнение команды внутри хостовой ОС:

```shell
$ resolvectl dns
```

```shell
Global:
Link 6 (tun0): 172.21.0.1
Link 5 (docker0):
Link 4 (virbr0-nic):
Link 3 (virbr0):
Link 2 (enp7s0): 192.168.1.1
```

То же самое, но внутри докер образа:

```shell
$ docker run -ti --rm centos:7.6.1810 bash
$ resolvectl dns
```

```shell
nameserver 192.168.1.1
nameserver 172.21.0.1
search beeline mail.msk
```

В итоге скрипты внутри образа не видят нужные куски сети и не могут нормально
работать.

Можно также проверить через nslookup.

На хостовой ОС:

```shell
nslookup <your corporate host>
```

```shell
nslookup <your corporate host>
Server:   172.21.0.1
Address:  172.21.0.1#53

Name:  <your corporate host>
Address: 10.8.7.12
Name:  <your corporate host>
Address: 10.8.7.10
Name:  <your corporate host>
Address: 10.8.7.11
```

### Решение проблемы с видимостью хостов в VPN

```shell
nm-connection-editor
```

Зайти во вкладку VPN, перейти на IPV4 и в поле "Additional search domains"
добавить "<corporate host1>, <corporate host2>, <corporate host3>".
