# DECISIONS.md — Kararlar ve Doğrulanmış Teknik Riskler

Bu dosya, proje hakkında verilmiş kararları ve **doğrulanmış** (varsayıma
dayanmayan) teknik riskleri kayıt altına alır. Doğrulanamayan konular buraya
risk olarak yazılmaz; bkz. `PROJECT_STATUS.md` → "Doğrulanmamış / İncelenmesi
Gereken Noktalar".

---

## D-012 — [İLK TESPİT — SONRADAN GERİ ALINDI, bkz. aşağıda "D-012 — [GERİ ALINDI ⚠️]"]

Bu maddenin ilk hali (`systemCode`'u her zaman 2 karaktere zorlama, `"GA1"`
→ `"A1"`) bu oturumda uygulanmış, ancak kullanıcının FORSDOC'a **daha önce
gerçekten başarıyla yüklediği dosyalar** incelendiğinde `systemCode="GA1"`
(3 karakter) değerinin FORSDOC tarafından sorunsuz kabul edildiği
görülmüştür. Bu nedenle düzeltme **hatalıydı ve geri alındı.** Asıl
FORSDOC hatasının gerçek nedeni D-014'te açıklanmıştır. Ayrıntılı geri
alma notu için aşağıdaki "D-012 — [GERİ ALINDI ⚠️]" bölümüne bakınız.

**Durum:** Aynı ZIP incelemesinde ikinci bir anomali olarak bulundu ve
düzeltildi.

**Bulunan gerçek durum:** `IMF-TH743-00001-001-01_001-00.XML` dosyasında
`icnObjectIdent="hot174"` **iki kez** görünüyordu — parça listesinde
"Konum" (hotspot) numarası "174" olan iki farklı satır (iki farklı NSN/
parça, ikisi de "SILINDIR GOMLEGI" başlıklı) olduğu için. `icnObjectIdent`
gibi kimlik amaçlı bir özniteliğin yinelenmesi, çoğu XML doğrulayıcısı ve
muhtemelen `icnmetadata.xsd` tarafından hata olarak işaretlenir.

**Kök neden:** `buildImfXml()`, `icnObjectIdent` değerini yalnızca
`r.hotspot` sayısından türetiyordu (`'hot'+String(r.hotspot).padStart(3,
'0')`), aynı hotspot numarasına sahip birden fazla kayıt olduğunda
tekilliği hiç kontrol etmiyordu.

**Uygulanan düzeltme:** Aynı `icnObjectIdent` daha önce kullanılmışsa,
sıradaki kayda harf soneki ekleniyor (`hot174` → ikinci kayıt için
`hot174b`, üçüncü için `hot174c`, ...). `icnObjectName` (görünen etiket)
değişmedi — hâlâ sade hotspot numarasını (`"174"`) gösteriyor, yalnızca
dahili kimlik benzersizleştirildi.

**Test sonuçları:** GRUP J — aynı hotspot="174" değerine sahip iki kayıt +
farklı bir "175" kaydıyla test edildi: tüm `icnObjectIdent` değerlerinin
benzersiz olduğu, ilk kaydın `hot174`, ikincisinin `hot174b` aldığı, ve
`icnObjectName`'in her ikisinde de sade `"174"` kaldığı doğrulandı → PASS.

---

## D-014 — KRİTİK: <dmodule> kök elemanında xsi:noNamespaceSchemaLocation HİÇ YOKTU — FORSDOC'un "Katmana veri aktarılırken hata" (S1000D.Error:00183) hatasının asıl nedeni buydu (ÇÖZÜLDÜ ✅ — kullanıcının önceden BAŞARIYLA yüklediği gerçek dosyalarla doğrudan karşılaştırılarak doğrulandı)

**Durum:** D-012 düzeltmesi (systemCode 2 karaktere zorlama) sonrası kullanıcı
AYNI FORSDOC hatasını tekrar aldı. Bunun üzerine kullanıcıdan **daha önce
FORSDOC'a BAŞARIYLA yüklediği gerçek dosyaları** istendi (11 dosyalık bir
ZIP) ve bizim uygulamamızın ürettiği dosyalarla **doğrudan bayt/yapı
karşılaştırması** yapıldı. Bu karşılaştırma kesin kanıt sağladı.

