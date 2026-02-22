Для того, чтобы скомпилировать доработанную сборку OpenSCAP 1.3.7 под Astra 1.8 необходимо:

# 1. Подготовка к сборке проекта

1. Установить базовые инструменты для сборки:
```bash
sudo apt install -y build-essential cmake git python3-pip pkg-config
```

2. # Системные зависимости OpenSCAP (согласно CMakeLists.txt) 
   libapt-pkg-dev нужен для dpkginfo probe 
   librpm-dev нужен для rpminfo probe 
   ```bash
   apt install -y \
    libacl1-dev \
    libblkid-dev \
    libcap-dev \
    libselinux1-dev \
    libgcrypt20-dev \
    libgpg-error-dev \
    libpopt-dev \
    libdbus-1-dev \
    libbz2-dev \
    libapt-pkg-dev \
    libyaml-dev \
    librpm-dev \
    libxml2-utils
   ```

3. Судя по CMakeLists необходимо установить также и Conan 1.x
	Ставим любой имеющий до версии 2.x (т.к .там уже принципиальные отличия)
	```bash
	pip3 install "conan<2.0"
	```

если установка не идет, добавить опцию **--break-system-packages**

После установки убеждаемся, что встал Conan 1.x
```bash
conan --version
```
![[Pasted image 20260222201549.png]]

4. В файле CMakeLists на строке 53 изменить набор требуемых пакетов (т.к. Conan 1 больше не поддерживается и часть пакетов не скачивается при компиляции)
```c++
set(CONAN_PACKAGES_LIST boost/1.86.0
                        libxml2/2.13.4
                        openssl/3.3.2
                        libiconv/1.17
                        libxslt/1.1.42
                        zlib/1.3.1
                        pcre/8.45
                        libssh/0.10.6
                        libcurl/8.10.1
)
```


5. На строке 66 удалить строку:
```c++
libssh:disable_export_symbols=True
```

т.к. в используемой версии пакета данного параметра нет

6. Также в одном из файлов забыли подключить стандартную библиотеку `limits.h`, где определены константы `INT_MIN` и `INT_MAX`. Раньше это работало потому что этот файл подключался неявно через другие файлы, но теперь это нужно делать явно.

Нужно вручную исправить файл исходного кода.

6.1.  Открой файл в редакторе:

    ```bash
    nano ../src/OVAL/results/oval_cmp_evr_string.c
    ```

6.2.. Добавь недостающую строку:
    Найди в самом начале файла блок, где идут строки `#include`.
    Добавь туда строку:
    `#include <limits.h>`


# 2. Сборка проекта