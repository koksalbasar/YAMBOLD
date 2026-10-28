# Üçüncü Taraf Kaynak Manifesti

> Bu dosya, `startup.md` tarafından **kurulum anında** okunur.
> Burada yalnızca **bize ait olmayan** (üçüncü taraf) varlıklar listelenir.
> Bize ait kaynakların tam kopyası `resources/` klasöründedir; bu listeye girmez.
>
> **Kural:** Her kurulumda, listedeki varlığın **en güncel sürümü** kaynağından (GitHub / npm / resmî web sitesi) çekilir. Aşağıdaki `last-known-version` alanları yalnızca referanstır; bağlayıcı değildir. Kurulum anında gerçek son sürüm doğrulanır (gerekirse `@Slim-Librarian` ile).

---

## 1. Eklentiler (Plugins)

### 1.1. oh-my-opencode-slim
- **Tür:** OpenCode eklentisi (agent orchestration / specialist lanes)
- **Açıklama:** Slim uzman temsilci şeritleri (`@Slim-Orchestrator`, `@Slim-Explorer`, `@Slim-Fixer`, `@Slim-Designer`, `@Slim-Oracle`, `@Slim-Librarian`) sağlayan, `oh-my-opencode`'un sadeleştirilmiş fork'u.
- **Kaynak (GitHub):** https://github.com/alvinunreal/oh-my-opencode-slim
- **npm:** `oh-my-opencode-slim`
- **Kurulum yöntemi (dev bağımlılık):**
  ```bash
  npm i -D oh-my-opencode-slim
  ```
- **OpenCode'e tanıtım:** `.opencode/opencode.json` içinde plugin referansı (giriş noktası `dist/index.js`). Kesin sözdizimi kurulum anında paketin `README`'sinden doğrulanır.
- **last-known-version:** `3.0.6`
- **Not:** Bu projedeki eski kurulumda `^2.1.0` + bundle (`plugins/oh-my-opencode-slim.js`) birlikte kullanılıyordu. Yeni projelerde yalnızca npm paketi kullanılır; bundle kopyası taşınmaz.

---

## 2. Yetenekler (Skills)

### 2.1. anthropics/skills
- **Tür:** Agent Skills koleksiyonu (çok sayıda skill)
- **Kaynak (GitHub):** https://github.com/anthropics/skills
- **Bu projede kullanılan ilgili skill:** `frontend-design` (distinctive/intentional görsel tasarım yönergesi)
- **Kurulum yöntemi:** Skill'ler kendi klasörlerinde `SKILL.md` içeren bağımsız klasörlerdir. İstenen skill'ler `.opencode/skills/<skill-adı>/` altına kopyalanır (yalnızca seçilenler). En güncel içerik ana repodan çekilir.
- **Lisans:** Apache 2.0 (çoğu skill; docx/pdf/pptx/xlsx hariç — bunlar source-available)

### 2.2. obra/superpowers
- **Tür:** Agent Skills framework + yazılım geliştirme metodolojisi
- **Kaynak (GitHub):** https://github.com/obra/superpowers
- **Bu projede kullanılan ilgili skill'ler:** `web-clone` (URL'den site klonlama), `visual-verdict` (görsel karşılaştırma), ayrıca `brainstorming`, `writing-plans`, `test-driven-development`, `subagent-driven-development`, `requesting-code-review`, `using-git-worktrees`, `finishing-a-development-branch` vb.
- **Kurulum yöntemi (OpenCode):**
  ```
  Fetch and follow instructions from https://raw.githubusercontent.com/obra/superpowers/refs/heads/main/.opencode/INSTALL.md
  ```
  Kurulum anında bu `INSTALL.md` okunup izlenir; yalnızca kullanıcının seçtiği skill'ler alınır.
- **Lisans:** MIT

---

## 3. MCP Sunucuları

> MCP sunucuları kod içi "skill" değil, OpenCode'e dış araç bağlayan konfigürasyonlardır.
> Her biri ayrı bir **opsiyonel modül** olarak sunulur. Kesin `command`/`args`/`env` değerleri kurulum anında resmî kaynağından doğrulanır.

### 3.1. Radzen Blazor Studio MCP
- **Açıklama:** Radzen Blazor Studio / Radzen Blazor for Visual Studio ile entegrasyon (solution yükleme, proje listeleme, sayfa ağacı, veritabanı bağlantıları).
- **Kaynak:** Radzen resmî dokümantasyonu / MCP eklentisi. Kurulum anında doğrulanır.
- **Not:** Yalnızca Radzen/Blazor projelerinde anlamlıdır.
- **Örnek MCP Konfigürasyonu (`.opencode/opencode.json`'a eklenecek):**
  ```json
  {
    "mcp": {
      "radzen": {
        "command": "npx",
        "args": ["-y", "@radzen/mcp-server"],
        "env": {}
      }
    }
  }
  ```
- **Kurulum notu:** Resmî paket adı ve komut kurulum anında `npm view @radzen/mcp-server` veya Radzen dokümantasyonundan doğrulanır.

### 3.2. LogSeq MCP
- **Açıklama:** LogSeq graph'ına sayfa/blok okuma-yazma (bilgi tabanı entegrasyonu).
- **Kaynak:** LogSeq MCP topluluk eklentisi. Kurulum anında doğrulanır.
- **Örnek MCP Konfigürasyonu (`.opencode/opencode.json`'a eklenecek):**
  ```json
  {
    "mcp": {
      "logseq": {
        "command": "npx",
        "args": ["-y", "mcp-logseq"],
        "env": {
          "LOGSEQ_GRAPH_NAME": "<GraphAdı>",
          "LOGSEQ_API_TOKEN": ""
        }
      }
    }
  }
  ```
- **Kurulum notu:** LogSeq masaüstü uygulaması açıkken çalışır (genelde port 12315). `LOGSEQ_GRAPH_NAME` kullanıcının graph'ına göre ayarlanır. Token gerekiyorsa LogSeq ayarlarından alınır.

### 3.3. Stitch MCP
- **Açıklama:** Tasarım-dan-koda (design-to-code) çalışması; design system / ekran oluşturma.
- **Kaynak:** Stitch resmî MCP'si. Kurulum anında doğrulanır.
- **Örnek MCP Konfigürasyonu (`.opencode/opencode.json`'a eklenecek):**
  ```json
  {
    "mcp": {
      "stitch": {
        "command": "npx",
        "args": ["-y", "@stitch-mcp/server"],
        "env": {
          "STITCH_API_KEY": ""
        }
      }
    }
  }
  ```
- **Kurulum notu:** Stitch API key gerektirir. Kurulum anında Stitch dokümantasyonundan güncel paket adı ve komut doğrulanır.

---

## Kurulum Anında Sürüm Doğrulama Kuralı

1. Her varlık için yukarıdaki `Kaynak` URL'sine bak.
2. `npm` tabanlı ise `npm view <paket> version` ile son sürümü oku.
3. GitHub tabanlı ise reponun son release/tag bilgisini (veya `main` dalını) kullan.
4. Kaynak ulaşılamazsa kullanıcıya bilgi ver ve **elle kurulum** alternatifini sun; sessizce eski bir sürüm kurma.
