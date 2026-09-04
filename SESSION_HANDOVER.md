# SESSION_HANDOVER.md — Oturum Devir Teslimi

## ALTINCI TUR (yeni bir konuşmada devam — gerçek FORSDOC referans üçlüleriyle karşılaştırma, IPD tablo eksikliği ve şema-bazlı legend kuralı bulundu)

1. Kullanıcı, resmi S1000D BIKE örneğinin üç dosyasını (XML, FORIPS PDF,
   ICN ZIP: PNG/CGM+IMF+`HotspotMaterials-Malzemeler.json`) paylaştı, sonra
   kendi M0117 (Soğutma Donanımı) projesinden Koluman ile üretip **FORSDOC'a
   başarıyla yüklediği** aynı yapıdaki üçlüyü paylaştı.
2. Karşılaştırmada M0117'nin FORIPS PDF'inde "Tablo 1"in yalnızca **tek
   satır** (üst montaj) gösterdiği, oysa Şekil'de **21 hotspot/parça**
   olduğu görüldü. `genIpd()` kodu doğrudan incelendi: `getIcnForRow()`
   hiç çağrılmıyordu, `icn.records` kullanılmıyordu.
3. **D-018 uygulandı:** Yeni `genIpdPartItemBlocks(row, icn)` fonksiyonu,
   `icn.records`'ı (VM'nin kendi sıralı hotspot listesiyle aynı sırada)
   ayrı `catalogSeqNumber indenture="2"` bloklarına dönüştürüyor. Üst
   montaj satırı (`item="000"`) değişmeden korundu; ICN bağlı değilse eski
   davranış (tek satır) aynen sürüyor.
4. Kullanıcı, BIKE örneğinin FORIPS PDF'inde görselin altında "Resim
   Açıklaması" olmadığını, ama M0117'ninkinde olduğunu fark etti. BIKE'ın
   IPD XML'i incelendi: `<figure>` yalnızca `<title>`+`<graphic>`,
   `<hotspot>`/`<legend>` hiç yok.
5. **D-019 uygulandı (ilk versiyon, sonra genişletildi):** ICN Oluşturucu
   kartına "Resim Açıklaması göm" onay kutusu (`icn.includeLegend`)
   eklendi, `figureBlock()` bunu kontrol edecek şekilde güncellendi.
6. Kullanıcı bunun yetersiz olduğunu belirtti: karar ICN bazlı değil,
   **şema türü bazlı** olmalı — "yalnızca RPK'da olmayacak, diğerlerinde
   olacak". Sonra bir mesajda yanlışlıkla `ipd`'yi de "olacaklar" listesine
   dahil etti ve ayrıca `crew`/`schedul`/`checklist`/`process`/
   `frontmatter`/`container`/`sb` gibi henüz üretim fonksiyonu olmayan
   şema türlerini de listeledi.
7. **Çelişki netleştirildi (`ask_user_input_v0` ile):** İki soru
   soruldu — (a) IPD'de legend olacak mı? → **Hayır, IPD hariç kalacak**
   (listeye yanlışlıkla girmiş); (b) 7 yeni şema türü için nasıl
   ilerlenecek? → **Şimdilik yalnızca mevcut 4 şemada kural kesinleştirilsin,
   yenileri ayrı bir iş (T-018) olarak backlog'a alınsın.**
8. **D-020 uygulandı:** `figureBlock(icn, figId, schema)` artık `schema`
   parametresi alıyor; `legendForbiddenBySchema = (schema==='ipd')` —
   yalnızca IPD'de legend zorla kapalı (onay kutusundan bağımsız), diğer
   tüm mevcut şemalarda (descript/proced varsayılan açık; fault de dahil)
   var. `injectIcnIntoXml()` artık `schema`'yı `figureBlock`'a iletiyor.
9. **D-021 uygulandı:** `injectIcnIntoXml()`'e `fault` şeması desteği
   eklendi — önceden hiç yoktu. `genFault()`'un 5 kalıbının 4'ünde
   (`<faultDescr>` olanlar: 411/412/413/410/varsayılan) figür
   `</faultDescr>`'dan önce, 414'te (`<faultDescr>` yok) `</correlatedFault>`
   içine ekleniyor.
