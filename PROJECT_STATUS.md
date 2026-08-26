# PROJECT_STATUS.md — Mevcut Durum

Son güncelleme: 2026-08-26 (T-001/T-006 düzeltmesi gerçek dosyada uygulandı ve doğrulandı)

## Genel Durum
Uygulama çalışır durumda ve 4 temel S1000D şema türü için XML üretebiliyor.
Kod tabanı henüz bu oturumda **değiştirilmedi** — bu dosyalar yalnızca analiz/hafıza amaçlıdır.

## Tamamlanmış / Doğrulanmış Bileşenler
- [x] Excel/CSV içe aktarma (yapıştırma + dosya seçme), otomatik ayraç tespiti (`,` `;` `\t`)
- [x] Türkçe kodlama otomatik tespiti (UTF-8 / Windows-1254 fallback)
- [x] Tam DMC string'inden regex ile alan ayrıştırma (`DMC_RE`, `parseDmcString`)
- [x] Detaylı Excel sütunlarından alan türetme (`HEADER_MAP`)
- [x] SNS ağacı görünümü, arama/filtre (şema türü, destek durumu)
- [x] Tekli XML önizleme/indirme
- [x] Toplu ZIP indirme (özel minimal ZIP yazıcı, dış bağımlılık yok)
- [x] `descript`, `proced`, `fault`, `ipd` şemaları için XML üretim fonksiyonları
- [x] ICN/IMF: görsel yükleme, parça listesi eşleme, hotspot koordinat üretimi
- [x] Tesseract.js OCR entegrasyonu (hotspot otomatik tespiti, çoklu geçiş: gray/otsu/adaptive)
- [x] ICN+IMF+VM birleşik ZIP paketleme
- [x] Proje ayarları `localStorage`'da kalıcı (MIC, SDC, CAGE, org, dil/ülke, issue no, BREX ref)
- [x] 4 örnek DMC XML dosyası ile çıktı yapısı çapraz kontrol edildi (identAndStatusSection düzeyinde tutarlı)

## Doğrulanmış Eksikler / Yapılmayanlar
- [ ] `schedul` (bakım planlama/PMCS) şeması — eşleme var, üretim fonksiyonu yok
- [ ] `wrngdata` (kablo verisi) şeması — eşleme var, üretim fonksiyonu yok
- [ ] `crew`, `process` ve diğer S1000D şema türleri — hiç desteklenmiyor
- [ ] XSD şema doğrulama — yok (`qualityAssurance` her zaman `<unverified/>`)
- [ ] BREX kural doğrulama — yok (yalnızca `brexDmRef` referansı gömülüyor, kural kontrolü yapılmıyor)
- [ ] Gerçek teknik içerik üretimi — mevcut içerik gövdeleri (descript/proced/fault) placeholder/iskelet niteliğinde, Content Rehberi'nde tarif edilen çok-seviyeli/illüstrasyonlu yapıyı yansıtmıyor

## Doğrulanmamış / İncelenmesi Gereken Noktalar
- ICN gömme yönteminin (DOCTYPE ENTITY/NDATA) güncel S1000D 5.0 object/graphic modeliyle uyumu
- ZWSP (zero-width space) şüphesi — ham kaynak dosyada doğrulanmadı, risk olarak kayıtlı değil (bkz. D-002)

## T-001 / T-006 Durumu (KAPATILDI ✅)
`systemCode` üretim tutarsızlığı iki noktada (`identAndStatus()` ve
`genIpd()`) gerçek dosyada düzeltildi ve 7 testle (TEST-01→07) doğrulandı,
tümü PASS. Diff ile yalnızca 2 satırın değiştiği, başka hiçbir davranışın
etkilenmediği teyit edildi. Detay: `DECISIONS.md` D-001.

## Güncellenmiş Tamamlanmış Bileşenler Listesine Ek
- [x] `systemCode` üretimi artık her iki üretim noktasında da (`identAndStatus()`, `genIpd()`) tek kaynak olan `row.dmc`'den türetiliyor.

## Offline Çalışma
Ana akış tamamen offline. Yalnızca Tesseract.js OCR özelliği ilk kullanımda
internet gerektirir (CDN'den yükleniyor).
