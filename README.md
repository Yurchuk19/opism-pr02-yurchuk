# Практична робота № 2

**Дисципліна:** Основи побудови інформаційних систем та мереж (ОК-13)

**Тема:** Структура повідомлень прикладного протоколу HTTP. Формування запиту вручну

| Поле | Значення |
|---|---|
| Студент | Юрчук Дмитро|
| Група | ЗКІ-21 |
| Номер варіанта | 1 |
| Індивідуальний домен | `fsf.org` |
| «Чужий» домен для завдання A.3.1 (варіант ± 20) | `arin.net` (варіант 16) |
| Середовище виконання | Windows 11, OpenSSL |
| Дата виконання | 2026-11-24 |

> Бланк заповнюють, не змінюючи структури розділів. Порожні заготовки блоків коду замінюють власними виводами. Позначки-підказки в кутових дужках вилучають.

---

## Частина A. Збір експериментальних даних

### Завдання A.1. Формування запиту вручну

**Команда:**

```powershell
$d = "fsf.org"
$c = New-Object System.Net.Sockets.TcpClient($d, 80)
$s = $c.GetStream()
$w = New-Object System.IO.StreamWriter($s)
$w.Write("GET / HTTP/1.1`r`nHost: $d`r`nConnection: close`r`n`r`n")
$w.Flush()
(New-Object System.IO.StreamReader($s)).ReadToEnd()
$c.Close()
```

*(виконано скриптом `pz2.ps1`, який відправляє той самий запит через `TcpClient` згідно з Додатком Г; вивід збережено в `raw/`)*

**Набраний запит:**

```
GET / HTTP/1.1
Host: fsf.org
Connection: close

```

*(у Windows запит не набирається з клавіатури, а передається одним рядком із `
`, див. Додаток Г)*

**Відповідь:**

```
HTTP/1.0 301 Moved Permanently
Server: nginx
Date: Tue, 24 Nov 2026 17:37:54 GMT
Content-Type: text/html
Content-Length: 178
Location: https://www.fsf.org/
X-Cache: MISS from www.fsf.org
X-Cache-Lookup: MISS from www.fsf.org:3128
Via: 1.0 www.fsf.org (squid/3.1.19)
Connection: close

<html>
<head><title>301 Moved Permanently</title></head>
<body bgcolor="white">
<center><h1>301 Moved Permanently</h1></center>
<hr><center>nginx</center>
</body>
</html>
```

---

### Завдання A.2. Запит без поля `Host` у версії 1.1

**Команда:**

```powershell
$d = "fsf.org"
$c = New-Object System.Net.Sockets.TcpClient($d, 80)
$s = $c.GetStream()
$w = New-Object System.IO.StreamWriter($s)
$w.Write("GET / HTTP/1.1`r`nConnection: close`r`n`r`n")
$w.Flush()
(New-Object System.IO.StreamReader($s)).ReadToEnd()
$c.Close()
```

*(виконано скриптом `pz2.ps1`, який відправляє той самий запит через `TcpClient` згідно з Додатком Г; вивід збережено в `raw/`)*

**Вивід:**

```
HTTP/1.0 200 OK
Pragma: no-cache
Connection: close
Content-Type: text/html; charset=iso-8859-1
Content-Length: 61

<html><body>ERROR ERR_INVALID_REQ<hr>Not Found</body></html>
```

---

### Завдання A.3. Вплив поля `Host` на відповідь сервера

#### A.3.1. Чуже доменне ім'я в полі `Host`

**Команда:**

```powershell
$d = "fsf.org"
$c = New-Object System.Net.Sockets.TcpClient($d, 80)
$s = $c.GetStream()
$w = New-Object System.IO.StreamWriter($s)
$w.Write("GET / HTTP/1.1`r`nHost: arin.net`r`nConnection: close`r`n`r`n")
$w.Flush()
(New-Object System.IO.StreamReader($s)).ReadToEnd()
$c.Close()
```

