# PROJECT_STATUS.md — Mevcut Durum

Son güncelleme: 2026-08-26 (T-007 durum düzeltmesi: "KAPATILDI" hatalıydı,
"BEKLEMEDE / XSD İLE DOĞRULANACAK" olarak düzeltildi — gerçek S1000D XSD
paketi bu değişikliğe karşı çalıştırılmadığı için)

## Genel Durum
Uygulama çalışır durumda ve 4 temel S1000D şema türü için XML üretebiliyor.
Bu oturumda **tek bir minimal kod değişikliği** uygulandı: assyCode/
disassyCodeVariant için proje uzunluk kuralı doğrulaması (T-007 — durumu
**BEKLEMEDE / XSD İLE DOĞRULANACAK**, henüz kapatılmadı). Diğer her şey
(BREX, SNS Sistem Kodu, MSB katmanı) yalnızca analiz/karar aşamasında,
koda henüz yansıtılmadı.

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
- [~] assyCode (4 karakter) / disassyCodeVariant (2 karakter) proje uzunluk kuralı — kod uygulandı, dahili testlerle doğrulandı, **ancak resmi S1000D XSD paketiyle henüz çapraz doğrulanmadı** (T-007, bkz. aşağıda)

## Doğrulanmış Eksikler / Yapılmayanlar
- [ ] `schedul` (bakım planlama/PMCS) şeması — eşleme var, üretim fonksiyonu yok
- [ ] `wrngdata` (kablo verisi) şeması — eşleme var, üretim fonksiyonu yok
- [ ] `crew`, `process` ve diğer S1000D şema türleri — hiç desteklenmiyor
- [ ] BREX kural doğrulama — yok (yalnızca `brexDmRef` referansı gömülüyor, kural kontrolü yapılmıyor); yaklaşım değişti, bkz. aşağıda "BREX Doğrulama Durumu"
- [ ] Gerçek teknik içerik üretimi — mevcut içerik gövdeleri (descript/proced/fault) placeholder/iskelet niteliğinde, Content Rehberi'nde tarif edilen çok-seviyeli/illüstrasyonlu yapıyı yansıtmıyor
- [ ] T-007 uzunluk kuralının resmi S1000D XSD paketiyle çapraz doğrulaması — **yapılmadı**, bkz. aşağıda

