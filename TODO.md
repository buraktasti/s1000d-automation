# TODO.md — Sonraki Geliştirme Adımları

Öncelik sırasına göre. Her madde bağımsız test edilebilir olacak şekilde
yazılmıştır. Koda dokunmadan önce `DECISIONS.md` ile çelişki olup olmadığı
kontrol edilmeli.

## T-001 — [KAPATILDI ✅] systemCode üretim tutarlılığı
- Gerçek `Koluman_S1000D_Veri_Modulu_Uretici.html` dosyasında iki nokta
  düzeltildi: `identAndStatus()` ve `genIpd()`.
- 7 test (TEST-01→07) gerçek dosyadan çıkarılan fonksiyonlarla çalıştırıldı,
  tümü PASS. Diff ile yalnızca 2 satırın değiştiği doğrulandı.
- Detay: `DECISIONS.md` D-001.

## T-006 — [KAPATILDI ✅] genIpd() içindeki bağımsız systemCode kullanımı
- T-001 ile birlikte aynı oturumda düzeltildi (bkz. yukarısı ve `DECISIONS.md` D-001).

## T-007 — [BEKLEMEDE / XSD İLE DOĞRULANACAK ⏳] assyCode (4 karakter) ve disassyCodeVariant (2 karakter) proje uzunluk kuralı
- DERMAN/FORSDOC proje girdisi olarak bildirilen uzunluk kuralları gerçek
  dosyada uygulandı: `dmcLengthError()` fonksiyonu eklendi, `importText()`
  içinde her iki import yolunu (tam-DMC ve detay-sütun) kapsayan ortak tek
  bir noktada çağrılıyor.
- 13 test (TEST-LEN-01→11 + 2 ek test) gerçek dosyadan çıkarılan
  fonksiyonlarla (Node.js + stub DOM) çalıştırıldı, tümü PASS. **Önemli
  netleştirme:** TEST-LEN-02/03/05/06'da "PASS", geçersiz uzunluktaki
  girdinin **doğru şekilde reddedildiğini** gösterir — geçersiz değerin
  kabul edildiği anlamına gelmez.
- `identAndStatus()`, `genIpd()`, `systemCode`, BREX fonksiyonlarına
  dokunulmadı.
- **Bu madde KAPATILMAMIŞTIR.** Çünkü bu değişiklik, resmi S1000D Issue
  5.0 XSD paketine karşı **çalıştırılmadı** — yalnızca dahili (Node.js
  tabanlı) birim/regresyon testleriyle doğrulandı. `assyCode`/
  `disassyCodeVariant` için 4/2 karakter uzunluk kuralının S1000D XSD'sinin
  karakter/datatype kısıtlarıyla çelişmediği yalnızca genel S1000D
  bilgisine dayanarak makul kabul edildi, resmi XSD ile çapraz
  doğrulanmadı.
- **Kapatma koşulu:** Resmi S1000D Issue 5.0 XSD paketi tekrar
  sağlandığında, bu değişiklikle üretilen örnek XML çıktıları
  `xmllint --schema` (veya eşdeğer) ile yeniden doğrulanmalı. Ancak o
  zaman madde "KAPATILDI ✅" olarak işaretlenebilir.
- Detay: `DECISIONS.md` D-005.

## T-002 — Eksik şema türlerini önceliklendir ve ekle
- Öncelik: `schedul` (bakım planlama/PMCS) — ZKA metodolojisinde sabit "05"
  sistemiyle proje geneli üretiliyor, kategori A-E mantığının dışında.
- Sonra: `wrngdata` (kablo/elektrik listesi verisi).
- Her yeni şema için: `INFO_SCHEMA`/`INFO_NAMES` tablosu zaten var, yalnızca
  `genXxx()` fonksiyonu ve `generateXml()` switch'ine case eklenmeli.

## T-003 — XSD/BREX doğrulama katmanı ekle
- XSD tarafı: baseline tamamlandı, bkz. `PROJECT_STATUS.md` → "XSD
  DOĞRULAMA BASELINE". **Ancak bu baseline T-007'yi kapsamıyor** — T-007
  ile eklenen uzunluk kuralı, baseline'dan sonra eklendi ve henüz XSD ile
  çapraz doğrulanmadı. Yeni bir kod değişikliği (content/dmCode/ICN
  üretimiyle ilgili) yapılırsa regresyon testi tekrar çalıştırılmalı.