*(виконано скриптом `pz2.ps1`, який відправляє той самий запит через `TcpClient` згідно з Додатком Г; вивід збережено в `raw/`)*

**Вивід:**

```
HTTP/1.0 503 Service Unavailable
Server: squid/3.1.19
Mime-Version: 1.0
Date: Tue, 24 Nov 2026 17:37:54 GMT
Content-Type: text/html
Content-Length: 3242
X-Squid-Error: ERR_CANNOT_FORWARD 0
Vary: Accept-Language
Content-Language: en
X-Cache: MISS from www.fsf.org
X-Cache-Lookup: MISS from www.fsf.org:3128
Via: 1.0 www.fsf.org (squid/3.1.19)
Connection: close

<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
<html><head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8">
<title>ERROR: The requested URL could not be retrieved</title>
<style type="text/css"><!-- 
 /*
 Stylesheet for Squid Error pages
 Adapted from design by Free CSS Templates
 http://www.freecsstemplates.org
 Released for free under a Creative Commons Attribution 2.5 License
*/

/* Page basics */
* {
	font-family: verdana, sans-serif;
}

html body {
	margin: 0;
	padding: 0;
	background: #efefef;
	font-size: 12px;
	color: #1e1e1e;
}

/* Page displayed title area */
#titles {
	margin-left: 15px;
	padding: 10px;
	padding-left: 100px;
	background: url('http://www.squid-cache.org/Artwork/SN.png') no-repeat left;
}

/* initial title */
#titles h1 {
	color: #000000;
}
#titles h2 {
	color: #000000;
}

/* special event: FTP success page titles */
#titles ftpsuccess {
	background-color:#00ff00;
	width:100%;
}

/* Page displayed body content area */
#content {
	padding: 10px;
	background: #ffffff;
}

/* General text */
p {
}

/* error brief description */
#error p {
}

/* some data which may have caused the problem */
#data {
}

/* the error message received from the system or other software */
#sysmsg {
}

pre {
    font-family:sans-serif;
}

/* special event: FTP / Gopher directory listing */
#dirmsg {
    font-family: courier;
    color: black;
    font-size: 10pt;
}
#dirlisting {
    margin-left: 2%;
    margin-right: 2%;
}
#dirlisting tr.entry td.icon,td.filename,td.size,td.date {
    border-bottom: groove;
}
#dirlisting td.size {
    width: 50px;
    text-align: right;
    padding-right: 5px;
}

/* horizontal lines */
hr {
	margin: 0;
}

/* page displayed footer area */
#footer {
	font-size: 9px;
	padding-left: 10px;
}


body
:lang(fa) { direction: rtl; font-size: 100%; font-family: Tahoma, Roya, sans-serif; float: right; }
:lang(he) { direction: rtl; }
 --></style>
</head><body id=ERR_CANNOT_FORWARD>
<div id="titles">
<h1>ERROR</h1>
<h2>The requested URL could not be retrieved</h2>
</div>
<hr>

<div id="content">
<p>The following error was encountered while trying to retrieve the URL: <a href="http://arin.net/">http://arin.net/</a></p>

<blockquote id="error">
<p><b>Unable to forward this request at this time.</b></p>
</blockquote>

<p>This request could not be forwarded to the origin server or to any parent caches. The most likely cause for this error is that the cache administrator does not allow this cache to make direct connections to origin servers, and all configured parent caches are currently unreachable.</p>

<p>Your cache administrator is <a href="mailto:webmaster?subject=CacheErrorInfo%20-%20ERR_CANNOT_FORWARD&amp;body=CacheHost%3A%20www.fsf.org%0D%0AErrPage%3A%20ERR_CANNOT_FORWARD%0D%0AErr%3A%20%5Bnone%5D%0D%0ATimeStamp%3A%20Tue,%2024%20Nov%202026%2017%3A37%3A54%20GMT%0D%0A%0D%0AClientIP%3A%2046.114.111.226%0D%0A%0D%0AHTTP%20Request%3A%0D%0AGET%20%2F%20HTTP%2F1.1%0AHost%3A%20arin.net%0D%0AConnection%3A%20close%0D%0A%0D%0A%0D%0A">webmaster</a>.</p>

<br>
</div>

<hr>
<div id="footer">
<p>Generated Tue, 24 Nov 2026 17:37:54 GMT by www.fsf.org (squid/3.1.19)</p>
<!-- ERR_CANNOT_FORWARD -->
</div>
</body></html>
```

