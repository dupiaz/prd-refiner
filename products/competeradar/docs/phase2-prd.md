# CompeteRadar — Tài Liệu Yêu Cầu Sản Phẩm (PRD)

**Phiên bản**: 1.0  
**Ngày**: 2026-03-11  
**Tác giả**: AI-assisted (Workflow Test Phase 2)  
**Trạng thái**: 🟡 Bản nháp — Chờ duyệt

---

## 1. Tổng Quan

### Tên sản phẩm
**CompeteRadar** — Nền tảng Theo dõi Đối thủ Cạnh tranh bằng AI cho Founder Cá nhân

### Mô tả ngắn
> Tự động theo dõi đối thủ. Nhận bản tóm tắt AI hàng tuần. Đưa ra quyết định chiến lược trong 5 phút thay vì 5 tiếng.

### Vấn đề cần giải quyết
Founder cá nhân mất 3-5 tiếng/tuần để kiểm tra thủ công website, mạng xã hội và quảng cáo đối thủ. Các công cụ CI doanh nghiệp (Crayon, Klue) giá $12K-47K/năm — quá đắt. Các công cụ giá rẻ chỉ theo dõi được một chiều. Hiện chưa có giải pháp toàn diện, AI-first ở tầm giá $29-79/tháng.

### Giải pháp
Ứng dụng web tự phục vụ (self-serve) giúp:
1. Tự động giám sát website, nội dung, giá cả và quảng cáo đối thủ
2. Dùng AI để nổi bật những thay đổi quan trọng nhất
3. Gửi bản tóm tắt hành động hàng tuần qua email/Slack
4. Tạo battlecard so sánh có thể chia sẻ

---

## 2. Đối Tượng Người Dùng

### Chính: Founder cá nhân / Indie Hacker
- Đang xây dựng SaaS, thương mại điện tử, hoặc sản phẩm số
- Theo dõi 3-10 đối thủ
- Ngân sách: $20-50/tháng cho công cụ CI
- Mong muốn: tự động hóa, hành động được ngay, thiết lập nhanh

### Mở rộng (giai đoạn sau)
- Quản lý Marketing tại SMB (5-50 nhân viên)
- Chủ agency quản lý nhiều thương hiệu khách hàng

---

## 3. Tính Năng Cốt Lõi (MVP)

### F1: Thiết lập & Bảng điều khiển Đối thủ
**Ưu tiên**: P0 (Bắt buộc)

| Hạng mục | Chi tiết |
|----------|----------|
| Thêm đối thủ qua URL | Người dùng dán URL website → hệ thống tự nhận diện tên, logo, mô tả |
| Bảng điều khiển tổng quan | Hiển thị dạng lưới/danh sách tất cả đối thủ đang giám sát |
| Hồ sơ đối thủ | Trang riêng cho mỗi đối thủ: dòng thời gian các thay đổi |
| Giới hạn | Free: 2 đối thủ, Pro: 10, Team: 25 |

**Tiêu chí chấp nhận**:
- Thêm đối thủ trong < 30 giây
- Bảng điều khiển tải trong < 2 giây
- Tự nhận diện tên công ty và favicon từ URL

### F2: Công cụ Phát hiện Thay đổi
**Ưu tiên**: P0 (Bắt buộc)

| Loại giám sát | Theo dõi gì | Tần suất quét |
|----------------|-------------|---------------|
| Thay đổi website | Trang chủ, trang giá, trang tính năng | Hàng ngày |
| Blog/nội dung | Bài viết mới, cập nhật nội dung | Hàng ngày |
| Thay đổi giá | Mức giá, tên gói, danh sách tính năng | Hàng ngày |
| Hồ sơ mạng xã hội | Bio, số followers, tần suất đăng bài | Hàng ngày |
| Công nghệ sử dụng | Stack công nghệ (phát hiện kiểu BuiltWith) | Hàng tuần |

**Tiêu chí chấp nhận**:
- Phát hiện thay đổi có ý nghĩa (không phải sửa CSS nhỏ)
- So sánh trực quan cho thay đổi website (so sánh screenshot)
- Phân loại thay đổi: `giá`, `nội dung`, `tính năng`, `thương hiệu`, `khác`

### F3: Bản Tóm Tắt AI Hàng Tuần
**Ưu tiên**: P0 (Bắt buộc)

