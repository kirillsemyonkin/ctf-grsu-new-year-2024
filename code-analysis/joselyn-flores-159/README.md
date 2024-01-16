# Joselyn Flores

> I know you so well, so well
> I mean, I can do anything that he can
> I've been pretty—

## Решение / Solution

Файл с зашифрованным сообщением (`joselyn_flores`):

```plain
}r3,_oeL'_lNS1NIfNF____0pnGjl3eo1APbN_tilB_H10{IdlN33_L'4l_uoggT_D
```

Дан файл `jf.pyc` скомпилированного кода Python, который при
[декомпиляции](https://github.com/zrax/pycdc) выглядит следующим образом:

```python
# Source Generated with Decompyle++
# File: jf.pyc (Python 3.6)

import random
flag = input('>')
seed = random.randint(1, 1000)
random.seed(seed)
flag = list(flag)
random.shuffle(flag)
flag = ''.join(flag)
print(flag)
```

Соответственно, нужно перебрать все значения `seed`, и обратить его влияние чтобы получить исходный
текст. Для этого возьмем список чисел от `0` до длины сообщения и выполним `shuffle` для каждого
`seed`. В данном списке будут храниться места, где окажется какой символ, после чего можно обратить
его индексы и значения, и применить над зашифрованным сообщением.

```python
import random

file = open("jocelyn_flores").read()
data_length = len(file)

for seed in range(1, 1000):
    random.seed(seed)

    shuf_order = list(range(data_length))
    random.shuffle(shuf_order)

    unshuf_order = [0] * data_length
    for j, i in enumerate(shuf_order):
        unshuf_order[i] = j
    
    flag = list(file)
    flag = [flag[i] for i in unshuf_order]
    flag = ''.join(flag)

    if 'grodno' in flag:
        print(flag)
```

Флаг:

```plain
grodno{1'Ll_Be_fe3L1NG_PAIN}
```

---

File with the encoded message (`joselyn_flores`):

```plain
}r3,_oeL'_lNS1NIfNF____0pnGjl3eo1APbN_tilB_H10{IdlN33_L'4l_uoggT_D
```

A compiled Python file `jf.pyc` is given, which when [decompiled](https://github.com/zrax/pycdc)
looks like:

```python
# Source Generated with Decompyle++
# File: jf.pyc (Python 3.6)

import random
flag = input('>')
seed = random.randint(1, 1000)
random.seed(seed)
flag = list(flag)
random.shuffle(flag)
flag = ''.join(flag)
print(flag)
```

To obtain the original text, we need to iterate over all values of `seed`, and apply its effect to
obtain the original message. We can do this by taking a list of numbers from `0` to the length of
the message, and perform `shuffle` for each `seed`. In this list, an index of each character after
it has been shuffled is stored, and we can invert the indices and their values to get a mapping for
the encoded message.

```python
import random

file = open("jocelyn_flores").read()
data_length = len(file)

for seed in range(1, 1000):
    random.seed(seed)

    shuf_order = list(range(data_length))
    random.shuffle(shuf_order)

    unshuf_order = [0] * data_length
    for j, i in enumerate(shuf_order):
        unshuf_order[i] = j
    
    flag = list(file)
    flag = [flag[i] for i in unshuf_order]
    flag = ''.join(flag)

    if 'grodno' in flag:
        print(flag)
```

Flag:

```plain
grodno{1'Ll_Be_fe3L1NG_PAIN}
```
