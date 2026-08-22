# CLAUDE.md — Easydict (fork)

## Önce bunu oku
Depoda **`AGENTS.md` var ve bağlayıcıdır** (proje kuralları, yapı, build komutları). Tekrarlamıyorum. Ayrıca `MIGRATION_PROGRESS.md` (Objective-C → Swift geçişi) ve `docs/` güncel durumu taşır.

## Ne bu
`tisfeng/Easydict` fork'u — macOS sözlük/çeviri uygulaması. Uzak: `fork` → `ResulKarahasanMD/Easydict`. Aktif dal `dev`.

Xcode projesi + **CocoaPods** (`Podfile`, `Pods/`) + Swift Package karışımı. SwiftLint + SwiftFormat yapılandırılmış. Sparkle ile güncelleme (`appcast.xml`).

## Dikkat
- Bu makinede **Xcode bozuk**; `AGENTS.md` içindeki `arch -arm64 env DEVELOPER_DIR=...` geçici çözümünü uygulamadan `xcodebuild` çağırma. Çalışmazsa kullanıcıya bildir, kendi başına Xcode kurulumunu değiştirme (`xcode-select` sudo ister).
- `Pods/` üretilmiş — elle düzenleme; `Podfile` değiştir, `pod install` çalıştır.
- Upstream aktif bir proje: değişikliği upstream stiline uydur, `.swiftlint.yml` / `.swiftformat` kurallarını gevşetme.
- `origin` yok, yalnız `fork` remote'u var. Push hedefini varsayma.
- 804 MB. `Pods/` ve türetilmiş veri disk yer; temizlik gerekirse önce sor.