## Doğrulanmamış / İncelenmesi Gereken Noktalar
- ZWSP (zero-width space) şüphesi — ham kaynak dosyada doğrulanmadı, risk olarak kayıtlı değil (bkz. D-002)
- **BREX kimliği (A vs G):** `S1000D-A-04-10-0301-00A-022A-D` (uygulamanın mevcut varsayılanı) ile
  `S1000D-G-04-10-0301-00A-022A-D` (kullanıcı tarafından sağlanan ve dosya içeriğiyle S1000D Issue 5.0
  default BREX'i olduğu doğrulanan kimlik) arasındaki ilişki. Kullanıcı, `A`'nın Issue 4.0'a ait olduğunu
  bildirdi; bu bildirim bu oturumda bağımsız bir Issue 4.0 kaynağıyla **doğrulanamadı**. Ayrıca proje artık
  S1000D Default BREX'i ana iş kuralı kaynağı olarak kullanmama kararı aldığından, bu kimlik farkının
  pratik önemi azaldı — bkz. `DECISIONS.md` D-006.
- **MSB'ye ait BREX/Business Rules kaynağı** — elimizde yok. "BEKLEYEN PROJE GİRDİSİ" (D-006).
- **SNS Sistem Kodu (FORSDOC "00") ile dmCode/@systemCode ilişkisi** — kavramsal olarak ayrı iki alan
  olduğu netleştirildi (aynı alan değiller), ancak FORSDOC/SNS Sistem Kodu'nun uygulamada tam olarak
  neye karşılık geldiği hâlâ doğrulanmadı. "BEKLEYEN FORSDOC/SNS KONFİGÜRASYON BİLGİSİ" (D-007).
- **MSB katmanının uygulamada temsili** — FORSDOC katmanı (S1000D 5.0 → MSB) uygulamada ayrı bir alan
  gerektirmiyor (henüz gerçek bir MSB iş kuralı kaynağı sağlanmadığı sürece). "BEKLEYEN MSB İŞ KURALI
  GİRDİSİ" (D-008).
- **ICN Metodu "İkisi Birden" ve SNS Dili** teriminin FORSDOC'taki kesin tanımı doğrulanamadı (bkz. TODO.md T-010).
- **T-007 uzunluk kuralı — resmi XSD ile çapraz doğrulama eksik:** assyCode/disassyCodeVariant için 4/2
  karakter kuralının S1000D XSD karakter/datatype kısıtlarıyla çelişmediği yalnızca genel S1000D bilgisine
  dayanarak makul kabul edildi; bu oturumda gerçek XSD paketi sağlanmadığından resmi doğrulama yapılamadı.

## T-001 / T-006 Durumu (KAPATILDI ✅)
`systemCode` üretim tutarsızlığı iki noktada (`identAndStatus()` ve
`genIpd()`) gerçek dosyada düzeltildi ve 7 testle (TEST-01→07) doğrulandı,
tümü PASS. Diff ile yalnızca 2 satırın değiştiği, başka hiçbir davranışın
etkilenmediği teyit edildi. Detay: `DECISIONS.md` D-001.

## T-007 Durumu (BEKLEMEDE / XSD İLE DOĞRULANACAK ⏳) — assyCode / disassyCodeVariant uzunluk kuralı

**Önceki bir özet bu maddeyi hatalı şekilde "KAPATILDI ✅" olarak sunmuştu.
Bu yanlıştı ve bu güncellemeyle düzeltildi.** Doğru durum:

DERMAN/FORSDOC proje girdisi olarak bildirilen uzunluk kuralları
(`assyCode` = 4 karakter, `disassyCodeVariant` = 2 karakter) gerçek dosyada
uygulandı: `dmcLengthError()` fonksiyonu eklendi (`looksLikeDmc()`'den hemen
sonra), `importText()` içindeki ortak satır döngüsünde her iki import
yolunu (tam-DMC ve detay-sütun) kapsayan **tek bir noktada** çağrılıyor.
Kural ihlali olan satırlar atlanıyor, kullanıcıya durum mesajında sayı ve
örnek hata metniyle bildiriliyor.

13 test (TEST-LEN-01→11 + 2 ek test) gerçek dosyadan çıkarılan
fonksiyonlarla (Node.js + stub DOM) çalıştırıldı, tümü PASS, 0 FAIL. Ancak
bu testlerin **hiçbiri resmi S1000D XSD paketine karşı çalıştırılmadı** —
yalnızca uygulamanın kendi mantığını (dahili JS fonksiyonları) test etti.

**Test sonuçlarının doğru okunuşu — önemli netleştirme:** TEST-LEN-02
(assyCode=001, 3 karakter), TEST-LEN-03 (assyCode=00001, 5 karakter),
TEST-LEN-05 (disassyCodeVariant=A, 1 karakter) ve TEST-LEN-06
(disassyCodeVariant=AAA, 3 karakter) testlerinde **"PASS", bu geçersiz
girdilerin doğru şekilde reddedildiğini gösterir** — geçersiz bir değerin
kabul edildiği anlamına **gelmez**. "PASS" burada "test senaryosu beklenen
sonucu (ret) doğru şekilde üretti" demektir.

`identAndStatus()`, `genIpd()`, `systemCode`, BREX fonksiyonlarına
dokunulmadı.

**Neden hâlâ açık:** Bu 4/2 karakter kuralının resmi S1000D XSD
karakter/datatype kısıtlarıyla çelişmediği yalnızca genel S1000D bilgisine
dayanarak makul kabul edildi — bu oturumda gerçek XSD dosyası
sağlanmadığından resmi çapraz doğrulama yapılamadı. Madde, resmi XSD
paketi tekrar sağlanıp bu değişiklikle üretilen örnek çıktılar
`xmllint --schema` ile test edilene kadar **"KAPATILDI" olarak
işaretlenemez.**

Detay: `DECISIONS.md` D-005.

## Offline Çalışma
Ana akış tamamen offline. Yalnızca Tesseract.js OCR özelliği ilk kullanımda
internet gerektirir (CDN'den yükleniyor).

## XSD DOĞRULAMA BASELINE (2026-08-26) — TAMAMLANDI ✅
Kullanıcı tarafından resmi **S1000D Issue 5.0 XML Schema Package.zip** yüklendi
ve gerçek `xmllint --schema` ile otomatik doğrulama çalıştırıldı (manuel
karşılaştırma değil — gerçek validator, gerçek exit code, negatif kontrolle
doğrulanmış kurulum).

**Sonuç: 16/16 test PASS, 0 FAIL.**

| Test | XML | XSD | Exit Code | Sonuç |
|---|---|---|---:|---|
| TEST-AUTO-XSD-01 | 040 descript (referans) | descript.xsd | 0 | PASS |
| TEST-AUTO-XSD-02 | 420 fault (referans) | fault.xsd | 0 | PASS |
| TEST-AUTO-XSD-03 | 520 proced (referans) | proced.xsd | 0 | PASS |
| TEST-AUTO-XSD-04 | 941 ipd (referans) | ipd.xsd | 0 | PASS |
| TEST-AUTO-XSD-05 | buildImfXml() canlı çıktısı | icnmetadata.xsd | 0 | PASS |
| ek | genDescript/genProced/genIpd canlı çıktıları | ilgili XSD | 0 | PASS |
| ek | genFault() 6 dalı (411/412/413/414/410/420) canlı çıktıları | fault.xsd | 0 | PASS |
| ek | ICN enjekte edilmiş descript/ipd canlı çıktıları | ilgili XSD | 0 | PASS |

**Yöntem notu:** `http://www.w3.org/2001/xml.xsd` bağımlılığı ağ erişimi
olmadığı için yerel XML catalog ile çözüldü (W3C'nin standart, değişmemiş
içeriği kullanıldı — S1000D paketi veya test edilen XML'ler değiştirilmedi).
Kurulumun gerçekten çalıştığı, kasıtlı bozulmuş bir XML ile negatif kontrol
yapılarak kanıtlandı (gerçek FAIL, exit code 3 alındı).

**Bu baseline'dan sonraki oturumlar XSD tarafını yeniden analiz etmemeli**
— XSD doğrulaması bu tarihte PASS olarak kabul edilmiştir. **Ancak bu
baseline, T-007'de eklenen uzunluk kuralını kapsamıyor** — T-007 bu
baseline'dan sonra, farklı bir oturumda, gerçek XSD paketi mevcut değilken
eklendi. Yeni bir kod değişikliği (özellikle content/dmCode/ICN
üretimiyle ilgili) yapılırsa veya T-007 XSD ile çapraz doğrulanacaksa,
regresyon testi resmi XSD paketiyle tekrar çalıştırılmalı.

## ICN Gömme Yöntemi (T-004) — XSD/XML Açısından KAPATILDI ✅
DOCTYPE + ENTITY + NDATA / `graphic/@infoEntityIdent` yaklaşımı, gerçek
S1000D Issue 5.0 XSD incelemesinde `infoEntityIdent`'in `xs:ENTITY` tipinde
olduğu ve mevcut yaklaşımın XSD/XML açısından geçerli olduğu doğrulandı. Bu
artık "doğrulanmamış açık risk" değil. BREX/proje iş kuralı düzeyinde ayrı
bir kısıt çıkarsa (örn. MSB'nin kendi kuralı varsa) bu ayrıca değerlendirilir.

## BREX Doğrulama Durumu — YAKLAŞIM DEĞİŞTİ (D-006)
Önceki oturumlarda S1000D Default BREX (`S1000D-A-...` / `S1000D-G-...`)
kimliği araştırılıyordu. Kullanıcı kararı ile bu yaklaşım değişti: **S1000D
Default BREX artık DERMAN projesinin proje iş kurallarının ana kaynağı
olarak kullanılmayacak.** S1000D 5.0 yalnızca XML/XSD yapısal doğrulama
standardı olarak kullanılmaya devam edecek. Proje iş kuralları için öncelik:
MSB katmanı → MPG/FORSDOC proje girdileri → doğrulanmış proje kararları →
S1000D XSD yapısı.

`S1000D-G-04-10-0301-00A-022A-D` (dosya içeriğiyle S1000D Issue 5.0 default
BREX'i olduğu doğrulanan kimlik) şimdilik yalnızca **referans bilgi**
olarak tutuluyor, koda yansıtılmadı. `SETTINGS.brex` ve 4 referans XML'in
`brexDmRef` değerleri **değiştirilmedi** (hâlâ `S1000D-A-...`).

MSB'ye ait gerçek bir BREX/Business Rules kaynağı sağlanana kadar BREX
konusu **"BEKLEYEN PROJE GİRDİSİ"** olarak tutulmalıdır. Detay: `DECISIONS.md` D-006.

## SNS Sistem Kodu ve MSB Katmanı — Kavramsal Netleştirme (D-007, D-008)
- FORSDOC'taki "SNS Sistem Kodu = 00" ile `dmCode/@systemCode` (örn. "GA1")
  **aynı alan değildir** — biri proje/SNS ağacı konfigürasyonu, diğeri VM
  DMC kodlama yapısının parçası. Mevcut `GA1` örnekleri bu nedenle bir
  çelişki olarak değerlendirilmemeli. Kod değişikliği yapılmadı.
- MSB katmanı (FORSDOC'un S1000D 5.0 → MSB tanımı) uygulamada ayrı bir alan
  gerektirmiyor; MSB'ye özgü gerçek bir iş kuralı kaynağı sağlanmadan
  uygulamaya MSB'ye özgü kod/alan **eklenmeyecek**.
