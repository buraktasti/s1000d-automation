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

## T-011 — [BEKLEMEDE / XSD İLE DOĞRULANACAK ⏳] infoCode kritik hatası düzeltmesi
- Detay: `DECISIONS.md` D-009. Bu oturumda regresyon testleriyle yeniden
  doğrulandı, kod değişmedi.

## T-012 — [BAĞIMSIZ TEYİT BEKLİYOR] icnmetadata.xsd dosya adı düzeltmesi
- Detay: `DECISIONS.md` D-010. Bu oturumda değişmedi.

## T-013 — [YENİ — BEKLEMEDE / XSD İLE DOĞRULANACAK ⏳] bilesen/sokum/altAltSis/mic/sdc/kkk kritik hatası düzeltmesi (7906 satır yanlış reddi)
- **Kaynak:** Gerçek FORSDOC CSV importunda 7906 satırın tamamı, proje
  uzunluk kuralı (`dmcLengthError`) nedeniyle yanlışlıkla reddedildi.
- **Kök neden:** Detay-sütun import yolunda `bilesen`/`sokum`/`altAltSis`/
  `mic`/`sdc`/`kkk` alanları, D-009'daki infoCode hatasıyla birebir aynı
  desende (`get('x') || p.x`), Excel'in sayısal biçimlendirme nedeniyle
  kırpılmış (ör. `"0000"` → `"0"`) ama boş olmayan sütun değerleriyle
  DMC'den doğru türetilen değerin önüne geçiyordu.
- **Düzeltme:** Tam DMC geçerliyken bu alanlar artık her zaman
  `parseDmcString(dmc)`'den geliyor; Excel'in ilgili sütunu yalnızca
  uyuşmazlık uyarısı (`dmcFieldMismatchCount`/`dmcFieldMismatchSamples`)
  için okunuyor, satır asla bu yüzden atlanmıyor, XML üretimini asla
  etkilemiyor.
- **Test sonuçları:** 54/54 dahili test PASS + 7906 satırlık gerçekçi
  ölçek testinde düzeltme öncesi 0/7906 → düzeltme sonrası 7906/7906
  kabul edildiği doğrulandı.
- **Neden hâlâ açık:** Resmi S1000D Issue 5.0 XSD paketiyle `xmllint
  --schema` çapraz doğrulaması bu oturumda da yapılamadı (paket
  sağlanmadı). Resmi paket sağlandığında, bu değişiklikle üretilen örnek
  XML'ler (özellikle `ipd`/941 şeması, çünkü `assyCode`/`subSystemCode`/
  `subSubSystemCode` doğrudan `row.bilesen`/`row.altAltSis`'ten geliyor)
  yeniden test edilmeli.
- Detay: `DECISIONS.md` D-011.

## T-017 — [YENİ — KULLANICI FORSDOC'TA YENİDEN TEST ETMELİ] IPD parça tablosu genişletmesi + şema-bazlı Resim Açıklaması kuralı
- **Kaynak:** Gerçek FORSDOC referans üçlüsü (resmi S1000D BIKE örneği +
  kullanıcının M0117 gerçek projesi) karşılaştırması. Detay: `DECISIONS.md`
  D-017, D-018, D-019, D-020, D-021.
- **Uygulanan 3 değişiklik:**
  1. `genIpdPartItemBlocks()` — IPD'de ICN'e bağlı hotspot/parça kayıtları
     artık `catalogSeqNumber` tablosuna tek tek yansıyor (önceden yalnızca
     üst montaj satırı vardı).
  2. `figureBlock()` — Resim Açıklaması (hotspot+legend) kuralı şema-bazlı:
     yalnızca `ipd` (RPK) hariç, `descript`/`proced`/`fault` dahil tüm
     mevcut şema türlerinde varsayılan olarak var.
  3. `injectIcnIntoXml()` — `fault` şeması için figure/legend gömme desteği
     eklendi (önceden hiç yoktu).
- **Test sonuçları:** Node.js'te izole fonksiyon testleriyle doğrulandı
  (genIpdPartItemBlocks sıralama/alan eşlemesi, figureBlock 6 senaryo
  matrisi, injectIcnIntoXml 411/412/414 fault kalıpları). `node --check`
  ile tüm dosyanın sözdizimi doğrulandı.
