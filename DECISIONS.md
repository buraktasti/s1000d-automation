# DECISIONS.md — Kararlar ve Doğrulanmış Teknik Riskler

Bu dosya, proje hakkında verilmiş kararları ve **doğrulanmış** (varsayıma
dayanmayan) teknik riskleri kayıt altına alır. Doğrulanamayan konular buraya
risk olarak yazılmaz; bkz. `PROJECT_STATUS.md` → "Doğrulanmamış / İncelenmesi
Gereken Noktalar".

---

## D-021 — fault şeması için figure/hotspot/legend gömme desteği eklendi (UYGULANDI ✅ — dahili testlerle doğrulandı; FORSDOC'ta yeniden import testi bekliyor)

**Durum:** `injectIcnIntoXml()` önceden yalnızca `descript`, `proced`, `ipd`
şemalarını destekliyordu; `fault` şeması hiç desteklenmiyordu (kod içinde
`// fault ve digerlerinde v1'de gomulu figure desteklenmiyor` notuyla
bilinçli olarak atlanıyordu). D-020 ile "yalnızca RPK/IPD hariç TÜM şema
türlerinde Resim Açıklaması olacak" kararı netleşince bu eksiklik kapatıldı.

**Uygulanan düzeltme:** `genFault()`'un ürettiği 5 farklı içerik kalıbının
(411/412/413/410/varsayılan `faultIsolation`) hepsinde bir
`<faultDescr>...</faultDescr>` bloğu bulunduğu doğrulandı; figür bu bloğun
kapanış etiketinden hemen önce ekleniyor. Tek istisna **414
(`correlatedFault`)** — bu kalıpta `<faultDescr>` yok, figür bu durumda
`</correlatedFault>`'un içine ekleniyor.

**Test sonuçları:** İzole Node.js testinde 411/412/414 kalıplarının hepsinde
figürün doğru konuma eklendiği ve `<legend>` üretildiği doğrulandı
(`node --check` ile ayrıca tüm dosyanın sözdizimi doğrulandı).

**Neden hâlâ tam "KAPATILDI" değil:** Bu oturumda **hiçbir gerçek FORSDOC
import testi yapılmadı** — yalnızca Node.js'te izole fonksiyon testi.
Kullanıcının bu düzeltmeyle üretilmiş bir `fault` (410/411/412/413/414)
veri modülünü FORSDOC'a yükleyip figür+legend'in doğru göründüğünü
doğrulaması gerekiyor.

---

## D-020 — Resim Açıklaması (hotspot+legend) kuralı şema-bazlı hale getirildi: yalnızca IPD/RPK'da yok, diğer TÜM (mevcut) şema türlerinde var (UYGULANDI ✅ — kullanıcı kararıyla netleşti)