**Bulunan kesin fark:** Kullanıcının başarıyla yüklediği TÜM dosyaların
`<dmodule>` kök elemanında şu vardı:
```xml
<dmodule xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ...
         xsi:noNamespaceSchemaLocation="http://www.s1000d.org/S1000D_5-0/xml_schema_flat/descript.xsd">
```
(şema türüne göre `descript.xsd`/`proced.xsd`/`ipd.xsd` değişiyor, fault
örneği bu partide yoktu ama aynı örüntü beklenir). **Bizim uygulamamızın
`HEADER` sabiti bu özniteliği HİÇ İÇERMİYORDU:**
```js
const HEADER = '<?xml version="1.0" encoding="UTF-8"?>\n<dmodule xmlns:xlink="...">\n';
```
Ne `xmlns:xsi`, ne `xsi:noNamespaceSchemaLocation` — hiçbiri yoktu.
FORSDOC muhtemelen veri modülünü hangi şema/"katmana" ait olduğunu bu
değerden belirliyor; değer tamamen yoksa hangi katmana yerleştireceğini
bilemiyor ve `"Katmana veri aktarılırken bir hata oluştu"` genel hatasını
veriyor. `validationErrors: null` olması da bunun bir XSD içerik hatası
değil, daha temel bir "bu dosya hangi şemaya ait, bilmiyorum" hatası
olduğuyla tutarlı.

**Ayrıca fark edilen (muhtemelen kritik olmayan ama not edilen):**
Başarılı dosyalarda her zaman (boş da olsa) bir `<!DOCTYPE dmodule []>`
satırı var; bizim üretimimizde ICN bağlı olmayan satırlarda hiç DOCTYPE
yoktu. Bu da eklendi (aşağıya bakınız).

**Uygulanan düzeltme:** `const HEADER = ...` sabiti kaldırıldı, yerine
şemaya duyarlı bir `dmHeader(schema)` fonksiyonu eklendi:
```js
function dmHeader(schema){
  const xsdMap = {descript:'descript.xsd', proced:'proced.xsd', fault:'fault.xsd', ipd:'ipd.xsd'};
  const xsd = xsdMap[schema] || 'descript.xsd';
  return '<?xml version="1.0" encoding="UTF-8"?>\n<!DOCTYPE dmodule []>\n' +
    '<dmodule xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xlink="http://www.w3.org/1999/xlink" ' +
    'xsi:noNamespaceSchemaLocation="http://www.s1000d.org/S1000D_5-0/xml_schema_flat/'+xsd+'">\n';
}
```
`genDescript()`, `genProced()`, `genFault()`, `genIpd()` artık sırasıyla
`dmHeader('descript')`, `dmHeader('proced')`, `dmHeader('fault')`,
`dmHeader('ipd')` çağırıyor (önceden hepsi aynı statik `HEADER`'ı
kullanıyordu). `generateXmlWithIcn()`/ICN entity enjeksiyonu da
güncellendi: artık her zaman var olan boş `<!DOCTYPE dmodule []>`
satırını (ikinci bir DOCTYPE eklemek yerine, ki bu geçersiz XML üretirdi)
ENTITY içeren DOCTYPE ile **değiştiriyor.**

**Test sonuçları (35/35 PASS):**
- GRUP L: `dmHeader()` her 4 şema için doğru XSD adresini ve
  `xmlns:xsi`'yi ürettiği doğrulandı.
- GRUP N/O: ICN bağlı olmadığında tam olarak 1 DOCTYPE (boş) üretildiği;
  ICN bağlı olduğunda hâlâ tam olarak 1 DOCTYPE (bu kez ENTITY içeren)
  üretildiği ve `xsi:noNamespaceSchemaLocation`'ın bu enjeksiyondan
  etkilenmediği doğrulandı.
