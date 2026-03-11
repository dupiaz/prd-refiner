# 🚀 SaaS / Tech Startup Marketing Workflow
## Áp dụng cho: PRD Refiner — AI-Powered PRD Writing Tool

> **Mục tiêu**: Quy trình marketing end-to-end cho SaaS startup, từ xây dựng brand đến growth — sử dụng 100% bộ skills có sẵn.
>
> **Đối tượng**: Product Manager, Startup Founder, Tech Lead
>
> **Mô hình**: One-person SaaS / Small team

---

## 📋 Tổng Quan 12 Phase

```
Phase 1: Brand Foundation          ──→ Xây dựng nền tảng thương hiệu
Phase 2: Market Intelligence       ──→ Nghiên cứu thị trường & đối thủ
Phase 3: Audience Deep Dive        ──→ Hiểu sâu khách hàng mục tiêu
Phase 4: Pricing & Positioning     ──→ Chiến lược giá & định vị
Phase 5: Product Launch            ──→ Kế hoạch ra mắt sản phẩm
Phase 6: SEO Foundation            ──→ Nền tảng SEO
Phase 7: Content Engine            ──→ Máy sản xuất nội dung
Phase 8: Paid Acquisition          ──→ Quảng cáo trả phí
Phase 9: Email & Automation        ──→ Email marketing & tự động hóa
Phase 10: Growth Loops             ──→ Vòng lặp tăng trưởng
Phase 11: Analytics & Optimization ──→ Đo lường & tối ưu
Phase 12: Scale & Reporting        ──→ Mở rộng & báo cáo
```

---

## Phase 1: Brand Foundation 🏗️
**Mục tiêu**: Thiết lập brand profile & voice guidelines làm nền tảng cho mọi hoạt động marketing.

### Bước 1.1 — Thiết lập Brand Profile
**Skill**: `brand-setup`
```
Prompt: "Thiết lập brand profile cho PRD Refiner — một công cụ AI giúp Product Manager 
viết PRD chuyên nghiệp. Target audience là PM và Startup Founder tại Việt Nam và Đông Nam Á."
```

**Output kỳ vọng**:
- Brand name, tagline, mission, vision
- Brand personality & tone of voice
- Visual identity guidelines
- Target market definition

### Bước 1.2 — Import Brand Guidelines  
**Skill**: `import-guidelines`
```
Prompt: "Import brand voice guidelines cho PRD Refiner:
- Tone: Professional nhưng friendly, tech-savvy
- Banned words: 'simple', 'easy' (tránh đánh giá thấp complexity)
- Channel styles: LinkedIn = formal, TikTok = casual & energetic"
```

### Bước 1.3 — Phân tích Brand Voice
**Skill**: `content-creator` (brand voice analyzer)
```
Prompt: "Phân tích brand voice của PRD Refiner từ các content hiện có 
(website copy, TikTok script) và tạo brand voice guide chi tiết."
```

**✅ Checkpoint Phase 1**: Brand profile hoàn chỉnh, voice guidelines documented.

---

## Phase 2: Market Intelligence 🔍
**Mục tiêu**: Hiểu rõ thị trường, đối thủ, và cơ hội.

### Bước 2.1 — Phân tích Cạnh tranh
**Skill**: `competitor-analysis`
```
Prompt: "Phân tích đa chiều các đối thủ của PRD Refiner:
- Trực tiếp: Notion AI, Linear, Productboard, Aha!
- Gián tiếp: ChatGPT, Google Docs + template
- Phân tích: content, SEO, paid ads, social presence, pricing, AI visibility"
```

### Bước 2.2 — Bản đồ Cạnh tranh Chi tiết
**Skill**: `competitive-landscape`
```
Prompt: "Áp dụng Porter's Five Forces và SWOT cho thị trường PRD tools. 
Xác định điểm khác biệt của PRD Refiner: chuyên biệt hóa cho PRD (không phải general-purpose AI)."
```

### Bước 2.3 — Giám sát Đối thủ Liên tục
**Skill**: `competitor-monitor`
```
Prompt: "Thiết lập competitor monitoring cho: Notion AI, Productboard, Linear. 
Theo dõi: pricing changes, feature launches, content strategy, ad campaigns."
```