**Durum:** D-019'da eklenen ICN-bazlı manuel onay kutusu (`icn.includeLegend`)
yetersiz kaldı çünkü karar aslında **ICN bazlı değil, şema türü bazlı**ydı.
Kullanıcı, resmi S1000D BIKE örneğinin IPD (941) veri modülünde hiç
hotspot/legend gömülü olmadığını fark etti (kanıt: BIKE örneğinin
`<figure>`'ı yalnızca `<title>`+`<graphic>`, `<hotspot>`/`<legend>` yok),
oysa Koluman'ın kendi M0117 (Soğutma Donanımı) IPD çıktısında legend
basılıyordu. Kullanıcı önce "yalnızca RPK'da olmayacak" dedi, sonra ek bir
mesajda (yanlışlıkla) `ipd`'yi de "olacaklar" listesine dahil etti; açık
uçlu soruyla netleştirildi: **IPD hariç kalacak, `crew`/`schedul`/
`checklist`/`process`/`frontmatter`/`container`/`sb` gibi henüz üretim
fonksiyonu olmayan şema türleri ayrı bir işe (bkz. `TODO.md` T-018)
ertelendi.**

**Kesin kural (uygulanan hâliyle, `figureBlock()`):**
```js
const legendForbiddenBySchema = (schema==='ipd');
if (!hotspots.length || icn.includeLegend===false || legendForbiddenBySchema){
  // sade <graphic>, legend yok
}
```
- `schema==='ipd'` → legend **HER ZAMAN** yok, `icn.includeLegend` onay
  kutusundan bağımsız (zorlanmış kural — BIKE referansıyla doğrulandı).
- Şu an gerçekten üretilen diğer 3 şema (`descript`, `proced`, `fault`) →
  legend varsayılan olarak **var**; `descript`/`proced`'de kullanıcı
  ICN bazında onay kutusuyla kapatabilir (`icn.includeLegend===false`),
  `fault`'ta da artık aynı mekanizma geçerli (bkz. D-021).
- `crew`, `schedul`, `checklist`, `process`, `frontmatter`, `container`,
  `sb` → bu şema türleri için `SUPPORTED_SCHEMAS`'ta hiç üretim fonksiyonu
  yok; bu kural onlara **henüz uygulanamıyor** (bkz. `TODO.md` T-018,
  kullanıcı bu turda bu 7 türün ayrı bir iş olarak ele alınmasını
  onayladı).

**Test sonuçları:** İzole Node.js test matrisinde 6 kombinasyon (ipd+
varsayılan, ipd+onay-kutusu-açık [yine de zorlanmış kapalı], descript+
varsayılan, proced+varsayılan, descript+onay-kutusu-kapalı, schema
parametresiz geriye-dönük-uyum) hepsi beklenen sonucu verdi.

**Neden hâlâ tam "KAPATILDI" değil:** FORSDOC'ta gerçek import testi
yapılmadı; ayrıca 7 yeni şema türü backlog'da (T-018), o türler için bu
kural henüz geçerli değil.

---

## D-019 — [D-020 İLE GENİŞLETİLDİ, bkz. yukarı] ICN Oluşturucu'ya "Resim Açıklaması göm" onay kutusu eklendi

**Durum:** Bu, D-020'nin ilk (ve eksik kalan) versiyonuydu — yalnızca ICN
bazlı manuel bir onay kutusu (`icn.includeLegend`, `icn-legend-toggle`
UI elemanı) ekleniyordu, şema-bazlı otomatik kural yoktu. Kullanıcının
BIKE örneği karşılaştırmasıyla bunun yetersiz olduğu ortaya çıktı; D-020
ile şema-bazlı zorlama eklendi. UI elemanı ve `ICN_LIBRARY` kaydındaki
`includeLegend` alanı **değişmeden kaldı** — yalnızca `figureBlock()`'un
karar mantığı D-020 ile genişletildi.

---

## D-018 — genIpd() yalnızca üst montaj satırı üretiyordu; ICN'e bağlı hotspot/parça kayıtları parça kataloğu tablosuna hiç yansımıyordu (ÇÖZÜLDÜ ✅ — kod tarafı; FORSDOC'ta yeniden import testi bekliyor)

**Durum:** Kullanıcının paylaştığı gerçek FORSDOC referans üçlüsüyle
(resmi S1000D BIKE örneği: XML+FORIPS PDF+ICN ZIP) ve kullanıcının kendi
gerçek M0117 (Soğutma Donanımı) çıktısıyla (aynı üçlü) karşılaştırma
yapıldı.

**Kesin bulgu:** M0117 örneğinde Şekil'de **21 farklı hotspot/parça**
(30, 35, 40, ..., 300) olmasına rağmen, üretilen XML'de yalnızca **TEK bir
`catalogSeqNumber`** (item="000", üst montaj) vardı — kullanıcının kendi
FORIPS PDF çıktısında "Tablo 1" de bunu doğruladı (tek satır, hemen
ardından "Veri Modülü Sonu"). `genIpd(row)` kodu doğrudan incelendi:
`getIcnForRow(row.dmc)` hiç çağrılmıyordu, `icn.records` (CSV/OCR ile
zaten içe aktarılmış `partNo`/`partName`/`qty`/`manufacturer` bilgisi)
hiçbir yerde kullanılmıyordu.

**Uygulanan düzeltme:** Yeni `genIpdPartItemBlocks(row, icn)` fonksiyonu
eklendi — `icn.records` varsa, VM'nin kendi hotspot sıralamasıyla (artan
hotspot numarası) her kayıt için ayrı bir `catalogSeqNumber indenture="2"`
bloğu üretir (`partRef`, `quantityPerNextHigherAssy`, `descrForPart`
dahil; `item` numarası resmi BIKE örneğindeki gibi ardışık `001,002,...`).
`genIpd()` üst montaj satırını (`item="000"`) **değiştirmeden** korur,
yeni blokları bunun ardına ekler.

