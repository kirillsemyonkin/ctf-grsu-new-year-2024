# LCG

> Линейный конгруэнтный генератор (ЛКГ) - это метод генерации псевдослучайных чисел. Его работа описывается формулой:
>
> **Xn+1 = (a * Xn + c) mod m, n >= 0**
>
> **X0 = const**
>
> Важно помнить, что ЛКГ неисправимо небезопасен и совершенно непригоден для криптографического использования .
>
> Допустим, вы смогли перехватить последовательные числа, когорые создал ЛКГ. Докажите что он не безопасен, взломав ЛКГ. Для этого достаточно найти его параметры a, c, m и предсказать следующее псевдослучайное число.
>
> Флаг в формате **grodno{a;c;m;next_number}**

---

> Linear congruent generator (LCG) is a method for generating pseudorandom numbers. Its work is described by the formula:
>
> **Xn+1 = (a * Xn + c) mod m, n >= 0**
>
> **X0 = const**
>
> It is important to remember that LCG is incorrigibly insecure and completely unsuitable for cryptographic use.
>
> Let's say you were able to intercept the sequential numbers that LCG created. Prove that it is not safe by hacking LCG. To do this, it is enough to find its parameters a, c, m and predict the next pseudo-random number.
>
> Flag in the format **grodno{a;c;m;next_number}**

## [Вывод / Output](LCG_numbers.txt)

```plain
387711161689525
4394061816296776
4807620591083657
1628387486050168
4678383467611709
4135587569352880
4911010274481381
2093641099789512
594490528484973
413558560567764
```

## Решение / Solution

Задача уже решена автором библиотеки [LCG для Rust](https://crates.io/crates/lcg-tools).
Единственное что, так это то, что она принимала `isize` вместо `BigInt`, и из-за этого могла
вылетать, поэтому требовала небольших правок.

```rust
fn main() {
    let file = File::open("LCG_numbers.txt").unwrap();
    let reader = BufReader::new(file);
    let numbers = reader
        .lines()
        .map(|str| BigInt::from_str_radix(&str.unwrap(), 10).unwrap())
        .collect::<Vec<_>>();
    let mut lcg = crack_lcg(&numbers).unwrap();
    println!(
        "grodno{{{};{};{};{}}}",
        lcg.a.clone(),
        lcg.c.clone(),
        lcg.m.clone(),
        lcg.next().unwrap()
    );
}
```

Флаг:

```rust
grodno{25847423595831;4911010480461301;4911010483207700;1550845196037885}
```

---

The task has already been solved by the author of the
[LCG library in Rust](https://crates.io/crates/lcg-tools). The only thing - it accepted `isize` as
opposed to `BigInt`, and was prone to crashing because of that, so a slight adjustment was needed.

```rust
fn main() {
    let file = File::open("LCG_numbers.txt").unwrap();
    let reader = BufReader::new(file);
    let numbers = reader
        .lines()
        .map(|str| BigInt::from_str_radix(&str.unwrap(), 10).unwrap())
        .collect::<Vec<_>>();
    let mut lcg = crack_lcg(&numbers).unwrap();
    println!(
        "grodno{{{};{};{};{}}}",
        lcg.a.clone(),
        lcg.c.clone(),
        lcg.m.clone(),
        lcg.next().unwrap()
    );
}
```

Flag:

```rust
grodno{25847423595831;4911010480461301;4911010483207700;1550845196037885}
```
