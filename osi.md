# OSI Modeli — To'liq Tushuntirish

OSI (Open Systems Interconnection) modeli — kompyuter tarmoqlarida ma'lumot almashinuvi jarayonini 7 ta qatlamga bo'lib tushuntiruvchi nazariy model. ISO tomonidan 1984-yilda ishlab chiqilgan.

## 7 ta qatlam (yuqoridan pastgacha)

### 1. Ilova qatlami (Application Layer — 7-qavat)
- **Vazifasi**: Foydalanuvchi va ilovalar uchun tarmoq xizmatlarini taqdim etish.
- **Protokollar**: HTTP/HTTPS, FTP, SMTP, DNS, Telnet, SNMP.
- **Qurilmalar**: Brauzer, email klientlari, proxy server, application gateway.
- **PDU**: Data.
- **Misol**: Veb-sayt ochish (HTTP).

### 2. Taqdimot qatlami (Presentation Layer — 6-qavat)
- **Vazifasi**: Ma'lumotlarni formatlash, shifrlash, siqish.
- **Protokollar**: SSL/TLS, JPEG, GIF, MPEG.
- **Qurilmalar**: Asosan dasturiy ta'minot.
- **PDU**: Data.
- **Misol**: HTTPS orqali shifrlangan aloqa.

### 3. Seans qatlami (Session Layer — 5-qavat)
- **Vazifasi**: Ulanishlarni ochish, boshqarish va yopish.
- **Protokollar**: NetBIOS, RPC, PPTP.
- **Qurilmalar**: Asosan dasturiy.
- **PDU**: Data.
- **Misol**: Video qo'ng'iroq sessiyasi.

### 4. Transport qatlami (Transport Layer — 4-qavat)
- **Vazifasi**: End-to-end ishonchli yetkazib berish, segmentlash, oqim nazorati.
- **Protokollar**: TCP (ishonchli), UDP (tez).
- **Qurilmalar**: Stateful firewall, load balancer.
- **PDU**: Segment (TCP) / Datagram (UDP).
- **Misol**: Fayl yuklashda paketlar tartibda kelishi.

### 5. Tarmoq qatlami (Network Layer — 3-qavat)
- **Vazifasi**: Logik manzillash (IP), marshrutlash.
- **Protokollar**: IP, ICMP, OSPF, BGP.
- **Qurilmalar**: Router, Layer 3 switch.
- **PDU**: Packet.
- **Misol**: Internet orqali paketlarni boshqa tarmoqqa yo'naltirish.

### 6. Ma'lumot uzatish qatlami (Data Link Layer — 2-qavat)
- **Vazifasi**: Fizik manzillash (MAC), xatolarni aniqlash.
- **Protokollar**: Ethernet, Wi-Fi (802.11), PPP.
- **Qurilmalar**: Switch, bridge, Wi-Fi access point.
- **PDU**: Frame.
- **Misol**: Mahalliy tarmoqda kompyuterlar o'rtasidagi aloqa.

### 7. Fizik qavat (Physical Layer — 1-qavat)
- **Vazifasi**: Bitlarni fizik muhit orqali uzatish.
- **Standartlar**: Ethernet kabellari, Wi-Fi signallari, RS-232.
- **Qurilmalar**: Hub, repeater, modem, NIC (tarmoq karta), kabellar.
- **PDU**: Bit.
- **Misol**: Kabel orqali elektr signal o'tishi.

## Ma'lumot oqimi (Enkapsulyatsiya)
- Yuboruvchida: Yuqoridan pastga → har bir qavat header qo'shadi.
  - Data → Segment → Packet → Frame → Bit
- Qabul qiluvchida: Pastdan yuqoriga → headerlar olib tashlanadi (dekapsulyatsiya).

## Eslab qolish uchun mnemonika
- Inglizcha: **All People Seem To Need Data Processing**
- O'zbekcha variant: **Ilova Taqdimot Seans Transport Tarmoq Uzatish Fizik**

Ushbu model tarmoq muammolarini aniqlashda juda qulay vosita hisoblanadi.
