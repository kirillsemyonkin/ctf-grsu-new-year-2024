# IsPrime

> Помните, какие числа называются простыми ?
>
> В этой задаче тоже все просто - получаете на вход число и говорите, является оно простым или нет.
>
> Подключайтесь и решайте. 50 раундов - и флаг ваш !

---

> Remember what numbers are called prime?
>
> In this problem, everything is also simple - you receive a number as input and say whether it is prime or not.
>
> Connect and decide. 50 rounds - and the flag is yours!

## Примеры / Examples

```plain
Помните, какие числа называются **простыми** ?
В этой задаче тоже все просто - получаете на вход число и говорите, является оно простым или нет.
50 раундов - и флаг ваш  ...
Время на ответ - не больше 5 секунд.
Ваш ответ: YES или NO

Раунд 1/50
210

NO
Правильно: NO
Раунд 2/50
61


```

## Решение / Solution

> [!NOTE]
> Если постараться, и если повезет с числами, можно ограничиться некоторыми правилами простых чисел
> и решить данную задачу без программирования.

Легко решается на языке Rust даже без решета Эратосфена, например через
[`primes::is_prime`](https://docs.rs/primes/latest/primes/fn.is_prime.html).

На Python тоже решается с библиотеками, например с помощью
[`sympy.isprime`](https://docs.sympy.org/latest/modules/ntheory.html#sympy.ntheory.primetest.isprime).

---

> [!NOTE]
> If you try hard enough, and if luck would be on your side, you can just use some basics of prime
> numbers and solve this problem without programming.

Easily solved on Rust even without the Sieve of Eratosthenes, like
[`use primes::is_prime`](https://docs.rs/primes/latest/primes/fn.is_prime.html).

On Python it also can be solved with libraries, for example
[`from sympy import isprime`](https://docs.sympy.org/latest/modules/ntheory.html#sympy.ntheory.primetest.isprime).

---

### Флаг / Flag

```plain
grodno{023860pr!m3_numb3r_!s_d!v!s!b13_0n1y_by_1_4nd_by_!+s31f5f0888}
```

### Rust

```rust
use primes::is_prime;

fn main() {
    let sock = TcpStream::connect("...").unwrap();
    let mut reader = BufReader::new(&sock);
    let mut writer = BufWriter::new(&sock);

    let mut buf = String::new();

    for _ in 0..4 {
        read_line(&mut reader, &mut buf);
    }
    for _ in 0..=50 {
        for _ in 0..3 {
            read_line(&mut reader, &mut buf);
        }
        let num = read_line(&mut reader, &mut buf).parse::<u64>().unwrap();
        let is_prime = is_prime(num);
        writeln!(writer, "{}", if is_prime { "YES" } else { "NO" }).unwrap();
        writer.flush().unwrap();
        println!("{}", if is_prime { "YES" } else { "NO" });
    }
}
```

### Python

```python
from sympy import isprime

sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
sock.connect(("...", ...))
reader = sock.makefile("r", encoding="utf-8")
writer = sock.makefile("w", encoding="utf-8")

for _ in range(4):
    read_line(reader)
for _ in range(50 + 1):
    for _ in range(3):
        read_line(reader)
    num = int(read_line(reader))
    is_prime = isprime(num)
    writer.write("{}\n".format("YES" if is_prime else "NO"))
    writer.flush()
    print("YES" if is_prime else "NO")
```
