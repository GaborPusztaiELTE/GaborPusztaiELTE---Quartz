---
dg-publish: true
---
## Mi történik egy hálózaton amikor te fel akarsz tölteni egy filet?

- A böngésző lesz egy kliens, aki üzenetet küld a hostnak (webcímre), feltölti a fájlt.

## Vegyünk fel egy valós forgalmat vagy töltsük le a korábban felvett forgalmat!

1. Töltsük le a követekező fájlt: [galaxisutikalauz.txt](https://canvas.elte.hu/courses/63326/files/4213292/download)
2. Hozzunk létre egy tmp mappát a C:\ meghajtón!
3. Nyissunk meg egy parancssort. (cmd)
4. Parancssorból adjuk ki a következő parancsot:  
    - SET SSLKEYLOGFILE=c:\tmp\ssKeys.log
    - Itt indítsuk el a böngészőt parancssorból:
        - Chrome: _"C:\Program Files\Google\Chrome\Application\chrome.exe" --ssl-key-log-file=c:\tmp\ssKeys.log_
        - Firefox: _"C:\Program Files\Mozilla Firefox\firefox.exe" --ssl-key-log-file=c:\tmp\ssKeys.log_
5. Indítsunk egy wireshark felvételt a megfelelő interface-n (ethernet vagy Wi-Fi)
6. Pingessük meg az ggombos.web.elte.hu címet a parancssorból!
    - ping ggombos.web.elte.hu
7. Tracert-el (vagy Traceroute-tal) nézzük meg a ggombos.web.elte.hu címet!
    - tracert ggombos.web.elte.hu
8. Frissítsük a dns adatainkat!
    - ipconfig /flushdns
9. Frissítsük az IP címünket:
    - ipconfig /release
    - ipconfig /renew
10. Nyissuk meg a következő oldalt: [http://oktnb147.inf.elte.hu/ggombos/upload/](http://oktnb147.inf.elte.hu/ggombos/upload/)
11. Töltsük fel a letöltött filet.
12. Nyissuk meg a [tanrend.elte.hu](https://tanrend.elte.hu/)-t!
13. Állítsuk meg a felvételt wiresharkban!

**Aki nem tudja megcsinálni, az töltse le a következő fájlokat:**

- [galaxisUpload.pcap](https://canvas.elte.hu/courses/63326/files/4213284/download)
- [galaxisSSLKeys.txt](https://canvas.elte.hu/courses/63326/files/4213285/download)

## Nézzük meg a lépéseket:

### 1 - A böngésző készít egy http kérést, amelyet odaad az alatta lévő rétegnek, aki az alatta lévőnek, aki....

- Ismerkedjünk a wiresharkkal és szűrjük le a csomagokat:
    - Olyan csomagot keresünk, amelyben van http kérés és a metódus GET. 
        - http.request.method == "GET"

- Nézzük meg a rétegeket egy adott csomagnál (TCP/IP modell)
    - alkalmazási réteg : HTTP: host
    - szállítási: TCP: src, dst port, FLAGS (ACK- nyugtázás. PUSH - azonnal adja az alkalmazási rétegnek)
    - internet (hálózati): IP: src, dst, version(4), Time To Live
    - adatkapcsolati (fizikai): Ethernet: src, dst
- Alkalmazási réteg: HTTP
    - szűrés: http.request.uri == "/ggombos/upload/" 
    - Jobb klikk arra a csomagra és 'Follow'  -> 'HTTP Stream'
    - Kérés - válasz felépítését láthatjuk

### 2 - Hova küldjük a csomagot ki a hoszt? Ki a címzet? DNS

- Nézzük meg a dns üzeneteket a wiresharkban
    
    - szűrés: dns
    - Van egy query és egy response üzenet. 
    - Típusok: A - ipv4, AAAA - ipv6, MX - mail server, NS - nameserver
- Mi lett az IP címe az oktnb147.inf.elte.hu gépnek?
    
    - szűrés: dns.qry.name == "oktnb147.inf.elte.hu"

### 3 - Ki fogja szállítani a csomagokat? TCP / UDP

- Keressünk olyan TCP forgalmat, amelynek a syn flag-e be van állítva, majd nézük meg a kommunikációát
    - szűrés: tcp.flags.syn == 1
    - Láthatjuk a kapcsolat kiépítést: SYN, SYN-ACK, ACK
    - Láthatjuk a kapcsolat lezárást: FIN, FIN-ACK, ACK
- Nézzük meg vannak-e udp forgalmak
    - szűrés: udp
    - Láthatjuk hogy a DNS csomagok is UDP-n keresztül mennek.

### 4 - Ping / Tracert (Traceroute) forgalom! ICMP protokol

- Nézzük meg a ping és tracert (traceroute) parancsok csomagjait!
- szűrés: icmp
- Láthatjuk, hogy mely kérésekre volt válasz.
- Láthatjuk, hogy a tracert-nél (traceroute-nál) a TTL mező hogyan változik a kéréseknél

Tracert példa:

![tracert_pl.png](https://canvas.elte.hu/courses/63326/files/4213283/preview)

### 5 - Keressük  meg a tanrend.elte.hu hívásunkat is!

- szűrés: http.request.uri contains "tanrend"
    - nem találunk semmit
- Keressük meg az IP címét
    - dns.qry.name == "tanrend.elte.hu"
- Nézzük meg a forgalmat erre az IP címre
    - ip.dst == 157.181.1.225
- Nem fogjuk látni a forgalmat mert titkosítva van. (tls)
- Adjuk meg az ssllog filet, hogy a Wireshark vissza tudja fejteni a forgalmat:
    - Edit --> Preferences --> Protocols --> TLS --> (Pre)-Master-Secret log filename
        - c:\tmp\ssKeys.log
- Ezután már látjuk a forgalmat!
- Ez a forgalom már HTTP2-n megy, amely stream alapú és lehet több header is egy csomagban
- Keressük meg a GET hívásunkat:
    - szűrés: http2.headers.method == "GET"
- Keressük meg azokat amelyek a "tanrend.elte.hu"-ra mentés egy css-t tölt le
    - szűrés: http2.headers.authority == "tanrend.elte.hu" and http2.headers.path contains ".css"

## Önálló feladatok

1. Hány '.css' kiterjesztésű fájt szerettek volna letölteni? (contains)
2. Keresd meg a HTTP POST kérést és  nézd meg a tartalmát. (vagy a Mime-ban, vagy Follow HTTP Stream)
3. Keress olyan HTTP válaszokat, amelyek sikeresek, és amelyek sikertelenek: response kód 200, >400.
4. Nézd meg milyen porton hívtuk meg a weboldalt!
5. Mi lett a címe a 'tanrend.elte.hu'-nak? (dns response)
6. Nézd meg milyen portra mennek a DNS kérések!
7. Vannak-e benne DHCP kérések?
8. Vannak-e benne ARP kérések? 
    1. Hány kérés ment egy adott IP-re? (pl: arp.dst.proto_ipv4 == 157.181.165.126)
9. Nézzük meg van-e benne titkosított kézfogás! (tls.handshake.type) 1 -kliens, 2-szerver

## További feladatok

1. Töltsük be a következő pcap-et: [httpIwiw.pcap](https://canvas.elte.hu/courses/63326/files/4213288/download)
2. Milyen oldalakat kértek le a szűrés alapján? Milyen böngészőt használtak hozzá?
3. Kérdezd le a POST kéréssel küldött adatokat és nézd meg mik lettek elküldve! Milyen Érdekességet látsz?
4. Hány darab .png képet érintett a böngészés? (help: contains)
5. Hány olyan erőforrás volt, amelyet nem kellett újra töltenie a böngészőnek? Mely oldalakat érintette ez? (304 Not Modified)

## Hálózati támadások elemzése (csak érdekesség)

[https://www.malware-traffic-analysis.net/2025/01/22/index.html](https://www.malware-traffic-analysis.net/2025/01/22/index.html)