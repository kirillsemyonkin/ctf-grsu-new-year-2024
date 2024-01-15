# QR-code

> Флаг спрятан в QR-кодах

---

> The flag is hidden in QR codes

## Примеры / Examples

```plain
Все что тебе надо - найти QR-код с флагом. И так раз 50 ...
Время на ответ - не больше 5 секунд ...

Раунд 1/54
iVBORw0KGgoAAAANSUhEUgAAACkAAAApAQAAAACAGz1bAAAA4klEQVR4nGP4DwINDFipD9JGc9gbGL7furv2ewPDl+j6r+JAKoorFEQFT3UVB8kp5ALlPoiGhgBV/v9Ufh2o78+hoyvrgdSTWt7tDQy/OI68PQ6U2ylUbA7UcGGx9XOghvdCwvMbGP6F5JgBVX4tXTkHqOSL7y9F+waGzzmp7uoglXt++jcw/E6yjCsHKjlmYXK/geHTQw3z6Q0Mf3m/P1vewPBzS8Q9oIbvs/0vlINs38buDjRa5MLSeKDg3U3PQe6M++CQDqRCprbNB7la0RfowO83/7NcB/nPJ6Meh98hFABQ4p39XBxi8wAAAABJRU5ErkJggg==

depict_&_depression_&_depth
Правильно: depict_&_depression_&_depth
Раунд 2/54
iVBORw0KGgoAAAANSUhEUgAAACkAAAApAQAAAACAGz1bAAAA5ElEQVR4nGP4DwINDFipDzIub9gbGL7fM8r93sDwJYo9VBxIxce1gqggd1Yg9f2G2Fqg3AfR0BCgyv9f6r4D9f05oqNZ38Dw49tL3fVA3oGix+4NDF8V95eqNzB8lnho/L6B4dvCiZf3A5Ucu3nLHmjK61CZ80Dtfr9OAPX9+5ipot/A8DuS62Z6A8MnVb9jQMHvoUZTrwOt3WVpDjLT6/hhoJm/k9XrgGZ+lXxtkA/U/t1jO1DlB4Hjqv4gl9k9BzrpS4zlAX4gFfhj3nwgFc6aCHTg9ztLTcKBKmWTNXD5HUIBAMXOmuRTozrTAAAAAElFTkSuQmCC


```

## Решение / Solution

### Флаг / Flag

```plain
grodno{527c70_d3c0d3_&_fr0m_&_QR_2f8887}
```

### Rust

> [!WARNING]
> `bardecoder` имеет трудности со столь маленькими QR кодами. Решается простым увеличением картинки
> через `image`.

---

> [!WARNING]
> `bardecoder` has trouble with such small QR codes. It can be solved by simply scaling the image
> via `image`.

---

```rust
fn main() {
    let sock = TcpStream::connect("...").unwrap();
    let mut reader = BufReader::new(&sock);
    let mut writer = BufWriter::new(&sock);

    let mut buf = String::new();

    let decoder = bardecoder::default_decoder();

    for _ in 0..1 {
        read_line(&mut reader, &mut buf);
    }
    for _ in 0..=60 {
        for _ in 0..3 {
            read_line(&mut reader, &mut buf);
        }
        let base64 = read_line(&mut reader, &mut buf);
        let image_data = base64::engine::general_purpose::STANDARD
            .decode(base64)
            .unwrap();
        std::fs::write("tmp.png", image_data).unwrap();
        let img = image::open("tmp.png").unwrap();
        let img = img.resize(
            img.width() * 5,
            img.height() * 5,
            image::imageops::FilterType::Nearest,
        );
        // img.save("tmp.png").unwrap();
        let res = decoder.decode(&img).into_iter().next().unwrap().unwrap();
        writeln!(writer, "{}", res).unwrap();
        writer.flush().unwrap();
        println!("{}", res);
    }
}
```

### Python

> [!WARNING]
> `pyzbar` имеет трудности со столь маленькими QR кодами. Решается простым увеличением картинки
> через `PIL`.

---

> [!WARNING]
> `pyzbar` has trouble with such small QR codes. It can be solved by simply scaling the image via
> `PIL`.

---

```python
import base64
from PIL import Image
from pyzbar.pyzbar import decode

sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
sock.connect(("ctf.mf.grsu.by", 9011))
reader = sock.makefile("r", encoding="utf-8")
writer = sock.makefile("w", encoding="utf-8")

for _ in range(1):
    read_line(reader)
for _ in range(60 + 1):
    for _ in range(3):
        read_line(reader)
    text = read_line(reader)
    image_data = base64.b64decode(text)
    with open("tmp.png", "wb") as f:
        f.write(image_data)
    img = Image.open("tmp.png")
    img = img.resize((img.size[0] * 5, img.size[1] * 5), Image.NEAREST)
    res = decode(img)[0].data.decode("utf-8")
    writer.write(f"{res}\n")
    writer.flush()
    print(res)
```