- GRUP P: 040/420/520/941 regresyonu ve `dmcLengthError` bozulmadı.

**Neden hâlâ tam "KAPATILDI" değil:** Bu düzeltme, kullanıcının FORSDOC'a
GERÇEKTEN BAŞARIYLA yüklediği dosyalarla doğrudan karşılaştırmaya dayanıyor
— bu, D-012'nin dayandığı "genel S1000D örnekleri"nden çok daha güçlü bir
kanıt standardı. Ancak yine de **kullanıcının bu düzeltmeyle üretilmiş yeni
bir ZIP'i FORSDOC'a tekrar import edip sonucu bildirmesi gerekiyor** —
başarılı dosyalarda bulunan `exportControl`, `restrictionInfo/copyright`,
`systemBreakdownCode`, `skillLevel`, `reasonForUpdate` gibi bazı ek/opsiyonel
elemanlar bizim üretimimizde hâlâ yok; bunların FORSDOC için zorunlu olup
olmadığı bilinmiyor (DOĞRULANAMADI). Eğer bu düzeltmeden sonra FORSDOC hâlâ
aynı hatayı verirse, bir sonraki şüpheli bu eksik elemanlar olmalı.

---

## D-012 — [GERİ ALINDI ⚠️] systemCode'u 2 karaktere zorlama girişimi — YANLIŞ ÇIKTI

**Durum:** Bu düzeltme bir önceki turda uygulanmış, ancak kullanıcının
FORSDOC'a **daha önce BAŞARIYLA yüklediği gerçek dosyalar** incelendiğinde
`systemCode="GA1"` (3 karakter) değerinin FORSDOC tarafından sorunsuz kabul
edildiği görüldü. Yani "dmCode/@systemCode her zaman 2 karakter olmalı"
varsayımı bu proje/FORSDOC kurulumu için **yanlıştı** — genel/dış S1000D
örneklerine dayanıyordu, bu projenin kendi FORSDOC yapılandırmasına değil.

**Geri alınan değişiklik:** `identAndStatus()` içinde `sys =
parts[2].slice(-2)` → tekrar `sys = parts[2]` (ham segment); `genIpd()`
içinde `row.dmc.split('-')[2].slice(-2)` → tekrar `row.dmc.split('-')[2]`.

**Ders:** Bir alanın "doğru" formatının ne olduğuna dair varsayımlar
(resmi/genel S1000D dokümantasyonuna dayansa bile), o projenin **gerçek,
kabul edilmiş** çıktılarıyla doğrulanmadan koda uygulanmamalı. Bu oturumda
bu ders D-011/D-014 sürecinde uygulanan yöntemle (gerçek başarılı/başarısız
dosyaları doğrudan karşılaştırma) somutlaştırıldı.

---

## D-011 — KRİTİK: detay-sütun importta bilesen/sokum/altAltSis/mic/sdc/kkk, Excel'in kırpılmış sütun değerleriyle eziliyordu — 7906 satırın tamamının yanlışlıkla reddedilmesine neden oldu (ÇÖZÜLDÜ ✅ — kod tarafı; XSD çapraz doğrulaması bekliyor)

**Durum:** Bu oturumda **gerçek kodda doğrulanmış ve düzeltilmiş** bir kritik
hatadır. Önceki oturumun devir notlarında (`SESSION_HANDOVER.md`,
`PROJECT_STATUS.md`) bu hatanın zaten düzeltildiği iddia ediliyordu, ancak
bu oturumda **gerçek dosya satır satır incelendiğinde hatanın hâlâ mevcut
olduğu doğrulandı** — yani önceki iddia yanlıştı veya düzeltme kaybolmuştu.
Bu oturumda hata yeniden tespit edilip gerçek dosyada düzeltildi.

**Kullanıcının bildirdiği gerçek sonuç:** Gerçek FORSDOC CSV importunda
7906 satırın **tamamı** "proje uzunluk kuralı" (`dmcLengthError`) nedeniyle
yanlışlıkla reddedildi.

