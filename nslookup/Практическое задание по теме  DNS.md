# Практическое задание по теме  DNS:

#### 1. Изучил способы вызова программы nslookup

#### 2. Работая с nslookup в режиме одного запроса:

##### -Выяснил адреса серверов имен(NS) для:

**urfu.ru**

Посмотрим DNSсервера для urfu.ru set type=ns - не авторитетный ответ

Выберем один из серверов как default - server ns1.urfu.ru - теперь когда мы захотим узнать для urfu.ru DNS сервера - ответ будет автортетным

```
C:\Users\111>nslookup
╤хЁтхЁ яю єьюыўрэш■:  UnKnown
Address:  192.168.43.1

> set type=ns
> urfu.ru
╤хЁтхЁ:  UnKnown
Address:  192.168.43.1

Не заслуживающий доверия ответ:
URFU.ru nameserver = ns3.urfu.ru
URFU.ru nameserver = ns1.urfu.ru
URFU.ru nameserver = ns2.urfu.ru

ns2.URFU.ru     internet address = 212.193.82.21
ns1.URFU.ru     internet address = 212.193.66.21
ns3.URFU.ru     internet address = 212.193.72.21
> server ns1.urfu.ru
╤хЁтхЁ яю єьюыўрэш■:  ns1.URFU.ru
Address:  212.193.66.21

> urfu.ru
╤хЁтхЁ:  ns1.URFU.ru
Address:  212.193.66.21

urfu.ru nameserver = ns2.urfu.ru
urfu.ru nameserver = ns3.urfu.ru
urfu.ru nameserver = ns1.urfu.ru
ns1.urfu.ru     internet address = 212.193.66.21
ns2.urfu.ru     internet address = 212.193.82.21
ns3.urfu.ru     internet address = 212.193.72.21
```

**msu.ru**

Аналогично для msu.ru

```
> msu.ru
╤хЁтхЁ:  ns.MSU.ru
Address:  93.180.0.1

msu.ru  nameserver = ns.msu.ru
msu.ru  nameserver = ns3.nic.fr
msu.ru  nameserver = ns1.orc.ru
msu.ru  nameserver = ns.msu.net
ns.msu.ru       internet address = 93.180.0.1
ns.msu.net      internet address = 212.16.0.1
```

##### -Выяснил IP адреса хостов для символьных имен:

Чтобы получить авторитетный ответ, как и в первом пункте пропишем DNS сервер, который отвечает за urfu.ru и который может дать авторитетный(не кешированный) ответ. Тип сообщения - set type=A

**urfu.ru**

```
C:\Users\111>nslookup
╤хЁтхЁ яю єьюыўрэш■:  UnKnown
Address:  192.168.43.1

> set timeout=4
> server ns1.urfu.ru
╤хЁтхЁ яю єьюыўрэш■:  ns1.URFU.ru
Address:  212.193.66.21

> urfu.ru
╤хЁтхЁ:  ns1.URFU.ru
Address:  212.193.66.21

╚ь :     urfu.ru
Address:  212.193.82.20
```

**rbc.ru**

Аналогично поступим и для rbc.ru

```
> rbc.ru
╤хЁтхЁ:  ns2.RBC.ru
Address:  80.68.247.10

╚ь :     rbc.ru
Addresses:  80.68.253.9
          185.72.229.9
```

#### 3. Перешел в режим командной строки nslookup. Выяснил имя и адрес dns-сервера, которому будут отправляться запросы:

Командой root узнаем DNS сервер (корневой (так как к нему будут отправляться запросы(мы не прописывали default server => этот сервер действительно корневой, так как к нему будут посылаться запросы даже если мы не прописали никакой другой сервер)))

```
> root
╤хЁтхЁ яю єьюыўрэш■:  A.ROOT-SERVERS.NET
Addresses:  2001:503:ba3e::2:30
          198.41.0.4
```

#### 4. Изучил команды перехода между серверами – server, lserver и root.

**server NAME**   - *установить сервер по умолчанию NAME, используя текущий сервер по умолчанию*
**lserver NAME**  - *установить сервер по умолчанию NAME, используя первоначальный сервер*