**Geriye dönük uyumluluk:** ICN bağlı değilse veya `icn.records` boşsa
`genIpdPartItemBlocks()` boş string döner — eski davranış (tek satır)
aynen korunuyor, regresyon yok.

**Test sonuçları:** İzole Node.js testinde 4 sahte hotspot kaydı (karışık
sırada verildi, biri yinelenen numara) doğru şekilde artan sırada ve doğru
alan eşlemeleriyle üretildi; boş-ICN durumunda `""` döndüğü ayrıca
doğrulandı.

**Neden hâlâ tam "KAPATILDI" değil:** FORSDOC'ta gerçek import testi
yapılmadı. Kullanıcının M0117 örneğini bu düzeltmeyle yeniden üretip
FORSDOC'a yükleyip FORIPS PDF'te Tablo 1'in artık 22 satır (1 üst montaj +
21 parça) gösterdiğini doğrulaması gerekiyor (bkz. `TODO.md` T-017).

---

## D-017 — Gerçek FORSDOC referans paketleriyle (resmi S1000D BIKE örneği + kullanıcının M0117 gerçek projesi) çapraz karşılaştırma; bir önceki oturumun kendi yanlış varsayımı düzeltildi

**Durum:** Kullanıcı üç dosyalık iki ayrı referans üçlüsü paylaştı: (1)
resmi S1000D BIKE örneği (XML + FORIPS PDF + ICN ZIP: PNG/CGM + IMF +
`HotspotMaterials-Malzemeler.json`), (2) kullanıcının kendi M0117 (Soğutma
Donanımı) projesinden Koluman ile üretilip **FORSDOC'a başarıyla
yüklenmiş** aynı yapıdaki üçlü.

