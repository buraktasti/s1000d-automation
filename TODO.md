# TODO.md — Sonraki Geliştirme Adımları

Öncelik sırasına göre. Her madde bağımsız test edilebilir olacak şekilde
yazılmıştır. Koda dokunmadan önce `DECISIONS.md` ile çelişki olup olmadığı
kontrol edilmeli.

## T-001 — [KAPATILDI ✅] systemCode üretim tutarlılığı
- Detay: `DECISIONS.md` D-001.

## T-006 — [KAPATILDI ✅] genIpd() içindeki bağımsız systemCode kullanımı
- T-001 ile birlikte aynı oturumda düzeltildi. Detay: `DECISIONS.md` D-001.

## T-007 — [BEKLEMEDE / XSD İLE DOĞRULANACAK ⏳] assyCode (4 karakter) ve disassyCodeVariant (2 karakter) proje uzunluk kuralı
- Kod uygulandı, 13 test PASS. **Resmi S1000D XSD paketiyle henüz çapraz
  doğrulanmadı** — bu nedenle kapatılamaz.
- Detay: `DECISIONS.md` D-005.

## T-011 — [YENİ — BEKLEMEDE / XSD İLE DOĞRULANACAK ⏳] infoCode kritik hatası düzeltmesi
- **Kaynak:** Gerçek FORSDOC importunda kullanıcı tarafından tespit edildi
  (üretilen gerçek ZIP paketinde `infoCode="40"` gibi 2 haneli hatalı
  değer bulundu; doğrusu `infoCode="040"`).
- Detay-sütun import yolunda, tam DMC mevcutken infoCode artık her zaman
  `parseDmcString(dmc)`'den geliyor; Excel'deki ayrı infoCode sütunu
  yalnızca uyuşmazlık uyarısı için okunuyor, XML üretiminde kullanılmıyor.
- 11 test (TEST-INFO-01→07 + TEST-IMF-01) PASS.
- **Neden hâlâ açık:** TEST-INFO-07 yalnızca `^[A-Z0-9]{3}$` regex deseniyle
  kontrol edildi; gerçek `descript.xsd` ile `xmllint --schema`
  çalıştırılmadı (XSD paketi bu oturumda sağlanmadı). Resmi XSD paketi
  sağlandığında, bu düzeltmeyle üretilen 040/420/520/941 örnek XML'leri
  yeniden `xmllint --schema` ile test edilmeli.
- Detay: `DECISIONS.md` D-009.

## T-012 — [YENİ — BAĞIMSIZ TEYİT BEKLİYOR] icnmetadata.xsd dosya adı düzeltmesi
- `buildImfXml()` içindeki `xsi:noNamespaceSchemaLocation` değeri
  `icnMetadata.xsd` → `icnmetadata.xsd` (küçük harf) olarak değiştirildi.
- **Bu değişiklik yalnızca kullanıcı beyanına dayanıyor** — Claude bu
  oturumda resmi S1000D XSD paketine erişemediğinden dosya adının gerçekten
  küçük harfli olduğunu bağımsız olarak doğrulayamadı.
- Resmi paket sağlandığında, ZIP içindeki gerçek dosya adı listelenerek bu
  nokta teyit edilmeli.
- Detay: `DECISIONS.md` D-010.

## T-002 — Eksik şema türlerini önceliklendir ve ekle
- Öncelik: `schedul` (bakım planlama/PMCS), sonra `wrngdata`. (Değişmedi.)

## T-003 — XSD/BREX doğrulama katmanı ekle
- XSD tarafı: baseline tamamlandı. **Ancak bu baseline T-007 ve T-011'i
  kapsamıyor** — ikisi de baseline'dan sonra eklendi/düzeltildi ve henüz
  XSD ile çapraz doğrulanmadı.
- BREX tarafı: **çalışma tamamen durduruldu** (bkz. `DECISIONS.md` D-006).
  S1000D Default BREX araştırması, BREX validator geliştirme ve BREX
  referans değişiklikleri artık geliştirme önceliği değildir. BREX konusu
  "MSB / proje girdisi gelene kadar BEKLEMEDE" olarak tutulmalı. Bu maddeye
  bir sonraki oturumda **kullanıcı açıkça talimat vermeden** dönülmemeli.

## T-004 — ICN gömme yöntemini S1000D 5.0 ile karşılaştır [KAPATILDI ✅ — yalnızca XSD/XML geçerliliği açısından]
- Detay: `DECISIONS.md` (önceki oturum notu, `graphic/@infoEntityIdent` = `xs:ENTITY`).

## T-005 — İçerik gövdelerini gerçek teknik yazım yapısına yaklaştır
- (Değişmedi — bu oturumda ele alınmadı.)

## T-008 — SNS Sistem Kodu (FORSDOC) doğrulaması — BEKLEYEN GİRDİ
- (Değişmedi.)

## T-009 — MSB katmanı — BEKLEYEN İŞ KURALI GİRDİSİ
- (Değişmedi.)

## T-010 — ICN Metodu "İkisi Birden" ve SNS Dili doğrulaması — DOĞRULANAMADI
- (Değişmedi.)

## Uçtan Uca Test Sonucu (bu oturumda tamamlandı, referans amaçlı)
DERMAN proje girdileriyle (M0117/AAA/TH743/tr-TR) 10 uçtan uca test
çalıştırıldı: DMC import, 4 şema türü üretimi, IMF üretimi, VM-ICN ilişki
yapıları (figure/graphic/ENTITY-NDATA), ZIP paketleme (Python `zipfile` ile
bağımsız açma testi dahil), regresyon — hepsi PASS. ICN'in yalnızca
görsel-yükleme/canvas/OCR kısmı headless test ortamında çalıştırılamadı
(KISMİ). Bu test sırasında gerçek FORSDOC importunda D-009'daki infoCode
hatası kullanıcı tarafından bulundu ve bu oturumda düzeltildi.

## Kapsam Dışı / Şimdilik Ertelenen
- ZWSP şüphesi — bkz. `DECISIONS.md` D-002, açık risk değil.
- Ayarların `localStorage` yerine dosya bazlı saklanması — henüz talep yok.
- **BREX çalışmasının tamamı** — kullanıcı tarafından durduruldu, MSB/proje
  girdisi gelene kadar ele alınmayacak (bkz. `DECISIONS.md` D-006).
