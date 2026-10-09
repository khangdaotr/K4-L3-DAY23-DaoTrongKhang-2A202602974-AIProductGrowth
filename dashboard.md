# OPERATING DASHBOARD - CareerX

**Mô hình:** B2B2C · **Cập nhật:** 09/10/2026 · **Đào Trọng Khang - 2A202602974**  
**Câu chốt:** Trường/trung tâm trả tiền, sinh viên của họ dùng, và CareerX chạm trực tiếp sinh viên qua web app mang thương hiệu CareerX cùng dữ liệu CV/phỏng vấn.  
**NORTH STAR:** Partner activation rate - hiện tại **chưa có baseline** - mục tiêu **≥60% trong 30 ngày**

> Value Metric: **Career Job hoàn tất** = CV-JD report hợp lệ hoặc mock interview đủ 4 lượt có report. Không tính login, upload lỗi hay phiên bỏ dở.

## Đèn báo sớm - xem hằng tuần

| Đèn | Hiện | 🟢 / 🟡 / 🔴 | Nguồn | Báo trước cho |
|---|---:|---|---|---|
| Partner activation | Chưa đo | ≥60% / 30-<60% / <30% | [TB] baseline 31/12/2026 | Reach → revenue/partner |
| End-user reach | Chưa đo | ≥15% / 5-<15% / <5% sau 60 ngày | [TB] baseline 31/12/2026 | Volume → GM sau chia |
| Time-to-first-end-user | Chưa đo | <30 ngày / 30-60 / >60 | [TB] baseline 31/12/2026 | Partner activation |

## Đèn vận hành - xem tuần/tháng

| Đèn | Hiện | 🟢 / 🟡 / 🔴 | Nguồn | Báo trước cho |
|---|---:|---|---|---|
| Volume volatility | Chưa đo | <20% / 20-40% / >40% | [TB] baseline 31/03/2027 | Inference cost |
| p95 inference/revenue theo partner | 0,168% kế hoạch | ≤5% / >5-8% / >8% | [MH] 210đ/125.000đ | GM sau chia → partner NRR |
| GM sau rev-share | 50% kế hoạch | ≥50% / 35-<50% / <35% | [MH] rev-share 15% | Partner NRR |
| End-user quality | Chưa đo | ≥95% / 90-<95% / <90% đạt SLA | [TB] baseline 31/10/2026 | Activation, NRR |

## Đèn kết quả - xem tháng/quý

| Đèn | Hiện | 🟢 / 🟡 / 🔴 | Nguồn |
|---|---:|---|---|
| Partner NRR | Chưa đo | ≥108% / 100-<108% / <100% | [BM] Benchmarkit 2026, 09/10/2026 |

## 5 luật quyết định (⏹ = luật dừng)

1. ⏹ **NẾU** activation <30% **TRONG** 60 ngày, ≥5 partner đủ cửa sổ **THÌ** Partner Lead dừng ký mới thứ Hai tới và lập kế hoạch cứu trong 5 ngày **KHÔNG THÌ** không báo cáo partner đã ký như tăng trưởng hay mở partner để bù.
2. **NẾU** reach <5% **TRONG** 60 ngày từ go-live, tracking ≥90% roster **THÌ** Partner Lead + Product chọn một luồng onboarding và thử incentive 14 ngày **KHÔNG THÌ** không thêm feature hay đổ lỗi sản phẩm trước khi sửa phân phối.
3. ⏹ **NẾU** volatility >40% **TRONG** 2 tháng liên tiếp **THÌ** Finance dừng giá phẳng và gửi minimum/cap/pass-through trong 5 ngày **KHÔNG THÌ** không ký SLA giá cố định.
4. ⏹ **NẾU** p95 inference/revenue >8% **TRONG** 2 tuần, mỗi tuần ≥100 job **THÌ** Tech Lead dừng model đắt trong ngày, sửa cache/routing trong 5 ngày **KHÔNG THÌ** không dùng mean che lỗ hay tăng giá trước khi sửa waste.
5. ⏹ **NẾU** GM sau chia <35% **TRONG** 2 quý liên tiếp **THÌ** Founder đàm phán trong 5 ngày và dừng partner sau 30 ngày nếu GM dự phóng <50% **KHÔNG THÌ** không bù biên âm bằng volume, giảm giá hay custom feature.

## Cổng gác 90 ngày

| Ngày | Metric duy nhất | Ngưỡng qua cổng | Bằng chứng | Nếu trượt |
|---:|---|---|---|---|
| 30 | End-user Path Readiness | 100% = đủ 5/5 mục: partner ký, owner, màn hình/nút, event, tracking plan | `end-user-path-pack.md` có sign-off partner | **GO** nếu đạt; **FIX một lần:** hoàn tất đúng mục còn thiếu trong 30 ngày |
| 60 | Activated Partner Count | ≥1 partner có ≥1 sinh viên thật hoàn tất Career Job trong 30 ngày | event log có `partner_id`, `user_id`, `career_job_completed_at` | **GO** nếu đạt; **PIVOT:** cắt value metric còn CV-JD analysis nếu vẫn bằng 0 |
| 90 | Viable Partner Count | ≥1 partner đạt đồng thời reach ≥5% và GM sau rev-share ≥50% | `partner-viability.csv` nối cohort dashboard với invoice/COGS ledger | **GO** nếu đạt; **KILL:** dừng kênh partner hiện tại nếu bằng 0 |

**KILL CRITERIA:** Đến **07/01/2027**, nếu **Viable Partner Count = 0** (không partner nào đồng thời đạt reach ≥5% và GM sau rev-share ≥50%), dừng kênh partner hiện tại, không ký partner mới và chuyển runway còn lại sang mô hình bán trực tiếp.

**CHƯA ĐO ĐƯỢC:** activation/reach/TTFE cần `partner_id`, roster và event timestamps (23/10/2026); quality cần completion/escalation/complaint log và SLA (31/10/2026); p95 inference và GM sau chia cần token ledger, billing, rev-share/COGS (30/11/2026); activation/reach baseline cần 2 cohort (31/12/2026); volatility cần 2 rolling window 3 tháng (31/03/2027); partner NRR cần billing cohort 12 tháng (09/10/2027).
