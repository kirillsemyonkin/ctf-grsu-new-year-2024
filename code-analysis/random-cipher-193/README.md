# Random cipher

> К вам попала процедура для шифрования, использующая случайные числа, и зашифрованный с ее помощью текст.
>
> Восстановите исходное сообщение и найдите в нем флаг

---

> You have received an encryption procedure that uses random numbers and text encrypted with it.
>
> Restore the original message and find the flag in it

## [Исходный код / Source code](code_terror.py)

```python
from random import randint

def encrypt(text):
    key = randint(1, 2 * len(text))
    print (ord(text[0]), key)
    result = []

    for c in text:
        result.append(ord(c) + (ord(c) % key))
        key = key + 1

    return result
```

## [Вывод / Output](terror.txt)

```plain
200 202 204 64 202 220 198 228 242 224 232 80 232 202 240 232 82 116 64 70 64 204 216 194 206 64 210 230 116 64 206 228 222 200 220 222 246 240 194 90 240 194 90 240 194 90 232 202 228 228 222 228 210 230 232 250 20 64 64 64 64 214 202 242 64 122 64 228 194 220 200 210 220 232 80 98 88 64 100 64 84 64 216 202 220 80 232 202 240 232 82 82 20 64 64 64 64 228 202 230 234 216 232 64 122 64 182 186 20 64 64 64 64 204 222 228 64 198 64 210 220 64 232 202 240 232 116 20 64 64 64 64 64 64 64 64 228 202 230 234 216 232 92 194 224 224 202 220 200 80 222 228 200 80 198 82 64 86 64 80 222 228 200 80 198 82 64 74 64 214 202 242 82 82 20 64 64 64 64 64 64 64 64 214 202 242 64 122 64 214 202 242 64 86 64 98 20 64 64 64 64 228 202 232 234 228 220 64 228 202 230 234 216 232
```

## Решение / Solution

`key` постоянно растет, и после какого-то символа, `key` станет больше 127, после чего `ord` не
будет никак ограничен. Тогда формула `ord(c) + (ord(c) % key)` выродится в `ord(c) + ord(c)`, и
чтобы получить `c`, достаточно поделить каждое число на 2 и преобразовать в символ через
`chr(int(x) // 2)` (Спойлер: `key` изначально больше любого `ord(c)`). Вывод:

```plain
def encrypt(text): # flag is: grodno{xa-xa-xa-terrorist}
    key = randint(1, 2 * len(text))
    result = []
    for c in text:
        result.append(ord(c) + (ord(c) % key))
        key = key + 1
    return result
```

Флаг:

```plain
grodno{xa-xa-xa-terrorist}
```

---

`key` is always rising, and after some symbol `key` will become larger than `127`, after which `ord`
will not be bound anymore. Then the formula `ord(c) + (ord(c) % key)` becomes `ord(c) + ord(c)`, and
to get `c` you need to divide each number by 2 and convert it to a symbol using `chr(int(x) // 2)`
(Spoiler: `key` initially is bigger than any `ord(c)`). Output:

```plain
def encrypt(text): # flag is: grodno{xa-xa-xa-terrorist}
    key = randint(1, 2 * len(text))
    result = []
    for c in text:
        result.append(ord(c) + (ord(c) % key))
        key = key + 1
    return result
```

Flag:

```plain
grodno{xa-xa-xa-terrorist}
```
