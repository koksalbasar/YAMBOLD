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

## 3. Namespace Yapısı

Proje bilgisi, LogSeq graph içinde `Projects/<ProjeAdı>/` namespace'i altında tutulur. Ayrı graph açılmaz; namespace yeterlidir.

```
Projects/<ProjeAdı>/
├── MemoryBank               # Ana sayfa (hub) — tüm alt sayfalara [[wikilink]]
├── projectbrief             # Kapsam ve hedefler
├── productContext           # Neden var, sorunlar, UX hedefleri
├── systemPatterns           # Mimari, desenler, kararlar
├── techContext              # Teknolojiler, kurulum, kısıtlar
├── activeContext            # Güncel odak, son değişiklikler, sonraki adımlar
├── progress                 # Ne çalışıyor, ne kaldı, bilinen sorunlar
└── project                  # Proje zekâsı, öğrenilen desenler
```

Günlük notlar (`journals/`) bu sayfalara `[[Projects/<ProjeAdı>/...]]` ile link verir → backlink'ler otomatik oluşur. Günlük düşünce akışı ↔ proje bilgi bankası böylece birbirine bağlanır.

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
| `query({...})` | LogSeq QL ile filtreleme (tag, namespace, property) |
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
| Namespace | `Projects/<ProjeAdı>/` |
| Senkronizasyon | Seçenek A (elle); Seçenek B (otomasyon) ileride |
