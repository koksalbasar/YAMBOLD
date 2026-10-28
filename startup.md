# Startup — OpenCode Ekosistemi Kurulum Talimatı

> **Bu belge bir "çalıştırılabilir" kurulum talimatıdır.**
> Yeni bir projede OpenCode oturumu açıp bu dosyanın içeriğini yapıştırdığında,
> sen (AI Orchestrator) aşağıdaki adımları sırayla uygularsın.
>
> **Nerede saklandığı (bu kitin kendisi):** `00_startup/`
> ```
> 00_startup/
> ├── startup.md                    ← bu dosya (ana talimat)
> ├── third-party-manifest.md       ← üçüncü taraf kaynak listesi
> ├── resources/                    ← BİZE AİT kaynakların tam kopyası
> │   ├── 00_SessionBasedWorkflow_Methodology.md
> │   ├── memory-bank.md
> │   └── istekler-listesi-sablon.md
> └── templates/                    ← şablonlar (yöntem; içerik değil)
>     ├── AGENTS.template.md
>     ├── projectbrief.template.md
>     ├── productContext.template.md
>     ├── systemPatterns.template.md
>     ├── techContext.template.md
>     ├── activeContext.template.md
>     ├── progress.template.md
>     └── project.template.md
> ```

---

## 0. Bu Belge Ne Yapar?

Yeni (veya mevcut) bir projede **OpenCode çalışma ekosistemini** kurar:

