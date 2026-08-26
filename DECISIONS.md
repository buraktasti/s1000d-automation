# DECISIONS.md — Kararlar ve Doğrulanmış Teknik Riskler

Bu dosya, proje hakkında verilmiş kararları ve **doğrulanmış** (varsayıma
dayanmayan) teknik riskleri kayıt altına alır. Doğrulanamayan konular buraya
risk olarak yazılmaz; bkz. `PROJECT_STATUS.md` → "Doğrulanmamış / İncelenmesi
Gereken Noktalar".

---

## D-001 — systemCode üretim tutarsızlığı (ÇÖZÜLDÜ ✅)

**Durum:** Çözüldü ve gerçek repo dosyasında doğrulandı. Kapatıldı.

**Sorun (kök neden):** İki ayrı yerde `systemCode`, `row.dmc`'nin 3. segmenti
yerine ayrı saklanan `row.malzKat + row.sysCode` alanlarından üretiliyordu:
1. `identAndStatus()` içindeki `<dmCode systemCode="...">`
2. `genIpd()` içindeki `<catalogSeqNumber systemCode="...">`

Her iki fonksiyondaki diğer tüm konum öznitelikleri (`subSystemCode`,
`subSubSystemCode`, `assyCode`, `disassyCode`, `disassyCodeVariant`) zaten
`row.dmc.split('-')`'den (tek kaynak) türetiliyordu. Detay-sütun-import
yolunda Excel'in "Tam DMC" sütunu ile ayrı "Malzeme Kategori Kodu"/"Sistem
Kodu" sütunları birbiriyle tutarsızsa, bu iki nokta `dmc` string'iyle çelişen
bir `systemCode` üretebiliyordu.

**Uygulanan düzeltme (gerçek dosyada, satır bazlı, minimal):**
1. `identAndStatus()`: `systemCode="'+xmlEscape(row.malzKat+row.sysCode)+'"` → `systemCode="'+xmlEscape(sys)+'"` (`sys` fonksiyonda zaten `row.dmc.split('-')[2]` olarak tanımlıydı, yeni değişken eklenmedi).
2. `genIpd()`: `systemCode="'+xmlEscape(row.malzKat+row.sysCode)+'"` → `systemCode="'+xmlEscape(row.dmc.split('-')[2])+'"` (satır içi, yeni değişken eklenmedi; `subSystemCode`/`subSubSystemCode`/`assyCode` gibi diğer öznitelikler dokunulmadan bırakıldı).

**Test sonuçları:** TEST-01→07, tümü PASS.

**Sonuç:** `systemCode` artık her iki üretim noktasında da tek gerçek kaynak
(`row.dmc`) kullanıyor. Risk giderildi.

---

## D-002 — ZWSP (zero-width space) şüphesi kayıt dışı bırakıldı

**Durum:** Kapalı, risk olarak izlenmiyor. (Değişmedi.)

---

## D-003 — Desteklenen şema kapsamı bilinçli olarak sınırlı tutuldu

**Durum:** Mevcut durum, henüz genişletme kararı yok. (Değişmedi.)

---

## D-004 — XSD/BREX doğrulama katmanı yok

**Durum:** Kısmen güncellendi — XSD tarafı tamamlandı, BREX tarafı yaklaşımı değişti.

**XSD tarafı:** 2026-08-26 tarihinde resmi S1000D Issue 5.0 XSD paketiyle
gerçek `xmllint --schema` doğrulaması yapıldı, 16/16 test PASS. Bu taraf
**baseline olarak kapatıldı**. **Önemli:** Bu baseline, T-007'de eklenen
uzunluk kuralını ve D-009'da düzeltilen infoCode hatasını kapsamıyor —
ikisi de baseline'dan sonra eklendi/düzeltildi ve gerçek XSD paketiyle
henüz teyit edilmedi.

**BREX tarafı:** D-006 ile ayrıntılandırıldı — **BREX çalışması kullanıcı
tarafından durduruldu** (bkz. D-006 güncel not). BREX validator geliştirme
işi, S1000D Default BREX araştırması ve BREX referans değişiklikleri artık
geliştirme önceliği değil; MSB/proje girdisi gelene kadar **BEKLEMEDE**.

