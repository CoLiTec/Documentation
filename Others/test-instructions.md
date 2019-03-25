# Инструкция по тестированию дистрибутива CoLiTec

### Интеграционная машина

##### Windows:

- ОС Windows 10, x64
- интерпретатор [Python 3.x](https://www.python.org)
- клиент [SVN](https://tortoisesvn.net) с поддержкой командной строки
- авторизоваться через установленный клиент SVN в [репозитории CoLiTec](https://subversion.assembla.com/svn/colitecclosed.test/trunk)
- Environment Variables, добавить в переменную Path путь к SVN клиенту, по умолчанию: "C:\Program Files\TortoiseSVN\bin"

Для проверки в командной строке выполнить команды:
```posh
python --version
svn --version
iscc
```

##### Настроить Google Drive:

- авторизоваться в Google (посредством браузера) и убедиться в том, что есть доступ к данным для тестов на Google Drive
- включить Drive API и загрузить файл конфигурации клиента «credentials.json», см. ["Step 1: Turn on the Drive API"](https://developers.google.com/drive/api/v3/quickstart/python)
- установить Python Google Client Library, см. ["Step 2: Install the Google Client Library"](https://developers.google.com/drive/api/v3/quickstart/python)

Пользовательский файл «credentials.json» необходимо разместить в корне директории «test».

### Данные

Есть три класса данных:
- начальные - исходные ("сырые") данные, которые будут обработаны программой
- текущие - данные обработанные программой
- верифицированные - заранее подготовленные и проверенные данные

На Google Drive размещаются *начальные* и *верифицированные* данные в zip-архиве.

В корне zip-архива должны находится собственно файлы и директории, а не "общая директория".

Пример имен: %typ%-init-someseries.zip и %typ%-very-someseries.zip, где *typ* - класс приложения (vs, as, sat)

### Скрипты

Скрипты автоматизированного тестирования находятся в диретории «test» (репозиторий TEST):

- initial.py - инициализация (подготовка)
- process.py - обрабротка
- compare.py - анализ (сравнение)

Поддерживаются следующие параметры:

- scenario - короткое имя файла сценария, обязательный параметр
- modules - директория модулей сравнения, по умолчанию: завист от ОС, "%ROOT%/BinWindows64" для windows, "%ROOT%/BinUbuntu64" для ubuntu
- dataexec - директория текущих данных, по умолчанию: "%ROOT%/dataexec"
- datavery - директория верифицированных данных, по умолчанию: "%ROOT%/datavery"
- scripts - директория test, по умолчанию: "%ROOT%/scripts"
- reports - директория отчетов, по умолчанию: "%ROOT%/reports"
- force - флаг принудительного обновления существующих файлов и директорий, по умолчанию: false

Директория %ROOT% определяется ОС: "C:\colitec-lasttest" для windows, "~/colitec-lasttest" для ubuntu

```bash
> python initial.py --scenario=some-scenario.xml --force=true
> python compare.py --scenario=some-scenario.xml --force=true
```

### Сценарии

Файлы сценариев находятся в диретории «test/scenarios» (репозиторий TEST).
Сценарий содержит список задач (отдельных тестов), которые необходимо выполнить.

```xml
<?xml version="1.0" encoding="utf-8"?>
<main>
  <name>Some Scenario</name>
  <tasks>
    <task>task1.xml</task>
    <task>task2.xml</task>
    ...
  </tasks>
</main>
```
- \<name\> - имя сценария
- \<task\> - короткое имя файла задачи

### Задачи

Файлы задач находятся в диретории «test/tasks» (репозиторий TEST).
Задача описывает отдельный тест.

```xml
<?xml version="1.0" encoding="utf-8"?>
<main>
  <name>Some Task</name>
  <data>
    <exec name="some-task-init.zip" id="zzz">%dataexec%/some-task-exec</exec>
    <very name="some-task-very.zip" id="zzz">%datavery%/some-task-very</very>
  </data>>
  <comp>
    <module>%modules%/compModule%ext%</module>
    <report>%reports%/compSomeTask.xml</report>
    <param1>p1</param1>
    <param2>p2</param2>
    ...
  </comp>
</main>
```
- \<name\> - имя задачи

Блок \<data\> описывает данные, которые используются в данной задаче:

- \<exec\> - путь к текущим данным
- \<very\> - путь к верифицированным данным

Тут атрибуты *name* и *id* задают имя и идентификатор данных на Google Drive.

Блок \<comp\> описывает функцию сравнения в данной задаче:

- \<module\> - путь к модулю сравнения
- \<report\> - путь к файлу отчета в формате xml, в котором модуль сравнения может детализировать свой выход
- \<param1\>,\<param2\> - дополнительные параметры модуля сравнения (необязательно)

Переменне %dataexec%, %datavery%, %modules% %reports% и %ext% устанавливаются в скрипте.

### Функция сравнения

Сравнение текущих данных с верифицированными данными.
Модули сравнения находятся в диретории «BinWindows64» и «BinUbuntu64» (репозиторий TEST).

```bash
> compSomeTest "compSomeTask.xml"
```
- compSomeTest - модуль сравнения, приложение (скрипт) которое собственно производит сравнение данных
- compSomeTask.xml - файл задачи

compSomeTest возвращает код (число):

- 0 - все хорошо, сравнение прошло успешно;
- 1...1000 - ошибки системы и чтения входных данных;
- 1001... - выявлено несоответствие между текущими и верифицированными данными

### Подготовка

1. Checkout\Update директории «test»

2. Определить файл сценария

3. Выполнить скрипт «test/initial.py» и дождаться сообщения "initial operation is complete"

Скрипт «test/initial.py», согласно задачам, указанным в сценарии:
- создает необходимые директории
- скачивает и распаковывает данные из Google Drive
- экспортирует из репозитория модули сравнения для данной ОС

### Обработка

Обработка данных осуществляется текущей версией программы, в "ручном режиме".

### Сравнение

1. Checkout\Update директории «test»

2. Определить файл сценария

3. Выполнить скрипт «test/compare.py» и дождаться сообщения "compare operation is complete"

Скрипт «test/compare.py», согласно задачам, указанным в сценарии:
- локализует задачи (устанавливает переменные)
- запускает модули сравнения
