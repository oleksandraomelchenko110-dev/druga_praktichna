# Практична робота № 2

**Дисципліна:** Основи побудови інформаційних систем та мереж (ОК-13)

**Тема:** Структура повідомлень прикладного протоколу HTTP. Формування запиту вручну

| Поле | Значення |
|---|---|
| Студент (прізвище, ім'я, по батькові) | Омельченко Олександра Віталіївна |
| Група | ІПЗ 2.01 |
| Номер варіанта | 26 |
| Індивідуальний домен | iana.org |
| «Чужий» домен для завдання A.3.1 (варіант ± 20) | w3.org |
| Середовище виконання | Windows 10 |
| Дата виконання | 06.10.2026 |

> Бланк заповнюють, не змінюючи структури розділів. Порожні заготовки блоків коду замінюють власними виводами. Позначки-підказки в кутових дужках вилучають.

---

## Частина A. Збір експериментальних даних

### Завдання A.1. Формування запиту вручну

**Команда:**

```
$d = "iana.org"
$c = New-Object System.Net.Sockets.TcpClient($d, 80)
$s = $c.GetStream()
$w = New-Object System.IO.StreamWriter($s)
$w.Write("GET / HTTP/1.1`r`nHost: $d`r`nConnection: close`r`n`r`n")
$w.Flush()
(New-Object System.IO.StreamReader($s)).ReadToEnd()
$c.Close()
```

**Набраний запит:**

Запит у PowerShell формується у змінній та виглядає так:
```
$w.Write("GET / HTTP/1.1`r`nHost: $d`r`nConnection: close`r`n`r`n")
```

**Відповідь:**

```
HTTP/1.1 301 Moved Permanently
Date: Tue, 06 Oct 2026 14:54:54 GMT
Server: Apache
Location: https://www.iana.org/
Cache-Control: max-age=345600
Expires: Sat, 10 Oct 2026 14:54:54 GMT
Content-Length: 229
Connection: close
Content-Type: text/html; charset=iso-8859-1

<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
<title>301 Moved Permanently</title>
</head><body>
<h1>Moved Permanently</h1>
<p>The document has moved <a href="https://www.iana.org/">here</a>.</p>
</body></html>
```

---

### Завдання A.2. Запит без поля `Host` у версії 1.1

**Команда:**

```
$d = "iana.org"
$c = New-Object System.Net.Sockets.TcpClient($d, 80)
$s = $c.GetStream()
$w = New-Object System.IO.StreamWriter($s)
$w.Write("GET / HTTP/1.1`r`nConnection: close`r`n`r`n")
$w.Flush()
(New-Object System.IO.StreamReader($s)).ReadToEnd()
$c.Close()
```

**Вивід:**

```
HTTP/1.1 400 Bad Request
Date: Tue, 06 Oct 2026 15:14:07 GMT
Server: Apache
Content-Length: 347
Connection: close
Content-Type: text/html; charset=iso-8859-1

<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
<title>400 Bad Request</title>
</head><body>
<h1>Bad Request</h1>
<p>Your browser sent a request that this server could not understand.<br />
</p>
<p>Additionally, a 400 Bad Request
error was encountered while trying to use an ErrorDocument to handle the request.</p>
</body></html>

PS C:\Users\my comp>
```

---

### Завдання A.3. Вплив поля `Host` на відповідь сервера

#### A.3.1. Чуже доменне ім'я в полі `Host`

**Команда:**

```
$d = "iana.org"
$c = New-Object System.Net.Sockets.TcpClient($d, 80)
$s = $c.GetStream()
$w = New-Object System.IO.StreamWriter($s)
$w.Write("GET / HTTP/1.1`r`nHost: w3.org`r`nConnection: close`r`n`r`n")
$w.Flush()
(New-Object System.IO.StreamReader($s)).ReadToEnd()
$c.Close()
```

**Вивід:**

```
HTTP/1.1 302 Found
Date: Tue, 06 Oct 2026 15:15:48 GMT
Server: Apache
Location: https://www.iana.org/
Cache-Control: max-age=345600
Expires: Sat, 10 Oct 2026 15:15:48 GMT
Content-Length: 205
Connection: close
Content-Type: text/html; charset=iso-8859-1

<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
<title>302 Found</title>
</head><body>
<h1>Found</h1>
<p>The document has moved <a href="https://www.iana.org/">here</a>.</p>
</body></html>

PS C:\Users\my comp>
```

#### A.3.2. Неіснуюче ім'я в полі `Host`

**Команда:**

```
$d = "iana.org"
$c = New-Object System.Net.Sockets.TcpClient($d, 80)
$s = $c.GetStream()
$w = New-Object System.IO.StreamWriter($s)
$w.Write("GET / HTTP/1.1`r`nHost: opism-pr02.invalid`r`nConnection: close`r`n`r`n")
$w.Flush()
(New-Object System.IO.StreamReader($s)).ReadToEnd()
$c.Close()
```

**Вивід:**

```
HTTP/1.1 302 Found
Date: Tue, 06 Oct 2026 15:18:11 GMT
Server: Apache
Location: https://www.iana.org/
Cache-Control: max-age=345600
Expires: Sat, 10 Oct 2026 15:18:11 GMT
Content-Length: 205
Connection: close
Content-Type: text/html; charset=iso-8859-1

<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
<title>302 Found</title>
</head><body>
<h1>Found</h1>
<p>The document has moved <a href="https://www.iana.org/">here</a>.</p>
</body></html>

PS C:\Users\my comp>
```

#### A.3.3. Запит без поля `Host` у версії 1.0

**Команда:**

```
$d = "iana.org"
$c = New-Object System.Net.Sockets.TcpClient($d, 80)
$s = $c.GetStream()
$w = New-Object System.IO.StreamWriter($s)
$w.Write("GET / HTTP/1.0`r`n`r`n")
$w.Flush()
(New-Object System.IO.StreamReader($s)).ReadToEnd()
$c.Close()
```

**Вивід:**

```
HTTP/1.1 302 Found
Date: Tue, 06 Oct 2026 15:20:48 GMT
Server: Apache
Location: https://www.iana.org/
Cache-Control: max-age=345600
Expires: Sat, 10 Oct 2026 15:20:48 GMT
Content-Length: 205
Connection: close
Content-Type: text/html; charset=iso-8859-1

<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
<title>302 Found</title>
</head><body>
<h1>Found</h1>
<p>The document has moved <a href="https://www.iana.org/">here</a>.</p>
</body></html>

PS C:\Users\my comp>
```

Зведення результатів наведено в **Додатку Д**.

---

### Завдання A.4. Два запити в одному з'єднанні

**Команда:**

```
$d = "iana.org"
$c = New-Object System.Net.Sockets.TcpClient($d, 80)
$s = $c.GetStream()
$w = New-Object System.IO.StreamWriter($s)
$w.Write("GET /opism-pr02-12345 HTTP/1.1`r`nHost: $d`r`n`r`nGET / HTTP/1.1`r`nHost: $d`r`nConnection: close`r`n`r`n")
$w.Flush()
(New-Object System.IO.StreamReader($s)).ReadToEnd()
$c.Close()
```

**Вивід:**

```
HTTP/1.1 301 Moved Permanently
Date: Tue, 06 Oct 2026 15:28:26 GMT
Server: Apache
Location: https://www.iana.org/opism-pr02-12345
Cache-Control: max-age=345600
Expires: Sat, 10 Oct 2026 15:28:26 GMT
Content-Length: 245
Content-Type: text/html; charset=iso-8859-1

<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
<title>301 Moved Permanently</title>
</head><body>
<h1>Moved Permanently</h1>
<p>The document has moved <a href="https://www.iana.org/opism-pr02-12345">here</a>.</p>
</body></html>
HTTP/1.1 301 Moved Permanently
Date: Tue, 06 Oct 2026 15:28:26 GMT
Server: Apache
Location: https://www.iana.org/
Cache-Control: max-age=345600
Expires: Sat, 10 Oct 2026 15:28:26 GMT
Content-Length: 229
Connection: close
Content-Type: text/html; charset=iso-8859-1

<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
<title>301 Moved Permanently</title>
</head><body>
<h1>Moved Permanently</h1>
<p>The document has moved <a href="https://www.iana.org/">here</a>.</p>
</body></html>

PS C:\Users\my comp>
```

**Кількість отриманих відповідей:** 2

**Коди стану отриманих відповідей:** Оба 301 Moved Permanently

---

### Завдання A.5. Запит за допомогою клієнтської програми

**Команда:**

```
curl.exe -v --http1.1 http://iana.org/ -o /dev/null
```

**Вивід:**

```
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0* Host iana.org:80 was resolved.
* IPv6: (none)
* IPv4: 192.0.43.8
*   Trying 192.0.43.8:80...
* Connected to iana.org (192.0.43.8) port 80
* using HTTP/1.x
> GET / HTTP/1.1
> Host: iana.org
> User-Agent: curl/8.13.0
> Accept: */*
>
* Request completely sent off
< HTTP/1.1 301 Moved Permanently
< Date: Tue, 06 Oct 2026 15:34:11 GMT
< Server: Apache
< Location: https://www.iana.org/
< Cache-Control: max-age=345600
< Expires: Sat, 10 Oct 2026 15:34:11 GMT
< Content-Length: 229
< Content-Type: text/html; charset=iso-8859-1
<
{ [229 bytes data]
Warning: Failed to open the file /dev/null: No such file or directory
* client returned ERROR on write of 229 bytes
  0   229    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
* closing connection #0
curl: (23) client returned ERROR on write of 229 bytes
PS C:\Users\my comp>
```

---

### Завдання A.6. Запит через захищене з'єднання

**Ресурс, на якому виконано завдання:** <власний домен / `iana.org`>

**Підстава для використання резервного ресурсу (заповнюють за потреби):**

**Команда:**

```
openssl s_client -connect iana.org:443 -servername iana.org -crlf -quiet 
```

**Набраний запит:**

```
GET / HTTP/1.1
Host: iana.org
Connection: close
```

**Вивід:**

```
HTTP/1.1 400 Bad Request
Date: Tue, 06 Oct 2026 15:39:12 GMT
Server: Apache
Content-Length: 347
Connection: close
Content-Type: text/html; charset=iso-8859-1

<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
<title>400 Bad Request</title>
</head><body>
<h1>Bad Request</h1>
<p>Your browser sent a request that this server could not understand.<br />
</p>
<p>Additionally, a 400 Bad Request
error was encountered while trying to use an ErrorDocument to handle the request.</p>
</body></html>
F8240000:error:0A000126:SSL routines::unexpected eof while reading:../openssl-3.5.2/ssl/record/rec_layer_s3.c:696:
PS C:\Users\my comp>
```

---

## Частина B. Розбір полів заголовка

Розбирається відповідь, отримана в завданні A.1.

**Загальна кількість полів заголовка у відповіді:**

| № | Поле заголовка | Значення | Призначення (власне формулювання) | Походження: сервер / проміжний вузол / не визначено | Обґрунтування |
|---|---|---|---|---|---|
| 1 | Date| Tue, 06 Oct 2026 15:41:33 GMT | Дата та час відповіді сервера | Сервер | Створюється самостійно сервером |
| 2 | Server | Apache | Забезпечення яке обробило запит | Сервер| Вказує на версію сервера |
| 3 | Location |  https://www.iana.org/ | Вказує адресу, на яку потрібно перейти після перенаправлення | Сервер | Визначає нове місце ресурсу, а статус 301 означає постійне перенаправлення |
| 4 | Cache-Control | max-age=345600 | Час допустимого зберігання в кеші | Сервер | Містить параметри кешування відповіді |
| 5 | Expires | Sat, 10 Oct 2026 15:41:33 GMT | Час, після якого відповідь вважається застарілою для кешу | Сервер | Значення задає час закінчення актуальності відповіді |
| 6 | Content-Length | 229 | Розмір відповіді в байтах | Сервер | Сам сервер визначає розмір |
| 7 | Connection | close | Повідомляє, що з'єднання буде закрито після завершення передачі відповіді | Сервер | Задає спосіб завершення поточного з'єднання |
| 8 | Content-Type | text/html; charset=iso-8859-1 | Вказує тип переданого вмісту та його кодування | Сервер | Описує формат HTML-документа і використане кодування символів |

> Рядок наводять на кожне поле, яке реально надійшло. Зайві рядки вилучають, за потреби додають нові. Поле, походження якого встановити не вдалося, зазначають із позначкою «не визначено» та поясненням утруднення.

---

## Частина D. Висновки

Обсяг — 150–300 слів. Висновки спираються на власні спостереження.

**D.1.** Що з поведінки сервера виявилося неочевидним або несподіваним. Конкретно, з посиланням на рядок виводу.
То що при отриманні 301 Moved Permanently, сервер усе одно виписував Content-Length навіть зі значенням. 

**D.2.** Яке з полів заголовка викликало найбільше утруднення при визначенні походження (частина B) та з якої причини.
Не мала з цим труднощів. 

**D.3.** Яке питання залишилося без відповіді після виконання роботи.
Не маю таких питань.

---

## Контрольні питання

**1.** У завданні A.1 сервер не надсилав відповіді, доки не було введено порожній рядок. Чим це зумовлено?

Щоб сервер зрозумів, що більше полів не буде. Якщо цього не зробити, сервер подовже чекати продовження. 

**2.** Порівняйте результати завдань A.1, A.2 та A.3.1–A.3.3 (таблиця Додатка Д). За яких значень поля `Host` і за якої версії протоколу сервер обслуговує запит, а за яких — ні? Яку задачу розв'язує поле `Host`? Відповідь має посилатися на конкретні рядки ваших виводів.

<відповідь>

**3.** Скільки відповідей надійшло у завданні A.4 і з якими кодами стану? Чи залежить відповідь сервера на порту 80 від запитаного шляху — і що це говорить про роль цього сервера? Якщо надійшла одна відповідь, знайдіть у ній поле заголовка, яке це пояснює, або зазначте, що такого поля немає. Якщо надійшло дві — що це означає для клієнтської програми, яка завантажує сторінку з великою кількістю вкладених ресурсів?

2 відповіді і обідві з 301 Moved Permanently. 

**4.** Які поля заголовка програма `curl` додала самостійно (завдання A.5)? Ці поля не є обов'язковими — сервер відповів і без них у завданні A.1. З якою метою їх додано?

<відповідь>

**5.** За якими ознаками у вашому виводі виявляється присутність проміжного вузла? Якщо таких ознак не виявлено, поясніть, що з цього випливає.

<відповідь>

**6.** Три рядки, про які не йшлося на лекції, наведено в **Додатку В**.

---

## Додаток В. Відповіді на питання 6

| № | Рядок виводу | Джерело (номер завдання) |
|---|---|---|
| 1 | Cache-Control: max-age=345600 | A.1 |
| 2 | | |
| 3 | | |

---

## Додаток Д. Зведення результатів завдання A.3

**Вузол, з яким установлювалося з'єднання (у всіх пробах однаковий):**

| Проба | Значення поля `Host` | Версія | Код стану | Обсяг тіла відповіді | Збігається з A.1 (так / ні) |
|---|---|---|---|---|---|
| A.1 (вихідна) | iana.org | 1.1 | 301 | 229 | — |
| A.2 | поле відсутнє | 1.1 | 400 | 347 | ні |
| A.3.1 | w3.org | 1.1 | 400 | 347 | ні |
| A.3.2 | `opism-pr02.invalid` | 1.1 | 302 | 205 | ні |
| A.3.3 | поле відсутнє | 1.0 | 302 | 205 | ні |

**Висновок за таблицею (2–4 речення):** що саме змінювалося у запиті від проби до проби і як на це реагував сервер.

<текст>

---

## Декларування використання технологій штучного інтелекту

Для цієї роботи встановлено **рівень Р3 — ШІ як співвиконавець**.

Виводи команд частини A не можуть бути згенеровані та мають бути отримані внаслідок фактичного виконання команд.

**Чи використовувалися технології ШІ під час виконання роботи:** ні

---

*ОПІСМ (ОК-13) · Практична робота № 2 · бланк звіту*
