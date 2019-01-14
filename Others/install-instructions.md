# Инструкция по установке/удалению<br/>дистрибутива CoLiTec

### windows

##### Установка
Происходит обычным образом: нужно просто следовать указаниям мастера.<br/>
Перед установкой проходит проверка Java, если ее нет - установка прервется и будет выдано соотв. сообщение.

##### Удаление
Происходит обычным образом: через оснастку "удаление программ".

##### Вопросы, проблемы:
Путь не должен содержать символы unicode.<br/>
Желательно не менять путь установки по умолчанию.

### ubuntu

##### Установка
```
sudo apt install ./colitec_package.deb
```
В процессе установки будет запрос на установку Java (стандартный механизм разрешения зависимостей).<br/>
В конце установки будет запрос - нужно будет ввести имя пользователя системы.
Это имя нужно, чтобы установить права ```700``` на директорию с программой.<br/>
Собственно приложение будет установлено в **"/opt/colitec\<class\>"**

##### Удаление
```
sudo apt purge colitec_package_name
```

##### Вопросы, проблемы:
Узнать имя установленного пакета colitec
```
dpkg --list | grep colitec
```
В случае ручной установки
- предоставить права на чтение/запись для всей директории colitec:
```
chmod -R 700 path_to_colitec
```
- если версия Java ниже 8 (проверить - ```java -version```):
```
sudo add-apt-repository ppa:webupd8team/java
sudo apt-get update
sudo apt-get install oracle-java8-installer 
```
- если версия gcc ниже 4.8 (проверить - ```gсс -v```):
```
sudo add-apt-repository ppa:ubuntu-toolchain-r/test
sudo apt-get update
sudo apt-get install gcc-4.8
sudo update-alternatives --remove-all gcc
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-4.8 20
sudo update-alternatives --config gcc
```
- если версия g++ ниже 4.8 (проверить - ```g++ -v```):
```
sudo add-apt-repository ppa:ubuntu-toolchain-r/test
sudo apt-get update
sudo apt-get install g++-4.8
sudo update-alternatives --remove-all g++
sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-4.8 20
sudo update-alternatives --config g++
```
При установки x32 пакета в x64 систему (режим "сова на глобусе"):
```
sudo apt-get install libpam0g:i386
sudo apt-get update
sudo apt-get upgrade -y
sudo apt-get dist-upgrade
```
