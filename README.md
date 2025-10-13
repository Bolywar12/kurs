# kurs
# 1) OSI modeli — nechta qatlam va PDU formatlari

OSI (Open Systems Interconnection) modeli — tarmoqlarni tushunishni yengillashtirish uchun yaratilgan 7 qatlamli konseptual model. Har bir qatlamning vazifasi va PDU (Protocol Data Unit) formati:

1. **Application (Ilova) — Qatlam 7**

   * Vazifa: Foydalanuvchi dasturlari uchun tarmoq xizmatlarini taqdim etadi (HTTP, FTP, SMTP, DNS va boshqalar).
   * PDU: **Data** (odatda “application data”).

2. **Presentation (Taqdimot) — Qatlam 6**

   * Vazifa: Ma’lumotni formatlash/konvertatsiya qilish, siqish, shifrlash/oshkor qilish (ASCII ↔ Unicode, TLS shifrlash).
   * PDU: **Data**.

3. **Session (Sessiya) — Qatlam 5**

   * Vazifa: Sessiya boshqaruvi — aloqa sessiyalarini o’rnatish, boshqarish, tugatish.
   * PDU: **Data**.

4. **Transport — Qatlam 4**

   * Vazifa: End-to-end yetkazib berish, ishonchlilik, oqim nazorati va portlash (TCP, UDP).
   * PDU: **Segment** (TCP) yoki **Datagram** (UDP).
   * Muhim header maydonlari (misol TCP): source port, dest port, seq, ack, flags (SYN, ACK, FIN), window, checksum.

5. **Network — Qatlam 3**

   * Vazifa: Manzilash va marshrutlash (IP — Internet Protocol).
   * PDU: **Packet**.
   * IP header: version, header length, total length, TTL, protocol, src IP, dst IP, checksum, fragment fields.

6. **Data Link — Qatlam 2**

   * Vazifa: Mahalliy tarmoq ustida ishonchli uzatish, MAC manzillar, framing, xatolikni tekshirish (FCS).
   * PDU: **Frame**.
   * Ethernet frame: Destination MAC, Source MAC, EtherType/Length, Payload, FCS (CRC).

7. **Physical — Qatlam 1**

   * Vazifa: Bitlarni fizik uzatish — kabel, nurlanish, elektr/optik signal, pin/connector.
   * PDU: **Bits** (0/1 signallari).

Oddiy misol (yuqoridan pastga): Application Data → Transport segment (TCP) → Network packet (IP) → Data Link frame (Ethernet) → Physical bits.

---

# 2) Physical layer (Qatlam 1) haqida chuqurroq

**Physical layer** — bu haqiqiy (fizik) uzatish qatlamidir. Har qanday tarmoqning “kabellari, ulagichlari va signallari” shu qatlamga tegishli.

## Asosiy vazifalar

* Bitlarni fizik formatda uzatish (0/1).
* Elektr/optik/radio signallarni yaratish va qabul qilish.
* Line coding (satrlash kodi), modulation (modulyatsiya) va bit synchronisation.
* Fizik interfeys specifikatsiyasi (pinout, voltaj, connector).
* Kanalning fizik parametrlarini boshqarish: bandwidth, attenuation, SNR, jitter.

## Fizik muhitlar (media)

* **To‘rtlik (copper)**: twisted-pair (Cat5e, Cat6, Cat6a), coaxial.
* **Optik tolalar (fiber)**: multimode (OM1/OM2/OM3...), single-mode.
* **Simsiz (wireless)**: RF (Wi-Fi, Bluetooth), millimeter-wave (5G).
* **Boshqalar**: powerline, infrared.

## Signallash va kodlash

* **Line coding**: NRZ, NRZI, Manchester — bitlarni elektr/optik shaklga aylantirish.
* **Modulyatsiya**: ASK, FSK, PSK, QAM — analog kanalda raqamli ma’lumotni uzatishda.
* **Framing fizikasi**: simbol tezligi, bit rate, baud.

## Fizik qurilmalar

* **Hubs** va **repeaters** (faqat fizik qatlamda ishlaydi) — signalni takrorlaydi.
* **Transceiver** (PHY chipset), **SFP**/SFP+ modullar, NIC PHY.
* **Connectors**: RJ45 (Ethernet), LC/SC/ST (fiber), BNC (coax).

## O‘lchovlar va chegaralar (misollar)

* **Ethernet misollari**: 10BASE-T, 100BASE-TX (Fast Ethernet), 1000BASE-T (Gigabit), 10GBASE-T — har biri fizik format va kabel talablari bilan.
* **Kabellar**: Cat5e (1 Gbps, 100 m), Cat6/Cat6a (1 Gbps/10 Gbps, cat6a 100 m uchun mosroq) — (bu misollar umumiy; maxsus sharoitlarda farq bo‘lishi mumkin).