- **Neden hâlâ açık:** Bu oturumda **hiçbir gerçek FORSDOC import testi
  yapılmadı.** Kullanıcının şunları FORSDOC'ta yeniden test edip
  bildirmesi gerekiyor:
  1. M0117 (Soğutma Donanımı) örneğini bu düzeltmeyle yeniden üretip
     FORSDOC'a yüklemek; FORIPS PDF'te Tablo 1'in artık 22 satır (1 üst
     montaj + 21 parça) gösterdiğini doğrulamak.
  2. IPD (RPK) çıktısında Resim Açıklaması'nın artık hiç basılmadığını
     doğrulamak (BIKE örneğiyle tutarlı olmalı).
  3. `descript`/`proced` çıktılarında Resim Açıklaması'nın hâlâ (önceki
     gibi) doğru bastığını doğrulamak — regresyon kontrolü.
  4. En az bir `fault` (410/411/412/413/414) veri modülünü FORSDOC'a
     yükleyip figür+legend'in doğru göründüğünü doğrulamak (bu şema türü
     için ilk kez test ediliyor).

## T-018 — [YENİ — BEKLEMEDE / KULLANICI KARARI GEREKLİ] 7 yeni şema türü için üretim fonksiyonu yok: crew, schedul, checklist, process, frontmatter, container, sb
- **Kaynak:** Kullanıcı, Resim Açıklaması kuralının kapsamını genişletirken
  bu 7 şema türünü de listeledi. `SUPPORTED_SCHEMAS = ['descript','proced',
  'fault','ipd']` (satır ~339) bunların hiçbirini içermiyor;
  `schedul` kısmen `INFO_SCHEMA`'da eşleniyor (`'0B0':'schedul'`) ama
  `generateXml()`'in switch'inde bir `case 'schedul'` yok (zaten T-002'de
  not edilmişti).
- **Kullanıcı kararı (bu oturumda netleşti):** Bu 7 şema türü için üretim
  fonksiyonu yazmak **ayrı, daha büyük kapsamlı bir iş** olarak ele
  alınacak; bu oturumda dokunulmadı. Bir sonraki oturumda kullanıcı hangi
  şema türünün önce ele alınacağına (muhtemelen `schedul`, T-002 ile
  aynı öncelik) karar vermeli.
- **Not:** Bu türler için üretim fonksiyonu eklendiğinde, D-020'deki
  "yalnızca ipd hariç tüm şemalarda Resim Açıklaması var" kuralı
  `figureBlock()`/`injectIcnIntoXml()`'e otomatik olarak uygulanmalı
  (kod zaten `schema==='ipd'` dışındaki HER şema için varsayılan `true`
  döner — yeni şema `injectIcnIntoXml()`'e bir `if (schema==='YENİ')` dalı
  olarak eklendiği sürece ek bir değişiklik gerekmez).


## T-016 — [YENİ — KULLANICI FORSDOC'TA YENİDEN TEST ETMELİ] FAZ 2: MIC bazlı ICN + VM hotspot/legend gömme + SNS eşleştirme
- **Kaynak:** Üç Faz 1 analiz turunda toplanan gerçek FORSDOC export
  verisi (Temel Motor + Egzoz Sistemi ICN/VM çiftleri) ve resmi "Bilgi
  Kontrol Numarası" eğitim materyali.
- **Uygulanan 3 değişiklik:**
  1. MIC bazlı ICN üretimi (`nextIcnCodeMic()`) — Firma Kodlu üretim
     (`nextIcnCode()`) hiç bozulmadan, UI'da radyo seçimiyle eklendi.
  2. VM `<graphic>` içine `<hotspot>` + `<legend>` gömme
     (`figureBlock()` yeniden yazıldı) — IMF ile VM hotspot verisi artık
     `computeIcnHotspotObjects()` üzerinden ortak kaynaktan geliyor.
  3. SNS hiyerarşik eşleştirme fonksiyonu (`icnBelongsToSnsNode`) ve
     `ICN_LIBRARY` veri modeli genişletmesi (`systemCode/subSystemCode/
     subSubSystemCode/assyCode` + hesaplanmış anahtarlar).
