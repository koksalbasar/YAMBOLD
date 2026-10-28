# {{PROJE_ADI}}

{{PROJE_ACIKLAMA}}

# ⚠️ Çalışma Metodolojisi ve Memory Bank (ZORUNLU)

Bu projede çalışma biçimi **Oturum Bazlı Faz Yönetimi Metodolojisi** ile yürütülür:
`.project-docs/00_workflow/00_SessionBasedWorkflow_Methodology.md`

Her işlem başlangıcında ve her yeni oturumda bu dokümanın talimatlarına göre çalışılmalıdır.

## 1. Oturum Başlangıç Protokolü (göreve başlamadan ÖNCE, zorunlu sırayla)

1. **Metodolojiyi oku:** `.project-docs/00_workflow/00_SessionBasedWorkflow_Methodology.md`
   - Eğer bu yeni bir görev ve müşteriye ait bir "istekler listesi" mevcut değilse
   - `.project-docs/30_ai-prompts` altında bir dizin adreslenmemişse
   - İşin niteliğine uygun isimde bir dizin oluştur.
   - Verilen yeni iş talimatlarını müşteri isteği olarak isimlendirerek bu klasöre yaz.
     - Müşteri isteği dosyası için `.project-docs/30_ai-prompts/istekler-listesi-sablon.md` dosyasını kullan.
   - İş planı ya da istenen raporu bu klasörde oluştur.
2. **Memory Bank'i oku** (LogSeq 2.0 DB tabanlı, çift mod):
   - **LogSeq açıksa (birincil):** LogSeq MCP (`mcp-logseq`) ile oku:
     - `get_pages_tree_from_namespace` → `Projects/<ProjeAdı>` sayfa ağacı
     - `get_page_content` → ilgili sayfalar (`projectbrief`, `productContext`, `systemPatterns`, `techContext`, `activeContext`, `progress`, `project`)
     - `get_page_backlinks` → bağlantıları keşfet
   - **LogSeq kapalıysa (fallback):** `.project-docs/20_memory-bank/` altındaki export edilmiş Markdown dosyalarını oku:
     - `projectbrief.md`, `productContext.md`, `systemPatterns.md`, `techContext.md`, `activeContext.md`, `progress.md`, `project.md`
3. **Yarım kalmış iş kontrolü:** `.project-docs/30_ai-prompts/` altındaki her iş klasörünü kontrol et (klasör yoksa yarım kalmış iş yok demektir):
   - Klasörde `COMPLETED.md` **yoksa** → iş **yarım kalmıştır / devam etmektedir** (son durum ve devir notu: `CHECKPOINT.md`)
   - Klasörde `COMPLETED.md` **varsa** → iş **tamamlanmıştır**
   - Kullanıcının talebi doğrudan yarım kalan işe / klasörüne / faz dosyasına aitse → bilgilendirme ve seçenek sunma adımı atlanır, doğrudan o işe devam edilir.
   - Yarım kalmış iş varken kullanıcı **farklı bir talep** ile geldiyse → kısa bilgi notu göster (hangi iş, hangi faz, son devir notu), iki seçenek sun: **(1)** yarım kalan işe devam et, **(2)** yeni talebe öncelik ver. Bilgilendirme sonrası **DERHAL DUR VE BEKLE** (Strict Stop kuralı); karar gelmeden yeni soru yanıtlanmaz, yeni işe başlanmaz.

## 2. İş Bitiminde Memory Bank Kuralı (ZORUNLU)

- Anlamlı her değişiklik / iş / faz sonunda Memory Bank güncellenmelidir:
  - `activeContext` — güncel odak, son değişiklikler, aktif kararlar
  - `progress` — ne çalışıyor, ne kaldı, bilinen sorunlar
- Kurallar ve workflow: `.project-docs/10_rules/memory-bank.md`
- Değişiklikler MCP ile LogSeq DB'ye yazılır; ardından export → `.project-docs/20_memory-bank/` → git commit (elle eşitleme).
- Faz bazlı işlerde: her faz sonunda `CHECKPOINT.md` güncellenir (devir notu); tüm fazlar onaylandığında `CHECKPOINT.md` → `COMPLETED.md` dönüşümü yapılır.

---

# Proje Bilgileri (kurulumda tespit edildi)

- **Proje tipi:** {{PROJE_TIPI}}
- **Çerçeve (framework):** {{CERCEVE}}
- **Programlama dili:** {{DIL}}
- **Runtime / paket yöneticisi:** {{RUNTIME}}
- **Diğer bağımlılıklar / araçlar:** {{BAGIMLILIKLAR}}

# Proje Amacı ve Hedefleri

{{PROJE_AMACI}}

# Yöntem (Opsiyonel — proje özelinde ek kurallar)

{{PROJE_YONTEM}}