1. Oturum bazlı faz yönetimi metodolojisi
2. Memory Bank (kalıcı bağlam)
3. `AGENTS.md` (OpenCode'in oturum başında okuduğu ana talimat)
4. Uzman temsilci şeritleri (Oh My OpenCode Slim)
5. Seçilen yetenekler (skills) ve MCP sunucuları
6. AI talimatları (istekler listesi) altyapısı

**Felsefe:** *Yöntem taşınır, içerik değil.* Valura projesinin kodları, metinleri, marka bilgileri ve proje özelindeki talimatları **taşınmaz**. Taşınan şey: çalışma biçimi, klasör konvansiyonu, dosya şablonları ve üçüncü taraf kaynak listesi.

---

## 1. Ön Koşullar

- Proje klasörü **bir git deposu** olarak başlatılmış olmalı (`git init` yapılmış).
- `00_startup/` kiti proje köküne kopyalanmış olmalı (bu dosyanın yanında `resources/`, `templates/`, `third-party-manifest.md` bulunur).
- OpenCode bu proje kökünde açık olmalı.
- İnternet erişimi (üçüncü taraf varlıkları son sürüm çekmek için).

---

## 2. Kurulum Akışı (Sırayla Uygula)

### Adım 0 — Proje Aşamasını Belirle

Kullanıcıya **sor**:

> Bu proje hangi aşamada?
> **(1) Başlangıç aşaması** — henüz kod yok veya çok az (greenfield)
> **(2) İlerlemiş proje** — kod/çerçeve mevcut, üzerine ekosistem kurulacak (brownfield)

Bu seçim, sonraki adımların davranışını belirler:
- **(1) Başlangıç:** Her şey sıfırdan kurulur; çakışma taraması kısa tutulur.
- **(2) İlerlemiş:** Adım 2'deki **mevcut varlık taraması** tam yapılır; hiçbir şey sessizce ezilmez.

---

### Adım 1 — Proje Tespiti

Projenin **tipi, çerçevesi, dili, runtime'ı ve paket yöneticisini** tespit et. Bunun için kökteki işaret dosyalarına bak (bkz. **Bölüm 8 — Tespit Kuralları**).

Tespit edilemeyen / belirsiz kalan bilgileri **kullanıcıya sor** (ör. proje adı, amacı, hedefleri). Tespit sonuçlarını kullanıcıya göster ve onaylat.

---

### Adım 2 — Mevcut Varlık Taraması (yalnızca ilerlemiş proje veya çakışma riski varsa)

Şu varlıkların mevcut olup olmadığını kontrol et:

| Varlık | Kontrol |
|--------|---------|
| `AGENTS.md` | kökte var mı? |
| `.opencode/` (opencode.json, package.json) | var mı? |
| `.project-docs/` (veya eski `docs/`, `Documents/`) | var mı? |
| Memory Bank dosyaları | var mı? |
| `.gitignore` | var mı? |

Mevcut her varlık için kullanıcıya **üç seçenek** sun (önerini belirt):

1. **Atla** — mevcut olanı koru, dokunma (raporla).
2. **Yedekle + üzerine yaz** — önce `_backup/` altına kopyala, sonra yenisiyle değiştir.
3. **Birleştir** — mevcut içerikle yeni şablonu uyumlu şekilde birleştir.

> **ZORUNLU:** Kullanıcı karar vermeden hiçbir mevcut dosya değiştirilmez. Varsayılan davranış asla "sessizce üzerine yazma" değildir.

---

### Adım 3 — Modül Seçimi

Kullanıcıya kurulacak modülleri bir **kontrol listesi** olarak sun (bkz. **Bölüm 6 — Modül Kataloğu**). Zorunlu (core) modüller her zaman kurulur; opsiyonel olanlar kullanıcının seçimine göre.

Her modül için: kısa açıklama + ne yüklediği + (varsa) bağımlılığı. Kullanıcının seçimlerini kaydet ve sonraki adımlarda yalnızca seçilenleri kur.

---

### Adım 4 — Dizin Yapısını Oluştur

**Bölüm 4 — Kanonik Dizin Yapısı**'na göre `.project-docs/` altındaki dizinleri oluştur. Mevcut değilse `.opencode/` klasörünü de oluştur.

Seçilen modüllere göre ilgili alt dizinleri de aç (ör. metodoloji seçilmediyse `00_workflow/` açılması gerekmez; Memory Bank her zaman açılır).

---

### Adım 5 — Üçüncü Taraf Kurulumları

Seçilen üçüncü taraf modüller için `00_startup/third-party-manifest.md` dosyasını oku ve oradaki **Kaynak** bilgilerine göre **en güncel sürümü** kur:

- npm tabanlı → `npm i -D <paket>` (son sürüm otomatik çekilir).
- GitHub tabanlı skill → ilgili repodan istenen `SKILL.md` klasörünü `.opencode/skills/<skill-adı>/` altına kopyala.
- MCP → `opencode.json` içinde gerekli `mcp` bloğunu, resmî kaynaktan doğrulanmış `command`/`args`/`env` ile yaz.

> Kaynak ulaşılamazsa **elle kurulum** alternatifini sun; sessizce eski sürüm kurma.

---

### Adım 6 — Şablonlardan Dosya Üret (Memory Bank)

`00_startup/templates/` içindeki şablonları, Adım 1'de tespit ettiğin + kullanıcıdan aldığın değerlerle doldurup `.project-docs/20_memory-bank/` altına **Markdown dosyaları** olarak yaz (bkz. **Bölüm 9 — Şablon Doldurma Haritası**).

- `{{...}}` yer tutucularını gerçek değerlerle değiştir.
- Bilinmeyen / projede henüz olmayan alanları "henüz tanımlanmadı" veya makul bir iskelet ile bırak; uydurma.
- Dosya adları: şablon adından `.template` çıkar (ör. `projectbrief.template.md` → `projectbrief.md`).
- Not: Bu Markdown dosyaları, LogSeq DB'nin **export** çıktısıyla aynı formattadır; git yedeği + fallback olarak hizmet eder.

---

### Adım 6b — LogSeq Entegrasyonu (Memory Bank canlı katmanı)

`mcp-logseq` modülü seçildiyse (LogSeq 2.0 DB tabanlı Memory Bank için):

1. LogSeq graph içinde `Projects/<ProjeAdı>/` namespace'ini oluştur.
2. `MemoryBank` (hub) + alt sayfaları (`projectbrief`, `productContext`, `systemPatterns`, `techContext`, `activeContext`, `progress`, `project`) MCP ile oluştur.
3. Adım 6'daki Markdown içeriklerini bu sayfalara aktar; sayfaları `[[wikilink]]` ile bağla.
4. Sayfalara **properties ekle (JSON formatında: `tags: ["..."], status: "active", project: "<ProjeAdı>"`)**.
5. `.opencode/opencode.json` içinde `mcp-logseq` konfigürasyonunu doğrula (manifest §3.2).

> **Çift mod:** AI, LogSeq açıkken MCP ile (birincil), kapalıyken `.project-docs/20_memory-bank/` Markdown dosyalarıyla (fallback) Memory Bank'a erişir. Senkronizasyon: **elle** (export → git commit). Otomasyon (git hook) ileride değerlendirilecek — bkz. `resources/memory-bank.md` §6.

---

### Adım 7 — `AGENTS.md` Üret

`AGENTS.template.md`'yi doldurup köke `AGENTS.md` olarak yaz. Bu dosya, projenin OpenCode ana talimatıdır:

- Metodoloji + Memory Bank okuma protokolünü içerir (jenerik).
- Adım 1'de tespit edilen **proje tipi / çerçeve / dil** bilgilerini içerir.
- Proje amacı ve yöntemi kullanıcıdan alınan bilgiyle doldurulur.

Ayrıca `.opencode/opencode.json` içinde `instructions` alanına `AGENTS.md`'ye (ve/veya `.project-docs/10_rules/memory-bank.md`'ye) referans ekle, böylece OpenCode oturum başında doğru talimatı yükler.

