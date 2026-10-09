# Worksheet - CareerX

**Họ tên:** Đào Trọng Khang · **MSSV:** 2A202602974 · **Ngày làm:** 09/10/2026

> Phạm vi: CareerX (P-131). P-131 chưa có bảng tài chính Day 24/25; các số dưới đây là giả định kế hoạch để kiểm thử tính sống được của mô hình, không phải actuals.

## Số liệu đầu vào

| Biến | Giá trị kế hoạch | Ghi chú |
|---|---:|---|
| ARPU | 5.000.000đ/partner/tháng | Gói B2B2C 60.000.000đ/năm |
| Gross margin trước rev-share | 65% | COGS tối đa 1.750.000đ/partner/tháng; GM sau rev-share 15% còn 50% |
| CAC | 15.000.000đ/tổ chức | Sales + pilot + onboarding trước ký |
| CAC payback mục tiêu | ≤ 6 tháng | Payback sau rev-share = 15.000.000 / (5.000.000 × 50%) = 6 tháng |
| Runway | 12 tháng | Giả định kế hoạch; phải thay bằng cash/burn thực tế |
| Value Metric | 1 **Career Job hoàn tất** | Một CV-JD analysis có report hợp lệ **hoặc** một mock interview đủ 4 lượt có report; không tính mở app, upload lỗi, phiên bỏ dở |
| AI Cost/Job | 210đ/job | 4 lượt LLM/job, mỗi lượt 1.500 input + 500 output token; giá tham chiếu P-131 tháng 05/2025, quy đổi 25.000đ/USD |

## Trạm 1 - Loại mô hình

- **Ai trả tiền?** Trường đại học hoặc trung tâm hướng nghiệp (partner) trả phí theo hợp đồng.
- **Ai dùng?** Sinh viên - người học/khách hàng của partner - dùng CareerX để phân tích CV-JD và luyện phỏng vấn; cố vấn dùng dashboard giám sát.
- **CareerX có chạm end-user không?** Có. Sinh viên đăng nhập trực tiếp trên giao diện CareerX, tải CV, chọn JD, luyện phỏng vấn và nhận report; CareerX giữ event sử dụng cùng dữ liệu CV/phỏng vấn theo quyền chia sẻ. Đây không phải white-label hay kênh chỉ giới thiệu.

**Câu chốt loại:** CareerX là **B2B2C** vì tiền đến từ trường đại học/trung tâm hướng nghiệp, người dùng thật là sinh viên của họ, và CareerX chạm trực tiếp sinh viên qua web app mang thương hiệu CareerX cùng dữ liệu CV, phân tích JD và phỏng vấn.

### Bảng đèn B2B2C và khả năng đo

| Đèn | ✅ / 🔧 / ❌ | Số nằm ở đâu / cần gì để đo |
|---|---|---|
| Partner activation rate | 🔧 | Nối `partner_go_live_at` với end-user thật đầu tiên trong 30 ngày; loại tài khoản test. Baseline 23/10/2026. |
| End-user reach trong partner | 🔧 | Cần roster sinh viên đủ điều kiện và event `career_job_completed` theo partner. Baseline 23/10/2026. |
| Time-to-first-end-user | 🔧 | Thêm `contract_signed_at` và timestamp end-user thật đầu tiên. Baseline 23/10/2026. |
| Volume volatility | 🔧 | Tổng Career Job theo partner/tháng; cần ≥2 tháng để tính độ lệch chuẩn/trung bình. Baseline 31/12/2026. |
| GM sau rev-share | 🔧 | Invoice, điều khoản rev-share, token/hosting/support phân bổ theo partner. Baseline 30/11/2026. |
| Chi phí inference ÷ doanh thu theo từng partner | 🔧 | Token log gắn `partner_id` và doanh thu partner; không lấy trung bình toàn hệ thống. Baseline 30/11/2026. |
| Tập trung volume | 🔧 | Career Job theo `partner_id`; cần ít nhất 2 partner go-live. Baseline 31/12/2026. |
| Chất lượng nhìn từ end-user | ✅ | P-131 đã có CSAT/feedback và report completion; cần thêm SLA theo hợp đồng để gắn màu. |
| Doanh thu/partner · partner NRR · GM tổng | 🔧 | Billing theo partner và cohort; NRR cần đủ 12 tháng, GM tổng có baseline 30/11/2026. |

## Trạm 2 - Cây 3 tầng và thẻ đèn

**North Star:** Partner activation rate - hiện tại **chưa có production baseline** - mục tiêu **≥60% trong 30 ngày**.