| Hạng mục | Chi tiết |
|----------|----------|
| Tóm tắt AI | Dùng AI (Gemini mặc định, xem mục 8.1) tóm tắt thay đổi quan trọng nhất từ tất cả đối thủ |
| Xếp hạng ưu tiên | Thay đổi xếp theo tầm quan trọng chiến lược (giá > nội dung > phụ) |
| Kênh gửi | Email (mặc định) + Slack webhook (tùy chọn) |
| Tần suất | Hàng tuần (sáng thứ Hai), có thể tùy chỉnh |
| Định dạng | Email HTML sạch đẹp với các mục hành động |

**Tiêu chí chấp nhận**:
- Bản tóm tắt nêu bật 5 thay đổi quan trọng nhất
- Mỗi thay đổi có: gì đã thay đổi, tại sao quan trọng, đề xuất hành động
- Người dùng click xem chi tiết diff đầy đủ

### F4: Tạo Battlecard
**Ưu tiên**: P1 (Nên có)

| Hạng mục | Chi tiết |
|----------|----------|
| Tự động tạo | Tạo bảng so sánh từ dữ liệu đang theo dõi |
| Mẫu | Dạng "Chúng ta vs Họ" với điểm mạnh/yếu |
| Xuất file | PDF, PNG, Markdown |
| Chia sẻ | Link công khai cho team/nhà đầu tư xem |

**Tiêu chí chấp nhận**:
- Battlecard tự cập nhật khi dữ liệu đối thủ thay đổi
- Thiết kế chuyên nghiệp, phù hợp cho pitch deck nhà đầu tư

### F5: Hệ thống Cảnh báo
**Ưu tiên**: P1 (Nên có)

| Hạng mục | Chi tiết |
|----------|----------|
| Cảnh báo tùy chỉnh | Đặt quy tắc như "thông báo khi đối thủ đổi giá" |
| Kênh | Email, Slack, thông báo trong app |
| Mức độ khẩn | 🔴 Quan trọng (giá), 🟡 Đáng chú ý (tính năng), 🔵 Thông tin (nội dung) |

---

## 4. Yêu Cầu Phi Chức Năng

| Hạng mục | Yêu cầu |
|----------|---------|
| **Hiệu năng** | Bảng điều khiển tải < 2s, API phản hồi < 500ms |
| **Quy mô** | Hỗ trợ 10.000 người dùng, 100.000 lượt giám sát |
| **Độ tin cậy** | Uptime 99.5%, quét hàng ngày hoàn thành trước 6h sáng UTC |
| **Bảo mật** | Sẵn sàng SOC2, mã hóa dữ liệu, xác thực OAuth2 |
| **Quyền riêng tư** | Tuân thủ GDPR, không lưu dữ liệu cá nhân ngoài xác thực |
| **Tiếp cận** | Tuân thủ WCAG 2.1 AA |

---

## 5. Ngoài Phạm Vi (Không làm trong MVP)

- ❌ Giám sát thời gian thực (quét hàng ngày là đủ)
- ❌ Ước tính chi tiêu quảng cáo (chưa có nguồn dữ liệu đáng tin cậy)
- ❌ Thu thập nội dung mạng xã hội (phức tạp pháp lý)
- ❌ App di động native (chỉ responsive web app)
- ❌ Tích hợp CRM (sau MVP)
- ❌ Báo cáo tùy chỉnh (sau MVP)

---

## 6. Chiến Lược Giá

### Mô hình Freemium

| Gói | Giá | Đối thủ | Quét | Tóm tắt AI | Battlecard |
|-----|-----|---------|------|------------|------------|
| **Miễn phí** | $0/tháng | 2 | Hàng ngày | Tóm tắt hàng tháng | ❌ |
| **Pro** | $29/tháng | 10 | Hàng ngày | Hàng tuần + theo yêu cầu | 5 active |
| **Team** | $79/tháng | 25 | Hàng ngày | Hàng tuần + cảnh báo tùy chỉnh | Không giới hạn |

### Lý do định giá
- **Miễn phí**: Rào cản thấp, tăng trưởng viral (chia sẻ battlecard)
- **Pro**: Phù hợp cho founder cá nhân ($29 ≈ ngân sách cà phê)
- **Team**: Tier cho agency/SMB với tính năng cộng tác

### Dự kiến Doanh thu (Năm 1)

| Tháng | Người dùng Free | Pro | Team | MRR |
|-------|-----------------|-----|------|-----|
| 3 | 500 | 25 | 5 | $1.120 |
| 6 | 2.000 | 80 | 15 | $3.505 |
| 12 | 8.000 | 250 | 50 | $11.200 |

---

