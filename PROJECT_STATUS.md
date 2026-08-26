# PROJECT_STATUS.md — Mevcut Durum

Son güncelleme: 2026-08-26 (kritik infoCode hatası — gerçek FORSDOC
importunda bulundu — düzeltildi; icnmetadata.xsd dosya adı düzeltmesi
yapıldı; 10 uçtan uca test PASS aldı; BREX çalışması kullanıcı tarafından
durduruldu)

## Genel Durum
Uygulama çalışır durumda. Bu oturumda **iki somut kod düzeltmesi**
uygulandı:
1. **Kritik hata düzeltmesi (D-009):** Detay-sütun importta, Excel'deki
   hatalı biçimlendirilmiş infoCode değeri ("40"), DMC'den doğru türetilen
   infoCode değerinin ("040") önüne geçiyordu — bu, S1000D XSD'sinin
   `infoCode` pattern'i (`[A-Z0-9]{3}`) ile uyumsuz XML üretimine yol
   açıyordu. Bu hata **gerçek FORSDOC importunda kullanıcı tarafından
   tespit edildi.**
2. **Küçük düzeltme (D-010):** `buildImfXml()` içindeki schemaLocation'da
   `icnMetadata.xsd` → `icnmetadata.xsd` (küçük harf) düzeltmesi (kullanıcı
   beyanına dayalı, bu oturumda bağımsız teyit edilmedi).

Ayrıca DERMAN proje girdileriyle 10 uçtan uca test çalıştırıldı (tamamı
PASS/KISMİ, hiç FAIL yok) ve **BREX çalışması kullanıcı tarafından tamamen
durduruldu** — artık aktif geliştirme önceliği değil.

## Tamamlanmış / Doğrulanmış Bileşenler
- [x] Excel/CSV içe aktarma, otomatik ayraç/kodlama tespiti
- [x] Tam DMC string'inden regex ile alan ayrıştırma (`DMC_RE`, `parseDmcString`)
- [x] Detaylı Excel sütunlarından alan türetme (`HEADER_MAP`)
- [x] SNS ağacı görünümü, arama/filtre
- [x] Tekli XML önizleme/indirme, toplu ZIP indirme
- [x] `descript`, `proced`, `fault`, `ipd` şemaları için XML üretim fonksiyonları
- [x] ICN/IMF: görsel yükleme, parça listesi eşleme, hotspot koordinat üretimi, Tesseract OCR
- [x] ICN+IMF+VM birleşik ZIP paketleme
- [x] Proje ayarları `localStorage`'da kalıcı
- [x] 4 örnek DMC XML dosyası ile çıktı yapısı çapraz kontrol edildi
- [~] assyCode (4)/disassyCodeVariant (2) proje uzunluk kuralı — kod uygulandı, XSD çapraz doğrulaması bekliyor (T-007)
- [~] **infoCode her zaman DMC'den türetiliyor (detay-sütun import yolunda)** — kod uygulandı, dahili testlerle doğrulandı, XSD çapraz doğrulaması bekliyor (T-011, D-009)
- [~] IMF schemaLocation küçük harf düzeltmesi — kod uygulandı, bağımsız teyit bekliyor (T-012, D-010)

## Doğrulanmış Eksikler / Yapılmayanlar
- [ ] `schedul`/`wrngdata` şemaları — üretim fonksiyonu yok
- [ ] `crew`, `process` ve diğer S1000D şema türleri — desteklenmiyor
- [ ] BREX kural doğrulama — **çalışma durduruldu** (bkz. aşağıda "BREX Durumu")
- [ ] Gerçek teknik içerik üretimi — placeholder/iskelet
- [ ] T-007 ve T-011'in resmi S1000D XSD paketiyle çapraz doğrulaması — **yapılmadı**

## Doğrulanmamış / İncelenmesi Gereken Noktalar
- ZWSP şüphesi — risk olarak kayıtlı değil (D-002)
- BREX kimliği (A vs G) — **artık aktif araştırma konusu değil**, BREX çalışması durduruldu (D-006)
- MSB'ye ait BREX/Business Rules kaynağı — elimizde yok, BEKLEMEDE (D-006)
- SNS Sistem Kodu (FORSDOC "00") ile dmCode/@systemCode ilişkisi — BEKLEYEN FORSDOC/SNS GİRDİSİ (D-007)
- MSB katmanının uygulamada temsili — BEKLEYEN MSB İŞ KURALI GİRDİSİ (D-008)
- ICN Metodu "İkisi Birden" ve SNS Dili — DOĞRULANAMADI (T-010)
- **T-007 ve T-011 uzunluk/infoCode kurallarının resmi XSD ile çapraz doğrulaması** — yapılmadı, XSD paketi bu oturumda sağlanmadı
- **T-012 icnmetadata.xsd dosya adının gerçekten küçük harfli olduğu** — yalnızca kullanıcı beyanına dayanıyor, Claude tarafından bağımsız teyit edilmedi

## T-001 / T-006 Durumu (KAPATILDI ✅)
Detay: `DECISIONS.md` D-001.

## T-007 Durumu (BEKLEMEDE / XSD İLE DOĞRULANACAK ⏳)
Detay: `DECISIONS.md` D-005.

