# BEEF

> Официант, я заказал говяжую, а это ~̡͆Q͍̓̆͟q̨̖͖̎̽̃͜͝x͍̕n̨̞̼̻̯̾͋̈́̏̓
>
> Найдите флаг.

---

> Waiter, I ordered BEEF and this is ~̡͆Q͍̓̆͟q̨̖͖̎̽̃͜͝x͍̕n̨̞̼̻̯̾͋̈́̏̓
>
> Find flag.

## Решение / Solution

Исходный код программы после выполнения декомпиляции под IDA:

```c++
// bad sp value at call has been detected, the output may be wrong!
int __cdecl sub_11BD(int a1)
{
  int v2; // [esp-14h] [ebp-44h]
  int v3; // [esp-10h] [ebp-40h]
  int v4; // [esp-Ch] [ebp-3Ch]
  int v5; // [esp-8h] [ebp-38h]
  int v6; // [esp-4h] [ebp-34h]
  long double v7; // [esp+0h] [ebp-30h] BYREF
  _DWORD v8[4]; // [esp+Ch] [ebp-24h] BYREF
  const char *v9; // [esp+1Ch] [ebp-14h] BYREF
  int v10; // [esp+20h] [ebp-10h]
  unsigned int v11; // [esp+24h] [ebp-Ch]
  int *v12; // [esp+28h] [ebp-8h]

  v12 = &a1;
  puts("Input:", v2, v3, v4, v5, v6, LODWORD(v7), DWORD1(v7), HIDWORD(v7), v8[0], v8[1], v8[2]);
  gets(v8);
  v10 = 4;
  v9 = "BEEF";
  if ( !((int (__cdecl *)(const char **))strncmp)(&v9) )
  {
    *(_DWORD *)((char *)&v7 + 1) = 1714434913;
    *(_DWORD *)((char *)&v7 + 5) = 1650684261;
    *(_WORD *)((char *)&v7 + 9) = 30778;
    HIBYTE(v7) = -11;
    v11 = 0;
    v10 = 0;
    while ( v11 <= 0xA )
    {
      v10 = *((unsigned __int8 *)&v7 + v11 + 1);
      v10 ^= v11;
      *((_BYTE *)&v7 + ++v11) = ++v10;
    }
    printf("Good BEEF! grodno{%s}\n", (char *)&v7 + 1);
  }
  else
  {
    v9 = (const char *)&v9;
    ((void (__cdecl *)(const char *))printf)("Try again, %s not BEEF\n");
  }
  return 0;
}
```

Переменная `v7` используется как массив байтов (`char*`). Для удобства преобразуем задаваемые
значения в HEX-формат:

```c++
    *(_DWORD *)(((char *) &v7)[1]) = 0x66303361;
    *(_DWORD *)(((char *) &v7)[5]) = 0x62637165;
    *(_WORD *)(((char *) &v7)[9]) = 0x783A;
```

Тут записывается 10 байт, аналогично концу цикла `0xA`.

Теперь посмотрим на цикл:

```c++
for (int i = 0; i <= 10; i++) {
    ((char*) &v7)[i + 1] = (((char*) &v7)[i + 1] ^ i) + 1;
}
```

Выполним данную операцию над значениями выше.

> [!WARNING]
> IDA / C++ / кто-там-еще-есть хранят числа в формате Little Endian.

```plain
  61333066657163623a78
^ 00010203040506070809
+ 01010101010101010101
= 62333366627566663372
```

Флаг:

```plain
grodno{b33fbuff3r}
```

---

Source code of the program after decompiling with IDA:

```c++
// bad sp value at call has been detected, the output may be wrong!
int __cdecl sub_11BD(int a1)
{
  int v2; // [esp-14h] [ebp-44h]
  int v3; // [esp-10h] [ebp-40h]
  int v4; // [esp-Ch] [ebp-3Ch]
  int v5; // [esp-8h] [ebp-38h]
  int v6; // [esp-4h] [ebp-34h]
  long double v7; // [esp+0h] [ebp-30h] BYREF
  _DWORD v8[4]; // [esp+Ch] [ebp-24h] BYREF
  const char *v9; // [esp+1Ch] [ebp-14h] BYREF
  int v10; // [esp+20h] [ebp-10h]
  unsigned int v11; // [esp+24h] [ebp-Ch]
  int *v12; // [esp+28h] [ebp-8h]

  v12 = &a1;
  puts("Input:", v2, v3, v4, v5, v6, LODWORD(v7), DWORD1(v7), HIDWORD(v7), v8[0], v8[1], v8[2]);
  gets(v8);
  v10 = 4;
  v9 = "BEEF";
  if ( !((int (__cdecl *)(const char **))strncmp)(&v9) )
  {
    *(_DWORD *)((char *)&v7 + 1) = 1714434913;
    *(_DWORD *)((char *)&v7 + 5) = 1650684261;
    *(_WORD *)((char *)&v7 + 9) = 30778;
    HIBYTE(v7) = -11;
    v11 = 0;
    v10 = 0;
    while ( v11 <= 0xA )
    {
      v10 = *((unsigned __int8 *)&v7 + v11 + 1);
      v10 ^= v11;
      *((_BYTE *)&v7 + ++v11) = ++v10;
    }
    printf("Good BEEF! grodno{%s}\n", (char *)&v7 + 1);
  }
  else
  {
    v9 = (const char *)&v9;
    ((void (__cdecl *)(const char *))printf)("Try again, %s not BEEF\n");
  }
  return 0;
}
```

The variable `v7` is used as a byte array (`char*`). For convenience, let's convert the given values
into HEX-format:

```c++
    *(_DWORD *)(((char *) &v7)[1]) = 0x66303361;
    *(_DWORD *)(((char *) &v7)[5]) = 0x62637165;
    *(_WORD *)(((char *) &v7)[9]) = 0x783A;
```

There are 10 bytes written, similar to the loop's ending value being `0xA`.

Now let's at a different look at the loop:

```c++
for (int i = 0; i <= 10; i++) {
    ((char*) &v7)[i + 1] = (((char*) &v7)[i + 1] ^ i) + 1;
}
```

Let's perform this operation on the values above.

> [!WARNING]
> IDA / C++ / whoever-else-there-is use Little Endian.

```plain
  61333066657163623a78
^ 00010203040506070809
+ 01010101010101010101
= 62333366627566663372
```

Flag:

```plain
grodno{b33fbuff3r}
```
