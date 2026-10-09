# Worksheet — UniAdmit AI (Trợ lý AI Tư vấn Tuyển sinh Đại học)

Họ tên: Nguyễn Thị Minh Khánh · MSSV: 2A202602546 · Ngày làm: 09/10/2026

### Số liệu đầu vào từ Mô hình tài chính & Value Metric:
- **ARPU:** 120.000.000 VNĐ / trường ĐH / năm (tương đương 10.000.000 VNĐ / tháng)
- **Gross Margin (mục tiêu):** 68%
- **CAC:** 24.000.000 VNĐ / trường ĐH
- **CAC Payback mục tiêu:** 3.5 tháng (< 6 tháng)
- **Runway:** 14 tháng
- **Value Metric:** Số phiên tư vấn tuyển sinh hoàn thành thành công (Resolved Admissions Sessions)
- **Cost/Job:** 800 VNĐ / phiên tư vấn thành công (~$0.032 / session với RAG tra cứu đề án & quy chế)

---

## Trạm 1 — Loại mô hình

**Câu chốt loại:** Chúng tôi là **B2B2C** vì tiền đến từ các trường Đại học/Cao đẳng (phí thuê bao giải pháp AI tuyển sinh hàng năm), người dùng thật là Thí sinh & Phụ huynh tìm hiểu thông tin xét tuyển, và chúng tôi trực tiếp chạm được họ qua Widget Chatbot AI thông minh nhúng trực tiếp trên cổng thông tin tuyển sinh của từng trường.

**Bảng đèn §3 của loại mình** (ghi đủ mọi đèn trong bảng B2B2C — HANDBOOK §3.3):

| Đèn | ✅ / 🔧 / ❌ | Số nằm ở đâu / cần gì để đo |
|---|---|---|
| **Partner activation rate** ⭐ | ✅ | Hệ thống Backend log: đếm số trường ĐH đã go-live có ≥1 thí sinh thật chat trong 30 ngày |
| **End-user reach trong partner** | 🔧 | Cần gắn Google Analytics / Web tracker trên trang tuyển sinh của trường để so sánh traffic tổng vs lượt mở chat |
| **Time-to-first-end-user** | ✅ | CRM Hubspot (ngày ký nghiệm thu/go-live) đối chiếu timestamp phiên chat đầu tiên trên Database |
| **Volume volatility** | ✅ | Server DB / APM: tính độ lệch chuẩn số lượt truy vấn & token tiêu thụ giữa các tháng (cao điểm vs thấp điểm) |
| **GM sau rev-share / chi phí kênh** | ✅ | Báo cáo tài chính nội bộ: Doanh thu trừ chi phí hạ tầng Cloud & API OpenAI/Anthropic |
| **Chi phí inference ÷ doanh thu (theo từng partner)** | ✅ | Hệ thống API Gateway gắn tag `partner_id` cho từng token call, chia cho MRR từng trường |
| **Tập trung volume** | ✅ | Dashboard hệ thống: % volume token của trường lớn nhất so với tổng token toàn bộ hệ thống |
| **Chất lượng nhìn từ end-user** | 🔧 | Cần gắn widget khảo sát đánh giá hài lòng (CSAT 👍/👎) và webhook bắt tỷ lệ escalate sang tư vấn viên người |
| **Doanh thu/partner & Partner NRR & GM tổng** | ❌ | Chưa đo được: Cần chạy qua 1 chu kỳ tuyển sinh trọn vẹn 12 tháng để có dữ liệu tái ký hợp đồng |


## Trạm 2 — Thẻ đèn

**North Star:** Số phiên tư vấn tuyển sinh thành công có xác nhận / tuần (Weekly Resolved Admissions Sessions) — hiện tại: 1.200 phiên/tuần — mục tiêu: 10.000 phiên/tuần (khi mở rộng 15 trường ĐH)