```
C:\Users\111>nslookup
╤хЁтхЁ яю єьюыўрэш■:  UnKnown
Address:  192.168.43.1

> server 194.226.235.1
╤хЁтхЁ яю єьюыўрэш■:  [194.226.235.1]
Address:  194.226.235.1

> server ns1.urfu.ru
DNS request timed out.
    timeout was 2 seconds.
DNS request timed out.
    timeout was 2 seconds.
DNS request timed out.
    timeout was 2 seconds.
DNS request timed out.
    timeout was 2 seconds.
*** Не найден адрес для сервера ns1.urfu.ru: Timed out
> lserver ns1.urfu.ru
╤хЁтхЁ яю єьюыўрэш■:  ns1.URFU.ru
Address:  212.193.66.21
```

Мы не смогли перейти на ns1.urfu.ru командой server, так как нам надо было разрешить ns1.urfu.ru в IP адрес, для этого мы бы воспользовались DNS сервером, а так как у нас прописан несуществующий сервер, то мы попросту н смогли разрешить это имя и не смогли перейти на него(на этот сервер).

Команда lserver же для того, чтобы установить новый сервер по умолчанию использует первоначальный сервер(удобно, если мы командой server случайно описались и написали несуществующий сервер, и захотели его поменять, тогда нам на помощь прийдет команда lserver)

#### 5. Перешел в режим запроса записей NS (set q=ns или set type=ns), выяснил адреса серверов имён для доменов верхнего уровня (и их общее количество):

**com**

```
> set type=ns
> com.
╤хЁтхЁ:  UnKnown
Address:  192.168.43.1

Не заслуживающий доверия ответ:
com     nameserver = l.gtld-servers.net
com     nameserver = b.gtld-servers.net
com     nameserver = h.gtld-servers.net
com     nameserver = j.gtld-servers.net
com     nameserver = g.gtld-servers.net
com     nameserver = m.gtld-servers.net
com     nameserver = c.gtld-servers.net
com     nameserver = a.gtld-servers.net
com     nameserver = f.gtld-servers.net
com     nameserver = k.gtld-servers.net
com     nameserver = i.gtld-servers.net
com     nameserver = e.gtld-servers.net
com     nameserver = d.gtld-servers.net

b.gtld-servers.net      internet address = 192.33.14.30
b.gtld-servers.net      AAAA IPv6 address = 2001:503:231d::2:30
h.gtld-servers.net      internet address = 192.54.112.30
h.gtld-servers.net      AAAA IPv6 address = 2001:502:8cc::30
m.gtld-servers.net      internet address = 192.55.83.30
m.gtld-servers.net      AAAA IPv6 address = 2001:501:b1f9::30
f.gtld-servers.net      internet address = 192.35.51.30
f.gtld-servers.net      AAAA IPv6 address = 2001:503:d414::30
j.gtld-servers.net      internet address = 192.48.79.30
j.gtld-servers.net      AAAA IPv6 address = 2001:502:7094::30
i.gtld-servers.net      internet address = 192.43.172.30
i.gtld-servers.net      AAAA IPv6 address = 2001:503:39c1::30
```

Но, как мы видим, нам вывелись не все записи (не по всем буквам) => почитав документацию, найдем, что можно заменить тип с NS на ALL => будут выводиться все записи, попробуем