#### A.3.2. Неіснуюче ім'я в полі `Host`

**Команда:**

```powershell
$d = "fsf.org"
$c = New-Object System.Net.Sockets.TcpClient($d, 80)
$s = $c.GetStream()
$w = New-Object System.IO.StreamWriter($s)
$w.Write("GET / HTTP/1.1`r`nHost: opism-pr02.invalid`r`nConnection: close`r`n`r`n")
$w.Flush()
(New-Object System.IO.StreamReader($s)).ReadToEnd()
$c.Close()
```

*(виконано скриптом `pz2.ps1`, який відправляє той самий запит через `TcpClient` згідно з Додатком Г; вивід збережено в `raw/`)*

**Вивід:**

```
HTTP/1.0 503 Service Unavailable
Server: squid/3.1.19
Mime-Version: 1.0
Date: Tue, 24 Nov 2026 17:37:55 GMT
Content-Type: text/html
Content-Length: 3272
X-Squid-Error: ERR_CANNOT_FORWARD 0
Vary: Accept-Language
Content-Language: en
X-Cache: MISS from www.fsf.org
X-Cache-Lookup: MISS from www.fsf.org:3128
Via: 1.0 www.fsf.org (squid/3.1.19)
Connection: close

<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
<html><head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8">
<title>ERROR: The requested URL could not be retrieved</title>
<style type="text/css"><!-- 
 /*
 Stylesheet for Squid Error pages
 Adapted from design by Free CSS Templates
 http://www.freecsstemplates.org
 Released for free under a Creative Commons Attribution 2.5 License
*/

/* Page basics */
* {
	font-family: verdana, sans-serif;
}

html body {
	margin: 0;
	padding: 0;
	background: #efefef;
	font-size: 12px;
	color: #1e1e1e;
}

/* Page displayed title area */
#titles {
	margin-left: 15px;
	padding: 10px;
	padding-left: 100px;
	background: url('http://www.squid-cache.org/Artwork/SN.png') no-repeat left;
}

/* initial title */
#titles h1 {
	color: #000000;
}
#titles h2 {
	color: #000000;
}

/* special event: FTP success page titles */
#titles ftpsuccess {
	background-color:#00ff00;
	width:100%;
}

/* Page displayed body content area */
#content {
	padding: 10px;
	background: #ffffff;
}

/* General text */
p {
}

/* error brief description */
#error p {
}

/* some data which may have caused the problem */
#data {
}

/* the error message received from the system or other software */
#sysmsg {
}

pre {
    font-family:sans-serif;
}

/* special event: FTP / Gopher directory listing */
#dirmsg {
    font-family: courier;
    color: black;
    font-size: 10pt;
}
#dirlisting {
    margin-left: 2%;
    margin-right: 2%;
}
#dirlisting tr.entry td.icon,td.filename,td.size,td.date {
    border-bottom: groove;
}
#dirlisting td.size {
    width: 50px;
    text-align: right;
    padding-right: 5px;
}

/* horizontal lines */
hr {
	margin: 0;
}

/* page displayed footer area */
#footer {
	font-size: 9px;
	padding-left: 10px;
}


body
:lang(fa) { direction: rtl; font-size: 100%; font-family: Tahoma, Roya, sans-serif; float: right; }
:lang(he) { direction: rtl; }
 --></style>
</head><body id=ERR_CANNOT_FORWARD>
<div id="titles">
<h1>ERROR</h1>
<h2>The requested URL could not be retrieved</h2>
</div>
<hr>

<div id="content">
<p>The following error was encountered while trying to retrieve the URL: <a href="http://opism-pr02.invalid/">http://opism-pr02.invalid/</a></p>

<blockquote id="error">
<p><b>Unable to forward this request at this time.</b></p>
</blockquote>

<p>This request could not be forwarded to the origin server or to any parent caches. The most likely cause for this error is that the cache administrator does not allow this cache to make direct connections to origin servers, and all configured parent caches are currently unreachable.</p>

<p>Your cache administrator is <a href="mailto:webmaster?subject=CacheErrorInfo%20-%20ERR_CANNOT_FORWARD&amp;body=CacheHost%3A%20www.fsf.org%0D%0AErrPage%3A%20ERR_CANNOT_FORWARD%0D%0AErr%3A%20%5Bnone%5D%0D%0ATimeStamp%3A%20Tue,%2024%20Nov%202026%2017%3A37%3A55%20GMT%0D%0A%0D%0AClientIP%3A%2046.114.111.226%0D%0A%0D%0AHTTP%20Request%3A%0D%0AGET%20%2F%20HTTP%2F1.1%0AHost%3A%20opism-pr02.invalid%0D%0AConnection%3A%20close%0D%0A%0D%0A%0D%0A">webmaster</a>.</p>

<br>
</div>

<hr>
<div id="footer">
<p>Generated Tue, 24 Nov 2026 17:37:55 GMT by www.fsf.org (squid/3.1.19)</p>
<!-- ERR_CANNOT_FORWARD -->
</div>
</body></html>
```

