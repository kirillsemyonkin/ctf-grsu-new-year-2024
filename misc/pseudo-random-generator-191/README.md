# Pseudo-Random Generator

> Боб использует генератор псевдослучайных чисел, который основан на возведении в степень. Его формула:
>
> Xn = pn mod m, n >= 0, 0 < p, m < 10000
>
> Докажите, что он тоже небезопасен и непригоден для криптографического использования .
>
> Допустим, вы смогли перехватить последовательные числа, когорые создал генератор. Взломайте его. Для этого достаточно найти его параметры p и m и предсказать следующее псевдослучайное число.
>
> Флаг в формате grodno{p;m;next_number}

---

> Bob uses a pseudorandom number generator that is based on exponentiation. Its formula:
>
> Xn = pn mod m, n >= 0, 0 < p, m < 10000
>
> Prove that it too is insecure and unsuitable for cryptographic use.
>
> Let's say you were able to intercept the sequential numbers that the generator created. Hack it. To do this, it is enough to find its parameters p and m and predict the next pseudo-random number.
>
> Flag in the format grodno{p;m;next_number}

## [Вывод / Output](PRG_numbers.txt)

```plain
8846
7664
5612
2894
6103
7030
3151
4579
4525
2508
3322
```

## Решение / Solution

Достаточно подобрать `n`, `p`, `m` в пределах от 1 до 10000. Выполним этот перебор на Rust с
использованием параллелизма через `rayon` и вычисления с помощью `mod_exp`.

```rust
fn main() {
    const NUMBERS: [usize; 3] = [8846, 7664, 5612]; // 3 is enough
    let m_min = NUMBERS.iter().max().unwrap() + 1;
    println!(
        "{:?}",
        (0..10000).into_par_iter().find_map_first(|n| {
            println!("{}", n);
            for p in 1..10000 {
                for m in m_min..10000 {
                    if NUMBERS
                        .iter()
                        .enumerate()
                        .all(|(i, num)| *num == mod_exp(p, n + i, m))
                    {
                        return Some(format!("grodno{{{};{};{}}}", p, m, mod_exp(p, n + 11, m)));
                    }
                }
            }
            None
        })
    );
}
```

Флаг:

```plain
grodno{893;9241;185}
```

---

It is enough to find the `n`, `p`, `m` in the range from 1 to 10000. Let's perform this brute-force
with Rust with parallelism using `rayon` and calculation with `mod_exp`.

```rust
fn main() {
    const NUMBERS: [usize; 3] = [8846, 7664, 5612]; // 3 is enough
    let m_min = NUMBERS.iter().max().unwrap() + 1;
    println!(
        "{:?}",
        (0..10000).into_par_iter().find_map_first(|n| {
            println!("{}", n);
            for p in 1..10000 {
                for m in m_min..10000 {
                    if NUMBERS
                        .iter()
                        .enumerate()
                        .all(|(i, num)| *num == mod_exp(p, n + i, m))
                    {
                        return Some(format!("grodno{{{};{};{}}}", p, m, mod_exp(p, n + 11, m)));
                    }
                }
            }
            None
        })
    );
}
```

Флаг:

```plain
grodno{893;9241;185}
```