```
> set type=all
> com.
╤хЁтхЁ:  UnKnown
Address:  192.168.43.1

Не заслуживающий доверия ответ:
com     ??? unknown type 46 ???
com     ??? unknown type 51 ???
com     ??? unknown type 46 ???
com     ??? unknown type 48 ???
com     ??? unknown type 48 ???
com     ??? unknown type 46 ???
com     ??? unknown type 46 ???
com     ??? unknown type 43 ???
com     nameserver = a.gtld-servers.net
com     nameserver = f.gtld-servers.net
com     nameserver = i.gtld-servers.net
com     nameserver = d.gtld-servers.net
com     nameserver = k.gtld-servers.net
com     nameserver = e.gtld-servers.net
com     nameserver = c.gtld-servers.net
com     nameserver = l.gtld-servers.net
com     nameserver = m.gtld-servers.net
com     nameserver = g.gtld-servers.net
com     nameserver = j.gtld-servers.net
com     nameserver = b.gtld-servers.net
com     nameserver = h.gtld-servers.net

com     nameserver = h.gtld-servers.net
com     nameserver = j.gtld-servers.net
com     nameserver = a.gtld-servers.net
com     nameserver = e.gtld-servers.net
com     nameserver = g.gtld-servers.net
com     nameserver = f.gtld-servers.net
com     nameserver = c.gtld-servers.net
com     nameserver = i.gtld-servers.net
com     nameserver = d.gtld-servers.net
com     nameserver = b.gtld-servers.net
com     nameserver = m.gtld-servers.net
com     nameserver = l.gtld-servers.net
com     nameserver = k.gtld-servers.net
b.gtld-servers.net      internet address = 192.33.14.30
b.gtld-servers.net      AAAA IPv6 address = 2001:503:231d::2:30
h.gtld-servers.net      internet address = 192.54.112.30
h.gtld-servers.net      AAAA IPv6 address = 2001:502:8cc::30
m.gtld-servers.net      internet address = 192.55.83.30
m.gtld-servers.net      AAAA IPv6 address = 2001:501:b1f9::30
f.gtld-servers.net      internet address = 192.35.51.30
f.gtld-servers.net      AAAA IPv6 address = 2001:503:d414::30
j.gtld-servers.net      internet address = 192.48.79.30
j.gtld-servers.net      AAAA IPv6 address = 2001:502:7094::30
i.gtld-servers.net      internet address = 192.43.172.30
i.gtld-servers.net      AAAA IPv6 address = 2001:503:39c1::30
a.gtld-servers.net      internet address = 192.5.6.30
a.gtld-servers.net      AAAA IPv6 address = 2001:503:a83e::2:30
e.gtld-servers.net      internet address = 192.12.94.30
e.gtld-servers.net      AAAA IPv6 address = 2001:502:1ca1::30
k.gtld-servers.net      internet address = 192.52.178.30
k.gtld-servers.net      AAAA IPv6 address = 2001:503:d2d::30
g.gtld-servers.net      internet address = 192.42.93.30
g.gtld-servers.net      AAAA IPv6 address = 2001:503:eea3::30
d.gtld-servers.net      internet address = 192.31.80.30
d.gtld-servers.net      AAAA IPv6 address = 2001:500:856e::30
l.gtld-servers.net      internet address = 192.41.162.30
l.gtld-servers.net      AAAA IPv6 address = 2001:500:d937::30
c.gtld-servers.net      internet address = 192.26.92.30
c.gtld-servers.net      AAAA IPv6 address = 2001:503:83eb::30
```

Теперь нам вывелись все записи обо всех DNS серверах. Но мы получили некоторые необычные строчки и названия именные серверов дважды(скорее всего из-за того, что нам вывелись и по IPv4 и по IPv6)

**org**

Аналогичные операции (только теперь только с set type=NS(ак как в данном случае выведутся все записи и не будет непонятных полей)) проделаем с org.

```
> org.
╤хЁтхЁ:  UnKnown
Address:  192.168.43.1

Не заслуживающий доверия ответ:
org     nameserver = b0.org.afilias-nst.org
org     nameserver = c0.org.afilias-nst.info
org     nameserver = a0.org.afilias-nst.info
org     nameserver = a2.org.afilias-nst.info
org     nameserver = b2.org.afilias-nst.org
org     nameserver = d0.org.afilias-nst.org

b0.org.afilias-nst.org  internet address = 199.19.54.1
b0.org.afilias-nst.org  AAAA IPv6 address = 2001:500:c::1
a2.org.afilias-nst.info internet address = 199.249.112.1
a2.org.afilias-nst.info AAAA IPv6 address = 2001:500:40::1
a0.org.afilias-nst.info internet address = 199.19.56.1
a0.org.afilias-nst.info AAAA IPv6 address = 2001:500:e::1
c0.org.afilias-nst.info internet address = 199.19.53.1
c0.org.afilias-nst.info AAAA IPv6 address = 2001:500:b::1
d0.org.afilias-nst.org  internet address = 199.19.57.1
d0.org.afilias-nst.org  AAAA IPv6 address = 2001:500:f::1
b2.org.afilias-nst.org  internet address = 199.249.120.1
b2.org.afilias-nst.org  AAAA IPv6 address = 2001:500:48::1
```

**ru**

Аналогичные операции (только теперь только с set type=NS(так как в данном случае выведутся все записи и не будет непонятных полей)) проделаем с ru.

