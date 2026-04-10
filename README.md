# Claude Skills Repository

Bu repository, Claude Code için özel yetenekler (skills) içermektedir. Her bir skill, belirli bir görev türünde Claude'ın nasıl davranması gerektiğini tanımlayan yapılandırılmış talimatlardır.

## 📂 Skills Listesi

| Skill Klasörü | Açıklama |
|---------------|----------|
| `design-taste-frontend` | Senior UI/UX Engineer. Dijital arayüzler mimarlığı, metrik tabanlı kurallar, Strict component architecture, CSS hardware acceleration. |
| `gsap-core` | GSAP core API - gsap.to(), from(), fromTo(), easing, duration, stagger, defaults, gsap.matchMedia(). |
| `gsap-performance` | GSAP performans optimizasyonu - transform'ları tercih et, layout thrashing'dan kaçın, will-change, batching. |
| `gsap-scrolltrigger` | ScrollTrigger - scroll-linked animations, pinning, scrub, triggers. |
| `scroll-video-animation` | Scroll-triggered video websites için kapsamlı rehber. |
| `security-audit` | Güvenlik denetimi ve güvenlik açığı değerlendirmesi. OWASP Top 10, gizli bilgi tespiti. |
| `web-page-design-skill` | Modern, yüksek performanslı web sayfaları - video backgrounds, glassmorphic effects, cinematic animations. |
| `wp-block-themes` | WordPress block themes geliştirme - theme.json, templates, patterns, style variations, Site Editor. |

## 📁 Klasör Yapısı

Her skill klasörü şu yapıyı takip eder:

```
skill-name/
├── SKILL.md           # Skill tanımı, kullanım kuralları
├── evals/            # (Opsiyonel) Değerlendirme test dosyaları
├── references/       # (Opsiyonel) Referans dokümanlar
└── .DS_Store         # macOS system file (genellikle görmezden gelinir)
```

## 🚀 Kullanım

Bu skiller, Claude Code içinde otomatik olarak yüklenir. Bir skill'in tetiklenmesi için kullanıcının ilgili görev türünde bir istek yapması gerekir.

Örneğin:
- "GSAP ile animasyon yap" → `gsap-core` skilli tetiklenir
- "Scroll-triggered animation yap" → `gsap-scrolltrigger` skilli tetiklenir
- "Güvenlik audit'i yap" → `security-audit` skilli tetiklenir

## 📝 Skill Geliştirme

Yeni bir skill oluşturmak için:

1. Yeni bir klasör oluşturun (isim: kebab-case)
2. `SKILL.md` dosyası ekleyin (skill tanımını yazın)
3. Varsa `evals/` ve `references/` alt klasörler ekleyin
4. Bu repository'ye push edin

SKILL.md formatı hakkında detaylı bilgi için mevcut skill dosyalarına bakın.

## 🔧 Teknik Detaylar

- **Skill Yükleyici:** Claude Skill Framework v2
- **Triggering:** Pattern matching + semantic intent detection
- **Evals:** JSON formatında değerlendirme dosyaları
- **Referanslar:** Markdown formatında

## 📄 Lisans

Bu skilller Anthropic Claude Code için tasarlanmıştır. Kullanım şartları için Anthropic吃着 belgelerine bakın.

---

**Son Güncelleme:** 10 Nisan 2025
**Repository Sahibi:** Ceyhan Molla
