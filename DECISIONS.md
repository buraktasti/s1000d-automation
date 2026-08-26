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

**Durum:** Doğrulandı, açık eksik.

Üretilen hiçbir XML, resmi S1000D XSD şemasına veya projenin BREX kurallarına
karşı doğrulanmıyor. `qualityAssurance` her zaman `<unverified/>` olarak
sabitlenmiş. Bu, araç çıktısının "S1000D uyumlu" olarak kabul edilmeden önce
harici bir doğrulama adımına ihtiyaç duyduğu anlamına gelir.