10. **Kendi hatasını düzeltme (D-017):** Analiz sırasında
    `HotspotMaterials-Malzemeler.json`'ın ve IMF'nin sırasız hotspot
    listesinin Koluman'ın kendi çıktısı olduğu yanlışlıkla varsayılmıştı.
    Gerçek `.html` kaynağında `grep -n "Malzemeler\|HotspotMaterials"`
    çalıştırılarak bu string'in dosyada hiç geçmediği, dolayısıyla bu
    dosyaların FORSDOC'un kendi export'una ait olduğu tespit edildi ve
    kullanıcıya açıkça düzeltilerek bildirildi.

**Test:** Her adımda `node --check` ile sözdizimi doğrulandı; her yeni/
değişen fonksiyon (`genIpdPartItemBlocks`, `figureBlock` 6 senaryo
matrisi, `injectIcnIntoXml` fault kalıpları) gerçek `.html` dosyasından
programatik olarak çıkarılıp izole Node.js testleriyle doğrulandı. **Bu
oturumda hiçbir gerçek FORSDOC import testi yapılmadı.**

**Bilinçli olarak YAPILMAYAN:** 7 yeni şema türü (`crew`, `schedul`,
`checklist`, `process`, `frontmatter`, `container`, `sb`) için üretim
fonksiyonu yazımı — kullanıcı onayıyla ayrı bir işe (T-018) ertelendi.

**Bir sonraki adım: kullanıcının M0117 örneğini bu düzeltmelerle yeniden
üretip FORSDOC'a yükleyip (a) IPD Tablo'nun 22 satır gösterdiğini, (b)
IPD'de legend basılmadığını, (c) descript/proced'de regresyon olmadığını,
(d) bir fault örneğinde figure/legend'in doğru göründüğünü doğrulaması**
(bkz. `TODO.md` T-017).

---

## BEŞİNCİ TUR / FAZ 2 (yeni analiz turlarının ardından kod değişikliği uygulandı)

**Önceki 3 alt-tur (Faz 1 x3, kod değişikliği YAPILMADI, yalnızca analiz):**
1. Kullanıcı, gerçek 26 dosyalık bir FORSDOC export ZIP'i paylaştı (M0117
   projesi). Analiz: VM'nin `<graphic>` içinde gerçekte `<hotspot>` +
   `<legend>` bulunduğu, bizim üretimimizin bunları hiç üretmediği; ICN
   kodlamasının MIC bazlı da olabildiği (`ICN-M0117-AAA-00310000-A-...`)
   tespit edildi. MIC bazlı kod formülü tek örnekle kesinleştirilemedi.
2. Kullanıcı, ikinci ve farklı SNS bağlamına ait odaklı bir paket paylaştı
   (Egzoz Sistemi, `GA1-14-0000` / ICN `GA1140000`). Formül
   (`systemCode+subSystemCode+subSubSystemCode+assyCode`) bu temiz örnekle
   GÜÇLÜ BULGU seviyesinde doğrulandı; SNS→ICN filtreleme ve parent/child
   davranışı için KARAR GEREKLİ olarak işaretlendi.
3. Kullanıcı, resmi "Bilgi Kontrol Numarası" eğitim materyalini paylaştı.
   Bu, ICN kod segmentlerinin (sorumlu firma kodu, varyant, versiyon,
   gizlilik derecesi) kesin anlamlarını doğruladı. Ardından kullanıcı,
   proje-özel iş kurallarını (SNS filtreleme = hiyerarşik/üst dahil alt,
   ICN Metodu = İkisi Birden, SNS Sistem Kodu=00 dmCode/systemCode'un
   yerine geçmez, M0117 kesin MIC değeri) açıkça onayladı.

**Bu turda (FAZ 2) uygulanan kod değişiklikleri (D-016):**
1. `snsKeysFromRow(row)` — DMC'den systemCode/subSystemCode/
   subSubSystemCode/assyCode ve systemKey/subSystemKey/subSubSystemKey/
   componentKey türetir. `dmCode/@systemCode`'u ASLA "SNS Sistem Kodu: 00"
   proje sabitiyle karıştırmıyor — her zaman gerçek DMC segmentini okuyor.
2. `nextIcnSerial()`/`extractIcnSerial()` — Firma Kodlu ve MIC bazlı
   ICN'ler arasında PAYLAŞILAN tek sıra numarası sayacı (çakışma önleme).
