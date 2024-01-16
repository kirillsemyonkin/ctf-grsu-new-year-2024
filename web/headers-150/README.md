# Headers

> Вам сюда *(ссылка на страницу)*

---

> Come here *(links to the webpage)*

## Решение / Solution

Пустая страница. Подсказка в названии: нужно посмотреть заголовки HTTP ответа.

```http
HTTP/1.1 200 OK
Server: nginx
Date: Mon, 15 Jan 2024 08:10:19 GMT
Content-Type: text/html; charset=UTF-8
Content-Length: 180
Connection: keep-alive
Access-Control-Allow-Origin: *
Authorization: Bearer eyJhbGciOiJIUzUxMiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiJncm9kbm97MWZRRFE1c3dhaEdjVGNmMn0iLCJuYW1lIjoiSm9obiBEb2UiLCJpYXQiOjE1MTYyMzkwMjJ9.Prqxaq3rMHc1eOs7-n67mvW3h0Zmug3q417Rac3roqsQ6GA_cMsJDUEWybB4ixROTEsd3bLERJWQArD8CIuFpg
Vary: Accept-Encoding
Content-Encoding: gzip
Access-Control-Allow-Methods: GET, POST
X-Content-Type-Options: nosniff
X-XSS-Protection: 1; mode=block
```

В заголовках ответа находится `Authorization`. Данный заголовок позволяет клиенту передать серверу
свои реквизиты для авторизации. Отправка данного заголовка сервером не имеет смысла.

Так как это Basic Auth с токеном в виде `ey...J9.ey...J9....`, скорее всего это JWT. Проверим данный
токен с помощью любого сервиса, например на [JWT.IO](https://jwt.io/).

```json
{
  "sub": "grodno{1fQDQ5swahGcTcf2}",
  "name": "John Doe",
  "iat": 1516239022
}
```

Флаг:

```plain
grodno{1fQDQ5swahGcTcf2}
```

---

Empty page. Hint is in the name: you need to check the HTTP response headers.

```http
HTTP/1.1 200 OK
Server: nginx
Date: Mon, 15 Jan 2024 08:10:19 GMT
Content-Type: text/html; charset=UTF-8
Content-Length: 180
Connection: keep-alive
Access-Control-Allow-Origin: *
Authorization: Bearer eyJhbGciOiJIUzUxMiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiJncm9kbm97MWZRRFE1c3dhaEdjVGNmMn0iLCJuYW1lIjoiSm9obiBEb2UiLCJpYXQiOjE1MTYyMzkwMjJ9.Prqxaq3rMHc1eOs7-n67mvW3h0Zmug3q417Rac3roqsQ6GA_cMsJDUEWybB4ixROTEsd3bLERJWQArD8CIuFpg
Vary: Accept-Encoding
Content-Encoding: gzip
Access-Control-Allow-Methods: GET, POST
X-Content-Type-Options: nosniff
X-XSS-Protection: 1; mode=block
```

In the headers there is `Authorization`. This header allows the client to pass to the server the
credentials for authorization. Having this header sent by a server does not make sense.

Since this is a Basic Auth with a token that looks like `ey...J9.ey...J9....`, this is probably JWT.
Let's check the token with any service, for example via [JWT.IO](https://jwt.io/).

```json
{
  "sub": "grodno{1fQDQ5swahGcTcf2}",
  "name": "John Doe",
  "iat": 1516239022
}
```

Flag:

```plain
grodno{1fQDQ5swahGcTcf2}
```