| # | Tầng | Đèn | Định nghĩa (đếm gì · không đếm gì) | Công thức | Nhịp · ai lấy số | Báo trước cho |
|---:|:---:|---|---|---|---|---|
| 1 | L | Partner activation rate | % partner đã đủ cửa sổ 30 ngày sau go-live có ≥1 sinh viên thật hoàn tất Career Job trong cửa sổ đó; không tính tài khoản test hoặc partner chưa đủ 30 ngày | Partner activate trong 30 ngày / partner đã go-live đủ 30 ngày | Tuần · Partner Ops | End-user reach → revenue/partner |
| 2 | L | End-user reach | % sinh viên đủ điều kiện của partner đã hoàn tất ≥1 Career Job; không tính login/upload lỗi | End-user có job / end-user đủ điều kiện | Tuần · Product analyst | Volume → GM sau rev-share |
| 3 | L | Time-to-first-end-user | Ngày từ ký đến sinh viên thật đầu tiên hoàn tất Career Job; không tính demo/seed | `first_end_user_at - signed_at` | Mỗi partner · Partner Ops | Partner activation |
| 4 | O | Volume volatility | Biến động Career Job hoàn tất theo tháng của từng partner trên rolling 3 tháng; không gộp partner, không tính job test/lỗi | SD(volume 3 tháng) / mean(volume 3 tháng) | Tháng/partner · Data owner | p95 inference/revenue → GM sau rev-share |
| 5 | O | p95 inference cost/revenue theo partner | Với từng partner, lấy percentile 95 của chi phí token trên mỗi Career Job hoàn tất rồi chia doanh thu thực thu/job; không dùng mean toàn hệ thống, không tính job lỗi/huỷ | `p95(token cost/job của partner) / (revenue partner / job hoàn tất)` | Tuần/partner · Tech lead + Finance | GM sau rev-share của partner → partner NRR |
| 6 | O | GM sau rev-share | Doanh thu sau khi trừ rev-share và COGS; không lấy GM trước chia | (Revenue - rev-share - COGS) / Revenue | Tháng/partner · Finance | Partner NRR, runway |
| 7 | O | End-user quality | % Career Job hoàn tất không lỗi/escalate/khiếu nại; không dùng đánh giá của partner thay end-user | Job đạt SLA / job hoàn tất | Tuần · Product/QA | Activation, partner NRR |
| 8 | G | Partner NRR | Doanh thu cohort partner sau expansion/contraction/churn; không tính partner mới | Ending recurring revenue / starting recurring revenue | Quý · Finance | Doanh thu bền vững |

**Đèn chi phí AI:** số 5 - p95 inference cost/revenue theo từng partner; p95 bắt nhóm job nặng và tách partner để số trung bình không che partner lỗ.

## Trạm 3 - Ngưỡng

| # | Đèn | 🟢 | 🟡 | 🔴 | Nguồn | Lý do · ngày kiểm tra nếu [BM] |
|---:|---|---|---|---|---|---|
| 1 | Partner activation rate | ≥60% | 30-<60% | <30% | [TB] | Chưa có chuẩn CareerX; dùng dải tạm của §3.3, đo 2 cohort partner đủ 30 ngày rồi thay baseline ngày 31/12/2026. |
| 2 | End-user reach | ≥15% | 5-<15% | <5% sau 60 ngày | [TB] | Chưa có chuẩn CareerX; đo 2 cohort partner trong 60 ngày rồi thay baseline ngày 31/12/2026. |
| 3 | Time-to-first-end-user | <30 ngày | 30-60 ngày | >60 ngày | [TB] | Chưa có chuẩn CareerX; đo 2 cohort partner rồi thay baseline ngày 31/12/2026. |
| 4 | Volume volatility | <20% | 20-40% | >40% | [TB] | Chưa có chuẩn CareerX; đo 2 chu kỳ rolling 3 tháng rồi thay baseline ngày 31/03/2027. |
| 5 | p95 inference cost/revenue theo partner | ≤5% | >5-8% | >8% | [MH] | Revenue/job 125.000đ; trần xanh 5% = 6.250đ p95/job, đỏ 8% = 10.000đ. |
| 6 | GM sau rev-share | ≥50% | 35-<50% | <35% | [MH] | 50% đạt payback 6 tháng; 35% cho payback 8,57 tháng và tổng cash cycle 10,57 tháng sau 2 tháng activation, sát runway 12 tháng. |
| 7 | End-user quality | ≥95% job đạt SLA | 90-<95% | <90% | [TB] | Chưa có chuẩn/SLA CareerX; đo 2 chu kỳ tuần rồi thay baseline và ký SLA ngày 31/10/2026. |
| 8 | Partner NRR | ≥108% | 100-<108% | <100% | [BM] | Benchmarkit 2026: usage pricing NRR 108%, seat pricing 98%; kiểm tra nguồn chính thức ngày 09/10/2026. |

### Phụ lục [MH] - phép tính

**[MH] 1 - p95 inference cost/revenue theo partner**

