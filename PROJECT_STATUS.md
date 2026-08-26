# PROJECT_STATUS.md — Mevcut Durum

Son güncelleme: 2026-08-26 (ÜÇÜNCÜ TUR — kullanıcının FORSDOC'a daha önce
BAŞARIYLA yüklediği gerçek dosyalarla karşılaştırma yapıldı; asıl kök neden
bulundu: <dmodule> kök elemanında xsi:noNamespaceSchemaLocation hiç yoktu
— D-014. İKİNCİ TUR'da uygulanan systemCode-2-karakter düzeltmesi [D-012]
YANLIŞ ÇIKTI ve GERİ ALINDI, çünkü kullanıcının başarılı dosyalarında
systemCode="GA1" [3 karakter] olarak duruyordu. İLK TUR'da: 7906 satırın
tamamının yanlışlıkla reddedilmesine neden olan hata bulundu ve düzeltildi
— D-011.)

## Genel Durum (ÜÇÜNCÜ TUR — bu mesajlaşmada)
D-012 düzeltmesinden sonra kullanıcı AYNI FORSDOC hatasını tekrar aldı.
Kullanıcıdan, FORSDOC'a **daha önce gerçekten başarıyla yüklediği** 11
dosyalık bir ZIP istendi ve bizim üretimimizle doğrudan karşılaştırıldı.
Bu karşılaştırma kesin kanıt sağladı:

1. **D-012 GERİ ALINDI:** Başarılı dosyalarda `systemCode="GA1"` (3
   karakter) duruyordu ve FORSDOC bunu kabul etmişti. "systemCode her
   zaman 2 karakter olmalı" varsayımı bu proje için yanlıştı (genel S1000D
   örneklerine dayanıyordu, bu projenin FORSDOC yapılandırmasına değil).
   Kod eski (ham segment) davranışına geri döndürüldü.
2. **D-014 (asıl kök neden, muhtemelen):** Başarılı dosyaların TÜMÜNDE
   `<dmodule>` kök elemanında `xmlns:xsi` + `xsi:noNamespaceSchemaLocation`
   (şemaya özel XSD adresi) vardı. Bizim üretimimizde bu **hiç yoktu.**
   FORSDOC muhtemelen veri modülünü hangi şema/katmana ait olduğunu bu
   değerden belirliyor. `dmHeader(schema)` fonksiyonu eklendi, artık her
   4 şema türü için doğru `xsi:noNamespaceSchemaLocation` üretiliyor.

**Kullanıcının bir sonraki adımı:** Güncellenmiş HTML ile yeni bir ZIP
üretip FORSDOC'a tekrar import etmek ve sonucu bildirmek (bkz. `TODO.md`
T-014, güncellendi).

## Genel Durum (İKİNCİ TUR — bu oturumun başında)
Kullanıcı, önceki düzeltmeyle (D-011) ürettiği bir ZIP paketini FORSDOC'a
aktarmayı denedi ve şu hatayı aldı:
```json
{"code":"S1000D.Error:00183","message":"Katmana veri aktarılırken bir hata oluştu.","validationErrors":null}
```
ZIP paketi (`ICN-TH743-00001-001-01.PNG`, `IMF-...XML`, `DMC-...XML`)
incelendi ve **iki anomali** bulundu:

0. **D-012 (İLK TAHMİN — SONRADAN YANLIŞ ÇIKTI, geri alındı; bkz. yukarı
   "ÜÇÜNCÜ TUR" ve `DECISIONS.md`):** `dmCode/@systemCode` özniteliği 3
   karakter (`"GA1"`) üretiliyordu; genel S1000D örnekleri 2 karakter
   gösteriyordu. Bu varsayıma dayanarak yapılan düzeltme YANLIŞTI.
1. **D-013 (hâlâ geçerli):** IMF dosyasında aynı "Konum" numarasına (174)
   sahip iki farklı parça satırı, yinelenen `icnObjectIdent="hot174"`
   üretiyordu. Artık ikinci ve sonraki yinelenen kayıtlara harf soneki
   ekleniyor (`hot174b`, ...).

**Önemli:** FORSDOC hatası `validationErrors: null` döndürdüğü için hangi
alanın tam olarak reddedilme nedeni olduğu kesin değil — D-012 en güçlü
adaydır (hem "katman" terminolojisiyle örtüşüyor hem resmi S1000D
örnekleriyle çelişiyor hem de kodun kendi iç tutarsızlığını gösteriyor),
ancak **kullanıcının düzeltilmiş dosyayı FORSDOC'ta yeniden denemesi ve
sonucu bildirmesi gerekiyor** (bkz. `TODO.md` T-014).

## Genel Durum (İLK TUR — bu oturumun başında)
Uygulama çalışır durumda. Bu oturumda **bir kritik kod düzeltmesi**
uygulandı ve önceki oturumların iddia ettiği bazı düzeltmelerin
**gerçek dosyada mevcut olmadığı** doğrulandı:

1. **KRİTİK hata düzeltmesi (D-011):** Detay-sütun (Excel/CSV) importta,
   `bilesen`, `sokum`, `altAltSis`, `mic`, `sdc`, `kkk` alanları Excel'in
   sayısal biçimlendirme nedeniyle kırpılmış (ör. `"0000"` → `"0"`)
   sütun değerleriyle eziliyordu (`get('x') || p.x` deseni, "0" truthy
   olduğu için DMC'den doğru türetilen değerin önüne geçiyordu). Bu,
   kullanıcının bildirdiği **7906 satırın tamamının** proje uzunluk
   kuralı (`dmcLengthError`) nedeniyle yanlışlıkla reddedilmesinin **tam
   olarak kanıtlanmış kök nedenidir.** Bu oturumda gerçek dosyadan kod
   çıkarılıp çalıştırılarak hem hata yeniden üretildi (düzeltme öncesi:
   0/7906 kabul) hem de düzeltme doğrulandı (düzeltme sonrası: 7906/7906
   kabul).

2. **Önceki oturum iddiası doğrulanamadı / yanlış çıktı:** Devir notları,
   bu hatanın önceki bir oturumda zaten düzeltildiğini ve "10/10 test
   PASS" aldığını iddia ediyordu. Bu oturumda **gerçek dosya satır satır
   incelendi** ve düzeltmenin **mevcut olmadığı** tespit edildi — yani ya
   iddia hatalıydı ya da düzeltme bir şekilde kaybolmuştu. Bu oturum,
   iddiaya güvenmek yerine kodu doğrudan test ederek gerçek durumu ortaya
   çıkardı ve şimdi gerçekten düzeltti.

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
- [x] `systemCode` üretim tutarlılığı (D-001)
- [~] assyCode (4)/disassyCodeVariant (2) proje uzunluk kuralı — kod uygulandı, XSD çapraz doğrulaması bekliyor (T-007)
- [~] infoCode her zaman DMC'den türetiliyor (detay-sütun import yolunda) — kod uygulandı, dahili testlerle doğrulandı, XSD çapraz doğrulaması bekliyor (T-011, D-009)
- [~] IMF schemaLocation küçük harf düzeltmesi — kod uygulandı, bağımsız teyit bekliyor (T-012, D-010)
- [~] **mic/sdc/altAltSis/bilesen/sokum/kkk her zaman DMC'den türetiliyor (detay-sütun import yolunda)** — kod bu oturumda uygulandı, dahili testlerle (54/54 + 7906 satırlık ölçek testi) doğrulandı, XSD çapraz doğrulaması bekliyor (T-013, D-011)

## Doğrulanmış Eksikler / Yapılmayanlar
- [ ] `schedul`/`wrngdata` şemaları — üretim fonksiyonu yok
- [ ] `crew`, `process` ve diğer S1000D şema türleri — desteklenmiyor
- [ ] BREX kural doğrulama — **çalışma durduruldu** (bkz. "BREX Durumu")
- [ ] Gerçek teknik içerik üretimi — placeholder/iskelet
- [ ] T-007, T-011 ve T-013'ün resmi S1000D XSD paketiyle çapraz doğrulaması — **yapılmadı**

## Doğrulanmamış / İncelenmesi Gereken Noktalar
- ZWSP şüphesi — risk olarak kayıtlı değil (D-002)
- BREX kimliği (A vs G) — **artık aktif araştırma konusu değil**, BREX çalışması durduruldu (D-006)
- MSB'ye ait BREX/Business Rules kaynağı — elimizde yok, BEKLEMEDE (D-006)
- SNS Sistem Kodu (FORSDOC "00") ile dmCode/@systemCode ilişkisi — BEKLEYEN FORSDOC/SNS GİRDİSİ (D-007)
- MSB katmanının uygulamada temsili — BEKLEYEN MSB İŞ KURALI GİRDİSİ (D-008)
- ICN Metodu "İkisi Birden" ve SNS Dili — DOĞRULANAMADI (T-010)
- **T-007, T-011 ve T-013'ün resmi XSD ile çapraz doğrulaması** — yapılmadı, XSD paketi bu oturumda da sağlanmadı
- **T-012 icnmetadata.xsd dosya adının gerçekten küçük harfli olduğu** — yalnızca kullanıcı beyanına dayanıyor, bağımsız teyit edilmedi

## T-001 / T-006 Durumu (KAPATILDI ✅)
Detay: `DECISIONS.md` D-001.

## T-007 Durumu (BEKLEMEDE / XSD İLE DOĞRULANACAK ⏳)
Detay: `DECISIONS.md` D-005.

## T-011 Durumu (BEKLEMEDE / XSD İLE DOĞRULANACAK ⏳) — infoCode kritik hatası
Detay: `DECISIONS.md` D-009. Bu oturumda regresyon testleriyle yeniden
doğrulandı, değişmedi.

## T-012 Durumu (BAĞIMSIZ TEYİT BEKLİYOR) — icnmetadata.xsd düzeltmesi
Detay: `DECISIONS.md` D-010. Bu oturumda değişmedi, `grep` ile varlığı
teyit edildi.

## T-013 Durumu (YENİ — BEKLEMEDE / XSD İLE DOĞRULANACAK ⏳) — bilesen/sokum/altAltSis/mic/sdc/kkk kritik hatası

**Kaynak:** Gerçek FORSDOC CSV importunda kullanıcı tarafından bildirilen
**7906 satırın tamamının** yanlışlıkla reddedilmesi. Bu oturumda kök neden
gerçek kod üzerinde doğrulandı ve düzeltildi (bkz. `DECISIONS.md` D-011).

**Kök neden:** Detay-sütun import yolunda, tam DMC geçerli olduğu halde
`bilesen`/`sokum`/`altAltSis`/`mic`/`sdc`/`kkk` alanları için
`get('x') || p.x` deseni kullanılıyordu. Excel'in sayısal biçimlendirmesi
bu sütunlardaki baştaki sıfırları kırptığında (ör. `"0000"` → `"0"`),
kırpılmış ama boş olmayan Excel değeri, DMC'den doğru türetilen değerin
önüne geçiyordu. Bu da hem `dmcLengthError()`'ın geçerli satırları
reddetmesine hem de `genIpd()`/`identAndStatus()` içinde yanlış
assyCode/subSystemCode/subSubSystemCode/itemLocationCode/modelIdentCode/
systemDiffCode üretilmesine yol açıyordu.

**Düzeltme:** D-009'daki infoCode ilkesiyle birebir aynı yaklaşım: tam DMC
geçerliyse bu alanlar artık her zaman `parseDmcString(dmc)`'den geliyor;
Excel'in karşılık gelen sütunu yalnızca uyuşmazlık uyarısı için okunuyor.

**Test sonuçları:** 54/54 dahili test PASS (bkz. `DECISIONS.md` D-011) +
7906 satırlık gerçekçi ölçek testinde düzeltme öncesi 0/7906 → düzeltme
sonrası 7906/7906 kabul.

**Neden hâlâ açık:** Resmi S1000D Issue 5.0 XSD paketiyle `xmllint
--schema` çapraz doğrulaması bu oturumda da yapılamadı (paket
sağlanmadı). T-007/T-011 ile aynı bekleme kategorisinde.

## Uçtan Uca Test Sonucu (bu oturumda tekrar koşuldu — kod tabanlı, gerçek dosyadan çıkarılan fonksiyonlarla)
| Test Grubu | Sonuç |
|---|---|
| GRUP A — Excel bilesen/sokum/infoCode kırpılmış (5 DMC) | PASS (20/20 alt kontrol) |
| GRUP B — dmc-only import regresyonu | PASS |
| GRUP C — Excel tutarlıysa uyarı üretilmemeli | PASS |
| GRUP D — gerçekten geçersiz DMC hâlâ reddediliyor mu | PASS |
| GRUP E — uyuşmazlık sayaçları/örnek mesajlar | PASS |
| GRUP F — 040/420/520/941 XML üretim regresyonu + 941/ipd alan doğruluğu | PASS |
| ÖLÇEK TESTİ — 7906 satır gerçekçi FORSDOC senaryosu | PASS (7906/7906) |

**Not:** Bu oturumda tarayıcı/DOM ortamı (canvas, Tesseract OCR, dosya
sürükle-bırak) test edilmedi — yalnızca `importText()`/XML üretim
fonksiyonlarının saf mantığı, gerçek HTML dosyasından programatik olarak
çıkarılıp Node.js'te test edildi. Bu, önceki oturumun E2E-06 (ICN/canvas/
OCR) sınırlamasıyla aynı kategoridedir.

## Offline Çalışma
Ana akış tamamen offline. Yalnızca Tesseract.js OCR ilk kullanımda internet
gerektirir.

## XSD DOĞRULAMA BASELINE (2026-08-26) — TAMAMLANDI ✅ (T-007/T-011/T-013 hariç)
16/16 test PASS (önceki oturum). **Bu baseline T-007, T-011 ve T-013'ü
kapsamıyor** — üçü de baseline'dan sonra eklendi/düzeltildi.

## ICN Gömme Yöntemi (T-004) — XSD/XML Açısından KAPATILDI ✅
Değişmedi.

## BREX Durumu — ÇALIŞMA DURDURULDU ⏸️ (D-006)
Bu oturumda BREX konusuna **hiç dönülmedi.** `SETTINGS.brex` ve referans
XML'lerin `brexDmRef` değerleri değişmedi (grep ile bu oturumda teyit
edildi). BREX konusu "MSB / proje girdisi gelene kadar BEKLEMEDE" olarak
kalmaya devam ediyor.

## SNS Sistem Kodu ve MSB Katmanı — Kavramsal Netleştirme (D-007, D-008)
Değişmedi.