**Kök neden (D-009 ile birebir aynı hata deseni, farklı alanlarda):**
`importText()`'in detay-sütun import yolunda:
```js
bilesen: get('bilesen') || p.bilesen,
sokum: get('sokum') || p.sokum,
altAltSis: get('altAltSis') || p.altAltSis,
mic: get('mic') || p.mic, sdc: get('sdc') || p.sdc,
kkk: get('kkk') || p.kkk,
```
Excel'de bu sütunlar sayısal hücre biçimlendirmesi nedeniyle baştaki
sıfırlarını kaybedebiliyor (ör. DMC'de doğru olan `bilesen="0000"` Excel
hücresinde `"0"` olarak görünüyor; `sokum="00AA"` da benzer şekilde
bozulabiliyor). `"0"` boş olmayan (truthy) bir string olduğundan,
`get('bilesen') || p.bilesen` ifadesi DMC'den regex ile güvenilir şekilde
türetilen doğru `p.bilesen` değeri yerine Excel'in **hatalı/kırpılmış**
`"0"` değerini seçiyordu. Bu durum:
1. `dmcLengthError(raw)` içinde `raw.bilesen.length !== 4` kontrolüne takılıp
   **geçerli DMC'ye sahip satırların tamamen reddedilmesine** yol açıyordu
   (kullanıcının bildirdiği 7906/7906 satır reddi tam olarak budur).
2. Reddedilmeyip kabul edilen (ör. dmc-only yolunda hiç sorun yoktu, ama
   detay-sütun yolunda uzunluk kontrolünü aşan satırlarda) `genIpd()`
   içinde `assyCode="'+xmlEscape(row.bilesen)+'"` ve
   `subSystemCode`/`subSubSystemCode="'+xmlEscape(row.altAltSis...)+'"`
   olarak **yanlış** değerler; `identAndStatus()` içinde
   `modelIdentCode="'+xmlEscape(row.mic)+'"`,
   `systemDiffCode="'+xmlEscape(row.sdc)+'"`,
   `itemLocationCode="'+xmlEscape(row.kkk)+'"` olarak da **yanlış** değerler
   üretilebiliyordu (Excel'in ilgili sütunu DMC ile tutarsızsa).

**Doğrulama testi (bu oturumda, gerçek dosyadan çıkarılan kodla):**
7906 satırlık, DMC'leri her zaman geçerli ama Excel'in
`Bilesen Kodu`/`Sokum Kodu ve Turevi`/`Alt ve Alt-Alt Sistem Kodu`
sütunlarının `"0"` olarak kırpıldığı gerçekçi bir CSV ile:
- **Düzeltme ÖNCESİ (orijinal kod, aynı veriyle tekrar test edildi):**
  0/7906 satır kabul edildi, 7906/7906 satır `assyCode ("0") 4 karakter
  olmalı` hatasıyla reddedildi. Kullanıcının bildirdiği hata **birebir
  yeniden üretildi.**
- **Düzeltme SONRASI:** 7906/7906 satır kabul edildi, 0 satır uzunluk
  kuralı nedeniyle reddedildi.

**Uygulanan düzeltme (gerçek dosyada, yalnızca detay-sütun import
yolundaki `raw` nesnesi oluşturma bloğunda, minimal):**
D-009'da `infoCode` için uygulanan ilke (*tam geçerli DMC mevcutken, konum/
kod alanları için TEK güvenilir kaynak her zaman `parseDmcString(dmc)`'dir;
Excel'deki karşılık gelen sütun yalnızca uyuşmazlık durumunda kullanıcıyı
uyarmak için okunur, XML üretiminde/uzunluk doğrulamasında ASLA
kullanılmaz*) şimdi aynı şekilde şu alanlara da uygulandı: `mic`, `sdc`,
`altAltSis`, `bilesen`, `sokum`, `kkk`, `bilgiTurevi`.
```js
// ÖNCE:
bilesen: get('bilesen') || p.bilesen,
sokum: get('sokum') || p.sokum,
// SONRA:
bilesen: p.bilesen,
sokum: p.sokum,
```
(aynı desen `altAltSis`, `mic`, `sdc`, `kkk`, `bilgiTurevi` için de
uygulandı). Excel'in ilgili sütunları hâlâ okunuyor, ancak yalnızca
`dmcFieldMismatchCount`/`dmcFieldMismatchSamples` adlı yeni bir uyuşmazlık
sayacına/örnek listesine besleniyor ve içe aktarma durum mesajında
kullanıcıya bilgi olarak gösteriliyor — satır asla bu yüzden atlanmıyor,
XML üretimini asla etkilemiyor.