- **Test sonuçları:** 52/52 dahili test PASS (bkz. `DECISIONS.md` D-016).
  Gerçek Egzoz Sistemi VM+ICN çiftiyle uçtan uca üretim, gerçek FORSDOC
  dosyasıyla yapısal olarak karşılaştırıldı.
- **Neden hâlâ açık / kullanıcıdan beklenenler:**
  1. **FORSDOC'ta gerçek import testi yapılmadı** — kullanıcının bu
     düzeltmeyle üretilmiş yeni paketleri (hem Firma Kodlu hem MIC bazlı)
     FORSDOC'a yükleyip sonucu bildirmesi gerekiyor.
  2. **"A" placeholder'ının (sorumlu firma kodu/varyant) DERMAN için
     doğruluğu** hâlâ DOĞRULANAMADI — yalnızca gözlemlenen sabit değer
     kullanıldı.
  3. **SNS ağacında "ICN kütüphanesi tarayıcısı" ekranı henüz yok** —
     `icnBelongsToSnsNode()` fonksiyonu hazır ama hiçbir UI ekranına
     bağlanmadı (spekülatif özellik icat etmemek için bilinçli olarak
     bu turda eklenmedi). Kullanıcı bu ekranın tam olarak nasıl
     görüneceğine karar verirse eklenebilir.
  4. Sistem/Alt Sistem seviyelerinde gerçek VM/ICN örneği hâlâ yok —
     yalnızca "Alt Alt Sistem" seviyesi (assyCode="0000") gerçek veriyle
     kanıtlı.

## T-015 — [YENİ — KULLANICI FORSDOC'TA YENİDEN TEST ETMELİ] ICN'de etkin noktalar (hotspot) görünmüyordu — imfIdentIcn "ICN-" ön eki hatası
- **Kaynak:** D-014 sonrası VM+ICN FORSDOC'a başarıyla yüklendi, ama
  ICN'deki etkin noktalar görünmüyordu. Kullanıcı, RPK Builder
  Professional V16.3 ile üretip FORSDOC'a etkin noktalarıyla birlikte
  BAŞARIYLA yüklediği bir referans dosya paylaştı.
- **Bulunan kesin hata:** `buildImfXml()` içinde `imfCode/@imfIdentIcn`
  yanlışlıkla "ICN-" ön ekiyle (`icn.code`) üretiliyordu; zaten hesaplanan
  ama kullanılmayan ön eksiz `code` değişkeni kullanılmalıydı.
- **Düzeltme:** `imfIdentIcn="'+xmlEscape(icn.code)+'"` →
  `imfIdentIcn="'+xmlEscape(code)+'"`. Ayrıca D-010 geri alındı:
  `icnMetadata.xsd` büyük M'ye döndürüldü (referans dosyada öyleydi).
- **Test sonuçları:** Node.js testinde `imfIdentIcn` artık ön eksiz
  üretiliyor, dosya adı değişmedi, PASS.
- **Neden hâlâ açık:** Kullanıcının bu düzeltmeyle üretilmiş yeni bir
  ICN/IMF paketini FORSDOC'a tekrar yükleyip etkin noktaların gerçekten
  göründüğünü doğrulaması gerekiyor.

## T-014 — [YENİ — KULLANICI FORSDOC'TA YENİDEN TEST ETMELİ] xsi:noNamespaceSchemaLocation eksikti (D-014); systemCode düzeltmesi (D-012) yanlış çıktı ve geri alındı
- **1. deneme:** Kullanıcının uygulamayla ürettiği bir ZIP FORSDOC'a
  aktarılamadı: `{"code":"S1000D.Error:00183","message":"Katmana veri
  aktarılırken bir hata oluştu.","validationErrors":null}`. Şüpheyle
  `systemCode="GA1"` (3 karakter) 2 karaktere zorlandı (D-012) ve IMF'deki
  yinelenen `icnObjectIdent` düzeltildi (D-013).
