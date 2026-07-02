---
dg-publish: false
---
<h2>Mi t&ouml;rt&eacute;nik egy h&aacute;l&oacute;zaton amikor te fel akarsz t&ouml;lteni egy filet?</h2>
<p>- A <span style="font-family: sans-serif; font-size: 1rem;">b&ouml;ng&eacute;sző lesz egy kliens, aki &uuml;zenetet k&uuml;ld a hostnak (webc&iacute;mre), felt&ouml;lti a f&aacute;jlt.</span></p>
<p>&nbsp;</p>
<h2>Vegy&uuml;nk fel egy val&oacute;s forgalmat vagy t&ouml;lts&uuml;k le a kor&aacute;bban felvett forgalmat!</h2>
<ol>
<li>T&ouml;lts&uuml;k le a k&ouml;vetekező f&aacute;jlt: <a href="https://canvas.elte.hu/courses/63326/files/4213292/download" data-api-endpoint="https://canvas.elte.hu/api/v1/courses/63326/files/4213292" data-api-returntype="File">galaxisutikalauz.txt</a></li>
<li>Hozzunk l&eacute;tre egy tmp mapp&aacute;t a C:\ meghajt&oacute;n!</li>
<li>Nyissunk meg egy parancssort. (cmd)</li>
<li>Parancssorb&oacute;l adjuk ki a k&ouml;vetkező parancsot:<br />
<ul>
<li>SET SSLKEYLOGFILE=c:\tmp\ssKeys.log</li>
<li>Itt ind&iacute;tsuk el a b&ouml;ng&eacute;szőt parancssorb&oacute;l:
<ul>
<li>Chrome: <em>"C:\Program Files\Google\Chrome\Application\chrome.exe" --ssl-key-log-file=c:\tmp\ssKeys.log</em></li>
<li>Firefox: <em>"C:\Program Files\Mozilla Firefox\firefox.exe" --ssl-key-log-file=c:\tmp\ssKeys.log</em></li>
</ul>
</li>
</ul>
</li>
<li>Ind&iacute;tsunk egy wireshark felv&eacute;telt a megfelelő interface-n (ethernet vagy Wi-Fi)</li>
<li>Pingess&uuml;k meg az ggombos.web.elte.hu c&iacute;met a parancssorb&oacute;l!
<ul>
<li>ping ggombos.web.elte.hu</li>
</ul>
</li>
<li>Tracert-el (vagy Traceroute-tal) n&eacute;zz&uuml;k meg a ggombos.web.elte.hu c&iacute;met!
<ul>
<li>tracert ggombos.web.elte.hu</li>
</ul>
</li>
<li>Friss&iacute;ts&uuml;k a dns adatainkat!
<ul>
<li><span data-teams="true">ipconfig /flushdns</span></li>
</ul>
</li>
<li><span data-teams="true">Friss&iacute;ts&uuml;k az IP c&iacute;m&uuml;nket:</span>
<ul>
<li><span data-teams="true">ipconfig /release</span></li>
<li><span data-teams="true">ipconfig /renew</span></li>
</ul>
</li>
<li>Nyissuk meg a k&ouml;vetkező oldalt:&nbsp;<a href="http://oktnb147.inf.elte.hu/ggombos/upload/">http://oktnb147.inf.elte.hu/ggombos/upload/</a></li>
<li>T&ouml;lts&uuml;k fel a let&ouml;lt&ouml;tt filet.</li>
<li>Nyissuk meg a <a href="https://tanrend.elte.hu/">tanrend.elte.hu</a>-t!</li>
<li>&Aacute;ll&iacute;tsuk meg a felv&eacute;telt wiresharkban!</li>
</ol>
<p><span style="font-size: 14pt;"><strong>Aki nem tudja megcsin&aacute;lni, az t&ouml;ltse le a k&ouml;vetkező f&aacute;jlokat:</strong> </span></p>
<ul>
<li><span style="font-size: 14pt;"><a href="https://canvas.elte.hu/courses/63326/files/4213284/download" data-api-endpoint="https://canvas.elte.hu/api/v1/courses/63326/files/4213284" data-api-returntype="File">galaxisUpload.pcap</a></span></li>
<li><span style="font-size: 14pt;"><a href="https://canvas.elte.hu/courses/63326/files/4213285/download" data-api-endpoint="https://canvas.elte.hu/api/v1/courses/63326/files/4213285" data-api-returntype="File">galaxisSSLKeys.txt</a></span></li>
</ul>
<p>&nbsp;</p>
<h2>N&eacute;zz&uuml;k meg a l&eacute;p&eacute;seket:</h2>
<h3><span>1 - A b&ouml;ng&eacute;sző k&eacute;sz&iacute;t egy http k&eacute;r&eacute;st, amelyet odaad az alatta l&eacute;vő r&eacute;tegnek, aki az alatta l&eacute;vőnek, aki....</span></h3>
<ul>
<li><span>Ismerkedj&uuml;nk a wiresharkkal &eacute;s szűrj&uuml;k le a csomagokat:</span>
<ul>
<li>Olyan csomagot keres&uuml;nk, amelyben van http k&eacute;r&eacute;s &eacute;s a met&oacute;dus GET.&nbsp;
<ul>
<li><span>http.request.method == "GET"</span></li>
</ul>
</li>
</ul>
</li>
</ul>
<ul>
<li><span>N&eacute;zz&uuml;k meg a r&eacute;tegeket egy adott csomagn&aacute;l (TCP/IP modell)</span>
<ul>
<li><span>alkalmaz&aacute;si r&eacute;teg : HTTP: host</span></li>
<li><span>sz&aacute;ll&iacute;t&aacute;si: TCP: src, dst port, FLAGS (ACK- nyugt&aacute;z&aacute;s. PUSH - azonnal adja az alkalmaz&aacute;si r&eacute;tegnek)</span></li>
<li><span>internet (h&aacute;l&oacute;zati): IP: src, dst, version(4), Time To Live</span></li>
<li><span>adatkapcsolati (fizikai): Ethernet: src, dst</span></li>
</ul>
</li>
<li><span>Alkalmaz&aacute;si r&eacute;teg: HTTP</span>
<ul>
<li>szűr&eacute;s: http.request.uri == "/ggombos/upload/"&nbsp;</li>
<li>Jobb klikk arra a csomagra &eacute;s 'Follow'&nbsp; -&gt; 'HTTP Stream'</li>
<li><span>K&eacute;r&eacute;s - v&aacute;lasz fel&eacute;p&iacute;t&eacute;s&eacute;t l&aacute;thatjuk</span></li>
</ul>
</li>
</ul>
<p>&nbsp;</p>
<h3><span>2 - Hova k&uuml;ldj&uuml;k a csomagot ki a hoszt? Ki a c&iacute;mzet? DNS</span></h3>
<ul>
<li>
<div><span>N&eacute;zz&uuml;k meg a dns &uuml;zeneteket a wiresharkban</span></div>
<ul>
<li><span>szűr&eacute;s: dns</span></li>
<li><span>Van egy query &eacute;s egy response &uuml;zenet.&nbsp;</span></li>
<li><span>T&iacute;pusok: A - ipv4, AAAA - ipv6, MX - mail server, NS - nameserver</span></li>
</ul>
</li>
<li>
<div><span>Mi lett az IP c&iacute;me az oktnb147.inf.elte.hu g&eacute;pnek?</span></div>
<ul>
<li><span>szűr&eacute;s: dns.qry.name == "oktnb147.inf.elte.hu"</span></li>
</ul>
</li>
</ul>
<p>&nbsp;</p>
<h3><span>3 - Ki fogja sz&aacute;ll&iacute;tani a csomagokat? TCP / UDP</span></h3>
<ul>
<li><span>Keress&uuml;nk olyan TCP forgalmat, amelynek a syn flag-e be van &aacute;ll&iacute;tva, majd n&eacute;z&uuml;k meg a kommunik&aacute;ci&oacute;&aacute;t</span>
<ul>
<li><span>szűr&eacute;s: tcp.flags.syn == 1</span></li>
<li><span>L&aacute;thatjuk a kapcsolat ki&eacute;p&iacute;t&eacute;st: SYN, SYN-ACK, ACK</span></li>
<li><span>L&aacute;thatjuk a kapcsolat lez&aacute;r&aacute;st: FIN, FIN-ACK, ACK</span></li>
</ul>
</li>
<li><span>N&eacute;zz&uuml;k meg vannak-e udp forgalmak</span>
<ul>
<li><span>szűr&eacute;s: udp</span></li>
<li><span>L&aacute;thatjuk hogy a DNS csomagok is UDP-n kereszt&uuml;l mennek.</span></li>
</ul>
</li>
</ul>
<p>&nbsp;</p>
<h3><span>4 - Ping / Tracert (Traceroute) forgalom! ICMP protokol</span></h3>
<ul>
<li><span>N&eacute;zz&uuml;k meg a ping &eacute;s tracert (traceroute) parancsok csomagjait!</span></li>
<li><span>szűr&eacute;s: icmp</span></li>
<li><span>L&aacute;thatjuk, hogy mely k&eacute;r&eacute;sekre volt v&aacute;lasz.</span></li>
<li><span>L&aacute;thatjuk, hogy a tracert-n&eacute;l (traceroute-n&aacute;l) a TTL mező hogyan v&aacute;ltozik a k&eacute;r&eacute;sekn&eacute;l</span></li>
</ul>
<p><span>Tracert p&eacute;lda:</span></p>
<p><span><img src="https://canvas.elte.hu/courses/63326/files/4213283/preview" alt="tracert_pl.png" width="709" height="240" data-api-endpoint="https://canvas.elte.hu/api/v1/courses/63326/files/4213283" data-api-returntype="File" /></span></p>
<p>&nbsp;</p>
<h3><span>5 - Keress&uuml;k&nbsp; meg a tanrend.elte.hu h&iacute;v&aacute;sunkat is!</span></h3>
<ul>
<li><span>szűr&eacute;s: http.request.uri contains "tanrend"</span>
<ul>
<li><span>nem tal&aacute;lunk semmit</span></li>
</ul>
</li>
<li><span>Keress&uuml;k meg az IP c&iacute;m&eacute;t</span>
<ul>
<li><span>dns.qry.name == "tanrend.elte.hu"</span></li>
</ul>
</li>
<li><span>N&eacute;zz&uuml;k meg a forgalmat erre az IP c&iacute;mre</span>
<ul>
<li><span>ip.dst == 157.181.1.225</span></li>
</ul>
</li>
<li><span>Nem fogjuk l&aacute;tni a forgalmat mert titkos&iacute;tva van. (tls)</span></li>
<li><span>Adjuk meg az ssllog filet, hogy a Wireshark vissza tudja fejteni a forgalmat:</span>
<ul>
<li><span>Edit --&gt; Preferences --&gt; Protocols --&gt; TLS --&gt; (Pre)-Master-Secret log filename</span>
<ul>
<li><span>c:\tmp\ssKeys.log</span></li>
</ul>
</li>
</ul>
</li>
<li><span>Ezut&aacute;n m&aacute;r l&aacute;tjuk a forgalmat!</span></li>
<li><span>Ez a forgalom m&aacute;r HTTP2-n megy, amely stream alap&uacute; &eacute;s lehet t&ouml;bb header is egy csomagban</span></li>
<li><span>Keress&uuml;k meg a GET h&iacute;v&aacute;sunkat:</span>
<ul>
<li><span>szűr&eacute;s: http2.headers.method == "GET"</span></li>
</ul>
</li>
<li><span>Keress&uuml;k meg azokat amelyek a "tanrend.elte.hu"-ra ment&eacute;s egy css-t t&ouml;lt le</span>
<ul>
<li><span>szűr&eacute;s: http2.headers.authority == "tanrend.elte.hu" and http2.headers.path contains ".css"</span></li>
</ul>
</li>
</ul>
<p>&nbsp;</p>
<h2><span>&Ouml;n&aacute;ll&oacute; feladatok</span></h2>
<ol>
<li><span>H&aacute;ny '.css' kiterjeszt&eacute;sű f&aacute;jt szerettek volna let&ouml;lteni? (contains)</span></li>
<li><span>Keresd meg a HTTP POST k&eacute;r&eacute;st &eacute;s&nbsp; n&eacute;zd meg a tartalm&aacute;t. (vagy a Mime-ban, vagy Follow HTTP Stream)</span></li>
<li><span>Keress olyan HTTP v&aacute;laszokat, amelyek sikeresek, &eacute;s amelyek sikertelenek: response k&oacute;d 200, &gt;400.</span></li>
<li><span>N&eacute;zd meg milyen porton h&iacute;vtuk meg a weboldalt!</span></li>
<li><span>Mi lett a c&iacute;me a 'tanrend.elte.hu'-nak? (dns response)</span></li>
<li><span>N&eacute;zd meg milyen portra mennek a DNS k&eacute;r&eacute;sek!</span></li>
<li>Vannak-e benne DHCP k&eacute;r&eacute;sek?</li>
<li>Vannak-e benne ARP k&eacute;r&eacute;sek?&nbsp;
<ol>
<li>H&aacute;ny k&eacute;r&eacute;s ment egy adott IP-re? (pl: arp.dst.proto_ipv4 == 157.181.165.126)</li>
</ol>
</li>
<li>N&eacute;zz&uuml;k meg van-e benne titkos&iacute;tott k&eacute;zfog&aacute;s! (tls.handshake.type) 1 -kliens, 2-szerver</li>
</ol>
<p>&nbsp;</p>
<h2>Tov&aacute;bbi feladatok</h2>
<ol>
<li>T&ouml;lts&uuml;k be a k&ouml;vetkező pcap-et: <a href="https://canvas.elte.hu/courses/63326/files/4213288/download" data-api-endpoint="https://canvas.elte.hu/api/v1/courses/63326/files/4213288" data-api-returntype="File">httpIwiw.pcap</a></li>
<li>Milyen oldalakat k&eacute;rtek le a szűr&eacute;s alapj&aacute;n? Milyen b&ouml;ng&eacute;szőt haszn&aacute;ltak hozz&aacute;?</li>
<li>K&eacute;rdezd le a POST k&eacute;r&eacute;ssel k&uuml;ld&ouml;tt adatokat &eacute;s n&eacute;zd meg mik lettek elk&uuml;ldve! Milyen &Eacute;rdekess&eacute;get l&aacute;tsz?</li>
<li>H&aacute;ny darab .png k&eacute;pet &eacute;rintett a b&ouml;ng&eacute;sz&eacute;s? (help: contains)</li>
<li>H&aacute;ny olyan erőforr&aacute;s volt, amelyet nem kellett &uacute;jra t&ouml;ltenie a b&ouml;ng&eacute;szőnek? Mely oldalakat &eacute;rintette ez? (304 Not Modified)</li>
</ol>
<h2>H&aacute;l&oacute;zati t&aacute;mad&aacute;sok elemz&eacute;se (csak &eacute;rdekess&eacute;g)</h2>
<p><a href="https://www.malware-traffic-analysis.net/2025/01/22/index.html"><span style="font-family: sans-serif; font-size: 1rem;">https://www.malware-traffic-analysis.net/2025/01/22/index.html</span></a></p>