# CLAUDE.md — Easydict (fork)

## Önce bunu oku
Depoda **`AGENTS.md` var ve bağlayıcıdır** (proje kuralları, yapı, build komutları). Tekrarlamıyorum. Ayrıca `MIGRATION_PROGRESS.md` (Objective-C → Swift geçişi) ve `docs/` güncel durumu taşır.

## Ne bu
`tisfeng/Easydict` fork'u — macOS sözlük/çeviri uygulaması. Aktif dal `dev`.

Xcode projesi + **CocoaPods** (`Podfile`, `Pods/`) + Swift Package karışımı. SwiftLint + SwiftFormat yapılandırılmış. Sparkle ile güncelleme (`appcast.xml`).

## Dikkat
- Xcode 27.0 (27A266a) çalışıyor; `xcodebuild` doğrudan çağrılır, `DEVELOPER_DIR` gerekmez (doğrulandı 2026-09-30). Build komutları için `docs/agents/build-and-test.md`. Çalışmazsa kullanıcıya bildir, kendi başına Xcode kurulumunu değiştirme (`xcode-select` sudo ister).
- `Pods/` üretilmiş — elle düzenleme; `Podfile` değiştir, `pod install` çalıştır.
- Upstream aktif bir proje: değişikliği upstream stiline uydur, `.swiftlint.yml` / `.swiftformat` kurallarını gevşetme.
- İki remote var: `origin` → `tisfeng/Easydict` (upstream), `fork` → `ResulKarahasanMD/Easydict`. Push hedefini varsayma; upstream'e push etme.
- 804 MB. `Pods/` ve türetilmiş veri disk yer; temizlik gerekirse önce sor.
