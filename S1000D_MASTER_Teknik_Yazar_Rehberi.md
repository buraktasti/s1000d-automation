# S1000D MASTER TEKNİK YAZAR REHBERİ
### Koluman Otomotiv Endüstri A.Ş. — Askeri Projeler Müdürlüğü / ELD Birimi

**Kaynaklar:** `Bilgi_Setleri-S1000D_Issue_5_0.pdf` (Chapter 5.1–5.2.1.21, 273 sayfa) + `Bilgi_Setleri-002-S1000D_Issue_5_0.pdf` (Chapter 5.2.3, 28 sayfa) — toplam 301 sayfa, sayfa sayfa/satır satır okunmuş, 26 bilgi setinin tamamı işlenmiştir.

> **Bu dokümanın amacı:** Bilgi setleri, teknik yazara **rehberlik** eder — bir konu hakkında *hangi başlık altında, hangi detay seviyesinde, hangi akışta, ne yazılması/ne yazılmaması gerektiğini* tarif eder. Bu doküman DMC (veri modülü kodlama) kurallarını **içermez** — sadece "ne ve nasıl yazılır" sorusuna odaklanır. DMC/SNS kodlama ayrı bir aşamada, ayrı bir referansla ele alınacaktır.

---

## İÇİNDEKİLER

