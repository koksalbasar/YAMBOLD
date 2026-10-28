# Memory Bank (LogSeq 2.0 DB Tabanlı)

> Bu doküman, AI kodlama asistanının oturumlar arası kalıcı bağlamı için kullanılan Memory Bank'ın **yöntemini** tanımlar.
>
> **Mimari:** LogSeq 2.0 DB (canlı kaynak) + Markdown export (git yedeği) + MCP (AI erişimi).
>
> **Uygun olduğu araçlar:** OpenCode + Oh My OpenCode Slim, Cline veya oturum bellek sıfırlaması olan herhangi bir AI kodlama aracı.

---

## 1. Neden Memory Bank?

AI, oturumlar arasında hafızasını kaybeder. Memory Bank, projenin kalıcı bilgisini tutar; **her oturum başında okunur**. Memory Bank olmadan AI her seferinde sıfırdan başlar. Bu bir sınırlama değil — mükemmel dokümantasyonun neden gerekli olduğunu gösteren şeydir.

## 2. Mimari (3 Katman)

| Katman | Konum | Rol |
|--------|-------|-----|
| **Canlı kaynak** | LogSeq 2.0 DB (yerel, `~\logseq\graphs\<graph>`) | Grafik, backlink, QL sorgusu |
| **Git yedeği** | `.project-docs/20_memory-bank/` | Markdown export, portabilite, geçmiş, fallback |
| **AI erişimi** | `mcp-logseq` (masaüstü uygulama parçası) | DB'ye sorgu / yazma |

> **Not:** LogSeq 2.0, Markdown dosyalarını *birincil kaynak* olarak değil **DB** olarak kullanır. Markdown, DB'den **export** edilerek elde edilir. Bu yüzden "canlı kaynak" DB'dir; Markdown bir **yedek/fallback** katmanıdır.

## 3. Namespace ve İlişkisel Yapı (LogSeq 2.0 DB)

> **Mimari Notu:** LogSeq 2.0 (DB sürümü), SQLite tabanlı ilişkisel yapı kullandığı için Markdown dönemindeki katı "klasör/dosya yolu" mantığı (`Projects/<ProjeAdı>/...`) yerine **nesne ilişkileri (Properties, Tags, Classes)** ve **DB Queries** yaklaşımını benimser.

Proje bilgisi LogSeq'te iki yönlü ele alınır:
1. **İlişkisel Özellikler (Properties - Önerilen):** Her sayfa `project:: [[ProjeAdı]]`, `parent:: [[MemoryBank]]`, `category::` gibi özellikler (properties) ile birbirine bağlanır. Bu sayede bir not birden fazla üst kavramla veya projeyle esnek şekilde ilişkilendirilebilir.
2. **Sayfalandırma / Etiket İsimlendirme (`/` Kullanımı):** Sayfa adlarında `Projects/<ProjeAdı>/MemoryBank` biçiminde eğik çizgi kullanılmaya devam edilebilir, ancak DB bunu fiziksel dosya yolu olarak değil benzersiz bir sayfa adı/etiketi olarak ele alır.

```
Pages:
- MemoryBank [[ProjeAdı]]
- projectbrief (project:: [[ProjeAdı]], parent:: [[MemoryBank]])
- productContext (project:: [[ProjeAdı]], parent:: [[MemoryBank]])
- systemPatterns (project:: [[ProjeAdı]], parent:: [[MemoryBank]])
- techContext (project:: [[ProjeAdı]], parent:: [[MemoryBank]])
- activeContext (project:: [[ProjeAdı]], parent:: [[MemoryBank]])
- progress (project:: [[ProjeAdı]], parent:: [[MemoryBank]])
- project (project:: [[ProjeAdı]], parent:: [[MemoryBank]])
```

Günlük notlar (`journals/`) bu sayfalara `[[MemoryBank]]` veya `[[ProjeAdı]]` ile link verir → backlink'ler ve DB ilişkisel filtreleri otomatik oluşur.

## 4. Sayfa Yapısı (Properties + Wikilink)

Her Memory Bank sayfası bir LogSeq page'dir; **properties (JSON)** ile etiketlenir:

