# YAMBOLD — Yet Another Memory Bank & OpenCode Logic Database

> **Yöntem taşınır, içerik değil.** Bu depo, OpenCode için sessizce çalışan, sürdürülebilir bir **Memory Bank + Oturum Bazlı Faz Yönetimi** ekosistemini kurar.

---

## 📦 Bu Depo Ne İçin?

Bu depo, **herhangi bir yeni (veya mevcut) projeye** şunları tek seferde kurmak için hazır bir **başlangıç kiti (startup kit)** sağlar:

| Bileşen | Ne Yapar |
|---------|----------|
| **Memory Bank** | 7 core dosya (`projectbrief`, `productContext`, `systemPatterns`, `techContext`, `activeContext`, `progress`, `project`) — LogSeq 2.0 DB canlı katmanı + Markdown export yedeği |
| **Oturum Bazlı Faz Yönetimi** | Görev → Faz → Checkpoint → Completed döngüsü; her oturum kendi bağlamını taşır |
| **AGENTS.md** | OpenCode oturum başında okuyacağı ana talimat (metodoloji + Memory Bank protokolü) |
| **Specialist Lanes (oh-my-opencode-slim)** | `@Slim-Orchestrator`, `@Slim-Explorer`, `@Slim-Fixer`, `@Slim-Designer`, `@Slim-Oracle`, `@Slim-Librarian` |
| **Skills** | `frontend-design`, `superpowers` (web-clone, visual-verdict, TDD, vb.) |
| **MCP Entegrasyonları** | `mcp-logseq` (Memory Bank canlı katmanı), `mcp-radzen` (Blazor), `mcp-stitch` (design-to-code) |

---

## 🚀 Hızlı Başlangıç (Yeni Proje İçin)

> **Zorunlu Kurallar:**
> - Kurulum sırasında tüm kullanıcı seçimleri **seçim kutuları (`( )` radyo / `[ ]` onay kutusu)** ile yapılır.
> - Tespit edilen proje bilgileri ve altyapı **otomatik tespit edilse bile kullanıcıya teyit ettirilir**.
> - Seçilen MCP sunucuları için **API Key, URL, port, graph adı gibi gereksinimler kullanıcıya tek tek sorulur**.

```bash
# 1. Proje klasörü oluştur ve git init
mkdir yeni-proje && cd yeni-proje && git init

# 2. Bu depoyu klonla (veya 00_startup/ klasörünü kopyala)
git clone https://github.com/koksalbasar/YAMBOLD.git 00_startup

# 3. LogSeq'i aç (graph seçili olsun) — mcp-logseq kullanacaksanız

# 4. OpenCode başlat
opencode

# 5. AI sohbetine startup.md'yi yapıştır
#    (OpenCode penceresine 00_startup/startup.md dosyasının içeriğini yapıştırın)

# 6. AI size soracak:
#    - Proje adı, tipi, çerçeve, dil, amacı
#    - Hangi modülleri istediğiniz (checklist)
#    Kurulum otomatik tamamlanır.

# 7. Bittiğinde:
#    - LogSeq'te projenin MemoryBank sayfalarını (`project::` / `parent::` ilişkili) kontrol edin
#    - .project-docs/20_memory_bank/ git commit edin
```

---

## 📁 Depo Yapısı

```
YAMBOLD/
├── startup.md                      # Ana kurulum talimatı (AI'ya verilecek)
├── third-party-manifest.md         # 3. taraf kaynak listesi (sürümler her kurulumda güncel çekilir)
├── how-to.md                       # Kullanım kılavuzu (insan için)
├── resources/
│   ├── 00_SessionBasedWorkflow_Methodology.md  # Metodoloji (bize ait)
│   ├── memory-bank.md                            # Memory Bank kuralları (bize ait)
│   └── istekler-listesi-sablon.md                # AI talimat şablonu (bize ait)
└── templates/
    ├── AGENTS.template.md
    ├── projectbrief.template.md
    ├── productContext.template.md
    ├── systemPatterns.template.md
    ├── techContext.template.md
    ├── activeContext.template.md
    ├── progress.template.md
    └── project.template.md
```

---

## 🧩 Modül Kataloğu (Kurulumda Seçilebilir)

