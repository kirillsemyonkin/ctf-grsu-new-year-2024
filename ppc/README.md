# PPC - Дополнительные Замечания / Additional Notes

Программы приведены на Rust и на Python. В них используется функция `read_line` для одновременного
ввода и вывода из сокета. Ниже также приведены общие импорты.

---

Programs are listed in Rust and Python. A `read_line` function is used in both for simultaneous
input and output from the socket. Common imports are also listed below.

---

```rust
use std::io::BufReader;
use std::io::BufWriter;
use std::io::Read;
use std::io::Write;
use std::net::TcpStream;

fn read_line<'a>(reader: &mut BufReader<&TcpStream>, buf: &'a mut String) -> &'a str {
    buf.clear();
    reader.read_line(buf).unwrap();
    let res = buf.trim();
    println!("{}", res);
    res
}
```

```python
import socket

def read_line(reader):
    res = reader.readline().strip()
    print(res)
    return res
```
