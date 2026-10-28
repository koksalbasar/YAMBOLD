# Oturum Bazlı Faz Yönetimi Metodolojisi
## Session-Based Phase Management Workflow (Portable)

**Sürüm:** 1.0  
**Tarih:** 1 Ağustos 2026  
**Yazar:** Can (Lead Agent / Orchestrator)  
**Kapsam:** Bu doküman, insan sahibi (Proje Sahibi) ile AI Orchestrator arasındaki standart çalışma biçimini tanımlar. **Projeden bağımsızdır ve herhangi bir projeye taşınabilir.**

---

## 1. Amaç

Bu metodoloji, büyük ve çok aşamalı işlerin **oturum bazlı fazlar halinde** yürütülmesini sağlar. Amaçlar:

- **Bağlam (context) şişmesini önlemek:** Her faz taze bir oturumda çalışır, önceki fazın gereksiz detayları sonraki faza taşınmaz.
- **Doğrulanabilir ilerleme:** Her fazın net başarı kriteri (Definition of Done) vardır.
- **Devamlılık:** Fazlar arası geçiş `CHECKPOINT.md` ile sağlanır; iş bittiğinde `COMPLETED.md`'ye dönüştürülür.
- **Taşınabilirlik:** Klasör yapısı ve dosya adlandırması standarttır; yeni projeye kopyalanarak kullanılabilir.

---

## 2. Roller ve Sorumluluklar

### 2.1. Proje Sahibi (İnsan)
- Yeni iş talebini tanımlar (gereksinim dokümanı, not, sözlü talimat).
- İş planını onaylar (veya reddeder / revize ister).
- Her faz için **yeni bir oturum açar** ve ilgili faz talimatını (prompt) gönderir.
- Faz tamamlandığında doğrulama yapar ve bir sonraki faza geçilmesini onaylar.
- Tüm fazlar bittiğinde işin tamamlandığını ortak onaylar (→ `COMPLETED.md`).

### 2.2. Orchestrator (AI Lead Agent)
- İş talebini analiz eder, **İş Planı** dokümanını hazırlar (`Planlama` adımı).
- Onay alır, **faz talimat dosyalarını** (hazır promptlar) üretir.
- Her oturumda ilgili fazı yürütür; uzman temsilcilere (specialist agents) görev dağıtır.
- Her faz sonunda `CHECKPOINT.md`'yi günceller (devir notları).
- İşin kapanışında `CHECKPOINT.md` → `COMPLETED.md` dönüşümünü yapar.

### 2.3. Uzman Temsilciler (Specialist Agents)
- Fazın tipine göre görevlendirilir (kodlama → Fixer, tasarım → Designer, inceleme → Oracle, araştırma → Explorer/Librarian).

---

## 3. Genel İş Akışı (Adım Adım)

```
Adım 0  Yeni İş Talebi (Proje Sahibi)
   ↓
Adım 1  Planlama (Orchestrator → İş Planı dokümanı)
   ↓
Adım 2  Onay (Proje Sahibi) — onay yoksa uygulamaya geçilmez
   ↓
Adım 3  Faz Talimatlarının Hazırlanması (Orchestrator)
   ↓
Adım 4  Faz N Oturumu (Proje Sahibi yeni oturum açar → talimatı gönderir)
   │        ├─ Orchestrator fazı yürütür (uzman temsilcilerle)
   │        ├─ Başarı kriteri doğrulanır (örn. build, görsel kontrol)
   │        └─ CHECKPOINT.md güncellenir (devir notu)
   ↓
Adım 5  Sonraki Faz için Adım 4 tekrarlanır
   ↓
Adım 6  Tüm Fazlar Tamamlandı → Ortak Onay
   ↓
Adım 7  CHECKPOINT.md → COMPLETED.md (final durum)
```

### Adım 0 — Yeni İş Talebi
- Proje Sahibi işi tanımlar; gerekiyorsa gereksinim dokümanına referans verir.
- Orchestrator talebi netleştirmek için soru sorabilir.

### Adım 1 — Planlama
- Orchestrator, **İş Planı** dokümanını hazırlar ve toplantı notları klasörüne kaydeder.
- Plan şunları içerir: mimari kurallar, fazlar, görevler, sorumlu temsilciler, başarı kriterleri, onay kuralı.
- Onay kuralı aksi belirtilmedikçe her adımda *Proje Sahibi* onayı zorunludur. Bu kurala istisna sadece *Proje Sahibi* tarafından getirilebilir.
- İş planı **oturum bazlı** çalışmayı destekleyecek şekilde fazlara bölünür.

### Adım 2 — Onay Kapısı
- **Onay alınmadan uygulamaya geçilmez.** (Bu metodolojinin tartışmasız kuralıdır.)