### Bước 2.4 — Phân tích Quảng cáo Đối thủ
**Skill**: `competitive-ads-extractor`
```
Prompt: "Trích xuất và phân tích quảng cáo của Notion AI và Productboard 
từ Facebook Ad Library và LinkedIn. Tìm messaging patterns và gaps."
```

### Bước 2.5 — Narrative Landscape
**Skill**: `narrative-landscape`
```
Prompt: "Map competitive narrative landscape cho PRD tools market. 
Tìm positioning territories chưa ai chiếm: 'AI PRD specialist' vs 'general PM tool'."
```

**✅ Checkpoint Phase 2**: Competitive intelligence report, positioning strategy.

---

## Phase 3: Audience Deep Dive 👥
**Mục tiêu**: Xây dựng buyer personas chi tiết và hiểu jobs-to-be-done.

### Bước 3.1 — Audience Intelligence
**Skill**: `audience-intelligence`
```
Prompt: "Nghiên cứu audience cho PRD Refiner:
- Segment 1: Product Manager (3-7 năm kinh nghiệm, công ty tech 50-500 người)
- Segment 2: Startup Founder (technical, đang scale team)
- Segment 3: Tech Lead phải viết PRD nhưng không chuyên
Phân tích: pain points, goals, channels, content preferences, buying triggers."
```

### Bước 3.2 — Buyer Personas
**Skill**: `audience-profile`
```
Prompt: "Tạo 3 buyer personas chi tiết cho PRD Refiner:
1. 'Minh the PM' — Senior PM tại startup Series A
2. 'Linh the Founder' — Technical founder đang scale
3. 'Hùng the Tech Lead' — Dev lead phải viết specs
Include: demographics, psychographics, JTBD, objections, content preferences."
```

### Bước 3.3 — Focus Group Ảo
**Skill**: `focus-group`
```
Prompt: "Chạy synthetic focus group test messaging cho PRD Refiner:
- Message A: 'Viết PRD chuẩn chỉnh trong 5 phút với AI'
- Message B: 'Không bao giờ phải viết lại PRD vì thiếu edge case'
- Message C: 'AI review PRD như một Senior PM'
Test trên 3 personas đã tạo."
```

**✅ Checkpoint Phase 3**: 3 buyer personas, validated messaging.

---

## Phase 4: Pricing & Positioning 💰
**Mục tiêu**: Xác định chiến lược giá và định vị sản phẩm.

### Bước 4.1 — Chiến lược Giá
**Skill**: `pricing-strategy`
```
Prompt: "Thiết kế pricing strategy cho PRD Refiner SaaS:
- Freemium model (X PRDs free/tháng)
- Pro tier: unlimited PRDs + advanced features
- Team tier: collaboration + shared templates
Phân tích: value metrics, willingness to pay, competitive pricing."
```

### Bước 4.2 — Test Giá
**Skill**: `pricing-test`
```
Prompt: "Simulate pricing sensitivity cho PRD Refiner:
- Free: 5 PRDs/tháng
- Pro: $19/tháng (unlimited)  
- Team: $49/tháng (5 users)
Test across PM personas. Tìm optimal price point."
```

### Bước 4.3 — SaaS Metrics Framework
**Skill**: `startup-metrics-framework`
```
Prompt: "Thiết lập SaaS metrics framework cho PRD Refiner:
CAC, LTV, churn rate, expansion revenue, rule of 40, burn multiple.
Đặt benchmarks cho Year 1."
```

**✅ Checkpoint Phase 4**: Pricing tiers defined, metrics framework ready.

---

## Phase 5: Product Launch 🎯
**Mục tiêu**: Kế hoạch go-to-market hoàn chỉnh.

### Bước 5.1 — Launch Strategy
**Skill**: `launch-strategy`
```
Prompt: "Tạo go-to-market strategy cho PRD Refiner launch:
- Product Hunt launch plan
- Beta program strategy
- Launch timeline (T-30 đến T+30)
- Channel priorities cho VN market"
```

### Bước 5.2 — Launch Plan Chi tiết
**Skill**: `launch-plan`
```
Prompt: "Tạo launch playbook chi tiết cho PRD Refiner:
- Pre-launch (T-30): beta users, testimonials, content seeding
- Launch day: Product Hunt, social media blitz, email blast
- Post-launch (T+30): retention, feedback loop, iteration"
```

