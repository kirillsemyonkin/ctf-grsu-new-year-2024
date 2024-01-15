# AND-encryption

> XOR-шифрование изестно всем:
>
> ```plain
>     cipher = message XOR key
>     message = cipher XOR key
> ```
>
> А я заменил XOR на AND ...

---

> XOR encryption is known to everyone:
>
> ```plain
>     cipher = message XOR key
>     message = cipher XOR key
> ```
>
> And I replaced XOR with AND...

## [Исходный код / Source code](AND-encryption.py)

```python
from random import randint

def and_encr(key):
    mess = [randint(32, 127) for x in range(0,len(key))]
    return "".join([hex(ord(k) & m)[2:].zfill(2) for (k,m) in zip(key, mess)])
    
flag = 'fake-flag'

while True:
    while True:
        choise = input("1. Прислать шифр\n2. Проверить флаг\nВаш выбор: ")
        if choise in ['1','2']:
            break
    if choise == '1':
        print (and_encr(flag))
    else:
        if flag == input("Flag: "):
            print (f"Правильно !\nВаш флаг: {flag}") # source file has a missing `)` here
            break
        else:
            print (f"Ошибка, сэр !")
            break
```

## Вывод / Output

```plain
AND vs XOR
1. Прислать шифр
2. Проверить флаг
Ваш выбор: 1
6550274428424152303040400c4012706412404c40205054404800685c41420022062c3d
1. Прислать шифр
2. Проверить флаг
Ваш выбор:
```

## Решение / Solution

Суть XOR-шифрования заключается в том, что эта операция, под одним и тем же ключом, является легко
обратимой. Если же использовать AND-шифрование, происходит неминуемая потеря данных. Например, если
ключ является нулем, то ответ всегда равен нулю.

Но не вся надежда потеряна. Пусть есть некоторое бинарное сообщение и ключ. Результатом AND-операции
над ними будет AND каждого бита этих сообщений. Можно заметить, что в выводе будут `1` именно в тех
позициях, где были `1` как сообщении, так и в ключе. Из этого следует главная мысль решения: `1` ни
в коем случае не появится в тех позициях, где в ключе не было `1`. А если ключ один и тот же для
множества сообщений (что нам позволяет сделать сокет), то если объединить части единиц полученные
для разных сообщений, то спустя какое то время будут получены все единицы, которые есть в ключе.
Если повезет, и встретится сообщение состоящее из всех битов равных `1`, то ключ проявится сразу же.

Насчет скорости проявления же ключа - сколько сообщений нужно, чтобы получить ответ? В исходном коде
видно генерируются сообщение по 8 бит. Таблица вероятностей каждого бита числа `m` выглядит так:

```plain
2^0. 0.13019391
2^1. 0.13019391
2^2. 0.13019391
2^3. 0.13019391
2^4. 0.13019391
2^5. 0.17451524
2^6. 0.17451524
2^7. 0, т.к. m < 127. Скорее всего, это означает, что задача подразумевает что этот бит не будет
                      использоваться, что логично, так как ключ-флаг является ASCII текстом.
```

Получается, что нужно взять как минимум 8 сообщений, чтобы получить ответ. Как показала практика,
этого обычно достаточно. А это значит, что это можно спросить у сокета вручную.

Ниже приведен код, объединяющий единицы из 8 ответов и преобразующий получившийся ключ в текст.

```python
import numpy as np
arr = np.array([
0x63626c2060436b73543148285622017424312241480010497020204e5e414e442e082855,
0x402022446e2433206431642851202050642350444830504a6040304c0e41000422260e3d,
0x44324260246c52415430244c434002242023604b003052036468304c01404e442c222040,
0x66424f646e6d32317431682c1222233064302042080040564468304a404004402c28222d,
0x27206a6060662963243060245d602060343130191820520f542820685341480428002438,
0x26204a646a233b2124316840144021747423205f58200045542830264841404426062459,
0x243226404023794374112c2856401174702260431830524d7428246e4f414040282a0829,
0x64606d24222d4a6244006024504002244021704a50305249442810225901424422200251
])

from Crypto.Util.number import long_to_bytes
print(long_to_bytes(np.bitwise_or.reduce(arr)))
```

Флаг:

```plain
grodno{st1ll_b3tt3r_X0R_th4n_AND...}
```

---

The point of the XOR encryption, is that this operation, under the same key, is easily reversible.
If we use AND encryption, then inevitable loss of data occurs. For example, if the key is zero,
then the encoded message will always be zero.

But not all hope is lost. Assume there is some binary message and a key. The result of an
AND-operation will be AND of each bit of these messages. It is easy to notice, that output will
contain `1` exactly at those positions, where there have been `1` in both message and key. From this
follows the main idea of the solution: `1` will never appear in the positions where the key had no
`1`. If the key is the same for many messages (which socket allows us to get), then if we combine
the parts of ones that were obtained for different messages, after some time all ones of the key
will be obtained. If we are lucky, and we get a message that contains all bits equal to `1`, then
the key will be revealed instantly.

About the instant-ness of key revealing: how many message is required to get the answer? In the
source code it is visible that the message is generated in chunks of 8 bits. The table of
probabilities of each bit of the number `m` looks like:

```plain
2^0. 0.13019391
2^1. 0.13019391
2^2. 0.13019391
2^3. 0.13019391
2^4. 0.13019391
2^5. 0.17451524
2^6. 0.17451524
2^7. 0, since m < 127. This probably means that the task assumes that this bit will not be used,
                       which is logical, since the key (flag) is an ASCII text.
```

As it turns out, you need a minimum of 8 messages to get the answer. As practice has shown, this is
usually enough. And this means that we can grab those numbers from the socket manually.

Below is the code that can combine the ones from 8 messages and convert the resulting key to text:

```python
import numpy as np
arr = np.array([
0x63626c2060436b73543148285622017424312241480010497020204e5e414e442e082855,
0x402022446e2433206431642851202050642350444830504a6040304c0e41000422260e3d,
0x44324260246c52415430244c434002242023604b003052036468304c01404e442c222040,
0x66424f646e6d32317431682c1222233064302042080040564468304a404004402c28222d,
0x27206a6060662963243060245d602060343130191820520f542820685341480428002438,
0x26204a646a233b2124316840144021747423205f58200045542830264841404426062459,
0x243226404023794374112c2856401174702260431830524d7428246e4f414040282a0829,
0x64606d24222d4a6244006024504002244021704a50305249442810225901424422200251
])

from Crypto.Util.number import long_to_bytes
print(long_to_bytes(np.bitwise_or.reduce(arr)))
```

Flag:

```plain
grodno{st1ll_b3tt3r_X0R_th4n_AND...}
```
