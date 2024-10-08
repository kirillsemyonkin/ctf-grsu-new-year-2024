# PPC - Дополнительные Замечания / Additional Notes

Программы приведены на Rust и на Python. В них используется функция `read_line` чтения из сокета и
одновременного вывода в консоль. Ниже также приведены общие импорты.

---

Programs are listed in Rust and Python. A `read_line` function is used to read from the socket with
simultaneous output to the console. Common imports are also listed below.

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