**Kapsam dışı bırakılan alanlar (bilinçli olarak dokunulmadı):**
`malzKat` ve `sysCode` alanları da aynı `get('x') || p.x` desenine sahip,
ancak kod genelinde hiçbir yerde (`identAndStatus()`, `genIpd()`)
doğrudan kullanılmadıkları doğrulandı — XML üretimi her zaman
`row.dmc.split('-')[2]` üzerinden `systemCode`'u türetiyor (bkz. D-001).
Bu nedenle bu iki alan, gereksiz değişiklik yapmamak için **olduğu gibi
bırakıldı**.

**Test sonuçları (bu oturumda, gerçek dosyadan çıkarılan kodla, 54/54
PASS, 0 FAIL):**
- TEST-A (5 farklı DMC × Excel'in bilesen/sokum/infoCode sütunları
  kırpılmış): tümü kabul edildi, `bilesen="0000"`, `sokum` 4 karakter,
  `infoCode` doğru 3 haneli değer → PASS
- TEST-B (dmc-only import yolu regresyonu): bozulmadı → PASS
- TEST-C (Excel değerleri DMC ile zaten tutarlıysa hiçbir uyarı
  üretilmemeli): PASS
- TEST-D (GERÇEKTEN geçersiz bir DMC — assyCode segmenti gerçekten 3
  karakter — hâlâ doğru şekilde reddediliyor mu): PASS
- TEST-E (uyuşmazlık sayaçları ve örnek mesajlar doğru üretiliyor mu):
  PASS
- TEST-F (941/ipd XML üretiminde assyCode/subSystemCode/itemLocationCode/
  modelIdentCode/infoCode artık DMC'den doğru geliyor mu, ayrıca 040/420/
  520/941 şema regresyonu): PASS
- ÖLÇEK TESTİ (gerçekçi 7906 satırlık FORSDOC benzeri CSV): düzeltme
  öncesi 0/7906, düzeltme sonrası 7906/7906 kabul → PASS

**Regresyon kontrolü:** D-001 (systemCode `row.dmc.split('-')[2]`
kaynaklı, değişmedi), D-009 (infoCode her zaman DMC'den, değişmedi/
regresyon yok), D-010 (`icnmetadata.xsd` küçük harf, değişmedi), BREX
(`SETTINGS.brex`, `brexDmRef`, hiçbiri değişmedi) — hepsi bu oturumda
tekrar doğrulandı.

**Neden hâlâ tam "KAPATILDI" değil:** Bu düzeltme dahili (Node.js,
gerçek dosyadan çıkarılan kodla) testlerle güçlü şekilde doğrulandı,
ancak T-007/T-011 ile aynı kategoride: resmi S1000D Issue 5.0 XSD
paketiyle `xmllint --schema` çapraz doğrulaması bu oturumda da
yapılamadı (paket sağlanmadı). Bu nedenle "BEKLEMEDE / XSD İLE
DOĞRULANACAK" kategorisinde tutulmalı — bkz. `TODO.md` T-013.

---

## D-001 — systemCode üretim tutarsızlığı (ÇÖZÜLDÜ ✅)