## 7. Chiến Lược Ra Mắt

### Giai đoạn 1: Beta đóng (Tuần 1-4)
- 50 người dùng beta từ Indie Hackers / Twitter
- Trọng tâm: xác nhận product-market fit
- Vòng lặp phản hồi: gọi hàng tuần với 10 người dùng tích cực nhất

### Giai đoạn 2: Ra mắt công khai (Tuần 5-6)
- Ra mắt Product Hunt (mục tiêu top #3+ trong ngày)
- Bài đăng cộng đồng Indie Hackers
- Thread ra mắt trên Twitter/X từ tài khoản founder
- Bài "Show HN" trên Hacker News

### Giai đoạn 3: Tăng trưởng qua nội dung (Tuần 7-12)
- Blog hàng tuần: "Cẩm nang Theo dõi Đối thủ"
- SEO: nhắm từ khóa "competitor analysis tool", "competitive intelligence for startups"
- Template: Mẫu battlecard miễn phí (lead magnet)
- Đối tác: Tích hợp với các công cụ phổ biến cho founder

### Lịch Nội dung (Tháng đầu sau ra mắt)

| Tuần | Bài Blog | Mạng xã hội | Email |
|------|----------|-------------|-------|
| 1 | "Tại sao Founder thường bỏ qua đối thủ (và sao đó là sai lầm)" | Thread ra mắt (X), thông báo LinkedIn | Email chào mừng #1 |
| 2 | "5 Thay đổi Đối thủ bạn phải theo dõi hàng tuần" | Carousel mẹo (X, LinkedIn) | Email chào mừng #2 + tip |
| 3 | "Cách xây dựng lợi thế cạnh tranh khi là Solo Founder" | Thread testimonial người dùng | Email giới thiệu tính năng |
| 4 | "Mẫu Battlecard: Cách định vị so với bất kỳ đối thủ nào" | Tặng template miễn phí | Tổng kết tháng 1 + upsell |

---

## 8. Công Nghệ Đề Xuất

| Tầng | Công nghệ | Lý do |
|------|-----------|-------|
| **Frontend** | Next.js 14 + React | SSR, trải nghiệm dev tốt, triển khai Vercel dễ dàng |
| **Giao diện** | Tailwind CSS | Phát triển UI nhanh chóng |
| **Xác thực** | Better Auth | Tự host, linh hoạt |
| **Cơ sở dữ liệu** | Supabase (PostgreSQL) | Real-time, gói miễn phí hào phóng |
| **Thu thập dữ liệu** | Puppeteer + Cheerio | Giám sát website linh hoạt |
| **AI (Dev)** | Google Gemini 3.0 | Model mặc định giai đoạn phát triển |
| **AI (Prod)** | Multi-model qua LLM Gateway | Xem mục 8.1 bên dưới |
| **LLM Gateway** | Portkey / LiteLLM | Điều phối multi-provider, fallback, cost tracking |
| **Email** | Resend | Email giao dịch thân thiện developer |
| **Hosting** | Vercel | Triển khai dễ, edge functions |
| **Hàng đợi** | Inngest / BullMQ | Lên lịch job nền |
| **Phân tích** | PostHog | Product analytics |

### 8.1 Chiến Lược Lựa Chọn Mô Hình AI

#### Bối cảnh
- **Giai đoạn Dev**: Sử dụng **Google Gemini 3.0** làm model mặc định để phát triển và test
- **Giai đoạn Prod**: Cần quyết định ai có quyền thay đổi AI model — người dùng hay admin (founder nền tảng)?

#### Phân tích 3 phương án

| Tiêu chí | Phương án A: Admin-only | Phương án B: User BYOK | Phương án C: Hybrid ⭐ |
|----------|------------------------|------------------------|----------------------|
| **Mô tả** | Admin chọn 1 model duy nhất cho toàn platform | Người dùng mang API key riêng (Bring Your Own Key) | Admin đặt mặc định + User có thể BYOK |
| **Độ phức tạp** | 🟢 Thấp | 🔴 Cao | 🟡 Trung bình |
| **Chi phí AI cho platform** | 🔴 Platform chịu toàn bộ | 🟢 User tự trả | 🟡 Platform trả mặc định, BYOK giảm tải |
| **Trải nghiệm người dùng** | 🟢 Đơn giản, consistent | 🟡 Phức tạp hơn, cần hiểu API | 🟢 Đơn giản mặc định, linh hoạt khi cần |
| **Vendor lock-in** | 🔴 Cao (1 provider) | 🟢 Thấp (user chọn) | 🟢 Thấp |
| **Khả năng chịu lỗi** | 🔴 Single point of failure | 🟢 Đa dạng provider | 🟢 Fallback tự động |
| **Phù hợp MVP** | ✅ Nhanh nhất | ❌ Quá phức tạp | ✅ Cân bằng tốt |

#### Khuyến nghị: Phương án C — Hybrid (cần xác nhận)

**Giai đoạn MVP:**
1. **Admin (founder)** đặt model mặc định: Gemini 3.0
2. **Admin** có thể chuyển đổi giữa các model bất kỳ lúc nào (Gemini, GPT, Claude, v.v.)
3. Người dùng sử dụng model mặc định, **không cần lo chọn model**

**Giai đoạn Growth (sau PMF):**
4. Mở tùy chọn **BYOK** (Bring Your Own Key) cho gói Pro/Team
5. Người dùng BYOK tự trả chi phí AI → giảm chi phí cho platform
6. Thêm routing thông minh: chọn model tối ưu theo loại tác vụ tự động

**Kiến trúc kỹ thuật:**
```
┌─────────────┐     ┌──────────────────┐     ┌─────────────────┐
│  App Logic   │────▶│  LLM Gateway     │────▶│  AI Providers   │
│  (Next.js)   │     │  (Portkey/Lite)   │     │  ┌─ Gemini 3.0  │
│              │     │  • Routing        │     │  ├─ GPT-4       │
│              │     │  • Fallback       │     │  ├─ Claude      │
│              │     │  • Cost tracking  │     │  └─ Mistral     │
│              │     │  • Key management │     │                 │
└─────────────┘     └──────────────────┘     └─────────────────┘
```

**Lợi ích của kiến trúc LLM Gateway:**
- 🔄 **Fallback tự động**: Gemini lỗi → tự chuyển sang GPT/Claude
- 💰 **Theo dõi chi phí**: Biết chính xác chi phí AI per user/per feature
- 🔑 **Quản lý key**: Hỗ trợ cả platform key và BYOK user key
- 📊 **A/B test model**: So sánh chất lượng output giữa các model
- 🛡️ **Không vendor lock-in**: Chuyển model không cần sửa code

### 8.2 Kết Quả Nghiên Cứu — 5 Câu Hỏi AI

#### Câu 1: Chi phí thực tế Gemini 3.0 cho CompeteRadar

**Ước tính chi phí AI per user/tháng:**

| Tác vụ | Tần suất | Input tokens | Output tokens | Model đề xuất |
|--------|----------|-------------|--------------|---------------|
| Phân tích thay đổi website (10 đối thủ) | Hàng ngày × 30 | ~50K/lần | ~5K/lần | Gemini 3 Flash |
| Tạo weekly digest | 4 lần/tháng | ~20K/lần | ~3K/lần | Gemini 3 Pro |
| Tạo battlecard | 5 lần/tháng | ~10K/lần | ~5K/lần | Gemini 3 Pro |

**Ước tính chi phí (Gemini 3 Flash + Pro kết hợp):**

| Hạng mục | Input tokens/tháng | Output tokens/tháng | Chi phí |
|----------|-------------------|---------------------|---------|
| Phân tích thay đổi (Flash) | 1.5M | 150K | $0.50 × 1.5 + $3.00 × 0.15 = **$1.20** |
| Digest + Battlecard (Pro) | 130K | 37K | $2.00 × 0.13 + $12.00 × 0.037 = **$0.70** |
| **Tổng chi phí AI / user / tháng** | | | **~$1.90** |

> ✅ **Kết luận**: Chi phí AI ~$2/user/tháng rất bền vững. Với giá Pro $29/tháng → AI chiếm ~7% doanh thu. Dùng Flash cho tác vụ lớn + Pro cho tóm tắt chất lượng cao.

#### Câu 2: So sánh chất lượng output — chiến lược thay vì benchmark ngay

| Model | Điểm mạnh | Phù hợp cho tác vụ nào |
|-------|-----------|------------------------|
| **Gemini 3 Flash** | Nhanh, rẻ, tốt cho structured data | Phân tích thay đổi website, phân loại changes |
| **Gemini 3 Pro** | Reasoning tốt, giá hợp lý | Weekly digest tóm tắt chiến lược |
| **GPT-4o** | Viết tốt, creative | Battlecard copy, so sánh marketing |
| **Claude 3.5 Sonnet** | An toàn, chính xác, dài | Phân tích cạnh tranh chi tiết |

> ✅ **Kết luận**: Không cần benchmark trước MVP. Thay vào đó:
> - Dùng **Gemini 3 Flash/Pro làm mặc định** (rẻ nhất, chất lượng đủ tốt)
> - Kiến trúc LLM Gateway cho phép **A/B test model tự động** sau launch
> - Thu thập user feedback qua 👍/👎 button trên mỗi digest → dữ liệu thực để quyết định

#### Câu 3: Latency — không phải vấn đề

| Tác vụ | Loại xử lý | Latency chấp nhận | Giải pháp |
|--------|-----------|-------------------|-----------|
| Phân tích thay đổi | **Batch** (cron job hàng ngày) | Không giới hạn | Background job, cache kết quả |
| Weekly digest | **Batch** (cron job hàng tuần) | Không giới hạn | Pre-generate, send email scheduled |
| Battlecard generation | **On-demand** (user click) | < 15 giây | Stream response, loading animation |
| Real-time alert | **Near-realtime** | < 5 phút | Queue-based, trigger khi phát hiện change |

> ✅ **Kết luận**: 90% tác vụ AI là batch processing → latency không phải concern. Chỉ battlecard generation cần optimized (~15s acceptable vì user hiểu AI đang "suy nghĩ"). Dùng streaming response cho UX tốt hơn.

#### Câu 4: Chính sách giá BYOK

Dựa trên khảo sát các SaaS có BYOK (Chatbot builders, AI tools):

| Mô hình | Mô tả | Ứng dụng cho CompeteRadar |
|---------|-------|---------------------------|
| **Giảm giá cố định** | BYOK users trả ít hơn 30-40% subscription | Pro BYOK: $19/tháng (thay vì $29) |
| **Loại bỏ giới hạn** | BYOK users được thêm quota vì platform không trả AI cost | BYOK Pro: 20 đối thủ thay vì 10 |
| **Tier riêng** | Tạo tier BYOK riêng biệt | Thêm gói "Pro BYOK" giữa Pro và Team |

**Đề xuất cho CompeteRadar:**

| Gói | Giá bình thường | Giá BYOK | Lợi ích BYOK thêm |
|-----|-----------------|----------|-------------------|
| **Pro** | $29/tháng | $19/tháng (-35%) | 15 đối thủ (thay vì 10) |
| **Team** | $79/tháng | $49/tháng (-38%) | 40 đối thủ (thay vì 25) |

> ✅ **Kết luận**: Giảm 35-38% cho BYOK users. Win-win: user trả ít hơn tổng thể, platform tiết kiệm chi phí AI ~$2/user/tháng. BYOK chỉ mở ở giai đoạn Growth (sau PMF), không phải MVP.

#### Câu 5: UX cho BYOK — Thiết kế trang cài đặt

**Best practices đã nghiên cứu:**

```
┌─ Settings ─────────────────────────────────────────────┐
│                                                         │
│  🤖 AI Model                                           │
│  ┌─────────────────────────────────────────────┐       │
│  │ Đang dùng: ✅ Gemini 3.0 (mặc định platform)│       │
│  │ Chất lượng: ⭐⭐⭐⭐ (dựa trên user feedback) │       │
│  └─────────────────────────────────────────────┘       │
│                                                         │
│  🔑 BYOK — Bring Your Own Key          [Pro/Team only]  │
│  ┌─────────────────────────────────────────────┐       │
│  │ Dùng API key riêng để giảm 35% phí          │       │
│  │                                               │       │
│  │ Provider:  [▼ OpenAI    ]                     │       │
│  │ API Key:   [sk-••••••••••••••••] [👁] [📋]    │       │
│  │ Status:    🟢 Connected — last tested 2m ago  │       │
│  │                                               │       │
│  │ [Test Connection]  [Save]  [Remove Key]       │       │
│  └─────────────────────────────────────────────┘       │
│                                                         │
│  ⚙️ Nâng cao                                           │
│  • Auto-fallback nếu key lỗi: [✅ Bật]                 │
│  • Nhắc rotate key mỗi 90 ngày: [✅ Bật]               │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

**Nguyên tắc UX BYOK:**
1. **Mặc định hoạt động mà không cần BYOK** — user không bao giờ bị block
2. **Mask key** sau khi lưu — chỉ hiện 4 ký tự cuối (sk-••••1234)
3. **Test connection** tự động khi lưu — feedback ngay lập tức
4. **Auto-fallback** về platform key nếu BYOK key hết quota/lỗi
5. **Nhắc rotate** mỗi 90 ngày — popup nhẹ, không ép buộc
6. **Hướng dẫn step-by-step** cho từng provider (link đến docs tạo key)
7. **Encrypted at rest** — key mã hóa AES-256 trong database

> ✅ **Kết luận**: BYOK UX phải invisible cho user bình thường. Chỉ power users cần biết đến. Đặt trong Settings > AI Model, không phải trang chính.

---

## 9. Chỉ Số Thành Công

### Thành công MVP (Tháng 3)
- [ ] 500+ lượt đăng ký
- [ ] 50+ người dùng hoạt động hàng tuần
- [ ] 25+ người dùng trả phí
- [ ] Tỷ lệ giữ chân tuần 4 > 40%
- [ ] NPS > 40

### Product-Market Fit (Tháng 6)
- [ ] Điểm "Rất thất vọng" > 40% (phương pháp Sean Ellis)
- [ ] Tăng trưởng tự nhiên > 30% đăng ký mới
- [ ] MRR > $3.000
- [ ] Churn hàng tháng < 5% trên gói trả phí

---

## 10. Rủi Ro & Biện Pháp Giảm Thiểu

| Rủi ro | Ảnh hưởng | Xác suất | Biện pháp |
|--------|-----------|----------|-----------|
| Web scraping bị chặn | Cao | Trung bình | Dùng nhiều chiến lược scraping, dự phòng API |
| Chi phí AI cao | Trung bình | Trung bình | Cache phản hồi AI, xử lý theo batch, giới hạn sử dụng |
| Tỷ lệ activation thấp | Cao | Trung bình | Wizard onboarding, template đối thủ sẵn có |
| Cạnh tranh từ Owler/Panoramata | Trung bình | Thấp | Tập trung vào khác biệt hóa AI digest, iteration nhanh hơn |
| Vấn đề GDPR/pháp lý khi scraping | Trung bình | Thấp | Chỉ scrape dữ liệu công khai, tuân thủ robots.txt |

---

## 11. User Stories (MVP)

### Epic 1: Quản lý Đối thủ
- US-1.1: Là founder, tôi có thể thêm đối thủ bằng cách dán URL của họ
- US-1.2: Là founder, tôi có thể xem tất cả đối thủ trong bảng điều khiển
- US-1.3: Là founder, tôi có thể xóa đối thủ khỏi danh sách theo dõi
- US-1.4: Là founder, tôi có thể xem dòng thời gian thay đổi của mỗi đối thủ

### Epic 2: Phát hiện Thay đổi
- US-2.1: Là founder, tôi nhận được quét hàng ngày về website đối thủ
- US-2.2: Là founder, tôi có thể xem so sánh trực quan các thay đổi website
- US-2.3: Là founder, thay đổi được phân loại theo loại (giá, nội dung, tính năng)

### Epic 3: Tóm tắt AI
- US-3.1: Là founder, tôi nhận email hàng tuần tóm tắt thay đổi đối thủ
- US-3.2: Là founder, bản tóm tắt nêu bật thay đổi quan trọng nhất về chiến lược
- US-3.3: Là founder, tôi có thể cấu hình tần suất và kênh gửi tóm tắt

### Epic 4: Battlecard
- US-4.1: Là founder, tôi có thể tạo battlecard so sánh với bất kỳ đối thủ nào
- US-4.2: Là founder, tôi có thể xuất battlecard dạng PDF/PNG
- US-4.3: Là founder, tôi có thể chia sẻ battlecard qua link công khai

### Epic 5: Xác thực & Cài đặt
- US-5.1: Là người dùng, tôi có thể đăng ký bằng email hoặc Google
- US-5.2: Là người dùng, tôi có thể quản lý đăng ký và thanh toán
- US-5.3: Là người dùng, tôi có thể cấu hình tùy chọn thông báo

---

## 12. Từ Điển Thuật Ngữ

Giải thích các thuật ngữ trong tài liệu này, sắp xếp theo nhóm để dễ tra cứu.

### Thuật ngữ Sản phẩm & Kinh doanh

| Thuật ngữ | Giải thích |
|-----------|-----------|
| **PRD** (Product Requirements Document) | Tài liệu yêu cầu sản phẩm — văn bản mô tả chi tiết sản phẩm cần xây dựng |
| **MVP** (Minimum Viable Product) | Sản phẩm khả dụng tối thiểu — phiên bản đầu tiên chỉ có tính năng cốt lõi |
| **SaaS** (Software as a Service) | Phần mềm dạng dịch vụ — người dùng trả phí hàng tháng để sử dụng qua web |
| **Micro SaaS** | SaaS quy mô nhỏ, thường do 1-2 người vận hành, nhắm vào niche cụ thể |
| **ICP** (Ideal Customer Profile) | Chân dung khách hàng lý tưởng — mô tả chi tiết người dùng mục tiêu |
| **PMF** (Product-Market Fit) | Sản phẩm-Thị trường phù hợp — khi sản phẩm thực sự giải quyết nhu cầu thị trường |
| **Freemium** | Mô hình kết hợp gói miễn phí (Free) + gói trả phí cao cấp (Premium) |
| **BYOK** (Bring Your Own Key) | Mang key riêng — người dùng tự cung cấp API key của họ thay vì dùng key platform |
| **Battlecard** | Bảng so sánh cạnh tranh — tài liệu 1-2 trang so sánh sản phẩm của mình với đối thủ |
| **CI** (Competitive Intelligence) | Tình báo cạnh tranh — hoạt động thu thập và phân tích thông tin đối thủ |
| **Lead magnet** | Nội dung miễn phí có giá trị (ebook, template...) dùng để thu hút email đăng ký |
| **Upsell** | Nâng cấp bán hàng — khuyến khích người dùng chuyển từ gói rẻ sang gói đắt hơn |

### Thuật ngữ Chỉ số & Đo lường

| Thuật ngữ | Giải thích |
|-----------|-----------|
| **MRR** (Monthly Recurring Revenue) | Doanh thu định kỳ hàng tháng — tổng tiền thu được từ tất cả subscription |
| **ARR** (Annual Recurring Revenue) | Doanh thu định kỳ hàng năm = MRR × 12 |
| **CAC** (Customer Acquisition Cost) | Chi phí thu hút khách hàng — tổng chi phí marketing ÷ số khách hàng mới |
| **LTV** (Lifetime Value) | Giá trị vòng đời khách hàng — tổng doanh thu 1 khách hàng mang lại |
| **ARPU** (Average Revenue Per User) | Doanh thu trung bình mỗi người dùng |
| **Churn** | Tỷ lệ rời bỏ — % người dùng hủy subscription mỗi tháng |
| **NPS** (Net Promoter Score) | Chỉ số đo lường mức độ hài lòng của khách hàng (thang -100 đến 100) |
| **AARRR** (Acquisition, Activation, Retention, Revenue, Referral) | Khung đo lường 5 giai đoạn vòng đời khách hàng, còn gọi là "Pirate Metrics" |
| **Retention** | Tỷ lệ giữ chân — % người dùng tiếp tục sử dụng sản phẩm sau một khoảng thời gian |
| **Activation** | Kích hoạt — khi người dùng mới thực hiện hành động có giá trị lần đầu (ví dụ: thêm đối thủ đầu tiên) |
| **Sean Ellis Test** | Phương pháp đo PMF: hỏi "Bạn sẽ cảm thấy thế nào nếu không dùng sản phẩm này nữa?" — nếu >40% trả lời "Rất thất vọng" = có PMF |

### Thuật ngữ Kỹ thuật

| Thuật ngữ | Giải thích |
|-----------|-----------|
| **API** (Application Programming Interface) | Giao diện lập trình — cách các phần mềm giao tiếp với nhau |
| **API Key** | Chìa khóa API — mã bí mật để xác thực khi gọi API |
| **LLM** (Large Language Model) | Mô hình ngôn ngữ lớn — AI có khả năng hiểu và tạo văn bản (ví dụ: Gemini, GPT, Claude) |
| **LLM Gateway** | Cổng điều phối LLM — lớp trung gian quản lý nhiều nhà cung cấp AI, xử lý fallback và theo dõi chi phí |
| **Token** | Đơn vị xử lý văn bản của AI — khoảng 0.75 từ tiếng Anh hoặc 0.5 từ tiếng Việt = 1 token |
| **SSR** (Server-Side Rendering) | Render phía server — kỹ thuật tạo trang web trên server trước khi gửi đến trình duyệt, giúp tải nhanh hơn |
| **OAuth2** | Tiêu chuẩn xác thực — cho phép đăng nhập bằng tài khoản bên thứ 3 (Google, GitHub...) |
| **Webhook** | Cơ chế gọi ngược — hệ thống A tự động gửi thông báo đến hệ thống B khi có sự kiện xảy ra |
| **Scraping** | Thu thập dữ liệu web — sử dụng phần mềm tự động đọc và trích xuất nội dung từ website |
| **Cron job** | Tác vụ lên lịch — chương trình chạy tự động theo lịch định kỳ (ví dụ: quét đối thủ hàng ngày lúc 5h sáng) |
| **Batch processing** | Xử lý hàng loạt — xử lý nhiều tác vụ cùng lúc theo lịch, thay vì từng cái một theo thời gian thực |
| **Streaming response** | Phản hồi dạng luồng — hiển thị kết quả AI từng phần khi đang tạo, thay vì chờ xong mới hiện |
| **Fallback** | Phương án dự phòng — tự động chuyển sang hệ thống thay thế khi hệ thống chính gặp lỗi |
| **Edge functions** | Hàm biên — mã chạy trên server gần người dùng nhất, giúp phản hồi nhanh hơn |
| **WCAG 2.1 AA** | Tiêu chuẩn tiếp cận web — đảm bảo website có thể sử dụng được bởi người khuyết tật |
| **GDPR** | Quy định bảo vệ dữ liệu chung của Liên minh Châu Âu |
| **SOC2** | Tiêu chuẩn bảo mật — chứng nhận rằng hệ thống xử lý dữ liệu an toàn |
| **AES-256** | Thuật toán mã hóa mạnh — tiêu chuẩn mã hóa dữ liệu mức quân sự |

### Thuật ngữ Marketing

| Thuật ngữ | Giải thích |
|-----------|-----------|
| **SEO** (Search Engine Optimization) | Tối ưu hóa công cụ tìm kiếm — làm cho website xuất hiện cao trên Google |
| **Product Hunt** | Nền tảng ra mắt sản phẩm công nghệ mới — nơi startup giới thiệu sản phẩm lần đầu |
| **Indie Hackers** | Cộng đồng online cho những người xây dựng sản phẩm/startup một mình hoặc nhóm nhỏ |
| **Content-led growth** | Tăng trưởng dẫn dắt bởi nội dung — thu hút khách hàng bằng blog, video, podcast... |
| **Visual diff** | So sánh trực quan — hiển thị sự khác biệt giữa 2 phiên bản bằng hình ảnh |

### Thuật ngữ Công nghệ cụ thể (Tech Stack)

| Thuật ngữ | Giải thích |
|-----------|-----------|
| **Next.js** | Framework React phổ biến nhất để xây dựng web app — hỗ trợ SSR, routing tự động |
| **React** | Thư viện JavaScript để xây dựng giao diện người dùng — do Meta phát triển |
| **Tailwind CSS** | Framework CSS utility-first — viết giao diện nhanh bằng các class có sẵn |
| **Better Auth** | Thư viện xác thực mã nguồn mở cho TypeScript — thay thế cho Auth0, Clerk |
| **Supabase** | Nền tảng backend mã nguồn mở — cung cấp database, auth, storage, realtime |
| **PostgreSQL** | Hệ quản trị cơ sở dữ liệu quan hệ mã nguồn mở, mạnh mẽ và phổ biến |
| **Puppeteer** | Thư viện Node.js để điều khiển trình duyệt tự động — dùng cho scraping và testing |
| **Cheerio** | Thư viện Node.js để phân tích HTML — nhanh hơn Puppeteer cho scraping đơn giản |
| **Gemini** | Mô hình AI của Google — có nhiều phiên bản: Flash (nhanh, rẻ), Pro (chất lượng cao) |
| **GPT-4o** | Mô hình AI của OpenAI — tốt cho viết sáng tạo và phân tích phức tạp |
| **Claude** | Mô hình AI của Anthropic — nổi bật về độ chính xác và an toàn |
| **Portkey / LiteLLM** | Công cụ LLM Gateway — quản lý nhiều nhà cung cấp AI từ 1 interface |
| **Resend** | Dịch vụ gửi email cho developer — API đơn giản, tỷ lệ vào inbox cao |
| **Vercel** | Nền tảng hosting web — triển khai Next.js nhanh chóng, CDN toàn cầu |
| **Inngest / BullMQ** | Thư viện quản lý hàng đợi tác vụ — lên lịch và xử lý job nền |
| **PostHog** | Công cụ phân tích sản phẩm mã nguồn mở — thay thế cho Google Analytics + Mixpanel |

---

*PRD v1.1 — Cập nhật nghiên cứu AI + Từ điển thuật ngữ — Sẵn sàng cho HITL Review*