---

## D-005 — assyCode / disassyCodeVariant proje uzunluk kuralı eklendi (BEKLEMEDE / XSD İLE DOĞRULANACAK ⏳)

**Durum:** Kod gerçek dosyada uygulandı ve dahili testlerle (birim + regresyon
+ uçtan uca) doğrulandı. Ancak **resmi S1000D Issue 5.0 XSD paketi bu
değişikliğe karşı çalıştırılmadı** — bu nedenle madde "KAPATILDI" değil,
**"BEKLEMEDE / XSD İLE DOĞRULANACAK"** olarak işaretlidir.

**Kaynak:** DERMAN/FORSDOC proje girdisi — kullanıcı tarafından kesin proje
girdisi olarak bildirildi: assyCode = 4 karakter, disassyCodeVariant = 2
karakter.

**Uygulanan düzeltme:** `dmcLengthError(raw)` fonksiyonu, `importText()`
içindeki ortak satır döngüsünde her iki import yolunu (tam-DMC ve
detay-sütun) kapsayan tek bir noktada çağrılıyor.

**Test sonuçları:** 13/13 PASS (TEST-LEN-01→11 + 2 ek test). Ayrıca bu
oturumda (D-009 ile birlikte) gerçek FORSDOC importundan gelen bir hatanın
tespit edilip düzeltilmesiyle, bu kuralın da dahil olduğu import akışının
**gerçek kullanım verisiyle** bir kez daha (dolaylı olarak) sınandığı
kaydedilmelidir — ancak bu hâlâ resmi XSD çapraz doğrulaması yerine geçmez.

**Neden hâlâ açık:** 4/2 karakter kuralının resmi S1000D XSD
karakter/datatype kısıtlarıyla çelişmediği yalnızca genel S1000D bilgisine
dayanarak makul kabul edildi — resmi XSD ile çapraz doğrulanmadı. Madde,
resmi XSD paketi tekrar sağlanıp bu değişiklikle üretilen örnek çıktılar
`xmllint --schema` ile test edilene kadar **"KAPATILDI" olarak
işaretlenemez.**

---

## D-006 — BREX yaklaşımı: S1000D Default BREX artık proje iş kurallarının ana kaynağı değil (ÇALIŞMA DURDURULDU ⏸️)

**Durum:** Kullanıcı tarafından bu oturumda **BREX çalışması tamamen
durduruldu.**

**Önceki karar (hâlâ geçerli, ancak artık aktif geliştirme önceliği
değil):** DERMAN projesinde S1000D Default BREX artık proje iş
kurallarının ana kaynağı olarak kullanılmayacak. Proje iş kuralları için
öncelik sırası: MSB katmanı → MPG/FORSDOC girdileri → doğrulanmış proje
kararları → S1000D XSD yapısı.

**Yeni ek karar (bu oturumda alındı):** S1000D Default BREX araştırması,
BREX validator geliştirme ve BREX referanslarının (`SETTINGS.brex`,
`brexDmRef`) değiştirilmesi **artık mevcut geliştirme önceliği değildir.**
BREX konusu: **"MSB / proje girdisi gelene kadar BEKLEMEDE"** olarak
tutulmalıdır. Mevcut çalışan uygulama BREX nedeniyle değiştirilmeyecek.

**Kod durumu (değişmedi):**
- `SETTINGS.brex` değeri **değiştirilmedi** (hâlâ `S1000D-A-...`).
- 040/420/520/941 referans XML'lerindeki `brexDmRef` değerleri
  **değiştirilmedi**.
- BREX validator geliştirilmedi.

---

## D-007 — SNS Sistem Kodu (FORSDOC) ile dmCode/@systemCode aynı alan DEĞİLDİR

**Durum:** Kavramsal ayrım netleştirildi, doğrulama bekliyor. (Değişmedi.)

---

