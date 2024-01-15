# Speeding up RSA

> Очень уж медленно строятся случайные простые числа для алгоритма RSA ...
>
> Посмотрел таблицу простых чисел. Супер ! Как их много и как они близко друг от друга ...

---

> The construction of random prime numbers for the RSA algorithm is very slow...
>
> I looked at the table of prime numbers. Super! How many there are and how close they are to each other...

## [Исходный код / Source code](FastRSA.txt)

```python
from Crypto.Util.number import getPrime, isPrime, bytes_to_long
from random import shuffle, randint
from secret import flag

flag = bytes_to_long(flag)

def GenerateModulo():
    p = getPrime(512)
    res = [q for q in range(p, p+100, 2) if isPrime(q)]
    shuffle(res)
    return (p, res[0])

p, q = GenerateModulo()
n = p * q
e = 65537

ciphertext = pow(flag, e, n)

print(f"n = {n}")
print(f"e = {e}")
print(f"ciphertext = {ciphertext}")
```

## [Вывод / Output](output_FRSA.txt)

```python
n = 68022741432432659084802752907723896845807597528827093397040482890296955569957917533647208679014132848196640022782537553867867116789555103992690960043358529714577060390999199352850076508734027336995147674705206553971423041116507591767092936323207651404971678259040137037188349250850647087365720392427587716357
e = 65537
ciphertext = 33645617730925667540706843258029945045275653793905004841831372367593972949577825491212282337168392705134683986040003011405449559098226368343868360682321377784836699252707573555021829931008096967637686723813766415931847878736794077491708783671648158579854763189010308346516641122365460325044193705398891622627
```

## Решение / Solution

Подсказка в условии задании (да и в коде тоже видно), что факторы `n` находятся рядом. Поэтому эта
задача подпадает под [Fermat Attack](https://bitsdeep.com/posts/attacking-rsa-for-fun-and-ctf-points-part-2/).

Так как этот вид атаки включен в [RsaCtfTool](https://github.com/RsaCtfTool/RsaCtfTool), достаточно
запустить его:

```bash
./RsaCtfTool.py -n 68022741432432659084802752907723896845807597528827093397040482890296955569957917533647208679014132848196640022782537553867867116789555103992690960043358529714577060390999199352850076508734027336995147674705206553971423041116507591767092936323207651404971678259040137037188349250850647087365720392427587716357 -e 65537 --decrypt 33645617730925667540706843258029945045275653793905004841831372367593972949577825491212282337168392705134683986040003011405449559098226368343868360682321377784836699252707573555021829931008096967637686723813766415931847878736794077491708783671648158579854763189010308346516641122365460325044193705398891622627 --attack fermat
```

> [!NOTE]
> Если запустить RsaCtfTool без `--attack fermat`, то он может решить задачу другими атаками,
> например, SQUFOF.

Флаг:

```plain
grodno{Ofru*YgePg8h}
```

---

The hint is in the description of the task (in the code it is visible too though): `n` has two prime
factors that are close to each other. Thus, this task falls under [Fermat Attack](https://bitsdeep.com/posts/attacking-rsa-for-fun-and-ctf-points-part-2/).

Since this attack is included in [RsaCtfTool](https://github.com/RsaCtfTool/RsaCtfTool), you can
simply run it:

```bash
./RsaCtfTool.py -n 68022741432432659084802752907723896845807597528827093397040482890296955569957917533647208679014132848196640022782537553867867116789555103992690960043358529714577060390999199352850076508734027336995147674705206553971423041116507591767092936323207651404971678259040137037188349250850647087365720392427587716357 -e 65537 --decrypt 33645617730925667540706843258029945045275653793905004841831372367593972949577825491212282337168392705134683986040003011405449559098226368343868360682321377784836699252707573555021829931008096967637686723813766415931847878736794077491708783671648158579854763189010308346516641122365460325044193705398891622627 --attack fermat
```

> [!NOTE]
> If you run RsaCtfTool without `--attack fermat`, it can solve the task with other attacks, for
> example, SQUFOF.

Flag:

```plain
grodno{Ofru*YgePg8h}
```