```
> set type=NS
> ru.
╤хЁтхЁ:  UnKnown
Address:  192.168.43.1

Не заслуживающий доверия ответ:
ru      nameserver = b.dns.ripn.net
ru      nameserver = e.dns.ripn.net
ru      nameserver = a.dns.ripn.net
ru      nameserver = d.dns.ripn.net
ru      nameserver = f.dns.ripn.net

e.dns.ripn.net  internet address = 193.232.142.17
e.dns.ripn.net  AAAA IPv6 address = 2001:678:15:0:193:232:142:17
b.dns.ripn.net  internet address = 194.85.252.62
b.dns.ripn.net  AAAA IPv6 address = 2001:678:16:0:194:85:252:62
a.dns.ripn.net  internet address = 193.232.128.6
a.dns.ripn.net  AAAA IPv6 address = 2001:678:17:0:193:232:128:6
d.dns.ripn.net  internet address = 194.190.124.17
d.dns.ripn.net  AAAA IPv6 address = 2001:678:18:0:194:190:124:17
f.dns.ripn.net  internet address = 193.232.156.17
f.dns.ripn.net  AAAA IPv6 address = 2001:678:14:0:193:232:156:17
```

#### 6.Прошлись по цепочке DNS серверов от корня и нашли IP адрес для символьного имени:

**cs.usu.edu.ru**

```
C:\Users\111>nslookup
╤хЁтхЁ яю єьюыўрэш■:  UnKnown
Address:  192.168.43.1

> set type=ns
> ru.
╤хЁтхЁ:  UnKnown
Address:  192.168.43.1

a.dns.ripn.net  internet address = 193.232.128.6

> lserver 193.232.128.6
╤хЁтхЁ яю єьюыўрэш■:  a.dns.ripn.net
Address:  193.232.128.6

> edu.ru.
╤хЁтхЁ:  a.dns.ripn.net
Address:  193.232.128.6

ns.MSU.RU       internet address = 93.180.0.1

> lserver 93.180.0.1
╤хЁтхЁ яю єьюыўрэш■:  ns.msu.ru
Address:  93.180.0.1

> usu.edu.ru
╤хЁтхЁ:  ns.msu.ru
Address:  93.180.0.1

usu.edu.ru      nameserver = ns.urgu.org

> lserver ns.urgu.org
╤хЁтхЁ яю єьюыўрэш■:  ns.urgu.org
Address:  212.193.68.254

> set type=A

> cs.usu.edu.ru
╤хЁтхЁ:  ns.urgu.org
Address:  212.193.68.254

╚ь :     cs.usu.edu.ru
Address:  212.193.68.254
```

**www.imm.uran.ru**

```
> set type=ns

> ru.
╤хЁтхЁ:  UnKnown
Address:  192.168.43.1

a.dns.ripn.net  internet address = 193.232.128.6

> lserver 193.232.128.6
╤хЁтхЁ яю єьюыўрэш■:  a.dns.ripn.net
Address:  193.232.128.6

> uran.ru.
╤хЁтхЁ:  a.dns.ripn.net
Address:  193.232.128.6

ns.URAN.RU      internet address = 195.19.137.69

> lserver 195.19.137.69
╤хЁтхЁ яю єьюыўрэш■:  ns.uran.ru
Address:  195.19.137.69

> set type=A

> www.imm.uran.ru.
╤хЁтхЁ:  ns.uran.ru
Address:  195.19.137.69

╚ь :     www.imm.uran.ru
Address:  195.19.137.125
```

**kma.imkn.urfu.ru**

```
> set type=ns

> ru.
╤хЁтхЁ:  UnKnown
Address:  192.168.43.1

a.dns.ripn.net  internet address = 193.232.128.6

> lserver 193.232.128.6
╤хЁтхЁ яю єьюыўрэш■:  a.dns.ripn.net
Address:  193.232.128.6

> urfu.ru
╤хЁтхЁ:  a.dns.ripn.net
Address:  193.232.128.6

ns1.URFU.RU     internet address = 212.193.66.21

> lserver 212.193.66.21
╤хЁтхЁ яю єьюыўрэш■:  [212.193.66.21]
Address:  212.193.66.21

> set type=A

> imkn.urfu.ru
╤хЁтхЁ:  [212.193.66.21]
Address:  212.193.66.21

╚ь :     imkn.urfu.ru
Address:  212.193.68.65

> kma.imkn.urfu.ru
╤хЁтхЁ:  [212.193.66.21]
Address:  212.193.66.21

╚ь :     kma.imkn.urfu.ru
Address:  212.193.66.79
```