3. `nextIcnCodeMic(row)` — MIC bazlı ICN kodu, gerçek Egzoz Sistemi
   örneğiyle birebir doğrulanan formülle.
4. `nextIcnCode()` — dışa dönük davranışı DEĞİŞMEDİ, içeride paylaşılan
   sayacı kullanıyor.
5. ICN oluşturma ekranına "Firma Kodlu" / "Model Tanımlama Kodlu" radyo
   seçimi eklendi (varsayılan: Firma Kodlu — mevcut davranış korunuyor).
6. `computeIcnHotspotObjects(icn)` — hotspot ident/sıra/D-013 yinelenen-
   numara mantığını TEK bir yerde hesaplar; hem `buildImfXml()` hem
   `figureBlock()` bunu kullanır (tek veri kaynağı garantisi).
7. `figureBlock(icn, figId)` **yeniden yazıldı** — artık `icn.records`
   doluysa VM'nin `<graphic>` içine `<hotspot>` elemanları + ardından
   `<legend><definitionList>` üretiyor (gerçek FORSDOC yapısıyla birebir
   element/öznitelik adları). `icn.records` boşsa eski self-closing
   `<graphic />` davranışı korunuyor (regresyon yok).
8. `icnBelongsToSnsNode(icn, nodeFullPath)` — hiyerarşik SNS eşleştirme
   (üst seçilince alt dallar da dahil, kullanıcının onayladığı kural).
9. `ICN_LIBRARY` kayıt yapısı genişletildi: `method, systemCode,
   subSystemCode, subSubSystemCode, assyCode, systemKey, subSystemKey,
   subSubSystemKey, componentKey` eklendi; kayıt sırasında ilk hedef
   VM'den türetiliyor, hedefler arası SNS uyuşmazlığında uyarı veriliyor.

**Test:** 52/52 yeni test PASS — gerçek Egzoz Sistemi verisiyle MIC kod
formülü, VM↔IMF hotspot senkronu, hiyerarşik SNS eşleştirme, ve D-001→D-015
regresyonlarının hiçbirinin bozulmadığı doğrulandı.

**Bilinçli olarak YAPILMAYAN:** SNS ağacında "ICN kütüphanesi tarayıcısı"
ekranı (spekülatif UI icat etmemek için) — yalnızca alt yapı hazır.

**Bir sonraki adım: kullanıcının bu düzeltmelerle üretilmiş yeni paketleri
FORSDOC'a tekrar yükleyip (a) hotspot'ların VM'de göründüğünü, (b) MIC
bazlı ICN'in kabul edildiğini doğrulaması** (bkz. `TODO.md` T-016).

---

## DÖRDÜNCÜ TUR (yeni bir konuşmada devam — VM+ICN başarıyla yüklendi, ama etkin noktalar gelmiyordu)