---

### Adım 8 — Doğrulama ve Özet

Kurulumu doğrula ve özet çıkar:

1. Dizin yapısı oluştu mu? (`.project-docs/...` ve seçilen modüller)
2. Memory Bank core dosyaları üretildi mi? (7 dosya)
3. `AGENTS.md` kökte var mı?
4. Seçilen üçüncü taraf varlıklar kuruldu mu? (versiyonlarıyla listele)
5. `00_startup/` kitinin akıbeti: kullanıcıya sor — **sakla** (arşiv) mı, **sil** mi?

Kullanıcıya kısa bir **kurulum raporu** göster: ne kuruldu, hangi sürümler, hangi modüller seçildi, hangi çakışmalar nasıl çözüldü.

---

## 3. Definition of Done (Kurulum Başarı Kriteri)

Kurulum şunlar sağlandığında başarılıdır:

- [ ] `.project-docs/` kanonik yapısı kuruldu.
- [ ] Memory Bank 7 core dosyası `.project-docs/20_memory-bank/` altında mevcut.
- [ ] (mcp-logseq seçildiyse) LogSeq graph'ta `Projects/<ProjeAdı>/` namespace'i + MemoryBank sayfaları oluşturuldu.
- [ ] Kökte `AGENTS.md` mevcut ve proje bilgilerini içeriyor.
- [ ] `.opencode/opencode.json` `instructions` referansı doğru.
- [ ] Seçilen üçüncü taraf modüller en güncel sürümle kuruldu.
- [ ] Hiçbir mevcut proje dosyası izinsiz değiştirilmedi.

---

## 4. Kanonik Dizin Yapısı (`.project-docs/`)

> Bu yapı, **bundan sonraki tüm projeler** için standarttır. Bu projedeki eski `docs/` + `Documents/` dağınık yapısının yerine geçer.

```
<proje-kökü>/
├── AGENTS.md                              # OpenCode ana talimatı
├── .opencode/
│   ├── opencode.json                      # instructions + plugin + mcp konfigürasyonu
│   ├── package.json                       # (varsa) npm dev bağımlılıkları
│   └── skills/                            # kurulan skill'ler (seçime göre)
├── .project-docs/
│   ├── 00_workflow/                       # Oturum bazlı faz yönetimi metodolojisi
│   │   └── 00_SessionBasedWorkflow_Methodology.md
│   ├── 10_rules/                          # Kurallar (bakım/naming kuralları)
│   │   └── memory-bank.md
│   ├── 20_memory-bank/                    # Kalıcı hafıza (core dosyalar)
│   │   ├── projectbrief.md
│   │   ├── productContext.md
│   │   ├── systemPatterns.md
│   │   ├── techContext.md
│   │   ├── activeContext.md
│   │   ├── progress.md
│   │   └── project.md
│   ├── 30_ai-prompts/                     # AI talimatları / görev klasörleri
│   │   ├── istekler-listesi-sablon.md
│   │   └── <is-konu-slug>/                # her iş için ayrı klasör
│   ├── 40_project/                        # Proje dokümanları (analiz, iş planı, toplantı notu, tasarım)
│   └── 50_helpers/                        # Yardımcı rehberler / dönüşüm referansları
└── (proje kodu)
```