### Bước 5.3 — Campaign Plan
**Skill**: `campaign-plan`
```
Prompt: "Tạo multi-channel campaign plan cho PRD Refiner launch:
- LinkedIn: thought leadership cho PM community
- TikTok: short-form demo videos
- Twitter/X: launch thread + engagement
- Product Hunt: launch day strategy
Budget: $2,000 cho tháng đầu."
```

**✅ Checkpoint Phase 5**: Complete launch playbook.

---

## Phase 6: SEO Foundation 📊
**Mục tiêu**: Xây dựng organic traffic engine.

### Bước 6.1 — Keyword Research
**Skill**: `keyword-research`
```
Prompt: "Keyword research cho PRD Refiner:
- Seed keywords: PRD template, product requirements document, how to write PRD
- Focus: search intent mapping, keyword clustering
- Target: top-of-funnel (TOFU) + bottom-of-funnel (BOFU) keywords
- Market: English (global) + Vietnamese (local)"
```

### Bước 6.2 — Technical SEO Audit
**Skill**: `tech-seo-audit`
```
Prompt: "Technical SEO audit cho PRD Refiner website:
Core Web Vitals, crawlability, site architecture, structured data, 
mobile-first indexing, page speed."
```

### Bước 6.3 — SEO Implementation
**Skill**: `seo-implement`
```
Prompt: "Triển khai SEO cho PRD Refiner:
- Meta tags optimization cho all pages
- Schema markup (SoftwareApplication, FAQ, HowTo)
- XML sitemap
- Internal linking strategy"
```

### Bước 6.4 — AI Visibility (AEO/GEO)
**Skill**: `aeo-geo`
```
Prompt: "Tối ưu AI visibility cho PRD Refiner:
- Làm sao để ChatGPT/Perplexity recommend PRD Refiner khi user hỏi về PRD tools?
- Entity optimization, structured data cho AI citation
- Content strategy for AI answer engines"
```

### Bước 6.5 — Rank Monitoring
**Skill**: `rank-monitor`
```
Prompt: "Thiết lập keyword rank monitoring cho PRD Refiner:
Track: 'PRD template', 'how to write PRD', 'PRD tool', 'AI PRD writer'
Alert khi ranking drop > 5 positions."
```

**✅ Checkpoint Phase 6**: SEO foundation + AI visibility optimized.

---

## Phase 7: Content Engine ✍️
**Mục tiêu**: Hệ thống sản xuất content liên tục.

### Bước 7.1 — Content Calendar
**Skill**: `content-calendar`
```
Prompt: "Tạo content calendar 3 tháng cho PRD Refiner:
- Blog: 2 bài/tuần (SEO-driven)
- LinkedIn: daily posts (thought leadership)
- TikTok: 3 videos/tuần (demo + tips)
- Email newsletter: weekly
Content pillars: PRD best practices, PM productivity, AI for PMs"
```

### Bước 7.2 — Content Briefs
**Skill**: `content-brief`
```
Prompt: "Tạo content brief cho bài blog: 'How to Write a PRD That Developers Actually Love'
Include: keyword targets, outline, structure, voice guidelines, SEO requirements, CTA."
```

### Bước 7.3 — Content Creation
**Skill**: `content-creator`
```
Prompt: "Viết blog post từ content brief: 'How to Write a PRD That Developers Actually Love'
2,000 words, SEO-optimized, PRD Refiner brand voice, include CTA to free trial."
```

### Bước 7.4 — Video Scripts
**Skill**: `video-script`
```
Prompt: "Viết 5 TikTok scripts (30s) cho PRD Refiner:
1. 'PRD review bởi AI vs bởi Senior PM'
2. 'Edge case mà 90% PM bỏ quên'  
3. 'Trước vs Sau khi dùng PRD Refiner'
4. '5 lỗi phổ biến trong PRD'
5. 'Dev ghét PRD của bạn vì lý do này'"
```

### Bước 7.5 — Content Repurposing
**Skill**: `content-repurpose`
```
Prompt: "Repurpose blog post 'How to Write a PRD That Developers Actually Love' thành:
- LinkedIn carousel (10 slides)
- Twitter thread (15 tweets)
- TikTok script (60s)
- Email newsletter excerpt
- Instagram infographic"
```

