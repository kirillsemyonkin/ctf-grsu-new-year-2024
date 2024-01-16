# Negative hash

> Вам дана еще одна функция, которая вычисляет одну из разновидностей полиномиального хэша.
>
> С ее помощью для некторой строки получен хэш 101871001089022554852470576840818198625
>
> Найдите эту строку. В ней содержится флаг

---

> You are given another function that calculates one of the varieties of polynomial hash.
>
> With its help, the hash 101871001089022554852470576840818198625 was obtained for a certain string
>
> Find this line. It contains a flag

## [Исходный код / Source code](-PolynomialHash.py)

```python
def PolynomialHash(string, a):
    result = 0
    l = len(string)
    for i in range(l):
        result += ord(string[i]) * ((-a) ** (l - i - 1))
    return result
 
flag = "****************"
PolynomialHash(flag, 100)
```

## Решение / Solution

Любое число `...N2N1N0` в основании `B` выглядит как `... + B^2 * N2 + B^1 * N1 + B^0 * N0`. Это
работает только при условии что `N0, N1, N2 < B`, иначе они будут пересекаться.

...Но именно это они тут и делают. `ord` выдает числа от 0 до 127. Так еще и отрицательная каждая
вторая степень. Хорошо лишь то, что 100 нормально работает с десятичными числами, поэтому можно
решать смотря на число.

Первым делом нужно определить сколько будет символов. Предположительно, какое то большое
положительное число (например `-10^20`), а за ним должна следовать отрицательная степень (`-10^19`).
Как раз можно предположить, что степень 20. Если это не так, потом исправим.

Начнем с правого конца. Если мы правильно выбрали 20 степень, то в конце под 0 степенью будет
положительное число. Возьмем последние две цифры (так как остаток от деления на 100) - `25`. Так как
такого символа не может быть (наши пределы от 32 до 127), добавим 100 (не меняет остаток от
деления на 100). Это `}`, значит мы движемся в правильно направлении.

Дальше пойдет некоторое ухищрение. Трудно разбираться, добавлять ли `1`, вычитать ли `-1`, может нет
переноса, или же там может перенос еще большого значения. Сделаем массив переносов (`[0, 0, ...]`),
и ручками подберем каждое из них так, чтобы в выводе после прогона наших стараний под этим же кодом
получился тот же самый вывод.

Будем проводить цикл от последнего к первому. Разделим исходное число по 2 цифры начиная с конца
(`1 01 87 10 01 08 90 22 55 48 52 47 05 76 84 08 18 19 86 25`). Добавим перенос из массива. Если
число в отрицательной степени, должно было произойти вычитание - например, `86` не показывает число,
которое вычиталось, а что получилось после вычитания, и чтобы вернуться обратно нужно вычесть это
число из `100`, получив `100 - 86 = 14`. После этого, если число не в пределах от 32 до 127, нужно
добавить `100`, чтобы получился символ.

```python
numbers = list(map(lambda x: int(x), "1 01 87 10 01 08 90 22 55 48 52 47 05 76 84 08 18 19 86 25".split(' ')))
carry = [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
for i in range(0, 19):
    base = numbers[20 - 1 - i]

    # first add carry
    base += carry[20 - 1 - i]

    # odd indices are negative
    if i % 2 == 1:
        base = 100 - base

    # make sure output character is in range
    if base < 28:
        base += 100

    numbers[20 - 1 - i] = base

flag = ''.join(map(lambda x: chr(x), numbers))
s = str(PolynomialHash(flag, 100))
s = ' ' + '0' * (40 - 1 - len(s)) + s
print(' 1 01 87 10 01 08 90 22 55 48 52 47 05 76 84 08 18 19 86 25')
print(' '.join(s[i:i+2] for i in range(0, len(s), 2)))
print(flag)
```

Работа будет выглядеть следующим образом - сейчас вывод такой:

```plain
 1 01 87 10 01 08 90 22 55 48 52 47 05 76 84 08 18 19 86 25
 0 0- 11 90 97 93 08 78 44 52 47 53 94 25 14 92 80 82 12 75
eqnclnz-00/_LtlRwr}
```

Такой вывод пока не понятный, нужно сделать его достаточно большим, чтобы было неотрицательное
число. Поменяем `carry[1]` на `1`.