**Bulgu 1 (D-018'e yol açtı):** IPD parça tablosu eksikliği — yukarı
bkz. D-018.

**Bulgu 2 (D-020'ye yol açtı):** BIKE örneğinin IPD veri modülünde hiç
figure/legend gömülü değildi — yukarı bkz. D-020.

**Bulgu 3 (kendi kendini düzeltme — ÖNEMLİ metodolojik not):** İlk analizde
`HotspotMaterials-Malzemeler.json` dosyasının ve IMF'deki sırasız
`icnObject` listesinin Koluman'ın kendi ZIP çıktısı olduğu **yanlışlıkla**
varsayıldı. Bu, gerçek `.html` kaynak kodunda `grep -n "Malzemeler\|
HotspotMaterials"` çalıştırılarak **çürütüldü**: bu string dosyada hiç
geçmiyor. `downloadVmZip()` ve `icn-zip-btn` handler'ı yalnızca `.PNG` +
`IMF-...XML` + `DMC-...XML` üretiyor. Sonuç: **bu JSON dosyası ve IMF'nin
sırasız hâli, FORSDOC'un ZIP'i import ettikten SONRA kendi CSDB'sinden
yaptığı export'a ait** — Koluman'ın ürettiği ham ZIP'te hotspot'lar zaten
`computeIcnHotspotObjects()`'in sıralı çıktısıyla sıralı üretiliyor
(D-016). Bu bulgu kullanıcıya açıkça düzeltilerek bildirildi.

**Ders (projenin kendi metodolojisiyle tutarlı):** Bir önceki turun/mesajın
kendi iddiası bile, koda bakılmadan doğru kabul edilmemeli — bu oturumda
Claude'un kendi ilk varsayımı, gerçek kaynak kod `grep`'lenerek test edildi
ve yanlış olduğu görülünce düzeltildi.

---

## D-016 — FAZ 2: MIC bazlı (Model Tanımlama Kodlu) ICN üretimi + VM hotspot/legend gömme + SNS hiyerarşik eşleştirme (UYGULANDI ✅ — gerçek FORSDOC verisiyle test edildi; FORSDOC'ta yeniden import testi bekliyor)

**Durum:** İki ayrı gerçek FORSDOC export paketi (Temel Motor/GA1-18-0000 ve
Egzoz Sistemi/GA1-14-0000) ve kullanıcının paylaştığı resmi "Bilgi Kontrol
Numarası" eğitim materyaliyle çapraz doğrulanan analiz sonucunda uygulandı.

**1) MIC bazlı ICN kod formülü (GÜÇLÜ BULGU → uygulandı):**
```
ICN-{MIC}-{SDC}-{systemCode+subSystemCode+subSubSystemCode+assyCode}-A-{CAGE}-{sıra:5hane}-A-001-01
```
Gerçek Egzoz Sistemi çiftiyle (`M0117-AAA-GA1-14-0000-00AA-040A-A` →
`ICN-M0117-AAA-GA1140000-A-TH743-00002-A-001-01`) birebir doğrulandı.
"Sorumlu firma kodu" ve "varyant" alanları için DERMAN'da gözlemlenen sabit
`"A"` değeri kullanıldı — bu değerin DERMAN için neden "A" olduğu hâlâ
**DOĞRULANAMADI** (proje kararı/kullanıcı onayına dayanıyor, teknik bir
zorunluluk değil).

**Uygulanan fonksiyonlar (yeni):**
- `snsKeysFromRow(row)` — DMC satırından `systemCode/subSystemCode/
  subSubSystemCode/assyCode` ve hesaplanmış `systemKey/subSystemKey/
  subSubSystemKey/componentKey` üretir.
- `nextIcnSerial()`/`extractIcnSerial()` — Firma Kodlu VE MIC bazlı ICN'ler
  arasında **paylaşılan tek bir sıra numarası sayacı** (çakışmayı önlemek
  için; gerçek FORSDOC verisinde de seri numarasının kodlama yönteminden
  bağımsız, proje geneli tek bir sayaç olduğu görüldü).
- `nextIcnCodeMic(row)` — MIC bazlı ICN kodu üretir.
- `nextIcnCode()` — **DEĞİŞMEDİ (dışa dönük davranışı aynı)**, yalnızca
  içeride artık paylaşılan `nextIcnSerial()`'ı kullanıyor.

**UI:** ICN oluşturma ekranına "Firma Kodlu" / "Model Tanımlama Kodlu" radyo
seçimi eklendi (varsayılan: Firma Kodlu, yani **mevcut davranış hiç
değişmedi** — kullanıcı açıkça MIC bazlı'yı seçmedikçe eskisi gibi çalışır).
"İkisi Birden" proje kuralı gereği her iki yöntem de aynı anda destekleniyor.

**2) VM içinde hotspot/legend eksikliği (KRİTİK, EKSİK → uygulandı):**
Gerçek FORSDOC export'unda VM'nin `<graphic>` elemanı içinde `<hotspot>`
çocukları ve ardından bir `<legend><definitionList>` bloğu bulunduğu, ama
uygulamamızın yalnızca self-closing `<graphic />` ürettiği daha önce
doğrulanmıştı (bkz. önceki Faz 1 analizi). Bu oturumda düzeltildi:

- `computeIcnHotspotObjects(icn)` — hotspot listesini (ident ataması,
  D-013 yinelenen-numara düzeltmesi, sıralama) **tek bir yerde** hesaplar;
  hem `buildImfXml()` (IMF) hem `figureBlock()` (VM) bu **aynı** fonksiyonu
  kullanır. Bu, "VM ve IMF hotspot verisi aynı kaynaktan gelmeli" kesin
  gereksinimini (gerçek FORSDOC verisiyle doğrulandı) sağlar.
- `figureBlock(icn, figId)` yeniden yazıldı: `icn.records` doluysa artık
  `<graphic>` içinde `<hotspot id=... applicationStructureIdent=...
  applicationStructureName=... hotspotTitle=... objectCoordinates=... />`
  elemanları ve ardından `<legend><definitionList>...</definitionList>
  </legend>` üretiyor — gerçek FORSDOC yapısıyla birebir aynı element/
  öznitelik adları.
- **Geriye dönük uyumluluk:** `icn.records` boşsa (hiç hotspot tanımlı
  değilse) **eski self-closing `<graphic />` davranışı korunuyor** —
  regresyon yok.

**3) SNS hiyerarşik eşleştirme (veri modeli + fonksiyon, uygulandı; UI
bağlama henüz yapılmadı):**
- `icnBelongsToSnsNode(icn, nodeFullPath)` — bir ICN'in verilen SNS
  düğümünün (veya üst düğümlerinden birinin) kapsamına girip girmediğini
  `componentKey` üzerinden hiyerarşik olarak (üst seçilince alt dahil)
  test eder. Bu davranış kullanıcı tarafından **"FORSDOC zorunlu kuralı
  değil, KOLUMAN uygulama kararı"** olarak netleştirildi.