```
tags: ["blazor", "sqlite"]
status: "active"
project: "<ProjeAdı>"
```

Sayfalar arası `[[wikilink]]` ile bağlanır; backlink'ler otomatik üretilir. İlgili kavramlar (ör. `[[techContext]]` içinde `[[Radzen]]`, `[[sqlite]]`) birbirine linklenerek grafik zenginleştirilir.

## 5. AI Erişimi (Çift Mod)

### 5.1 Birincil — MCP (LogSeq açıkken)

| MCP Aracı | Kullanım |
|-----------|----------|
| `get_pages_tree_from_namespace({namespace: "Projects/<ProjeAdı>"})` | Proje sayfa ağacını listele |
| `get_page_content({...})` | Bir sayfanın içeriğini oku |
| `get_page_backlinks({...})` | Hangi sayfalar bu sayfaya link veriyor |
| `query({...})` | LogSeq QL veya DB Query ile filtreleme (`project:: [[ProjeAdı]]`, `parent::`, properties) |
| `update_block` / `create_block` | AI yazma / güncelleme |

### 5.2 Fallback — Markdown export (LogSeq kapalıyken)

- `.project-docs/20_memory-bank/` altındaki export edilmiş Markdown dosyaları okunur.
- Bu katman, LogSeq açık olmasa bile Memory Bank'ın her zaman erişilebilir olmasını sağlar.

## 6. Senkronizasyon (Seçenek A — Elle)

Her önemli güncellemeden sonra:

1. LogSeq'ten **Markdown export** al.
2. `.project-docs/20_memory-bank/` içine yaz.
3. `git add` + `git commit`.

> ⚠️ **Hatırlatma (Seçenek B — Otomasyon):** Elle eşitleme yerine `pre-commit` git hook ile otomatik LogSeq export senkronizasyonu **ileride değerlendirilecek**. Otomasyon ihtiyacı doğduğunda bu not hatırlatıcıdır.

## 7. İş Akışı

### Oturum başında
1. LogSeq açık mı?
   - **Evet (birincil):** MCP ile `get_pages_tree_from_namespace` + `get_page_content` ile canlı sayfaları oku; `get_page_backlinks` ile bağlantıları keşfet.
   - **Hayır (fallback):** `.project-docs/20_memory-bank/` Markdown dosyalarını oku.
2. Özellikle `activeContext.md` ve `progress.md` güncel durumu ve kalan işleri verir.

### İş bitiminde
- Anlamlı her değişiklik / iş / faz sonunda Memory Bank güncellenir:
  - `activeContext` — güncel odak, son değişiklikler, aktif kararlar.
  - `progress` — ne çalışıyor, ne kaldı, bilinen sorunlar.
- Değişiklikler MCP ile DB'ye yazılır; ardından export → `.project-docs/20_memory-bank/` → git commit (Seçenek A).
- Faz bazlı işlerde `CHECKPOINT.md` güncellenir; tüm fazlar onaylandığında `CHECKPOINT.md → COMPLETED.md` dönüşümü yapılır.

## 8. Ne Yakalanır?

- Kritik implementasyon yolları
- Kullanıcı tercihleri ve iş akışı
- Projeye özel desenler
- Bilinen zorluklar
- Kararların evrimi
- Araç kullanım desenleri

> Format esnektir — AI'nın projeyle daha etkili çalışmasına yardımcı olan değerli içgörüleri yakalamaya odaklan. Memory Bank, proje büyüdükçe zenginleşen canlı bir yapıdır.

---

## Özet (Mimari Karar Kaydı)

| Karar | Değer |
|-------|-------|
| Canlı kaynak | LogSeq 2.0 DB (yerel) |
| Git yedeği | `.project-docs/20_memory-bank/` (Markdown export) |
| AI erişimi | MCP (birincil) + Markdown (fallback) |
| Hiyerarşi / Yapı | `project::`, `parent::` properties + LogSeq 2.0 DB relational queries |
| Senkronizasyon | Seçenek A (elle); Seçenek B (otomasyon) ileride |
