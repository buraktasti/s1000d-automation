# S1000D Ortak Bilgi Setleri — Kapsamlı Analiz ve SNS-Bazlı Kodlandırma Rehberi
### Kaynak: Bilgi_Setleri-S1000D_Issue_5_0.pdf (273 sayfa, Chapter 5.1 – 5.2.1.21) — tamamı satır satır okundu

---

## 0. En kritik 3 bulgu (mevcut çalışmamızı doğrudan etkiliyor)

### Bulgu 1 — Arıza bilgisi (411-414, 420-428, 430, 441, 442) BİLEŞEN değil SİSTEM/ALT SİSTEM seviyesinde kodlanır
DMC deseni her zaman `SS-YY-00` (assyCode sabit **00**). Yani Isolated/Detected/Observed/
Correlated fault list'leri ve Fault Isolation Procedure, sistem/alt sistem seviyesinde tek bir VM'dir
ve içinde birden fazla LRU'yu değerlendirir — her bileşen için ayrı 410/420 üretilmez.
**Mevcut "DMC Detay" tablomuzdaki Kategori B (490 bileşen × kendi fault kodu) bu kuralla çelişiyor.**

*Ancak* — Equipment/Component Maintenance bilgi setinde (Ch 5.2.1.9, "CM" — sökülmüş bileşenler
için ayrı el kitabı hazırlanıyorsa) fault isolation (IC 4YY) bileşen seviyesinde de üretilebiliyor,
çünkü orada birim kendi başına bağımsız bir yayının konusu. **Karar noktası:** ZKA'da bileşenler
ana yayının SNS'i içinde mi kalacak (→ fault sistem seviyesinde) yoksa bazı büyük LRU'lar (motor,
şanzıman gibi) kendi CM el kitabına mı çıkacak (→ o zaman bileşen seviyesinde fault olabilir)?

### Bulgu 2 — IC 040 basit sistemlerde bölünmez
"For simple systems, IC 040 must not be subdivided" — 041/042/044 gibi alt kodlara bölme sadece
karmaşık sistemlerde. Motor gibi karmaşık bir alt-alt sistemde 041 (nasıl yapıldığı) + 042
(fonksiyon açıklaması) ayrımı yapılabilir; basit bir bileşende sadece 040 yeterli.

### Bulgu 3 — MSB İş Kuralları dosyanız bu dokümanın "Business Rule Decision Point" (BRDP-S1-xxxxx)
numaralarına karşılık geliyor. Yani elinizdeki 578 satırlık İş Kuralları dosyası, bu 273 sayfalık
dokümanda dağınık halde bulunan onlarca karar noktasının (BRDP-S1-00404, 00405, ... 00451) proje
için verilmiş cevapları. İkisini yan yana okumak, hangi kararın zaten verildiğini netleştirir.

---

## 1. Bölüm bazlı referans tablosu

Her satır: **Bilgi Seti (Chapter)** | **Kapsam/İçerik** | **SNS Seviyesi** | **infoCode(lar)** | **Şema**

### 5.2.1.1 — Crew/Operator information
Operatör talimatları (kullanım, kontrol/gösterge, acil durum). Sistem/alt sistem seviyesinde.
IC 1YY (Operation), crew/descript/proced şemaları.

### 5.2.1.2 — Description and operation ⭐ (Motor sorunuzun cevabı burada)
- **Sistem (SS-00-00):** sistemin ve alt sistemlerinin amacı, işlevsel kapsamı, diğer sistemlerle ilişkisi
- **Alt Sistem (SS-Y0-00):** fonksiyon/çalışma/kontrol açıklaması + alt sistemdeki ana bileşenlerin genel amacı ve konumu
- **Alt-alt Sistem (SS-YY-00):** aynı detay seviyesi, alt-alt sistemdeki ana bileşenler için
- **Bileşen/Assembly (SS-YY-YY):** bileşenin fonksiyonu, çalışması, kontrolü + testleri + performansı etkileyen ayarlar + özel bakım uygulamaları
- Her seviyede: **Component index** (bileşen listesi), **Access/Area identification** (erişim noktaları), **Component location/identification** (fiziksel konum) tabloları/illüstrasyonları önerilir
- IC 040 basit sistemde bölünmez (yukarıdaki Bulgu 2); IC 1YY = Operation (kontrol/gösterge, ön/son operasyon, acil durum)
- Şema diyagramları (051) üç seviyeli: **Block** (basit, eğitim amaçlı) → **Simplified** (elektriksel doğru ama konum bağımsız) → **Detailed** (bakım için tam detay, tüm kablolama)