**Numaralandırma mantığı:** `00` temel metodoloji, sonrası 10'ar boşluklu. Araya yeni kategori gerektiğinde (`05_`, `15_` gibi) eklenebilir.

---

## 5. Dosya ve Klasör İsimlendirme Kuralları

| Öğe | Kural | Örnek |
|-----|-------|-------|
| Görev klasörü | `[YYMMDD]-[NN]-[is-konu-slug]` — `NN` gün içindeki görev sayısı (`01`, `02`, …), her gün sıfırlanır | `261025-01-RaporEntegrasyonu` |
| Faz dosyası | `Faz_[NN]_[FazAdi]_Prompt.md` — `NN` görev içi sıra (`01`, `02`, …), her görevde sıfırlanır | `Faz_01_Altyapi_Prompt.md` |
| İlerleme / devir | `CHECKPOINT.md` — aktif ilerleme, her faz sonunda güncellenir | `CHECKPOINT.md` |
| Kapanış kanıtı | `COMPLETED.md` — tüm fazlar onaylanınca oluşur | `COMPLETED.md` |
| Müşteri isteği | `Kullanici-Istekleri.md` (veya `istekler-listesi-sablon.md`'ye göre) | `Kullanici-Istekleri.md` |

> Not: Metodoloji dokümanı (`.project-docs/00_workflow/00_SessionBasedWorkflow_Methodology.md`) içindeki dizin yapısı buradaki **kanonik `.project-docs/` yapısıyla** eşleşir. Metodolojinin içeriği (roller, fazlar, kurallar) metodoloji sahibi (Can) tarafından yönetilir — kurulum sırasında içeriğini değiştirme.

---

## 6. Modül Kataloğu

### Zorunlu (Core) — her zaman kurulur

| Modül | Ne kurar |
|-------|----------|
| `core-directory` | `.project-docs/` kanonik dizin yapısı |
| `core-agents` | Kökte `AGENTS.md` |
| `core-memory-bank` | `.project-docs/20_memory-bank/` 7 core dosyası (şablonlardan; LogSeq DB canlı katman + Markdown export yedeği) |
| `core-rules` | `.project-docs/10_rules/memory-bank.md` |

### Opsiyonel — kullanıcı seçer

| Modül | Ne kurar | Kaynak |
|-------|----------|--------|
| `workflow-methodology` | `.project-docs/00_workflow/00_SessionBasedWorkflow_Methodology.md` | `resources/` (bize ait) |
| `ai-prompts-infra` | `.project-docs/30_ai-prompts/istekler-listesi-sablon.md` + görev klasörü konvansiyonu | `resources/` (bize ait) |
| `plugin-orchestrator` | `oh-my-opencode-slim` (specialist lanes) | manifest §1.1 |
| `skill-frontend-design` | `frontend-design` skill'i | manifest §2.1 |
| `skill-superpowers` | `web-clone`, `visual-verdict` ve seçilen diğer superpowers skill'leri | manifest §2.2 |
| `mcp-radzen` | Radzen Blazor Studio MCP | manifest §3.1 |
| `mcp-logseq` | LogSeq MCP (Memory Bank'ın canlı sorgu/yazma katmanı) | manifest §3.2 |
| `mcp-stitch` | Stitch MCP | manifest §3.3 |

> `skill-superpowers` modülü seçildiğinde, superpowers'ın tüm skill'lerini değil; kullanıcıya **hangilerini** istediğini ayrıca sor (`web-clone`, `visual-verdict`, `test-driven-development`, vb.).

---

## 7. Üçüncü Taraf Manifest

Üçüncü taraf varlıkların kaynak listesi ve kurulum yöntemleri `00_startup/third-party-manifest.md` dosyasındadır. Kurulumda bu dosyayı oku ve oradaki kurallara uy (en güncel sürüm çek).

---

## 8. Proje Tespit Kuralları

Kökte şu işaret dosyalarını ara ve eşle:

| İşaret dosyası | Dil / Çerçeve | Not |
|----------------|---------------|-----|
| `*.sln`, `*.csproj` | C# / .NET | `*.razor` dosyaları → Blazor |
| `package.json` | JavaScript/TypeScript | `next.config.*` → Next.js; `vite.config.*` → Vite; `angular.json` → Angular; `tsconfig.json` → TypeScript |
| `pyproject.toml`, `requirements.txt`, `setup.py` | Python | `pyproject.toml` içinde framework (FastAPI/Django/Flask) |
| `go.mod` | Go | |
| `Cargo.toml` | Rust | |
| `pom.xml`, `build.gradle*` | Java / Kotlin | |
| `composer.json` | PHP | |
| `Gemfile` | Ruby | |
| `Dockerfile`, `docker-compose*.yml` | (altyapı ipucu) | |
| `.git` dizini | git repo var | ön koşul |

**Paket yöneticisi tespiti:** `package.json` → npm; `pnpm-lock.yaml` → pnpm; `yarn.lock` → yarn; `*.csproj` → NuGet/dotnet; `go.mod` → go modules; vb.

**Tespit sonucunu** şu alanlara yaz:
- Proje tipi (web/API/kütüphane/CLI/masaüstü/mobil)
- Çerçeve (framework) + versiyonu (mümkünse manifest'ten)
- Dil + versiyonu
- Runtime + paket yöneticisi
- Kurulum/derleme/çalıştırma komutları (paket yöneticisinden türet)

---

## 9. Şablon Doldurma Haritası

| Tespit / Kullanıcı bilgisi | Nereye yazılır |
|----------------------------|----------------|
| Proje adı | `AGENTS.md`, `projectbrief.md`, `project.md` (`{{PROJE_ADI}}`) |
| Proje açıklaması / amacı | `AGENTS.md`, `projectbrief.md`, `productContext.md` |
| Proje tipi | `AGENTS.md` (`{{PROJE_TIPI}}`), `techContext.md` |
| Çerçeve + versiyon | `AGENTS.md` (`{{CERCEVE}}`), `techContext.md`, `projectbrief.md` |
| Dil + versiyon | `AGENTS.md` (`{{DIL}}`), `techContext.md` |
| Runtime / paket yöneticisi | `AGENTS.md` (`{{RUNTIME}}`), `techContext.md` |
| Kurulum/build/run komutları | `techContext.md` (Development Setup) |
| Mimari / bileşen ilişkileri | `systemPatterns.md` (proje büyüdükçe doldurulur; başta iskelet) |
| Tarih / son değişiklik | `activeContext.md` |
| Durum özeti | `progress.md` |

> Tespit edilemeyen alanları boş bırak ve ilgili satıra `(henüz tanımlanmadı)` yaz; tahminle doldurma.

---

## 10. Güvenlik ve İdempotency Kuralları

1. **Asla sessizce üzerine yazma.** Mevcut dosya varsa Adım 2'deki karar akışını uygula.
2. **Yedekleme:** "Üzerine yaz" seçilirse, dosya değiştirilmeden önce `.project-docs/50_helpers/_backup/<zaman-damgası>/` altına kopyala.
3. **Proje bazlı izolasyon:** Kurulum yalnızca mevcut proje kökünde çalışır; başka projelere veya global OpenCode yapılandırmasına dokunmaz.
4. **Geri alınabilirlik:** Her üçüncü taraf kurulumda, kaldırma yöntemini (ör. `npm rm`, klasör silme, `opencode.json` mcp bloğu kaldırma) özete yaz.
5. **Önceden başlatılmış projeler bozulmaz:** Bu kit bir projeye kurulurken o projenin mevcut `.opencode/`, `AGENTS.md`, dokümanları korunur (Adım 2 kararlarıyla).
