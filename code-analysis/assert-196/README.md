# Assert

> По своей сути инструкция assert в языке Python является средством отладки, которое проверяет условие.
>
> Если условие в assert истинно, то ничего не происходит и ваша программа продолжает выполняться как обычно.
>
> Но если же вычисление условия дает результат ложно, то вызывается исключение AssertionError с необязательным сообщением об ошибке.
>
> Взломайте эту программу, в которой много разных assert, и найдите флаг

---

> At its core, the assert statement in Python is a debugging tool that tests a condition.
>
> If the condition in assert is true, then nothing happens and your program continues to execute as normal.
>
> However, if the condition evaluates to false, then an AssertionError exception is raised with an optional error message.
>
> Hack this program, which has a lot of different assert, and find the flag

## [Исходный код / Source code](crackme.py)

```python
# uncompyle6 version 3.5.0
# Python bytecode 2.7 (62211)
# Decompiled from: Python 2.7.5 (default, Nov 16 2020, 22:23:17) 
# [GCC 4.8.5 20150623 (Red Hat 4.8.5-44)]
# Embedded file name: crackme.py
# Compiled at: 2011-09-19 04:54:37
import sys, types
assert len(sys.argv) == 10
a, b, c, d, e, f, g, h, i = [ int(x) for x in sys.argv[1:] ]
assert b == c
assert c == g
assert g == h
assert g + b + c == 0
codestr = ('').join(chr(x) for x in [a, b, c, d, e, f, g, h, i])
result = ''
assert 3 * a + 12 * d + e + 4 * f + 6 * i == 2194
assert -6 * a + 2 * d - 4 * e - f + 9 * i == -243
assert a + 6 * d + 2 * e + 7 * f + 11 * i == 2307
assert 5 * a - 2 * d - 7 * e + 76 * f + 8 * i == 8238
assert 2 * a - 2 * d - 2 * e - 2 * f + 2 * i == -72

def xorc(a, b):
    return chr(ord(a) ^ ord(b))


def xorstr(a, b):
    return ('').join([ xorc(a[(i % len(a))], c) for i, c in enumerate(b) ])


result += xorstr(codestr, '\x1bro#&\x0b{t;\x19_44;Wrt\x0cLp35|\x100r\x0c\x15s_1{\x16y_&\x0f3f2$;wh`\x12_dt*\x11gg:\x12g_7:Tgrg\x11s}')

def getSolutionAsParameterAndPrint(myChallengeSolution):
    print (0)


recycled_code = getSolutionAsParameterAndPrint.func_code
new_code = types.CodeType(recycled_code.co_argcount, recycled_code.co_nlocals, recycled_code.co_stacksize, recycled_code.co_flags, codestr, recycled_code.co_consts, recycled_code.co_names, recycled_code.co_varnames, recycled_code.co_filename, recycled_code.co_name, recycled_code.co_firstlineno, recycled_code.co_lnotab, recycled_code.co_freevars, recycled_code.co_cellvars)
new_fun = types.FunctionType(new_code, globals(), 'keepOnDigging', getSolutionAsParameterAndPrint.func_defaults)
new_fun(result)
```

## Решение / Solution

Из первых выражений ясно, что `b == c == g == h == 0`. Тогда
`codestr = ('').join(chr(x) for x in [a, 0, 0, d, e, f, 0, 0, i])`.

Дальше происходит XOR `codestr` и некой строки. Возьмем первые 7 символов этой строки, и выполним
XOR с `grodno{` в тех символах, где в `codestr` нет нулей. Это будет `7c 00 00 47 48 64 00`.

Для оставшегося `i` достаточно просто подобрать такое число, чтобы строка преобразовалось во что-то
внятное. Тогда получаем `7c 00 00 47 48 64 00`.

Флаг:

```plain
grodno{the_4ss3rt_0p3r4t0r_is_v3ry_us3ful_wh3n_d3bugging_pr0gr4ms}
```

---

Out of the first expressions it is clear that `b == c == g == h == 0`. Then
`codestr = ('').join(chr(x) for x in [a, 0, 0, d, e, f, 0, 0, i])`.

Then `codestr` is XORed with some string. Let's take the first 7 characters of this string, and do
a XOR with `grodno{` in the places where there are no zeros in `codestr`. This will give us
`7c 00 00 47 48 64 00`.

Then for the remaining `i` we just have to pick some number that will turn the string into something
meaningful. This will give us `7c 00 00 47 48 64 00 00 53`.

Flag:

```plain
grodno{the_4ss3rt_0p3r4t0r_is_v3ry_us3ful_wh3n_d3bugging_pr0gr4ms}
```