- `ICN_LIBRARY` kayıtlarına `method, systemCode, subSystemCode,
  subSubSystemCode, assyCode, systemKey, subSystemKey, subSubSystemKey,
  componentKey` alanları eklendi; ICN kaydedilirken **ilk seçili hedef
  VM'den** türetiliyor. Birden fazla hedef VM farklı SNS bağlamlarındaysa
  kullanıcıya uyarı gösteriliyor (engellemeden).
- **Henüz yapılmadı (kapsam dışı bırakıldı):** SNS ağacında bir düğüm
  seçildiğinde `ICN_LIBRARY`'yi bu fonksiyonla görsel olarak filtreleyen
  bir "ICN kütüphanesi tarayıcısı" ekranı — böyle bir ekran hiç yoktu
  (Faz 1'de EKSİK olarak işaretlenmişti), spekülatif bir UI icat etmemek
  için bu oturumda eklenmedi; yalnızca alt yapı (veri + fonksiyon) hazır.

**SNS Sistem Kodu=00 çelişkisi ele alındı:** `snsKeysFromRow()` hiçbir
yerde "SNS Sistem Kodu: 00" proje sabitini `dmCode/@systemCode` yerine
KULLANMIYOR — her zaman gerçek DMC'nin kendi `systemCode` segmentini
(`row.dmc.split('-')[2]`) okuyor. Kullanıcının açıkça yasakladığı karışıklık
oluşmadı.

**Test sonuçları (52/52 PASS):**
- GRUP Q: `snsKeysFromRow()` ve `nextIcnCodeMic()` gerçek Egzoz Sistemi
  çiftiyle (`GA1140000`) birebir doğrulandı.
- GRUP R/S: Paylaşılan seri numarası sayacının Firma Kodlu ↔ MIC bazlı
  arasında çakışma üretmediği doğrulandı.
- GRUP T: `figureBlock()`'un gerçek Egzoz Sistemi hotspot verisiyle
  ürettiği VM çıktısı, gerçek FORSDOC dosyasıyla element/öznitelik
  düzeyinde karşılaştırıldı; VM hotspot ident listesi ile IMF icnObject
  ident listesinin **birebir aynı** olduğu doğrulandı.
- GRUP U: Hotspot'suz bir ICN için eski self-closing davranışın
  korunduğu (regresyon yok) doğrulandı.
- GRUP V: `icnBelongsToSnsNode()` hiyerarşik eşleştirme (bileşen → alt alt
  sistem → alt sistem → sistem, üst her seviyede eşleşiyor; farklı
  sistem/alt-alt-sistem eşleşmiyor) doğrulandı.
- GRUP W: D-001 (systemCode), D-009 (infoCode), D-010 (icnMetadata.xsd),
  D-011 (dmcLengthError), D-012 (systemCode ham segment), D-014
  (xsi:noNamespaceSchemaLocation), D-015 (imfIdentIcn ön eksiz) — hiçbiri
  bozulmadı.

**Neden hâlâ tam "KAPATILDI" değil:** Bu oturumda **hiçbir gerçek FORSDOC
import testi yapılmadı** (yalnızca Node.js'te dahili test + gerçek dosyayla
statik karşılaştırma). Kullanıcının bu düzeltmeyle üretilmiş yeni bir
VM+ICN+IMF paketini (hem Firma Kodlu hem MIC bazlı ICN ile) FORSDOC'a
import edip: (a) VM içindeki hotspot'ların artık göründüğünü, (b) MIC bazlı
ICN'in FORSDOC tarafından kabul edildiğini doğrulaması gerekiyor. Ayrıca
"A" placeholder'ının DERMAN için doğruluğu ve SNS hiyerarşik filtreleme
UI'ının nereye/nasıl ekleneceği kullanıcı kararı bekliyor (bkz. `TODO.md`
T-016).