**Durum:** Çözüldü ve gerçek repo dosyasında doğrulandı. Kapatıldı. Bu
oturumda da regresyon kontrolü yapıldı, bozulmadı.

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
uzunluk kuralını, D-009'da düzeltilen infoCode hatasını ve bu oturumda
D-011 ile düzeltilen bilesen/sokum/altAltSis/mic/sdc/kkk hatasını
kapsamıyor — üçü de baseline'dan sonra eklendi/düzeltildi ve gerçek XSD
paketiyle henüz teyit edilmedi.

**BREX tarafı:** D-006 ile ayrıntılandırıldı — **BREX çalışması kullanıcı
tarafından durduruldu** (bkz. D-006 güncel not). BREX validator geliştirme
işi, S1000D Default BREX araştırması ve BREX referans değişiklikleri artık
geliştirme önceliği değil; MSB/proje girdisi gelene kadar **BEKLEMEDE**.
Bu oturumda BREX konusuna hiç dönülmedi.

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

**Bu oturumda önemli ek not (D-011 ile ilişkili):** Bu kuralın kendisi
(4/4 karakter) doğruydu ve değiştirilmedi — ancak `dmcLengthError()`'a
verilen `raw.bilesen`/`raw.sokum` girdileri detay-sütun import yolunda
Excel'in kırpılmış sütun değerleriyle bozulmuş olabiliyordu (bkz. D-011).
Bu oturumda o girdi kaynağı düzeltildi; `dmcLengthError()` fonksiyonunun
kendisi değişmedi.

**Test sonuçları:** 13/13 PASS (TEST-LEN-01→11 + 2 ek test, önceki
oturumdan). Bu oturumda D-011 testleri (TEST-A/D) ile bu kuralın artık
her iki import yolunda da DMC'den güvenilir şekilde türetilmiş girdilerle
çalıştığı ayrıca doğrulandı.

**Neden hâlâ açık:** 4/2 karakter kuralının resmi S1000D XSD
karakter/datatype kısıtlarıyla çelişmediği yalnızca genel S1000D bilgisine
dayanarak makul kabul edildi — resmi XSD ile çapraz doğrulanmadı. Madde,
resmi XSD paketi tekrar sağlanıp bu değişiklikle üretilen örnek çıktılar
`xmllint --schema` ile test edilene kadar **"KAPATILDI" olarak
işaretlenemez.**

---

## D-006 — BREX yaklaşımı: S1000D Default BREX artık proje iş kurallarının ana kaynağı değil (ÇALIŞMA DURDURULDU ⏸️)

**Durum:** Kullanıcı tarafından durdurulmuştu; bu oturumda da BREX
konusuna hiç dönülmedi, `SETTINGS.brex`/`brexDmRef` değişmedi (bkz. yukarı
D-011 regresyon kontrolü).

**Önceki karar (hâlâ geçerli, ancak artık aktif geliştirme önceliği
değil):** DERMAN projesinde S1000D Default BREX artık proje iş
kurallarının ana kaynağı olarak kullanılmayacak. Proje iş kuralları için
öncelik sırası: MSB katmanı → MPG/FORSDOC girdileri → doğrulanmış proje
kararları → S1000D XSD yapısı.

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

**Durum:** Kod gerçek dosyada düzeltildi ve dahili testlerle doğrulandı,
bu oturumda regresyon kontrolüyle tekrar doğrulandı, değişmedi.
(Ayrıntılar önceki oturum kaydıyla aynı — bkz. önceki sürüm. Bu oturumda
aynı ilke, D-011 ile mic/sdc/altAltSis/bilesen/sokum/kkk alanlarına da
genişletildi.)

---

## D-010 — buildImfXml() içinde schemaLocation dosya adı düzeltmesi: icnMetadata.xsd → icnmetadata.xsd

**Durum:** Kod gerçek dosyada düzeltildi (önceki oturum). Bu oturumda
değişmedi, regresyon kontrolüyle (`grep icnmetadata.xsd`) doğrulandı.
Hâlâ yalnızca kullanıcı beyanına dayanıyor — resmi XSD paketiyle bağımsız
teyit edilmedi (bkz. `TODO.md` T-012).
