# DATA17 Monetization One Pager

Trần Đình Hinh · 2A202602399 · P-149 · Day22 · 08/10/2026

DATA-17 hỗ trợ Data Steward thẩm định hồ sơ nghi trùng từ Web, App và Referral để tạo Golden Record. Dự án đang triển khai; economics dưới đây là **kịch bản tự động hóa sau pilot, chưa phải kết quả thực đo**. [6_Benchmarks!B51:B52]

## 1 Pricing

User: Steward VSF. Buyer: Head of Data/CDO; ngân sách Data Operations, IT/Procurement/Security duyệt. Chọn **Hybrid**; Attribution **2/10**, Autonomy **0/10** theo bằng chứng hiện có. Pilot phí cố định theo phạm vi; chưa bán Outcome. [6_Benchmarks!B59; 3_Value_Metric!B10,B18,B30:B34]

Sau kiểm chứng: **$1,000/tháng** gồm **5 seats** + **$0.04/clean AI-resolved pair** từ cặp đầu tiên; không quota miễn phí. Pair tính tiền phải commit Approve/Reject, có audit, không revert sau **14 ngày**; không tính retry, trùng version hoặc ca Steward quyết định. Cap **$5,000**: cần duyệt trước xử lý vượt. [1_Cost_Job!B5; 2_Pricing!B19,B56,B71; 6_Benchmarks!B56]

| Chỉ số dự toán (USD) | Kết quả | Ô Excel |
| --- | --- | --- |
| Attempts; clean containment; completed | 100,000; 80%; 80,000 | 1_Cost_Job!B9:B11 |
| Cost/job; giá sàn 3× | $0.010302; $0.030906 | 2_Pricing!B5,B7 |
| Usage GM; Hybrid GM | 74.24%; 80.38% | 2_Pricing!B21,B61 |
| R tối thiểu: GM60%; hòa vốn usage | 51.51%; 20.60% | 2_Pricing!B33,B63 |
| Doanh thu; còn lại sau overhead/tháng | $4,200; $375.83 | 2_Pricing!B59,B62 |

Neo giá: 1 phút/ca × $10/giờ → giá trị thời gian $13,333/tháng; gói chiếm 31.5%, cần pilot chứng minh willingness-to-pay. **Usage GM <50% khi R<41.21%**; hòa vốn sau overhead cần R≥70.60%. [1_Cost_Job!B50,B53; 2_Pricing!B10,B64,B68,B76]

Benchmark: [Tamr](https://www.tamr.com/pricing) subscription + golden records, giá theo báo giá; [AWS Entity Resolution](https://aws.amazon.com/entity-resolution/pricing/) $0.25/1,000 input records. Pair, record và golden record khác đơn vị. [3_Value_Metric!A26:D27]

## 2 Go to market

Chọn **Sales-Led**, Founder bán trực tiếp. ACV $50,400; CAC cap template $74,839 (24 tháng, GM usage bảo thủ), CAC giả định $32,000 = CPO $8,000/win25% → 0.428× cap. Quota $500,000/250 ngày cần 0.0397 deal/ngày; payback Hybrid 9.48 tháng. Nội bộ giới hạn 12 tháng, CAC cap $40,510. Đây là kiểm tra số học, chưa có sales thực tế. [4_Channel_Fit!B5:B23; 2_Pricing!B65,B72:B73]

**Pain Moment:** 09:00 thứ Hai, Steward VSF rà backlog sau batch tuần trong console MDM, Review Queue/Evidence Card DATA-17 (bề mặt thiết kế; app hiện tại cần discovery). Nhúng bằng chứng và Approve/Reject vào queue, nối API với pipeline nguồn. [5_90Day_Plan!B5:B8]

| Giai đoạn | Hành động và KPI mục tiêu | Owner |
| --- | --- | --- |
| 08/10–07/11/2026 | 30 accounts, 10 interviews, 4 opportunities, 2 LOI; holdout 10k pair. | Hinh; DE/ML cần bố trí |
| 08/11–06/01/2027 | 2 pilot ×4 tuần×10k pair; review60→25s (58.33%); NSM+30%, revert≤1%; mục tiêu1 hợp đồng. | Hinh; Steward khách |
| Từ07/01/2027 | Mục tiêu3 khách cùng ngách; chỉ scale khi quality đủ14d, GM≥60%, payback≤12 tháng. | Hinh; Sales/CS |

KPI pilot chưa đo; baseline60s cần xác minh. R80% và false merge<0.1% là gate bổ sung cho auto-usage; không suy ra từ việc Steward chấp nhận gợi ý. [5_90Day_Plan!B36:B48,B53:B69]

## 3 Evidence Pack

| Tài sản và trạng thái | Nội dung cần kiểm chứng | Owner và deadline |
| --- | --- | --- |
| Eval Results: kế hoạch | Holdout có nhãn; precision/recall, clean R14d, false merge và CI; chưa kết quả. | Hinh/ML · 07/11/2026 |
| Procurement Q&A: dự thảo | Rollback; masking/paid API, DPA/retention; export và exit. Chưa duyệt security. | Hinh/Security · 14/11/2026 |
| Pilot Report: template | Khách chưa xác nhận; baseline vs AI, quality/thời gian/ROI; chờ cohort14d. | Hinh/Steward · 06/01/2027 |

Nguồn giá kiểm tra08/10/2026: [Gemini3.7 Flash Standard paid](https://ai.google.dev/gemini-api/docs/pricing) $0.75 input/$3.75 output mỗi1M token, tăng2× từ01/01/2027. Budget tổng mọi agent1,000 input+200 output/attempt; không cache/batch. API2× → usageGM69.32%. [6_Benchmarks!B46:B50; 2_Pricing!B70]

Bài test người lạ: chưa thực hiện; mục tiêu đọc2phút, hỏi lại≤3câu. [5_90Day_Plan!B29:B32]