#### A.3.3. Запит без поля `Host` у версії 1.0

**Команда:**

```powershell
$d = "fsf.org"
$c = New-Object System.Net.Sockets.TcpClient($d, 80)
$s = $c.GetStream()
$w = New-Object System.IO.StreamWriter($s)
$w.Write("GET / HTTP/1.0`r`n`r`n")
$w.Flush()
(New-Object System.IO.StreamReader($s)).ReadToEnd()
$c.Close()
```

*(виконано скриптом `pz2.ps1`, який відправляє той самий запит через `TcpClient` згідно з Додатком Г; вивід збережено в `raw/`)*

**Вивід:**

```
HTTP/1.0 200 OK
Pragma: no-cache
Content-Type: text/html; charset=iso-8859-1
Content-Length: 61

<html><body>ERROR ERR_INVALID_REQ<hr>Not Found</body></html>
```

Зведення результатів наведено в **Додатку Д**.

---

### Завдання A.4. Два запити в одному з'єднанні

**Команда:**

```powershell
$d = "fsf.org"
$c = New-Object System.Net.Sockets.TcpClient($d, 80)
$s = $c.GetStream()
$w = New-Object System.IO.StreamWriter($s)
$w.Write("GET /opism-pr02-12345 HTTP/1.1`r`nHost: $d`r`n`r`nGET / HTTP/1.1`r`nHost: $d`r`nConnection: close`r`n`r`n")
$w.Flush()
(New-Object System.IO.StreamReader($s)).ReadToEnd()
$c.Close()
```

*(виконано скриптом `pz2.ps1`, який відправляє той самий запит через `TcpClient` згідно з Додатком Г; вивід збережено в `raw/`)*

**Вивід:**