## D-008 — MSB katmanı: FORSDOC proje konfigürasyonu, uygulamaya yeni alan gerektirmez (henüz)

**Durum:** Kavramsal ayrım netleştirildi, doğrulama bekliyor. (Değişmedi.)

---

## D-009 — infoCode kritik hatası: Excel infoCode sütunu, DMC'den türetilen infoCode'un önüne geçiyordu (ÇÖZÜLDÜ ✅ — kod tarafı; XSD çapraz doğrulaması bekliyor)

**Durum:** Kod gerçek dosyada düzeltildi ve dahili testlerle doğrulandı.
Bu hata **gerçek FORSDOC importunda kullanıcı tarafından tespit edildi** —
üretilen gerçek ZIP paketinde bulundu, simülasyon veya varsayım değil.

**Bulunan gerçek hata:** FORSDOC üzerinde üretilen bir dosyada, dosya adı
`...-040A-A_...` (doğru, DMC'den türetilmiş) olduğu halde, XML içindeki
`dmCode/@infoCode="40"` (2 haneli, **hatalı**) olarak üretilmişti. Doğru
değer `infoCode="040"` (3 haneli) olmalıydı. Kullanıcı, S1000D Issue 5.0
`descript.xsd` içinde `infoCode` pattern'inin `[A-Z0-9]{3}` olduğunu ve
bu nedenle "40" değerinin **geçersiz** olduğunu bildirdi.

**Kök neden:** `importText()` fonksiyonunun detay-sütun import yolunda:
```js
let dmc = get('dmc');
let infoCode = get('infoCode');   // Excel sütunu önce okunuyor
...
const p = parseDmcString(dmc);
if (!infoCode) infoCode = p.infoCode;  // yalnızca Excel değeri BOŞSA DMC'den alınıyor
```
Excel'in "InfoCode" sütununda "040" yerine "40" gibi hatalı biçimlendirilmiş
(baştaki sıfırı Excel tarafından otomatik kırpılmış) bir değer varsa, bu
değer **doğru olan, DMC'den regex ile 3 karakter garantili türetilen
`p.infoCode` değerinin önüne geçiyordu** ve doğrudan XML'e, dosya adına
(kısmen — dosya adı zaten `row.dmc`'den türetiliyor, bu yüzden dosya adı
doğruydu) ve `INFO_SCHEMA`/`INFO_NAMES` eşleşmelerine taşınıyordu.

**Uygulanan düzeltme (gerçek dosyada, minimal):**
1. `let infoCode = get('infoCode');` satırı kaldırıldı; yerine
   `const excelInfoCode = get('infoCode');` (yalnızca karşılaştırma/uyarı
   amaçlı okuma) eklendi.
2. `if (!infoCode) infoCode = p.infoCode;` satırı kaldırıldı; yerine
   `const infoCode = p.infoCode;` — **tam DMC mevcutsa infoCode her zaman
   ve kesin olarak `parseDmcString(dmc)`'den (DMC_RE tarafından zorunlu 3
   karakter olarak eşleştirilir) geliyor.**
3. Excel'deki infoCode sütunu ile DMC'den türetilen infoCode farklıysa,
   kullanıcıya `import-status` mesajında açık bir uyarı veriliyor (sayı +
   ilk 5 örnek), ancak **DMC değeri her zaman esas alınıyor**, satır
   atlanmıyor.
4. `dmc-only` (yalnızca tam DMC) import yolu **hiç değiştirilmedi** —
   bu yolda zaten infoCode her zaman `p.infoCode`'dan geliyordu, bug bu
   yolda hiç yoktu.
5. `INFO_SCHEMA`/`INFO_NAMES` eşleşmeleri kod değişikliği gerektirmedi —
   zaten `infoCode` değişkenini kullanıyorlardı; `infoCode` artık her
   zaman doğru (3 haneli, DMC-türetilmiş) olduğundan bu eşleşmeler
   otomatik olarak düzeldi.