## Xatoliklar va fizik muammolar

* **Attenuation** (signalning zaiflashishi)
* **Noise / EMI** (elektromagnit aralashuvi)
* **Crosstalk** (sathlararo aralashuv) — NEXT/ FEXT
* **Reflection** (improper termination)
* **Kabellar uzilishi yoki yomon kontakt**
* **Simsiz**: multipath, interference, fading

## Qo‘shimcha konseptlar

* **Bandwidth vs Throughput**: fizik bandwidth (Hz) va ma’lumot uzatish tezligi (bit/s) farqi.
* **Multiplexing**: TDM (time-division), FDM (frequency-division), WDM (wavelength-division — fiber uchun).
* **Duplex**: Half-duplex vs Full-duplex — hublar half, switchlar full.

---

# 3) Wireshark’da OSI modelini tahlil qilish — amaliy ko‘rsatma

Wireshark — paketlarni tutish va tahlil qilish uchun eng mashhur vosita. U aslida sizning NIC orqali **link-layer** formatidagi ramkalarni (frames) oladi, lekin paketni yuqoridagi qatlamlarga dekodlab ko‘rsatadi.

## Qanday qilib Wireshark OSI qatlamlariga mos keladi

* **Physical (layer 1)**: Wireshark signallarni emas, balki NIC orqali kelgan **frame**ni oladi — fizik xususiyatlar (voltaj va boshqalar) ko‘rinmaydi. Ammo capture uchun to‘g‘ri interface va SPAN kerak.
* **Data Link (layer 2)**: Wiresharkning “Frame” va “Ethernet” bo‘limlari. MAC manzillar, EtherType, FCS (ba’zi holatlarda).
* **Network (layer 3)**: IP header ko‘rinadi (`Internet Protocol Version 4` yoki `IPv6`).
* **Transport (layer 4)**: TCP/UDP segmentlar — portlar, seq/ack, flags.
* **Application (layer 7)**: HTTP, DNS, TLS va boshqalar dekodlanadi (agar Wireshark protokollar to‘g‘ri aniqlasa).

## Asosiy amaliy funktsiyalar va qadamlar

### 1) Capture va Display filter farqi

* **Capture filter (libpcap BPF)** — paketni tutish paytida cheklaydi (misol: `port 80`, `host 192.168.1.5`).
  Misollar:

  ```text
  port 80
  host 10.0.0.5
  net 192.168.1.0/24
  ```
* **Display filter** — all capture qilingan paketlardan keraklilarini ko‘rsatadi (Wiresharkning o‘z sintaksisi).
  Misollar:

  ```text
  ip.src == 192.168.1.10
  tcp.port == 443
  dns && ip.addr == 8.8.8.8
  http.request.method == "GET"
  tcp.analysis.retransmission
  eth.addr == 00:11:22:33:44:55
  ```

### 2) Paketni tahlil qilish (bir paket bo‘yicha)

1. **Frame (Data Link)**: Frame length, capture length, arrival time.
2. **Ethernet**: dst MAC, src MAC, EtherType (0x0800 → IPv4).
3. **IP (Network)**: src IP, dst IP, TTL, protocol (6 → TCP, 17 → UDP), total length.
4. **TCP/UDP (Transport)**: src port, dst port, flags (SYN, ACK), seq/ack.
5. **Application (HTTP, DNS, TLS...)**: HTTP request line, DNS query/response, TLS ClientHello va hokazo.

Wireshark oynasida: yuqori — paketlar ro‘yxati, o‘rta — tanlangan paketning dekodlangan qatlamlari, past — raw bytes (hex + ASCII).

### 3) Amaliy filter va tekshirishlar

* Barcha HTTP so‘rovlarini ko‘rish: `http`
* Faqat TCP retransmissionlarni topish: `tcp.analysis.retransmission`
* ARP so‘rovlarini ko‘rish: `arp`
* ICMP pinglarni ko‘rish: `icmp`
* Faqat 192.168.1.0/24 tarmog‘idan kelgan paketlar: `ip.src==192.168.1.0/24` (note: Wireshark versiyasiga qarab sintaksis biroz farq qilishi mumkin — `ip.addr==192.168.1.0/24` ishlovchi holatlar bor)

### 4) Foydali funksiyalar

