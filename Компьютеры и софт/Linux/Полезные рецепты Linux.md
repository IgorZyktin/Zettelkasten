# Полезные рецепты





- [{{ компьютеры }}](../../__tags/kompytery.md)
- [{{ программное обеспечение }}](../../__tags/programmnoe_obespechenie.md)
- [{{ linux }}](../../__tags/linux.md)
- [{{ полезные рецепты }}](../../__tags/poleznye_retsepty.md)
- [{{ системное администрирование }}](../../__tags/sistemnoe_administrirovanie.md)


1. [Конвертация текста из cp866](#Конвертация-текста-из-cp866)
1. [Пинг с метками времени](#Пинг-с-метками-времени)
1. [OOM killer](#OOM-killer)
1. [Нагрузить процессор](#Нагрузить-процессор)
1. [Узнать кто задействует порт](#Узнать-кто-задействует-порт)
1. [Узнать тип файловой системы](#Узнать-тип-файловой-системы)
1. [Создать файл с мусорными данными](#Создать-файл-с-мусорными-данными)

## Конвертация текста из cp866

При переносе данных между терминалами/процессами/виртуальными машинами можно
иногда получить кракозябры из-за различия кодировок.

Получить вменяемый текст можно вот таким способом:

```shell
echo '╧Ёшёюхфшэхээюх ъ ёшёЄхьх єёЄЁющёЄтю эх ЁрсюЄрхЄ. ' | iconv -f utf8 -t cp866 | iconv -f cp1251 -t utf8
```

```shell
Присоединенное к системе устройство не работает.
```

## Пинг с метками времени

Иногда бывает необходимо видеть метки времени при выполнении ping. Рецепт для
CentOS.

```shell
ping www.google.fr | bash -c 'trap "" INT; awk "{print strftime(\"%Y-%m-%d %H:%M:%S - \") \$0}"'
```

## OOM killer

Если процесс жрёт слишком много памяти, система может убить его. Узнать об этом
можно тут:

```shell
dmesg | less
```

Попытаться узнать факт убийства:

```shell
#!/bin/bash
<your_job_here>
ret=$?
#
#  returns > 127 are a SIGNAL
#
if [ $ret -gt 127 ]; then
        sig=$((ret - 128))
        echo "Got SIGNAL $sig"
        if [ $sig -eq $(kill -l SIGKILL) ]; then
                echo "process was killed with SIGKILL"
                dmesg > $HOME/dmesg-kill.log
        fi
fi
```

## Нагрузить процессор

Может быть полезно для создания искусственной нагрузки при проверке работы.

```shell
cat /dev/random > /dev/null
```

## Узнать кто задействует порт

Для получения информации об имени программы надо быть рутом, иначе вместо имён
будут прочерки.

```shell
netstat -nltp
```

Пример ответа:

```shell
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp        0      0 127.0.0.1:5900          0.0.0.0:*               LISTEN      30398/qemu-kvm
tcp        0      0 127.0.0.1:5901          0.0.0.0:*               LISTEN      30464/qemu-kvm
tcp        0      0 127.0.0.1:5902          0.0.0.0:*               LISTEN      30527/qemu-kvm
tcp        0      0 127.0.0.1:5903          0.0.0.0:*               LISTEN      30590/qemu-kvm
tcp        0      0 0.0.0.0:111             0.0.0.0:*               LISTEN      1888/rpcbind
tcp6       0      0 :::111                  :::*                    LISTEN      1888/rpcbind
tcp6       0      0 :::22                   :::*                    LISTEN      2587/sshd
tcp6       0      0 ::1:25                  :::*                    LISTEN      4101/master
```

## Узнать тип файловой системы

```shell
lsblk -f
```

## Создать файл с мусорными данными

```shell
head -c 1G </dev/urandom >myfile
```

Если head старый и не понимает сокращения:

```shell
head -c 1073741824 </dev/urandom >myfile
```
