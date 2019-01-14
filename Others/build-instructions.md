# Инструкция для сборки дистрибутива CoLiTec

### Интеграционная машина

##### Windows:
- ОС Windows 10
- интерпретатор [Python 3.x](https://www.python.org)
- клиент [SVN](https://tortoisesvn.net) с поддержкой командной строки
- система создания инсталляторов для Windows [Inno Setup](http://www.jrsoftware.org/isdl.php#stable)
- Environment Variables: добавить в переменную Path пути к SVN клиенту и Inno Setup
- авторизоваться через установленный клиент SVN в [репозитории CoLiTec] (https://subversion.assembla.com/svn/colitecclosed.clt/trunk)

Для проверки в командной строке выполнить команды:
```
python --version
svn --version
iscc
```

##### Ubuntu:
- ОС Ubuntu 16.04 или выше
- интерпретатор [Python 3.x](https://www.python.org)
- клиент [SVN](https://subversion.apache.org)
- авторизоваться через установленный клиент SVN в [репозитории CoLiTec] (https://subversion.assembla.com/svn/colitecclosed.clt/trunk)

Для проверки в терминале выполнить команды:
```
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

##### Windows:
НУЖНО ОБНОВИТЬ ИНФУ
1. Checkout\Update директории «Build» в корень диска «С:\»
2. Выполнить команду в командной строке:
```posh
PowerShell -ExecutionPolicy Bypass -File "C:\Build\Windows\setup.ps1"
```
На диске «C:\» будет создана директория «C:\colitec-lastbuild». В нее будет произведен экспорт модулей из репозитория, заданной в «Build\Version.xml» ревизии. Потом будут проведены корректирующие работы и собственно упаковка в инсталлятор отдельного набора модулей для каждого класса. После будет выведено сообщение об успешной сборке «Ok, operation is complete». Все! Готовые инсталляционные пакеты CoLiTec будут находится в директории **«С:\colitec-lastbuild\Out»**
