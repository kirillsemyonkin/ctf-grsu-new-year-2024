# Polynomial hash

> Вам дана функция, которая вычисляет одну из разновидностей полиномиального хэша.
>
> С ее помощью для некторой строки получен хэш 35201194166317999524907401661096042001277
>
> Найдите эту строку. В ней содержится флаг

---

> You are given a function that calculates one of the varieties of a polynomial hash.
>
> With its help, the hash 35201194166317999524907401661096042001277 was obtained for a certain string
>
> Find this line. It contains a flag

## [Исходный код / Source code](PolynomialHash.py)

```python
def PolynomialHash(s, a):
    return sum([ord(s[i]) * pow(a, len(s)-i-1) for i in range(len(s))])
 
flag = "***********"
PolynomialHash(flag, 256)

#35201194166317999524907401661096042001277
```

## Решение / Solution

Любое число `...N2N1N0` в основании `B` выглядит как `... + B^2 * N2 + B^1 * N1 + B^0 * N0`. Это
работает только при условии что `N0, N1, N2 < B`, иначе они будут пересекаться.

Наше удобство степени 256 то, что это означает байт. То есть, по сути, достаточно просто
преобразовать число в строку (`from Crypto.Util.number import long_to_bytes`).

Флаг:

```plain
grodno{256NumSys}
```

---

Any number `...N2N1N0` base `B` looks like `... + B^2 * N2 + B^1 * N1 + B^0 * N0`. This works only
if `N0, N1, N2 < B`, otherwise they will clash.

Our convenience of the power of 256 is that it means a byte. So it looks like it is enough to just
convert the number to a string (`from Crypto.Util.number import long_to_bytes`).

Flag:

```plain
grodno{256NumSys}
```