- **2. deneme:** Kullanıcı D-012+D-013 içeren yeni bir ZIP ile tekrar
  denedi — **AYNI hata** tekrar alındı. Bu, D-012'nin yanlış bir teşhis
  olduğunu gösterdi.
- **Karşılaştırma:** Kullanıcıdan FORSDOC'a **daha önce BAŞARIYLA
  yüklediği** 11 dosyalık gerçek bir ZIP istendi. Doğrudan karşılaştırma
  şunu ortaya çıkardı:
  - `systemCode="GA1"` (3 karakter) FORSDOC tarafından **kabul ediliyordu**
    → D-012 **YANLIŞTI, geri alındı.**
  - Başarılı dosyaların `<dmodule>` kök elemanında HER ZAMAN `xmlns:xsi` +
    `xsi:noNamespaceSchemaLocation` (şemaya özel XSD adresi) vardı; bizim
    üretimimizde bu **hiç yoktu** → **D-014, muhtemel asıl kök neden.**
- **Düzeltme (D-014):** `dmHeader(schema)` fonksiyonu eklendi; artık
  `descript`/`proced`/`fault`/`ipd` şemalarının her biri için doğru
  `xsi:noNamespaceSchemaLocation` üretiliyor. ICN entity/DOCTYPE
  enjeksiyonu da (çift DOCTYPE oluşmaması için) buna göre güncellendi.
- **Test sonuçları:** 35/35 yeni test PASS (bkz. `DECISIONS.md` D-014).
- **Neden hâlâ açık:** Kullanıcının FORSDOC'a **üçüncü kez** güncellenmiş
  HTML ile yeni bir ZIP üretip import etmesi ve sonucu bildirmesi
  gerekiyor. Başarılı referans dosyalarında bulunan ama bizim üretimimizde
  hâlâ olmayan bazı ek elemanlar var (`exportControl`, `restrictionInfo/
  copyright`, `systemBreakdownCode`, `skillLevel`, `reasonForUpdate`) —
  bunların FORSDOC için zorunlu olup olmadığı DOĞRULANAMADI. Eğer D-014
  sonrası hata devam ederse, sıradaki şüpheli bunlar olmalı.

## T-002 — Eksik şema türlerini önceliklendir ve ekle
- Öncelik: `schedul` (bakım planlama/PMCS), sonra `wrngdata`. (Değişmedi.)

## T-003 — XSD/BREX doğrulama katmanı ekle
- XSD tarafı: baseline tamamlandı. **Ancak bu baseline T-007, T-011 ve
  T-013'ü kapsamıyor** — üçü de baseline'dan sonra eklendi/düzeltildi ve
  henüz XSD ile çapraz doğrulanmadı.
- BREX tarafı: **çalışma tamamen durduruldu** (bkz. `DECISIONS.md` D-006).
  Bu oturumda da BREX konusuna dönülmedi. Bu maddeye bir sonraki oturumda
  **kullanıcı açıkça talimat vermeden** dönülmemeli.

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

## Bu Oturumda Yapılan Test Çalışması (referans amaçlı)
Gerçek HTML dosyasından programatik olarak çıkarılan (`importText()`
döngüsü, `parseDmcString`, `dmcLengthError`, `generateXml`,
`identAndStatus`, `genIpd` vb.) kod parçalarıyla Node.js üzerinde 54
birim/regresyon testi ve 7906 satırlık gerçekçi bir ölçek testi
çalıştırıldı — tümü PASS. Tarayıcı DOM'una bağımlı kısımlar (canvas,
Tesseract OCR, dosya yükleme UI'ı) bu oturumda test edilmedi (bkz.
`PROJECT_STATUS.md`).

## Kapsam Dışı / Şimdilik Ertelenen
- ZWSP şüphesi — bkz. `DECISIONS.md` D-002, açık risk değil.
- Ayarların `localStorage` yerine dosya bazlı saklanması — henüz talep yok.
- **BREX çalışmasının tamamı** — kullanıcı tarafından durduruldu, MSB/proje
  girdisi gelene kadar ele alınmayacak (bkz. `DECISIONS.md` D-006).
