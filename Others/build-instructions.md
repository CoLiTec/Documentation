# Инструкция для сборки дистрибутива CoLiTec

### Интеграционная машина

##### Windows:
- ОС Windows 10, x64
- интерпретатор [Python 3.x](https://www.python.org)
- клиент [SVN](https://tortoisesvn.net) с поддержкой командной строки
- система создания инсталляторов для Windows [Inno Setup](http://www.jrsoftware.org/isdl.php#stable)
- Environment Variables: добавить в переменную Path пути к SVN клиенту и Inno Setup
- авторизоваться через установленный клиент SVN в [репозитории CoLiTec](https://subversion.assembla.com/svn/colitecclosed.clt/trunk)

Для проверки в командной строке выполнить команды:
```posh
python --version
svn --version
iscc
```

##### Ubuntu:
- ОС Ubuntu 16.04 или выше, x64  
- интерпретатор [Python 3.x](https://www.python.org)
- клиент [SVN](https://subversion.apache.org)
- авторизоваться через установленный клиент SVN в [репозитории CoLiTec](https://subversion.assembla.com/svn/colitecclosed.clt/trunk)

Для проверки в терминале выполнить команды:
```posh
python3 --version
svn --version
```

### Подготовка к сборке

##### 1. Checkout\Update директории «Build»

##### 2. Добавить номер новой версии и описание изменений в «Build\changelog.xml»:
- Добавить в начало дерева новую ветку \<changelog\> с атрибутами:<br/>
*«version="%major%.%minor%.%release%.$Rev$"»*<br/>
*«date="$Date$"»*
- Атрибуты предыдущей ветки очистить от ключевых слов *$Date$* и *$Rev$*
- В новую ветку \<changelog\> добавить группирирующие ветки \<added\>, \<changed\> и др. Описание этих веток см. на сайте [keepachangelog.com](https://keepachangelog.com))
- Для каждой группирирующей ветки добавить \<item\>. Возможно в \<item\> добавить атрибут *«class="as,sat,vs"»* – это  определит класс(ы) приложения к которому(-ым) относится данный \<item\>, если атрибут не указан, то данный \<item\> относится ко всем классам.
```xml
<?xml version="1.0" encoding="utf-8"?>
<changelogs tag="1">
  <changelog version="1.9.1.$Rev: 87 $" date="$Date: 2019-01-13 20:42:08Z $">
    <added>
      <item>Software distribution</item>
      <item>New License file</item>
      <item>New Changelog file</item>
    </added> 
  </changelog>
  <changelog version="1.0.0.0." date="2018-01-01 00:00:00Z">
    <added>
      <item>for new features</item>
      <item>features for all: as, sat and vs</item>
      <item class="as">features for as only</item>
      <item class="as,sat">features for as and sat only</item>
    </added>   
    <changed>
      <item>for changes in existing functionality</item>
    </changed> 
    <deprecated>
      <item>for soon-to-be removed features</item>
    </deprecated>   
    <removed>
      <item>for now removed features</item>
    </removed>
    <fixed>
      <item>for any bug fixes</item>
    </fixed> 
    <security>
      <item>in case of vulnerabilities</item>
    </security>   
  </changelog>
</changelogs>
```
##### 3. Commit директории Build
SVN автоматически установит значения номера сборки атрибута *«version»* и значение атрибута *«date»* для текущей ветки в «Build\changelog.xml». Таким образом версия дистрибутива привяжется к номеру ревизии в репозитории.

### Запуск сборки
1. Checkout\Update директории **«Build»**
2. Запустить скрипт **main.py** (возможно с параметрами, см. ниже)
3. Дождаться сообщения об успешной сборке: **«Ok, operation is complete, see \<out\> directory»**

Поддерживаются следующие параметры (все необязательные):
- cpu - разряность, возможные значения: (x32, x64), по умолчанию: определяется разрядность ос машины
- sys - целевая ос, возможные значения: (windows, ubuntu), по умолчанию: определяется ос машины
- typ - класс приложения, возможные значения: (vs, as, sat), по умолчанию: vs
- own - бренд, возможные значения: (colitec, verbitsky), по умолчанию: colitec
- rev - номер ревизии в репозитории (trunk), по данным этой ревизии будет собиратся дистрибутив, возможные значения: (90: HEAD], по умолчанию: HEAD
- out - выходная директория, где будет сохранен готовый дистрибутив, по умолчанию: завист от sys, "с:\colitec-lastbuild\out" для windows, "/~/colitec-lastbuild/out" для ubuntu

windows-пример:
```
python main.py --cpu=x64 --typ=sat --out=c:\temp\res
```

ubuntu-пример:
```
python3 main.py --cpu=x64 --typ=sat --out=/home/vmcolitec/Documents/res
```

Допускается запускать сразу несколько команд:
```
python main.py --cpu=x32 --typ=sat --own=colitec --out=c:\temp\res
python main.py --cpu=x64 --typ=sat --own=colitec --out=c:\temp\res
python main.py --cpu=x32 --typ=sat --own=verbitsky --out=c:\temp\res
python main.py --cpu=x64 --typ=sat --own=verbitsky --out=c:\temp\res
```