### 5.2.1.3.1 — Maintenance procedures (Servis/Test/Söküm/Onarım/Montaj)
- **200 (Servicing):** diğer bakım işlemleri sonucu gereken rutin/onarıcı servis prosedürleri
- **300 (Examinations/tests):** karmaşıklık, replace edilen parçaya göre değişir; testler tekrar etmemeli
- **500 (Disconnect/remove/disassemble):** adım adım, erişim→söküm sırası; gerekli malzeme/alet listesi başta; ön koşullar (örn. "erişim panelini aç") belirtilmeli
- **600 (Repair):** aşınmış/hasarlı parçayı servis edilebilir hale getirme; görünümler, ölçüler, kontrol gereksinimleri dahil
- **700 (Assemble/install/connect):** montaj, tork değerleri adım içinde verilmeli, illüstrasyon+callout zorunlu
- Kara/deniz araçları için sistem 00-19 kullanılır (uçaklardan farklı); **BRDP-S1-00404** ile proje uygulanabilir sistemleri belirler (MSB İş Kuralları'nda cevaplanmış olmalı)

### 5.2.1.3.2 — Fault isolation ⭐ (Bulgu 1)
- **410 (Isolated), 412 (Detected), 413 (Observed), 414 (Correlated) fault list:** mesaj kimliği, açıklama, arızayı tespit eden LRU, arızalı LRU, düzeltici eylem — hepsi **sistem seviyesinde** (SS-YY-00)
- **420-428 (Fault isolation procedure):** tam izolasyon prosedürü; adım = eylem+soru+cevap; başka DM'e yönlendirme YOK, her DM kendi içinde tam olmalı
- **430:** destekleyici veri (sadece gerekiyorsa, mümkünse mevcut diyagramlar kullanılır)
- **441 (Fault code index), 442 (Maintenance message index):** türetilmiş (derived) indeksler

### 5.2.1.3.3 — Non-destructive testing (NDT)
İtemin kendi SNS kodunda (bileşen seviyesinde uygulanabilir). Standart başlıklar: Applicability,
Tools/equipment/materials, Item description, Preparation/cleaning, Test equipment adjustment,
Procedure, Indication evaluation, Acceptance/rejection standards, Restoring the Product.
IC 350-359 (dye penetrant, magnetic particle, eddy current, X-ray, ultrasonic, gamma-ray, vb.)

### 5.2.1.3.4 — Corrosion control
Sistem 00-19 (kara/deniz), yapısal büyük gruplar bazında (IC 041 açıklama, 350 korozyona
eğilimli/kritik alan, 258 korozyon giderme, 257 kaplama, 259 sızdırmazlık). ZKA gibi kara aracı
için düşük öncelikli olabilir ama korozyona eğilimli alanlar (şasi, alt takım) için değerlendirilmeli.

### 5.2.1.3.5 — Storage
**Proje geneli, sistem 10-30 sabit SNS kullanılır** (bileşenin kendi SNS'i DEĞİL). Geçici depolama
(≤90 gün) / uzun süreli depolama (>90 gün) / muhafaza / paketleme prosedürleri. IC 800-890.

### 5.2.1.4 — Wiring data
- Açıklayıcı bilgi (elektrik kimliklendirme sistemi, bağlantı üniteleri, tel/demet) → **descript** şeması, IC 040
- Kablo şeması (diyagram) → **descript**, IC 051 (SS-YY-YY seviyesinde, sistemin kendi SNS'i)
- Demet çizimleri (kurulum/rota/düz düzen) → **descript**, IC 052
- Ekipman/panel konumu → IC 055
- Elektriksel standart parça verisi, ekipman listesi, tel listesi, demet listesi → **wrngdata** şeması, IC 031/056/057/058
- Kara/deniz araçlarında bu bilgilerin SNS kodlaması **sistemin kendi kodunu** kullanır (uçaklardaki gibi sabit "91" sistemi yok)

### 5.2.1.5 — IPD
Bu bölüm sadece pointer; detaylı kural başka bir chapter'da (bu dokümanda yok). Proje kararıyla
ayrı yayın ya da başka yayına entegre olabilir.

### 5.2.1.6 — Maintenance planning information
**Sistem SEVİYESİNDE, SNS'ten bağımsız sabit "05" kodu kullanılır:**
- Time limits: `05-10-SS` (SS=ilgili sistem)
- Maintenance/inspection task lists: `05-20-SS`
- Check definitions: `05-SS(40/50/60)` (40=scheduled, 50=unscheduled, 60=acceptance/functional)
- Maintenance allocation: `05-80-00`, IC 916

### 5.2.1.7 — Mass and balance
Proje geneli sabit "08" kodu (`08-00`, `08-10`, `08-20`, `08-30`, `08-40`), IC 000.

### 5.2.1.8 — Recovery information ⭐ (ZKA'nın ANA GÖREVİ — kurtarıcı araç!)
Bu bilgi seti ZKA için özellikle kritik çünkü aracın kendi görevi "recovery". Sabit "07" sistemi:
- Genel/hızlı referans checklist: `07-4Y`, IC 121/125
- İlk inceleme (hasar tespiti): `07-4Y`, IC 311
- Hasar kontrolü/güvenlik: sıvı tahliyesi (`12-10`, IC 220), akü söküm (`24`, IC 500)
- Kütle/ağırlık merkezi: `08-40`, IC 050
- Yük/silah/dış depo sökümü: `94-10`, IC 520
- Yakıt boşaltma: `12-10`, IC 221
- Stabilize etme/kaldırma: `07-10/20/30`, IC 100
- Taşıma/çekme/vinçleme: `07-40/50`, IC 100/061
- Destek ekipmanı: `07-00`, IC 060/061/062

### 5.2.1.9 — Equipment information (CM/SE/TE) ⭐ (bileşen-seviyesi el kitabı rehberi)
Sökülmüş bileşen/destek ekipmanı için AYRI bir el kitabı hazırlanıyorsa (bizim Kategori B mantığımıza
en yakın bölüm) — tam içerik yapısı:
- Front matter/Introduction, Functional&technical description (**IC 040/042/044**), Operation (SE/TE için),
  **Maintenance and servicing** (Procedure/Fault isolation/Process şeması kullanır):
  - Cleaning IC 25Y, Inspection IC 28Y, Visual exam IC 31Y, Operation test IC 320,
    Test prep IC 33Y, Function test IC 34Y, Automatic test/BIT IC 341/342,
    Structure test IC 35Y, Checks IC 36Y
  - **Fault isolation IC 4YY** — "genelde examination/test'lerle karşılanır, ek prosedür gerekirse tamamlayıcı olarak eklenir" (bileşen seviyesinde fault isolation burada MÜMKÜN — Bulgu 1'deki istisna)
  - Disconnect/remove/disassemble IC 5YY, Repair IC 6YY (662 geçici/663 standart/664 özel),
    Assemble/install IC 7YY
- Parts information: en az bir IPD DM zorunlu

### 5.2.1.10-12 — Weapon/Cargo/Stores loading
ZKA'da muhtemelen düşük öncelikli (silah taşımıyorsa) ama kurtarma ekipmanı/palet yükleme
söz konusuysa Cargo loading (14-20/21/22/23) uygulanabilir.

### 5.2.1.13 — Role change
Aracın farklı konfigürasyonlar arası dönüşümü varsa (örn. farklı görev kitleri) `16-00/11/12/13`.

### 5.2.1.14 — Battle damage assessment and repair (BDAR)
Askeri araç için potansiyel olarak önemli. Sabit "00-90" kodu (özel donanıma atanamayan durumlar
için) ya da gövde/motor için kendi sistem kodu. IC 681 (işaretleme), 682 (hasarlı donanım
tanımlama), 683 (hasar değerlendirme), 684 (kullanım kısıtlaması), 685/686 (onarım/izolasyon),
687 (onarım sonrası test), 688 (BDR kiti).

### 5.2.1.15 — Illustrated tool and support equipment (ITE)
Özel/standart alet-ekipman kimliklendirme. Alfasayısal indeks (IC 014) + ekipman bilgisi (IC 066).

### 5.2.1.16 — Service bulletins
Modifikasyon/özel muayene bildirimleri. Core SB (**IC 930**), Task set (**IC 933**), Material info
(**IC 934**). SNS = ilgili bileşenin kendi SNS'i + sıralı SB numarası.

### 5.2.1.17 — Material data
Malzeme/ürün veri sayfaları. Sabit "00-50" kodu. IC 077 (sarf) / 074 (tehlikeli sarf).

### 5.2.1.18 — Common information and data (CID)
Kısaltma listesi (IC 005), terim listesi (IC 006), sembol listesi (IC 007), uygulanabilir
spesifikasyon listesi (IC 00V) — proje genelinde, sabit "00-00" kodu.

### 5.2.1.19 — Training
Eğitim modülleri (course/module/lesson/topic/test) — Learn Code + Learn Event Code ile 21/41
karakterlik genişletilmiş DMC. Item location code = "T" (SCORM olmayan) veya operasyon/bakım
DM'iyle aynı kod (SCORM entegre).

### 5.2.1.20 — List of applicable publications (LOAP)
Tüm yayınların/dokümanların listesi. Front matter şeması (fm05) veya descript şeması ile,
`pmRef`/`dmRef`/`externalPubRef` kullanılarak. Sabit "00-40" kodu, IC 014.

### 5.2.1.21 — Maintenance checklists and inspections
PMCS (Preventive Maintenance Checks and Services) — sabit "05" kodu:
- `05-10-SS`, IC 200 (PMCS)
- `05-20-SS`, IC 870 (paketten çıkarma kontrolü)
- `05-XX-00`, IC 310 (periyodik/özel muayene, XX=40 scheduled/50 unscheduled/60 acceptance)

---

## 2. ZKA metodolojimiz için somut aksiyon önerileri

1. **Fault (Bulgu 1) — EN ÖNCELİKLİ KARAR:** Kategori B'deki 410/420 kodlarını bileşen
   seviyesinden çıkarıp sistem/alt-sistem seviyesine (Kategori A) taşımalı mıyız? Yoksa bazı
   büyük LRU'lar (motor, vinç, şanzıman) için CM tarzı ayrı bilgi seti mi açacağız (o zaman
   bileşen seviyesinde fault kalabilir)? **Bu karar DMC Detay tablosundaki ~1000+ satırı etkiler.**
2. **Recovery information (5.2.1.8)** ZKA'nın görev tanımıyla birebir örtüşüyor — SNS listenizde
   bu bilgi setine karşılık gelen bir dal var mı kontrol edilmeli (07-serisi sistemler).
3. **Maintenance planning (05-serisi) ve PMCS (05-21)** proje SNS'inizde henüz karşılığı
   belirlenmemiş — bunlar bileşen bazlı değil, ayrı "sabit sistem" mantığıyla üretilecek.
4. **BDAR** askeri araç olarak değerlendirilmeli — kapsam dahilinde mi karar verilmeli.
5. **IC 040 bölünme kuralı** (Bulgu 2) — Kategori A/B/C/D paketlerimizde 040/041/042 seçimini
   "bileşen basit mi karmaşık mı" sorusuna göre ayarlamalıyız (şu an hepsi tek tip 040 kullanıyor).

---

## 3. EK: Chapter 5.2.3 — Land/Sea Specific Information Sets (2. doküman, tamamen okundu)

### Yapısal ana bulgu ⭐⭐⭐
Bu bölümün **tamamı sabit "15" (Crew/Operator alanı) ve "19" (INRSC alanı) sistem kodlarını**
kullanır. Bunlar aracın gerçek mekanik/elektrik sistem ağacından (bizim GA1/GB2/... SNS'imiz)
**tamamen bağımsız, ayrı bir SNS bölgesidir**. Yani "Crew/Operator" ve "INRSC" içerikleri
bileşenlerin kendi SNS koduna değil, 15-xx / 19-xx sabit alanına yazılır — tıpkı Maintenance
Planning'in "05", Storage'ın "10-30", Material data'nın "00-50" sabit alanlarını kullanması gibi
(bkz. Bölüm 1). **Projemizde bu sabit alanlar için SNS'te ayrı bir düğüm/dal tanımlanmalı.**

### 5.2.3.1 — Crew/Operator descriptive information
- **General description** (IC 043, `15-00-00`): Ürünün resmi/amacı/genel yapı ve kullanım açıklaması — tek, bütün ürün için.
- **Technical data** (IC 033): ağırlık/ölçüler/performans verisi/limitler (örn. optik cihazlarla görüş alanı).
- **Operation areas and devices** (IC 055, `15-00-00`): aracın fonksiyonel/taktik amaç gruplarına bölünmesi + kullanılan cihaz özeti.
- **Technical description - functional breakdown** (IC 043, `15-05-YY`): birden fazla cihaz/bileşenin birlikte fonksiyonunu anlatan, eğitim amaçlı da kullanılabilir üst seviye açıklama.
- **Technical description - physical breakdown** (IC 044, bileşenin kendi SNS'i): her kontrol + konumu, otomatik sistemlerde iç mekanizma/yazılım etkisi, tüm gösterge/uyarı cihazları — **illüstrasyon zorunlu**.
- **Technical description - independent equipment** (IC 033/043, `15-06-00`): MIC kapsamı dışındaki ama operatörün bilmesi gereken ekipman (örn. mühimmat).
- **Karar noktası: BRDP-S1-00459** — hangi açıklama tiplerinin (functional/physical/external) kullanılacağı proje kararı.

### 5.2.3.2 — Crew/Operator operation information
7 grup: Genel / Operasyon / Operasyon verisi / Özel koşullarda çalıştırma / Acil durum / Taşıma / Ekipman istifleme.
- Operasyon (fiziksel, bileşenin kendi SNS'i) veya (fonksiyonel, `15-3Y-YY` Y=1-8): IC **111/112/121/131/141/151** (kontrol&gösterge, ön-operasyon, normal operasyon, acil operasyon, son-operasyon — hepsi crew-attributed).
- **Özel koşullar** (`15-51` ısı/soğuk/toz varyantları A/B/C, `15-52` **kurtarma/çekme** ⭐, `15-53` su geçişi, `15-54` NBC, `15-55` yangın, `15-59` imha).
- **Taşıma** (`15-39-1Y/2Y/3Y/4Y` tren/gemi/uçak/kara) IC 111-151.
- **Ekipman istifleme** (IC 056, `15-04-YY`).

### 5.2.3.3 — Crew/Operator sequential operation (checklist formu)
Aynı 5.2.3.2 içeriğinin **checklist/sıralı görev listesi** hâli — `schedul.xsd` şemasının
`inspectionDefinition` dalı kullanılır (`taskSeqNumber` özniteliği ile). 4 grup: Genel/Operasyon
(sıralı)/Özel koşullar(sıralı)/Taşıma(sıralı). Kodlama aynı sistem alanlarını (`15-36..39`)
kullanır ama IC'ler 125(kuruluma alma)/135(kullanım sırasında)/145(acil)/155(kapatma) olarak
farklılaşır. **Sequential operation list** (`05-SS-00`, SS=45 zamanlı/55 zamansız/65 kabul) ayrı
bir liste DM'idir, görevlere link verir.

### 5.2.3.4 — Crew/Operator fault detection, isolation and resolution (FDIR) ⭐⭐
**Önceki bulgumuzu netleştiren kritik bölüm.** Burada da IC 410/420 kullanılıyor ama **`15-41-YY`
/ `15-42-YY` sabit crew-alanında** — yani bizim önceki "fault sistem seviyesinde" bulgumuzla
ÇELİŞMİYOR, tamamlıyor: **İKİ AYRI fault bilgi seti var:**
1. **Bakım teknisyeni FI** (Ch 5.2.1.3.2, sistemin kendi SNS'i, `SS-YY-00`) — detaylı, LRU
   değiştirme odaklı, teknisyen için.
2. **Operatör FDIR** (Ch 5.2.3.4, sabit `15-41`/`15-42`) — basit, semptom bazlı, "ne oldu → soru
   → cevap → ya çöz ya üst bakım seviyesine yönlendir" akışı, **operatör/sürücü** için.
- Fonksiyonel test (manuel, IC 321 / BITE otomatik, IC 322) hem fiziksel (bileşenin SNS'i) hem
  fonksiyonel (`15-35-YY`) kırılımda olabilir.
- Fault codes/symptoms (`15-41-YY`, IC 410): arıza tespitinin giriş noktası.
- Fault detection/resolution description (`15-42-YY`, IC 420): akış şeması tarzı — eylem+soru+
  cevap+çözüm(veya üst bakım seviyesine yönlendirme).

### 5.2.3.5 — International, national and regulatory scheduled check (INRSC)
Kara aracına özgü, **düzenleyici/yasal zorunlu muayeneler** (örn. araç muayenesi/trafik
mevzuatı gibi) — sabit **"19"** alanı. 6 grup: Genel/Güvenlik talimatları/Ön koşullar/Sistem-alt
sistem kontrolü/Yasal kurallara göre kontroller/Operasyon testleri. Kod alanları `19-1Y` (genel)
`19-2Y` (güvenlik) `19-3Y` (ön koşul) `19-4Y` (sistem kontrolü, teknik durum+muharebe hazırlığı
tespiti) `19-5Y` (yasal zorunlu kontrol+sertifikasyon) `19-6Y` (operasyon testleri). **ZKA askeri
bir araç olduğu için bu bölümün proje kapsamında olup olmadığı** (muhtemelen düşük öncelik,
ama yasal muayene varsa uygulanabilir) MPG/MSB ile netleştirilmeli.

### Aksiyon: SNS listemize eklenmesi gereken sabit alanlar
Mevcut 1216 satırlık SNS listemizde bulunmayan ama S1000D'nin öngördüğü sabit alanlar:
- **"15" (Crew/Operator)** — kullanıcı el kitabı içeriği için ayrı, sistem-bağımsız bir SNS dalı
- **"19" (INRSC)** — düzenleyici muayeneler için (uygulanabilirse)
- **"05" (Maintenance planning/PMCS)** — zaten Bölüm 1'de not edildi
- **"10-30" (Storage)** — zaten Bölüm 1'de not edildi

Bu 4 alan, bileşen bazlı Kategori A-E metodolojimizin **DIŞINDA**, ürün geneli için bir kez
üretilecek ayrı bir "sabit sistem" katmanı oluşturuyor. SNS listenizde bu dallar yoksa MPG/MSB
ile birlikte eklenmesi gerekip gerekmediği görüşülmeli.