```
HTTP/1.0 301 Moved Permanently
Server: nginx
Date: Tue, 24 Nov 2026 17:37:55 GMT
Content-Type: text/html
Content-Length: 178
Location: https://www.fsf.org/opism-pr02-12345
X-Cache: MISS from www.fsf.org
X-Cache-Lookup: MISS from www.fsf.org:3128
Via: 1.0 www.fsf.org (squid/3.1.19)
Connection: keep-alive

<html>
<head><title>301 Moved Permanently</title></head>
<body bgcolor="white">
<center><h1>301 Moved Permanently</h1></center>
<hr><center>nginx</center>
</body>
</html>
HTTP/1.0 301 Moved Permanently
Server: nginx
Date: Tue, 24 Nov 2026 17:37:55 GMT
Content-Type: text/html
Content-Length: 178
Location: https://www.fsf.org/
X-Cache: MISS from www.fsf.org
X-Cache-Lookup: MISS from www.fsf.org:3128
Via: 1.0 www.fsf.org (squid/3.1.19)
Connection: close

<html>
<head><title>301 Moved Permanently</title></head>
<body bgcolor="white">
<center><h1>301 Moved Permanently</h1></center>
<hr><center>nginx</center>
</body>
</html>
```

**Кількість отриманих відповідей:** 2

**Коди стану отриманих відповідей:** 301, 301

---

### Завдання A.5. Запит за допомогою клієнтської програми

**Команда:**

```
curl.exe -v --http1.1 http://fsf.org/ -o NUL
```

*(у Windows замість `/dev/null` використано `NUL`)*

**Вивід:**

```
* Host fsf.org:80 was resolved.
* IPv6: (none)
* IPv4: 209.51.188.174
*   Trying 209.51.188.174:80...
* Established connection to fsf.org (209.51.188.174 port 80) from 192.168.179.204 port 52874 
  % Total    % Received % Xferd  Average Speed  Time    Time    Time   Current
                                 Dload  Upload  Total   Spent   Left   Speed

  0      0   0      0   0      0      0      0                              0* using HTTP/1.x
> GET / HTTP/1.1

> Host: fsf.org

> User-Agent: curl/8.22.0

> Accept: */*

> 

* Request completely sent off
* HTTP 1.0, assume close after body
< HTTP/1.0 301 Moved Permanently

< Server: nginx

< Date: Tue, 24 Nov 2026 17:37:55 GMT

< Content-Type: text/html

< Content-Length: 178

< Location: https://www.fsf.org/

< X-Cache: MISS from www.fsf.org

< X-Cache-Lookup: MISS from www.fsf.org:3128

< Via: 1.0 www.fsf.org (squid/3.1.19)

* HTTP/1.0 connection set to keep alive
< Connection: keep-alive

< 

{ [178 bytes data]

100    178 100    178   0      0    629      0                              0
100    178 100    178   0      0    629      0                              0
100    178 100    178   0      0    628      0                              0
* Connection #0 to host fsf.org:80 left intact
```

---

### Завдання A.6. Запит через захищене з'єднання

**Ресурс, на якому виконано завдання:** власний домен `fsf.org`

**Підстава для використання резервного ресурсу (заповнюють за потреби):**

**Команда:**

```
openssl s_client -connect fsf.org:443 -servername fsf.org -crlf -quiet
```

**Набраний запит:**

```
GET / HTTP/1.1
Host: fsf.org
Connection: close

```

*(запит подано у stdin `openssl s_client`, а не набрано вручну)*

**Вивід:**

```
Connecting to 209.51.188.174
depth=3 C=US, O=Internet Security Research Group, CN=ISRG Root X1
verify return:1
depth=2 C=US, O=ISRG, CN=Root YR
verify return:1
depth=1 C=US, O=Let's Encrypt, CN=YR2
verify return:1
depth=0 CN=*.fsf.org
verify return:1
HTTP/1.0 301 Moved Permanently
Server: nginx
Date: Tue, 24 Nov 2026 17:40:50 GMT
Content-Type: text/html
Content-Length: 178
Location: https://www.fsf.org/
X-Cache: MISS from www.fsf.org
X-Cache-Lookup: MISS from www.fsf.org:3128
Via: 1.0 www.fsf.org (squid/3.1.19)
Connection: close

<html>
<head><title>301 Moved Permanently</title></head>
<body bgcolor="white">
<center><h1>301 Moved Permanently</h1></center>
<hr><center>nginx</center>
</body>
</html>
```

---

## Частина B. Розбір полів заголовка