1. Kullanıcı, D-011→D-014 düzeltmeleriyle üretilmiş dosyaları FORSDOC'a
   yükledi: **VM ve ICN başarıyla geldi** (büyük ilerleme, D-014'ün gerçek
   kök nedeni doğru bulduğu teyit edildi). Ancak **ICN'deki etkin noktalar
   (hotspot'lar) görünmüyordu.**
2. Kullanıcı, daha önce **RPK Builder Professional V16.3 (Tesseract OCR)**
   adlı başka bir araçla üretip FORSDOC'a etkin noktalarıyla birlikte
   **başarıyla** yüklediği bir referans ICN/IMF çıktısı paylaştı
   (`ICN-TB317-00024-001-01.PNG` + `IMF-TB317-00024-001-01_000-01.XML`).
3. Bu referans IMF dosyası bizim `buildImfXml()` çıktımızla satır satır
   karşılaştırıldı. İki kesin fark bulundu:
   - **`imfCode/@imfIdentIcn`**: referans dosyada ön eksiz
     (`"TB317-00024-001-01"`), bizim üretimimizde yanlışlıkla ön ekli
     (`"ICN-TH743-00001-001-01"`). Kod incelendiğinde, ön eki doğru
     kaldıran `code` değişkeninin zaten hesaplandığı ama **hiç
     kullanılmadığı** görüldü — yerine ham `icn.code` yazılıyordu.
   - **`xsi:noNamespaceSchemaLocation`**: referans dosyada büyük M ile
     (`icnMetadata.xsd`); bizim üretimimizde (önceki oturumun D-010
     düzeltmesiyle) küçük harfle (`icnmetadata.xsd`).
4. **D-015 uygulandı:** `imfIdentIcn="'+xmlEscape(icn.code)+'"` →
   `imfIdentIcn="'+xmlEscape(code)+'"` (tek satır, zaten var olan doğru
   değişken kullanıldı).
5. **D-010 GERİ ALINDI:** `icnmetadata.xsd` → `icnMetadata.xsd` (büyük M,
   referans dosyayla eşleşecek şekilde).
6. **Test:** Gerçek dosyadan çıkarılan `buildImfXml()` ile doğrulandı:
   `imfIdentIcn` artık ön eksiz üretiliyor, `icnMetadata.xsd` büyük M,
   dosya adı (zaten doğruydu) değişmedi.
7. Güncellenmiş HTML ve 4 hafıza dosyası tekrar üretildi.

**Neden bu belirti (görsel geliyor, hotspot gelmiyor) mantıklı:** ICN
görseli, veri modülündeki DOCTYPE/ENTITY + `<graphic infoEntityIdent=
"ICN-...">` bağlantısı üzerinden gösteriliyor (bu zaten doğruydu,
etkilenmedi). Hotspot verisi ayrı bir dosyada (IMF) taşınıyor ve FORSDOC
muhtemelen bu IMF'yi doğru ICN'e `imfIdentIcn` üzerinden eşliyor — değer
tutarsız olunca görsel geliyor ama hotspot katmanı sessizce atlanıyor.

**Bir sonraki adım: kullanıcının bu düzeltmeyle üretilmiş yeni bir ICN/IMF
paketini FORSDOC'a tekrar yükleyip etkin noktaların gerçekten göründüğünü
doğrulaması** (bkz. `TODO.md` T-015).

---

## ÜÇÜNCÜ TUR (aynı oturumda — D-012 yanlış çıktı, gerçek kök neden D-014 ile bulundu)

1. Kullanıcı, D-012+D-013 içeren yeni bir ZIP ile FORSDOC'a tekrar denedi
   ve **birebir aynı hatayı** tekrar aldı: `{"code":"S1000D.Error:00183",
   "message":"Katmana veri aktarılırken bir hata oluştu.",
   "validationErrors":null}`.
2. Yüklenen yeni ZIP incelendi: `systemCode="A1"` (D-012 uygulanmış) ve
   `icnObjectIdent="hot174"/"hot174b"` (D-013 uygulanmış) — her iki
   düzeltme de dosyada doğru şekilde mevcuttu. Yani **bu iki düzeltme
   gerçek FORSDOC hatasının nedeni DEĞİLDİ.**
3. Kullanıcıdan, FORSDOC'a **daha önce gerçekten BAŞARIYLA yüklediği**
   dosyaları istendi. Kullanıcı 11 dosyalık gerçek bir ZIP paylaştı.
4. Bu 11 dosya incelenip bizim üretimimizle karşılaştırıldı. İki kesin
   bulgu ortaya çıktı:
   - **`systemCode="GA1"` (3 karakter) FORSDOC tarafından kabul
     edilmişti.** Bu, D-012'nin (systemCode'u 2 karaktere zorlama)
     **yanlış bir düzeltme olduğunu** kanıtladı — genel/dış S1000D
     örneklerine dayanıyordu, bu projenin FORSDOC yapılandırmasına değil.
   - **Başarılı dosyaların TÜMÜNDE** `<dmodule>` kök elemanında
     `xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"` VE
     `xsi:noNamespaceSchemaLocation="http://www.s1000d.org/S1000D_5-0/
     xml_schema_flat/{schema}.xsd"` vardı (şemaya göre descript.xsd/
     proced.xsd/ipd.xsd). **Bizim uygulamamızın `HEADER` sabiti bu
     özniteliği HİÇ İÇERMİYORDU.** Bu, FORSDOC'un "katmana yerleştirme"
     hatasının en güçlü açıklaması: FORSDOC muhtemelen dosyanın hangi
     şemaya/katmana ait olduğunu bu değerden anlıyor.
