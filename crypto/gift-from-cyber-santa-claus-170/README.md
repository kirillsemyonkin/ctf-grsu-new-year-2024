# Gift from Cyber Santa Claus

> Пришел Кибер Дед Мороз. Принес мешок случайных чисел. И что мне с ними делать ???
>
> Решил сделать из них ключ шифрования в новом XOR-криптоалгортме

---

> Cyber Santa Claus has arrived. Brought a bag of random numbers. And what should I do with them???
>
> I decided to make an encryption key from them in the new XOR crypto algorithm

## [Исходный код / Source code](Gift_from_Santa.py)

```python
import random
from math import gcd

def _prime_factors(n):   # Returns a list that includes prime factors with their repetitions
    prime_factors = lambda n: [i for i in range(2, n+1) if n%i == 0 and all(i % j != 0 for j in range(2, int(i**0.5)+1))]

    factors = []
    while n > 1:
        for factor in prime_factors(n):
            factors.append(factor)
            n //= factor
    return factors

def lcm(a, b):  # Least common multiple
    return abs(a*b) // gcd(a, b)

def create_initial_seed(primes):
    res = 0
    for p1 in primes:
        for p2 in primes:
            if p1 <= p2:
                res = res ^ lcm(p1, p2) 
    return res

# Read random integer numbers from [1000, 100000]
numbers = map(int, list(open('From _bag_of_Santa.txt').read().split())) 

primes_set = set()
initial_value = []

# Create Initial seed
for n in numbers:
    primes = _prime_factors(n)
    for p in primes:
        primes_set.add(p)
    initial_value.append(create_initial_seed(primes))

random.shuffle(initial_value)
iv = initial_value[0] ** 3 # big enough to fail brute force on seed
random.seed(iv)

# Generate cipher with random key
flag = "grodno{fake_flag}"  # This is message

enc = [ord(flag[i]) ^ random.randint(0, 255) for i in range(len(flag))]

with open('output_for_Santa.txt', 'w') as file:
    print(primes_set, file=file)
    print(enc, file=file)
```

## [Input / Ввод](From__bag_of_Santa.txt)

```plain
79928 13451 10201 22547 41520 50933 1874 93918 66780 12301 24989 45468 34784 12309 114599 12936
```

## [Вывод / Output](output_for_Santa.txt)

```plain
{97, 2, 3, 5, 421, 7, 103, 13451, 11, 173, 12301, 1423, 3221, 53, 24989, 31}
[163, 42, 71, 224, 93, 26, 168, 23, 114, 249, 221, 83, 92, 137, 216, 70, 135, 193, 113, 119, 65, 93, 116, 60, 184, 231, 138, 234, 88, 234, 157, 245, 19, 89, 111, 36, 69, 155, 103, 6, 5, 125, 35, 241, 253, 245, 197, 6, 250, 8, 52, 250, 185, 81, 160, 149]
```

## Решение / Solution

Странный комментарий `big enough to fail brute force on seed`. Как будто меня дразнит. Давайте
попробуем перебрать `initial_value[0]`, который в кубе будет `iv` для проверки получилось ли
`grodno{` в начале сообщения или нет. Перебрать 2 млрд значений будет не так страшно. Но ограничимся
пока 100000.

```python
import random

output = [163, 42, 71, 224, 93, 26, 168, 23, 114, 249, 221, 83, 92, 137, 216, 70, 135, 193, 113, 119, 65, 93, 116, 60, 184, 231, 138, 234, 88, 234, 157, 245, 19, 89, 111, 36, 69, 155, 103, 6, 5, 125, 35, 241, 253, 245, 197, 6, 250, 8, 52, 250, 185, 81, 160, 149]

for i in range(1, 100000):
    iv = i ** 3
    random.seed(iv)

    enc = [output[i] ^ random.randint(0, 255) for i in range(len(output))]
    str = ''.join([chr(i) for i in enc])
    
    if str.startswith('gr'):
        print(i)
        print(str)
```

Ответ был найден при `i = 12301`.

Флаг:

```plain
grodno{XOR_w0rks_with_number5_the_s@me_way_@s_w1th_bits}
```

---

Weird comment `big enough to fail brute force on seed`. As if it is mocking me. Let's try
brute-forcing `initial_value[0]`, which when cubed be `iv` for checking if `grodno{` has appeared in
the beginning of the message. Brute-forcing 2 billion values is not that scary. But let's limit it
to 100000 for now.

```python
import random

output = [163, 42, 71, 224, 93, 26, 168, 23, 114, 249, 221, 83, 92, 137, 216, 70, 135, 193, 113, 119, 65, 93, 116, 60, 184, 231, 138, 234, 88, 234, 157, 245, 19, 89, 111, 36, 69, 155, 103, 6, 5, 125, 35, 241, 253, 245, 197, 6, 250, 8, 52, 250, 185, 81, 160, 149]

for i in range(1, 100000):
    iv = i ** 3
    random.seed(iv)

    enc = [output[i] ^ random.randint(0, 255) for i in range(len(output))]
    str = ''.join([chr(i) for i in enc])
    
    if str.startswith('gr'):
        print(i)
        print(str)
```

Answer was found at `i = 12301`.

Flag:

```plain
grodno{XOR_w0rks_with_number5_the_s@me_way_@s_w1th_bits}
```