Розбирається відповідь, отримана в завданні A.1.

**Загальна кількість полів заголовка у відповіді:** 9

| № | Поле заголовка | Значення | Призначення (власне формулювання) | Походження: сервер / проміжний вузол / не визначено | Обґрунтування |
|---|---|---|---|---|---|
| 1 | `Server` | nginx | назва програми сервера | кінцевий сервер | у тілі відповіді теж підпис nginx, а Squid, коли відповідав сам (A.3.1), писав `Server: squid/3.1.19` |
| 2 | `Date` | Tue, 24 Nov 2026 17:37:54 GMT | дата й час відповіді | не визначено | час могли поставити і nginx, і Squid, а в обох він має однаковий формат, тому з виводу не видно, хто з них |
| 3 | `Content-Type` | text/html | тип вмісту (HTML) | кінцевий сервер | тип описує сам вміст сторінки, а сторінку видав nginx |
| 4 | `Content-Length` | 178 | розмір тіла в байтах | кінцевий сервер | 178 байтів відповідає короткій сторінці переадресації nginx, а в сторінки самого Squid (A.3.1) розмір інший, 3242 |
| 5 | `Location` | https://www.fsf.org/ | куди переадресувати | кінцевий сервер | у A.4 шлях у `Location` повторює запитаний (`/opism-pr02-12345`), тобто адресу будує сайт, а не проксі |
| 6 | `X-Cache` | MISS from www.fsf.org | чи була відповідь у кеші проксі | проміжний вузол | у значенні `MISS from www.fsf.org` видно, що це відмітка кешу, а кеш є тільки в проксі |
| 7 | `X-Cache-Lookup` | MISS from www.fsf.org:3128 | результат пошуку в кеші проксі | проміжний вузол | тут порт 3128, а це стандартний порт Squid |
| 8 | `Via` | 1.0 www.fsf.org (squid/3.1.19) | через який проксі пройшла відповідь | проміжний вузол | прямо написано `squid/3.1.19` |
| 9 | `Connection` | close | що з'єднання закриється після відповіді | не визначено | з'єднання з нами йде через Squid, але `close` міг поставити і nginx, а в A.4 на перший запит було `keep-alive`, а на другий `close`, тож хто саме керує з'єднанням, з виводу не видно |

> Рядок наводять на кожне поле, яке реально надійшло. Зайві рядки вилучають, за потреби додають нові. Поле, походження якого встановити не вдалося, зазначають із позначкою «не визначено» та поясненням утруднення.

---

## Частина D. Висновки

Обсяг — 150–300 слів. Висновки спираються на власні спостереження.

**D.1.** Що з поведінки сервера виявилося неочевидним або несподіваним. Конкретно, з посиланням на рядок виводу.

Я не чекав, що без `Host` (A.2) прийде `HTTP/1.0 200 OK`, а в тілі буде `ERR_INVALID_REQ`: код каже «успіх», а насправді це помилка. Ще одне, що мене здивувало: запит був `HTTP/1.1`, а всі відповіді прийшли як `HTTP/1.0`, бо їх віддавав Squid. І навіть у A.6, усередині захищеного з'єднання, знову є `Via: 1.0 www.fsf.org (squid/3.1.19)`, тобто проксі стоїть і за TLS.

**D.2.** Яке з полів заголовка викликало найбільше утруднення при визначенні походження (частина B) та з якої причини.

Найважче було визначити походження в `Connection`. В A.4 на перший запит прийшло `keep-alive`, а на другий `close`, а з'єднання з нами тримає Squid, тому з виводу не видно, чи це рішення nginx, чи самого проксі. Тому я поставив «не визначено»: щоб знати точно, треба було б побачити відповідь nginx без Squid.

**D.3.** Яке питання залишилося без відповіді після виконання роботи.

