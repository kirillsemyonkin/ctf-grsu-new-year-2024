# Sir John and his notebook

> Сэр Джон Смокинг, сидя у камина в Рождественский вечер, листает старый секретный блокнот, в котором вроде бы защифрована история его знаменитого предка.
>
> Помогите сэру Джону открыть семейную тайну !

---

> Sir John Tuxedo, sitting by the fireplace on Christmas Eve, leafs through an old secret notebook in which the history of his famous ancestor seems to be encrypted.
>
> Help Sir John discover a family secret!

## [Вывод / Output](otp_message.txt)

Состоит из 66 Hex-сообщений (каждые две цифры при дешифровании станут символом читаемого текста).

---

Consists of 66 Hex-messages (every two digits after deciphering will become a readable character).

## Решение / Solution

OTP заключается в обычном XOR между сообщением и ключом. В каждом сообщении здесь один и тот же
ключ, который нужно найти.

OTP задачи [обычно решаются](https://ctftime.org/writeup/8857) через Crib Dragging - когда мы берем
известную часть ключа (crib) и двигаем по сообщению пока не получится что-либо внятное.

```python
strings = list(map(lambda x: bytes.fromhex(x).decode('utf-8'), open("otp_message.txt").read().split('\n')))

flag = 'grodno{'

def xor_str(a, b):
    max_len = max(len(a), len(b))
    a = (a * (max_len // len(a) + 1))[:max_len]
    b = (b * (max_len // len(b) + 1))[:max_len]
    return ''.join([chr(ord(x) ^ ord(y)) for x, y in zip(a, b)])

print('\n'.join([xor_str(strings[i], flag) for i in range(0, len(strings))]))
```

Как оказалось, при `flag = '  grodno{'` появляются отголоски некого английского текста (например,
`England`):

```plain
...
6)from wh60t;`rn3t%.`e,h>rx:!yje*<)im<Z342{s
n`ons werx=pvmsn rl-rx0-{rdd-.njmo&z?lk#F3/97f
rht count!1zfsn=hcv1l{*re`,w-lnlb<n`&Q`)97Y
hlutznaer^x1do$7,"Rdm&!b-t:rqn-%;}vih<1g"8Pa"$7{
t)EnglandIx$t;ie+{rcbv2m>r?!167-ueDe}2m" Ja52{d
z and wr,61t}en5}`c,7s.=w:6 i"s&q'!a Rc'9~}
...
```

Иногда некоторые строки очевидно имеют окончания от конкретных английских слов, иногда очевидно
продолжение английского слова справа от флага.

Одна из строк содержит `rhrdships`. Если `flag='hagrodno{`, то вывод превращается в `:)rdships`.
Наконец, при `flag=':)grodno{` текст превращается в искомое `hardships`. Так и дальше нужно
продолжать работу, но уже с правой стороны от флага.

Подправим немного код так, чтобы было проще подбирать английские слова. Ограничим вывод длиной
флага, и еще поместим один символ справа, чтобы знать что подставить в конец флага. Также для
удобства (чтобы постоянно не терять строки) перед каждой строкой добавим номер.

```python
strings = list(map(lambda x: bytes.fromhex(x).decode('utf-8'), open("otp_message.txt").read().split('\n')))

flag = ':)grodno{'

def xor_str(a, b):
    max_len = max(len(a), len(b))
    a = (a * (max_len // len(a) + 1))[:max_len]
    b = (b * (max_len // len(b) + 1))[:max_len]
    return ''.join([chr(ord(x) ^ ord(y)) for x, y in zip(a, b)])

print('\n'.join([str(i) + " " + xor_str(strings[i], flag)[:len(flag)] + " " + xor_str(strings[i], flag)[len(flag)] for i in range(0, len(strings))]))
```

```plain
0 I was bor 
1 of a good _
2 ather bei 
3 st at Hul 
4 nd leavin 
5 , from wh 
6 tions wer 
...
```

Например, видно не хватает в слове `bei...` части `ng`. Подставив в конец флага `ng`, `:)grodno{ng`,
получилось `beiEx`. Подставляем `Ex` в конец флага, `:)grodno{Ex`, получили `being`, а также все
остальные строки также продвинулись со своими английскими словами.

```plain
0 I was born  g
1 of a good f o
2 ather being .
3 st at Hull. .
4 nd leaving  a
5 , from when m
6 tions were  `
...
```

Флаг:

```plain
grodno{Ex4ctly! R0b1ns0n Crus03 by Dan1el D3f0e}
```

---

OTP is a simple XOR between a message and a key. In each message here there is exactly the same key,
which the task makes us find.

OTP tasks [usually are solved](https://ctftime.org/writeup/8857) via Crib Dragging - when we take a
known part of the flag and drag it along the message until something intelligible can be found.

```python
strings = list(map(lambda x: bytes.fromhex(x).decode('utf-8'), open("otp_message.txt").read().split('\n')))

flag = 'grodno{'

def xor_str(a, b):
    max_len = max(len(a), len(b))
    a = (a * (max_len // len(a) + 1))[:max_len]
    b = (b * (max_len // len(b) + 1))[:max_len]
    return ''.join([chr(ord(x) ^ ord(y)) for x, y in zip(a, b)])

print('\n'.join([xor_str(strings[i], flag) for i in range(0, len(strings))]))
```

As it turns out, when `flag = '  grodno{'` echoes of some English text appear (e.g. `England`):

```plain
...
6)from wh60t;`rn3t%.`e,h>rx:!yje*<)im<Z342{s
n`ons werx=pvmsn rl-rx0-{rdd-.njmo&z?lk#F3/97f
rht count!1zfsn=hcv1l{*re`,w-lnlb<n`&Q`)97Y
hlutznaer^x1do$7,"Rdm&!b-t:rqn-%;}vih<1g"8Pa"$7{
t)EnglandIx$t;ie+{rcbv2m>r?!167-ueDe}2m" Ja52{d
z and wr,61t}en5}`c,7s.=w:6 i"s&q'!a Rc'9~}
...
```

Sometimes some strings clearly have endings of some English words, sometimes continuation of an
English word to the right of the flag is clear.

One of the strings contains `rhrdships`. If `flag='hagrodno{`, then the output is `:)rdships`.
Finally, if `flag=':)grodno{`, then the output is `hardships` which we were looking for. Moving in
this direction is what we will do, now on the other side of the flag.

Let's fix the code a little, so that we can keep guessing english words more easily. We will limit
the output by the length of the flag, and add one more symbol to the right, so that we know what
to put at the end of the flag. Also for ease of use (so that we won't keep losing track of lines)
before each line a number will be added.

```python
strings = list(map(lambda x: bytes.fromhex(x).decode('utf-8'), open("otp_message.txt").read().split('\n')))

flag = ':)grodno{'

def xor_str(a, b):
    max_len = max(len(a), len(b))
    a = (a * (max_len // len(a) + 1))[:max_len]
    b = (b * (max_len // len(b) + 1))[:max_len]
    return ''.join([chr(ord(x) ^ ord(y)) for x, y in zip(a, b)])

print('\n'.join([str(i) + " " + xor_str(strings[i], flag)[:len(flag)] + " " + xor_str(strings[i], flag)[len(flag)] for i in range(0, len(strings))]))
```

```plain
0 I was bor 
1 of a good _
2 ather bei 
3 st at Hul 
4 nd leavin 
5 , from wh 
6 tions wer 
...
```

For example, it is clear that `bei...` wants a `ng` at the end. After we add `ng` to the end of the
flag, `:)grodno{ng`, the output will be `beiEx`. Now if we put `Ex` instead, `:)grodno{Ex`, the
output will be `being`, and also all the other strings have advanced with their english words.

```plain
0 I was born  g
1 of a good f o
2 ather being .
3 st at Hull. .
4 nd leaving  a
5 , from when m
6 tions were  `
...
```

Flag:

```plain
grodno{Ex4ctly! R0b1ns0n Crus03 by Dan1el D3f0e}
```