**Test sonuçları (11/11 PASS, 0 FAIL):**
- TEST-INFO-01 (DMC=...-040A-A, Excel infoCode="40" hatalı): row.infoCode="040" ✓, XML dmCode/@infoCode="040" ✓, uyuşmazlık uyarısı verildi ✓ → PASS
- TEST-INFO-02 (DMC=...-040A-A, Excel infoCode="040" doğru): row.infoCode="040" ✓, uyarı verilmedi (gerekli değil) ✓ → PASS
- TEST-INFO-03 (dmc-only import, ...-040A-A): row.infoCode="040" ✓, davranış bozulmadı → PASS
- TEST-INFO-04 (520, Excel kasıtlı hatalı "5"): row.infoCode="520" (DMC'den, Excel göz ardı edildi) → PASS
- TEST-INFO-05 (420, Excel kasıtlı hatalı "42"): row.infoCode="420" → PASS
- TEST-INFO-06 (941, Excel kasıtlı hatalı "9"): row.infoCode="941" → PASS
- TEST-INFO-07 (040 XML infoCode değeri `[A-Z0-9]{3}` desenine uyuyor mu — yalnızca regex ile, gerçek XSD olmadan): PASS

**Neden hâlâ tam kapatılmadı:** TEST-INFO-07, yalnızca bir regex deseniyle
(`^[A-Z0-9]{3}$`) kontrol yapıldı — gerçek `descript.xsd` ile `xmllint
--schema` çalıştırılmadı, çünkü bu oturumda resmi S1000D XSD paketi
sağlanmadı. Bu nedenle "infoCode artık her zaman 3 karakter üretiyor"
iddiası dahili testlerle güçlü şekilde destekleniyor, ancak resmi XSD ile
**henüz çapraz doğrulanmadı**. T-007 ile aynı bekleme kategorisinde
tutulmalı — bkz. `TODO.md` T-011.

**Kapsam notu:** Bu düzeltme yalnızca `importText()`'in detay-sütun import
yolundaki `infoCode` atamasını değiştirdi. `identAndStatus()`, `genIpd()`,
`systemCode`, BREX fonksiyonlarına dokunulmadı; başka hiçbir fonksiyonda
refactor yapılmadı.

---

## D-010 — buildImfXml() içinde schemaLocation dosya adı düzeltmesi: icnMetadata.xsd → icnmetadata.xsd

**Durum:** Kod gerçek dosyada düzeltildi. **Bu düzeltme, kullanıcının
bildirdiği doğrulamaya dayanıyor — bu oturumda Claude tarafından resmi
S1000D XSD paketiyle bağımsız olarak teyit edilmedi** (paket bu oturumda
sağlanmadı).

**Bulunan durum:** `buildImfXml()` fonksiyonu, `xsi:noNamespaceSchemaLocation`
değerinde `.../xml_schema_flat/icnMetadata.xsd` (büyük M harfi) kullanıyordu.
Kullanıcı, resmi S1000D Issue 5.0 paketindeki gerçek dosya adının küçük
harfle `icnmetadata.xsd` olduğunu bildirdi.

**Uygulanan düzeltme:** İlgili satırda `icnMetadata.xsd` → `icnmetadata.xsd`
olarak değiştirildi (tek karakter büyük/küçük harf düzeltmesi).

**Test sonucu:** TEST-IMF-01 — `buildImfXml()` çıktısında schemaLocation'ın
küçük harfli `icnmetadata.xsd` içerdiği ve büyük harfli eski değeri
içermediği doğrulandı → PASS.

**Neden tam kapatılmadı:** Bu değişiklik, dosya sisteminde büyük/küçük harf
duyarlı olabilecek bir ortamda (örn. Linux tabanlı CSDB sunucuları) pratik
önem taşır, ancak Claude bu oturumda resmi S1000D paketine erişemediğinden
dosya adının gerçekten küçük harfli olduğunu **bağımsız olarak
doğrulayamadı** — yalnızca kullanıcı beyanına dayanarak uygulandı. İleride
resmi paket sağlanırsa bu nokta ayrıca teyit edilmeli — bkz. `TODO.md`
T-012.