* **Follow → TCP Stream** — bir TCP sessiyani alohida dialog ko‘rinishida ko‘radi (HTTP so‘rov/responselarni qayta tiklash uchun juda qulay).
* **Statistics → Protocol Hierarchy** — protokollar bo‘yicha bo‘lingan statistikani ko‘radi.
* **Statistics → Conversations / Endpoints** — IP/port/ETH darajasida kim bilan kim gaplashgani.
* **Statistics → IO Graphs** — tarmoq trafigi vaqt bo‘yicha grafigi.
* **Analyze → Expert Information** — Wireshark tahlilchisining ogohlantirishlari (retransmission, malformed, checksum error).
* **Name Resolution** — IP→DNS yoki MAC→vendor nomlarini ko‘rsatish (sozlamalarda yoqish/ochirish mumkin).

### 5) Kuzatuv (capture) bo‘yicha amaliy maslahatlar

* **Switch tarmog‘ida to‘g‘ridan-to‘g‘ri tahlil**: port mirroring (SPAN) yoki hubdan foydalanish; aks holda faqat NICga yo‘naltirilgan paketlarni ko‘rasiz.
* **NIC checksum offload**: ko‘pincha NIC-L4 tekshiruvlarini offload qiladi, shuning uchun Wiresharkda checksum xatolari ko‘rinishi mumkin — bu haqiqiy xato emas. Agar kerak bo‘lsa, NIC offload funksiyasini o‘chirib, qayta tuting.
* **Promiscuous mode**: LANdagi barcha ramkalarni qo‘lga olish uchun yoqilishi kerak (Settings → Capture options).
* **Siz TCP reassembly va HTTP reassembly funksiyalarini yoqishingiz mumkin** (Preferences → Protocols → TCP/HTTP).

### 6) Muammolarni aniqlash uchun qadamlar (misol: sekin yuklanish / retransmission)

1. `tcp.analysis.retransmission` yoki `tcp.analysis.fast_retransmission` bilan paketlarni qidiring.
2. `tcp.window_size` va `tcp.analysis.window_update`ni tekshiring (oqim nazorati bilan bog‘liq muammo).
3. `ip.frag_offset` va `ip.flags.mf` — fragmentatsiya bo‘lsa tekshiring.
4. ARP so‘rovlar (agar host topa olmayotgan bo‘lsa): `arp` filtri.
5. DNS kechikishi: `dns` statistikasi va `udp.port==53` bilan tahlil.
6. Qo‘shimcha: `tcp.options.mss` va MTU masalalari.

### 7) Qo‘llaniladigan display filter misollari (tez-tez kerak bo‘ladiganlar)

```text
# Faqat bir IP manzildan kelgan paketlar
ip.addr == 192.168.1.10

# Bir nechta portlar
tcp.port == 80 || tcp.port == 443

# Faqat TLS/HTTPS traffik
tls

# TCP 3-way handshakega oid paketlarni ko'rish (SYN yoki SYN+ACK)
tcp.flags.syn == 1

# Faqat HTTP GET so'rovlari
http.request.method == "GET"

# Muammolar: retransmission yoki out-of-order
tcp.analysis.retransmission || tcp.analysis.out_of_order
```

## Misol o‘rganish ssenariysi

Agar sizga bir sayt sekin ochilsa:

1. Wiresharkda capture oching (faqat host/maqsad port).
2. `tcp.analysis.retransmission` va `tcp.analysis.delay` filtrlari bilan qidiring.
3. `Follow TCP Stream` bilan sessiyani kuzatib, HTTP javob vaqtini o‘lchang.
4. `Statistics → IO Graphs` orqali tarmoqda tutilish yoki burstlarni tekshiring.
5. Agar MAC bo‘yicha muammo bo‘lsa — `eth.addr == aa:bb:cc:...` filtri.

---

# Yakun — tavsiyalar va tez ko‘rsatmalar

* OSI qatlamlari: **7 qatlam** — har bir qatlami uchun PDU nomini yodda saqlang: Application/Presentation/Session → **Data**; Transport → **Segment/Datagram**; Network → **Packet**; Data Link → **Frame**; Physical → **Bits**.
* Physical qatlam: kabel/connector, signallash va modulyatsiya masalalari — diagnostika qilishda har doim fizik sabablarni (kabellar, port, signal darajasi) tekshiring.
* Wireshark: **Capture filter** (oldindan) vs **Display filter** (keyin), `Follow TCP Stream`, `tcp.analysis.*`, `Statistics` menyulari — asosiy vositalar. NIC offload va switch SPANni esdan chiqarmang.

Agar xohlasangiz, men sizga:

* qisqacha **qo‘l kitob** (one-page cheat-sheet) tayyorlab beraman (komandalar, eng ko‘p ishlatiladigan filtrlarga ega), yoki
* konkret **Wireshark tahlil misoli** — siz bergan .pcap faylni (yoki ekranlardan nusxa) tahlil qilib, qatlam-qatlam bo‘yicha izoh beraman.

Qaysi variantni xohlayapsiz?