### Adım 3 — Faz Talimatlarının Hazırlanması
- Orchestrator her faz için ayrı bir talimat (prompt) dosyası üretir.
- Her talimat dosyası şunları içerir:
  - Oturum hedefi
  - Referans dokümanlar (iş planı + önceki fazların CHECKPOINT'i)
  - Görev listesi (task spec)
  - Görevlendirilecek uzman temsilci
  - Başarı kriterleri (Definition of Done) — build/test komutu dahil

### Adım 4 — Faz Oturumu
- Proje Sahibi **yeni bir oturum açar** ve faz talimat dosyasının içeriğini gönderir (veya dosya yoluna referans verir).
- Orchestrator talimatı uygular, uzman temsilcileri yönetir, doğrulama yapar.
- Faz tamamlanınca `CHECKPOINT.md`'ye devir notu yazılır.

### Adım 5 — Fazlar Arası Geçiş
- Sonraki fazın oturumu `CHECKPOINT.md`'yi okuyarak kaldığı yerden devam eder.

### Adım 6 — Ortak Onay
- Tüm fazlar bittiğinde Proje Sahibi ve Orchestrator işin bittiğini **birlikte onaylar**.

### Adım 7 — Kapanış (CHECKPOINT → COMPLETED)
- Onay sonrası `CHECKPOINT.md`, dosya adı `COMPLETED.md` olacak şekilde son durumla güncellenir.
- Bu, işin **resmi olarak tamamlandığının** kanıtıdır.

---

## 4. Oturum Başlangıç Protokolü (Yarım Kalmış İş Kontrolü)

Her yeni oturum başlangıcında Orchestrator aşağıdaki kontrolü yapar:

### 4.1. Kontrol
- Talimat klasörü (`.project-docs/30_ai-prompts/`) altındaki **her iş klasörünü** kontrol et.
- Klasör içinde `COMPLETED.md` **yoksa** → iş **yarım kalmıştır / devam etmektedir**.
- Klasör içinde `COMPLETED.md` **varsa** → iş **tamamlanmıştır**.

### 4.2. Aynı İş Talebi Kontrolü (İstisna / Özel Koşul)
- Eğer *Proje Sahibi*nin yeni oturumdaki ilk mesajı/talebi, tespit edilen yarım kalmış işin kendisini, klasörünü veya o işe ait bir faz/prompt dosyasını doğrudan içeriyorsa/hedefliyorsa:
  - **Ayrı bir bilgilendirme, durdurma veya seçenek sunma adımı uygulanmaz.**
  - Kullanıcının doğrudan o yarım kalan işe devam etmek istediği kabul edilerek derhal ilgili fazın uygulamasına geçilir.

### 4.3. Farklı/Yeni Talep Durumunda Bilgilendirme ve Durdurma (ZORUNLU BEKLEME)
- Yarım kalmış iş varken *Proje Sahibi* **farklı bir konu/soru/talep** ile geldiyse:
  - *Proje Sahibi*ne **kısa bir bilgi notu** gösterilir:
    - Hangi iş, hangi fazda kalmış, son devir notu ne diyor (`CHECKPOINT.md`).
  - Proje Sahibine net iki seçenek sunulur:
    1. **Yarım kalmış işe devam et** (checkpoint'ten itibaren)
    2. **Yeni oturumun işine / sorusuna öncelik ver** (yarım kalan iş beklemeye alınır)
  - 🛑 **KRİTİK DURMA KURALI (Strict Stop):** Bilgilendirme yapıldıktan sonra Orchestrator **DERHAL DURUR VE BEKLER**. Karar gelmeden KESİNLİKLE yeni soru yanıtlanmaz veya yeni işe başlanmaz.

### 4.4. Öncelik Kararı Sonrası Devam
- *Proje Sahibi* **Seçenek 1 (Yarım kalmış iş)** derse: Checkpoint'ten itibaren yarım kalan faza devam edilir.
- *Proje Sahibi* **Seçenek 2 (Yeni iş/soru)** derse: Oturumun ilk mesajındaki yeni talep/soru işlenir. Aynı oturum içinde yarım kalan iş bir daha hatırlatılmaz; durumu `CHECKPOINT.md`'de korunur ve bir sonraki oturumda tekrar kontrol edilir.

---

## 5. Klasör ve Dosya Konvansiyonları

### 5.1. Standart Yerleşim (proje köküne göre)

```
<proje-kökü>/
├── AGENTS.md                        # OpenCode ana talimatı
├── .opencode/                       # OpenCode yapılandırması (instructions + plugin + mcp)
├── .project-docs/
│   ├── 00_workflow/                 # Bu metodoloji (taşınabilir)
│   ├── 10_rules/                    # Kurallar (bakım/naming kuralları)
│   ├── 20_memory-bank/              # Kalıcı bağlam (Memory Bank)
│   ├── 30_ai-prompts/               # AI talimatları merkezi
│   │   └── <is-konu-slug>/          # Her iş için ayrı klasör
│   │       ├── Faz_1_<FazAdi>_Prompt.md
│   │       ├── Faz_2_<FazAdi>_Prompt.md
│   │       ├── ...
│   │       ├── CHECKPOINT.md        # Aktif ilerleme / devir takibi
│   │       └── COMPLETED.md         # (İş bittiğinde oluşur)
│   ├── 40_project/                  # Proje dokümanları (iş planları, toplantı notları, tasarım)
│   └── 50_helpers/                  # Yardımcı rehberler / dönüşüm referansları
└── (proje kodu)
```

#### 5.1.1 İsimlendirme Kuralları

* **Görev Klasörü (`<is-konu-slug>`)**: `[YYMMDD]-[NN]-[<is-konu-slug>]`-> [NN] gün içindeki görev sayısı `01 - 02 - ...` Her yeni günde sıfırlanır.

* **Fazlar**: `Faz_[NN]_<FazAdi>.md` -> [NN] Göreve ait sıra numarası `01 - 02 - ...` Her yeni görevde sıfırlanır.

### 5.2. Dosya Adlandırma Kuralları
| Dosya | Amaç | Ne zaman güncellenir |
|---|---|---|
| `Faz_N_*_Prompt.md` | Faz talimatı (hazır prompt) | Plan onayından sonra bir kez |
| `CHECKPOINT.md` | İlerleme + devir notları | Her faz sonunda |
| `COMPLETED.md` | Resmi kapanış kanıtı | Tüm fazlar onaylandığında |

---

## 6. Uzman Temsilci Görevlendirme Rehberi

| Faz Tipi | Temsilci | Örnek |
|---|---|---|
| Backend / mekanik kod | `Fixer` | Servis katmanı, DI, iş mantığı |
| UI/UX tasarım | `Designer` | Sayfa, bileşen, görsel polisaj |
| Mimari inceleme / risk | `Oracle` | Kod review, güvenlik denetimi |
| Kod tabanı keşfi | `Explorer` | Dosya/symbol arama |
| Dış doküman araştırması | `Librarian` | Kütüphane/API dokümantasyonu |

---

## 7. Taşınabilirlik (Portability) Talimatı

Bu metodoloji **projeden bağımsızdır**:

1. Bu dokümanı hedef projede `.project-docs/00_workflow/` klasörüne kopyala.
2. Hedef projede `.project-docs/30_ai-prompts/` klasörü yoksa oluştur.
3. `.project-docs/20_memory-bank/` yoksa, Memory Bank referansları (Bölüm 8) hedef projenin kendi hafıza sistemine karşılık gelecek şekilde güncelle.
4. Roller ve adımlar (Bölüm 2–6) projeden bağımsız olduğu için değişiklik gerektirmez.

---

## 8. Memory Bank Referansı

Bu metodoloji, projenin kalıcı hafızasında şu şekilde referanslanır:

- **Memory Bank:** `.project-docs/20_memory-bank/`
  - `systemPatterns.md` → "Çalışma Metodolojisi" pattern'i olarak.
  - `activeContext.md` → "Aktif Kararlar" bölümünde güncel referans.
- **Metodoloji dokümanı:** `.project-docs/00_workflow/00_SessionBasedWorkflow_Methodology.md`

---

## 9. Sürüm Geçmişi

| Sürüm | Tarih | Değişiklik |
|---|---|---|
| 1.0 | 2026-08-01 | İlk sürüm. Oturum bazlı faz yönetimi, CHECKPOINT → COMPLETED akışı, oturum başlangıç kontrolü ve taşınabilirlik kuralı tanımlandı. |

---

## 10. Onay ve İmza Bloğu

Bu metodoloji, aşağıda adı ve imzası bulunan taraflarca incelenmiş, onaylanmış ve **ortak temel çalışma yöntemi** olarak kabul edilmiştir.

| Rol | Ad | Tarih | Onay / İmza |
|---|---|---|---|
| Proje Sahibi | **Başar** | 01 / 08 / 2026 | ____________ |
| Orchestrator (Lead Agent) | **Can** | 01 / 08 / 2026 | ____________ |

> **Taşınabilirlik notu:** Bu doküman başka bir projeye taşındığında, imza bloğu hedef projenin sahibi ve orchestrator'ına göre güncellenerek yeniden onaya sunulur.