| # | Tầng (L/O/G) | Đèn | Định nghĩa (đếm gì · **không** đếm gì) | Công thức | Nhịp · ai lấy số | Báo trước cho |
|---|---|---|---|---|---|---|
| 1 | **L** | **Partner Activation Rate** ⭐ | % trường ĐH đã go-live có **≥10 phiên hỏi đáp thật từ thí sinh** trong 14 ngày đầu. **Không** tính lượt test nội bộ của ban tuyển sinh trường | `(Số trường ĐH có ≥10 phiên chat thật trong 14 ngày) ÷ (Tổng số trường ĐH đã go-live trong kỳ) × 100%` | Hằng tuần · Product Lead (log backend) | Đèn 3 (End-user reach) → Đèn 7 (Partner NRR) |
| 2 | **L** | **Time-to-First-End-User (TTFEU)** | Số ngày từ lúc ký hợp đồng bàn giao đến khi widget ghi nhận **phiên tư vấn đầu tiên từ thí sinh thật**. **Không** đếm thời gian cấu hình dữ liệu test | `Timestamp phiên chat thật đầu tiên − Timestamp ký hợp đồng (tính theo ngày)` | Mỗi trường mới · CS Lead (CRM + DB log) | Đèn 1 (Partner Activation) → Đèn 5 (Chi phí/Doanh thu) |
| 3 | **O** | **End-User Reach Rate** | % thí sinh truy cập trang tuyển sinh của trường có mở widget và gửi **≥2 câu hỏi**. **Không** đếm lượt click mở widget rồi đóng ngay (<10 giây) | `(Số unique visitors có ≥2 câu hỏi) ÷ (Tổng unique visitors vào trang tuyển sinh trường) × 100%` | Hằng tuần · Growth Lead (GA4 + DB) | Đèn 4 (Chất lượng AI) → Đèn 7 (Partner NRR) |
| 4 | **O** | **Tỷ lệ tư vấn chính xác & không bị Escalate (SLA Quality)** | % phiên tư vấn AI trả lời đúng quy chế/ngành học và thí sinh hài lòng (không bấm escalate chuyển tư vấn viên người). **Không** đếm câu hỏi ngoài phạm vi tuyển sinh | `(Tổng phiên tư vấn hợp lệ − Phiên bị escalate/đánh giá 👎) ÷ (Tổng phiên tư vấn hợp lệ) × 100%` | Hằng tuần · AI/Prompt Engineer (Log chat & Webhook) | Đèn 7 (Partner NRR — tỷ lệ tái ký hợp đồng) |
| 5 | **O** | **Chi phí Inference ÷ Doanh thu (theo từng partner)** ⭐ | Tỷ lệ chi phí API token (OpenAI/Anthropic) tiêu thụ của một trường ĐH so với doanh thu gói tháng của trường đó. **Không** lấy trung bình toàn bộ mà tính riêng từng trường | `(Tổng chi phí token LLM của trường X trong tháng) ÷ (Doanh thu MRR từ trường X) × 100%` | Hằng tháng · Tech Lead & Kế toán (APM Gateway log) | Đèn 6 (Gross Margin thực tế) |
| 6 | **G** | **Gross Margin (Biên lợi nhuận gộp)** | Tỷ lệ lợi nhuận gộp sau khi trừ chi phí server cloud, token LLM và support kỹ thuật trực tiếp. **Không** trừ chi phí bán hàng/marketing (thuộc OpEx/CAC) | `(Tổng Doanh thu − Tổng COGS gồm token & cloud) ÷ (Tổng Doanh thu) × 100%` | Hằng tháng / Quý · Finance Lead (P&L report) | Runway & Lợi nhuận ròng công ty |
| 7 | **G** | **Partner NRR (Net Revenue Retention)** | % doanh thu duy trì và mở rộng từ nhóm trường ĐH đã ký sau 1 chu kỳ 12 tháng (gồm phí gia hạn và nâng cấp gói volume). **Không** tính doanh thu từ trường mới ký | `(Doanh thu gia hạn + Doanh thu upsell từ các trường cũ) ÷ (Doanh thu ban đầu của nhóm trường đó) × 100%` | Hằng quý / năm · Head of Sales (CRM & ERP) | Định giá công ty & LTV/CAC dài hạn |

**Đèn chi phí AI là đèn số:** 5 (Chi phí Inference ÷ Doanh thu theo từng partner)


## Trạm 3 — Ngưỡng

