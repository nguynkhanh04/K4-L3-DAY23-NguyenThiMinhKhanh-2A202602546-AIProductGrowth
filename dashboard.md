# OPERATING DASHBOARD — UniAdmit AI

**Loại mô hình:** B2B2C · **Cập nhật:** 09/10/2026 · Nguyễn Thị Minh Khánh – 2A202602546  
**NORTH STAR:** Số phiên tư vấn tuyển sinh thành công / tuần (Weekly Resolved Sessions) — **Hiện tại:** 1.200 — **Mục tiêu:** 10.000

---

### Đèn báo sớm (Leading — nhìn hằng ngày/tuần)

| Đèn | Hiện | 🟢 / 🟡 / 🔴 | Nguồn | Báo trước cho |
|---|---|---|---|---|
| **Partner Activation Rate** ⭐ | 50% (1/2 trường) | $\ge 60\%$ / $35\% - 60\%$ / $< 35\%$ | **[TB]** chốt 30/11/2026 | Reach thí sinh $\rightarrow$ Partner NRR |
| **Time-to-First-End-User (TTFEU)** | 18 ngày | $< 14$d / $14 - 30$d / $> 30$d | **[TB]** chốt 30/11/2026 | Partner Activation $\rightarrow$ Chi phí AI |

### Đèn vận hành (Operating — nhìn hằng tuần/tháng)

| Đèn | Hiện | 🟢 / 🟡 / 🔴 | Nguồn | Báo trước cho |
|---|---|---|---|---|
| **End-User Reach Rate** | 11% | $\ge 15\%$ / $8\% - 15\%$ / $< 8\%$ | **[TB]** chốt 30/11/2026 | SLA Quality $\rightarrow$ Partner NRR |
| **Tỷ lệ tư vấn chính xác (SLA Quality)** | 89% | $\ge 92\%$ / $85\% - 92\%$ / $< 85\%$ | **[BM]** 09/10/2026 | Partner NRR (tái ký HĐ) |
| **Chi phí Inference ÷ Doanh thu (từng partner)** ⭐ | 24% | $\le 20\%$ / $20\% - 35\%$ / $> 35\%$ | **[MH]** Phụ lục 1 | Gross Margin thực tế |

### Đèn kết quả (Lagging — nhìn hằng quý)

| Đèn | Hiện | 🟢 / 🟡 / 🔴 | Nguồn |
|---|---|---|---|
| **Gross Margin (Biên lợi nhuận gộp)** | 62% | $\ge 65\%$ / $50\% - 65\%$ / $< 50\%$ | **[MH]** Phụ lục 2 |
| **Partner NRR (Net Revenue Retention)** | Chưa đo (M12) | $\ge 110\%$ / $100\% - 110\%$ / $< 100\%$ | **[BM]** 09/10/2026 |

---

### 5 luật quyết định (⏹ = luật dừng)

1. ⏹ **NẾU** Partner Activation Rate $< 35\%$ **TRONG** 3 trường liên tiếp **THÌ** dừng ký nghiệm thu/go-live trường mới, tập trung CS hỗ trợ trường cũ đưa widget ra trang chủ trong 2 tuần **KHÔNG THÌ** không tuyển thêm Sales/ký thêm đối tác dồn dập.
2. ⏹ **NẾU** Chi phí Inference $> 35\%$ doanh thu **TRONG** 2 tuần **VÀ** volume $> 1.000$ chat/tuần **THÌ** bật Rate Limit 15 câu/user/ngày, Semantic Cache và chuyển câu hỏi cơ bản sang GPT-4o-mini **KHÔNG THÌ** không tự bù tiền túi trả thêm phí API token.
3. ⏹ **NẾU** TTFEU $> 30$ ngày **TRÊN** 2 trường gần nhất **THÌ** đóng băng tuỳ biến giao diện riêng, chuẩn hóa 1 script nhúng JS duy nhất trong 5 ngày **KHÔNG THÌ** không nhận code tuỳ biến riêng lẻ.
4. **NẾU** SLA tư vấn chính xác $< 85\%$ **TRONG** 7 ngày **VÀ** $\ge 200$ phiên **THÌ** bật Strict RAG Mode (chỉ trả lời khi tin cậy $\ge 90\%$, còn lại gửi form cho ban tuyển sinh), cập nhật vector DB trong 48h **KHÔNG THÌ** không tắt hệ thống.
5. ⏹ **NẾU** Partner NRR $< 100\%$ VÀ Reach $< 8\%$ **TRÊN** 2 quý **THÌ** dừng mô hình widget website thụ động, chuyển sang tích hợp Zalo OA và thu phí theo hồ sơ thành công (Success Fee) **KHÔNG THÌ** không tiếp tục giảm giá gói thuê bao năm.

---

### Cổng gác 90 ngày

| Ngày | Metric (1) | Ngưỡng qua cổng | Bằng chứng | Nếu trượt |
|---|---|---|---|---|
| **30** | Evidence Pack & Pipeline tích hợp | Hoàn thành Evidence Pack v1 + 3 trường ĐH ký biên bản thử nghiệm tích hợp | File `Evidence_Pack_v1.pdf` + 3 biên bản thỏa thuận kỹ thuật | **FIX:** Cắt phạm vi tích hợp xuống 1 khoa trọng điểm trong 30 ngày |
| **60** | Partner Activation & Chi phí AI | $\ge 2$ trường ĐH activate ($\ge 10$ thí sinh thật/trường) + log đo chi phí token riêng từng trường | APM Token Usage Report + Backend DB Logs $\ge 20$ session thật | **FIX:** Đổi vị trí đặt widget trên web trường; nếu trượt lần 2 $\rightarrow$ **PIVOT** sang Zalo OA |
| **90** | End-User Reach & Chi phí Inference | Reach $\ge 10\%$ **và** Chi phí Inference $\le 30\%$ Doanh thu trên $\ge 2$ trường | Pilot Report có xác nhận số liệu của Trưởng phòng Tuyển sinh trường ĐH | **PIVOT:** Chuyển sang thu phí Success Fee trên Zalo OA, hoặc **KILL** nếu GM $< 40\%$ |

**KILL CRITERIA:** Đến ngày **09/01/2027** (sau 90 ngày), nếu sau 1 lần FIX mà không có đối tác trường ĐH nào đạt Partner Activation Rate $\ge 35\%$ hoặc Gross Margin tổng $< 40\%$ sau chi phí token $\rightarrow$ **DỪNG DỰ ÁN HẲN**, hoàn tiền cọc còn lại và giải tán hạ tầng để bảo toàn vốn runway.

**CHƯA ĐO ĐƯỢC:**
- **Partner NRR:** Cần 12 tháng chu kỳ tuyển sinh để đo tỷ lệ tái ký hợp đồng của các trường (dự kiến có số: **15/10/2027**).
- **Chất lượng End-User (SLA & CSAT):** Cần gắn webhook bắt sự kiện escalate sang nhân viên tư vấn và nút CSAT $\pm 1$ trên widget (dự kiến có số: **24/10/2026** sau sprint 2 tuần).
