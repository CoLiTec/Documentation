# Инструкция для сборки дистрибутива CoLiTec

### Интеграционная машина

##### Windows:
- ОС Windows 10
- клиент [SVN](https://tortoisesvn.net) с поддержкой командной строки
- система создания инсталляторов для Windows [Inno Setup](http://www.jrsoftware.org/isdl.php#stable)
- Environment Variables: добавить в переменную Path пути к SVN клиент и Inno Setup
- авторизоваться через установленный клиент SVN в репозитории CoLiTec

### Подготовка к сборке

##### 1. Checkout\Update директории «Build»

##### 2. Добавить описание изменений для новой версии в «Build\Changelog.xml»:
- В текущую (первую) ветку \<changelog\> добавить атрибуты:<br/>
*«version="%major%.%minor%.%release%.%build%"»*<br/>
*«date="%YYYY-MM-DD HH:MM:SSZ"»*<br/>
Значения атрибутов взять из «Build\Version.xml»
- Добавить в начало дерева новую ветку \<changelog\> (без атрибутов)
- В новую ветку \<changelog\> добавить группирирующие ветки \<added\>, \<changed\> и др. Описание этих веток см. на сайте [keepachangelog.com](https://keepachangelog.com))
- Для каждой группирирующей ветки добавить \<item\>. Возможно в \<item\> добавить атрибут *«class="as,sat,vs"»* – это  определит класс(ы) приложения к которому(-ым) относится данный \<item\>, если атрибут не указан, то данный \<item\> относится ко всем классам.

Пример «Build\Changelog.xml»:
```sh
<?xml version="1.0" encoding="utf-8"?>
<changelogs>
  <changelog>
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
##### 2. Установить номер версии в «Build\Version.xml» - новые значения \<major\>, \<minor\> и \<release\>

##### 4. Commit директории Build
> SVN автоматически установит значения узла \<build\> и атрибута *«Date»* в «Build\Version.xml». Таким образом версия дистрибутива привяжется к номеру ревизии в репозитории.

### Запуск сборки

##### Windows:
1. Checkout\Update директории «Build» в корень диска «С:\»
2. Выполнить команду в командной строке:
```sh
PowerShell -ExecutionPolicy Bypass -File "C:\Build\Windows\setup.ps1"
```

> На диске «C:\» будет создана директория «C:\colitec-lastbuild». В нее будет произведен экспорт модулей из репозитория, заданной в «Build\Version.xml» ревизии. Потом будут проведены корректирующие работы и собственно упаковка в инсталлятор отдельного набора модулей для каждого класса. После будет выведено сообщение об успешной сборке «Ok, operation is complete».

3. Все! Готовые инсталляционные пакеты CoLiTec будут находится в директории **«С:\colitec-lastbuild\Out»**
