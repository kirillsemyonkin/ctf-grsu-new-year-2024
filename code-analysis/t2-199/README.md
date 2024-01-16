# T2

> Просто найдите флаг

---

> Just find the flag

## Решение / Solution

В декомпиляции через IDA нас интересуют следующие моменты:

```c++
v3 = sub_1349(v11);
((void (*)(void))std::__cxx11::basic_string<char,std::char_traits<char>,std::allocator<char>>::_M_dispose)();
v4 = 13LL;
v5 = "Oh, thanks))\n";
if ( v3 )
{
    v4 = 28LL;
    v5 = "Don't tell me this anymore.\n";
}
```

А вот `sub_1349`:

```c++
bool __fastcall sub_1349(_QWORD *a1)
{
  __int64 v1; // r9
  int v2; // r8d
  __int64 i; // rcx
  int v4; // eax
  int v5; // esi

  v1 = a1[1];
  v2 = v1 != 23;
  for ( i = 0LL; ; ++i )
  {
    v4 = i;
    if ( v1 == i )
      break;
    v5 = *(char *)(*a1 + i);
    v2 += (v5 + dword_4020[v4 % 23]) % 1337;
  }
  return v2 != 0;
}
```

Вот `dword_4020` в виде десятичных чисел:

```plain
.data:0000000000004020 ; _DWORD dword_4020[24]
.data:0000000000004020 dword_4020      dd 1234, 1223, 1226, 1237, 1227, 1226, 1214, 1265, 1240
.data:0000000000004020                                         ; DATA XREF: sub_1349+12↑o
.data:0000000000004044                 dd 1225, 1225, 1216, 1242, 1259, 1286, 1218, 1242, 1248
.data:0000000000004068                 dd 1286, 1240, 1223, 1304, 1212, 0
```

Требуется найти такие `v5`, чтобы `(v5 + dword_4020) % 1337 == 0`, оно же `v5 + dword_4020 == 1337`.
Вычитание dword_4020 из `1337` - `103, 114, 111, 100, 110, 111, 123, 72, 97, 112, 112, 121, 95, 78
51, 119, 95, 89, 51, 97, 114, 33, 125` - достаточно преобразовать это в строку.

Флаг:

```plain
grodno{Happy_N3w_Y3ar!}
```

---

In decompilation using IDA, we are interested in the following moments:

```c++
v3 = sub_1349(v11);
((void (*)(void))std::__cxx11::basic_string<char,std::char_traits<char>,std::allocator<char>>::_M_dispose)();
v4 = 13LL;
v5 = "Oh, thanks))\n";
if ( v3 )
{
    v4 = 28LL;
    v5 = "Don't tell me this anymore.\n";
}
```

Here is `sub_1349`:

```c++
bool __fastcall sub_1349(_QWORD *a1)
{
  __int64 v1; // r9
  int v2; // r8d
  __int64 i; // rcx
  int v4; // eax
  int v5; // esi

  v1 = a1[1];
  v2 = v1 != 23;
  for ( i = 0LL; ; ++i )
  {
    v4 = i;
    if ( v1 == i )
      break;
    v5 = *(char *)(*a1 + i);
    v2 += (v5 + dword_4020[v4 % 23]) % 1337;
  }
  return v2 != 0;
}
```

`dword_4020` here in decimal format:

```plain
.data:0000000000004020 ; _DWORD dword_4020[24]
.data:0000000000004020 dword_4020      dd 1234, 1223, 1226, 1237, 1227, 1226, 1214, 1265, 1240
.data:0000000000004020                                         ; DATA XREF: sub_1349+12↑o
.data:0000000000004044                 dd 1225, 1225, 1216, 1242, 1259, 1286, 1218, 1242, 1248
.data:0000000000004068                 dd 1286, 1240, 1223, 1304, 1212, 0
```

So it is necessary to find such `v5` so that `(v5 + dword_4020) % 1337 == 0`, which is also
`v5 + dword_4020 == 1337`. Subtracting `dword_4020` from `1337` - `103, 114, 111, 100, 110, 111,
123, 72, 97, 112, 112, 121, 95, 78 51, 119, 95, 89, 51, 97, 114, 33, 125` - it is enough to convert
this to a string.

Флаг:

```plain
grodno{Happy_N3w_Y3ar!}
```