Без відповіді залишилося питання, чому Squid на запит без `Host` відповідає кодом `200`, а не `400`, адже запит явно неправильний (A.2, A.3.3). Я не знаю, чи так задумано в Squid 3.1.19, чи це його помилка. Ще не зрозуміло, чи Squid і nginx стоять на одному комп'ютері, бо в `Via` і `X-Cache` обидва рази написано `www.fsf.org`.

---

## Контрольні питання

**1.** У завданні A.1 сервер не надсилав відповіді, доки не було введено порожній рядок. Чим це зумовлено?

Порожній рядок показує серверу, що заголовок запиту закінчився. Полів у заголовку може бути скільки завгодно, тому сервер не знає наперед, коли вони скінчаться, і чекає кінця заголовка. У моєму запиті A.1 в кінці стоїть `

` (два переходи рядка підряд, це видно в `raw/commands.log`), після нього сервер зрозумів, що полів більше не буде, і відповів. У Windows запит надсилався одразу цілим разом із `\r\n\r\n`, тому саме очікування я не бачив, але відповідь прийшла лише на повний запит.

**2.** Порівняйте результати завдань A.1, A.2 та A.3.1–A.3.3 (таблиця Додатка Д). За яких значень поля `Host` і за якої версії протоколу сервер обслуговує запит, а за яких — ні? Яку задачу розв'язує поле `Host`? Відповідь має посилатися на конкретні рядки ваших виводів.

Сервер обслужив запит тільки з `Host: fsf.org` (A.1, відповідь `301`). На `Host: arin.net` і `opism-pr02.invalid` прийшло `503` (`ERR_CANNOT_FORWARD`), а без `Host` і в HTTP/1.1 (A.2), і в HTTP/1.0 (A.3.3) прийшла сторінка помилки `ERR_INVALID_REQ`. Поле `Host` потрібне, бо на одній IP-адресі може працювати багато сайтів, а з'єднання йде тільки на адресу й порт, і сервер дізнається, який саме сайт потрібен клієнту, з імені в `Host`.

**3.** Скільки відповідей надійшло у завданні A.4 і з якими кодами стану? Чи залежить відповідь сервера на порту 80 від запитаного шляху — і що це говорить про роль цього сервера? Якщо надійшла одна відповідь, знайдіть у ній поле заголовка, яке це пояснює, або зазначте, що такого поля немає. Якщо надійшло дві — що це означає для клієнтської програми, яка завантажує сторінку з великою кількістю вкладених ресурсів?

В A.4 прийшли 2 відповіді, обидві з кодом `301`. Перша мала `Location: https://www.fsf.org/opism-pr02-12345` і `Connection: keep-alive`, друга мала `Location: https://www.fsf.org/` і `Connection: close`. Відповідь на порту 80 від шляху не залежить: навіть на неіснуючий шлях не було `404`, сервер просто переадресував на той самий шлях через `https`, тобто його роль на порту 80 тільки перекинути клієнта на захищену версію сайту. Дві відповіді означають, що в одному з'єднанні можна надіслати кілька запитів (`keep-alive`), і для програми, яка завантажує сторінку з багатьма ресурсами, це зручно, бо не треба щоразу відкривати нове з'єднання.

**4.** Які поля заголовка програма `curl` додала самостійно (завдання A.5)? Ці поля не є обов'язковими — сервер відповів і без них у завданні A.1. З якою метою їх додано?

У запиті curl (A.5) є три поля: `Host`, `User-Agent: curl/8.22.0` і `Accept: */*`. `Host` каже, до якого сайту звертаємось, `User-Agent` повідомляє серверу, яка програма надсилає запит (тут curl і його версія), а `Accept: */*` каже, що клієнт приймає вміст будь-якого типу. В моєму запиті A.1 двох останніх полів не було, бо я писав його вручну.

**5.** За якими ознаками у вашому виводі виявляється присутність проміжного вузла? Якщо таких ознак не виявлено, поясніть, що з цього випливає.

Проміжний вузол видно за такими ознаками:

* `Via: 1.0 www.fsf.org (squid/3.1.19)` прямо називає проксі;
* `X-Cache` і `X-Cache-Lookup` показують роботу кешу, а в другому ще й порт 3128, стандартний для Squid;
* відповідь прийшла як `HTTP/1.0`, хоча запит був `HTTP/1.1`: сторінку створив nginx, але версію в першому рядку відповіді поставив проксі;
* в A.3.1 є `Server: squid/3.1.19`, тобто Squid сам відповів на запит.

**6.** Три рядки, про які не йшлося на лекції, наведено в **Додатку В**.

---

## Додаток В. Відповіді на питання 6

| № | Рядок виводу | Джерело (номер завдання) |
|---|---|---|
| 1 | `X-Cache-Lookup: MISS from www.fsf.org:3128` | A.1 |
| 2 | `X-Squid-Error: ERR_CANNOT_FORWARD 0` | A.3.1 |
| 3 | `Mime-Version: 1.0` | A.3.1 |

---

## Додаток Д. Зведення результатів завдання A.3

**Вузол, з яким установлювалося з'єднання (у всіх пробах однаковий):** `fsf.org`, порт 80

| Проба | Значення поля `Host` | Версія | Код стану | Обсяг тіла відповіді | Збігається з A.1 (так / ні) |
|---|---|---|---|---|---|
| A.1 (вихідна) | `fsf.org` | 1.1 | 301 Moved Permanently | 178 байт | — |
| A.2 | поле відсутнє | 1.1 | 200 OK | 61 байт | ні |
| A.3.1 | `arin.net` | 1.1 | 503 Service Unavailable | 3242 байт | ні |
| A.3.2 | `opism-pr02.invalid` | 1.1 | 503 Service Unavailable | 3272 байт | ні |
| A.3.3 | поле відсутнє | 1.0 | 200 OK | 61 байт | ні |

**Висновок за таблицею (2–4 речення):** що саме змінювалося у запиті від проби до проби і як на це реагував сервер.

У всіх пробах я підключався до того самого вузла `fsf.org`, а змінювався тільки заголовок `Host` у запиті (а в A.3.3 ще й версія протоколу, 1.0). Нормальну відповідь (`301` від nginx з переадресацією на `https://www.fsf.org/`) дала лише проба зі своїм ім'ям (A.1), а на чужий і неіснуючий `Host` (A.3.1, A.3.2) відповів Squid кодом `503` і `ERR_CANNOT_FORWARD`. Без `Host` (A.2, A.3.3) прийшов код `200 OK`, але в тілі було `ERR_INVALID_REQ`, тобто код каже «успіх», а насправді це помилка.

---

## Декларування використання технологій штучного інтелекту

Для цієї роботи встановлено **рівень Р3 — ШІ як співвиконавець**.

Виводи команд частини A не можуть бути згенеровані та мають бути отримані внаслідок фактичного виконання команд.

**Чи використовувалися технології ШІ під час виконання роботи:** так

Якщо так, заповнюють таблицю. Якщо ні, таблицю вилучають.

Примітка до таблиці. Промпти наведено дослівно. Повідомлення, які містили лише вставлений вивід терміналу або скріншот, позначено як «[вивід терміналу …]» / «[скріншот …]»: самі виводи повністю наведено у звіті. Повідомлення з текстами моїх відповідей позначено як «[мій текст …]»: ці тексти наведено у звіті дослівно, з виправленнями ШІ, вказаними в останній колонці.



| № | Інструмент (назва, версія) | Етап роботи | Дослівний текст запиту (промпту) | Як використано результат |
|---|---|---|---|---|
| 1 | Claude Opus 5.5 |частина A | [скріншот вікна PowerShell з помилкою запуску скрипта] | ШІ пояснив причину |


**Підтвердження:** усі виводи команд, наведені в частині A, отримано внаслідок фактичного виконання команд на зазначеному індивідуальному домені.

---

*ОПІСМ · Практична робота № 2 · бланк звіту*