#### 7. Изучить способы получения с сервера всех записей (команда ls). Подключиться к нужномусерверу, вывести на экран и сохранить в файл записи для:

**edu.ru (все записи)**

```
C:\Users\111\Desktop>nslookup
╤хЁтхЁ яю єьюыўрэш■:  UnKnown
Address:  192.168.43.1

> set type=ns
> set timeout=4
> edu.ru
╤хЁтхЁ:  UnKnown
Address:  192.168.43.1

Не заслуживающий доверия ответ:
EDU.ru  nameserver = ns2.informika.ru
EDU.ru  nameserver = ns.informika.ru
EDU.ru  nameserver = ns.msu.ru
EDU.ru  nameserver = ns2.free.net
EDU.ru  nameserver = ns.runnet.ru

ns2.free.net    internet address = 193.233.1.67
ns2.free.net    AAAA IPv6 address = 2001:640:1:1::67
ns.RUNNET.ru    internet address = 194.85.32.18
ns.MSU.ru       internet address = 93.180.0.1
ns2.INFORMIKA.ru        internet address = 194.190.241.65
> lserver 93.180.0.1
╤хЁтхЁ яю єьюыўрэш■:  ns.msu.ru
Address:  93.180.0.1

> ls -d edu.ru > res1.txt
[ns.msu.ru]
####
Received 1800 records.
```

**urfu.ru (записи типа A)**

```
C:\Users\111\Desktop>nslookup
╤хЁтхЁ яю єьюыўрэш■:  UnKnown
Address:  192.168.43.1

> set type=ns
> set timeout=4
> urfu.ru.
╤хЁтхЁ:  UnKnown
Address:  192.168.43.1

Не заслуживающий доверия ответ:
URFU.ru nameserver = ns3.urfu.ru
URFU.ru nameserver = ns2.urfu.ru
URFU.ru nameserver = ns1.urfu.ru

ns2.URFU.ru     internet address = 212.193.82.21
ns3.URFU.ru     internet address = 212.193.72.21
ns1.URFU.ru     internet address = 212.193.66.21
> lserver 212.193.66.21
╤хЁтхЁ яю єьюыўрэш■:  [212.193.66.21]
Address:  212.193.66.21

> ls -d urfu.ru > res2.txt
[[212.193.72.21]]
Received 0 records.
*** Can't list domain urfu.ru: Query refused
DNS-сервер отклонил передачу зоны urfu.ru на данный компьютер. Если это
ошибка, проверьте параметры безопасности передачи зоны для urfu.ru на DNS-
сервере по IP-адресу 212.193.72.21.
> ls -t a urfu.ru > res2.txt
ls: connect: No error
*** Can't list domain urfu.ru: Unspecified error
> ls -t a urfu.ru > res2.txt
[[212.193.66.21]]
Received 0 records.
*** Can't list domain urfu.ru: Query refused
```

**mail.ru (записи типа MX)**

```
C:\Users\111\Desktop>nslookup
╤хЁтхЁ яю єьюыўрэш■:  UnKnown
Address:  192.168.43.1

> set type=ns
> set timeout=4
> mail.ru
╤хЁтхЁ:  UnKnown
Address:  192.168.43.1

Не заслуживающий доверия ответ:
MAIL.ru nameserver = ns1.mail.ru
MAIL.ru nameserver = ns2.mail.ru
MAIL.ru nameserver = ns3.mail.ru

ns3.MAIL.ru     internet address = 185.30.176.202
ns3.MAIL.ru     AAAA IPv6 address = 2a00:1148:db00::2
ns1.MAIL.ru     internet address = 217.69.139.112
ns1.MAIL.ru     AAAA IPv6 address = 2a00:1148:db00::2
ns2.MAIL.ru     internet address = 94.100.180.138
ns2.MAIL.ru     AAAA IPv6 address = 2a00:1148:db00::1
> lserver 217.69.139.112
╤хЁтхЁ яю єьюыўрэш■:  ns1.mail.ru
Address:  217.69.139.112

> ls -t mx mail.ru > res3.txt
[ns1.mail.ru]
Received 0 records.
*** Can't list domain mail.ru: Query refused
```

