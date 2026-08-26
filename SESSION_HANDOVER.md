# SESSION_HANDOVER.md — Oturum Devir Teslimi

## Bu Oturumda Yapılanlar
1. Repo'daki 5 ana dosya + 4 örnek DMC XML incelendi (GitHub Project Context
   üzerinden, `project_knowledge_search` ile — dosyalar uploads klasöründe
   değil, repo bağlantısı üzerinden okundu).
2. 4. örnek DMC XML (`..._040A-A_descript.xml`) erişimi ayrıca doğrulandı.
3. Tam teknik devralma analizi yapıldı (amaç, mimari, şema desteği, DMC/ICN/IMF
   mantığı, XSD/BREX durumu, offline durum, güçlü yönler, riskler, referans
   XML ile kod arasındaki uyumsuzluklar).
4. Kullanıcı analiz üzerinde 2 düzeltme talep etti:
   - ZWSP şüphesi risk olarak kaydedilmeyecek (ham dosyada doğrulanmadı).
   - `systemCode` üretim tutarsızlığı (parts[2] vs malzKat+sysCode) açık,
     öncelikli teknik risk olarak kayıt altına alınacak.
5. Bu 5 hafıza dosyası oluşturuldu: `CLAUDE.md`, `PROJECT_STATUS.md`,
   `DECISIONS.md`, `TODO.md`, `SESSION_HANDOVER.md`.

## Bu Oturumda YAPILMAYANLAR (kasıtlı)
- `Koluman_S1000D_Veri_Modulu_Uretici.html` dosyasına **hiç dokunulmadı**.
- Hiçbir kod değişikliği, düzeltme veya yeni özellik eklenmedi.
- Bu oturum yalnızca analiz + hafıza dosyası üretimiydi.

## Bir Sonraki Oturum Nereden Başlamalı
1. Önce `CLAUDE.md` → `PROJECT_STATUS.md` → `DECISIONS.md` sırasıyla oku.
2. `TODO.md`'deki T-001 (systemCode tutarlılığı) **ilk ele alınacak konu** —
   kod değişikliğinden önce test/doğrulama gerektirir.
3. Kod değişikliği yapılacaksa: önce `Koluman_S1000D_Veri_Modulu_Uretici.html`
   dosyasının ilgili fonksiyonu (`identAndStatus()`, `parseDmcString()`)
   tekrar okunmalı — bu dosyalar değişikliği değil, bağlamı taşır.
4. Yeni bir teknik risk/karar bulunursa `DECISIONS.md`'ye eklenmeli, varsayıma
   dayanan şüpheler risk olarak değil, "doğrulanmamış nokta" olarak
   `PROJECT_STATUS.md`'ye eklenmeli.

## Devir Teslim Kuralı
Bir bulgu ancak kaynak dosya üzerinde doğrudan doğrulanmışsa `DECISIONS.md`'ye
"teknik risk" olarak yazılır. Doğrulanamayan şüpheler ayrı tutulur ve risk
listesini şişirmez.
