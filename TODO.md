# TODO.md — Sonraki Geliştirme Adımları

Öncelik sırasına göre. Her madde bağımsız test edilebilir olacak şekilde
yazılmıştır. Koda dokunmadan önce `DECISIONS.md` ile çelişki olup olmadığı
kontrol edilmeli.

## T-001 — [ÖNCELİKLİ] systemCode üretim tutarlılığını test et
- Aynı bileşen için hem tam-DMC yapıştırma hem detay-sütun-import yollarını
  çalıştır, üretilen `dmCode`'un `systemCode` özniteliğini karşılaştır.
- `identAndStatus()` fonksiyonundaki `row.malzKat + row.sysCode` mantığının
  tam-DMC-import satırlarında `malzKat`/`sysCode` alanları dolu mu, boş mu
  geldiğini doğrula.
- Sonuca göre: ya `identAndStatus()` her iki yol için de `parseDmcString()`
  çıktısını tek kaynak olarak kullanacak şekilde düzeltilmeli, ya da iki yolun
  neden farklı olduğu belgelenmeli (kasıtlıysa).
- Bkz. `DECISIONS.md` D-001.

## T-002 — Eksik şema türlerini önceliklendir ve ekle
- Öncelik: `schedul` (bakım planlama/PMCS) — ZKA metodolojisinde sabit "05"
  sistemiyle proje geneli üretiliyor, kategori A-E mantığının dışında.
- Sonra: `wrngdata` (kablo/elektrik listesi verisi).
- Her yeni şema için: `INFO_SCHEMA`/`INFO_NAMES` tablosu zaten var, yalnızca
  `genXxx()` fonksiyonu ve `generateXml()` switch'ine case eklenmeli.

## T-003 — XSD/BREX doğrulama katmanı ekle
- En azından: üretilen XML'in well-formed olduğunu client-side kontrol et.
- Mümkünse: resmi S1000D 5.0 XSD'lerine karşı doğrulama (offline çalışma
  gereksinimiyle çelişmeyecek şekilde — XSD dosyaları yerel olarak paketlenebilir).
- `qualityAssurance` durumunun `<unverified/>` sabit kalıp kalmayacağına karar ver.

## T-004 — ICN gömme yöntemini S1000D 5.0 ile karşılaştır
- Mevcut yöntem: DOCTYPE ENTITY/NDATA (`<!ENTITY icn SYSTEM "...PNG" NDATA png>`).
- Bu yaklaşımın güncel S1000D 5.0 object/graphic modeliyle (dmodule için XSD
  şema referansı da dahil) uyumlu olup olmadığını doğrula.
- `dmodule` çıktısında hiç `xsi:noNamespaceSchemaLocation` yok — IMF dosyasında
  var. Bu farkın kasıtlı mı eksik mi olduğunu belirle.

## T-005 — İçerik gövdelerini gerçek teknik yazım yapısına yaklaştır
- Şu an descript/proced/fault içerikleri tek paragraf/tek adım placeholder.
- `S1000D_Teknik_Yazim_Icerik_Rehberi.md` ve
  `S1000D_MASTER_Teknik_Yazar_Rehberi.md` referans alınarak, en azından
  descript için 3 standart illüstrasyon/tablo yapısı (component index,
  access/area identification, component location) ve fault için
  eylem+soru+cevap çok adımlı akış şablonu değerlendirilmeli.
- Bu madde kapsam genişletme kararı gerektirir, önce ürün sahibiyle teyit edilmeli.

## Kapsam Dışı / Şimdilik Ertelenen
- ZWSP şüphesi — bkz. `DECISIONS.md` D-002, açık risk değil.
- Ayarların `localStorage` yerine dosya bazlı saklanması — henüz talep yok.