5. **D-012 GERİ ALINDI:** `identAndStatus()`/`genIpd()` içindeki
   `.slice(-2)` çağrıları kaldırıldı, DMC'nin ham 3. segmenti tekrar
   olduğu gibi kullanılıyor.
6. **D-014 uygulandı:** `const HEADER = ...` sabiti kaldırıldı, yerine
   şemaya duyarlı `dmHeader(schema)` fonksiyonu eklendi — her 4 şema
   türü için doğru `xsi:noNamespaceSchemaLocation` üretiyor. Ayrıca her
   zaman (boş de olsa) bir `<!DOCTYPE dmodule []>` satırı ekleniyor.
   ICN entity enjeksiyon mantığı, artık her zaman var olan bu boş
   DOCTYPE'ı ENTITY içerenle DEĞİŞTİRECEK şekilde güncellendi (çift
   DOCTYPE oluşmasın diye).
7. **Test (35/35 PASS):** `dmHeader()` her şema için doğru XSD adresini
   ürettiği; `systemCode="GA1"` artık değiştirilmediği; ICN bağlı/bağlı
   olmayan durumlarda her zaman TAM OLARAK 1 DOCTYPE üretildiği (önceki
   koddaki potansiyel çift-DOCTYPE riski de bu vesileyle önlendi); 040/
   420/520/941 regresyonu ve `dmcLengthError` (D-011) bozulmadığı
   doğrulandı.
8. Güncellenmiş HTML ve 4 hafıza dosyası tekrar üretildi.

**ÖNEMLİ ders:** D-012, "genel/resmi S1000D dokümantasyonuna dayanan
makul bir varsayım" idi ama bu projenin **gerçek, FORSDOC tarafından
kabul edilmiş** çıktılarıyla çelişiyordu. D-014 ise doğrudan
başarılı-vs-başarısız dosya karşılaştırmasına dayanıyor — çok daha güçlü
bir kanıt standardı. **Bir sonraki oturum/adım: kullanıcının bu düzeltmeyle
üretilmiş yeni bir ZIP'i FORSDOC'a (üçüncü kez) tekrar denemesi ve sonucu
bildirmesi gerekiyor** (bkz. `TODO.md` T-014). Eğer hata devam ederse,
başarılı referans dosyalarında olup bizim üretimimizde olmayan
`exportControl`/`restrictionInfo`/`systemBreakdownCode`/`skillLevel`/
`reasonForUpdate` elemanlarına bakılmalı.

---

## İKİNCİ TUR (aynı oturumda, kullanıcı gerçek bir ZIP çıktısını FORSDOC'a aktaramadığını bildirdi)

1. Kullanıcı, D-011 düzeltmesinden sonra uygulamayla ürettiği gerçek bir
   VM+ICN+IMF ZIP paketini (`ICN-TH743-00001-001-01_VM_ICN_IMF.zip`)
   yükledi ve FORSDOC'un şu hatayı verdiğini bildirdi:
   `{"code":"S1000D.Error:00183","message":"Katmana veri aktarılırken bir
   hata oluştu.","validationErrors":null}`.
2. ZIP açılıp içindeki 3 dosya (`DMC-...XML`, `IMF-...XML`,
   `ICN-...PNG`) tek tek incelendi: PNG geçerli, her iki XML da geçerli
   UTF-8 ve iyi biçimlendirilmiş (well-formed), ama iki somut anomali
   bulundu:
   - `dmCode/@systemCode="GA1"` (3 karakter) — resmi S1000D örnekleri
     (S1000D'nin kendi deposu dahil) her zaman 2 karakter gösteriyor.
   - IMF'de `icnObjectIdent="hot174"` iki kez görünüyordu (parça
     listesinde aynı "Konum" numarasına sahip iki farklı parça satırı
     yüzünden).
3. **D-012 düzeltmesi uygulandı:** `identAndStatus()` içinde
   `sys = parts[2]` → `sys = parts[2].slice(-2)`; `genIpd()` içinde
   `row.dmc.split('-')[2]` → `row.dmc.split('-')[2].slice(-2)`. Bu,
   segment 2 karakterse davranışı değiştirmiyor (`"00"`→`"00"`), segment
   3 karakterse düzeltiyor (`"GA1"`→`"A1"`).