```text
Đầu vào: ARPU 5.000.000đ/tháng; 40 Career Job/partner/tháng; rev-share 15%; non-AI COGS budget 27%.
Doanh thu/job = 5.000.000 / 40 = 125.000đ.
Ngân sách AI xanh = 125.000 × 5% = 6.250đ/job → GM sau chia = 100% - 15% - 27% - 5% = 53%.
Trần đỏ = 125.000 × 8% = 10.000đ/job → GM sau chia = 50%; vượt 8% làm payback vượt 6 tháng.
Cost ước tính = 4 × [(1.500 × $0,15/M) + (500 × $0,60/M)] × 25.000 = 52,5đ/job;
nhân hệ số dự phòng retry/guardrail 4× ≈ 210đ/job.
Tính riêng từng partner: p95 token cost/job ÷ revenue/job.
🟢 ≤5% · 🟡 >5-8% · 🔴 >8%.
```

**[MH] 2 - GM sau rev-share**

```text
Đầu vào: doanh thu 5.000.000đ/partner/tháng; rev-share kế hoạch 15%; COGS trần 35%.
Rev-share = 750.000đ; COGS = 1.750.000đ.
GM sau rev-share = (5.000.000 - 750.000 - 1.750.000) / 5.000.000 = 50%.
Payback ở GM 50% = 15.000.000 / (5.000.000 × 50%) = 6 tháng.
Payback ở GM 35% = 15.000.000 / (5.000.000 × 35%) = 8,57 tháng.
Cash cycle tại sàn vàng = 2 tháng activation + 8,57 tháng payback = 10,57 tháng, chỉ còn 1,43 tháng runway.
🟢 ≥50% · 🟡 35-<50% · 🔴 <35%.
```

**Kiểm tra CAC payback sau rev-share:** lợi nhuận gộp/tháng = 5.000.000 × 50% = 2.500.000đ; payback = 15.000.000 / 2.500.000 = **6 tháng**. CAC tối đa để vẫn đạt mục tiêu 6 tháng = **15.000.000đ**.

## Trạm 4 - 5 luật quyết định

1. ⏹ **NẾU** partner activation rate <30% **TRONG** 60 ngày **VÀ** có ≥5 partner đã go-live đủ cửa sổ đo **THÌ** Partner Lead dừng ký partner mới ngay thứ Hai kế tiếp, chuyển 100% capacity partnership sang checklist kích hoạt và hoàn tất kế hoạch cứu từng partner trong 5 ngày làm việc **KHÔNG THÌ** không dùng “số partner đã ký” làm tăng trưởng và không mở thêm partner để bù activation thấp.
2. **NẾU** end-user reach <5% **TRONG** cửa sổ 60 ngày kể từ go-live ở một partner **VÀ** tracking phủ ≥90% roster đủ điều kiện **THÌ** Partner Lead cùng Product Owner họp với partner trong 3 ngày làm việc, chọn đúng một nút/luồng onboarding cần sửa và thử incentive mới trong 14 ngày **KHÔNG THÌ** không thêm feature theo yêu cầu partner và không đổ lỗi cho chất lượng sản phẩm khi chưa sửa động lực phân phối.
3. ⏹ **NẾU** volume volatility >40% **TRONG** 2 tháng liên tiếp ở một partner **THÌ** Finance Owner dừng báo giá phẳng cho partner đó ngay lập tức và gửi phụ lục minimum commitment, volume cap hoặc pass-through inference trong 5 ngày làm việc **KHÔNG THÌ** không ký SLA giá cố định hay lấy volume cao bất thường làm bằng chứng tăng trưởng bền vững.
4. ⏹ **NẾU** p95 inference cost/revenue >8% ở một partner **TRONG** 2 tuần liên tiếp **VÀ** mỗi tuần có ≥100 Career Job hoàn tất **THÌ** Tech Lead dừng model tier đắt cho partner đó trong ngày, giới hạn context và triển khai cache/routing trong 5 ngày làm việc **KHÔNG THÌ** không lấy mean toàn hệ thống để che partner lỗ và không tăng giá trước khi loại token waste.
5. ⏹ **NẾU** GM sau rev-share <35% **TRONG** 2 quý liên tiếp ở một partner **THÌ** Founder gửi đề nghị đàm phán lại rev-share trong 5 ngày làm việc và dừng phục vụ partner trong 30 ngày nếu không đạt GM dự phóng ≥50% **KHÔNG THÌ** không bù biên âm bằng thêm volume, giảm giá hay custom feature.

## Tự kiểm tra

- [x] 8 thẻ đèn; 3 Leading; 1 đèn AI cost; 1 Lagging.
- [x] 100% đèn có ba màu, nguồn và lý do.
- [x] 2 phép tính `[MH]` đầy đủ; các ngưỡng `[TB]` có lịch thay baseline.
- [x] 5 luật đủ NẾU / TRONG-TRÊN / THÌ / KHÔNG THÌ; 4 luật dừng; mỗi hành động có owner và deadline.
- [x] Benchmark có ngày kiểm tra 09/10/2026; số chưa có được gắn `[TB]` và lịch baseline.