### Zorunlu (Core) — Her Zaman Kurulur
| Modül | Açıklama |
|-------|----------|
| `core-directory` | `.project-docs/` kanonik dizin yapısı |
| `core-agents` | Kök `AGENTS.md` |
| `core-memory-bank` | 7 Memory Bank dosyası (Markdown export) |
| `core-rules` | `memory-bank.md` kural dosyası |

### Opsiyonel — Kullanıcı Seçer
| Modül | Açıklama | Ne Zaman |
|-------|----------|----------|
| `workflow-methodology` | Oturum bazlı faz metodolojisi | ✅ Önerilir |
| `ai-prompts-infra` | İstekler listesi + görev klasörü | ✅ Önerilir |
| `plugin-orchestrator` | `oh-my-opencode-slim` (specialist lanes) | ✅ Önerilir |
| `mcp-logseq` | **LogSeq MCP** (canlı Memory Bank) | LogSeq kullanıyorsanız |
| `skill-frontend-design` | Görsel tasarım skill'i | Frontend varsa |
| `skill-superpowers` | Web clone, visual verdict, TDD, vb. | İsterseniz |
| `mcp-radzen` | Radzen Blazor Studio MCP | Blazor projelerde |
| `mcp-stitch` | Stitch MCP (design-to-code) | Tasarım-kod akışı varsa |

---

## 📚 Kaynaklar ve Referanslar

| Kaynak | Link |
|--------|------|
| **oh-my-opencode-slim** | https://github.com/alvinunreal/oh-my-opencode-slim |
| **anthropics/skills (frontend-design)** | https://github.com/anthropics/skills |
| **obra/superpowers** | https://github.com/obra/superpowers |
| **LogSeq MCP** | LogSeq topluluk eklentisi |
| **Radzen MCP** | https://www.radzen.com/ |
| **Stitch MCP** | https://stitch.so/ |

---

## 🔧 Kurulum Sonrası Elde Edilecek Yapı

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

**LogSeq Graph'ta (canlı katman, mcp-logseq seçildiyse):**
```
MemoryBank (hub page)
├── projectbrief
├── productContext
├── systemPatterns
├── techContext
├── activeContext
├── progress
└── project
```
Tümü `[[wikilink]]` ile bağlı, **properties** (`project:: [[ProjeAdı]]`, `parent:: [[MemoryBank]]`) ile etiketlenmiş.

---

## 📝 Günlük Kullanım

### Yeni Oturum Başladığında
1. `opencode` açın
2. AI otomatik `AGENTS.md` okur
3. **Memory Bank okuma:**
   - **LogSeq açıksa:** MCP ile grafik sorgusu + backlink keşfi
   - **LogSeq kapalıysa:** `.project-docs/20_memory-bank/` Markdown dosyaları

### İş Bitiminde (Memory Bank Güncelleme)
1. AI değişiklikleri **MCP ile LogSeq DB'ye** yazar
2. **Siz** LogSeq'ten **Markdown export** alırsınız (`Dosya > Export > Markdown` / `Ctrl+E`)
3. Export edilen dosyaları `.project-docs/20_memory-bank/` içine atarsınız
4. `git add . && git commit -m "MemoryBank sync: <açıklama>"`

---

## ⚠️ Önemli Notlar

- **Sürümler sabit değil:** `third-party-manifest.md` içindeki `last-known-version` alanları yalnızca referanstır. Her kurulumda **en güncel sürüm** kaynaktan çekilir.
- **LogSeq açık olmalı:** `mcp-logseq` çalışması için LogSeq masaüstü uygulaması açık olmalıdır (port 12315).
- **Kitin akıbeti:** Kurulum bittikten sonra `00_startup/` klasörünü saklayın (gelecek projelerde tekrar kullanırsınız) veya silin.

---

## 📄 Lisans

Bu startup kiti **MIT Lisansı** altındadır. Üçüncü taraf varlıkların (oh-my-opencode-slim, anthropics/skills, obra/superpowers, MCP sunucuları) kendi lisansları geçerlidir.

---

## 🤝 Katkı

Bu bir **şablon/kit deposudur** — doğrudan buraya PR atmak yerine, bu kiti kullanarak kendi projenizi kurun ve orada geliştirin. Kit zelfindeki bir hata bulursanız issue açabilirsiniz.

---

**Hazırlayan:** [koksalbasar](https://github.com/koksalbasar)  
**Tarih:** 2026-10-28  
**Sürüm:** 1.0