---

## D-015 — KRİTİK: buildImfXml() içinde imfCode/@imfIdentIcn yanlışlıkla "ICN-" ön ekiyle üretiliyordu — ICN görseli geliyor ama etkin noktalar (hotspot) gelmiyordu (ÇÖZÜLDÜ ✅ — kullanıcının RPK Builder ile ürettiği, FORSDOC'ta etkin noktaları da başarıyla çalışan gerçek bir IMF dosyasıyla doğrudan karşılaştırılarak doğrulandı)

**Durum:** D-014 düzeltmesinden sonra kullanıcı VM ve ICN'i FORSDOC'a
başarıyla yükleyebildi, ancak **ICN'deki etkin noktalar (hotspot'lar)
görünmüyordu.** Kullanıcı, daha önce **RPK Builder Professional V16.3**
adlı başka bir araçla üretip FORSDOC'a etkin noktalarıyla birlikte
**başarıyla** yüklediği bir ICN/IMF çıktısını (`ICN-TB317-00024-001-01.PNG`
+ `IMF-TB317-00024-001-01_000-01.XML`) paylaştı; bu, bizim üretimimizle
doğrudan karşılaştırıldı.

**Bulunan kesin hata:** `buildImfXml()` fonksiyonu içinde:
```js
function buildImfXml(icn){
  const code = imfIdentFromIcn(icn);   // "ICN-" ön eki doğru şekilde kaldırılıyor
  ...
  '<imfCode imfIdentIcn="'+xmlEscape(icn.code)+'" />'   // AMA burada icn.code (ön ekli!) kullanılıyor, code DEĞİL
```
`code` değişkeni (ön eksiz, doğru kimlik) hesaplanıyor ama **hiç
kullanılmıyordu** — dosya adı için doğru şekilde kullanılıyordu
(`imfFileName()` zaten `imfIdentFromIcn(icn)` çağırıyor, o kısım hep
doğruydu), ama `imfCode/@imfIdentIcn` XML özniteliği için yanlışlıkla ham
`icn.code` (`"ICN-TH743-00001-001-01"`, ön ekli) yazılıyordu.

RPK Builder'ın çalışan referans dosyasında bu değer **ön eksiz**:
```xml
<imfCode imfIdentIcn="TB317-00024-001-01" />
```
(dosya adı `ICN-TB317-00024-001-01.PNG` olsa bile).