| # | Đèn | 🟢 | 🟡 | 🔴 | Nguồn [BM]/[MH]/[TB] | Lý do (1 câu) · ngày kiểm tra nếu [BM] |
|---|---|---|---|---|---|---|
| 1 | **Partner Activation Rate** ⭐ | $\ge 60\%$ | $35\% - 60\%$ | $< 35\%$ | **[TB]** | Chưa có chuẩn ngành EdTech B2B2C tại VN; đo baseline 2 chu kỳ tuyển sinh và chốt mốc ngày 30/11/2026. |
| 2 | **Time-to-First-End-User (TTFEU)** | $< 14$ ngày | $14 - 30$ ngày | $> 30$ ngày | **[TB]** | Chu kỳ tuyển sinh gấp rút; quá 30 ngày chưa có thí sinh hỏi chứng tỏ trường chưa gắn widget, pilot thất bại. Chốt baseline 30/11/2026. |
| 3 | **End-User Reach Rate** | $\ge 15\%$ | $8\% - 15\%$ | $< 8\%$ | **[TB]** | Tỷ lệ tương tác widget hỗ trợ tuyển sinh chuẩn; $< 8\%$ sau 60 ngày cho thấy widget bị đặt khuất hoặc CTA kém hấp dẫn. |
| 4 | **Tỷ lệ tư vấn chính xác (SLA Quality)** | $\ge 92\%$ | $85\% - 92\%$ | $< 85\%$ | **[BM]** | Tư vấn tuyển sinh sai quy chế gây khủng hoảng truyền thông cho trường; chuẩn SLA chatbot hỗ trợ (Klarna Case Study & Gartner, kiểm tra ngày 09/10/2026). |
| 5 | **Chi phí Inference ÷ Doanh thu (từng partner)** ⭐ | $\le 20\%$ | $20\% - 35\%$ | $> 35\%$ | **[MH]** | Suy từ mô hình: với ARPU 10tr/tháng và GM mục tiêu 68%, ngân sách token tối đa 20%; nếu $> 35\%$ thì biên lợi nhuận bị ăn mòn nghiêm trọng (xem Phụ lục [MH] 1). |
| 6 | **Gross Margin (Biên lợi nhuận gộp)** | $\ge 65\%$ | $50\% - 65\%$ | $< 50\%$ | **[MH]** | Suy từ mô hình: Để thu hồi CAC 24tr trong $\le 4.8$ tháng với ARPU 10tr/tháng, GM bắt buộc phải $\ge 50\%$ (xem Phụ lục [MH] 2). |
| 7 | **Partner NRR (Net Revenue Retention)** | $\ge 110\%$ | $100\% - 110\%$ | $< 100\%$ | **[BM]** | NRR $< 100\%$ nghĩa là doanh thu mất đi do trường hủy bỏ lớn hơn doanh thu mở rộng/gia hạn (Benchmarkit SaaS Report trung vị ngành 101%, kiểm tra ngày 09/10/2026). |

### Phụ lục [MH] — phép tính (≥2)

**[MH] 1 — Chi phí Inference ÷ Doanh thu theo từng partner (Đèn số 5)**

```
Đầu vào (từ mô hình tài chính & Cost/Job của tôi):
- ARPU phân bổ theo tháng = 10.000.000 VNĐ / trường ĐH / tháng (tương đương 120.000.000 VNĐ / năm)
- Gross Margin mục tiêu của công ty = 68% → Tổng COGS tối đa cho phép = 10.000.000 × (1 − 68%) = 3.200.000 VNĐ / tháng
- Chi phí Server Cloud cố định, Database RAG & bảo trì phân bổ cho mỗi trường = 1.200.000 VNĐ / tháng
- Ngân sách tối đa còn lại dành cho API Inference Token LLM = 3.200.000 − 1.200.000 = 2.000.000 VNĐ / tháng

Phép tính:
- Tỷ lệ chi phí Inference tối ưu trên Doanh thu = 2.000.000 ÷ 10.000.000 = 20%
- Ngưỡng hòa vốn cận dưới (Gross Margin chạm ngưỡng tối thiểu 53% theo benchmark AI-native SaaS):
  Tổng COGS trần = 10.000.000 × (1 − 53%) = 4.700.000 VNĐ
  Chi phí Token trần = 4.700.000 − 1.200.000 = 3.500.000 VNĐ → Tỷ lệ Token / Doanh thu tối đa = 35%

Kết quả → 🟢 ≤ 20% · 🟡 20% – 35% · 🔴 > 35%
```

**[MH] 2 — Gross Margin tối thiểu theo CAC Payback mục tiêu (Đèn số 6)**

```
Đầu vào:
- Chi phí sở hữu đối tác (CAC) = 24.000.000 VNĐ / trường ĐH
- CAC Payback mục tiêu = 3.5 tháng; Payback tối đa chấp nhận được trước khi cạn Runway = 4.8 tháng
- Doanh thu phân bổ hàng tháng từ 1 trường = 10.000.000 VNĐ / tháng

Phép tính:
- Lợi nhuận gộp cần thu về mỗi tháng để hoàn vốn trong thời gian trần (4.8 tháng):
  Lãi gộp tối thiểu / tháng = CAC ÷ Payback tối đa = 24.000.000 ÷ 4.8 = 5.000.000 VNĐ / tháng
- Biên lợi nhuận gộp sàn (Gross Margin Floor):
  GM_floor = Lãi gộp tối thiểu ÷ Doanh thu tháng = 5.000.000 ÷ 10.000.000 = 50%
- Để đạt Payback mục tiêu lý tưởng 3.5 tháng:
  Lãi gộp mục tiêu = 24.000.000 ÷ 3.5 = 6.857.000 VNĐ / tháng → GM mục tiêu = 6.857.000 ÷ 10.000.000 ≈ 68% (lấy mốc 🟢 ≥ 65%)

Kết quả → 🟢 ≥ 65% · 🟡 50% – 65% · 🔴 < 50%
```