```plain
 1 01 87 10 01 08 90 22 55 48 52 47 05 76 84 08 18 19 86 25
 0 00 88 09 02 06 91 21 55 47 52 46 05 74 85 07 19 17 87 25
fqnclnz-00/_LtlRwr}
```

Так, теперь очевидны значения `carry` которые нужно всего лишь подогнать. Подправляя справа-налево,
можно получить `carry = [0, 2, -1, 1, -1, 2, -1, 1, 0, 1, 0, 1, 0, 2, -1, 1, -1, 2, -1, 0]`.

Флаг:

```plain
grodno{-100_NumSys}
```

---

Any number `...N2N1N0` base `B` looks like `... + B^2 * N2 + B^1 * N1 + B^0 * N0`. This works only
if `N0, N1, N2 < B`, otherwise they will clash.

...But that is precisely what happens here. `ord` gives numbers from 0 to 127. And every other power
is negative. At the very least the 100 works alright with decimal numbers, so we can solve it by
looking at the number.

First of all, it would be useful to know how many characters there are. Assuming there is some big
number (like `-10^20`), and after that should come a negative power (`-10^19`). We could actually
assume the power to be 20. If that is not the case, we can fix it later.

Let's start from the right end. If we correctly picked the power of 20, then the last number under
the 0th power will be a positive number. Let's take our 2 numbers (because of the modulo 100) -
`25`. Since this symbol cannot be ASCII (thats 32 through 127), let's add 100 (which does not change
the remainder under modulo 100). This is `}`, so we are moving in the right direction.

Next there will be some tricks. It is hard to figure out whether to add `1`, `-1`, maybe there is no
carry-over, maybe the carry is larger in magnitude. Let's make an array of carries (`[0, 0, ...]`)
and using bare hands pick each one so that output of our hard work will under the task's code match
the given number.

We will do a loop from the last number to the first one. Let's divide the given number into chunks
of 2 digits starting from the end (`1 01 87 10 01 08 90 22 55 48 52 47 05 76 84 08 18 19 86 25`).
Then we add the carry. If the number is in the negative power, it signals a subtraction - for
example, `86` does not show the number that was subtracted, but the result, and to go back to the
wanted number we should subtract that number from a `100`, which turns into `100 - 86 = 14`. After
that, if the number is not in the range of 32 to 127, we need to add `100` to turn it into ASCII.

```python
numbers = list(map(lambda x: int(x), "1 01 87 10 01 08 90 22 55 48 52 47 05 76 84 08 18 19 86 25".split(' ')))
carry = [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
for i in range(0, 19):
    base = numbers[20 - 1 - i]

    # first add carry
    base += carry[20 - 1 - i]

    # odd indices are negative
    if i % 2 == 1:
        base = 100 - base

    # make sure output character is in range
    if base < 28:
        base += 100

    numbers[20 - 1 - i] = base

flag = ''.join(map(lambda x: chr(x), numbers))
s = str(PolynomialHash(flag, 100))
s = ' ' + '0' * (40 - 1 - len(s)) + s
print(' 1 01 87 10 01 08 90 22 55 48 52 47 05 76 84 08 18 19 86 25')
print(' '.join(s[i:i+2] for i in range(0, len(s), 2)))
print(flag)
```

The work will look like this - the current output is:

```plain
 1 01 87 10 01 08 90 22 55 48 52 47 05 76 84 08 18 19 86 25
 0 0- 11 90 97 93 08 78 44 52 47 53 94 25 14 92 80 82 12 75
eqnclnz-00/_LtlRwr}
```

This does not look understandable, let's fix it by making it large enough to not have a negative
number. Changing `carry[1]` to `1`:

```plain
 1 01 87 10 01 08 90 22 55 48 52 47 05 76 84 08 18 19 86 25
 0 00 88 09 02 06 91 21 55 47 52 46 05 74 85 07 19 17 87 25
fqnclnz-00/_LtlRwr}
```

Okay, now the necessary values of `carry` should be obvious. Adjusting from right to left, we get
`carry = [0, 2, -1, 1, -1, 2, -1, 1, 0, 1, 0, 1, 0, 2, -1, 1, -1, 2, -1, 0]`.

Flag:

```plain
grodno{-100_NumSys}
```