### Bước 7.6 — Content Quality Check
**Skill**: `eval-content`
```
Prompt: "Đánh giá chất lượng blog post vừa tạo:
- Brand voice consistency
- SEO optimization score
- Hallucination check
- Readability score
- Actionable recommendations"
```

**✅ Checkpoint Phase 7**: Content machine running, quality validated.

---

## Phase 8: Paid Acquisition 💸
**Mục tiêu**: Kênh quảng cáo trả phí để accelerate growth.

### Bước 8.1 — Ad Strategy
**Skill**: `paid-advertising`
```
Prompt: "Paid advertising strategy cho PRD Refiner:
- Google Ads: search campaigns cho PRD-related keywords
- LinkedIn Ads: sponsored content targeting PMs
- Meta Ads: retargeting website visitors
Budget allocation: $2,000/tháng, optimize cho signups"
```

### Bước 8.2 — Ad Creative
**Skill**: `ad-creative`
```
Prompt: "Tạo ad copy variations cho PRD Refiner:
- Google RSA: 15 headlines + 4 descriptions
- LinkedIn Sponsored Post: 3 variants
- Meta Ads: 3 primary text + 3 headlines
Focus: pain point → solution → CTA"
```

### Bước 8.3 — A/B Test Plan
**Skill**: `ab-test-plan`
```
Prompt: "Thiết kế A/B test plan cho PRD Refiner ads:
- Test: headline variations (pain point vs benefit vs social proof)
- Test: CTA ('Free trial' vs 'Try for free' vs 'Start now')
- Sample size calculation, duration, success metrics"
```

### Bước 8.4 — Landing Page Audit
**Skill**: `landing-page-audit`
```
Prompt: "Audit landing page PRD Refiner cho conversion:
Above-fold clarity, trust signals, form friction, message match với ads,
mobile experience, page speed."
```

### Bước 8.5 — Retargeting Strategy
**Skill**: `retargeting-strategy`
```
Prompt: "Thiết kế retargeting strategy cho PRD Refiner:
- Segment: visited pricing page but didn't signup
- Segment: started signup but abandoned
- Segment: free trial but not converted to paid
Creative sequencing & frequency caps."
```

**✅ Checkpoint Phase 8**: Paid channels launched, tests running.

---

## Phase 9: Email & Automation 📧
**Mục tiêu**: Nurture leads và convert free → paid.

### Bước 9.1 — Email Sequences
**Skill**: `email-sequence`
```
Prompt: "Tạo email sequences cho PRD Refiner:
1. Welcome sequence (5 emails, 14 ngày): onboarding free trial
2. Nurture sequence (8 emails, 30 ngày): education + social proof
3. Conversion sequence (3 emails, 7 ngày): trial ending → upgrade
4. Winback sequence (4 emails, 30 ngày): churned users
Include: subject lines, body copy, timing, segmentation."
```

### Bước 9.2 — Marketing Automation
**Skill**: `marketing-automation`
```
Prompt: "Thiết kế marketing automation workflows cho PRD Refiner:
- Trigger: user creates first PRD → send tips email
- Trigger: user inactive 7 days → re-engagement sequence
- Trigger: user hits PRD limit → upgrade nudge
- Lead scoring model based on product usage"
```

### Bước 9.3 — CRO
**Skill**: `cro`
```
Prompt: "CRO strategy cho PRD Refiner conversion funnel:
- Homepage → Signup: optimize CTA, reduce friction
- Signup → First PRD: onboarding UX
- Free → Paid: upgrade triggers, pricing page optimization
- Paid → Retained: feature adoption, usage milestones"
```

**✅ Checkpoint Phase 9**: Automated funnel operational.

---

## Phase 10: Growth Loops 🔄
**Mục tiêu**: Xây dựng sustainable growth engines.

### Bước 10.1 — Detect Growth Loops
**Skill**: `loop-detect`
```
Prompt: "Phát hiện và mô hình hóa growth loops cho PRD Refiner:
- Content loop: SEO blog → signup → create PRD → share → backlinks
- Viral loop: PM tạo PRD → share với dev team → dev refer PM khác
- Data loop: more PRDs → better AI → better product → more users
Đánh giá effectiveness và đề xuất new loops."
```

