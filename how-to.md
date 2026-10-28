# Yeni Proje için LogSeq MemoryBank Kurulum Kılavuzu

> Bu belge, `docs/00_startup/` kitini kullanarak **yeni bir projeye** (greenfield) LogSeq 2.0 DB tabanlı MemoryBank + OpenCode ekosistemini nasıl kuracağınızı adım adır açıklar.

---

## 1. Ön Hazırlıklar (Bir Kere Yapılır)

### 1.1. LogSeq 2.0 Kurulu Olmalı
- [LogSeq indir](https://logseq.com/) → v2.0+ (DB mimarisi)
- LogSeq'i açın, bir **graph** oluşturun veya mevcut graph'ınızı kullanın
- Graph konumu: `~\logseq\graphs\<graph-adi>\` (yerel disk)
- **Önemli:** LogSeq açıkken MCP çalışır; kapalıyken Markdown fallback kullanılır.

### 1.2. OpenCode Kurulu Olmalı
```bash
# OpenCode kurulumu (yoksa)
npm i -g @opencode-ai/cli
```

### 1.3. Git Reposu Hazır
```bash
mkdir yeni-proje && cd yeni-proje
git init
```

---

## 2. Kit Dosyalarını Kopyalama

ValuraWeb'den `docs/00_startup/` klasörünü **yeni projenizin köküne** kopyalayın:

```bash
# ValuraWeb klonundaysanız:
cp -r /path/to/ValuraWeb/docs/00_startup ./00_startup

# Veya GitHub'dan indirip açtıysanız:
# 00_startup/ klasörü proje kökünde olmalı
```

**Kopyalanacak yapı:**
```
yeni-proje/
├── 00_startup/
│   ├── startup.md                    # Ana kurulum talimatı (AI'ya vereceksiniz)
│   ├── third-party-manifest.md       # 3. taraf kaynak listesi
│   ├── resources/
│   │   ├── 00_SessionBasedWorkflow_Methodology.md
│   │   ├── memory-bank.md
│   │   └── istekler-listesi-sablon.md
│   └── templates/
│       ├── AGENTS.template.md
│       ├── projectbrief.template.md
│       ├── productContext.template.md
│       ├── systemPatterns.template.md
│       ├── techContext.template.md
│       ├── activeContext.template.md
│       ├── progress.template.md
│       └── project.template.md
```

---

## 3. Kurulumu Başlatma (OpenCode Oturumunda)

### 3.1. OpenCode'i Proje Kökünde Açın
```bash
cd yeni-proje
opencode
```

### 3.2. AI'ya `startup.md`'yi Verin
OpenCode sohbet penceresine **`00_startup/startup.md` dosyasının tamamını yapıştırın**.

> **Ne olacak:** AI (Orchestrator) `startup.md` içindeki 10 adımlı prosedürü yürütecek. Sizden onay/karar alacağı yerlerde soru soracak.

---

## 4. AI Sizden İsteyeceği Bilgiler (Hazır Olun)

Kurulum sırasında AI şu bilgileri soracak — önceden hazırlayın:

| Bilgi | Örnek |
|---------|-------|
| **Proje adı** | `AcmeAnalytics` |
| **Proje tipi** | `Web uygulaması` / `API` / `Kütüphane` / `CLI` |
| **Çerçeve (framework)** | `Blazor Server .NET 10` / `Next.js 15` / `FastAPI` |
| **Programlama dili** | `C#` / `TypeScript` / `Python` |
| **Runtime / paket yöneticisi** | `.NET 10 / NuGet` / `Node 22 / npm` |
| **Proje amacı (1-2 cümle)** | `Şirket içi analitik dashboard'u` |
| **Temel hedefler** | `1. Gerçek zamanlı veri 2. Çoklu kiracı 3. Offline-first` |

---

## 5. AI Karşınızda Yürecek Adımlar (Ne Olacak)

### Adım 0 — Proje Aşaması
> "Bu proje başlangıç mı (greenfield) yoksa ilerlemiş mi (brownfield)?"

**Cevap:** `Başlangıç` (yeni proje için).

### Adım 1 — Proje Tespiti
AI kökteki dosyalara bakarak çerçeve/dili tespit eder. Eksikse sizden ister.

### Adım 2 — Çakışma Kontrolü
Yeni projede dosya yok → atlanır.

### Adım 3 — Modül Seçimi (Siz Seçersiniz)
AI bir checklist sunar. **Zorunlular** her zaman kurulu; opsiyonelleri işaretlersiniz:

| Modül | Açıklama | Seç? |
|-------|----------|------|
| `core-directory` | `.project-docs/` klasör yapısı | ✅ Zorunlu |
| `core-agents` | Kök `AGENTS.md` | ✅ Zorunlu |
| `core-memory-bank` | 7 Memory Bank dosyası (Markdown export) | ✅ Zorunlu |
| `core-rules` | `memory-bank.md` kural dosyası | ✅ Zorunlu |
| `workflow-methodology` | Oturum bazlı faz metodolojisi | ✅ Önerilir |
| `ai-prompts-infra` | İstekler listesi şablonu + görev klasörü | ✅ Önerilir |
| `plugin-orchestrator` | `oh-my-opencode-slim` (specialist lanes) | ✅ Önerilir |
| `mcp-logseq` | **LogSeq MCP** (Memory Bank canlı katmanı) | ✅ **LogSeq kullanıyorsanız Evet** |
| `skill-frontend-design` | Görsel tasarım skill'i | İsterseniz |
| `skill-superpowers` | Web clone, visual verdict, TDD vb. | İsterseniz |
| `mcp-radzen` | Radzen Blazor Studio MCP | Blazor ise |
| `mcp-stitch` | Stitch MCP | Tasarım-kod akışı varsa |

### Adım 4 — Dizin Yapısı Oluşturulur
AI `.project-docs/` altındaki tüm klasörleri oluşturur.

### Adım 5 — 3. Taraf Kurulumları
Seçtiğiniz modüller için:
- `npm i -D oh-my-opencode-slim` (plugin)
- Skill klasörleri `.opencode/skills/` altına kopyalanır
- MCP konfigürasyonu `.opencode/opencode.json`'a yazılır

### Adım 6 — Memory Bank Dosyaları Üretilir (Markdown Export)
7 şablon (`templates/*.template.md`) doldurularak `.project-docs/20_memory-bank/` altına yazılır:
- `projectbrief.md`, `productContext.md`, `systemPatterns.md`, `techContext.md`, `activeContext.md`, `progress.md`, `project.md`

### Adım 6b — LogSeq Entegrasyonu (mcp-logseq Seçildiyse)
**LogSeq AÇIK OLMALI.** AI şunları yapar:
1. LogSeq graph'ta `Projects/<ProjeAdı>/` namespace'ini oluşturur
2. `MemoryBank` (hub) + 7 alt sayfayı MCP ile oluşturur
3. Markdown içeriklerini sayfalara aktarır
4. Sayfaları `[[wikilink]]` ile bağlar
5. Sayfalara **properties** ekler (JSON formatında: `tags: ["blazor", "sqlite"]`, `status: "active"`, `project: "<ProjeAdı>"`)

> **Not:** Bu adım LogSeq açıkken çalışır. Kapalıysa sadece Markdown dosyaları üretilir (fallback).

### Adım 7 — `AGENTS.md` Üretilir
Kök dizine `AGENTS.md` yazılır. İçerisinde:
- Metodoloji + Memory Bank protokolü (MCP + fallback)
- Proje bilgileri (Adım 1'de toplananlar)
- Proje amacı/yöntemi

### Adım 8 — Doğrulama ve Özet
AI kurulum raporu verir:
- Ne kuruldu, hangi sürümler
- LogSeq namespace'i
- `00_startup/` kitini saklamak mı silmek mi? (Öneri: saklayın)

---

## 6. Kurulum Sonrası — Ne Elde Edersiniz?

### Proje Yapısı
```
yeni-proje/
├── AGENTS.md                              # OpenCode ana talimatı
├── .opencode/
│   ├── opencode.json                      # Plugin + MCP konfigürasyonu
│   ├── package.json                       # npm dev bağımlılıkları
│   └── skills/                            # Kurulan skill'ler
├── .project-docs/
│   ├── 00_workflow/
│   │   └── 00_SessionBasedWorkflow_Methodology.md
│   ├── 10_rules/
│   │   └── memory-bank.md
│   ├── 20_memory-bank/                    # Markdown export (fallback + git)
│   │   ├── projectbrief.md
│   │   ├── productContext.md
│   │   ├── systemPatterns.md
│   │   ├── techContext.md
│   │   ├── activeContext.md
│   │   ├── progress.md
│   │   └── project.md
│   ├── 30_ai-prompts/
│   │   ├── istekler-listesi-sablon.md
│   │   └── <is-konu-slug>/                # Gelecek görevler için
│   ├── 40_project/                        # Proje dokümanları
│   └── 50_helpers/                        # Yardımcılar
└── (proje kodunuz)
```

### LogSeq Graph'ta (Canlı Katman)
```
Projects/<ProjeAdı>/
├── MemoryBank
├── projectbrief
├── productContext
├── systemPatterns
├── techContext
├── activeContext
├── progress
└── project
```
Tümü `[[wikilink]]` ile birbirine bağlı, **properties (JSON: `tags: ["..."], status: "active", project: "<ProjeAdı>"`)** ile etiketlenmiş.

---

## 7. Günlük Kullanım (AI ile Çalışırken)

### Yeni Oturum Başladığında
1. OpenCode açın
2. AI otomatik `AGENTS.md` okur
3. **Memory Bank okuma:**
   - **LogSeq açıksa:** AI MCP ile `get_page_content` + `get_page_backlinks` kullanır (grafik sorgusu, backlink keşfi)
   - **LogSeq kapalıysa:** `.project-docs/20_memory-bank/` Markdown dosyalarını okur

### İş Bitiminde (Memory Bank Güncelleme)
1. AI değişiklikleri **MCP ile LogSeq DB'ye** yazar (`update_block` / `create_block`)
2. **Siz** LogSeq'ten **Markdown export** alırsınız: `Dosya > Export > Markdown` (veya `Ctrl+E`)
3. Export edilen dosyaları `.project-docs/20_memory-bank/` içine atarsınız
4. `git add . && git commit -m "MemoryBank sync: <açıklama>"`

> **Hatırlatma:** Otomasyon (git hook) ileride eklenebilir — bkz. `resources/memory-bank.md` §6.

---

## 8. Sık Karşılaşılan Sorunlar

| Sorun | Çözüm |
|-------|-------|
| `mcp-logseq` çalışmıyor | LogSeq masaüstü uygulaması **açık** olmalı. Port (genelde 12315) erişilebilir olmalı. |
| LogSeq graph bulunamıyor | `.opencode/opencode.json` içindeki `mcp-logseq` konfigürasyonunda `graph` adının doğru olduğundan emin olun. |
| MCP timeout | LogSeq DB'si büyükse ilk sorguda yavaş olabilir. Tekrar deneyin. |
| Markdown export eski | Export'ı manuel yapmayı unutmayın. `git diff .project-docs/20_memory-bank/` ile kontrol edin. |

---

## 9. Tek Komutla Özet (Deneyimliyseniz)

```bash
# 1. Proje oluştur
mkdir yeni-proje && cd yeni-proje && git init

# 2. Kit kopyala
cp -r /path/to/ValuraWeb/docs/00_startup ./00_startup

# 3. LogSeq'i aç (graph seçili olsun)

# 4. OpenCode başlat
opencode

# 5. AI sohbetine 00_startup/startup.md'yi yapıştır
# 6. Sorduğu bilgileri ver, modül seç, bekle

# 7. Bittiğinde:
#    - LogSeq'te Projects/<ProjeAdı>/ kontrol et
#    - .project-docs/20_memory-bank/ git commit et
```

---

## 10. Kitin Akıbeti

Kurulum bittikten sonra `00_startup/` klasörünü:
- **Saklayın (önerilen):** Gelecek projelerde tekrar kullanırsınız; versiyon kontrolüne de koyabilirsiniz.
- **Silin:** Tek kullanımlıksa `rm -rf 00_startup`.

---

## Ek: LogSeq Export Nasıl Alınır?

1. LogSeq açın, graph seçili olsun
2. `Dosya` > `Export` > `Markdown files`
3. Çıktı klasörü: `.project-docs/20_memory-bank/` (veya istediğiniz yer)
4. Dosyaları taşıyın/kopyalayın → `git add && git commit`

> LogSeq 2.0'da export, DB'den güncel Markdown üretir. Bu dosyalar **sadece yedek/fallback** için git'te durur; canlı kaynak LogSeq DB'sidir.

---

**Hazırlayan:** ValuraWeb ekibi — `docs/00_startup/` kiti  
**Tarih:** 2026-10-28  
**Sürüm:** 1.0