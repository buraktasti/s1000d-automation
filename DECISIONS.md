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

Diff ile doğrulandı: dosyada yalnızca bu 2 satır değişti (1675 satır → 1675 satır, yapısal fark yok).

**Test sonuçları (gerçek dosyadan çıkarılan fonksiyonlarla, Node.js'te çalıştırıldı):**
- TEST-01 (tam DMC import, descript): PASS — systemCode=GA1
- TEST-02 (detay import, malzKat="H" tutarsız): PASS — systemCode=GA1 (önceden FAIL idi, "HA1" üretiyordu)
- TEST-03 (aynı tutarsız veriyle IPD): PASS — catalogSeqNumber systemCode=GA1, diğer öznitelikler (subSystemCode=1, subSubSystemCode=1, assyCode=0001) bozulmadı
- TEST-04 (040 descript regresyon): PASS
- TEST-05 (420 fault regresyon): PASS
- TEST-06 (520 proced regresyon): PASS
- TEST-07 (941 ipd regresyon): PASS

**Sonuç:** `systemCode` artık her iki üretim noktasında da tek gerçek kaynak
(`row.dmc`) kullanıyor. Risk giderildi.

---

## D-002 — ZWSP (zero-width space) şüphesi kayıt dışı bırakıldı

**Durum:** Kapalı, risk olarak izlenmiyor.

Önceki bir analizde HTML kaynağının bazı template literal'lerinde (`<title>`
gibi etiketler içinde) zero-width space karakteri şüphesi gündeme geldi.
Bu şüphe **ham kaynak dosya üzerinde doğrulanamadı** (yalnızca arama/indeksleme
çıktısında gözlemlendi, kaynak dosyanın kendisinde mi yoksa arama motoru
render'ında mı olduğu belirlenemedi). Bu nedenle proje hafızasında açık bir
sorun/risk olarak **kaydedilmemiştir**. İleride ham dosya doğrudan
incelendiğinde tekrar değerlendirilebilir.

---

## D-003 — Desteklenen şema kapsamı bilinçli olarak sınırlı tutuldu

**Durum:** Mevcut durum, henüz genişletme kararı yok.

`SUPPORTED_SCHEMAS` yalnızca `descript`, `proced`, `fault`, `ipd` içeriyor.
`schedul` ve `wrngdata` `INFO_SCHEMA` tablosunda eşlenmiş ama üretim
fonksiyonu yazılmamış. Bu bir hata değil, henüz yapılmamış iş olarak
kayıtlıdır (bkz. `TODO.md`).

---

## D-004 — XSD/BREX doğrulama katmanı yok

**Durum:** Kısmen güncellendi — XSD tarafı tamamlandı, BREX tarafı yaklaşımı değişti.

Üretilen hiçbir XML, resmi S1000D XSD şemasına veya projenin BREX kurallarına
karşı doğrulanmıyor. `qualityAssurance` her zaman `<unverified/>` olarak
sabitlenmiş.

**XSD tarafı:** 2026-08-26 tarihinde resmi S1000D Issue 5.0 XSD paketiyle
gerçek `xmllint --schema` doğrulaması yapıldı, 16/16 test PASS. Bu taraf
**baseline olarak kapatıldı** — bkz. `PROJECT_STATUS.md` → "XSD DOĞRULAMA
BASELINE". **Önemli:** Bu baseline, T-007'de eklenen uzunluk kuralını
kapsamıyor (T-007 sonradan eklendi ve gerçek XSD paketiyle henüz teyit
edilmedi — bkz. D-005).

**BREX tarafı:** D-006 ile ayrıntılandırıldı — proje artık S1000D Default
BREX'i "proje iş kurallarının ana kaynağı" olarak kullanmama kararı aldı
(bkz. D-006). Bu nedenle BREX validator geliştirme işi bu aşamada
**beklemeye alındı**, iptal edilmedi.

---

## D-005 — assyCode / disassyCodeVariant proje uzunluk kuralı eklendi (BEKLEMEDE / XSD İLE DOĞRULANACAK ⏳)

**Durum:** Kod gerçek dosyada uygulandı ve dahili testlerle (birim + regresyon)
doğrulandı. Ancak **resmi S1000D Issue 5.0 XSD paketi bu değişikliğe karşı
çalıştırılmadı** — bu nedenle madde "KAPATILDI" değil, **"BEKLEMEDE / XSD İLE
DOĞRULANACAK"** olarak işaretlidir. Önceki bir özet bu maddeyi hatalı şekilde
"KAPATILDI" olarak sunmuştu; bu, bu güncellemeyle düzeltilmiştir.

**Kaynak:** DERMAN/FORSDOC proje girdisi — kullanıcı tarafından kesin proje
girdisi olarak bildirildi:
- Bileşen Kodu (assyCode) uzunluğu: **tam 4 karakter**
- Sökme Kodu Türevi (disassyCodeVariant) uzunluğu: **tam 2 karakter**
  (disassyCode standart olarak zaten 2 karakter olduğundan, DMC'deki
  birleşik "sokum" segmentinin toplamda 4 karakter olması gerekir)

**Sorun (kök neden):** `DMC_RE` regex'i içinde `assyCode` (grup 5, `bilesen`)
ve birleşik `disassyCode+disassyCodeVariant` (grup 6, `sokum`) segmentleri
`[^-\s]+` ile tanımlıydı — yani tire/boşluk olmayan **herhangi bir uzunlukta**
dizi kabul ediliyordu. `parseDmcString()` bu segmentleri olduğu gibi
döndürüyor, hiçbir yerde uzunluk kontrolü yoktu. Bu durum hem "tam DMC"
import yolunda hem "detay-sütun" import yolunda geçerliydi.

**Uygulanan düzeltme (gerçek dosyada, minimal, ~15 satır ekleme):**
1. `looksLikeDmc()` fonksiyonundan hemen sonra yeni bir `dmcLengthError(raw)`
   fonksiyonu eklendi — `raw.bilesen` uzunluğunun 4, `raw.sokum` uzunluğunun
   4 olup olmadığını kontrol eder, aksi halde açıklayıcı bir hata metni
   döndürür.
2. `importText()` içindeki ana satır döngüsünde, `raw` nesnesi her iki
   import yolunda da (dmc-only ve detay-sütun) oluşturulduktan **hemen
   sonra, ortak tek bir noktada** `dmcLengthError(raw)` çağrılır; hata
   dönerse satır atlanır (`lengthSkipped` sayacı artırılır, ilk 5 örnek
   `lengthErrorSamples` içinde tutulur), `rows.push(raw)` çağrılmaz.
3. Kullanıcıya gösterilen durum mesajlarına (`import-status`), atlanan
   satır sayısı ve örnek hata metinleri eklendi.
4. `identAndStatus()`, `genIpd()`, `systemCode`, BREX ile ilgili hiçbir
   fonksiyona dokunulmadı — değişiklik yalnızca import/doğrulama katmanında.

**Karakter kümesi ile ilgili not:** `DMC_RE` zaten `[^-\s]+` (tire/boşluk
dışında her karakter) kabul ediyordu; yeni kural bu karakter kümesini
**genişletmedi**, yalnızca uzunluk üzerinde ek bir kısıt ekledi (mevcut kabul
edilen kümenin bir alt kümesi). S1000D XSD'sinin `assyCode`/
`disassyCodeVariant` için tam karakter/datatype kısıtları bu oturumda
**doğrulanamadı** — gerçek XSD dosyası bu oturumda sağlanmadı (önceki
oturumda kullanılan XSD paketi bu oturumda mevcut değildi). Bu nedenle 4
karakter kuralının S1000D XSD'siyle çelişmediği yalnızca genel S1000D
bilgisine dayanarak makul kabul edildi, **resmi XSD ile çapraz
doğrulanmadı**. İleride gerçek XSD sağlanırsa bu nokta ayrıca teyit
edilmeli — madde ancak bu teyitten sonra "KAPATILDI" olarak işaretlenmelidir.

**Test sonuçları (gerçek dosyadan çıkarılan fonksiyonlarla, Node.js/jsdom
benzeri stub DOM ile çalıştırıldı):**

Aşağıdaki testlerde **"PASS" ifadesi, test senaryosunun beklenen davranışı
doğru şekilde gösterdiği anlamına gelir — geçersiz bir DMC değerinin kabul
edildiği anlamına GELMEZ.** Özellikle TEST-LEN-02/03/05/06'da PASS, "geçersiz
girdi doğru şekilde reddedildi" demektir:

- TEST-LEN-01 (assyCode=0001, geçerli): girdi kabul edildi (hata yok) → PASS
- TEST-LEN-02 (assyCode=001, 3 karakter, geçersiz): **girdi reddedildi** (dmcLengthError hata döndürdü) → PASS
- TEST-LEN-03 (assyCode=00001, 5 karakter, geçersiz): **girdi reddedildi** (dmcLengthError hata döndürdü) → PASS
- TEST-LEN-04 (disassyCodeVariant=AA, geçerli): girdi kabul edildi (hata yok) → PASS
- TEST-LEN-05 (disassyCodeVariant=A, 1 karakter, geçersiz): **girdi reddedildi** (dmcLengthError hata döndürdü) → PASS
- TEST-LEN-06 (disassyCodeVariant=AAA, 3 karakter, geçersiz): **girdi reddedildi** (dmcLengthError hata döndürdü) → PASS
- TEST-LEN-07 (geçerli tam DMC import): satırlar başarıyla içe aktarıldı → PASS
- TEST-LEN-08 (040 descript regresyon): XML üretimi ve öznitelikler bozulmadı → PASS
- TEST-LEN-09 (420 fault regresyon): XML üretimi ve öznitelikler bozulmadı → PASS
- TEST-LEN-10 (520 proced regresyon): XML üretimi ve öznitelikler bozulmadı → PASS
- TEST-LEN-11 (941 ipd regresyon): XML üretimi ve öznitelikler bozulmadı → PASS
- Ek test (dmc-only modda geçersiz satırın geçerli satır yanında reddedilmesi): **geçersiz satır atlandı, geçerli satır alındı** → PASS
- Ek-2 test (detay-sütun import modunda da aynı kontrolün uygulanması): **geçersiz satır atlandı, geçerli satır alındı** → PASS

**Toplam: 13/13 test PASS, 0 FAIL.** (Yukarıdaki netleştirme gereği: bu 13
PASS'ın 4 tanesi — TEST-LEN-02/03/05/06 — "geçersiz girdi doğru şekilde
reddedildi" anlamına gelir, "geçersiz girdi kabul edildi" anlamına gelmez.)

**Sonuç:** `assyCode` ve `disassyCodeVariant` artık her iki import yolunda
da (tam-DMC ve detay-sütun) tek ortak bir noktada, tutarlı şekilde
doğrulanıyor ve dahili testlerle davranışı teyit edildi. **Ancak resmi S1000D
XSD paketiyle çapraz doğrulama yapılmadığından madde açık kalmaya devam
ediyor** — bkz. `TODO.md` T-007.

---

## D-006 — BREX yaklaşımı: S1000D Default BREX artık proje iş kurallarının ana kaynağı değil

**Durum:** Karar alındı, uygulama katmanına henüz yansıtılmadı (bekliyor).

**Önceki durum:** `SETTINGS.brex` varsayılanı ve 4 referans XML'in
`brexDmRef` değeri `S1000D-A-04-10-0301-00A-022A-D` idi. Kullanıcı tarafından
sağlanan gerçek bir BREX Data Module dosyası (`DMC-S1000D-G-04-10-0301-00A-
022A-D_001-00_EN-US.XML`) incelendi ve bunun S1000D Issue 5.0'ın resmi
default BREX'i olduğu dosya içeriğiyle (`commonInfo`, 265 kural, `brexDmRef`
self-reference) doğrulandı. `A` ile `G` arasındaki fark netleştirilmeye
çalışıldı; kullanıcı `A`'nın Issue 4.0'a, `G`'nin Issue 5.0'a ait olduğunu
bildirdi (bu bildirim, bu oturumda bağımsız bir Issue 4.0 kaynağıyla
doğrulanamadı — kullanıcı beyanına dayanıyor).

**Yeni karar (kullanıcı tarafından alındı):** DERMAN projesinde S1000D
Default BREX artık proje iş kurallarının **ana kaynağı olarak
kullanılmayacak**. S1000D Issue 5.0, yalnızca XML/XSD yapısal doğrulaması
ve şema kuralları için teknik standart olarak kullanılmaya devam edecek.
Proje iş kuralları için öncelik sırası:
1. MSB katmanı / MSB proje kuralları
2. MPG/FORSDOC üzerinde tanımlanan kritik proje girdileri
3. DERMAN projesi için doğrulanmış proje kararları
4. S1000D Issue 5.0 XSD yapısı

`S1000D-G-04-10-0301-00A-022A-D` şimdilik yalnızca **referans bilgi** olarak
tutulmalı, DERMAN projesine otomatik uygulanmamalı.

**Bu kararın uygulama koduna etkisi (HENÜZ YAPILMADI):**
- `SETTINGS.brex` değeri **değiştirilmedi** (hâlâ `S1000D-A-...`).
- 040/420/520/941 referans XML'lerindeki `brexDmRef` değerleri
  **değiştirilmedi**.
- BREX validator geliştirilmedi.
- Proje-özel veya MSB'ye özgü BREX üretilmedi/tahmin edilmedi.

**Açık konu:** MSB katmanına ait gerçek bir BREX veya Business Rules kaynağı
elimizde yok. Bu konu "BEKLEYEN PROJE GİRDİSİ" olarak tutulmalı — bkz.
`PROJECT_STATUS.md` → "Doğrulanmamış / İncelenmesi Gereken Noktalar".

---

## D-007 — SNS Sistem Kodu (FORSDOC) ile dmCode/@systemCode aynı alan DEĞİLDİR

**Durum:** Kavramsal ayrım netleştirildi, doğrulama bekliyor.

Önceki bir analizde, FORSDOC proje tanımındaki "SNS Sistem Kodu = 00" değeri
ile örnek VM'lerdeki `dmCode/@systemCode = GA1` değeri arasında bir çelişki
olduğu izlenimi oluşmuştu. Kullanıcı bu değerlendirmeyi düzeltti:

- **SNS Sistem Kodu = 00**, FORSDOC üzerindeki SNS/proje ağacının
  tanımlanmasına ait bir **proje konfigürasyon girdisidir**.
- **dmCode/@systemCode**, belirli bir veri modülünün DMC kodlama yapısının
  parçasıdır.

Bu iki alanın aynı kavram olduğuna dair elimizde kanıt yoktur; bu nedenle
mevcut `GA1` örnekleri "SNS Sistem Kodu 00 ile çelişiyor" şeklinde
**değerlendirilmemelidir**. SNS Sistem Kodu konusu:
**DOĞRULANMAYI BEKLEYEN FORSDOC/SNS KONFİGÜRASYON BİLGİSİ** olarak
tutulmalıdır. Bu konu için henüz kod değişikliği yapılmamıştır ve
yapılmamalıdır.

---

## D-008 — MSB katmanı: FORSDOC proje konfigürasyonu, uygulamaya yeni alan gerektirmez (henüz)

**Durum:** Kavramsal ayrım netleştirildi, doğrulama bekliyor.

DERMAN projesinin FORSDOC üzerindeki katmanı **S1000D 5.0 → MSB** olarak
tanımlıdır. Bu, projenin **oluşturulduğu katmandır**. HTML uygulamasında
ayrıca "MSB" adında bir alan bulunmaması, tek başına bir hata olarak
değerlendirilmemelidir.

MSB iş kurallarının XML çıktısında özel bir alan veya değer gerektirdiğini
gösteren gerçek bir kaynak sağlanmadıkça, uygulamaya MSB'ye özgü yeni bir
alan/kod **eklenmeyecektir**. Bu konu:
**BEKLEYEN MSB İŞ KURALI GİRDİSİ** olarak tutulmalıdır.