- [0. Teknik Yazar İçin Altın Kurallar](#0-teknik-yazar-icin-altin-kurallar)
- [1. Bilgi Seti → Şema Türü Hızlı Referans Tablosu](#1-sema-turu-tablosu)
- [Ek Bölüm — BIKE Örnekleriyle Doğrulanmış Şema Türü Rehberi](#ek-bike-sema-rehberi)
- [2. Ortak Bilgi Setleri (Chapter 5.2.1)](#2-ortak-bilgi-setleri)
  - 5.2.1.1 — Mürettebat/Operatör Bilgisi
  - 5.2.1.2 — Tanım ve Çalışma Bilgisi
  - 5.2.1.3.1 — Bakım Prosedürleri
  - 5.2.1.3.2 — Arıza İzolasyonu (Bakım Teknisyeni Seviyesi)
  - 5.2.1.3.3 — Tahribatsız Muayene
  - 5.2.1.3.4 — Korozyon Kontrolü
  - 5.2.1.3.5 — Depolama
  - 5.2.1.4 — Elektrik/Kablo Bilgisi
  - 5.2.1.5 — Resimli Parça Kataloğu
  - 5.2.1.6 — Bakım Planlama Bilgisi
  - 5.2.1.7 — Kütle ve Denge Bilgisi
  - 5.2.1.8 — Kurtarma Bilgisi
  - 5.2.1.9 — Ekipman/Sökülmüş Bileşen Bilgisi
  - 5.2.1.10 — Silah Yükleme Bilgisi
  - 5.2.1.11 — Kargo Yükleme Bilgisi
  - 5.2.1.12 — Depo (Mühimmat-dışı) Yükleme Bilgisi
  - 5.2.1.13 — Rol Değişikliği Bilgisi
  - 5.2.1.14 — Muharebe Hasarı Değerlendirme ve Onarım
  - 5.2.1.15 — Resimli Alet/Destek Ekipmanı
  - 5.2.1.16 — Servis Bültenleri
  - 5.2.1.17 — Malzeme Verisi
  - 5.2.1.18 — Ortak Bilgi ve Veri
  - 5.2.1.19 — Eğitim
  - 5.2.1.20 — Uygulanabilir Yayın Listesi
  - 5.2.1.21 — Bakım Checklist ve Muayeneleri
- [3. Kara/Deniz Özel Bilgi Setleri (Chapter 5.2.3)](#3-karadeniz-ozel-bilgi-setleri)
  - 5.2.3.1 — Sürücü Tanım Bilgisi
  - 5.2.3.2 — Sürücü Çalıştırma Bilgisi
  - 5.2.3.3 — Sürücü Sıralı/Checklist Çalıştırma
  - 5.2.3.4 — Sürücü Seviyesi Arıza Tespit/Çözüm
  - 5.2.3.5 — Yasal/Düzenleyici Muayene

---

## 0. TEKNİK YAZAR İÇİN ALTIN KURALLAR {#0-teknik-yazar-icin-altin-kurallar}

Bu 6 kural, tüm bilgi setlerinde tekrar tekrar karşımıza çıkan, S1000D'nin en temel yazım disiplinidir:

### 1. Her konu kendi kutusuna (bilgi setine) yazılır, tekrar edilmez
Bir bilgi başka bir yayında/bölümde zaten varsa, buraya kopyalanmaz — sadece **referans verilir**. Özellikle sürücü el kitabı ve özel koşul (ısı/soğuk/su geçişi/NBC) bölümlerinde: "standart pratikler veya genel bilgi başka yayında tanımlıysa dahil edilmez, sadece referans verilir" kuralı sıkça tekrarlanır.

### 2. Arıza bulma bilgisinde İKİ FARKLI SEVİYE vardır — karıştırılmamalı
- **Bakım teknisyeni seviyesi** (Fault Isolation): Detaylı, parça değiştirme seviyesine kadar götüren, **kendi içinde eksiksiz** prosedür. Kullanıcıyı asla başka bir dokümana yönlendirmez.
- **Sürücü/operatör seviyesi** (Fault Detection, Isolation and Resolution): Basit, semptom bazlı — "eylem → soru → cevap → ya çöz ya bakım ekibini çağır" akışı. Derin sökme/parça değiştirme detayına inmez.

Hangi seviyede yazdığımızı (kime hitap ettiğimizi) net bilmeden bu bölümü yazmaya başlamamalıyız.

### 3. Basit konu bölünmez, karmaşık konu bölünür
Tanım/açıklama yazarken: konu basitse ("nasıl yapıldığı" ve "fonksiyonu" birbirinden ayrılamayacak kadar iç içeyse) **tek bir açıklama** yeterlidir. Konu karmaşıksa (motor gibi), "nasıl yapıldığı" ile "fonksiyonu" ayrı ayrı ele alınabilir. Gereksiz bölme, gereksiz basitleştirme kadar yanlıştır.

### 4. Prosedürler kendi içinde tam olmalı, gereksiz adım içermemeli
Bir bakım/arıza prosedürü yazarken: gerekli **tüm** ölçüm/tork/malzeme/alet bilgisi ilgili adımın **içinde** verilir — okuyucu başka bir yere bakmak zorunda kalmamalı. Aynı zamanda **gereksiz adım eklenmemeli** — en doğrudan, en kısa yol izlenmeli.

### 5. Her bilgi setinin "ne yazılmaz" tarafı da vardır
Örnekler: NDT'ye rutin görsel muayeneler girmez; Korozyon Kontrolü'nde yüklenici spesifikasyonlarına referans verilmez; BDAR kendi başına yeterli olmalı, dış referans içermemeli; Servis Bülteni'nin özeti, bültenin işini yapacak kadar detaylı olmamalı. Her bölümün altında bu "ne yazılmaz" uyarıları ayrıca işaretlenmiştir.

### 6. İllüstrasyon çoğu zaman metnin kendisi kadar zorunludur
Kontrol/gösterge açıklaması, bakım adımı, arıza izolasyon şeması, kablo şeması gibi birçok bilgi seti illüstrasyonsuz eksik sayılır — "illüstrasyon zorunlu" ifadesi metinde sıkça tekrar eder.

---

## 1. BİLGİ SETİ → ŞEMA TÜRÜ HIZLI REFERANS TABLOSU {#1-sema-turu-tablosu}

Aşağıdaki tablo, hangi bilgi setinin FORSDOC'ta genellikle hangi **şema türüyle** eşleştiğini gösterir (proje İş Kuralları/BREX ile teyit edilmelidir — bazı satırlar proje kararına göre değişebilir, bu satırlar belirtilmiştir). Kesin DMC/SNS kodlama kuralları bu dokümanın kapsamı dışındadır.

| Bilgi Seti | Genellikle Kullanılan Şema Türü |
|---|---|
| 5.2.1.1 Crew/Operator Information | crew, descript, proced |
| 5.2.1.2 Description and Operation | descript |
| 5.2.1.3.1 Maintenance Procedures | proced |
| 5.2.1.3.2 Fault Isolation | fault |
| 5.2.1.3.3 Non-Destructive Testing (NDT) | proced |
| 5.2.1.3.4 Corrosion Control | descript, proced |
| 5.2.1.3.5 Storage | proced |
| 5.2.1.4 Wiring Data | descript (diyagram), wrngdata (liste/veri) |
| 5.2.1.5 Illustrated Parts Data (IPD) | ipd |
| 5.2.1.6 Maintenance Planning Information | schedul |
| 5.2.1.7 Mass and Balance Information | descript |
| 5.2.1.8 Recovery Information | proced, crew |
| 5.2.1.9 Equipment Information (SE/TE/CM) | descript, proced, fault, ipd (birlikte) |
| 5.2.1.10 Weapon Loading Information | proced |
| 5.2.1.11 Cargo Loading Information | proced |
| 5.2.1.12 Stores Loading Information | proced |
| 5.2.1.13 Role Change Information | proced |
| 5.2.1.14 Battle Damage Assessment and Repair (BDAR) | proced |
| 5.2.1.15 Illustrated Tool and Support Equipment (ITE) | descript |
| 5.2.1.16 Service Bulletins | sb |
| 5.2.1.17 Material Data | descript / process (proje kararına bağlı) |
| 5.2.1.18 Common Information and Data (CID) | comrep / descript (proje kararına bağlı) |
| 5.2.1.19 Training | learning |
| 5.2.1.20 List of Applicable Publications (LOAP) | descript / pm (proje kararına bağlı) |
| 5.2.1.21 Maintenance Checklists and Inspections | schedul, proced |
| 5.2.3.1 Crew/Operator Descriptive Information (Land/Sea) | descript |
| 5.2.3.2 Crew/Operator Operation Information (Land/Sea) | crew, proced |
| 5.2.3.3 Crew/Operator Sequential Operation Information (Land/Sea) | schedul |
| 5.2.3.4 Crew/Operator Fault Detection, Isolation and Resolution (FDIR) | fault, proced |
| 5.2.3.5 International, National and Regulatory Scheduled Check (INRSC) | schedul, proced |

---

## EK BÖLÜM — BIKE ÖRNEKLERİYLE DOĞRULANMIŞ ŞEMA TÜRÜ REHBERİ {#ek-bike-sema-rehberi}

> **Neden bu bölüm var:** "Bu VM için hangi şema türünü ve hangi infoCode'u kullanacağım?" — en çok zaman/işçilik harcanan karar noktası budur. Aşağıda, ASD'nin resmi **S1000D 5.0 BIKE referans projesinden** gerçek, üretimde kullanılmış **80 adet** veri modülü şema türüne göre gruplanmış ve neyi anlattığı özetlenmiştir. Kafanız karıştığında önce buraya, sonra ilgili bilgi setinin detaylı "Ne Yazılır" bölümüne bakın.

### `descript` — Tanım/Açıklama (9 örnek incelendi)

**Ne zaman kullanılır:** Bir şeyin NE OLDUĞUNU ve NASIL ÇALIŞTIĞINI anlatmak için. Eylem/adım içermez, sadece anlatır/tarif eder. "X nedir, ne işe yarar, nasıl çalışır" sorusuna cevap veriyorsanız descript kullanın.

**Gerçek BIKE örnekleri:**

| infoCode | Konu | Bu VM'de ne anlatılmış |
|---|---|---|
| 041A | Bicycle | Description of how it is made |
| 042A | Bicycle | Description of function |
| 041A | Wheel | Description of how it is made |
| 041A | Brake system | Description of how it is made |
| 041A | Steering | Description of how it is made |
| 041A | Headset | Description of how it is made |
| 041A | Frame | Description of how it is made |
| 041A | Drivetrain | Description of how it is made |
| 041A | Gears | Description of how it is made |

### `proced` — Prosedür (33 örnek incelendi)

**Ne zaman kullanılır:** Adım adım bir İŞLEM yaptırmak için (sökme, takma, servis, test, onarım, modifikasyon). "Şunu yapın, sonra bunu yapın" tarzı bir talimat yazıyorsanız proced kullanın. S1000D'nin en çok kullanılan şema türüdür.

**Gerçek BIKE örnekleri:**

| infoCode | Konu | Bu VM'de ne anlatılmış |
|---|---|---|
| 121A | Bicycle | Pre-operation procedures (crew) |
| 151A | Bicycle | Post-operation procedures (crew) |
| 258A | Bicycle | Other procedures to clean |
| 258B | Bicycle | Other procedures to clean |
| 330A | Bicycle | Place on test stand |
| 663A | Bicycle | Standard repair procedures |
| 341A | Fork | Manual test |
| 520A | Fork | Remove procedures |
| 720A | Fork | Install procedures |
| 933A | Fork | Replacement procedure |
| 93AA | Bicycle axis | Modification procedures |
| 921A | Inner tube | Remove and install a new item |
| 215A | Tire | Fill with air |
| 362B | Tire | Check pressure |
| 921A | Tire | Remove and install a new item |
| 520A | Rear wheel | Remove procedures |
| 520A | Front wheel | Remove procedures |
| 720A | Front wheel | Install procedures |
| 341A | Brake system | Manual test |
| 251A | Brake pads | Clean with rubbing alcohol |
| 520A | Front brake | Remove procedures |
| 720A | Front brake | Install procedures |
| 520A | Stem | Remove procedures |
| 720A | Stem | Install procedures |
| 520A | Handlebar | Remove procedures |
| 720A | Handlebar | Install procedures |
| 520A | Headset | Remove procedures |
| 720A | Headset | Install procedures |
| 720A | Spacer | Install procedures |
| 921A | Horn | Remove and install a new item |
| 241A | Chain | Oil |
| 251B | Chain | Clean with chemical agent |

### `fault` — Arıza (4 örnek incelendi)

**Ne zaman kullanılır:** Bir arızayı TANIMLAMAK (isolatedFault/detectedFault/observedFault/correlatedFault) veya arızayı GİDERMEK için izlenecek karar ağacını (faultIsolation) anlatmak için. "Bu arıza oldu, sebebi bu, çözümü bu" ya da "şu soruları sırayla sorup arızayı bulun" yazıyorsanız fault kullanın.

**Gerçek BIKE örnekleri:**

| infoCode | Konu | Bu VM'de ne anlatılmış |
|---|---|---|
| 400A | Front wheel | Fault reports and isolation procedures |
| 412A | Rear wheel | Detected fault |
| 411A | Horn | Isolated fault |
| 414A | Drive train | Correlated fault |

### `ipd` — Resimli Parça Kataloğu (2 örnek incelendi)

**Ne zaman kullanılır:** Bir montajın parça-parça dökümünü (illüstrasyon + parça numarası + adet) vermek için. Metin neredeyse hiç yoktur, veri tablo/görsel ağırlıklıdır.

**Gerçek BIKE örnekleri:**

| infoCode | Konu | Bu VM'de ne anlatılmış |
|---|---|---|
| 941A | Bicycle | Illustrated Parts Data - IPD |
| 941A | Bicycle | Illustrated parts data |

### `schedul` — Bakım Planlama / Checklist (4 örnek incelendi)

**Ne zaman kullanılır:** HANGİ bakımın NE ZAMAN yapılacağını (süre limiti, muayene aralığı, görev listesi) veya sıralı bir checklist'i tanımlamak için. Kendisi bakımın NASIL yapılacağını anlatmaz — proced'deki prosedürlere referans/link verir.

**Gerçek BIKE örnekleri:**

| infoCode | Konu | Bu VM'de ne anlatılmış |
|---|---|---|
| 000A | Bicycle | Time limits |
| 000A | Bicycle | Scheduled maintenance lists |
| 000A | Bicycle | Scheduled maintenance checks |
| 916A | Bicycle | Maintenance Allocation Chart |

### `sb` — Servis Bülteni (1 örnek incelendi)

**Ne zaman kullanılır:** Üreticinin sonradan yayınladığı modifikasyon/uyarı/özel muayene bildirimini duyurmak için. Kendi başına detaylı prosedür yazmaz — proced/ipd DM'lerine referans vererek "bu değişikliği şu adımlarla yapın" der.

**Gerçek BIKE örnekleri:**

| infoCode | Konu | Bu VM'de ne anlatılmış |
|---|---|---|
| 930A | Bicycle | Service Bulletin - Replacement of standard forward fork by telescopic fork |

### `learning` — Eğitim İçeriği (5 örnek incelendi)

**Ne zaman kullanılır:** Bir konuyu ÖĞRETMEK için (kavram tekrarı, bilgi kontrolü, interaktif prosedür eğitimi). Bakım/tanım DM'lerinin içeriğini KOPYALAMAZ, onlara referans verir — sadece eğitime özgü ek materyali (soru-cevap, senaryo) içerir.

**Gerçek BIKE örnekleri:**

| infoCode | Konu | Bu VM'de ne anlatılmış |
|---|---|---|
| 200A | Bicycle | Servicing: Prerequisite concept review |
| 952A | Bicycle | Performance support |
| 041A | Wheels | Description of how it is made: Knowledge Check |
| 520A | Front wheel | Remove procedures: Interactive content - Procedure |
| 041A | Steering | Description of how it is made: Knowledge Check |

### `crew` — Mürettebat/Operatör (2 örnek incelendi)

**Ne zaman kullanılır:** Operatöre atfedilen tanım veya normal operasyon prosedürünü işaretlemek için. Bu proje örneklerinde çoğunlukla descript/proced içeriğinin "crew" etiketli bir varyantı şeklinde görülüyor.

**Gerçek BIKE örnekleri:**

| infoCode | Konu | Bu VM'de ne anlatılmış |
|---|---|---|
| 043A | Bicycle | Description attributed to crew |
| 131A | Bicycle | Normal operation procedures (crew) |

### `process` — Süreç (1 örnek incelendi)

**Ne zaman kullanılır:** Birden fazla alt-adımı/kararı içeren daha üst seviyeli bir kullanım sürecini (örn. "bisiklete binme") anlatmak için — proced'e benzer ama daha genel/bütünsel bir süreç akışı tarif eder.

**Gerçek BIKE örnekleri:**

| infoCode | Konu | Bu VM'de ne anlatılmış |
|---|---|---|
| 130A | Bicycle | Riding a bicycle |

### `comrep` — Ortak Bilgi Deposu (CIR) (6 örnek incelendi)

**Ne zaman kullanılır:** Birden çok DM'de tekrar tekrar geçecek ortak veriyi (uyarı listesi, dikkat listesi, organizasyon listesi) TEK YERDE tanımlayıp diğer DM'lerin buna referans vermesini sağlamak için. Aynı bilgiyi her DM'de tekrar yazmamak için kullanılır.

**Gerçek BIKE örnekleri:**

| infoCode | Konu | Bu VM'de ne anlatılmış |
|---|---|---|
| 00KA | Mountain bicycle | Organizations common information repository |
| 0A4A | Mountain bicycle | Warnings - List of warnings in the common information repository |
| 0A5A | Mountain bicycle | Cautions - List of cautions in the common information repository |

### `frontmatter` — Ön Bölüm (3 örnek incelendi)

**Ne zaman kullanılır:** Yayının başlık sayfası, içindekiler, geçerli DM listesi gibi idari/yapısal ön bilgilerini vermek için.

**Gerçek BIKE örnekleri:**

| infoCode | Konu | Bu VM'de ne anlatılmış |
|---|---|---|
| 001A | Bicycle | Title page |
| 002A | Bicycle | List of effective data modules |
| 009A | Bicycle | Table of contents |

### `brex` — İş Kuralları Değişimi (BREX) (3 örnek incelendi)

**Ne zaman kullanılır:** Projenin S1000D iş kurallarını makine-okunabilir biçimde tanımlamak için — teknik yazarın doğrudan içerik yazdığı bir şema değildir, proje kurulumunun parçasıdır.

**Gerçek BIKE örnekleri:**

| infoCode | Konu | Bu VM'de ne anlatılmış |
|---|---|---|
| 022A | Mountain bicycle | Business rules exchange |

### `brdoc` — İş Kuralları Dokümanı (3 örnek incelendi)

**Ne zaman kullanılır:** BREX'in insan-okunabilir, dokümante edilmiş halini vermek için.

**Gerçek BIKE örnekleri:**

| infoCode | Konu | Bu VM'de ne anlatılmış |
|---|---|---|
| 024A | Mountain bicycle | Business rules |
| 024A | S1000DBIKE | Business rules document |

### `appliccrossreftable` — Uygulanabilirlik Çapraz Referans Tablosu (2 örnek incelendi)

**Ne zaman kullanılır:** Farklı konfigürasyon/model varyantlarının hangi koşullarda geçerli olduğunu tanımlamak için — proje kurulumunun parçası.

**Gerçek BIKE örnekleri:**

| infoCode | Konu | Bu VM'de ne anlatılmış |
|---|---|---|
| 00WA | — | — |
| 0A3A | Mountain bicycle | Applicability cross-reference table catalog |

### `condcrossreftable` — Koşul Çapraz Referans Tablosu (1 örnek incelendi)

**Ne zaman kullanılır:** Uygulanabilirlik koşullarının (applic) tanımlarını merkezi olarak vermek için — proje kurulumunun parçası.

**Gerçek BIKE örnekleri:**

| infoCode | Konu | Bu VM'de ne anlatılmış |
|---|---|---|
| 00QA | Mountain bicycle | Conditions cross-reference table |

### `prdcrossreftable` — Ürün Çapraz Referans Tablosu (1 örnek incelendi)

**Ne zaman kullanılır:** Ürün varyantlarının (model/seri) tanımlarını merkezi olarak vermek için — proje kurulumunun parçası.

**Gerçek BIKE örnekleri:**

| infoCode | Konu | Bu VM'de ne anlatılmış |
|---|---|---|
| 00PA | Mountain bicycle | Products cross-reference table |

### Somut içerik örnekleri (gerçek VM'lerden alınmış, kısaltılmış)

**`proced` — Chain Oil (241A, Servicing):** Prosedür, önce ön koşul olarak "bisiklet zinciri temiz ve kuru olmalı" şartını, gerekli kişi/beceri seviyesini (Operatör, sk02) ve gerekli malzemeyi (temiz kuru bez, parça numarasıyla) tablo halinde verir — asıl yağlama adımı bundan SONRA gelir. Yani bir proced DM'i, adımdan önce "bunlar olmadan başlama" listesini eksiksiz vermelidir.

**`fault` — Rear Wheel Detected Fault (412A):** Kısa ve net üç parçadan oluşur: arızanın açıklaması ("arka teker düzgün çalışmıyor"), arızayı tespit eden parça (Lastik) ve arızalı olduğu düşünülen parça (Arka teker) — parça adı+üretici kodu+parça numarasıyla kimliklendirilmiş. Sonda bir not: "lastiği sökmeye hazırlanın" (bir sonraki adıma yönlendirme, ayrı DM'e değil, mantıksal sıraya işaret).

**`ipd` — Bicycle (941A):** İlk girişte ürünün tamamı tek satır: parça adı "Bicycle", üretici kodu, parça numarası, sonra ek veri alanları (kaynak/bakım onarılabilirlik kodu, miktar, NATO stok numarası benzeri kodlar). Metin yoktur — her satır yapılandırılmış veridir. Alt bileşenler aynı desende, artan "item" numarasıyla devam eder.

**`schedul` — Scheduled Maintenance Checks (D05-40):** "Pre-ride" (biniş öncesi) gibi bir kontrol aralığı tanımlanır, ardından bir görev listesi ("Inspect Brakes" gibi) verilir — her görev kendi prosedür DM'ine (proced) **referans** verir, prosedürün kendisini burada tekrar yazmaz.

---

## 2. ORTAK BİLGİ SETLERİ (Chapter 5.2.1) {#2-ortak-bilgi-setleri}

## 5.2.1.1 — Crew/Operator Information — Mürettebat/Operatör Bilgisi

*Genellikle kullanılan şema türü: **crew, descript, proced***

Operatörün aracı yeterli düzeyde anlaması için gereken bilgi. Kontrol/gösterge bilgisi, ön/son operasyon prosedürleri, operasyon ve acil durum prosedürlerini kapsar.

---

## 5.2.1.2 — Description and Operation — Tanım ve Çalışma Bilgisi

*Genellikle kullanılan şema türü: **descript***

**Amaç:** Bakım personelinin bir sistemin/alt sistemin/alt-alt sistemin/ünitenin yapısını, fonksiyonunu, çalışmasını ve kontrolünü anlaması.

**Ne yazılır — seviyeye göre:**
- **Sistem/Alt Sistem seviyesi:** Sistemin ve alt sistemlerin amacı, fonksiyonel kapsamı, alt-alt sistemlerle ve diğer sistemlerle ilişkisi.
- **Alt-alt Sistem seviyesi:** Alt-alt sistemin fonksiyon/çalışma/kontrol açıklaması + alt-alt sistemdeki ana bileşenlerin genel amacı, işlev kapsamı ve **konumu**. Diğer alt-alt sistem/sistemlerle ilişkiler dahil edilmeli.
- **Assembly (bileşen) seviyesi:** Bireysel ana bileşen/montajların fonksiyon, çalışma, kontrol **detaylı** açıklaması — **testler dahil**. Performansı etkileyen ayarlamalar. Özel bakım uygulamaları veya elleçleme prosedürleri (varsa).

**Her seviyede standart 3 illüstrasyon/tablo önerilir:**
1. **Component index** — ilgili bileşenlerin alt sistem/alt-alt sisteme çapraz referanslı listesi
2. **Access/Area identification** — erişim açıklıklarının/konumlarının listelenmiş illüstrasyonu (indekslenen bileşenlere erişim yolu/konumu gösterir)
3. **Component location/identification** — indekslenen bileşenlerin bilinen yapısal/sistem özelliklerine göre fiziksel konumunu gösteren illüstrasyon (devre kesici/sigorta gibi küçük parçalar illüstre edilmeyebilir, panel konumu + bileşen referansı yeterli)

**Basit sistem kuralı:** Basit sistemlerde IC 040 (nasıl yapıldığı ve fonksiyonu) **bölünmemeli**. İçerik mekanik trainee'nin anlayabileceği düzeyde, açık/mantıklı/kolay okunur üslupla ve iyi illüstre edilmiş olmalı — **eğitim amaçlı da kullanılabilir** nitelikte yazılmalı.

**Operation (IC 1YY):** Aracı çalıştırmak için gerekli tüm prosedürler — kontrol/gösterge verisi, ön/son operasyon prosedürleri, operasyon ve acil durum prosedürleri.

**Şema diyagramları — 3 seviye:**
- **Block schematic:** Karmaşık devreleri basitleştirmek için, descriptive kısımda kullanılır. Uzman olmayan personel de sistemi anlayabilmeli. Ana değişim parçalarını ve aralarındaki ilişkiyi hızlı anlatmak amaçlı — eğitim yardımcısı niteliğinde, geniş elektrik bilgisi gerektirmeden.
- **Simplified schematic:** Block'tan daha geniş kapsamlı ama ünitenin araç üzerindeki konumundan bağımsız, elektriksel olarak doğru. Sistemin/alt sistemin genel elektrik çalışmasını göstermek ve eğitimde daha detaylı anlayış sağlamak için.
- **Detailed schematic:** Lojik veya iki-durumlu cihaz kullanan elektronik sistem/bileşenleri göstermek için. Fiziksel yapıyı göstermeden fonksiyon/çalışmayı anlatır. Alt sistem bakımı için yeterli detayı sağlamalı.

---

## 5.2.1.3.1 — Maintenance Procedures — Bakım Prosedürleri

*Genellikle kullanılan şema türü: **proced***

**Amaç:** Bakım personelinin test ekipmanı/güç kaynağı bağlama-ayırma, özel alet/destek ekipmanı kullanma, ürünü servis etme, testleri yapma, arızaları giderme, sistem/bileşenleri en kısa sürede söküp-takma yapabilmesini sağlamak.

- **Servicing (IC 2YY):** Diğer bakım eylemleri sonucu normal olarak gereken servis prosedürleri. Mümkünse kendi içinde tam (self-contained) olmalı; rutin veya onarıcı nitelikte olabilir.
- **Examinations/tests/checks (IC 3YY):** Testler, değiştirilen ünitenin çalıştığı koşullara göre karmaşıklık/sıkılıkta değişir. Karmaşık bir tam sistem testi, değiştirilen ünite sistemi aktive edip go/no-go spesifikasyonu içinde çalışıyorsa gerekmez. Test tekrarını önlemek için bakım prosedürleri, sadece ana test prosedürü öyle üniteleştirilmişse (parça/ünite testi çakışmadan çağrılabiliyorsa ve tüm testin tamamlanmasını gerektirmiyorsa) bir teste referans vermeli — bu durumda özel başlangıç/bitiş talimatları verilmeli.
- **Disconnect/remove/disassemble (IC 5YY):** Bir bileşen/montaj/alt-montaj/ünite/parça kombinasyonunun sökümünü açıklar — erişim sağlayıp donanımı sökmek için gerekli **mantıksal iş akışı sırasında adım adım** işlem net verilmeli. Aynı veya yedek parçanın gelecekte takılmasına ilişkin söküm varyasyonlarına dikkat çekilmeli. Gerekli malzeme/alet/tertibat/destek ekipmanı listesi **prosedürün başında tablo halinde** verilmeli. Ön koşul prosedürleri (örn. "xyz erişim panelini aç") verilmeli veya uygun şekilde referans verilmeli. Panel açma talimatları panel numarasına referans vermeli. Bireysel görevden önce belgelenmesi gereken tüm ölçüm/değerler ilgili adımın başında listelenmeli.
- **Repairs (IC 6YY):** Aşınmış/hasarlı parçayı servis edilebilir duruma getirmek için gereken adım adım onarım süreçleri ve spesifikasyonlar, mantıksal iş akışında. Onarım seviyesi, ürünün bakım konseptinin gerektirdiği restorasyon seviyesini göstermeli. Her onarım DM'i kendi içinde tam olmalı ve şunları içermeli: onarım alanlarının temel durum/konum görünümleri; ilgili bitiş, referans boyutlar, akış hızı gibi veriler; özel boyutsal talimatlar; onarımın bütünlüğünü belirlemek için gerekli muayene gereksinimleri (test gerekiyorsa referans verilmeli).
- **Assemble/install/connect (IC 7YY):** Bir bileşen/montaj/alt-montaj/ünite/parça kombinasyonunun ve ilişkili parçaların kurulumunu açıklar. Uygulanabilirse, panel kapatma gibi kurulum sonrası işlemler de açıklanmalı. Temel ve erişim donanımını kurmak için gerekli mantıksal iş akışında adım adım işlemler net anlatılmalı. Tüm ölçüm/değerler (özel tork değerleri gibi) başka bölüme referans vermeden doğrudan adım metninde verilmeli. Gerekli alet/ekipman kullanımını gösteren uygun illüstrasyonlar eşlik etmeli — her illüstrasyonun parçaları numaralı callout ile işaretlenmeli, adım metni bu numaralara referans vermeli. Kurulum/reaktivasyonun parçası olarak test gerekiyorsa dahil edilmeli veya referans verilmeli.

---

## 5.2.1.3.2 — Fault Isolation — Arıza İzolasyonu (Bakım Teknisyeni Seviyesi)

*Genellikle kullanılan şema türü: **fault***

**Amaç:** Yetkin personelin, izleme sistemleri/mürettebat-bakım personeli/uçuş sonrası muayeneler tarafından sağlanan raporları analiz edebilmesi. Veri doğruluğuna göre analiz, muayene/kontrol/test, düzeltici eylem veya arıza izolasyon prosedürüyle devam etmeli.

**Fault list'lerde (isolated/detected/observed/correlated) yazılması gerekenler:**
- Bakım ekibine gösterildiği haliyle mesaj kimliği
- Kısa ve/veya detaylı açıklama
- Arızayı tespit eden LRU (kimlik, isim, kısaltma)
- Arızalı LRU (kimlik, isim, kısaltma) — Detected fault list'te bu "potansiyel arızalı LRU listesi" olarak verilir, her potansiyel LRU için: doğrulama testi (test/BIT açıklaması veya bakım prosedürü DM referansı) + arıza bu LRU için doğrulanırsa düzeltici eylem
- Sistemi normal koşula döndürecek düzeltici eylem
- Varsa arızalı SRU da belirtilmeli
- **Observed fault list** için: semptom açıklaması **basit ve belirsizlik içermemeli**. Arıza belirsizse (birden fazla LRU sorumlu olabilir): ürün arıza izolasyon prosedürü DM referansı VEYA potansiyel arızalı LRU listesi verilmeli
- **Correlated fault list** için: ilişkilendirilmiş mesaj/uyarı listesi (arıza koduyla tanıtılan); opsiyonel olarak her arıza parçası için: tanıtıldığı DM referansı, kısa/detaylı açıklama, tespit bilgisi; ayrıca tüm ilişkili arıza için izolasyon bilgisi ve opsiyonel notlar

**Fault Isolation Procedure'da yazılması gerekenler:**
- Her arıza izolasyon prosedürü, arızayı izole etmek için gereken **tüm işlemleri** içermeli ve **düzeltme talimatlarıyla sonlanmalı**. En doğrudan ve en kısa izolasyon yöntemi olmalı — **gereksiz adım içermemeli**.
- Aynı prosedür, her biri o prosedür içinde yeterince izole edilebiliyorsa **birden fazla arıza kodu için** kullanılabilir.
- Arıza izolasyonu, **bileşen değiştirme seviyesine** veya ürün üzerinde gerçekleştirilebilecek başka bir eyleme (kablo araştırması, kart değiştirme, ayar gibi) kadar götürülmeli.
- ⭐ **Her DM, kendi içinde her arıza kodu için TAM prosedürü içerir** — bilgi normalde birden fazla farklı DM ile ilişkilendirilse bile. **Prosedürler, izolasyonu tamamlamak için kullanıcıyı başka bir DM'e YÖNLENDİREMEZ.**
- Birden fazla bileşen aynı düzeltici eyleme dahilse, tüm bileşenler **arıza olasılığı sırasına göre** listelenmeli.
- İçerik: arıza kodu, arıza açıklaması (kısa/detaylı), ön koşullar, arıza izolasyon prosedürünün kendisi. Diyagram ve/veya yapılandırılmış izolasyon adımları dizisi ile anlatılabilir.
- **Her izolasyon adımı**: bir eylem (ne yapılmalı?) → bir soru (eylemin sonucu nedir?) → olası cevaplar. Her adım prosedürdeki başka bir adıma referans verir (karar ağacı yapısı).

**Fault isolation task supporting data:** Sadece **kesinlikle gerekliyse** üretilmeli. Mümkün olduğunda mevcut DM'ler (blok diyagramları, kablo şemaları, konum çizimleri, erişim düzeni) bunun yerine kullanılmalı.

**Fault code index / Maintenance message index:** Türetilmiş (derived) bilgidir — sistem için tüm arıza kodlarının/bakım mesajlarının sayısal sırayla listesi, her biri için arıza izolasyon prosedürüne bir veya daha fazla DM referansı verir.

---

## 5.2.1.3.3 — Non-Destructive Testing (NDT) — Tahribatsız Muayene

*Genellikle kullanılan şema türü: **proced***

**Amaç:** Yetkin NDT teknisyenlerine sistem/alt sistem ve parçaları üzerinde NDT tekniklerini uygularken talimat ve rehberlik sağlamak.

**NDT bilgisi şu konuları içermeli:** genel bilgi, dye penetrant, magnetic particle, eddy current, X-ray, ultrasonic, gamma-ray, resonance frequency, thermographic.

**Her test (self-standing DM) şu standart başlıkları içermeli:**
1. **Applicability** — etkilenen parçayı/parçaları tanımla/konumlandır; parça numarası, seri numarası, ürün seri model numarası vb. tüm gerekli tanımlayıcıları ver
2. **Tools/equipment/materials** — enstrüman/ekipman/malzeme için minimum kabul edilebilir performans standartları; yalnızca bir üreticinin ürünü onaylıysa bunun sebebi belirtilmeli; gerekli tertibat/prob/transdüser/referans standart/özel aletler listelenmeli, yerel yapılabilecek parçalar için çizim verilmeli
3. **Item description** — test edilecek parça tanımlanmalı/kimliklendirilmeli; yüzey durumuna duyarlıysa malzeme, ısıl işlem durumu, üretim yöntemi, yüzey işlemleri belirtilmeli; olası arıza ve sebebi açıklanmalı, arızanın konumu illüstrasyonda gösterilmeli
4. **Preparation and cleaning** — erişim gereksinimleri, elektrik gücü açık/kapalı olmalı mı, özel çalışma standı, sıvı giderme gereksinimleri, özel temizlik/yüzey kaplama giderme gereksinimleri, gerekli uyarı/dikkat notları
5. **Test equipment adjustment**
6. **Procedure** — gerekirse, başlangıç testinin kesin veri sağlamadığı durumlar için bir **doğrulama (backup) test prosedürü** başka bir yöntemle belirtilmeli
7. **Indication evaluation** — arızaların ölçüleceği kesin koşullar
8. **Acceptance and rejection standards** — arıza tanımlanmalı ve gereken eylem belirtilmeli; gerçek arıza limitleri verilmeli veya ilgili yayının limitler bölümüne referans verilmeli
9. **Restoring the Product** — ürünü çalışır duruma geri getirme açıklaması (takip bakımı)

**Başlık kuralı:** Data modülü başlığı, test edilen parçanın adını içermeli — bu ad, ilgili resimli parça dökümünde verilen ad olmalı.

**Ne dahil edilmez:** Teknisyenler tarafından normalde yapılan büyüteç/borescope/fiber optik gibi rutin görsel muayeneler NDT bilgisine dahil edilmez, ürünün ilgili bakım DM'lerine dahil edilmeli. (İstisna: doğrulama prosedürü olarak veya test alanını tanımlamak için kullanılıyorsa dahil edilebilir, ama detaylı prosedür olması gerekmez.)

---

## 5.2.1.3.4 — Corrosion Control — Korozyon Kontrolü

*Genellikle kullanılan şema türü: **descript, proced***

**Amaç:** Tüm bakım seviyelerindeki personele korozyon önleme ve kontrolü için talimat/rehberlik sağlamak. Korozyon hasarının konum/kapsamının tespiti, sınıflandırılması, giderilmesi ve tedavisi talimatlarını kapsar.

**Genel korozyon kontrolü kapsamalı:** genel güvenlik verisi, korozyon kontrolü hakkında genel veri, ürünün korozyon kontrolü/önlemesinin açıklaması (korozyon kontrolünün amacı, kullanılan özel malzeme/sarf malzemesi dahil korozyon önleyici bileşikler, korozyon türleri, korozyon kontrol faktörleri, korozyon önleyici bakım, ana yapısal grup kırılımı, korozyona eğilimli sistemler/bileşenler), temizlik ve yüzey hazırlığı, korozyon kontrolü için boya giderme, korozyon hasarının muayenesi, korozyonun mekanik giderilmesi, kimyasal korozyon giderme ve yüzey işlemi (alüminyum/magnezyum alaşımları, paslanmaz çelik ve nikel bazlı alaşımlar, bakır ve bakır bazlı alaşımlar, titanyum bazlı alaşımlar, kaplı ve fosfatlı yüzeyler için özel talimatlar dahil), tipik alanların tedavisi, korozyon önleyici bitirme (astar, emaye, üst kaplama uygulama/rötuş verisi dahil), korozyon önleyici sızdırmazlık sistemleri.

**Sistem korozyon kontrolü** — belirli sistem/bileşen/parçalar için (yakıt sistemi, uçuş/operasyon kontrolleri, iniş takımı/şasi, motorlar, dış depolar, dış işaretler, gövdeler, güverte makineleri) — her korozyona eğilimli ve kritik alan/öge için, uygunsa şu bakım DM'leri üretilmeli: özel öge/alanları temizle/yıka ve geçici koru; korozyona eğilimli/kritik alanlara erişim sağla; korozyona eğilimli/kritik alanları muayene et (NDT kullanımı dahil); korozyon türünü tanımla; korozyonlu alanlardan boyayı gider; korozyon hasarını değerlendir; korozyon hasarını gider; korozyon giderildikten sonra bitirme sistemini yeniden boya/rötuşla; korozyona eğilimli/kritik alanları sızdırmaz hale getir; korozyona eğilimli/kritik alanlara erişimi kapat.

**Yapısal korozyon kontrolü — ana içerik:** Her büyük yapısal bileşen tartışılmalı ve içerik daha da bölünerek şunları kapsamalı: korozyona eğilimli ve kritik ögeler/alanlar, korozyon giderme, malzeme tedavisi, bitirme süreçleri ve korozyon önleyici sızdırmazlık süreçleri. Bileşen açıklaması korozyona eğilimli/kritik alanları da içermeli, ilgili özel korozyon kontrol verisinin nerede bulunacağını belirtmeli.

**Yasak:** CC bilgisi hiçbir yüklenicinin süreç ve/veya malzeme spesifikasyonuna referans veremez. Özel veya patentli ekipman CC'de belirtilemez.

---

## 5.2.1.3.5 — Storage — Depolama

*Genellikle kullanılan şema türü: **proced***

**Amaç:** Tüm bakım seviyelerindeki personelin ürünü (kurulu sistem, ekipman ve bileşenler dahil) depolayabilmesi, depoda muayene edebilmesi ve depodan çıkarabilmesi.

**Kapsam:** Ürünün kullanan kuruluşa teslim edilmeden önceki hazırlık ve sevkiyatını kapsamaz. Yedek parçaları kapsamaz (ürünün parçası olarak depolanmadıkça).

**İçermesi gerekenler:** Genel depolama gereksinimleri; ürün depolama bilgisi ve prosedürleri — şunlar dahil: genel ürün depolama bilgisi (depolama türlerinin tanımları), geçici depolama (30 güne kadar, 30-90 gün), uzun süreli depolama (90 gün üzeri), koruma, koruma malzemelerinin giderilmesi, ürünü konteynerlere yerleştirme prosedürleri, konteynerlerden çıkarma prosedürleri, depoda servis edilebilir tutma prosedürleri, depolanan ürünün başka bir yere taşınması, depolama sonrası kullanıma hazırlama, kullanım öncesi depodan kabul etme, depodaki ürünün ömür verisi, depolama sırasında ürünü sabitlemek için gereken bağlama noktaları ve kablo mukavemeti.

**Genel bakış illüstrasyonu:** Geçici ve uzun süreli depolama için tüm koruyucu kapakların, korunacak alanların ve bağlama noktalarının konumunu gösteren bir genel bakış illüstrasyonu içermeli.

**Uzun süreli depolamada özellikle dikkat edilmesi gerekenler:** ürün öncesinde çıkarılması gereken ögeler (aküler, patlayıcı cihazlar, güvenlik ekipmanı, oksijen tüpleri); yağmur/toz/kuş/haşere girişini önlemek için kapatılması gereken açıklıklar; havalandırma için hava kanalı/tahliye deliği konumu; güneş ışığından korunması gereken akrilik pencereler/yüzeyler; boyasız metal yüzeylerin genel konumu.

---

## 5.2.1.4 — Wiring Data — Elektrik/Kablo Bilgisi

*Genellikle kullanılan şema türü: **descript (diyagram), wrngdata (liste/veri)***

**Amaç:** Elektrik devrelerini, yetkin bakım personelinin elektrik sistemlerinde arıza izolasyonu ve bakım yapabileceği kadar yeterli detayda açıklamak.

**İçermesi gerekenler:**
- **Descriptive information — Electrical identification system:** Üründe kullanılan elektrik kimliklendirme sisteminin açıklaması; sistem ve alt sistem kodlarıyla birlikte elektrik/elektronik/aviyonik sistemlerin listesi.
- **Connection units:** Konnektörler, terminal blokları, modüler terminal blokları, splice'lar, bonding gibi bağlantı ünitelerinin açıklaması (illüstrasyonla desteklenmeli).
- **Wires/harnesses/conductors:** Tek tel, çoklu tel, ekranlı tel, bükümlü tel gibi tüm tel/demet türlerinin açıklaması, tel boyutları/sınıflandırma/iletken ve tel türlerini veren illüstrasyon/tablolar dahil.
- **Standard practices:** Proje/kuruluşa özel standart elektrik bakım pratikleri ve onarım bilgisi (tel sonlandırma, konnektör/splice kurulumu, ekranlama/toprak kayışı ve toprak stud/demetleri için sonlandırma noktası hazırlığı gibi). Elektrik/elektronik iletken, ayırıcı ve sonlandırma noktalarının kurulum/bakım/onarımı için özel bakım pratikleri de dahil. Gerekli alet/ekipman bilgisi. Standart pratikler: süreklilik testi, izolasyon testi, gerilim kırılma testi, koaksiyel kablo testi (time domain reflectometry), optik fiber testi, MIL-Bus testi, bonding testi.
- **Wiring diagrams:** Tüm terminal noktaları, tel kimlik kodları, ekipman kimlikleri, bağlantı kutuları, ekranlar, iç jumperlar, toprak bağlantıları gösterilmeli. Her terminal noktası kimliklendirilmeli. Telin bağlandığı her noktada tel numarası belirtilmeli. Yedek teller "Spare" olarak not edilmeli. Terminal ve ayırma noktaları arası, elektrik bileşenleri arası tel bağlantıları mümkün olduğunca direkt çizilmeli. Devre kesicileri besleyen tüm birincil/ikincil bara için güç dağıtım şemaları verilmeli.
- **Numeric/Alphabetic index:** Her sistem için tüm diyagramları listeleyen indeks — başlık, DM kodu, şekil no, uygulanabilirlik bilgisi.
- **Harness drawings:** Kurulum, rota, düz düzen çizimleri. Ürünün her zone/alt-zone'unda demet kurulumunu göstermeli.
- **Equipment and panel locations:** Ana elektrik/elektronik bileşenlerin veya bileşen gruplarının konumu (ilgili sistem dokümantasyonundaki konum illüstrasyonlarında yer almayanlar). Panel/istasyon hattı/su hattı/pruva hattı gibi bir referans sistemine göre kimliklendirilmeli.
- **Electrical standard parts data:** Her standart parça için teknik bilginin özeti, teknik standartlara dayalı.
- **Electrical equipment information:** Fişler/soketler, terminaller, splice'lar, toprak bağlantıları, anahtarlar, röleler, lambalar, dirençler, diyotlar ve fonksiyonel öge referanslı diğer elektrik/elektronik ekipman.
- **Wire data:** Devre kodu, tel kimlik numarası, tel bölüm kimliği, demet kimliği, renk, uzunluk, EMC kodu, bağlantı bilgisi (kimden-kime ekipman, kontakt, ekran), uygulanabilirlik, izlenebilirlik.
- **Harness data:** Demet kimliği, parça numarası, dash numarası, issue, illüstrasyon referansı, uygulanabilirlik.

**Not:** Kablo şeması kullanmak zorunlu değildir — descriptive şema ile de tablo/diyagram şeklinde kablo yayını hazırlamak mümkündür, ama bu durumda interaktif kablo yayını fonksiyonları (ağ analizi, görünüm/filtre, bağlam duyarlı veri sunumu) kullanılamaz/gerekmez.

---

## 5.2.1.5 — Illustrated Parts Data (IPD) — Resimli Parça Kataloğu

*Genellikle kullanılan şema türü: **ipd***

Bu bölüm kısa bir yönlendirme paragrafıdır — yedek parça bilgisinin yayınlanma kurallarını kapsar. Proje kararıyla IPD, ayrı bir yayın yerine başka bir yayının (örn. ekipman bakım yayını) parçası olarak da sağlanabilir; bu durumda bir bilgi seti üretilmeli ve yayınlama sürecine entegre edilmeli.

---

## 5.2.1.6 — Maintenance Planning Information — Bakım Planlama Bilgisi

*Genellikle kullanılan şema türü: **schedul***

**Amaç:** Yetkin personelin ürünün bakımını yönetebilmesi için önleyici kontrol ve bakım (zamanlı ve zamansız) gereken kuralları vermek.

**Time limits (Süre limitleri):** Belirli sistemde kurulu, belirli sayıda çevrim veya zaman aralığı sonunda servis edilebilir ekipmanla değiştirilmesi gereken ekipman listesi. Her satır: **Equipment** (isim, kimlik numarası/parça numarası, CSN, ISN), **Quantity** (bir üst montajda kurulu toplam adet), **Category** (proje/kuruluş tanımlı, örn. güvenlik nedeniyle değiştirilen), **Time limit** (elapsed süre veya değiştirme çevrimi), **Applicability**.

**Maintenance/inspection task lists:** Belirli sistem için, bir veya daha fazla farklı kontrol sırasında yapılması gereken bakım/muayene görevleri listesi. Her satır: **Task** (kısa eylem açıklaması), **Preliminary requirements**, **Reference** (görevi açıklayan DM/yayın referansı), **Name** (sistemin adı), **Equipment**, **Supervise** (gerekli denetim seviyesi), **Check interval/limit**, **Applicability**, **Related task** (ilişkili zamanlı bakım görevi, zorunlu değil ama ekonomik avantaj sağlar — After/Before/Complied/Finished/Precludes/Started/With kodları).

**Scheduled/unscheduled checks:** Her kontrol için bir DM olmalı, iki kısım içermeli: kontrol hakkında bilgi + bu kontrol sırasında yapılan bakım/muayene görevleri listesi. İçerik: **Limit** (kontrolün yapılması gereken süre/çevrim/olay), **Remarks**, **Requirement source**, **Equipment information**, görev listesi (Reference/Task/Applicability).

**Maintenance allocation:** Bakım fonksiyonları + bu fonksiyonlar için bakım seviyeleri ve süre bilgisi. Tanımlı son ürün veya bileşen üzerinde bakım fonksiyonlarının yerine getirilmesi için genel bakım otoritesi ve sorumluluğu verir. Bakım fonksiyonları gruplanabilir. Gerekli tüm bilgi Logistics Support Analysis Record (LSAR)'dan gelir. Ayrıca alet listesi ve notlar içerir.

---

## 5.2.1.7 — Mass and Balance Information — Kütle ve Denge Bilgisi

*Genellikle kullanılan şema türü: **descript***

**Kapsadığı konular:** Seviyeleme ve tartma - genel, kütle ve denge, seviyeleme/trimleme, tartma, kütle ve ağırlık merkezi verisi.

**Charts/forms/records:** Kütle-denge kaydı için kullanılan grafik/form/kayıt örnekleri (bakım personeli kaydı, temel kütle checklist kaydı, tartma kaydı, temel kütle-denge kaydı, yükleme verisi, kütle-denge izin formu/manifestosu).

**Mass and balance computers/automated systems:** Amaç, hesaplamalar, yazılım, program değişiklikleri, kullanım kolaylığı, hesaplama gereksinimleri, doğruluk, kullanım yetkisi, talimat kitabı, bilgisayar kullanım kısıtlamaları, donanım gereksinimleri.

**Principles:** Kütle, denge, kütle-denge etkileri, kütle-denge kontrolü, performans etkileri.

**Control (management):** Kütle-denge sınıfları, kütle-denge yayınları, kütle-denge uçuş izni, tartma gereksinimleri (tartmanın kendisi değil), kütle-denge grafik/formları, safra beyanları.

**Weighing the Product:** Tartma ekipmanı, tartma aksesuarları, tartma prosedürleri, ağırlık merkezi konumu için gereken ölçüler, ölçüm alma, kütle/ölçü kaydı, tartma sonuçlarının doğrulanması, tartma kontrol listesi.

**Product reports:** Marka/model/seri/tescil kimliği, gerçek tartma tarihi, ekipman listesinin kırılımında kullanılan ürün bölümlerini tanımlayan diyagram, sertifikasyon tartma kaydı ve teslimata kadar tüm sonraki ayarlamalar, teslimat kalkış kütle-denge izin formu, SNS'e çapraz referanslı ekipman ve sıvı listesi.

**Center of gravity loading calculations:** En ileri ve en geri ağırlık merkezi koşullarının hesaplanma prensipleri; izin verilen limitler dışında kalan bir görev için düzeltme yöntemlerinin belirlenmesi.

---

## 5.2.1.8 — Recovery Information — Kurtarma Bilgisi

*Genellikle kullanılan şema türü: **proced, crew***

**Kapsadığı konular:** Giriş, muayene ve hazırlık, ürünü stabilize etme/kaldırma, ürünü hareket ettirme, destek ve kurtarma ekipmanı.

**Introduction:** Boyutlar, yer boşlukları, kriko/kaldırma noktaları, istasyon diyagramları, servis noktası konumları, yakıt tankı konumları, yük/silah/dış depo düzenlemeleri, güçlü nokta konumları, iniş takımı ayak izi boyutları gibi genel ürün verilerine referanslar dahil olmalı (bunlarla sınırlı değil).

**General and quick reference checklist:** Kurtarma operasyonuyla birlikte veya öncesinde yapılması gereken idari faaliyetler.

**Preliminary examination:** Ürün hasarının ilk taraması bilgisi; ürünün yapısal durumu, kaza yerinin zemin durumu ve bunların kurtarma sorununa yaklaşımı üzerindeki etkisi.

**Damage control and safety procedures:** Kurtarma prosedürleri yapılırken kişilere zarar vermeyi veya ek ürün hasarını önlemek için gerekli veri. Şunları kapsamalı (bunlarla sınırlı değil): tehlikeli sıvıların tümünün tahliyesi, kanopilerin/fırlatma koltuklarının/minyatür patlayıcı kordonların/kapsüllerin/kaza kayıt cihazlarının etkisiz hale getirilmesi, tüm gaz/hidrolik sıvı basıncının serbest bırakılması, akülerin ayrılıp çıkarılması.

**Mass and center of gravity:** Yük (kargo uçakları için), silah/dış depolar (askeri araçlar için) ve/veya yakıt çıkarılırken ürünün kütle, moment kolu ve ağırlık merkezindeki değişiklikleri gösteren grafikler. Tüm birincil sökülebilir bileşenlerin kütle listesi de dahil edilmeli.

**Removal of payload/weapons/external stores:** Kargo kapısının normal ve manuel açılması, farklı konfigürasyon ve yük türü için yük çıkarma verisi. Askeri ürünler için silah/dış depoların elleçlenmesi konusunda uyarılar ve çıkarma prosedürleri de dahil edilmeli.

**De-fueling:** İllüstre yakıt boşaltma prosedürleri (olası koşullar altında), anormal ürün duruşları, ürün elektrik gücü olan/olmayan durumlar dahil. Gerekli tüm özel adaptörler listelenmeli. Çeşitli ürün kurtarma duruşları nedeniyle çıkarılamayan yakıt listelenmeli.

**Removal of primary components:** Bakım bilgi setinin DM'leriyle kapsanmayan birincil bileşenlerin çıkarma prosedürleri.

**Stabilizing/lifting the Product:**
- **Preparation:** Tüm özel ekipman listesi (çelik plaka, hava yastığı gibi) ve hava yastıkları ile yapı/yüzey arasındaki yapılanma gibi veriler.
- **Stabilizing procedures:** Ürünü kurtarma prosedürleri sırasında istenmeyen hareketi önlemek için stabilize etmek için kullanılabilecek prosedürler üzerine veri. Kurtarma sırasında hakim rüzgarların ürün üzerindeki etkilerini gösteren illüstrasyon/tablolar da dahil edilmeli. Gerekirse, yeterli tutma için kabloların gerilme mukavemeti gibi diğer veriler dahil edilmeli.
- **Lifting the damaged Product:** Ürünü kaldırma, kaldırma krikoları üzerine konumlandırma, davit kaldırma, araç sapanlama yöntem ve prosedürleri.

**Moving the Product:**
- **General preparation:** Ürünü hareket ettirmek için gereken tüm özel ekipman/malzeme listesi.
- **Towing/winching/dragging-off in abnormal conditions:** Kablo ve halatları bağlama prosedürleri. İniş takımı veya diğer bağlama noktalarına uygulanabilecek maksimum kuvvetlerin illüstrasyonu verilmeli.
- **Transportation of the damaged Product:** Ürünü taşımaya hazırlama; iniş takımları kullanılamaz durumdayken taşıma; nakliye için sistem/birincil bileşenleri koruma; bileşenleri nakliye konteynerlerine paketleme; nakliye konteynerlerinin nasıl yapılacağı (illüstrasyonlarla).

**Support and general recovering equipment:** Özel-tip destek/kurtarma ekipmanı ve genel destek/kurtarma ekipmanı listeleri.

---

## 5.2.1.9 — Equipment Information (SE/TE/CM) — Ekipman/Sökülmüş Bileşen Bilgisi

*Genellikle kullanılan şema türü: **descript, proced, fault, ipd (birlikte)***

**Kapsam:** Support Equipment (SE), Training Equipment (TE), Component Maintenance (CM) bilgisi. Yetkin personelin SE/TE'yi hazırlaması, çalıştırması, bakımını yapması ve üründen çıkarılan bileşenleri bakımını yapabilmesi.

**İçerebileceği DM türleri:**
- **Front matter/Introduction:** Güvenlik ve veri kısıtlamaları (düzenleyici, ihracat, IP, garanti dahil); kalite güvence (doğrulama, verifikasyon, yöntemler dahil); kısaltma/terim/sembol/spesifikasyon-dokümantasyon listeleri; genel uyarı/dikkat/not'lar; önerilen onarım istasyonları.
- **Functional and technical descriptions:** Fonksiyon açıklaması, teknik açıklamalar, elektrik/elektronik veri, yazılım verisi, diyagram/şema.

**Description of function (IC 040, IC 042, IC 044 ile):** Temel fonksiyonel açıklama bilgisi HER ZAMAN, bu bilgi setinde sağlanan bakım talimatlarını yapmak için yeterli detayda verilmelidir. Fonksiyonun açıklaması ekipmanın **nasıl çalıştığını** anlatmak içindir, **"nasıl kullanılacağını" anlatmak için değildir**. Şunları içermeli:
- Ünitenin ve her ana alt-montajın (modül/kart) fonksiyonel açıklaması + birbirleriyle ve son ürün ünitesiyle fiziksel/fonksiyonel ilişkisi. Gerektiğinde alt-montajlar arası bağlantıların blok diyagramları dahil edilmeli.
- Normal çalışma sırasında karşılaşılmayan özel çalışma koşullarının açıklaması (aşırı buz, sıcak, soğuk koşullar ve bunların bileşen çalışmasına etkisi gibi örnekler)
- Kritik değerler ve özel önlemler
- Ünitenin bir sistem içindeki genel fonksiyon açıklaması verilebilir; bileşenin farklı uygulamalarda kullanılabildiği durumlarda özellikle **belirli bir sistemdeki kurulum/arayüzüne dair açık referanslardan kaçınılmalı**
- Mekanik bileşenin çalışması sırasında oluşabilecek karmaşık sıvı akışları, basınçlar, sıcaklıklar, hızlar açıklaması
- Metin açıklamalarını desteklemek için basitleştirilmiş/kısmi blok, devre ve lojik şemaları **bolca kullanılmalı** (bunlar sadece açıklayıcı amaçlıdır, detaylı şema/diyagramların yerini tutmaz)
- Karmaşık devreleri basitleştirmek için blok diyagramları
- Basit ögeler için blok diyagramın yerini tutabilecek veya karmaşık ögeler için tamamlayabilecek şematik diyagramlar
- İlgiliyse, ünitenin yapımında kullanılan malzeme bilgisi + tehlikeli malzemelerin elleçlenmesi için uyarı/dikkat notları. Ünitenin bakımını desteklemek için gerekiyorsa üretim süreç açıklamaları dahil edilmeli.

**Technical description:** Temel teknik açıklama bilgisi HER ZAMAN sağlanmalı. Önemli parametreler, bu bilgi setinde sağlanan bakım talimatlarını desteklemek için yeterli detayda tanımlanmalı. Uygulanabilirse dahil edilmesi gerekenler: temel fiziksel özellikler (boyut, ağırlık vb.), güç gereksinimleri, giriş/çıkış özellikleri, içerikler (yağlayıcı/hidrolik sıvı gibi), ünitedeki ana alt-montajların açıklaması (fiziksel konumları dahil), ana kontrol/gösterge/dış bağlantıları gösteren en az bir genel dış illüstrasyon, teknisyenin ünitenin temel fiziksel düzenini yeterince anlayabilmesi için gereken ek kesit/plan görünüm illüstrasyon/diyagramları, birden fazla parça numarası kapsanıyorsa konfigürasyon farklılıkları açıklaması (uygulanabilirse değiştirilebilirlik/modifikasyon bilgisi).

**Electrical and electronic data:** Devrelerin fonksiyon/çalışmasının detaylı açıklaması; bileşene tasarlanmış karmaşık/olağandışı devrelerin açıklaması; programlanmış cihazların destekleyici diyagramları (teknisyenin bakım talimatlarını gerçekleştirmesi için gerekiyorsa); üretici literatüründe özel uygulamalar için operasyonel tartışma bulunmayan özel/programlanmış entegre devrelerin detaylı açıklamaları; lojik diyagramları/bilgisi; NVM açıklaması (ne kaydediliyor, nasıl/ne zaman/nerede kaydediliyor ve siliniyor, hangi format kullanılıyor, servis içi/atölye-özel fonksiyonellik); dahili öz-test/durum izleme özellikleri açıklaması (BIT/BITE lojiği dahil — kullanıcının fonksiyonu anlayabilmesi için).

**Software data:** Normal çalışma açıklaması (temel mimari ve temel program fonksiyonları); yazılım modüllerinin fonksiyonel açıklaması ve ünitle ilişkisi (giriş/çıkış verisi ve lojik akış bilgisi dahil).

**Diagrams and schematics:** Ekipman/bileşen için tam devre diyagramları seti. Kablo diyagramları (bileşenlerin fiziksel düzenini, kablo demetlerini gösteren, bağlantı noktaları/ara bağlantılar/bileşenleri tanımlayan — IC 051, 053); Wiring list (karmaşık bileşenler için diyagramda gösterilen tüm telleri sonlanma sırasına göre listeler — pin numarası, tel numarası, diyagram referansı, to-from rota; sistem arayüz konnektör pinleri ve ilgili sinyaller dahil, kullanılmayan/yedek teller tanımlanmalı — IC 057, 058); Şematik diyagramlar (devrelerin elektriksel düzenini gösterir — IC 054); IPD DM'lerinde tanımlanan fonksiyonel ögeler (referans tanımlayıcılar).

**Operation (sadece SE/TE için):** Kontrol ve göstergeler; ön-operasyon prosedürleri; operasyon prosedürleri (otomatik ve manuel her ikisi de uygulanabiliyorsa ikisi de verilmeli; sadece çalıştırılacak cihazlarla ilgili — etkileri "Controls and indicators" altında anlatılır; **acil durum prosedürleri içermemeli**, ayrı başlıkta verilmeli); acil durum prosedürleri; son-operasyon prosedürleri.

**Maintenance and servicing:**
- **Cleaning (IC 25Y):** Ekipman temizliği için özel yöntem ve süreçler veya uygun standart pratiklere referanslar. Mantıksal iş akışı takip eden adım adım prosedürler. Temizlik prosedürleri, önceden uygulanmış onarımları (bonded/dolgu/lehimli parçalar gibi) dikkate almalı.
- **Inspection (IC 28Y):** Muayene gerektiren her öge için tablo: öge açıklaması, parça numarası, CSN, limit, limite ulaşıldığında yapılacak prosedürün DM kodu.
- **Examinations/tests/checks (IC 3YY):** Karmaşıklık ve sıkılık değişir. Bilgi, ekipmanın servis edilebilirliğini belirlemek için gereken muayene/test/kontrolleri **net tanımlamalı**. Prosedürler, parça değişimi/servis/onarım yoluyla üniteyi servise geri döndürmeye yönelik olmalı. Ünitenin servis edilebilir durumunu belirlemek için gereken minimum muayene/test/kontrol seti net tanımlanmalı. Her muayene/test/kontrolün olası sonuçlarına dayanarak prosedür şu belirlemelerden biri için koşulları vermeli: tanımlı servis edilebilir limitler içinde devam eden operasyon için kabul edilebilir; prosedürde referans verilen diğer DM'lere göre ek muayene/test/kontrol gerekir; prosedürde referans verilen parça değişimi veya servis prosedürleriyle restore edilebilir; prosedürde referans verilen özel onarım prosedürlerine göre onarılabilir; artık servis edilebilir, restore edilebilir veya onarılabilir değil.
- **Fault isolation (IC 4YY):** Ögedeki arızaları tespit etmek ve arızalı bileşen/parçayı izole edip değiştirme/onarıma geçmek için gereken prosedürler. **Çoğu durumda arızalar, muayene/test/kontrollerde kapsanır**, ancak bazı durumlarda arızayı izole etmek için **ek arıza izolasyon prosedürleri gerekebilir**. Sağlandığında, muayene/test/kontrolleri **tamamlayıcı** nitelikte olmalı. Bu prosedürler ayrıca BIT, BITE, arıza kaydı ve NVM'de depolama özellikleri gibi arıza raporlama özelliklerini desteklemek için ek arıza belirleme talimatları sağlamak için de kullanılır. Prosedürler her montaj/alt-montaj/parçayı **aşamalı olarak** izole edip tanımlayacak şekilde düzenlenmeli. Her arıza izolasyon prosedürü arızayı izole etmek için gereken tüm işlemleri içermeli ve düzeltme talimatlarıyla sonlanmalı. Arıza kodları ve bakım mesajlarına yanıt verme bilgisi (uygulanabilir arıza izolasyon veya düzeltici prosedüre referanslarla birlikte arıza kodu/bakım mesajı listeleri dahil) dahil edilmeli. Arıza izolasyon sırasında yapılması gereken servis/onarım prosedürleri, basitse (sigorta değişimi, rezervuar doldurma gibi) doğrudan ilgili adımda verilebilir, değilse ilgili DM'lere veya standart pratiklere referans verilmeli.
- **Disconnect/remove/disassemble (IC 5YY):** Bir ögeyi ayırma, çıkarma ve sökme talimatları. Bireysel bir eylem gerçekleştirilmeden önce belgelenmesi gereken tüm ölçüm/değerler ilgili adımda açıklanmalı. Hizalama veya eşleştirilmiş set gibi özel dikkat gerektiren özellikler için talimatlar dahil edilmeli. Minimum rahatsızlıkla erişim sağlamak, diğer servis edilebilir ögeleri ayırıp/çıkarmak ve ardından verilen ögeyi çıkarmak için gereken iş akışı sağlanmalı. **Gereksiz eylem yapılmamalı** (kalıcı bağlantıların açılması, bağlantıların lehim sökülmesi gibi — ögenin bakımı için gerekmedikçe).
- **Repair instructions (IC 6YY):** Aşınmış/hasarlı bir parçayı servis edilebilir duruma geri getirmek için gereken detaylı onarım süreçleri ve veriler. **Onarım tanımı:** Sonuçtaki parçanın konfigürasyonunu orijinal onaylı konfigürasyondan farklı hale getiren aktiviteler "onarım"dır (bir çatlağı kaynaklama, aşınmış bir milin büyütülmesi/işlenmesi gibi) — bu onarımlar hâlâ onaylı mühendislik verisiyle desteklenmelidir. **Aşınmış bir yatak, burç veya mil'in orijinal konfigürasyon parçası veya onaylı alternatif parçayla değiştirilmesi "onarım" değildir** — bunlar üniteyi orijinal onaylı konfigürasyona geri döndüren bakım aktiviteleridir, rutin montaj/söküm prosedürleriyle gerçekleştirilir. Hasarın kapsamı onarılabilir hasar veya onarılamaz hasar (IC 661 "İzin verilen hasar" kullanılarak ekipmanın değiştirilmesini gerektiren) olarak kategorize edilebilir. Onarım tipleri: Geçici onarım prosedürü (IC 662), Standart onarım prosedürü (IC 663), Özel onarım prosedürü (IC 664). **Geçici onarımlar veya ömür kısıtlaması olan onarımlar mümkün olduğunda kaçınılmalı**. Erişim sağlama/ekipmanı onarım için hazır hale getirme adımları DM'nin `<preliminaryRqmts>` elementinde yer almalı. Her onarım prosedürü, prosedürü desteklemek için gereken illüstrasyonları içermeli. Onarım prosedürleri, onarılan parçanın veya montajın değiştirilebilirliğini değiştirmemeli (sleeve veya piston gibi bir onarım-boyutu parçayla onarıldığında). Her onarım kimliklendirilmeli, bu kimlik değiştirilmemeli/tekrar kullanılmamalı.
- **Assemble/install/connect (IC 7YY):** Ekipman/ögenin montaj/kurulum/bağlantısı için talimatlar. Montaj fitleri ve boşlukları, ayarlar, tork değerleri ilgili adımlarda verilmeli. Kurulduğunda kilitlenmesi gereken her parçayı sabitlemek için gereken adım adım talimatlar sağlanmalı. Kalibrasyon veya nihai montajdan sonra yapılamayan/montaj sırasında yapılması daha kolay olan testler için ilgili adımda referans verilmeli.
- **Storage procedures:** Uygunsa depolama talimatları dahil edilmeli. Ömrü sınırlı parçalar için depolama süre limitleri ve ekipmanı depoda servis edilebilir tutma prosedürleri.

**Parts information:** Değiştirilebilir bileşen/montaj/alt-montaj/parçası olan her ekipman/bileşen için **en az bir IPD DM'i sağlanmalı**.

---

## 5.2.1.10 — Weapon Loading Information — Silah Yükleme Bilgisi

*Genellikle kullanılan şema türü: **proced***

**Amaç:** Yetkin personelin mühimmatı yükleyip boşaltabilmesi ve kontrol/ateşleme/salıverme için kullanılan silah sistemlerini kontrol edebilmesi.

**Basic information:** Ürün açıklaması (ilgili ürün sistemlerinin nasıl yapıldığı — ürün kontrolleri, mühimmat salıverme/kontrol sistemleri, yükleme ekibini doğrudan ilgilendiren diğer ekipman: pilonlar, hafif/ağır görevli fırlatıcı üniteleri, adaptörler); mühimmat açıklaması (mühimmatın nasıl yapıldığı ve fonksiyonu, tüm mühimmat tipleri ve bileşenleri için — her fünyeli mühimmat için fünye uyumluluğu gösterilmeli, ön-fünyeleme ve boşaltma sırasında fünye tutma için yetkili olanlar özellikle belirtilmeli); destek ekipmanı açıklaması.

**Supplementary information:** Genel güvenlik gereksinimleri; ürün kontrol/göstergeleri (yükleme ekibinin kullandığı); ürün hazırlığı (fonksiyonel/no-volt testler ve mühimmat yüklemesi için temel ürünü hazırlamak için gereken tüm adımlar — tüm bireysel mühimmat yükleme prosedürleri için standartlaştırılmış bir adım seti kullanılmalı); acil durum prosedürleri (yangın durumunda alınacak eylemler — "Emergency procedures" ifadesi başlıkta yer almalı); kurulu aksesuar hazırlığı; fonksiyonel testler; no-volt testler; standart prosedürler.

**Loading procedures:** Genel prosedürler (her yetkili mühimmat/mühimmat grubu/kombinasyonu için genel bir DM hazırlanmalı, tam yükleme prosedürünü oluşturan tüm DM'leri doğru sırayla listeler); detaylı prosedürler: mühimmat hazırlığı (her mühimmatı muayene/hazırlamak, yetkili fünzeleri kurmak — adımlar **tekil** olmalı), kartuş kurulumu (adımlar **çoğul** olmalı), yükleme (adımlar tekil), fünyeleme, yükleme sonrası (adımlar çoğul), yükleme sonrası kartuş kurulumu, yükleme sonrası muayene, gecikmeli operasyon veya alarm, fırlatmadan hemen önce.

**Offloading procedures:** Güvenli hale getirme, boşaltma öncesi, fünye çıkarma, kartuş çıkarma, boşaltma, boşaltma sonrası.

**Loading/offloading procedure checklists:** Orijinal yükleme/boşaltma prosedürlerinden türetilen özel güvenlik gereksinimlerini içermeli. Sadece belirli bir mühimmat konfigürasyonuna/aksesuarına uygulanan her prosedürel adım kimliklendirme için önek almalı. Tork değerleri, ön ayarlanmış boyutlar, orifis tanımlayıcıları gibi orijinal prosedürdeki veriler checklist'e yerleştirilmeli. Eğitimli bir teknisyenin yetenekleri dahilinde tüm Ürün üzerinde rutin eylem sayılan adımlar atlanabilir. Gereksiz eylem/kontroller çıkarılmalı. Maksimum güvenlik için, her fünye kombinasyonu için ayrı prosedürler uygun alt başlıklar altında dahil edilmeli.

**Integrated combat turnaround procedures:** Muharebe veya muharebe eğitimi turnaround koşullarında servis, bakım ve mühimmat yükleme/boşaltma için kullanılır.

---

## 5.2.1.11 — Cargo Loading Information — Kargo Yükleme Bilgisi

*Genellikle kullanılan şema türü: **proced***

**Amaç:** Yetkin personelin kargo taşımak için donatılmış ürünün yük planlamasını ve yükleme/boşaltmasını yapabilmesi.

**Cargo:** Kargo planlama, yükleme ve boşaltmanın genel açıklaması, sonraki bilgiye giriş.

**The Product general:** Genel; boyutlar ve alanlar (bölme boyutları/düzeni, kapı boyutları); bağlama noktalarının konum ve mukavemeti; güvenlik önlemleri.

**Load planning:** Genel; bölme yüklemesi (genel açıklama, maksimum izin verilen zemin yükleri, maksimum izin verilen entegre yükleme, maksimum paket boyutları); kargo yükleme/boşaltma için gereken ekipman (genel açıklama, bağlama cihazı türleri, konteyner/palet türleri vb., taşıma sistemleri - mandal/ayırıcı/kılavuzlar, bağlama cihazı türleri); ağırlık merkezi limitleri (operasyon, devrilme limitleri, ağırlık merkezi konumunun belirlenmesi, konteyner/palet yük ağırlık merkezinin izin verilen aralığı); kütle-denge limitleri; istifleme ve sabitleme yöntemleri (genel, dökme yükleme, gerekli sabitleme, etkili sabitleme, gerekli bağlama cihazı sayısı, konteyner/palet yüklemesi: taşıma sistemleri/palet yükleme sistemleri ekipmanının çalıştırılması, maksimum palet yükleri, palet yükünün sabitlenmesi).

**Loading/Offloading:** Ürünün yükleme/boşaltma için hazırlanması; kapı çalıştırma talimatları; bağlama cihazlarının kurulumu; taşıma cihazlarının kurulumu; taşıma sistemi çalıştırma talimatları; konteyner/palet vb. yükleme ve sabitleme; yükleme rampası çalıştırma talimatları; yükleme tekniği örnekleri.

---

## 5.2.1.12 — Stores Loading Information — Depo (Mühimmat-dışı) Yükleme Bilgisi

*Genellikle kullanılan şema türü: **proced***

**Amaç:** Ürüne özgü olmayan personelin, ürünün konfigürasyonunun parçası olan mühimmat-dışı depoları (dış yakıt tankları, keşif paketleri gibi) güvenle yükleyip boşaltabilmesi.

İçerik yapısı, Weapon loading (5.2.1.10) ile büyük ölçüde paraleldir: basic information (ürün açıklaması, depo açıklaması, destek ekipmanı açıklaması), supplementary information (genel güvenlik gereksinimleri, ürün kontrol/göstergeleri, ürün hazırlığı, acil durum prosedürleri, kurulu aksesuar hazırlığı, fonksiyonel testler, no-volt testler, standart prosedürler), loading procedures (genel: depo hazırlığı — adımlar tekil, kartuş kurulumu — adımlar çoğul, yükleme, yükleme sonrası, yükleme sonrası muayene, fırlatmadan hemen önce), offloading procedures (boşaltma öncesi, kartuş çıkarma, boşaltma, boşaltma sonrası), loading/offloading procedure checklists.

---

## 5.2.1.13 — Role Change Information — Rol Değişikliği Bilgisi

*Genellikle kullanılan şema türü: **proced***

**Amaç:** Yetkin bakım personelinin ürünün rolünü değiştirebilmesi.

**Change of role - General:** Her rolün tanımı. Gerekli konfigürasyonu oluşturmak için gerekli tüm bileşen ve ekipman listelenmeli. Rol değişikliğini gerçekleştirmek için gerekli özel destek ekipmanı ve/veya özel malzemeler de dahil edilmeli.

**Role change lists:** Ürünün farklı rollerini detaylandırır ve istenen konfigürasyonu elde etmek için yapılması gereken prosedürleri **mantıksal sırada** listeler. Her liste, ilgili rol değişikliği prosedürlerine referansları içermeli (örn. yolcu-kargo, saldırı-eğitim uçağı). Her rol illüstre edilmeli. Çok sayıda role sahip ürünler için (çok-görevli askeri araç gibi) iş paketleri matris formatında listelenmeli.

**Role change procedures:** Herhangi bir rolden herhangi bir role tüm değişiklikleri kapsayan bireysel prosedürler. Hazırlık, çıkarma, kurulum, ayarlama ve test örnek prosedürlerdir. Ürün bakım, silah yükleme veya depo yükleme bilgi setlerindeki DM'lere veya verilere mümkün olduğunda çapraz referans yapılmalı. Yeniden kullanılacak, atılacak veya depolanacak ögelere ait veri de dahil edilmeli.

**First installation procedures:** Ünitelerin ilk kurulumu için hazırlanması gereken tüm prosedürler (teslim edilen ögelerden montajların monte edilmesi ve ardından komple ünite olarak çıkarılıp/kurulması gerektiği durumlarda).

---

## 5.2.1.14 — Battle Damage Assessment and Repair (BDAR) — Muharebe Hasarı Değerlendirme ve Onarım

*Genellikle kullanılan şema türü: **proced***

**Amaç:** Yetkin bakım personelinin ürünü değerlendirip onarabilmesi. Şunları yapmasını sağlayacak veri: hasarlı bölge/ögeleri işaretlemek, hasarlı alana erişmek, hasarı değerlendirmek, yapılacak eyleme karar vermek (onarmak, izole etmek, hasarlı halde bırakmak — operasyon/görev üzerindeki etkisini dikkate alarak), ögeleri onarmak veya izole etmek, gerektiğinde eylemlerin etkinliğinden emin olmak (fonksiyon testleri), BDR kitinin içeriğinden haberdar olmak.

**Genel gereksinim:** BDAR bilgisi savaş alanı/muharebe ortamı için tasarlanmıştır. Bu nedenle **kendi başına yeterli (self-standing)** olmalıdır. Genel bir BDR yayını dışında başka yayınlara referans **olmamalı**, özel alet veya özel destek ekipmanı gereksinimi **olmamalı**. BDR bir BDR kiti tarafından desteklenmeli.

**Introduction:** Ürün çerçevesi ve motor için kullanılan zon kavramı; BDAR yayınında bilgi/veri nasıl bulunur; BDAR yayını nasıl kullanılır. İllüstrasyon/diyagram kullanımı tercih edilir. Giriş 3 ayrı DM'de hazırlanmalı: (1) ürün çerçevesi ve motor için ortak bilgi, (2) ürün çerçevesine özel bilgi, (3) motora özel bilgi.

**Damage repair symbol marking:** Hasarlı bölge/alan/bileşen/parçaları net işaretlemek için kullanılması gereken kurallar ve semboller — yapıların işaretlenmesi (semboller, kodlar, renkler — hasar kategorisi ve hasar değerlendirmesini takiben yapılacak eylem türü) ve sistemlerin işaretlenmesi (etiketler/çıkartmalar ve renkleri, semboller ve kodlar) olarak ikiye ayrılır.

**Identification of damaged hardware:** Hasarlı donanımın (yapı, sistem, demet) kesin kimliklendirilmesi ve hasar değerlendirme verisi sağlayan bir DM'e referans. Şunları kimliklendirme verisi/bilgisi sağlamalı: ürün çerçevesi zonları, ürün çerçevesi sistemleri, motor montajları ve/veya modülleri, ürün çerçevesi ve motor bileşenleri, ürün çerçevesi ve motor kablo demetleri.
- Bileşen tanımlama tablosu şunları içermeli: isim, hasar değerlendirme verisini sağlayan DM kodu, bileşene güç beslemesi olup olmadığı, ilişkili olduğu sistem.
- Kablo demeti kimliklendirmesi için: demeti tanıma prensibi ve standart kimliklendirme prosedürleri (hasarlı alandan tespit edilen hasarın kimliklendirilmesi yöntemi — rota, ayrımlar, ulaşılan bileşenler); parçayı bağlı olduğu bileşene ek zarar vermeyi önleme; süreklilik ölçümü yoluyla telleri kimliklendirme.

**Damage assessment:** Hasarlı ve kimliklendirilmiş bir öge (zon, bileşen vb.) için gerekli veri/bilgi: ögenin ürün çerçevesi/motor operasyonu üzerindeki etkisini belirlemek; ögenin onarımsız hasarlı bırakılıp bırakılamayacağı; onarılıp/izole edilip edilemeyeceği; uygulanacak ilgili eylem; eylemlerin ürün çerçevesi/motor kullanımı üzerindeki sonuçları (görev, kısıtlama vb.).
- **Yapısal hasar kategorileri:** Kategori 1 (kırmızı ile vurgulanmalı), Kategori 2 (sarı ile vurgulanmalı), Kategori 3 (vurgulanmaz, normalde sadece referans için gösterilir).
- **Yapısal hasar limitleri:** penetrasyon için maksimum izin verilen delik çapı ve minimum hasarsız kenar mesafesi cinsinden; fragmantasyon için maksimum izin verilen hasar alanı çapı, herhangi bir deliğin maksimum çapı, hasarlı alandaki maksimum delik sayısı ve minimum hasarsız kenar mesafesi cinsinden; yangın ve aşırı ısınma için kritik sıcaklık göstergesi, iletkenlik, nihai gerilme mukavemeti ve sertlik cinsinden.
- **Bileşen fonksiyon yeteneği:** üç kategoride verilmeli (Imperative - yok olması/izolasyonu ürün çerçevesi/motorun kullanımını yasaklar; Optional - yok olması/izolasyonu operasyonu etkilemez ama operatöre gösterilen bilgiyi etkileyebilir örn. yağ basınç göstergesi; Performance - yok olması/izolasyonu performansta kısıtlamalara neden olur).
- **Yapıların hasar değerlendirmesi tablosu içermeli:** isim (M), hasar kategorisi (M), hasar limitleri (M), kullanımda ulaşılan sıcaklık seviyesi (O), orijinal malzeme ve BDR kitinde bulunan onarımda kullanılabilecek alternatif/ikame malzemesi (O), onarım kabiliyeti ve/veya izin verilen hasarların verildiği DM kodu (yapı onarılamıyorsa ve/veya hiç hasar tolere edilmiyorsa "onarılamaz"/"hasar tolere edilmez" kaydedilmeli).
- **Bileşen ve parçaların hasar değerlendirmesi tablosu içermeli:** açıklama (parça numarası, fiziksel ve maksimum operasyonel özellikler), isim, onarım kabiliyeti (bileşeni onarma prosedürünün DM kodu; onarılamıyorsa "Onarılamaz"), fonksiyon kategorisi, izolasyon (bileşeni izole etme prosedürünün DM kodu; uygulanamıyorsa "Uygulanamaz"), özel operasyon talimatı (onarım ve/veya izolasyon nedeniyle özel talimat; sonraki kısıtlama yoksa "Kısıtlama yok").

**Utilization degradation:** Onarılmamış hasarla veya hasarlı donanım üzerinde yapılan eylemi takiben ürün çerçevesi veya motorun kullanımı (görev, operasyonel kısıtlamalar) üzerindeki sonuçları verir.

**Repair and isolation procedures:** BDR kitini oluşturan donanımı kullanır. Standart prosedürler (ürün çerçevesine/motora özgü olmayan yöntemler — boru/tel onarımı, normal izolasyon gibi); yapıların onarımı (tolerans varsa prosedürde verilmeli; illüstrasyon mümkün olduğunca kullanılmalı, metin sadece illüstrasyonların anlaşılmasını tamamlamak için kullanılır); bileşen/parçaların onarımı (adım adım açıklanmalı); sistem/bileşenlerin izolasyonu (adım adım prosedür).

**Function test after battle damage repair:** Onarılan sistemlerin görev gereksinimlerini yerine getirebileceğinden emin olmak için verilmeli. **Sadece minimum zaman alacak şekilde** tasarlanmalı. Mevcutsa, üzerindeki test tesislerinden tam olarak yararlanılmalı. İzolasyon veya onarım, ürün çerçevesi/motorun operasyon talimatlarında kısıtlamalar içeriyorsa, mürettebatı/operatörü kısıtlamalar konusunda bilgilendirmek için prosedürde belirtilmeli.

**Battle damage repair kit:** BDR kitindeki donanımın listesi — destek ekipmanı/aletler, malzemeler, sarf malzemeleri, tüketilebilirler olarak alt bölünmüş.

---

## 5.2.1.15 — Illustrated Tool and Support Equipment (ITE) — Resimli Alet/Destek Ekipmanı

*Genellikle kullanılan şema türü: **descript***

**Amaç:** Destek ekipmanı (SE) ve özel aletlerin kimliklendirilmesi ve kullanımı için bilgi.

**Alphanumeric index:** Tüm ekipmanı listeler. Sütunlar: Part No. (IPD'de verildiği gibi parça numarası, A-Z sonra 0-9 sırasıyla), CAGE code (tedarikçi/tasarım otoritesi kimliği), Name (IPD'de verilen isim — tanımlayıcı isim/anahtar kelime her zaman önce, gerekli sıfatlar sonra gelir, örn. "Tool, spring compression"), Used in procedure/data module code.

**Equipment information — her ekipman için ayrı DM:** Title (tanımlayıcı isim/anahtar kelime önce), Application (kullanıcının ne için kullanıldığını anlaması için kısa açıklama; gerekli ilgili alet/ekipman da verilmeli), Dimensions (büyük ekipman için), Mass (ağır ekipman için), Parts list (kullanıcının onarabileceği/parça değiştirebileceği ekipman için parça kırılımı tablosu verilmeli; kullanıcının onaramayacağı/değiştiremeyeceği ekipman için sadece SE/özel aletin kendisi hakkında veri olmalı), illüstrasyon (ekipmanı ve mümkünse kullanımdaki konumunu gösteren).

---

## 5.2.1.16 — Service Bulletins — Servis Bültenleri

*Genellikle kullanılan şema türü: **sb***

**Amaç:** Kullanıcılara şu türde önerileri bildirmek için tek kaynak sağlamak: ürün veya parçasında modifikasyonlar (yerleşik yazılım dahil, performansı etkileyen/güvenilirliği artıran/operasyon güvenliğini artıran/ekonomi sağlayan/bakım-operasyonu kolaylaştıran); bir parçanın diğerinin yerini alması (tam olarak değiştirilebilir değilse veya değişiklik yeterince acil/kritikse); güvenli operasyon durumunu belirlemek için özel muayene/kontrol; tamamen değiştirilebilir olsa bile bir parçanın diğeriyle değiştirilmesi/alternatif parça eklenmesi; yerleşik yazılım programının değiştirilmesi (bileşen fonksiyonunu ve programlanmış hafıza cihazının parça numarasını değiştiren); bir modelden diğerine dönüşüm; parça değiştirilebilirliğini etkileyen değişiklikler; mevcut ömür limitlerinin azaltılması veya bileşenler için ilk kez ömür limiti belirlenmesi; ürünün operasyonunu etkileyebilecek özelliklerini etkileyen değişiklikler; geçici olarak kurulu bir parçanın değerlendirilmesi.

**Kural:** Bir SB yayınlandıktan sonra **iptal edilemez**. Orijinal amaç geçersiz hale gelirse, zaten modifiye edilmiş üniteleri orijinal veya tercih edilen konfigürasyona geri döndürmek amacıyla orijinal SB'ye bir revizyon veya yeni bir SB yayınlanmalı.

**SB neyi kapsamaz:** Rutin önerilen muayeneler/kontroller, standart onarımlar veya bakım pratiklerine/atölye prosedürlerine revizyonlar için kullanılmamalı.

**Core SB ana başlıklar:** Management information, Revision information, Summary, Planning information, Material information, Accomplishment instructions, Additional information.

- **Summary:** SB'ye genel bakış vermeli ama SB'nin işini yapmak için **yeterli bilgi içermemeli**. Şunları içerebilir (bunlarla sınırlı değil): SB'nin sebebi/arka planı, eylem/açıklama/işin niteliği (ürün üzerinde, bileşenler üzerinde), etkilenen uçuşa elverişlilik ögelerinin genel değerlendirmesi ve faydalar, ticari öge/malzeme bilgisinin genel değerlendirmesi, uyumluluk, endüstri destek bilgisi, SB'nin uygulanabilirliği, eş zamanlı gereksinimler, SB'yi tamamlamak için iş yükü ve geçen sürenin genel tahmini, işin nerede yapılması gerektiğini gösteren genel illüstrasyon.
- **Planning information — Reason:** operatörün uygulanabilirliğini/operasyonu üzerindeki etkisini belirlemesine yardımcı olacak yeterli gerçek sağlamalı, nicel terimlerle hazırlanmalı: Objective (yeni parçanın acil etkisi), Problem and effect (mevcut parça ile sorun, ne olmuş ve/veya sorun çözülmezse ne olabilir), Solution (yeni parça veya prosedürün sorunu nasıl hafifleteceği).
- **Description:** SB'nin ne yaptığını özetleyen kısa ama eksiksiz bir ifade sağlamalı.
- **Manpower:** SB'nin amacını gerçekleştirmek için gereken adam-saati tahmini sağlamalı.
- **Accomplishment instructions:** SB'nin amacını gerçekleştirmek için adım adım prosedürleri içerir. Karmaşık prosedürler için görev seti (task set) ve prosedürler hiyerarşisine bölünebilir. Core SB'deki talimatlar, çoğunlukla DM'lere referans veren, hiyerarşik olarak düzenlenmiş yüksek seviyeli bir adım seti olmalı — adımlar detaylı talimat/prosedürlerin kendisini içermemeli. Ancak illüstrasyon gerekmiyorsa ve parça tüketilmiyorsa, referans verilen prosedürel DM'ler kullanmadan basit bir prosedür doğrudan core SB'ye dahil edilebilir.

---

## 5.2.1.17 — Material Data — Malzeme Verisi

*Genellikle kullanılan şema türü: **descript / process (proje kararına bağlı)***

**Amaç:** Yetkin personelin malzeme/ürünleri sipariş edip, depolayıp kullanabilmesi.

**Material data sheet — 11 giriş gerektirir:**
1. **Material identification name** — malzemeyi/ürünü en iyi tanımlayan yaygın isim (genelde marka adı veya jenerik isim)
2. **Identifier Number (IN)** — proje tarafından atanır, prosedürlerde referans için kullanılır; bir kez atandıktan sonra başka bir ürün/malzeme için kullanılamaz; ürün/malzeme temin edilemez hale gelirse IN iptal edilmeli ve bir daha kullanılmamalı
3. **Description** — kimliklendirmeye yardımcı olacak diğer bilgi; fonksiyonu (temizlik ajanı, yağlayıcı kaplama gibi) ve formu/durumu (macun, sıvı, gaz) belirtilmeli
4. **Trade name** — üreticinin verdiği yaygın isim
5. **Supplier trade name code** — her veri sayfasında en az bir tedarikçi listelenmeli (tercihen birkaç)
6. **Specification references** — malzemeyi/ürünü tanımlayan bilinen uluslararası/ulusal spesifikasyonlar (NATO stok numarası, ABD/UK spesifikasyonu gibi)
7. **Storage information** — sıcaklık limitleri, raf ömrü kısıtlaması gibi depolama koşulları; normal depolama kabul edilebilirse "Standard" yazılmalı
8. **Packaging information** — paket boyutu, şekli vb.
9. **Transportation information** — renk kodları (varsa) ve nakliye ile ilgili kısıtlama/sorunlara ilişkin yararlı bilgi; risk sınıflandırmaları ("harmful", "inflammable", "combustible", "poisonous") ve parlama noktaları dahil olmalı
10. **Specification requirements/General characteristics** — tedariki ve/veya kimliklendirmeyi desteklemek için uygun veri
11. **Disposal information** — tehlikeli atık/çevresel kirletici imhası için gereken prosedürlerin kimliklendirilmesi

**Not:** Bir giriş karşılanmıyorsa "None" olarak belirtilmeli.

---

## 5.2.1.18 — Common Information and Data (CID) — Ortak Bilgi ve Veri

*Genellikle kullanılan şema türü: **comrep / descript (proje kararına bağlı)***

**Kapsam:** Kısaltma listesi (LOA), terim listesi (LOT), sembol listesi (LOS), uygulanabilir spesifikasyon ve dokümantasyon listesi (LOASD). Her bilgi seti için hazırlanan giriş DM'lerinden derlenir.
- **List of abbreviations:** Alfabetik sırayla kısaltma + tam yazılışı içeren gayri resmi tablo.
- **List of terms:** Alfabetik sırayla terim + anlamının açıklaması.
- **List of symbols:** Sembol + anlamının açıklaması.

---

## 5.2.1.19 — Training — Eğitim

*Genellikle kullanılan şema türü: **learning***

**Kapsam:** Course, Module (opsiyonel), Lesson, Topic, Test öğeleri. Personel ürün üzerinde uygun seviyede eğitilebilmeli.

**Tanımlar:** Course (ilgili derslerin serisi), Lesson (bir Terminal Learning Objective — TLO içeren, birden fazla topic'ten oluşan eğitim aggregate'i), Topic (bir Enabling Learning Objective — ELO ile ilişkili en küçük eğitim birimi), Test (bilgi kontrolü — senaryo, pratik vb. olabilir), TLO (dersi başarıyla tamamlamak için gereken bilgi/beceri), ELO (bir topic'i tamamlamak için gereken beceriler).

**Eğitim içerik DM'leri şunları belirtir:** giriş, ders/topic amacı, topic'in içeriği (bakım DM'lerinde bulunmayan kısım) ve bakım DM'lerine referanslar. **Zaten yazılmış veya yazılması planlanan prosedürler bakım DM'i olarak sunulmalı, eğitim DM'lerinden bunlara link verilmeli** — eğitim DM'leri içine kopyalanmamalı.

---

## 5.2.1.20 — List of Applicable Publications (LOAP) — Uygulanabilir Yayın Listesi

*Genellikle kullanılan şema türü: **descript / pm (proje kararına bağlı)***

**Amaç:** Kullanıcıların ürünü planlamak, bakımını yapmak, çalıştırmak ve desteklemek için ihtiyaç duydukları yayınları seçebilmelerini sağlamak.

**Her giriş şunları içermeli:** publication code (yayın modülü kodu veya diğer ilgili yayın/doküman kodu/numarası); title (manufacturer parça/referans numarası eklenebilir); issue identifier (issue numarası ve/veya issue tarihi); security classification (yayının en yüksek sınıflandırma seviyesi — CD-ROM gibi bir medyada dağıtılıyorsa medyadaki en yüksek sınıflı bilgiye karşılık gelmeli); publisher (sorumlu şirket adı veya CAGE kodu); media information (yayının hangi medya türünde mevcut olduğu ve kimliği).
**Opsiyonel olarak da:** language (yayının hangi dilde yazıldığı).

---

## 5.2.1.21 — Maintenance Checklists and Inspections — Bakım Checklist ve Muayeneleri

*Genellikle kullanılan şema türü: **schedul, proced***

**Amaç:** Yetkin personelin ürünün gerekli kontrol ve servislerini yapabilmesi.

**PMCS (Preventive Maintenance Checks and Services) girdileri şunları içermeli:**
1. **Item number** — PMCS prosedürlerine atanır; minimum zaman/hareket gerektirecek ve aynı anda kontrol yapan personel arası müdahaleyi en aza indirecek mantıksal sırada düzenlenir.
2. **Intervals** — kontrolün yapılması gereken belirlenen aralık ("Before", "During", "After", "Weekly" vb.). En sık/ilk yapılan prosedürler önce sıralanır. **Çekirdek PMCS aralıkları:** Before, During, After, Daily, Weekly, Monthly, Quarterly, Semiannually, Annually, Periodic, Intermediate (sadece havacılık), Man-hour/day (sadece havacılık), Phased (sadece havacılık), Other.
3. **Duration or man-hours** — talep edilen tüm reçeteli yağlama servislerini tamamlamak için gereken süre/adam-saati; en yakın 0,1 saate yuvarlanmalı.
4. **Equipment** — kontrol edilecek öge/sistemi tanımlayan bilgi, mümkün olan en az kelimeyle; genelde yaygın isim (tampon, benzin bidonu ve montaj braketi, ön aks gibi) yeterlidir.
5. **PMCS Procedure** — her kontrolün nasıl yapılacağına dair prosedür, gerekli tüm bilgi dahil (yağlama, uygun toleranslar, ayar limitleri, gösterge okumaları). Görevin yapıldığı yer veya süreci tanımlamak için illüstrasyon hazırlanmalı ve prosedürlerle entegre edilmeli. Değiştirme veya onarım önerildiğinde bakım görevi dahil edilmeli veya ilgili bakım DM'ine referans verilmeli. Ekipman için gereken periyodik/zamanlı yağlama prosedürleri PMCS prosedürlerine dahil edilmeli.
6. **Equipment not ready or available** — ekipmanın görevini tam olarak yerine getirmeye hazır olmamasına neden olacak durumun kısa ifadesi ("Ekipman arızalıysa mevcut değildir" gibi).

**Checking unpacked equipment conditions:** Sevkiyatın durum kontrolü için talimatlar (paletler, konteynerler, kutular, işaretlerin okunabilirliği dahil). Her öge konuma göre listelenir. Location (muayene edilen ögenin konumunun kısa açıklaması), Equipment item (muayene gerektiren ögenin en az kelimeyle kimliği), Action (her liste ögesi için muayene eylemi ve gerektiğinde ilişkili DM referansı — her ana bileşen ögesi birden fazla eylem/alt eylem gerektirebilir), Remarks.

**Special inspections:** Sert iniş, ani duruş, aşırı hız gibi özel muayene gerektiren durumlar için — belirli araç alanları altında gruplandırılır. Zone (alan adı ve numarası), Item number, Interval, Equipment item, Inspection procedure.

---

# BÖLÜM B — KARA/DENİZ ÖZEL BİLGİ SETLERİ (Chapter 5.2.3)

---

## 3. KARA/DENİZ ÖZEL BİLGİ SETLERİ (Chapter 5.2.3) {#3-karadeniz-ozel-bilgi-setleri}

## 5.2.3.1 — Crew/Operator Descriptive Information (Land/Sea) — Sürücü Tanım Bilgisi

*Genellikle kullanılan şema türü: **descript***

**Amaç:** Operatöre aracı yeterli düzeyde anlaması için gerekli bilgiyi sağlamak.

**Ne yazılmamalı:** Operatörün doğrudan ilgilenmediği gereksiz teori ve fazla mühendislik detayı **hariç tutulmalı**. Başka dokümanlarda (araç yayınları, mevzuatlar, resmi yayınlar) zaten olan açıklamaların tekrarı **önlenmeli**. **Performans bilgisi mutlaka dahil edilmeli.**

**3 açıklama tipi (proje kararı — BRDP-S1-00459):** Fonksiyonel açıklama (fonksiyonların baskın açıklaması), Fiziksel kırılım odaklı açıklama (fiziksel kırılımın her parçasının açıklaması), Bağımsız ekipman açıklaması (proje kapsamı dışındaki ama operatöre bilgi sağlanması gereken bileşen/ekipman, örn. mühimmat).

**General description:** Aracın tamamına genel bakış: illüstrasyon/fotoğraflar, aracın amacı, yapı ve kullanımının genel açıklaması.

**Technical data:** Araç ve bileşenleri hakkında gerekli tüm bilgi (ağırlık ve ölçüler, performans verisi, limitler — tanımlayıcı biçimde: optik bileşen kullanımında farklı mürettebat üyelerinin/operatörlerin görüş alanları gibi).

**Operation areas and devices:** Aracın fonksiyonel veya taktik amaç gruplarına bölündüğü tüm farklı yapı türlerinin açıklaması + bu gruplar için kullanılan tüm cihaz/bileşenlerin özeti.

**Technical description — functional breakdown:** Operatörün birden fazla cihaz/bileşenin birleşimini veya fonksiyonunu kavraması için gereken tüm konular. Amaç: operatörü aracın fonksiyonlarını anlayabilecek konuma getirmek. **Bilgi eğitim amaçlı da kullanılabilir olmalı.**

**Technical description — physical breakdown:** Bileşenlerin (gerekli ve operatör seviyesindeyse) yapısı ve fonksiyonu hakkında açıklama:
- Aracın **her kontrolü** açıklanmalı ve **konumu** belirlenmeli
- Bazı durumlarda (özellikle otomatik sistemlerde) aracın **iç mekanizmasını** açıklamak tavsiye edilir, böylece operatör araçtan ne bekleyebileceğini tam kavrar. Uygunsa, **yazılım ve aracın üzerindeki etkileri** açıklanmalı
- Aracın parçası olan **tüm gösterge ve uyarı cihazları** açıklanmalı
- **Tüm göstergeleri ve kontrolleri illüstre etmek için illüstrasyonlar dahil edilmeli** (Chap 3.9.2'nin izin verdiği tüm illüstrasyon türleri kullanılabilir)
- Gerekirse teknik açıklama DM'leri arasında referanslar kullanılmalı (bağımlılığı göstermek için)
- Bilgi **eğitim amaçlı da kullanılabilir olmalı**

**Technical description — independent equipment:** Model kimlik tanımının kapsamadığı ama aracta kullanılan tüm ekipman (mühimmat gibi) — yapısı ve fonksiyonu operatör seviyesinde açıklanır.

**Content and references:** Farklı açıklama türleri referanslarla birbirine bağlanır — fonksiyonel kırılım odaklı açıklama operatöre genel bakış sağlar, fiziksel kırılım odaklı açıklama bireysel bileşenin detaylarını sağlar. Fiziksel kırılım, aynı kırılımdaki diğer tüm bilgi arasında zımni bir bağlantıya izin verir.

---

## 5.2.3.2 — Crew/Operator Operation Information (Land/Sea) — Sürücü Çalıştırma Bilgisi

*Genellikle kullanılan şema türü: **crew, proced***

**Amaç:** Kara/deniz mürettebatına aracın çalıştırılması hakkında bilgi sağlamak.

**7 grup:** Genel bilgi, Operasyon, Operasyon verisi el kitabı, Özel koşullarda çalıştırma, Acil durum prosedürleri, Taşıma, Ekipman istifleme.

**Operation — physical breakdown:** Aracı çalıştırmaya alma, kullanım sırasında işletme veya tanımlı aracı kapatma için gereken tüm açıklamalar. **Adım adım yapı** olarak oluşturulmalı. Tüm ön koşullar (açıklama olarak veya ilgili DM'lere referans olarak) dahil edilmeli. **Güvenlik koşulları ve talimatları** DM içine dahil edilmeli veya ilgili DM'lere referans olarak eklenmeli.

**Operation — functional breakdown:** Bileşenlerin (fonksiyonel grupların) çalıştırılması için gereken tüm açıklamalar. Adım adım yapı. Tanımlı bileşen (fonksiyonel grup) içindeki tek tek ürünler arasındaki **tüm bağımlılıklar, mürettebatın ihtiyacı ölçüsünde açıklanmalı** veya DM içeriğinde dikkate alınmalı.

**Land/Sea Product operating data:** Aktif hizmet için "Land/sea Product operation" konusunda açıklanmayan tüm veri. Bunlar, eğitim kullanımı için olmayan muharebe durumundaki araç parametreleri bilgisi veya sadece muharebe koşullarında uygulanabilir diğer operasyon talimatları olabilir.

**Özel koşullarda çalıştırma:**
- **Heat conditions:** Aracın yüksek sıcaklık altında tam fonksiyonelliğini gerçekleştirmek için gereken tüm ilgili ek operasyon talimatları.
- **Cold conditions:** Düşük sıcaklık altında aynı şekilde.
- **Dust conditions:** Anormal toz etkisi altındaki alanlarda kullanılan araç için ek talimatlar. Örnek: temizlik periyodunun kısaltılması veya filtre değişimi.
- **Recovery or tow away:** ⭐ Şu detaylı talimatları içermeli: (1) güvenlik talimatları dikkate alınarak aracın kurtarma için hazırlığı ve ön koşulları, (2) güvenlik talimatları dikkate alınarak kurtarmanın gerçekleştirilmesi ve/veya sonrasında aracın çekilmesi. **Sadece araca özgü talimatlar oluşturulmalı. Standart pratikler veya kurtarma/çekme hakkındaki diğer genel bilgiler — başka genel yayınlarda tanımlıysa — dahil edilmemeli. Sadece referanslar dahil edilmeli.**
- **Crossing of stretch water:** Şu detaylı talimatları içermeli: (1) güvenlik talimatları dikkate alınarak su geçişi için hazırlık ve ön koşullar, (2) su geçişinin gerçekleştirilmesi ve sonrasında aracın kapatma prosedürleri. Yine sadece araca özgü talimatlar — genel bilgi/standart pratikler dahil edilmemeli, sadece referans verilmeli.
- **Nuclear, biological, chemical (NBC) conditions:** NBC koşulları altında ön koşulların hazırlığı ve gerçekleştirilmesi + aracın NBC koşulları altında kullanımı için detaylı talimatlar. Yine sadece araca özgü talimatlar — genel bilgi dahil edilmemeli.
- **In case of fire at the Land/Sea Product:** Araçtaki yangını söndürmek veya bastırmak için tüm talimatlar.
- **Make unserviceable or destruction:** Aracı sistematik olarak imha etmek için tüm talimatlar — şu durumlarda: operatöre imha etme emri verilmişse veya taktik durum imhayı gerektiriyorsa. Talimatlar farklı imha seviyelerinde tanımlanabilir.

**Emergency procedures:** Şunları kapsayan tüm bilgi: gerekirse acil çıkışların kullanımı; azaltılmış çalışma hazırlığı altında arızalı aracın çalıştırılması.

**Transportation:** Aracın taşınmasını hazırlamak ve gerçekleştirmek için tüm prosedürler. **Standart pratikler sadece başka genel yayınlarda tanımlı değilse dahil edilmeli.** Tüm gerekli güvenlik talimatları (açıklama veya referans olarak) dahil edilmeli. Taşıma türleri: tren, gemi, uçak, kara (örn. alçak yükleyici ile), proje tarafından tanımlanan diğer taşıma türleri.

**Equipment stowing:** Şunlar için gereken tüm bilgi: operatöre istifleme konumlarına genel bakış vermek; hangi ekipmanın hangi sayıda tanımlı konumlarda istiflenmesi gerektiği.

---

## 5.2.3.3 — Crew/Operator Sequential Operation Information (Land/Sea) — Sürücü Sıralı/Checklist Çalıştırma

*Genellikle kullanılan şema türü: **schedul***

**Amaç:** Kara/deniz mürettebatına/operatörüne aracın ve alt sistemlerinin **checklist formunda sıralı çalıştırılması** bilgisini sağlamak.

**4 grup:** Genel bilgi, Operasyon (sıralı), Özel koşullarda çalıştırma (sıralı), Taşıma (sıralı).

**Land/Sea Product sequential operation list:** Görevlerin sıralı listesini sağlamalı. Schedules şemasının `inspectionDefinition` dalı kullanılarak oluşturulmalı. `<taskItem>` elementinin `taskSeqNumber` özniteliği görevin sıra numarasını yakalamak için kullanılmalı. Bu liste ayrıca listelenen görevlere linkler sağlamalı.

**Preliminary requirements (sıralı operasyon için):** Aracı çalıştırmaya almak, kullanım sırasında işletmek veya kapatmak için sıralı operasyonu hazırlamak üzere gereken tüm bilgi. Sıralı operasyon açıklaması **checklist formunda, adım adım yapı** olarak oluşturulmalı. Tüm ön koşullar açıklama veya ilgili DM referansı olarak dahil edilmeli. Güvenlik koşulları/talimatları DM içinde veya referans olarak dahil edilmeli.

**Put into operation / Operation during use / Emergency operation / Operation to turn off / Close up operation:** Hepsi aynı formatı takip eder — ilgili faaliyet için gereken tüm bilgi, checklist formunda adım adım yapı olarak.

**Operating data (sıralı):** "Land/sea Product operation" konusunda açıklanmayan aktif hizmet verisi tüm bilgi, checklist formunda adım adım yapı olarak.

**Özel koşullar (sıralı):** Isı/soğuk/toz koşulları, kurtarma ve çekme, su geçişi, NBC koşulları — hepsi aynı içerik mantığını (araca özgü ek talimatlar) checklist/sıralı formda tekrarlar.

**Transportation (sıralı):** Aracın taşınmasını hazırlamak ve gerçekleştirmek için tüm sıralı prosedürler — tren/gemi/uçak/kara/proje tanımlı.

---

## 5.2.3.4 — Crew/Operator Fault Detection, Isolation and Resolution (FDIR) — Sürücü Seviyesi Arıza Tespit/Çözüm

*Genellikle kullanılan şema türü: **fault, proced***

**Amaç:** Kara/deniz mürettebatına/operatörüne arıza tespiti ve ya tespit edilen arızaların çözümü ya da aracın teknik sorunlarını çözmek için ileri faaliyetler bilgisini sağlamak.

**3 grup:** Genel bilgi, Fonksiyonel test (mürettebat/operatör seviyesi), Arıza tespiti ve çözümü.

**Functional tests (manual):** Aracın mürettebat/operatörler tarafından manuel fonksiyonel testinin hazırlanması ve gerçekleştirilmesi için gereken tüm bilgi. Tüm gerekli ön koşullar (açıklama veya ilgili DM referansı olarak) dahil edilmeli. Ayrıca fonksiyonel testlerin adım adım açıklaması ve gerekirse kapatma prosedürleri dahil edilmeli. Güvenlik koşulları/talimatları DM içinde veya referans olarak dahil edilmeli.

**Functional tests with built-in test equipment:** Aracın mürettebat/operatörler tarafından Built In Test Equipment (BITE) kullanılarak otomatik veya yarı-otomatik fonksiyonel testinin hazırlanması ve gerçekleştirilmesi için gereken tüm bilgi — aynı yapı (ön koşullar, adım adım açıklama, kapatma prosedürleri, güvenlik).

**Fault codes, symptoms and fault tracing for detection of faulty Land/Sea Product:** Mürettebat/operatörler tarafından araçtaki arızaların tespitine izin vermek için gereken tüm bilgi. **Sonraki arıza tespit sürecinin temelidir ve arıza tespitine bir giriş noktası tanımlar.** Gerekirse ön koşullar (açıklama veya referans olarak) dahil edilmeli. Ayrıca arıza kodları veya semptomların ele alınmasının adım adım açıklaması ve gerekirse kapatma prosedürleri dahil edilmeli. Güvenlik koşulları/talimatları dahil edilmeli.

**Fault detection and resolution - Description:** Mürettebat/operatörler tarafından araçtaki arızaların tespitine izin vermek için gereken tüm bilgi. Açıklamalar **akış şeması veya benzeri yapılar** biçiminde olabilir, şunları içerir:
- Eylemlerin açıklamaları — **gerekli ön koşullar dahil**
- Sorular — **gerekirse limitlerle birlikte**
- Cevaplar — **muhtemelen mürettebat/operatör için bir seçim listesi olarak**
- Arızayı çözmek için önlemler, ayrıca eğer mürettebat/operatör tarafından netleştirilemiyorsa **üst bakım seviyesine referanslar** (açıklama veya ilgili DM referansı olarak)

Güvenlik koşulları/talimatları DM içinde veya referans olarak dahil edilmeli.

---

## 5.2.3.5 — International, National and Regulatory Scheduled Check (INRSC) — Yasal/Düzenleyici Muayene

*Genellikle kullanılan şema türü: **schedul, proced***

**Amaç:** Onay personeline, aracın checklist formunda uluslararası, ulusal ve düzenleyici zamanlı kontrolü hakkında açıklamalar sağlamak.

**6 grup:** Genel bilgi, Güvenlik talimatları, Ön koşullar ve iş hazırlığı, Sistem ve alt sistem kontrolü, Yasal kurallara göre kontroller, Operasyon testleri.

**General:** Aracın uluslararası, ulusal ve düzenleyici zamanlı kontrolü hakkında genel bilgiyi içermeli. INRSC işi için kullanım genel açıklamalarını kapsamalı.

**Safety instructions:** Aracın INRSC bilgisini gerçekleştirmek için gerekli ortak güvenlik talimatlarını içermeli.

**Preliminary requirements and preparation of work:** Aracın INRSC bilgisini gerçekleştirmek için ortak ön koşulları içermeli.

**Check of systems and subsystems:** Aracın INRSC bilgisi için bilgiyi içermeli. Gerekli özel ön koşulları ve tüm güvenlik talimatlarını içermeli. INRSC şu kontrolleri kapsayabilir: **aracın teknik durumunun tespiti**; **muharebe hazırlığının tespiti ve aracı sürdürmek için gerekli önlemler**.

**Checks according law rules:** Araç için kontrolleri içermeli. Gerekli özel ön koşulları ve tüm güvenlik talimatlarını içermeli. Yasal kurallara göre kontroller şunları kapsayabilir: **araç için yasal kuralların tutulması**; **zorunlu kontroller ve teknik durum talebinin ilgili sertifikasyonu**.

**Operation tests:** Aracın güvenli operasyonu için pratik test ve kontrolleri içermeli. Gerekli özel ön koşulları ve tüm güvenlik talimatlarını içermeli.

---

*Bu doküman, iki kaynak PDF'in (Chapter 5.2.1 ve Chapter 5.2.3) içerdiği içerik-yazım rehberliğinin bire bir aktarımıdır; DMC kodlama kuralları önceki dokümanda (S1000D_Bilgi_Setleri_Analiz_ve_Rehber.md) ayrıca ele alınmıştır.*

---

*Bu Master Rehber, S1000D'nin Ortak (5.2.1) ve Kara/Deniz Özel (5.2.3) Bilgi Setleri bölümlerinin (301 sayfa, tamamı okunmuş) teknik yazım içeriğini birleştirir. DMC/SNS kodlama kuralları ayrı bir aşamada ele alınacaktır.*