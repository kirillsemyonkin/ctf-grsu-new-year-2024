# Traffic Check

> К нам попал дамп трафика, в котором кто-то передавал пароль в незащищеном виде.
>
> Флаг в формате grodno{password}

---

> We received a traffic dump in which someone transmitted an unprotected password.
>
> Flag in the format grodno{password}

## Решение / Solution

Прогоняя `strings | grep grodno` получаем вывод:

```plain
  username=easy_login&password=grodno%7Be@sy_p@ckages_ch3ck%7D
```

Флаг:

```plain
grodno{e@sy_p@ckages_ch3ck}
```

---

Passing through `strings | grep grodno` we get:

```plain
  username=easy_login&password=grodno%7Be@sy_p@ckages_ch3ck%7D
```

Flag:

```plain
grodno{e@sy_p@ckages_ch3ck}
```
