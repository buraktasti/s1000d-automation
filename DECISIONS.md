# DECISIONS.md — Kararlar ve Doğrulanmış Teknik Riskler

Bu dosya, proje hakkında verilmiş kararları ve **doğrulanmış** (varsayıma
dayanmayan) teknik riskleri kayıt altına alır. Doğrulanamayan konular buraya
risk olarak yazılmaz; bkz. `PROJECT_STATUS.md` → "Doğrulanmamış / İncelenmesi
Gereken Noktalar".

---

## D-001 — systemCode üretim tutarsızlığı (AÇIK, ÖNCELİKLİ TEKNİK RİSK)

**Durum:** Doğrulandı, çözülmedi.

**Sorun:** `identAndStatus()` fonksiyonu içinde DMC'nin `systemCode` özniteliği
iki farklı içe aktarma yolunda farklı kaynaklardan üretiliyor:

- **Tam-DMC-parse yolu:** `parseDmcString()` fonksiyonu, yapıştırılan/CSV'den
  gelen tam DMC string'ini regex ile ayrıştırırken `systemCode` değerini
  doğrudan DMC'nin 3. segmentinden (`parts[2]`) alır.
- **Detay-sütun-import yolu:** `identAndStatus()` fonksiyonu XML üretirken
  `row.malzKat + row.sysCode` (iki ayrı Excel sütununun birleşimi) kullanır.

**Risk:** Aynı satır için bu iki yol farklı `systemCode` değeri üretebilir;
detay-sütun-import yolunda `malzKat`/`sysCode` alanları boş/eksikse
(`undefined` birleşimi gibi) `dmCode` malformed olabilir.

**Karar:** Bu konu koda dokunulmadan önce test edilecek — iki import yolundan
aynı bileşen için üretilen `dmCode`'lar karşılaştırılmalı.

**Sahiplik:** Bir sonraki geliştirme oturumu, kod değişikliğinden önce bunu
doğrulamalı (bkz. `TODO.md` T-001).

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