### Bước 10.2 — Growth Engineering
**Skill**: `growth-engineering`
```
Prompt: "Growth engineering plan cho PRD Refiner:
- Product-led growth tactics (free → paid conversion)
- Referral program design ('Invite PM, get 1 month Pro free')
- Viral mechanics (shareable PRD reports, 'Made with PRD Refiner' badge)
- Growth experiments backlog (10 experiments, prioritized by ICE)"
```

### Bước 10.3 — Funnel Analysis
**Skill**: `funnel-audit`
```
Prompt: "Audit PRD Refiner funnel:
Awareness → Visit → Signup → Activate → Convert → Retain → Refer
Tìm drop-off points lớn nhất và optimization opportunities."
```

### Bước 10.4 — Revenue Simulation
**Skill**: `simulate`
```
Prompt: "Monte Carlo simulation cho PRD Refiner Year 1:
Scenarios: 
- Conservative: 500 signups/month, 5% conversion, $19 ARPU
- Base: 1,500 signups/month, 8% conversion, $25 ARPU  
- Optimistic: 3,000 signups/month, 12% conversion, $30 ARPU
Include: MRR projection, CAC payback, runway analysis."
```

**✅ Checkpoint Phase 10**: Growth engines identified & modeled.

---

## Phase 11: Analytics & Optimization 📈
**Mục tiêu**: Data-driven decision making.

### Bước 11.1 — Performance Dashboard
**Skill**: `executive-dashboard`
```
Prompt: "Thiết kế executive dashboard cho PRD Refiner:
- MRR, churn, CAC, LTV, NRR
- Funnel conversion rates by stage
- Channel performance (organic, paid, email, referral)
- Feature adoption metrics"
```

### Bước 11.2 — Attribution Model
**Skill**: `attribution-model`
```
Prompt: "Thiết lập multi-touch attribution cho PRD Refiner:
First-touch vs last-touch vs linear vs time-decay.
Channel: organic search, LinkedIn, TikTok, referral, direct.
Goal: understand true CAC by channel."
```

### Bước 11.3 — Anomaly Detection
**Skill**: `anomaly-scan`
```
Prompt: "Thiết lập anomaly detection cho PRD Refiner:
- Alert khi: signup rate drop > 20%, churn spike, CAC increase > 30%
- Monitor: daily active users, PRDs created, conversion rate
- Auto-investigate root cause khi anomaly detected."
```

### Bước 11.4 — ROI Calculator
**Skill**: `roi-calculator`
```
Prompt: "Tính ROI cho PRD Refiner marketing channels:
- Content marketing: $500/month → X signups → Y paid
- LinkedIn Ads: $800/month → X signups → Y paid
- Google Ads: $700/month → X signups → Y paid
ROAS, CAC, LTV:CAC ratio by channel."
```

### Bước 11.5 — Cohort Analysis
**Skill**: `cohort-analysis`
```
Prompt: "Cohort analysis cho PRD Refiner users:
- Acquisition cohorts: monthly signup cohorts
- Retention curves: Day 1, 7, 30, 60, 90
- LTV by acquisition channel
- Feature adoption impact on retention"
```

**✅ Checkpoint Phase 11**: Analytics infrastructure complete.

---

## Phase 12: Scale & Reporting 🏆
**Mục tiêu**: Scale operations và communicate results.

### Bước 12.1 — Executive Summary
**Skill**: `exec-summary`
```
Prompt: "Tạo monthly executive summary cho PRD Refiner:
- Portfolio ROI
- Key wins & losses
- Strategic recommendations
- Competitive positioning update
- Next month priorities"
```

### Bước 12.2 — What-If Analysis
**Skill**: `what-if`
```
Prompt: "What-if analysis cho Q2 budget allocation:
- Scenario A: double content budget, cut paid
- Scenario B: double paid budget, maintain content
- Scenario C: invest 50% in referral program
- Scenario D: balanced growth across all channels
Compare: projected signups, CAC, MRR impact."
```

### Bước 12.3 — Intelligence Report
**Skill**: `intelligence-report`
```
Prompt: "Tạo monthly intelligence report cho PRD Refiner:
- Compound learnings từ tất cả campaigns
- Competitive intelligence updates
- AI visibility trends
- Actionable playbooks cho next period"
```