#### 8. Получить «начальную запись зоны» (SOA – start of authority), выяснить вероятную дату последнего обновления зоны, время жизни записей в промежуточных кеширующих серверах и прочую информацию для:

**ya.ru**

```
C:\Users\111\Desktop>nslookup
╤хЁтхЁ яю єьюыўрэш■:  UnKnown
Address:  192.168.43.1

> set type=soa
> ya.ru.
╤хЁтхЁ:  UnKnown
Address:  192.168.43.1

Не заслуживающий доверия ответ:
ya.ru
        primary name server = ns1.yandex.ru
        responsible mail addr = sysadmin.yandex.ru
        serial  = 2018031600
        refresh = 900 (15 mins)
        retry   = 600 (10 mins)
        expire  = 2592000 (30 days)
        default TTL = 900 (15 mins)

YA.ru   nameserver = ns1.yandex.ru
YA.ru   nameserver = ns2.yandex.ru
ns2.YANDEX.ru   internet address = 93.158.134.1
ns2.YANDEX.ru   AAAA IPv6 address = 2a02:6b8:0:1::1
ns1.YANDEX.ru   internet address = 213.180.193.1
ns1.YANDEX.ru   AAAA IPv6 address = 2a02:6b8::1
```

**urfu.ru**

```
> urfu.ru
╤хЁтхЁ:  UnKnown
Address:  192.168.43.1

Не заслуживающий доверия ответ:
urfu.ru
        primary name server = ns1.urfu.ru
        responsible mail addr = hostmaster.urfu.ru
        serial  = 2012091861
        refresh = 3600 (1 hour)
        retry   = 1800 (30 mins)
        expire  = 2419200 (28 days)
        default TTL = 3600 (1 hour)

URFU.ru nameserver = ns1.urfu.ru
URFU.ru nameserver = ns3.urfu.ru
URFU.ru nameserver = ns2.urfu.ru
ns1.URFU.ru     internet address = 212.193.66.21
ns3.URFU.ru     internet address = 212.193.72.21
ns2.URFU.ru     internet address = 212.193.82.21
```

**mail.ru**

```
> mail.ru
╤хЁтхЁ:  UnKnown
Address:  192.168.43.1

Не заслуживающий доверия ответ:
mail.ru
        primary name server = ns1.mail.ru
        responsible mail addr = hostmaster.mail.ru
        serial  = 3312856144
        refresh = 900 (15 mins)
        retry   = 900 (15 mins)
        expire  = 604800 (7 days)
        default TTL = 60 (1 min)

MAIL.ru nameserver = ns2.mail.ru
MAIL.ru nameserver = ns3.mail.ru
MAIL.ru nameserver = ns1.mail.ru
ns2.MAIL.ru     internet address = 94.100.180.138
ns2.MAIL.ru     AAAA IPv6 address = 2a00:1148:db00::1
ns1.MAIL.ru     internet address = 217.69.139.112
ns1.MAIL.ru     AAAA IPv6 address = 2a00:1148:db00::2
ns3.MAIL.ru     internet address = 185.30.176.202
ns3.MAIL.ru     AAAA IPv6 address = 2a00:1148:db00::2
```

#### 9.1 Нашел на www.iana.org (www.icann.org) полный список доменов верхнего уровня

