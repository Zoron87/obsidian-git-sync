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
  sudo apt update
  sudo apt install -y build-essential cmake git pkg-config \
    libacl1-dev libblkid-dev libcap-dev libselinux1-dev \
    libgcrypt20-dev libgpg-error-dev libpopt-dev libdbus-1-dev \
    libbz2-dev libapt-pkg-dev libyaml-dev librpm-dev libxml2-utils \
    python3-pip python3-venv
   ```
P.S.  *.-dev пакеты возможны и не нужны, т.к. Conan скачает свои нужные пакеты

3. Судя по CMakeLists необходимо установить также и менеджер пакетов - Conan 1.x
	Ставим любой имеющий до версии 2.x (т.к .там уже принципиальные отличия)
	```bash
	pip3 install "conan<2.0"
	```

если установка не идет и сборка идет на тестовом стенде, то можно  добавить опцию **--break-system-packages**

или сделать более безопасно в изолированном окружении:
```bash
# Создаем изолированное окружение в папке .venv
python3 -m venv .venv

# Активируем его (python будет выполняться внутри папки)
# Выполнять команду каждый раз при открытии нового терминала
source .venv/bin/activate

# Устанавливаем Conan 
pip install "conan<2.0"
```

После установки убеждаемся, что встал Conan 1.x
```bash
conan --version
```
![[Pasted image 20260222210740.png]]

и настроим профиль Conan 
 ```bash 
conan profile new default --detect 
conan profile update settings.compiler.libcxx=libstdc++11 default 
 ```

4. В файле CMakeLists на строке 53 изменить набор требуемых пакетов (т.к. Conan 1 больше не поддерживается и часть пакетов не скачивается при компиляции)
```c++
set(CONAN_PACKAGES_LIST boost/1.86.0
                        libxml2/2.13.4
                        openssl/3.3.2
                        libiconv/1.17
                        libxslt/1.1.42
                        zlib/1.3.1
                        pcre/8.45
                        xmlsec/1.3.4
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
Должно получиться примерно так:
![[Pasted image 20260222211116.png]]

7. Теперь нужно инициализировать yaml-filter (если в архиве он является файлом или пустой директорией), то
```bash
# Удаляем пустой файл и качем сборку с гита
rm -rf yaml-filter
git clone --depth=1 https://github.com/OpenSCAP/yaml-filter.git yaml-filter
```

# 2. Сборка проекта

	1. Запуск CMake 
	   Отключаем тесты, документацию и возможность запуск Perl и Python
	```bash
	cmake .. \
    -DCMAKE_BUILD_TYPE=Release \
    -DENABLE_TESTS=OFF \
    -DENABLE_DOCS=OFF \
    -DENABLE_PYTHON3=OFF \
    -DENABLE_PERL=OFF
	```

При необходимости можно добавить параметр
```bash 
-DENABLE_OSCAP_UTIL_DOCKER=OFF 
``` 
-отключение утилиты oscap-docker сканирования образов и докер и контейнеров напрямую.

2. Компиляция `
	```bash
	# -j$(nproc) использует все ядра процессора 
	make -j$(nproc)
	``` 
Данный процесс  длительный и может занять от 10 до 40+ минут


#3. Готовим проект к переносу

```bash
rm -rf /tmp/openscap-root  
mkdir -p /tmp/openscap-root  
DESTDIR=/tmp/openscap-root cmake --install .
```

и далее архивируем и переносим на нужный хост