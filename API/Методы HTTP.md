
HTTP поддерживает 9 методов запросов, каждый из которых выполняет определенную функцию:

- **GET** - запрос информации о ресурсе.
- **POST** - запрос на создание ресурса (например, регистрация пользователя).
- **PUT** - запрос на обновление ресурса (обновляет ресурс полностью).
- **PATCH** - запрос на обновление ресурса (частично обновляет ресурс).
- **DELETE** - запрос на удаление ресурса.
- **OPTIONS** - запрос информации о поддерживаемых методах у ресурса (в заголовке `Allow` представляются поддерживаемые методы).
- **HEAD** - запрос заголовков ресурса (аналогичен методу GET, но без получения тела ответа).
- **CONNECT** - преобразование соединения в прозрачный TCP/IP-туннель, например, для соединения с сайтом через SSL.
- **TRACE** - позволяет клиенту видеть, что происходит на каждом этапе между клиентом и сервером.