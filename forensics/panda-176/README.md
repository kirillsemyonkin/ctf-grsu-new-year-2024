# Panda

> Xотел передать сообщение своему другу, но так, чтоб никто не догадался. Закинул его в архив, но забыл пароль...
> *(Xотел тут начинается с латинской икс)*

---

> I wanted to convey a message to my friend, but so that no one would guess. I archived it, but forgot the password...

## Решение / Solution

Данный `Panda.zip` файл запаролен. Проведем стандартную процедуру взлома паролей через John The
Ripper:

```bash
$ zip2john Panda.zip > Panda.john
$ john --wordlist=/usr/share/wordlists/rockyou.txt Panda.john
Using default input encoding: UTF-8
Loaded 2 password hashes with 2 different salts (ZIP, WinZip [PBKDF2-SHA1 128/128 SSE2 4x])
Loaded hashes with cost 1 (HMAC size) varying from 77886 to 77889
Will run 2 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
2611             (Panda.zip/panda1.jpg)     
2611             (Panda.zip/panda.jpg)     
2g 0:00:00:27 DONE (2024-01-16 20:20) 0.07145g/s 15950p/s 31901c/s 31901C/s 27032525..20000002
Use the "--show" option to display all of the cracked passwords reliably
Session completed.
```

Нас встречают два `.jpg` файла: один из них нормальный, а второй подкорректирован так, чтобы
картинка все равно открывалась (хоть и не везде). Дальше нужно вывести все байты второго файла,
которые не соответствуют байтам первого. Сделаем это через Rust:

```rust
fn main() {
    let mut f1 = File::open("Panda/panda.jpg").unwrap();
    let mut f2 = File::open("Panda/panda1.jpg").unwrap();
    let mut f3 = File::create("panda1.out").unwrap();
    let mut buf = [0u8; 1];
    while f1.read(&mut buf).unwrap() > 0 {
        let a = buf[0];
        f2.read_exact(&mut buf).unwrap();
        if a != buf[0] {
            f3.write_all(&buf).unwrap();
        }
    }
}
```

Флаг:

```plain
grodno{kung_fu_p4nd4}
```

---

Given `Panda.zip` file is password-protected. Let's do the standard password picking procedure with
John The Ripper:

```plain
$ zip2john Panda.zip > Panda.john
$ john --wordlist=/usr/share/wordlists/rockyou.txt Panda.john
Using default input encoding: UTF-8
Loaded 2 password hashes with 2 different salts (ZIP, WinZip [PBKDF2-SHA1 128/128 SSE2 4x])
Loaded hashes with cost 1 (HMAC size) varying from 77886 to 77889
Will run 2 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
2611             (Panda.zip/panda1.jpg)     
2611             (Panda.zip/panda.jpg)     
2g 0:00:00:27 DONE (2024-01-16 20:20) 0.07145g/s 15950p/s 31901c/s 31901C/s 27032525..20000002
Use the "--show" option to display all of the cracked passwords reliably
Session completed.
```

We are greeted with two `.jpg` files: one is normal, the other one is slightly modified in a way
that it still opens (although not everywhere). Now we need to output all bytes from the second file
that do not correspond to the bytes from the first one. Let's do it with Rust:

```rust
fn main() {
    let mut f1 = File::open("Panda/panda.jpg").unwrap();
    let mut f2 = File::open("Panda/panda1.jpg").unwrap();
    let mut f3 = File::create("panda1.out").unwrap();
    let mut buf = [0u8; 1];
    while f1.read(&mut buf).unwrap() > 0 {
        let a = buf[0];
        f2.read_exact(&mut buf).unwrap();
        if a != buf[0] {
            f3.write_all(&buf).unwrap();
        }
    }
}
```

Flag:

```plain
grodno{kung_fu_p4nd4}
```