```
DOMAIN	TYPE	TLD MANAGER
.aaa	generic	American Automobile Association, Inc.
.aarp	generic	AARP
.abarth	generic	Fiat Chrysler Automobiles N.V.
.abb	generic	ABB Ltd
.abbott	generic	Abbott Laboratories, Inc.
.abbvie	generic	AbbVie Inc.
.abc	generic	Disney Enterprises, Inc.
.able	generic	Able Inc.
.abogado	generic	Top Level Domain Holdings Limited
.abudhabi	generic	Abu Dhabi Systems and Information Centre
.ac	country-code	Network Information Center (AC Domain Registry) c/o Cable and Wireless (Ascension Island)
.academy	generic	Half Oaks, LLC
.accenture	generic	Accenture plc
.accountant	generic	dot Accountant Limited
.accountants	generic	Knob Town, LLC
.aco	generic	ACO Severin Ahlmann GmbH & Co. KG
.active	generic	Active Network, LLC
.actor	generic	United TLD Holdco Ltd.
.ad	country-code	Andorra Telecom
.adac	generic	Allgemeiner Deutscher Automobil-Club e.V. (ADAC)
.ads	generic	Charleston Road Registry Inc.
.adult	generic	ICM Registry AD LLC
.ae	country-code	Telecommunication Regulatory Authority (TRA)
.aeg	generic	Aktiebolaget Electrolux
.aero	sponsored	Societe Internationale de Telecommunications Aeronautique (SITA INC USA)
.aetna	generic	Aetna Life Insurance Company
.af	country-code	Ministry of Communications and IT
.afamilycompany	generic	Johnson Shareholdings, Inc.
.afl	generic	Australian Football League
.africa	generic	ZA Central Registry NPC trading as Registry.Africa
.ag	country-code	UHSA School of Medicine
.agakhan	generic	Fondation Aga Khan (Aga Khan Foundation)
.agency	generic	Steel Falls, LLC
.ai	country-code	Government of Anguilla
.aig	generic	American International Group, Inc.
.aigo	generic	aigo Digital Technology Co,Ltd.
.airbus	generic	Airbus S.A.S.
.airforce	generic	United TLD Holdco Ltd.
.airtel	generic	Bharti Airtel Limited
.akdn	generic	Fondation Aga Khan (Aga Khan Foundation)
.al	country-code	Electronic and Postal Communications Authority - AKEP
.alfaromeo	generic	Fiat Chrysler Automobiles N.V.
.alibaba	generic	Alibaba Group Holding Limited
.alipay	generic	Alibaba Group Holding Limited
```

и так далее...

####9.2 Выяснить на www.nic.ru стоимость регистрации собственного домена в различных зонах, необходимые для этого документы и способы оплаты.

```
Рекомендуем
ara
.rent
5 990 руб.

В корзину
evan
.sale
Премиум
9 990 руб.

В корзину
member
.rent
5 990 руб.

В корзину
teams
.plus
2 990 руб.

В корзину
trash
.moscow
Премиум
1 690 руб.

В корзину
marcia
.cafe
2 990 руб.

В корзину
caddy
.style
2 990 руб.

В корзину
koster
.studio
890 руб.

В корзину
badges
.world
890 руб.

В корзину
kuzov
.team
890 руб.

В корзину
produc
.cloud
SSL В ПОДАРОК
890 руб.

В корзину
мебельная
.рф
Магазин доменов
70 000 руб.
Заказать

```

и так далее...

#### 9.3 Найти (например, в google) регистратора с минимальной стоимостью домена в зоне ru.

```
fookysnick.ru
Действуют ограничения. 
Добавить в корзину889,00 руб 56,00 руб
fookysnickhome.ru
Действуют ограничения. 
Добавить в корзину889,00 руб 56,00 руб
fookysnicktech.ru
Действуют ограничения. 
Добавить в корзину889,00 руб 56,00 руб
fookysnickshop.ru
Действуют ограничения. 
Добавить в корзину889,00 руб 56,00 руб
fookysnickmart.ru
Действуют ограничения. 
Добавить в корзину889,00 руб 56,00 руб
fookysnickstar.ru
Действуют ограничения. 
Добавить в корзину889,00 руб 56,00 руб
fookysnickzone.ru
Действуют ограничения. 
Добавить в корзину889,00 руб 56,00 руб
```

#### 9.4 Найти регистратора с минимальной стоимость домена в зонах com и org

```
fookysnick.org Добавить в корзину1 292,00 руб 482,00 руб
fookysnicksolutions.com Добавить в корзину1 212,00 руб 69,00 руб
fookysnickproperties.com Добавить в корзину1 212,00 руб 69,00 руб
fookysnickreviews.com Добавить в корзину1 212,00 руб 69,00 руб
fookysnicksystems.com Добавить в корзину1 212,00 руб 69,00 руб
fookysnicknetwork.com Добавить в корзину1 212,00 руб 69,00 руб
fookysnickacademy.com Добавить в корзину1 212,00 руб 69,00 руб
fookysnickpro.com Добавить в корзину
```

**По итогу самый дешевый регистратор - GoDaddy - **https://goo.gl/pwW9qv - от 56 рублей











