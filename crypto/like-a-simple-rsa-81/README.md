# Like a simple RSA

> Посмотрел код алгоритма шифрования.
> Только вот как-то сложно вычисляются ключи ...

---

> I looked at the encryption algorithm code.
> Only it’s somehow difficult to calculate the keys...

## [Исходный код / Source code](Asim_en.py)

```python
from random import randint

def gen_key():

    a, b, c, d = randint(3 * 64), randint(4 * 64), randint(5 * 64), randint(6 * 64)
    # this is not valid usage, randint takes in two args

    e = a * b - 1
    f = c * e + a + e
    g = d * e + b * f
    h = c * d * e + a * d + c * b + g * g

    public_key = (h, f)
    private_key = g
    key = (public_key, private_key)

    return key

def encrypt(m, public_key):
    c = (m * public_key[1]) % public_key[0]
    return c

def decrypt(c, public_key, private_key):
    m = (c * private_key) % public_key[0]
    return m
```

## [Вывод / Output](Asim_en_data.txt)

```plain
h = 8470387347298476177456807592234800305223146039241805029722152502623609066137655800632000167660507958129073278126249206662322559690599099900670113252751054479281818630329178438136876189437058031446326100810231155131782952040600724

f = 6416887830534433629567050229667684734973282397714251811755752025377169146409477365161229363756766441347368380751372023179641944107478195964183801916988232697767349642657617

C = 4501577816736015596060497850546201260925821617971707200151522683849342994222685636584343269899367495933875369681542486925080096048706520947165012158736670485935607004216130611982263313862618848808033812929735326651289123228929207
```

## Решение / Solution

Даже не будем смотреть на `gen_key`. В `encrypt` и `decrypt` не RSA. Если `C = M * f % h`, то
`M = C * (f^-1 % h) % h` - обратное к умножению под модулем. Вычислим `private_key` через
`pow(f, -1, h)`, а затем расшифруем через `from Crypto.Util.number import long_to_bytes`:

```plain
f^-1 % h = 12468907769155212120179272650536242151875558295870799902367582536335979967851456570308516414155060180800505780545740290603511864854971854174764624739870829280183561704135093
M = 17561148557246144611131320117893685669771198740375801967598601606256504710980914341947431958923493470823
  = b'}syek_owt_sesu_noitpyrcne_cirtemmysA{ondorg'
```

Флаг:

```plain
grodno{Asymmetric_encryption_uses_two_keys}
```

---

We won't even look at `gen_key`. `encrypt` and `decrypt` are not RSA. If `C = M * f % h`, then
`M = C * (f^-1 % h) % h` - inverse of multiplication under modulo. We can calculate `private_key`
via `pow(f, -1, h)`, and then decrypt it with `from Crypto.Util.number import long_to_bytes`:

```plain
f^-1 % h = 12468907769155212120179272650536242151875558295870799902367582536335979967851456570308516414155060180800505780545740290603511864854971854174764624739870829280183561704135093
M = 17561148557246144611131320117893685669771198740375801967598601606256504710980914341947431958923493470823
  = b'}syek_owt_sesu_noitpyrcne_cirtemmysA{ondorg'
```

Flag:

```plain
grodno{Asymmetric_encryption_uses_two_keys}
```
