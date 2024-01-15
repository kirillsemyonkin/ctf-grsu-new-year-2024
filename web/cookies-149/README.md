# Cookies

> Запаситесь терпением. Сайт очень голодный )
>
> Флаг ищем здесь *(ссылка на страницу)*

Явно что требуется забить запрос большим количеством cookie.

> [!WARNING]
> Забивание одной cookie не принимается сервером за ответ.
>
> Ограничение на размер заголовка `Cookie` - 4096 символов (дальше сервер отвечает кодом `400`).
>
> Cookie с одинаковым названием не различаются сервером.

Нужно сделать много (74) cookie, для этого можно повторять алфавит, будто это счет по основанию 26:

```http
Cookie: a=;b=;c=;d=;e=;f=;g=;h=;i=;j=;k=;l=;m=;n=;o=;p=;q=;r=;s=;t=;u=;v=;w=;x=;y=;z=;aa=;aa=;ab=;ac=;ad=;ae=;af=;ag=;ah=;ai=;aj=;ak=;al=;am=;an=;ao=;ap=;aq=;ar=;as=;at=;au=;av=;aw=;ax=;ay=;az=;ba=;ba=;bb=;bc=;bd=;be=;bf=;bg=;bh=;bi=;bj=;bk=;bl=;bm=;bn=;bo=;bp=;bq=;br=;bs=;bt=;bu=;bv=;bw=;bx=;by=;bz=;ca=;ca=;cb=;cc=;cd=;ce=;cf=;cg=;ch=;ci=;cj=;ck=;cl=;cm=;cn=;co=;cp=;cq=;cr=;cs=;ct=;cu=;cv=
```

Для удобства можно воспользоваться каким-либо внешним средством отправки запросов, например
[Postman](https://www.postman.com/).

```html
<html>
    <head>
        <title>CTF-task Grodno</title>
        <meta name="owner" content="grodno_CTF" />
        <meta name="publisher" content="grodno_CTF" />
        <meta name="copyright" content="grodno_CTF" />
        <meta name="robots" content="noindex,nofollow">
        <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
    </head>
    <body>
        Я наелся! Вот флаг:grodno{e2d5a0oW0uJWX1ZYxG9caZFX1Dcff874}
    </body>
</html>
```

Флаг:

```plain
grodno{e2d5a0oW0uJWX1ZYxG9caZFX1Dcff874}
```

---

> Be patient. The site is very hungry)
>
> Flag search here *(links to the page)*

It is clear that you need to fill a request with a large amount of cookies.

> [!WARNING]
> Filling request with one huge cookie is not accepted by the server.
>
> The limit on the size of the `Cookie` header is 4096 characters (after that the server responds
> with code `400`).
>
> Cookies with the same name are not distinguished by the server.

You need to make many (74) cookies, for that you can repeat alphabet as if it is base-26 counting:

```http
Cookie: a=;b=;c=;d=;e=;f=;g=;h=;i=;j=;k=;l=;m=;n=;o=;p=;q=;r=;s=;t=;u=;v=;w=;x=;y=;z=;aa=;aa=;ab=;ac=;ad=;ae=;af=;ag=;ah=;ai=;aj=;ak=;al=;am=;an=;ao=;ap=;aq=;ar=;as=;at=;au=;av=;aw=;ax=;ay=;az=;ba=;ba=;bb=;bc=;bd=;be=;bf=;bg=;bh=;bi=;bj=;bk=;bl=;bm=;bn=;bo=;bp=;bq=;br=;bs=;bt=;bu=;bv=;bw=;bx=;by=;bz=;ca=;ca=;cb=;cc=;cd=;ce=;cf=;cg=;ch=;ci=;cj=;ck=;cl=;cm=;cn=;co=;cp=;cq=;cr=;cs=;ct=;cu=;cv=
```

For convenience, you can use some external request sending tool, for example
[Postman](https://www.postman.com/).

```html
<html>
    <head>
        <title>CTF-task Grodno</title>
        <meta name="owner" content="grodno_CTF" />
        <meta name="publisher" content="grodno_CTF" />
        <meta name="copyright" content="grodno_CTF" />
        <meta name="robots" content="noindex,nofollow">
        <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
    </head>
    <body>
        Я наелся! Вот флаг:grodno{e2d5a0oW0uJWX1ZYxG9caZFX1Dcff874}
    </body>
</html>
```

Flag:

```plain
grodno{e2d5a0oW0uJWX1ZYxG9caZFX1Dcff874}
```
