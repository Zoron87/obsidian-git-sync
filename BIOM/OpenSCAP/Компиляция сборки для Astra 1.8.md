Для того, чтобы скомпилировать доработанную сборку OpenSCAP 1.3.7 под Astra 1.8 необходимо:

1. В файле CMakeLists на строке 53 изменить набор требуемых пакетов (т.к. Conan 1 больше не поддерживается и часть пакетов не скачивается при компиляции)
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

2. На строке 66 удалить строку:
```c++
libssh:disable_export_symbols=True
```