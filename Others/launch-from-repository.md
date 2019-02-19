# Инструкция для запуска приложения CoLiTec из репозитория

### Расположение

Скомпилированные модули находятся репозиторий **"clt/trunk"**, в директории **"Bin\<sys\>\<cpu\>"**, где:
- cpu - разряность, возможные значения: (x32, x64)
- sys - целевая ос, возможные значения: (windows, ubuntu)

BinUbuntu32<br/>
BinUbuntu64<br/>
BinWindows32<br/>
BinWindows64<br/>

### Подготовка

1. Определить typ - класс приложения, возможные значения: (vs, as, sat)<br/>
2. Установить typ в файле конфигурации "colitec.conf", узел "\<Type\>typ-value\</Type\>"<br/>
3. Скопировать файлы "colitec.conf", "colitec-\<typ\>.conf", "config.conf" директорию "colitec\conf"<br/>
