# 39 Bob's friends

> У Боба почти 100 друзей и он послал им свой "привет", зашифровав его по этой схеме. Но он забыл про агента Хос Тада, который перехватил все письма

---

> Bob has almost 100 friends and he sent them his “hello” encrypted using this scheme. But he forgot about agent Jos Thad, who intercepted all the letters

## [Исходный код / Source code](39_letters.py)

```python
from Crypto.Util.number import getPrime, bytes_to_long

flag = b'grodno{fake_flag}'
m = bytes_to_long(flag)
e = 39
n = [getPrime(1024) * getPrime(1024) for i in range(e)]
c = [pow(m, e, n[i]) for i in range(e)]

open("all_letters.txt", 'w').write(f"e = {e}\nc = {c}\nn = {n}")
```

## [Вывод / Output](all_letters.txt)

```py
e = 39
c = [...]
n = [...]
```

## Решение / Solution

Так как число сообщений равно публичной экспоненте, задача может быть решена с помощью
[Hastad Attack](https://github.com/ashutosh1206/Crypton/blob/master/RSA-encryption/Attack-Hastad-Broadcast/README.md).

Можно запустить
[`hastad_unpadded.py`](https://github.com/ashutosh1206/Crypton/blob/master/RSA-encryption/Attack-Hastad-Broadcast/hastad_unpadded.py)
для `ct_list=c` и `mod_list=n`.

Флаг:

```plain
grodno{I_s3nd_y0u_gr33t1ngs,_long-no5ed_b4rb@rian!}
```

---

Since the number of messages is equal to the public exponent, the task can be solved with the
[Hastad Attack](https://github.com/ashutosh1206/Crypton/blob/master/RSA-encryption/Attack-Hastad-Broadcast/README.md).

[`hastad_unpadded.py`](https://github.com/ashutosh1206/Crypton/blob/master/RSA-encryption/Attack-Hastad-Broadcast/hastad_unpadded.py)
can be run under `ct_list=c` and `mod_list=n`.

Flag:

```plain
grodno{I_s3nd_y0u_gr33t1ngs,_long-no5ed_b4rb@rian!}
```