## Trạm 4 — 5 luật quyết định

Đánh dấu ⏹ cho luật dừng (cần ≥2 — ở đây có 4 luật dừng ⏹):

1. ⏹ **[Luật dừng — Đèn 1: Partner Activation Rate]**
   - **NẾU** Partner Activation Rate $< 35\%$ (dưới 35% trường ĐH go-live có $\ge 10$ thí sinh thật trong 14 ngày)
   - **TRONG** 3 đối tác trường ĐH liên tiếp được bàn giao
   - **THÌ** **dừng ngay việc ký biên bản nghiệm thu/go-live cho các trường mới trong pipeline; cử ngay đội Onboarding và CS đến làm việc trực tiếp với ban tuyển sinh trường để đưa widget AI ra vị trí nổi bật trên trang chủ và truyền thông cho học sinh trong 2 tuần.**
   - **KHÔNG THÌ** **không được tuyển thêm nhân viên Sales hoặc ký hợp đồng dồn dập** — ký thêm trường khi trường cũ chưa kích hoạt chỉ tạo ra đối tác rác và tăng chi phí hỗ trợ.

2. ⏹ **[Luật dừng — Đèn 5: Chi phí Inference ÷ Doanh thu từng partner (Chi phí AI)]**
   - **NẾU** Chi phí Inference $> 35\%$ doanh thu phân bổ tháng của một trường
   - **TRONG** 2 tuần liên tiếp
   - **VÀ** trường đó có lượng truy vấn $> 1.000$ phiên chat/tuần
   - **THÌ** **áp dụng ngay cơ chế Rate Limit tối đa 15 câu hỏi/thí sinh/ngày, kích hoạt Semantic Cache cho các câu hỏi phổ biến, và định tuyến tự động các câu hỏi tra cứu cơ bản sang model nhẹ hơn (như GPT-4o-mini).**
   - **KHÔNG THÌ** **không tự động bù tiền túi trả thêm phí API token không giới hạn hoặc tăng giá thuê bao đột ngột cho toàn bộ các trường khác.**

3. ⏹ **[Luật dừng — Đèn 2: Time-to-First-End-User]**
   - **NẾU** Time-to-First-End-User $> 30$ ngày
   - **TRÊN** 2 trường Đại học gần nhất
   - **THÌ** **đóng băng toàn bộ các yêu cầu phát triển tính năng tuỳ biến giao diện riêng biệt; chuyển sang áp dụng 1 đoạn mã nhúng Javascript chuẩn hóa và bộ tài liệu hướng dẫn kỹ thuật 1 trang cho đội IT của trường trong vòng 5 ngày.**
   - **KHÔNG THÌ** **không nhận thêm yêu cầu viết code tuỳ biến theo mong muốn riêng của trường** — tuỳ biến riêng lẻ làm tắc nghẽn toàn bộ đội kỹ thuật và kéo dài thời gian go-live.

4. **[Luật chất lượng — Đèn 4: Tỷ lệ tư vấn chính xác & SLA Quality]**
   - **NẾU** Tỷ lệ tư vấn chính xác & không bị escalate $< 85\%$
   - **TRONG** 7 ngày liên tiếp
   - **VÀ** ghi nhận $\ge 200$ phiên tư vấn
   - **THÌ** **chuyển chế độ AI sang "Strict RAG Mode" (chỉ trả lời khi độ tin cậy trích xuất quy chế $\ge 90\%$, nếu dưới mức này thì tự động chuyển form để tư vấn viên trường giải đáp trong 2 giờ), đồng thời đội Prompt Engineering phải cập nhật lại vector database của trường đó trong 48 giờ.**
   - **KHÔNG THÌ** **không được tắt toàn bộ hệ thống hoặc đổ lỗi cho dữ liệu đề án của trường** — phải bảo vệ uy tín tuyển sinh và ngăn chặn ảo giác (hallucination) ngay lập tức.

5. ⏹ **[Luật dừng / Chuyển hướng — Đèn 7 & 3: Partner NRR & End-User Reach]**
   - **NẾU** Partner NRR $< 100\%$ VÀ End-User Reach Rate $< 8\%$
   - **TRÊN** 2 quý liên tiếp (sau 1 chu kỳ năm học)
   - **THÌ** **dừng mô hình phân phối qua Widget website thụ động; chuyển hướng sang tích hợp trực tiếp trên Zalo OA / Fanpage tuyển sinh (nơi học sinh chủ động nhắn tin) và chuyển đổi hình thức tính phí sang thu theo hồ sơ nộp thành công (Success Fee).**
   - **KHÔNG THÌ** **không tiếp tục giảm giá gói thuê bao hàng năm để níu giữ các trường không có học sinh sử dụng.**