## T-011 Durumu (YENİ — BEKLEMEDE / XSD İLE DOĞRULANACAK ⏳) — infoCode kritik hatası

**Kaynak:** Bu, önceki oturumlarda hiç tespit edilmemiş, **gerçek FORSDOC
importunda kullanıcı tarafından bulunan bir kritik hatadır.** Üretilen
gerçek bir ZIP paketinde, dosya adı `...-040A-A_...` (doğru) olduğu halde
XML içindeki `dmCode/@infoCode="40"` (2 haneli, S1000D `descript.xsd`
pattern'ine `[A-Z0-9]{3}` göre **geçersiz**) olarak üretilmişti.

**Kök neden:** `importText()`'in detay-sütun import yolunda, Excel'deki
ayrı "InfoCode" sütunu değeri, DMC'den regex ile 3 karakter garantili
türetilen değerin önüne geçiyordu (`if (!infoCode) infoCode = p.infoCode;`
— yalnızca Excel değeri boşsa DMC'ye başvuruluyordu).

**Düzeltme:** Tam DMC mevcutken infoCode artık **her zaman**
`parseDmcString(dmc)`'den geliyor; Excel'in infoCode sütunu yalnızca
uyuşmazlık durumunda kullanıcıyı uyarmak için okunuyor, XML üretiminde asla
kullanılmıyor. `dmc-only` import yolu (bug'ın hiç olmadığı yol)
değiştirilmedi.

**Test sonuçları: 11/11 PASS, 0 FAIL** (TEST-INFO-01→07 + TEST-IMF-01).
Ayrıca önceki T-007 testleri (13/13) ve E2E testleri (10/10 + 1 KISMİ) bu
değişiklikten sonra yeniden çalıştırıldı, hiçbiri bozulmadı (regresyon yok).

**Neden hâlâ açık:** TEST-INFO-07 yalnızca `^[A-Z0-9]{3}$` regex deseniyle
kontrol edildi, gerçek `descript.xsd` ile `xmllint --schema` çalıştırılmadı
(XSD paketi bu oturumda sağlanmadı). Detay: `DECISIONS.md` D-009.

## T-012 Durumu (YENİ — BAĞIMSIZ TEYİT BEKLİYOR) — icnmetadata.xsd düzeltmesi
`buildImfXml()` içinde `icnMetadata.xsd` → `icnmetadata.xsd` düzeltildi.
Bu, yalnızca kullanıcı beyanına dayanıyor; Claude bu oturumda resmi S1000D
XSD paketine erişemediğinden dosya adının gerçekten küçük harfli olduğunu
bağımsız olarak doğrulayamadı. Test: TEST-IMF-01 PASS (yalnızca kodun doğru
string'i ürettiğini doğruladı, gerçek dosya adıyla eşleştiğini değil).
Detay: `DECISIONS.md` D-010.

## Uçtan Uca Test Sonucu (bu oturumda tamamlandı)
DERMAN proje girdileriyle 10 test çalıştırıldı:

| Test | Sonuç |
|---|---|
| E2E-01 DMC import | PASS |
| E2E-02 descript (040) | PASS |
| E2E-03 proced (520) | PASS |
| E2E-04 fault (420) | PASS |
| E2E-05 ipd (941) — systemCode tutarlılığı | PASS |
| E2E-06 ICN | KISMİ (görsel/canvas/OCR headless test edilemedi) |
| E2E-07 IMF | PASS |
| E2E-08 VM-ICN ilişkisi | PASS |
| E2E-09 ZIP (Python zipfile ile bağımsız açma) | PASS |
| E2E-10 Regresyon | PASS |

Bu test sürecinde, kullanıcının gerçek FORSDOC importundan bildirdiği
infoCode hatası (D-009) bu oturumda ayrıca ele alınıp düzeltildi.

## Offline Çalışma
Ana akış tamamen offline. Yalnızca Tesseract.js OCR ilk kullanımda internet
gerektirir.

## XSD DOĞRULAMA BASELINE (2026-08-26) — TAMAMLANDI ✅ (T-007/T-011 hariç)
16/16 test PASS (önceki oturum). **Bu baseline T-007 ve T-011'i kapsamıyor**
— ikisi de baseline'dan sonra eklendi/düzeltildi.

## ICN Gömme Yöntemi (T-004) — XSD/XML Açısından KAPATILDI ✅
Değişmedi.

## BREX Durumu — ÇALIŞMA DURDURULDU ⏸️ (D-006)
Kullanıcı tarafından bu oturumda açıkça talimat verildi: S1000D Default
BREX araştırması, BREX validator geliştirme ve BREX referanslarının
değiştirilmesi **artık geliştirme önceliği değildir.** BREX konusu "MSB /
proje girdisi gelene kadar BEKLEMEDE" olarak tutulmalı. `SETTINGS.brex` ve
referans XML'lerin `brexDmRef` değerleri **değiştirilmedi** ve MSB/proje
girdisi gelmeden **değiştirilmemeli.**

## SNS Sistem Kodu ve MSB Katmanı — Kavramsal Netleştirme (D-007, D-008)
Değişmedi.