**Neden bu, "görsel geliyor ama hotspot gelmiyor" belirtisini açıklıyor:**
ICN görselinin kendisi, veri modülündeki DOCTYPE/ENTITY + `<graphic
infoEntityIdent="ICN-...">` bağlantısı üzerinden gösteriliyor — bu
zaten doğruydu ve etkilenmedi. Ama etkin nokta (hotspot) verisi ayrı bir
dosyada (IMF) taşınıyor ve FORSDOC muhtemelen bu IMF'yi doğru ICN'e
`imfIdentIcn` değeri üzerinden eşliyor. Değer yanlış/tutarsız (ön ekli)
olunca, FORSDOC görseli göstermeye devam ediyor ama hotspot katmanını
hangi görsele ait olduğunu bulamadığı için sessizce atlıyor.

**Uygulanan düzeltme (tek satır, minimal):**
```js
'<imfCode imfIdentIcn="'+xmlEscape(icn.code)+'" />'
```
→
```js
'<imfCode imfIdentIcn="'+xmlEscape(code)+'" />'
```
(`code` değişkeni zaten fonksiyonun en başında doğru şekilde hesaplanmıştı,
sadece kullanılmıyordu.)

**Ayrıca D-010 GERİ ALINDI:** Aynı RPK referans dosyasında
`xsi:noNamespaceSchemaLocation` **büyük M ile** `icnMetadata.xsd`
kullanıyordu — önceki oturumda (D-010) bu, doğrulanmamış bir kullanıcı
beyanına dayanarak küçük harfe (`icnmetadata.xsd`) çevrilmişti. Bu artık
gerçek, FORSDOC'ta çalışan bir dosyayla çelişiyor, bu yüzden büyük M'ye
geri döndürüldü.

**Test sonuçları:** Node.js'te gerçek dosyadan çıkarılan `buildImfXml()`
ile: `imfIdentIcn` artık `"TH743-00001-001-01"` (ön eksiz) üretiyor,
`"ICN-"` ön ekli değer bir daha görünmüyor, `icnMetadata.xsd` büyük M ile
üretiliyor, dosya adı (`imfFileName()`) değişmedi → hepsi PASS.

**Neden hâlâ tam "KAPATILDI" değil:** Bu düzeltme çok güçlü bir kanıta
(gerçek, hotspot'ları çalışan bir referans dosyayla doğrudan karşılaştırma)
dayanıyor, ancak **kullanıcının bu düzeltmeyle üretilmiş yeni bir ICN/IMF
paketini FORSDOC'a tekrar yükleyip etkin noktaların gerçekten göründüğünü
doğrulaması gerekiyor.** `TODO.md` T-015 olarak izleniyor.

**Kapsam dışı bırakılan (dokunulmadı):** RPK'nın yinelenen hotspot'lar
için kullandığı `hot174a`/`hot174b` (harf soneki İLK yinelenende de)
adlandırma biçimi, bizim `hot174`/`hot174b` (ilkinde sonek yok)
biçiminden farklı — ancak bu yalnızca bir kimlik biçimi tercihi, benzersizlik
açısından ikisi de geçerli ve bu, hotspot'ların hiç görünmemesi belirtisini
açıklamıyor. Bu nedenle dokunulmadı; yalnızca bir gözlem notu olarak kayda
geçirildi.

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

## D-010 — [GERİ ALINDI ⚠️, bkz. D-015] buildImfXml() içinde schemaLocation dosya adı — icnMetadata.xsd → icnmetadata.xsd

**Durum:** Bu düzeltme (küçük harfe çevirme) yalnızca doğrulanmamış bir
kullanıcı beyanına dayanıyordu. Bu oturumda, kullanıcının **RPK Builder**
ile üretip FORSDOC'a etkin noktalarıyla birlikte gerçekten başarıyla
yüklediği bir referans IMF dosyası incelendi ve bu dosyanın **büyük M ile**
(`icnMetadata.xsd`) kullandığı görüldü. Bu, D-010'un yanlış olduğunu
gösterdi; büyük M'ye geri döndürüldü. Ayrıntı: `DECISIONS.md` D-015.