4. **D-013 düzeltmesi uygulandı:** `buildImfXml()` içinde, aynı hotspot
   numarasına sahip birden fazla kayıt varsa, ikinci ve sonrakilere harf
   soneki ekleniyor (`hot174`, `hot174b`, ...), `icnObjectName` (görünen
   etiket) değişmedi.
5. **Test:** Kullanıcının gerçek DMC'siyle (`M0117-AAA-GA1-18-0000-00AA-
   040A-A`) ve gerçek yinelenen hotspot senaryosuyla (174, 174, 175)
   birebir eşleşen testler yazıldı — 24/24 PASS. Önceki oturumun 54
   testi de yeniden çalıştırıldı — hepsi hâlâ PASS (regresyon yok).
6. Güncellenmiş HTML ve 4 hafıza dosyası tekrar üretildi.

**ÖNEMLİ — Kesinlik notu:** FORSDOC `validationErrors: null` döndürdüğü
için hangi değişikliğin (D-012, D-013, ya da ikisi birlikte) FORSDOC
hatasının gerçek nedenini giderdiği **kanıtlanamadı, yalnızca güçlü
şekilde çıkarım yapıldı.** D-012 en güçlü adaydır çünkü (a) resmi S1000D
örnekleriyle doğrudan çelişiyordu, (b) kodun kendi iç tutarsızlığını
gösteriyordu (zaten hesaplanan ama kullanılmayan doğru alan vardı), ve
(c) "katman" (layer) terminolojisi projenin "Katman: S1000D 5.0 → MSB"
tanımıyla örtüşüyor. **Bir sonraki adım kesinlikle: kullanıcının
düzeltilmiş HTML ile yeni bir ZIP üretip FORSDOC'a tekrar import etmesi
ve sonucu bildirmesi** (bkz. `TODO.md` T-014).

---

## İLK TUR — Bu Oturumda Yapılanlar (7906 satırlık gerçek FORSDOC import hatasının kök nedeni bulundu ve düzeltildi, D-011)

1. Devir talimatındaki iddia doğrudan koda bakılarak test edildi: önceki
   oturumun "bilesen/sokum uzunluk hatası zaten `raw.dmc`'nin yeniden
   ayrıştırılmasına dayandırılarak düzeltildi" iddiası **gerçek dosyada
   doğrulanamadı** — kod incelendiğinde `bilesen: get('bilesen') ||
   p.bilesen` ve `sokum: get('sokum') || p.sokum` deseninin **hâlâ
   olduğu tespit edildi.** Varsayıma dayanmak yerine, gerçek dosyadan
   kod çıkarılıp Node.js'te doğrudan çalıştırılarak hem bu iddia
   çürütüldü hem de gerçek hatanın var olduğu kanıtlandı.
2. Hata, kullanıcının verdiği 5 örnek DMC ve gerçekçi 7906 satırlık bir
   FORSDOC benzeri CSV ile **yeniden üretildi**: düzeltme öncesi (orijinal
   kod, aynı veriyle) **0/7906 satır kabul edildi**, tamamı `assyCode
   ("0") 4 karakter olmalı` hatasıyla reddedildi — kullanıcının bildirdiği
   sorunla birebir örtüşüyor.
3. **Kök neden doğrulandı (D-011):** Detay-sütun import yolunda, tam DMC
   geçerli olduğu halde `bilesen`, `sokum`, `altAltSis`, `mic`, `sdc`,
   `kkk` alanları için `get('x') || p.x` deseni kullanılıyordu. Excel'in
   sayısal biçimlendirmesi bu sütunlardaki baştaki sıfırları kırptığında
   (`"0000"` → `"0"` gibi), kırpılmış ama boş olmayan (truthy) Excel
   değeri DMC'den doğru türetilen değerin önüne geçiyordu. Bu, D-009'da
   `infoCode` için daha önce düzeltilen hatayla **birebir aynı desendir**
   — yalnızca farklı alanlarda tekrar etmiş.
4. **Düzeltme uygulandı:** D-009'daki ilke (tam DMC geçerliyken konum/kod
   alanları için tek güvenilir kaynak her zaman `parseDmcString(dmc)`'dir)
   şimdi `mic`, `sdc`, `altAltSis`, `bilesen`, `sokum`, `kkk`,
   `bilgiTurevi` alanlarına da uygulandı. Excel'in ilgili sütunları hâlâ
   okunuyor ama yalnızca yeni bir uyuşmazlık sayacına
   (`dmcFieldMismatchCount`/`dmcFieldMismatchSamples`) besleniyor ve
   kullanıcıya bilgi amaçlı gösteriliyor; satır asla bu yüzden atlanmıyor.
   `malzKat`/`sysCode` alanları bilinçli olarak **dokunulmadan bırakıldı**
   çünkü XML üretiminde hiçbir yerde doğrudan kullanılmadıkları
   doğrulandı (systemCode her zaman `row.dmc.split('-')[2]`'den geliyor,
   D-001).
5. **Düzeltme doğrulandı:** Aynı 7906 satırlık test verisiyle düzeltme
   sonrası **7906/7906 satır kabul edildi**, 0 satır uzunluk kuralı
   nedeniyle reddedildi.
6. **Kapsamlı regresyon testi (54/54 PASS, 0 FAIL):**
   - Kullanıcının verdiği 5 örnek DMC, hem detay-sütun hem dmc-only
     import yollarında ayrı ayrı test edildi.
   - Excel değerleri DMC ile zaten tutarlıysa hiçbir uyarı üretilmediği
     doğrulandı.
   - GERÇEKTEN geçersiz bir DMC'nin (assyCode segmenti gerçekten 3
     karakter) hâlâ doğru şekilde reddedildiği doğrulandı — yani
     düzeltme, gerçek hataları maskelemiyor.
   - 040 (descript), 420 (fault), 520 (proced), 941 (ipd) şema
     regresyonu tekrar çalıştırıldı — genIpd() içindeki
     assyCode/subSystemCode/subSubSystemCode/itemLocationCode/
     modelIdentCode değerlerinin artık DMC'den doğru geldiği ayrıca
     doğrulandı (bu alanlar önceden, Excel sütunu DMC ile tutarsızsa,
     YANLIŞ XML üretebiliyordu — bu da ayrıca düzeltilmiş oldu).
   - D-001 (systemCode), D-009 (infoCode), D-010 (icnmetadata.xsd),
     BREX (`SETTINGS.brex`/`brexDmRef`) — hiçbiri bozulmadı, `grep` ve
     kod testleriyle teyit edildi.
7. Tüm testler, gerçek `.html` dosyasından **programatik olarak
   çıkarılan** (uydurma/yeniden yazılan değil) kod parçalarıyla
   Node.js'te çalıştırıldı — bu, testlerin gerçekten teslim edilen
   dosyayı yansıttığından emin olmak için bilinçli bir tercihtir.
8. Güncellenmiş `Koluman_S1000D_Veri_Modulu_Uretici.html` (sözdizimi
   `node --check` ile doğrulandı) ve dört hafıza dosyası üretildi.
   **`CLAUDE.md` oluşturulmadı/değiştirilmedi.**

## ÖNEMLİ: Bu Oturumda Neyin Değiştiği, Neyin Değişmediği

**Değişti (kod, D-011) — yalnızca `importText()`'in detay-sütun (`else`)
dalındaki `raw` nesnesi oluşturma bloğu:**
- `bilesen: get('bilesen') || p.bilesen,` → `bilesen: p.bilesen,`
- `sokum: get('sokum') || p.sokum,` → `sokum: p.sokum,`
- `altAltSis: get('altAltSis') || p.altAltSis,` → `altAltSis: p.altAltSis,`
- `mic: get('mic') || p.mic, sdc: get('sdc') || p.sdc,` → `mic: p.mic, sdc: p.sdc,`
- `kkk: get('kkk') || p.kkk,` → `kkk: p.kkk,`
- `bilgiTurevi: get('bilgiTurevi') || (infoCode + p.infoVar),` → `bilgiTurevi: infoCode + p.infoVar,`
- Yeni: `dmcFieldMismatchCount`/`dmcFieldMismatchSamples` sayaçları
  eklendi (uyarı amaçlı, davranışı etkilemiyor) ve durum mesajlarına
  dahil edildi.

**Değişmedi (kod):**
- `DMC_RE`, `parseDmcString()`, `dmcLengthError()` — hiçbiri
  değiştirilmedi (kullanıcının devir notunda belirttiği gibi bunlar
  zaten hatalı değildi).
- `identAndStatus()`, `genIpd()`, `genDescript()`, `genProced()`,
  `genFault()`, `generateXml()` — fonksiyon gövdeleri değişmedi (yalnızca
  artık doğru `row.*` değerleriyle çağrılıyorlar).
- `systemCode` üretimi (D-001) — değişmedi.
- `infoCode` mantığı (D-009) — değişmedi.
- `icnmetadata.xsd` düzeltmesi (D-010) — değişmedi.
- `SETTINGS.brex` ve `brexDmRef` değerleri — değişmedi.
- BREX validator — geliştirilmedi.
- `dmc-only` import yolu — hiç değiştirilmedi (bu yolda bug hiç yoktu,
  zaten her zaman `p.*` kullanıyordu).
- `malzKat`, `sysCode` alanları — bilinçli olarak dokunulmadı (kullanılmıyorlar).
- UI tasarımı, refactor yok.

## Bu Oturumda YAPILMAYANLAR (kasıtlı)
- BREX konusuna hiç dönülmedi.
- `malzKat`/`sysCode` alanlarındaki aynı desen bilinçli olarak
  düzeltilmedi (kullanılmadıkları için gereksiz değişiklik olurdu).
- Geniş refactor yapılmadı — yalnızca kök nedene odaklanan minimal
  düzeltme.
- Resmi S1000D XSD paketiyle çapraz doğrulama yapılamadı (paket bu
  oturumda da sağlanmadı).
- Tarayıcı/DOM (canvas, Tesseract OCR, gerçek dosya sürükle-bırak) test
  edilmedi — yalnızca saf JS mantığı Node.js'te test edildi.
- `CLAUDE.md` oluşturulmadı/değiştirilmedi.

## Bir Sonraki Oturum Nereden Başlamalı
1. **T-007, T-011 ve T-013'ün kapatılması için:** Resmi S1000D Issue 5.0
   XSD paketi sağlanmalı; bu düzeltmelerle üretilen örnek XML çıktıları
   (özellikle 941/ipd şeması) `xmllint --schema` ile test edilmeli.
2. **T-012'nin teyidi için:** Resmi XSD paketi içinde gerçek dosya adının
   doğrulanması.
3. **BREX konusuna KESİNLİKLE dönülmemeli** — kullanıcı MSB/proje girdisi
   sağlamadan bu konu tekrar gündeme getirilmemeli.
4. Kullanıcı gerçek FORSDOC CSV'sini tekrar denediğinde, bu oturumda
   düzeltilen alanların (mic/sdc/altAltSis/bilesen/sokum/kkk) yanı sıra,
   benzer "Excel'in kırpma/biçimlendirme davranışı" kaynaklı başka
   sütunlarda da (ör. gelecekte eklenecek yeni alanlar) aynı desenin
   tekrarlanmadığından emin olunmalı — bu oturumda saptanan genel ilke:
   *tam geçerli DMC varken, konum/kod niteliğindeki hiçbir alan Excel
   sütunundan doğrudan alınmamalı; DMC her zaman tek gerçek kaynaktır.*
5. T-002 (schedul/wrngdata) veya T-005 (gerçek teknik içerik) öncelik
   sırasına göre ele alınabilir.

## Devir Teslim Kuralı (değişmedi + yeni ek)
XSD ve BREX doğrulamaları birbirinden kesin çizgilerle ayrı tutulur.
Dahili (Node.js/uygulama-içi) testlerin PASS vermesi, resmi S1000D XSD ile
uyumlu olduğu anlamına gelmez.

**Yeni ek kural (bu oturumdan):** Bir önceki oturumun "bu düzeltildi"
iddiası, **gerçek dosya üzerinde bağımsız olarak yeniden test edilmeden**
kesin doğru kabul edilmemelidir. Bu oturum, devir notundaki iddiayı
doğrudan kabul etmek yerine kodu satır satır inceleyip gerçek bir Node.js
testiyle doğrulamış ve iddianın yanlış olduğunu (düzeltmenin dosyada
mevcut olmadığını) tespit etmiştir. Bir sonraki oturum da benzer şekilde,
bu oturumun iddialarını (özellikle "54/54 PASS" ve "7906/7906 kabul"
sonuçlarını) mümkünse gerçek dosya üzerinde bağımsız olarak yeniden
doğrulamalıdır.
