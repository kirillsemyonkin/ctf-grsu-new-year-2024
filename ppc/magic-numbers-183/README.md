# Magic numbers

> Вам предлагается стандартная задача - по представлению начала файла в hex-формате назвать его тип (указать расширение). И через много циклов вы получите флаг )
>
> Например, если файл начинается с FF D8 FF E0 00 10 4A 46 49 46 00 01 ...., то это графический файл формата JPG

---

> You are offered a standard task - given the beginning of the file in hex format, name its type (specify the extension). And after many cycles you will get a flag)
>
> For example, if the file begins with FF D8 FF E0 00 10 4A 46 49 46 00 01 ...., then it is a graphic file and the response will be JPG

## Примеры / Examples

```plain
Решайте задачу в удаленном терминале. 50 раундов и флаг ваш !
Время на ответ - не больше 5 секунд ...

Вам необходимо по начальным байтам файла определить его тип.

Ответ - строка символов, соответствующих расширению файла

Раунд 1/50

FF D8 FF E0 6E DC 62 50 A8 C8 4D 94 7B 38 17 4A 79 20 E6 1D 3C A3 6A 52 40 2A FC A3 F9 F6 4F 1E 1B 5B

JPG
Правильно: JPG
Раунд 2/50

5F 27 A8 89 27 A0 62 4D 5F 70 63 3D 5B FA 1A C3 3F E4 34 72 B3 8C 42 14 C3 24 1A 75 E6 9B 45 19 CD D3


```

## Решение / Solution

Не все "магические числа" выводимые сервером легко найти в интернете. Проще отправить серверу
неверный ответ (например, `UNKNOWN`), после чего можно сопоставить начальный байт с каждым таким
ответом.

---

Not all "magic numbers" sent by the server can be easily found on the internet. It is easier to send
the server a wrong answer (e.g. `UNKNOWN`), and you can map the first byte to each such answer.

---

```plain
50 -> ZIP
42 -> BMP
89 -> PNG
FF -> JPG
52 -> RAR
60 -> ARJ
5F -> JAR
47 -> GIF
5A -> SWF
46 -> SWF
49 -> TIF
```

### Флаг / Flag

```plain
grodno{b27e80m4g!c_numb3rs_y0u_n33d_t0_kn0wbfb881}
```

### Rust

```rust
fn main() {
    let sock = TcpStream::connect("...").unwrap();
    let mut reader = BufReader::new(&sock);
    let mut writer = BufWriter::new(&sock);

    let mut buf = String::new();

    for _ in 0..5 {
        read_line(&mut reader, &mut buf);
    }
    for _ in 0..=50 {
        for _ in 0..4 {
            read_line(&mut reader, &mut buf);
        }
        let str = read_line(&mut reader, &mut buf);
        let res = match &str[..2] {
            "50" => "ZIP",
            "42" => "BMP",
            "89" => "PNG",
            "FF" => "JPG",
            "52" => "RAR",
            "60" => "ARJ",
            "5F" => "JAR",
            "47" => "GIF",
            "5A" => "SWF",
            "46" => "SWF",
            "49" => "TIF",
            _ => "UNKNOWN",
        };
        writeln!(writer, "{}", res).unwrap();
        writer.flush().unwrap();
        println!("{}", res);
    }
}
```

### Python

```python
sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
sock.connect(("...", ...))
reader = sock.makefile("r", encoding="utf-8")
writer = sock.makefile("w", encoding="utf-8")

for _ in range(5):
    read_line(reader)
for _ in range(50 + 1):
    for _ in range(4):
        read_line(reader)
    str = read_line(reader)
    res = "UNKNOWN"
    match str[:2]:
        case "50": res = "ZIP"
        case "42": res = "BMP"
        case "89": res = "PNG"
        case "FF": res = "JPG"
        case "52": res = "RAR"
        case "60": res = "ARJ"
        case "5F": res = "JAR"
        case "47": res = "GIF"
        case "5A": res = "SWF"
        case "46": res = "SWF"
        case "49": res = "TIF"
        case _: pass
    writer.write(f"{res}\n")
    writer.flush()
    print(res)
```
