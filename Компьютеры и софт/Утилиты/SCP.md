# scp

Утилита для копирования файлов по ssh.



- [{{ компьютеры }}](../../__tags/kompytery.md)
- [{{ программное обеспечение }}](../../__tags/programmnoe_obespechenie.md)
- [{{ linux }}](../../__tags/linux.md)
- [{{ полезные рецепты }}](../../__tags/poleznye_retsepty.md)
- [{{ системное администрирование }}](../../__tags/sistemnoe_administrirovanie.md)
- [{{ утилиты }}](../../__tags/utility.md)
- [{{ scp }}](../../__tags/scp.md)


## Использование

Справка:

```shell
usage: scp [-346BCpqrv] [-c cipher] [-F ssh_config] [-i identity_file]
           [-l limit] [-o ssh_option] [-P port] [-S program] source ... target
```

Дополнительные ключи:

- \-r копировать каталог рекурсивно
- \-p сохранить оригинальные метки создания и модификации файлов
- \-q без вывода подробностей работы
- \-С использовать компрессию при отправке

Пример копирования содержимого каталога с удалённой машины:

```shell
scp -r -P 1027 pi@192.168.1.67:/var/log/nginx/ /local
```

Внимание: scp перезапишет файлы с совпадающими именами без предупреждений.