### Bước 12.4 — Knowledge Management
**Skill**: `learn` + `save-knowledge`
```
Prompt: "Lưu learnings từ tháng vừa qua:
- Channel nào hiệu quả nhất?
- Messaging nào convert tốt nhất?  
- Audience segment nào có LTV cao nhất?
- Content type nào drive nhiều signups nhất?"
```

**✅ Checkpoint Phase 12**: Insights captured, ready for next cycle.

---

## 🗺️ Dependency Map

```
Phase 1 (Brand) ─────┬──→ Phase 2 (Market Intel)
                      │              │
                      │              ▼
                      ├──→ Phase 3 (Audience) ──→ Phase 4 (Pricing)
                      │                                    │
                      │                                    ▼
                      └────────────────────────→ Phase 5 (Launch)
                                                     │
                                    ┌────────────────┼────────────────┐
                                    ▼                ▼                ▼
                              Phase 6 (SEO)   Phase 7 (Content)  Phase 8 (Paid)
                                    │                │                │
                                    └────────┬───────┘                │
                                             ▼                        │
                                      Phase 9 (Email) ◄──────────────┘
                                             │
                                             ▼
                                      Phase 10 (Growth)
                                             │
                                             ▼
                                      Phase 11 (Analytics)
                                             │
                                             ▼
                                      Phase 12 (Scale)
                                             │
                                             ▼
                                      ┌──────┴──────┐
                                      │  NEXT CYCLE  │
                                      └─────────────┘
```

---

## ⚡ Quick Start — Test Ngay 5 Phút

Nếu muốn test nhanh, chạy theo thứ tự:

1. **`brand-setup`** → Thiết lập brand cho PRD Refiner
2. **`competitor-analysis`** → Phân tích Notion AI, Productboard
3. **`audience-profile`** → Tạo buyer persona "PM tại startup"
4. **`content-brief`** → Brief cho 1 blog post
5. **`ad-creative`** → Tạo ad copy cho LinkedIn

---

## 📊 Skills Sử Dụng Theo Phase

| Phase | Skills | Số lượng |
|-------|--------|----------|
| 1. Brand | `brand-setup`, `import-guidelines`, `content-creator` | 3 |
| 2. Market Intel | `competitor-analysis`, `competitive-landscape`, `competitor-monitor`, `competitive-ads-extractor`, `narrative-landscape` | 5 |
| 3. Audience | `audience-intelligence`, `audience-profile`, `focus-group` | 3 |
| 4. Pricing | `pricing-strategy`, `pricing-test`, `startup-metrics-framework` | 3 |
| 5. Launch | `launch-strategy`, `launch-plan`, `campaign-plan` | 3 |
| 6. SEO | `keyword-research`, `tech-seo-audit`, `seo-implement`, `aeo-geo`, `rank-monitor` | 5 |
| 7. Content | `content-calendar`, `content-brief`, `content-creator`, `video-script`, `content-repurpose`, `eval-content` | 6 |
| 8. Paid | `paid-advertising`, `ad-creative`, `ab-test-plan`, `landing-page-audit`, `retargeting-strategy` | 5 |
| 9. Email | `email-sequence`, `marketing-automation`, `cro` | 3 |
| 10. Growth | `loop-detect`, `growth-engineering`, `funnel-audit`, `simulate` | 4 |
| 11. Analytics | `executive-dashboard`, `attribution-model`, `anomaly-scan`, `roi-calculator`, `cohort-analysis` | 5 |
| 12. Scale | `exec-summary`, `what-if`, `intelligence-report`, `learn`, `save-knowledge` | 5 |
| **Tổng** | | **50 skills** |

---

## 🎯 KPIs Theo Giai Đoạn

| Giai đoạn | KPI chính | Target Year 1 |
|-----------|-----------|---------------|
| Awareness | Website traffic, social impressions | 10K visits/month |
| Acquisition | Signups, CAC | 1,000 signups/month, CAC < $15 |
| Activation | First PRD created, Day 1 retention | 60% activation rate |
| Revenue | MRR, conversion rate | $10K MRR, 8% free→paid |
| Retention | Monthly churn, NRR | < 5% churn, > 100% NRR |
| Referral | Viral coefficient, referral rate | K-factor > 0.3 |
