# CLAUDE.md — Proje Bağlamı

## Bu dosyanın amacı
Bu repo'yu ilk kez açan bir Claude oturumunun, önceki analizleri tekrarlamadan
projeyi kaldığı yerden anlayıp devam edebilmesi için yazılmıştır.
Diğer hafıza dosyaları: `PROJECT_STATUS.md`, `DECISIONS.md`, `TODO.md`,
`SESSION_HANDOVER.md`.

## Proje Amacı
`Koluman_S1000D_Veri_Modulu_Uretici.html` — Excel/CSV kaynaklı DMC (Data Module
Code) listesini S1000D uyumlu XML veri modüllerine (VM) dönüştüren, tamamen
tarayıcı içinde çalışan (sunucu/build gerektirmeyen) tek-dosya bir araç.
Koluman Otomotiv Endüstri A.Ş. — Askeri Projeler Müdürlüğü / ELD Birimi için,
ZKA (Zırhsız Kurtarıcı Araç) projesi kapsamında geliştiriliyor.

## Repo İçeriği
- `Koluman_S1000D_Veri_Modulu_Uretici.html` — uygulamanın tamamı (HTML+CSS+JS, tek dosya)
- `S1000D_MASTER_Teknik_Yazar_Rehberi.md` — bilgi seti → şema türü referansı, "ne yazılır" rehberi
- `S1000D_Teknik_Yazim_Icerik_Rehberi.md` — her bilgi setinde teknik içeriğin ne olması gerektiği
- `ZKA_Bilgi_Paketi_Metodolojisi.md` — MIL-STD-40051 + S1000D eşlemesi, proje-özel kategori mantığı (A-E)
- `S1000D_Bilgi_Setleri_Analiz_ve_Rehber.md` — SNS bazlı kodlama analizleri, kritik bulgular (fault seviyesi, IC 040 bölünmesi vb.)
- 4 örnek DMC XML: `..._040A-A_descript.xml`, `..._420A-A_fault.xml`, `..._520A-A_proced.xml`, `..._941A-D_ipd.xml`

## Mimari Özet
- Backend yok. Tüm mantık tarayıcıda çalışan vanilla JS içinde.
- Veri girişi: yapıştırma veya CSV dosyası (otomatik ayraç + Türkçe kodlama tespiti).
- İki içe aktarma yolu: (a) tam DMC string'i regex ile parse, (b) detaylı Excel sütunları `HEADER_MAP` ile eşlenir.
- XML üretimi: DOM/XML builder yok, template literal string concatenation + `xmlEscape`.
- ZIP üretimi: özel/minimal ZIP yazıcı (dış kütüphane yok).
- Ayarlar: `localStorage` (`koluman_s1000d_settings_v2`).
- ICN/IMF: oturum içi bellekte tutulan `ICN_LIBRARY`; Tesseract.js (CDN'den yükleniyor) ile OCR tabanlı hotspot tespiti.

## Desteklenen Şema Türleri
`SUPPORTED_SCHEMAS = ['descript','proced','fault','ipd']` — yalnızca bu 4 tür
gerçekten XML üretiyor (`generateXml()` switch). `INFO_SCHEMA`/`INFO_NAMES`
tablosunda `schedul` ve `wrngdata` da eşlenmiş görünür ama bunlar için üretim
fonksiyonu **yoktur** (arayüzde "şablon yok" olarak işaretlenir).

## Offline Çalışma Durumu
Ana akış (import/parse/XML üretim/ZIP) tamamen offline çalışır. **İstisna:**
Tesseract.js OCR, ilk kullanımda 3 CDN'den (jsdelivr/unpkg/cdnjs) internetten
yükleniyor. Uygulama alt bilgisindeki "Tamamen offline çalışır" ifadesi bu
özellik için geçerli değildir.

## Doğrulanmış Öncelikli Teknik Risk
`identAndStatus()` fonksiyonunda `dmCode`'un `systemCode` özniteliği
**iki farklı kaynaktan** üretilebiliyor:
- Tam-DMC-parse yolunda: `parseDmcString()` → DMC string'inin 3. segmentini (`parts[2]`) doğrudan `systemCode` olarak kullanır.
- Detay-sütun-import yolunda: `identAndStatus()` içinde `row.malzKat + row.sysCode` birleştirmesi kullanılır.

Bu iki yolun aynı satır için tutarlı/aynı sonucu üretip üretmediği
**doğrulanmadı**. Öncelikli test/inceleme konusudur — bkz. `DECISIONS.md` ve `TODO.md`.

## Notlar
- ZWSP (zero-width space) şüphesi önceki bir analizde gündeme geldi; ham kaynak
  dosyada doğrulanamadığı için **açık bir risk olarak kayıtlı değildir**.