- BREX tarafı: **yaklaşım değişti** (bkz. `DECISIONS.md` D-006). S1000D
  Default BREX artık DERMAN projesinin ana iş kuralı kaynağı olarak
  kullanılmayacak; öncelik MSB → MPG/FORSDOC girdileri → doğrulanmış proje
  kararları → S1000D XSD şeklinde. BREX validator geliştirme işi bu
  nedenle **beklemeye alındı** — MSB'ye ait gerçek bir BREX/Business Rules
  kaynağı sağlanmadan bu işe başlanmamalı.
- `qualityAssurance` durumunun `<unverified/>` sabit kalıp kalmayacağına
  karar verilmedi.

## T-004 — ICN gömme yöntemini S1000D 5.0 ile karşılaştır [KAPATILDI ✅ — yalnızca XSD/XML geçerliliği açısından]
- Mevcut yöntem: DOCTYPE ENTITY/NDATA (`<!ENTITY icn SYSTEM "...PNG" NDATA png>`).
- Gerçek S1000D Issue 5.0 XSD incelemesinde `graphic/@infoEntityIdent`
  alanının `xs:ENTITY` tipinde olduğu ve mevcut ENTITY/NDATA yaklaşımının
  XSD/XML açısından **geçerli** olduğu doğrulandı. Bu nedenle "doğrulanmamış
  açık risk" olarak artık değerlendirilmiyor.
- **Not:** Bu yalnızca XSD/XML geçerliliği açısından kapatıldı. BREX/proje
  iş kuralı düzeyinde ayrı bir kısıt çıkarsa (örn. MSB'nin ICN gömme
  yöntemine dair kendi kuralı varsa), bu ayrıca ele alınmalı — bkz. T-003
  BREX maddesi.

## T-005 — İçerik gövdelerini gerçek teknik yazım yapısına yaklaştır
- Şu an descript/proced/fault içerikleri tek paragraf/tek adım placeholder.
- `S1000D_Teknik_Yazim_Icerik_Rehberi.md` ve
  `S1000D_MASTER_Teknik_Yazar_Rehberi.md` referans alınarak, en azından
  descript için 3 standart illüstrasyon/tablo yapısı (component index,
  access/area identification, component location) ve fault için
  eylem+soru+cevap çok adımlı akış şablonu değerlendirilmeli.
- Bu madde kapsam genişletme kararı gerektirir, önce ürün sahibiyle teyit edilmeli.
- **Not:** Bu rehber dosyaları şu an Project Knowledge'da mevcut değil
  (kapasite nedeniyle eklenmedi) — gerektiğinde ayrıca sağlanmalı.

## T-008 — SNS Sistem Kodu (FORSDOC) doğrulaması — BEKLEYEN GİRDİ
- FORSDOC'taki "SNS Sistem Kodu = 00" tanımının uygulamadaki hangi alana
  (varsa) karşılık geldiği netleştirilmeli. `dmCode/@systemCode` ile
  **aynı alan olmadığı** kavramsal olarak netleştirildi (bkz. `DECISIONS.md`
  D-007) — mevcut `GA1` örnekleri bu nedenle bir çelişki olarak
  değerlendirilmemeli.
- Kod değişikliği yapılmadan önce FORSDOC/SNS konfigürasyon kaynağından
  netleştirme gerekiyor.

## T-009 — MSB katmanı — BEKLEYEN İŞ KURALI GİRDİSİ
- DERMAN projesinin FORSDOC katmanı (S1000D 5.0 → MSB) uygulamada ayrı bir
  alan/kural olarak temsil edilmiyor; bu tek başına hata değil (bkz.
  `DECISIONS.md` D-008).
- MSB'ye ait gerçek bir BREX veya Business Rules kaynağı sağlanmadan
  uygulamaya MSB'ye özgü XML/kod **eklenmemeli**.

## T-010 — ICN Metodu "İkisi Birden" ve SNS Dili doğrulaması — DOĞRULANAMADI
- FORSDOC'taki "ICN Metodu: İkisi Birden" teriminin kesin tanımı elimizde
  yok. Uygulamada gözlemlenen manuel+OCR ikili hotspot konumlandırma
  yönteminin bu terime karşılık gelip gelmediği doğrulanamadı.
- "SNS Dili: Turkish" için ayrı bir açık ayar yok; genel Türkçe-öncelikli
  tasarım (kodlama tespiti, `turkishFold()`, Türkçe arayüz) bunu dolaylı
  olarak karşılıyor gibi görünüyor, ancak FORSDOC'un bu terime kesin
  karşılığı doğrulanmadı.

## Kapsam Dışı / Şimdilik Ertelenen
- ZWSP şüphesi — bkz. `DECISIONS.md` D-002, açık risk değil.
- Ayarların `localStorage` yerine dosya bazlı saklanması — henüz talep yok.
