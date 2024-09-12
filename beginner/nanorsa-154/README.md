# nanoRSA

> Где бы взять наноКомпьютер ...

---

> Where can I get a nanocomputer...

## [Вывод / Output](rsa.txt)

```ini
e = 1
c = 9908255308151638808626355523286556242109836830117153917
n = 245841236512478852752909734912575581815967630033049838269083
```

## Решение / Solution

Для RSA, где `e = 1` по формуле `c = m ^ e % n` ничего не происходит для сообщений `m` меньше `n`.

```python
from Crypto.Util.number import long_to_bytes
print(long_to_bytes(c))
```

Флаг:

```plain
grodno{R3sTcD4gH6iJ0kL}
```

---

For RSA where `e = 1` the formula `c = m ^ e % n` does nothing for messages `m` smaller than `n`.

```python
from Crypto.Util.number import long_to_bytes
print(long_to_bytes(c))
```

Flag:

```plain
grodno{R3sTcD4gH6iJ0kL}
```
