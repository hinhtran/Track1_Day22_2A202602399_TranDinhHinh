# Thuật ngữ cần biết

> **Về bài lab này** Lab thực chiến 120 phút: định nghĩa 1 job, tính Cost/Job đủ 5 thành phần, chốt Value Metric, chọn 1 kênh GTM có số chứng minh và ráp Monetization One-Pager.

> Build is no longer the bottleneck. Distribution is. Sản phẩm chạy được là bài toán kỹ thuật. Bán được là bài toán sinh tồn.

Quy ước ngôn ngữ. Tiếng Việt ưu tiên. Các term, công thức, prompt giữ tiếng Anh để chuẩn ngành và không lệch nghĩa khi làm việc với AI.

Đọc lướt 10 phút trước Lab, quay lại tra khi làm bài.

| Thuật ngữ gốc                    | Bản chất khái niệm                                                                                                                                                                             | Minh hoạ trực quan                                                                                                                                                            |
| ----------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Marginal cost`(chi phí biên)   | Với SaaS truyền thống → tiến về 0. Với AI →**tăng theo usage** : mỗi câu hỏi là tiền token, mỗi phút gọi là tiền inference, mỗi ca AI làm sai là tiền người sửa    | GitHub Copilot bán*$10/user/tháng*nhưng lỗ trung bình hơn*$20/user/tháng*(WSJ, 10/2023); user nặng nhất tốn tới $80/tháng                                         |
| `Budget line`(ngân sách khách) | Tiền trả cho bạn**rút từ túi nào**trong công ty khách: Phần mềm (nhỏ, đông đúc, qua IT & Procurement) hay Nhân sự / Vận hành (lớn hơn nhiều lần, quyết nhanh hơn). | "Nền tảng AI cho…" → khách hỏi*"Tôi có cần thêm một nền tảng nữa không?"*· "Thay ca trực đêm" → khách hỏi*"Rẻ hơn lương 1 người không?"*         |
| `Value Metric`                    | **Đơn vị bạn dùng để tính tiền** , không phải con số. Chọn sai đơn vị thì mọi mức giá đều sai — không sửa được bằng tăng hay giảm giá                        | Seat / Usage / Outcome / Hybrid. Intercom Fin tính $0,99 mỗi*resolution* , không tính theo số nhân viên                                                                |
| `Attribution`                     | Bạn có**đo được**kết quả này do AI tạo ra hay không. Đây là điều kiện để được phép bán Outcome                                                                       | Khách nói*"tôi mua vì quảng cáo, không phải vì AI"*— bạn có số liệu eval để phản biện không?                                                                 |
| `Autonomy`                        | AI**tự chạy hết**một job hay vẫn cần người can thiệp giữa chừng                                                                                                                   | Code completion tự chạy · Agent viết PR rồi cần người review là autonomy thấp                                                                                         |
| `Job`(đơn vị công việc)      | Một đơn vị công việc**có ý nghĩa với khách hàng** , không phải một thao tác kỹ thuật                                                                                       | "1 ticket được**giải quyết xong** " ✓ · "1 lần gọi API" ✗ · "1 khách hàng hài lòng" ✗ (không đếm được)                                              |
| `Cost/Job`                        | Tổng chi phí để AI làm xong**một**job, chia cho số job**HOÀN THÀNH**— không phải số job đã thử                                                                         | `(API + Infra + HITL + Retry + Overhead) / Số job hoàn thành`                                                                                                              |
| `Containment rate`                | Tỷ lệ % job AI**tự làm xong**không cần người. Là mẫu số thật của Cost/Job và là biến sinh tử của mô hình                                                                 | 1.000 ticket vào, containment 82% → 820 resolved, 180 escalate sang người                                                                                                   |
| `HITL`(Human-in-the-loop)         | Chi phí**người**kiểm tra, sửa, hoặc xử lý ca AI làm không xong. Thành phần hay bị quên nhất                                                                                   | Biến thể A: khách tự escalate → không vào COGS của bạn. Biến thể B: bạn bán outcome, bạn phải giao kết quả → vào thẳng COGS                                 |
| `Retry`                           | Chi phí gọi lại khi API timeout hoặc kết quả hỏng. Thực tế 5–10% là bình thường, để 0% là sai                                                                                     | 8% job chạy lại → cộng thêm 8% chi phí LLM                                                                                                                                |
| `Prompt caching`                  | Trả tiền rẻ hơn nhiều cho phần input**lặp lại**(system prompt, KB, few-shot) thay vì tính giá đầy đủ mỗi lượt                                                              | Cache read ≈ 0,1× giá input → cắt ~38% chi phí LLM trong worked example ở §4                                                                                            |
| `Gross Margin (GM)`               | Phần trăm còn lại sau khi trừ chi phí trực tiếp phục vụ khách. Dưới 50% là vùng nguy hiểm                                                                                          | Benchmark AI-native 2026 ≈ 52–53%; cloud truyền thống 60–80%. GM 85% ở mô hình AI thường nghĩa là**quên một khoản chi phí**                               |
| `Ngân sách CAC`                 | Số tiền bạn**được phép**chi để có một khách, tính từ chính mô hình của bạn — không phải con số nghe kể lại                                                          | `ARPU_tháng × GM × Số tháng payback cho phép`. ARPU $200, GM 60%, SMB 12 tháng → $1.440/khách                                                                        |
| `CAC payback`                     | Bao nhiêu tháng để thu hồi chi phí có được một khách                                                                                                                                   | SMB < 12 tháng · Mid-market < 18 · Enterprise < 24 (Bessemer 2024)                                                                                                           |
| `PLG / Sales-Led / Partner-Led`   | Ba cơ chế đưa sản phẩm tới khách: user tự tìm tự mua · team đi gặp và chốt · cắm vào platform đã có sẵn khách                                                              | Partner-Led mà không nêu được**tên công ty cụ thể**thì chưa phải là một kênh                                                                              |
| `Pain Moment`                     | **Thời điểm khách đau nhất + nơi họ đang đứng lúc đó**= mấy giờ + đang làm gì + dùng app nào                                                                            | *"23h, dev vừa push PR, không ai review → tắc. Họ đang ở GitHub."*→ sản phẩm phải là GitHub App, không phải website riêng                                      |
| `Evidence Pack`                   | Ba tài sản bằng văn bản để Procurement và IT dám ký: Eval Results, Procurement Q&A, Pilot Report                                                                                         | Khách vỗ tay ở demo, nhưng về công ty phòng mua hàng hỏi*"AI này hallucinate không? Data của tôi có bị train không?"*— và bạn không có mặt để trả lời |

# Mục tiêu & đầu ra

Lab này trả lời ba câu hỏi mà bất kỳ nhà đầu tư, khách hàng doanh nghiệp hay chính co-founder của bạn cũng sẽ hỏi — và bạn không thể trả lời bằng demo:

```
Câu 1: AI TRẢ TIỀN?          → và họ lấy tiền từ ngân sách nào?
            ↓
Câu 2: TRẢ THEO ĐƠN VỊ GÌ?   → theo người, theo lượt dùng, hay theo kết quả?
            ↓
Câu 3: QUA KÊNH NÀO?         → họ tìm thấy bạn ở đâu? bạn nhúng vào đâu?
            ↓
        Và: TẠI SAO HỌ TIN BẠN? → bằng chứng bằng văn bản, không phải demo
```

Chép

Sai 1 trong 3 → demo ai cũng khen, nhưng không ai rút ví.

Bạn đã đi qua các Day trước: tìm insight, build MVP, đo PMF, thiết kế Evals. Bạn có một sản phẩm chạy được. Nhưng build AI bây giờ quá dễ — ai cũng wrap được API, và giá model đang giảm rất nhanh. Khi lớp Model rẻ đi, giá trị chảy về hai lớp cuối:

```
Compute  →  Model  →  Tooling  →  [ Application ]  →  [ Services ]
GPU         rẻ đi     framework    đóng gói           sở hữu kênh
hạ tầng     rất nhanh  · API       giải pháp          · quan hệ khách
                                   ↑ giá trị nằm ở đây ↑
```

### 2.1 Bốn block và đầu ra tương ứng

Block	Câu hỏi	Output của bạn

1. Ngân sách & Value Metric	Khách lấy tiền từ đâu? Tính tiền theo đơn vị gì?	Value Metric + lý do defendable
2. Cost/Job & Giá	Chi phí thật để AI làm xong 1 việc là bao nhiêu?	Cost/Job + giá sàn + giá trần + GM%
3. GTM	Bán qua kênh nào? Nhúng vào đâu? Lúc nào?	Kênh + Pain Moment + 90-day plan
4. Evidence	Vì sao Procurement và IT tin bạn?	Evidence Pack — 3 tài sản bán hàng

### 2.2 Bằng chứng đầu ra

Bạn hoàn thành khi:

* File Excel 5 tab đã điền đủ, không ô nào trống vô lý, và cho ra được Cost/Job, giá bán đề xuất, Gross Margin, breakeven containment.
* Monetization One-Pager — một trang gói cả bốn block, mọi con số truy được về một ô trong Excel.
* One-Pager qua được bài test người lạ: một người chưa biết gì đọc trong 2 phút, hiểu được bạn bán gì, cho ai, tính tiền thế nào, có lãi trên mỗi đơn vị không, tiếp cận khách qua đâu — mà không hỏi lại quá 3 câu.

> Nguyên tắc xuyên suốt: mọi con số bạn viết ra phải trả lời được câu "Tại sao là con số này, không phải con số khác?" Số không có lý do = số ảo, và bị trừ điểm nặng ở Rubric §6.

# Chuẩn bị

### 3.1 Đầu vào bắt buộc

| Điều kiện                                                                                 | Vì sao cần                                                                            |
| -------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------- |
| Một sản phẩm AI (thật hoặc đang phát triển) mà bạn mô tả được bằng một câu | Toàn bộ Lab chạy trên sản phẩm của bạn, không phải case giả định           |
| **Kết quả Eval**— hoặc ít nhất một ước lượng containment rate               | Eval tạo ra Attribution; không có Attribution thì không được phép bán Outcome |
| Máy tính có Excel / Google Sheets và trình duyệt để mở trang giá nhà cung cấp    | Trạm 3 bắt buộc tự cập nhật giá API                                              |

### 3.2 File đi kèm

> #### TÀI LIỆU BÀI LAB: [https://drive.google.com/drive/folders/1lAN_fxwtTr1fOHm1SoPR7OH3F0Z8D5Ya?usp=drive_link](https://drive.google.com/drive/folders/1lAN_fxwtTr1fOHm1SoPR7OH3F0Z8D5Ya?usp=drive_link)

| File                              | Dùng để                                                                                              |
| --------------------------------- | ------------------------------------------------------------------------------------------------------- |
| `day22_monetization_model.xlsx` | 5 tab làm việc + 2 tab tham chiếu, đã build sẵn formula.**Chỉ điền vào ô MÀU VÀNG.** |
| `day22_one_pager_template.docx` | Khung nộp bài — Monetization One-Pager                                                               |

Quy ước màu trong file Excel:

| Màu        | Ý nghĩa                                        |
| ----------- | ------------------------------------------------ |
| 🟡 Vàng    | Ô bạn phải điền                             |
| ⬜ Xám     | Công thức tự tính —**không sửa**    |
| 🟩 Xanh lá | Đạt ngưỡng                                   |
| 🟥 Đỏ     | Không đạt — phải quay lại sửa giả định |

Bản đồ các tab:

| Tab | Tên               | Vai trò                                                       |
| --- | ------------------ | -------------------------------------------------------------- |
| 0   | `0_README`       | Quy ước, ngày chốt giá, thứ tự làm                     |
| 1   | `1_Cost_Job`     | **Tab nặng nhất**— tính Cost/Job đủ 5 thành phần |
| 2   | `2_Pricing`      | Giá sàn, giá trần, Gross Margin, stress test               |
| 3   | `3_Value_Metric` | Chấm điểm Attribution × Autonomy → gợi ý Value Metric   |
| 4   | `4_Channel_Fit`  | Ngân sách CAC, số deal/AE/ngày, chấm điểm 3 kênh       |
| 5   | `5_90Day_Plan`   | Kế hoạch 90 ngày + checklist Evidence Pack                  |
| 6   | `6_Benchmarks`   | Bảng giá tham chiếu có nguồn —**chỉ đọc**       |

Tab 1 điền theo 8 section, từ trên xuống:

| Section                             | Điền gì                                                 | Hướng dẫn                                                                                      |
| ----------------------------------- | ---------------------------------------------------------- | ------------------------------------------------------------------------------------------------- |
| **S1. Định nghĩa Job**     | 1 câu                                                     | Phải là thứ**khách coi là giá trị** . "1 ticket resolved" ✓ · "1 lần gọi API" ✗ |
| **S2. Khối lượng**         | Số job thử/tháng,**containment rate %**           | Chưa đo được → lấy từ eval, hoặc ước tính thô và ghi rõ là ước tính            |
| **S3. LLM**                   | Giá token, số lượt, token/lượt                       | **Tách riêng phần cache được và phần fresh**— đây là chỗ tiết kiệm 30–40%   |
| **S4. Speech** *(nếu có)* | Giá STT/phút, TTS/1M ký tự, thời lượng              | Bỏ trống nếu sản phẩm không có thoại                                                      |
| **S5. Infra**                 | $/job                                                      | Vector DB, embedding, storage, logging, telephony                                                 |
| **S6. Retry**                 | % job phải chạy lại                                     | Đừng để 0%. Thực tế 5–10% là bình thường                                               |
| **S7. HITL**                  | Chọn**Biến thể A hay B** , % QA, phút/ca, $/giờ | Chọn sai biến thể → Cost/Job sai vài lần                                                    |
| **S8. Overhead**              | R&D + sales phân bổ/tháng                               | Để trống được — model hiện 2 con số: có và không có overhead                         |

### 3.3 Quy ước số liệu — đọc trước khi chạm vào con số nào

Mọi con số về giá API, giá sản phẩm và benchmark trong tài liệu này đều có nguồn ở §3.5 và được chốt ngày 26/08/2026. Giá AI thay đổi rất nhanh — nhiều mức giá ở đây là giá khuyến mại có hạn (đánh dấu ⏳).

> Khi làm bài, bạn phải tự mở lại trang giá của nhà cung cấp và cập nhật, kèm ghi ngày kiểm tra. Đây là một phần của bài tập, không phải chi tiết vặt. Một mô hình không ghi ngày là một mô hình không tin được.

Quy ước tiền tệ. Giá API niêm yết bằng USD. Ví dụ quy đổi dùng tỷ giá giả định 26.000 ₫/USD — thay bằng tỷ giá thực tế khi bạn làm bài.

### 3.4 Thời lượng

120 phút. Đây là mức cường độ thật. Nếu bạn xong trong 60 phút, gần như chắc chắn bạn đã bỏ qua phần tính toán ở Trạm 3 và Trạm 4 — đó đúng là hai trạm quyết định điểm số.

### 3.5 Nguồn tham chiếu

Số liệu chốt ngày 26/08/2026. Mục có ⏳ là giá khuyến mại có hạn — kiểm tra lại trước khi dùng.

Giá API (dùng cho Cost/Job)

* 1. Anthropic — Claude API pricing · [https://platform.claude.com/docs/en/about-claude/pricing](https://platform.claude.com/docs/en/about-claude/pricing) — Opus 5 $5/$25, Sonnet 5 $2/$10, Haiku 4.5 $1/$5 per 1M token. Cache read = 0,1× input; batch −50%.
* 2. OpenAI — API pricing (docs) · [https://developers.openai.com/api/docs/pricing](https://developers.openai.com/api/docs/pricing) — ⏳ GPT-5.6 Sol $4/$20 là giá khuyến mại 21/08–21/11/2026 (list $5/$30). Cached input = 10% input; Batch API −50%. ⚠️ Dòng Sol đang trong giai đoạn điều chỉnh giá liên tục — bắt buộc tự mở trang docs và ghi lại mức bạn thấy cùng ngày kiểm tra trước khi đưa vào model.
* 3. Google — Gemini API pricing · [https://ai.google.dev/gemini-api/docs/pricing](https://ai.google.dev/gemini-api/docs/pricing) — ⏳ Gemini 3.7 Flash $0,75/$3,75 khuyến mại đến 31/12/2026, sau đó về $1,50/$7,50. Gemini tính audio input theo giá riêng, cao hơn text.
* 4. Deepgram — pricing · [https://deepgram.com/pricing](https://deepgram.com/pricing) — Nova-3 pre-recorded $0,0043/phút; ⏳ streaming list $0,0077/phút, đang khuyến mại $0,0048/phút.
* 5. AssemblyAI — pricing · [https://www.assemblyai.com/pricing](https://www.assemblyai.com/pricing) — Universal-2 async $0,0025/phút.
* 6. Google Cloud Speech-to-Text · [https://cloud.google.com/speech-to-text/pricing](https://cloud.google.com/speech-to-text/pricing) — có bậc giá theo sản lượng; tính tiền theo từng kênh audio.
* 7. ElevenLabs — API pricing · [https://elevenlabs.io/pricing/api](https://elevenlabs.io/pricing/api) — v3/v2 Multilingual $100 / 1M ký tự; Flash/Turbo $50 / 1M ký tự.
* 8. Google Cloud Text-to-Speech · [https://cloud.google.com/text-to-speech/pricing](https://cloud.google.com/text-to-speech/pricing) — Standard $4 / 1M ký tự, Studio $160 / 1M ký tự. Ký tự nhiều byte (CJK) vẫn tính là 1 ký tự.

Giá sản phẩm AI (dùng để benchmark Value Metric)

* 1. Intercom Fin — pricing · [https://fin.ai/pricing/](https://fin.ai/pricing/) — $0,99/resolution · $9,99/qualification. Định nghĩa outcome: [https://fin.ai/help/en/articles/13975800-fin-pricing-outcomes](https://fin.ai/help/en/articles/13975800-fin-pricing-outcomes)
* 2. Intercom — Building outcome-based pricing for Fin for Sales (08/05/2026) · [https://www.intercom.com/blog/building-outcome-based-pricing-for-fin-for-sales/](https://www.intercom.com/blog/building-outcome-based-pricing-for-fin-for-sales/)
* 3. GitHub Copilot — plans & AI Credits · [https://docs.github.com/en/copilot/get-started/plans](https://docs.github.com/en/copilot/get-started/plans) · [https://docs.github.com/en/copilot/concepts/billing/usage-based-billing-for-individuals](https://docs.github.com/en/copilot/concepts/billing/usage-based-billing-for-individuals)
* 4. GitHub — Copilot is moving to usage-based billing (hiệu lực 01/06/2026) · [https://github.blog/news-insights/company-news/github-copilot-is-moving-to-usage-based-billing/](https://github.blog/news-insights/company-news/github-copilot-is-moving-to-usage-based-billing/) — case study đổi Value Metric quan trọng nhất năm 2026.
* 5. Salesforce Agentforce — pricing · [https://www.salesforce.com/agentforce/pricing/](https://www.salesforce.com/agentforce/pricing/) — Flex Credits $500/100.000 ($0,10/action), $2/conversation, $5/user/tháng.
* 6. Zendesk — outcome-based pricing · [https://www.zendesk.com/blog/ai/agentic-ai/outcome-based-pricing/](https://www.zendesk.com/blog/ai/agentic-ai/outcome-based-pricing/) · [https://support.zendesk.com/hc/en-us/articles/9570369117338-About-automated-resolution-tiers](https://support.zendesk.com/hc/en-us/articles/9570369117338-About-automated-resolution-tiers) — 3 bậc resolution; $1,50/automated resolution (công bố trên blog, không trên trang giá).
* 7. Notion — pricing & credits · [https://www.notion.com/pricing](https://www.notion.com/pricing) · [https://www.notion.com/help/what-are-notion-credits](https://www.notion.com/help/what-are-notion-credits) — bỏ add-on AI theo seat (05/2025), thêm credits (2026).
* 8. Cursor — pricing · [https://cursor.com/pricing](https://cursor.com/pricing) · [https://cursor.com/blog/june-2025-pricing](https://cursor.com/blog/june-2025-pricing)
* 9. Clay — pricing · [https://www.clay.com/pricing](https://www.clay.com/pricing) — hybrid: phí nền không giới hạn seat + Actions/Data Credits.
* 10. Sierra — Outcome-based pricing for AI agents · [https://sierra.ai/blog/outcome-based-pricing-for-ai-agents](https://sierra.ai/blog/outcome-based-pricing-for-ai-agents) — Sierra mô tả mô hình nhưng không công bố giá; mọi con số $/resolution lưu hành trên mạng đều là nguồn thứ cấp chưa xác nhận.

Benchmark biên lợi nhuận & GTM

* 1. ICONIQ — 2026 State of AI Report: The Builder's Economy (07/2026) · [https://www.iconiq.com/growth/reports/state-of-ai-2026](https://www.iconiq.com/growth/reports/state-of-ai-2026) — GM AI-native ~45% (2025) → ~53% (2026E) → ~59% (2027E); 84% đẩy một phần chi phí token sang khách; 92% nói chi phí AI khó dự đoán.
* 2. Bessemer — State of the Cloud 2024 · [https://www.bvp.com/atlas/state-of-the-cloud-2024](https://www.bvp.com/atlas/state-of-the-cloud-2024) — vertical AI ~65% GM, model cost ≈10% doanh thu (~25% COGS).
* 3. Bessemer — Scaling to $100 Million (cập nhật 2024) · [https://www.bvp.com/assets/uploads/2021/09/Scaling-to-100-Million-by-Mary-DOnofrio_updated2024.pdf](https://www.bvp.com/assets/uploads/2021/09/Scaling-to-100-Million-by-Mary-DOnofrio_updated2024.pdf) — GM cloud trung bình 65–70% (middle 50%: 60–80%); CAC payback SMB <12 · Mid <18 · Enterprise <24 tháng.
* 4. David Skok — SaaS Metrics 2.0 (cập nhật 06/2026) · [https://www.forentrepreneurs.com/saas-metrics-2/](https://www.forentrepreneurs.com/saas-metrics-2/) — LTV:CAC > 3 (tốt nhất 7–8×); CAC payback tốt nhất 5–7 tháng.
* 5. a16z — The New Business of AI (2020) · [https://a16z.com/the-new-business-of-ai-and-how-its-different-from-traditional-software/](https://a16z.com/the-new-business-of-ai-and-how-its-different-from-traditional-software/) — đã 6 năm, dùng làm khung tư duy, không dùng làm số liệu hiện tại.
* 6. ICONIQ — The State of GTM in 2026 · [https://www.iconiq.com](https://www.iconiq.com/) — cost per lead $500 / $600 / $800 và cost per opportunity $6.300 / $8.000 / $11.200 theo phân khúc (tr.22).
* 7. Tomasz Tunguz — The Smallest ACV to Justify an Inside Sales Team (2016) · [https://tomtunguz.com/smallest-acv-to-justify-inside-sales-team/](https://tomtunguz.com/smallest-acv-to-justify-inside-sales-team/) — ngưỡng ~$3.000 ACV, có kèm giả định để tính lại.
* 8. Tomasz Tunguz — Is There a No Man's Land in SaaS ACVs? (2017) · [https://tomtunguz.com/no-mans-land-saas/](https://tomtunguz.com/no-mans-land-saas/) — bác bỏ giả thuyết "vùng chết ACV" bằng dữ liệu công ty đại chúng.
* 9. Christoph Janz — Five Ways to Build a $100M Business (2014) · [http://christophjanz.blogspot.com/2014/10/five-ways-to-build-100-million-business.html](http://christophjanz.blogspot.com/2014/10/five-ways-to-build-100-million-business.html) — khung phân tầng ACV và kênh tương ứng.

Về con số "Copilot lỗ $80/user"

* 1. The Register (11/10/2023) · [https://www.theregister.com/2023/10/11/github_ai_copilot_microsoft/](https://www.theregister.com/2023/10/11/github_ai_copilot_microsoft/) — tường thuật bản tin WSJ: lỗ trung bình >$20/user/tháng, user nặng tới $80/tháng.
* 2. ARK Invest, Issue #388 · [https://www.ark-invest.com/newsletters/issue-388](https://www.ark-invest.com/newsletters/issue-388) — dẫn phát biểu phủ nhận của GitHub VP Product Mario Rodriguez.

> ⚠️ Đính chính một con số bị lan truyền sai. Câu "$10 thu → $80 chi" là bóp méo: $80 là chi phí của user nặng nhất, không phải trung bình. Khi bạn dùng số liệu để pitch, hãy dùng con số gốc có nguồn. Một nhà đầu tư biết nguồn gốc con số sẽ phát hiện ra ngay.

# Thực hành

Bấm giờ từng trạm. Hết giờ thì chuyển trạm, kể cả khi chưa hoàn hảo — bạn quay lại hoàn thiện ở Trạm 6.

| Trạm       | Nội dung                                  | Thời gian     | Output                                    |
| ----------- | ------------------------------------------ | -------------- | ----------------------------------------- |
| **1** | Ngân sách khách & định nghĩa Job     | 15'            | 1 câu định vị + định nghĩa Job     |
| **2** | Value Metric                               | 20'            | Value Metric + lý do defendable          |
| **3** | **Cost/Job, giá sàn & giá trần** | **30'**  | Cost/Job + giá + GM + breakeven          |
| **4** | Kênh phân phối & Affordability Test     | 20'            | Kênh + số chứng minh                   |
| **5** | Pain Moment & 90-Day Plan                  | 15'            | Pain moment cụ thể + plan 3 giai đoạn |
| **6** | Evidence Pack & ráp One-Pager             | 20'            | One-Pager hoàn chỉnh                    |
|             | **Tổng**                            | **120'** |                                           |

### 4.1 Trạm 1 — Ngân sách khách & định nghĩa Job · 15 phút

Mục tiêu. Xác định tiền của khách đến từ ngân sách nào, và định nghĩa chính xác "1 job" là gì.

Cách kiểm tra nhanh trước khi làm: viết ra một câu mô tả sản phẩm rồi tự hỏi "Người nghe câu này sẽ lấy tiền từ đâu để trả?"

| Cách đóng gói             | Khách xếp vào       | Câu hỏi khách tự đặt ra                      |
| ----------------------------- | ---------------------- | -------------------------------------------------- |
| "Nền tảng AI cho…"         | Ngân sách phần mềm | "Tôi có cần thêm một nền tảng nữa không?" |
| "Công cụ hỗ trợ…"        | Ngân sách phần mềm | "Tool này khác gì cái tôi đang dùng?"       |
| "Thay ca trực đêm"         | Ngân sách nhân sự  | "Rẻ hơn lương 1 người không?"               |
| "Xử lý xong X việc/tháng" | Ngân sách vận hành | "Đơn giá mỗi việc là bao nhiêu?"            |

Các bước:

* 1. (4') Viết 2 phiên bản một câu mô tả sản phẩm của bạn:
* Phiên bản A — đóng gói kiểu công cụ / nền tảng
* Phiên bản B — đóng gói kiểu thay thế một công việc đang có người làm
* 1. (3') Với mỗi phiên bản, trả lời: ai ký duyệt? và tiền lấy từ ngân sách nào? (Phần mềm / Nhân sự / Vận hành / Marketing)
* 2. (3') Chọn 1 phiên bản. Viết 1 câu lý do — dựa trên ngân sách, không dựa trên "nghe hay hơn".
* 3. (5') Điền S1 Tab 1: định nghĩa "1 job". Kiểm tra bằng 3 câu:
* Khách có coi job này là giá trị không? (không phải "1 lần gọi API")
* Bạn đếm được nó tự động không?
* Bạn viết được định nghĩa chặt như Intercom ("khách xác nhận xong hoặc không hỏi lại") chưa?

Kết quả mong đợi: 1 câu định vị + tên ngân sách + định nghĩa Job viết được thành câu.

> Bằng chứng ngành. Intercom định giá Fin theo outcome ($0,99/resolution) chứ không theo seat — họ đang bán vào ngân sách vận hành CSKH, cạnh tranh với chi phí mỗi ticket do người xử lý. Khi Fin chạy trên helpdesk của bên thứ ba (Salesforce, HubSpot, Zendesk), Intercom bỏ hoàn toàn phí seat.

### 4.2 Trạm 2 — Value Metric · 20 phút

Mục tiêu. Chọn đơn vị tính tiền và bảo vệ được lựa chọn đó.

Bốn lựa chọn:

|                          | **Seat**                                      | **Usage**                    | **Outcome**                                       | **Hybrid**                                    |
| ------------------------ | --------------------------------------------------- | ---------------------------------- | ------------------------------------------------------- | --------------------------------------------------- |
| **Cách tính**    | Phí cố định / user                              | Theo lượt dùng                  | Theo kết quả đạt được                            | Phí nền + usage                                   |
| **Ưu điểm**     | Dễ hiểu, dễ bán, doanh thu đoán được       | Chi phí ↔ doanh thu cùng chiều | Align đúng giá trị, khách gần như không rủi ro | Cân bằng rủi ro 2 bên                           |
| **Nhược điểm** | Âm biên nếu user dùng nhiều                    | Khách sợ hoá đơn vượt trần | Phải đo được attribution ~100%                     | Phức tạp khi giải thích                         |
| **Dùng khi**      | AI là feature phụ, kiểm soát được mức dùng | AI là core, dùng nhiều          | AI tự chạy, kết quả đo được                     | Chưa chắc chắn — an toàn nhất để bắt đầu |

Ma trận Attribution × Autonomy:

```
AUTONOMY THẤP          AUTONOMY CAO
                 (cần người)            (AI tự chạy)
ATTRIBUTION      ┌─────────────────┐   ┌═════════════════┐
CAO              │     Usage       │   ║    OUTCOME      ║
(đo được)        │ đo được, cần    │   ║    đỉnh cao     ║
                 │ người vận hành  │   ║                 ║
                 └─────────────────┘   └═════════════════┘
ATTRIBUTION      ┌─────────────────┐   ┌─────────────────┐
THẤP             │  Seat / Hybrid  │   │     Usage       │
(chưa đo được)   │                 │   │   (an toàn)     │
                 └─────────────────┘   └─────────────────┘
```

Chép

Ba yếu tố ngoài ma trận thường quyết định lựa chọn cuối:

* 1. Thói quen chi trả của thị trường. SME Việt Nam quen trả theo mức dùng và theo gói cố định hơn là theo kết quả. Đúng về lý thuyết không có nghĩa là bán được.
* 2. Khả năng dự đoán chi phí của khách. ICONIQ 2026: 92% công ty AI nói chi phí AI khó dự đoán; một workflow mô hình hoá ở $0,10/lần chạy về sau tốn hơn $1,50 — sai số 15×.
* 3. Ai chịu chi phí inference? ICONIQ 2026: chỉ 15% công ty pricing theo consumption tự gánh 100% chi phí inference — 84% đẩy ít nhất một phần hoá đơn token sang khách.

Bốn mô hình ngoài đời (giá chốt 26/08/2026) — dùng để benchmark:

① Seat thuần → đang biến mất khỏi AI. Notion bỏ hẳn add-on AI theo seat từ 05/2025, gộp vào gói Business ($20/member/tháng), nhưng đến 2026 lại thêm lớp đo usage: credits $10 / 1.000 credits. Seat thuần không sống được khi sản phẩm chuyển từ "gợi ý khi gõ" sang "agent tự chạy".

② Usage. GitHub Copilot từ 01/06/2026 chuyển sang AI Credits: 1 credit = $0,01, đo theo token thực tế. Code completion vẫn miễn phí; chat, agent, code review thì tính credit.

③ Outcome. Intercom Fin: $0,99 / resolution, định nghĩa rất chặt — "khách xác nhận Fin đã giải quyết xong, hoặc không hỏi thêm nữa", tính một lần cho mỗi conversation. Không tính tiền khi escalate sang người, khi lỗi kỹ thuật, hay khi khách bỏ ngang. Zendesk chia 3 bậc resolution và gắn định nghĩa với khoảng thời gian khách không mở lại: 72 giờ với email, 2 giờ với messaging, ngay lập tức với thoại.

> Điểm cần rút ra: cả Intercom và Zendesk đều dành rất nhiều công sức để định nghĩa chính xác cái gì được tính tiền và cái gì không. Đó không phải chi tiết pháp lý — đó là thiết kế sản phẩm. Nếu bạn chọn Outcome mà chưa viết được định nghĩa chặt như vậy, bạn chưa sẵn sàng bán Outcome.

④ Hybrid — phổ biến nhất trên thực tế.

| Sản phẩm                                 | Phí nền                                            | Phần usage                                           |
| ------------------------------------------ | ---------------------------------------------------- | ----------------------------------------------------- |
| Intercom Fin (trên Intercom)              | $29 / seat helpdesk / tháng                         | + $0,99 / outcome                                     |
| Intercom Fin (trên helpdesk bên thứ ba) | **Không phí nền, không giới hạn seat**   | $0,99 / outcome · tối thiểu 50 outcome/tháng      |
| GitHub Copilot Business                    | $19 / seat / tháng                                  | (chính là 1.900 credits) + $0,01/credit vượt      |
| Salesforce Agentforce                      | $5 / user / tháng                                   | + Flex Credits $500/100.000 =**$0,10 / action** |
| Clay                                       | $167–$446 / tháng,**không giới hạn seat** | + Actions & Data Credits (top-up +30%)                |
| Cursor                                     | $20–$200 / tháng cá nhân; $40–$120 / user teams | + on-demand usage tính sau                           |

> Đọc kỹ "phí tối thiểu" vs "phí nền". Sàn chi tiêu thì 50 outcome đầu đã nằm trong đó; phí nền thì bạn trả rồi vẫn tính tiếp từ outcome thứ nhất. Nhầm hai thứ này là bạn tính tiền khách hai lần cho cùng một phần.

Các bước:

* 1. (6') Mở 3_Value_Metric. Trả lời 5 câu Attribution + 5 câu Autonomy. Trả lời theo thực tế hôm nay, không theo kế hoạch quý sau.
* 2. (3') Đọc gợi ý của model. Đối chiếu với ma trận ở trên.
* 3. (6') Benchmark ngược: tìm 2 sản phẩm thật cùng loại job với bạn, ghi lại họ tính tiền theo đơn vị gì, giá bao nhiêu, kèm link nguồn.
* 4. (5') Chốt Value Metric. Viết Decision Note 3 câu:
* Tôi chọn đơn vị nào?
* Attribution và Autonomy của tôi ở mức nào — bằng chứng gì?
* Nếu chọn khác gợi ý của model, lý do thị trường là gì?

Kết quả mong đợi: Value Metric đã chốt + 2 sản phẩm benchmark có link + Decision Note 3 câu.

> Ô Market override ở cuối tab: nếu chọn khác gợi ý, phải viết lý do. Lý do thị trường ("khách phân khúc này chưa quen trả theo kết quả") là hợp lệ. "Tôi thấy hợp hơn" thì không.

🔁 Nếu bí: chạy Prompt 4.7.2 (Value Metric Challenger).

### 4.3 Trạm 3 — Cost/Job, giá sàn & giá trần · 30 phút ⭐ trạm nặng nhất

Mục tiêu. Ra được một con số Cost/Job có thể bảo vệ trước CFO, rồi từ đó ra vùng giá bán.

Input cần có: định nghĩa Job (Trạm 1), Value Metric (Trạm 2), giá API hiện hành (tab 6_Benchmarks hoặc trang giá nhà cung cấp).

```
Cost/Job = ( API + Infra + HITL + Retry + Overhead ) / Số job HOÀN THÀNH
```

Chép

| Thành phần       | Gồm những gì                                           | Mức độ hay bị quên           |
| ------------------ | --------------------------------------------------------- | --------------------------------- |
| **API**      | Token LLM (input/output/cache), STT, TTS, embedding       | Ai cũng nhớ                     |
| **Infra**    | Server, vector DB, telephony, storage, logging            | Ít người tính                 |
| **HITL**     | Người kiểm tra / sửa / xử lý ca AI làm không xong | ⚠️**Hay quên nhất**     |
| **Retry**    | API timeout, gọi lại 2–3 lần, tốn gấp đôi token   | ⚠️**Hay quên nhất**     |
| **Overhead** | R&D, sales, support phân bổ                             | Thường để riêng khi tính GM |

> HITL có vào Cost/Job của BẠN không? Chỉ khi bạn là người chịu chi phí đó. - Biến thể A — bạn bán phần mềm, khách tự xử lý ca escalate. HITL của khách không vào COGS của bạn. Nhưng QA nội bộ của bạn thì có. - Biến thể B — bạn bán outcome / managed service. Ca AI làm không xong mà bạn vẫn phải giao kết quả → HITL vào thẳng COGS của bạn. Hai biến thể cho ra Cost/Job chênh nhau nhiều lần. Bạn phải nói rõ mình ở biến thể nào.

### Worked Example — "Trợ lý AI xử lý ticket CSKH cho SME"

Đây là ví dụ mẫu bạn sẽ lặp lại với sản phẩm của mình. Mọi con số đều truy được nguồn và bạn có thể tính lại.

Định nghĩa job: 1 ticket được giải quyết xong (khách xác nhận hoặc không hỏi lại). Giả định vận hành: 1.000 ticket vào / tháng · containment rate 82% → 820 resolved, 180 escalate.

Bước 1 — Chi phí LLM cho 1 ticket. Claude Haiku 4.5: input $1,00 / 1M, output $5,00 / 1M, cache write $1,25 / 1M, cache read $0,10 / 1M. Hội thoại 6 lượt, mỗi lượt 4.000 token input (3.000 là system prompt + KB — cache được), 300 token output.

| Khoản                      | Phép tính                    | Chi phí          |
| --------------------------- | ------------------------------ | ----------------- |
| Cache write (1 lần)        | 3.000 × $1,25/1M              | $0,00375          |
| Cache read (5 lượt sau)   | 15.000 × $0,10/1M             | $0,00150          |
| Input không cache          | 6.000 × $1,00/1M              | $0,00600          |
| Output                      | 1.800 × $5,00/1M              | $0,00900          |
| **Tổng có cache**   |                                | **$0,0203** |
| *Tổng nếu KHÔNG cache* | *24.000 × $1 + 1.800 × $5* | *$0,0330*       |

→ Prompt caching cắt 38% chi phí LLM. Batch API (nếu không cần realtime) giảm thêm 50%.

Bước 2 — Cộng đủ 5 thành phần, cho 1.000 ticket / tháng.

| Thành phần                                         | Cách tính        | Tổng / tháng   |
| ---------------------------------------------------- | ------------------ | ---------------- |
| API — LLM                                           | 1.000 × $0,0203   | $20,25           |
| API — Retry (8% chạy lại)                         | $20,25 × 8%       | $1,62            |
| Infra (vector DB, embedding, logging)                | 1.000 × $0,005    | $5,00            |
| HITL — QA nội bộ (review 5%, 2 phút/ca, $9/giờ) | 50 × 0,033h × $9 | $15,00           |
| **Cộng — phần luôn có**                   |                    | **$41,87** |
| HITL — escalation*(chỉ ở Biến thể B)*           | 180 × 0,1h × $9  | $162,00          |

Bước 3 — Chia cho số job HOÀN THÀNH (820), không phải 1.000.

|                                                | Cost/Job                        | So với Intercom Fin $0,99 |
| ---------------------------------------------- | ------------------------------- | -------------------------- |
| **Biến thể A**— khách tự escalate   | $41,87 / 820 =**$0,051**  | GM = 94,8%                 |
| **Biến thể B**— bạn chịu escalation | $203,87 / 820 =**$0,249** | GM = 74,8%                 |

Bước 4 — Stress test biến sinh tử. Containment tụt từ 82% xuống 60% (Biến thể B) → 600 resolved, 400 escalate: Cost = $41,87 + (400 × $0,90) = $401,87 → Cost/Job = $0,670 → bán $0,99 thì GM chỉ còn 32,3%.

Ngưỡng sống còn: để GM ≥ 60% ở giá $0,99, cần Cost/Job ≤ $0,396.

```
(41,87 + 0,90 × (1.000 − R)) / R  ≤  0,396
941,87  ≤  1,296 × R
R  ≥  727
```

Chép

> Containment rate phải ≥ ~73% thì mô hình $0,99/resolution mới có lãi lành mạnh. Con số 73% này không phải benchmark ngành — nó là kết quả tính từ giả định của ví dụ này. Bạn sẽ tính ra con số của riêng bạn, và đó chính là con số đáng đưa vào One-Pager: nó nối thẳng Evals → Pricing.

### Biến thể thoại — nếu sản phẩm của bạn là voice

Một cuộc gọi 3 phút, giá công khai 26/08/2026:

| Khoản                                              | Giả định                          | Chi phí                        |
| --------------------------------------------------- | ------------------------------------ | ------------------------------- |
| STT streaming (Deepgram Nova-3, list $0,0077/phút) | 3 phút                              | $0,0231                         |
| TTS (ElevenLabs Flash/Turbo, $50 / 1M ký tự)      | bot nói ~1,5 phút ≈ 1.350 ký tự | $0,0675                         |
| LLM (Haiku 4.5, ~8 lượt, có cache)               |                                      | $0,0250                         |
| Telephony                                           | ~$0,01/phút × 3                    | $0,0300                         |
| **Cost/Job (chưa HITL, chưa retry)**        |                                      | **≈ $0,146 ≈ 3.800 ₫** |

Ba cái bẫy trong bảng này:

* 1. TTS thường đắt hơn LLM — chênh giữa nhà cung cấp rất lớn: Google Standard $4 / 1M ký tự vs ElevenLabs v3 $100 / 1M là 25×, tới 40× nếu so Google Studio $160.
* 2. Streaming thường đắt hơn async — nhưng bội số từ 1× đến ~5×. AssemblyAI tính streaming bằng đúng giá async; Deepgram Nova-3 khuyến mại chỉ đắt hơn ~1,1×; Google STT standard đắt hơn dynamic batch tới ~5×. Đừng dùng một hệ số chung.
* 3. Đọc kỹ đơn vị tính tiền. Google STT tính theo từng kênh audio — 4 kênh × 120 giây = 480 giây bị tính tiền.

> ⏳ Bảng trên dùng giá LIST cho an toàn. Nếu dùng giá khuyến mại Deepgram $0,0048/phút, Cost/Job tụt xuống ≈ $0,137 ≈ 3.560 ₫. Cùng một mô hình, hai mức giá, lệch 6% — đây là lý do §3.3 bắt bạn ghi ngày kiểm tra giá.

### Hai ngưỡng phải nhớ và cách neo giá trần

| Ngưỡng                             | Ý nghĩa                                                    |
| ------------------------------------ | ------------------------------------------------------------ |
| **Giá bán ≥ 3 × Cost/Job** | Đủ để cover R&D, sales, support, và sai số ước tính |
| **Gross Margin ≥ 60%**        | Dưới**50% là vùng nguy hiểm**                     |

Đối chiếu thực tế: benchmark AI-native 2026 là ~52–53%; Bessemer đo vertical AI ~65% GM. Nếu mô hình của bạn ra GM 85% ngay từ đầu, khả năng cao là bạn đang quên một khoản chi phí, không phải bạn giỏi hơn thị trường.

| Cách neo giá trần                 | Công thức                                | Ví dụ                                                |
| ------------------------------------ | ------------------------------------------ | ------------------------------------------------------ |
| **Neo theo giá trị tạo ra** | Charge**10–25%**giá trị                 | Tiết kiệm cho khách 100tr/tháng → charge 10–25tr |
| **Neo theo nhân công**       | Charge**50–70%**lương vị trí bị thay | Lương 1 người 10tr/tháng → charge 5–7tr         |

```
[ Giá sàn: Cost/Job × 3 ]────[ VÙNG GIÁ BÁN ]────[ Giá trần: neo theo giá trị / lương ]
```

Chép

> Đừng để khách so bạn với ChatGPT $20. Hãy để họ so với lương nhân viên hoặc chi phí outsource — đó là ngân sách bạn đang cạnh tranh.

Pricing là giả thuyết, không phải chân lý. GitHub Copilot (06/2026), Intercom (05/2026), Salesforce (05/2025) và Notion (05/2025, rồi 2026) đều đã đổi mô hình giá trong 18 tháng qua. Đổi giá không phải thất bại — không dám đổi giá mới là thất bại.

Các bước:

* 1. (3') Cập nhật giá API. Mở tab 6_Benchmarks, kiểm tra các dòng có dấu ⏳, mở trang giá gốc và cập nhật nếu đã đổi. Ghi ngày bạn kiểm tra.
* 2. (7') Tính chi phí LLM cho 1 job. Điền S3: số lượt, token input cache được vs fresh, token output/lượt. Ghi lại % tiết kiệm nhờ cache. Nếu job không cần realtime, thử giá Batch (−50%) và ghi con số thứ hai.
* 3. (4') Speech (nếu có) + Infra. Điền S4, S5.
* 4. (4') Retry & HITL. Điền S6, S7: Retry đừng để 0% (chưa đo thì dùng 5–10% và ghi rõ là ước tính); HITL chọn Biến thể A hay B và viết 1 câu lý do.
* 5. (4') Đọc Cost/Job và tính giá sàn. Sang Tab 2: giá sàn = 3 × Cost/Job. Nếu Cost/Job nhỏ đến mức vô lý → bạn đang quên một khoản, quay lại S5–S7.
* 6. (5') Neo giá trần. Điền 2 con số neo (giá trị khách tiết kiệm/tháng · lương vị trí bị thay/tháng). Nếu hai con số chênh nhau nhiều, chọn cái khách dễ tự kiểm chứng nhất.
* 7. (3') Stress test. Đọc bảng sensitivity và ô Breakeven containment: containment phải đạt bao nhiêu % để GM ≥ 60%? Đối chiếu với eval của bạn. Nếu đang ở dưới ngưỡng → viết 1 câu: bạn sẽ tăng containment, tăng giá, hay đổi biến thể HITL?

Kết quả mong đợi — bắt buộc có 5 con số:

```
[ ] Cost/Job                     = ______
[ ] Giá sàn (= 3 × Cost/Job)     = ______
[ ] Giá bán đề xuất              = ______
[ ] Gross Margin                 = ______%   (mục tiêu ≥ 60%)
[ ] Breakeven containment        = ______%   (so với eval hiện tại: ______%)
```

Chép

> Nếu đèn đỏ ở Tab 2: đừng sửa giá cho đẹp. Quay lại Tab 1 và đổi mô hình kinh doanh — giảm token/job, tăng cache, tăng containment, đổi biến thể HITL, hoặc đổi Value Metric.

🔁 Nếu bí: chạy Prompt 4.7.1 (Cost/Job Stress Test) — prompt chính của Lab này.

### 4.4 Trạm 4 — Kênh phân phối & Affordability Test · 20 phút

Mục tiêu. Chọn đúng 1 kênh cho 90 ngày đầu, và chứng minh bằng số rằng bạn nuôi nổi kênh đó.

Nguyên tắc sống còn: chạy song song 3 kênh với đội 5 người = không kênh nào đủ sâu để biết nó có work hay không.

|                | **PLG**                           | **Sales-Led**                     | **Partner-Led**                              |
| -------------- | --------------------------------------- | --------------------------------------- | -------------------------------------------------- |
| Cơ chế       | User tự tìm, tự dùng, tự mua       | Team đi gặp, demo, chốt từng khách | Cắm vào platform đã có sẵn khách            |
| CAC            | Thấp                                   | Cao                                     | Thấp (đòn bẩy partner)                         |
| Ưu điểm     | Scale nhanh, ít người                | Deal lớn, hiểu sâu khách            | Đòn bẩy, không cần sales team lớn            |
| Nhược điểm | Khó với B2B, churn cao                | Chậm, tốn nhân lực                  | Phụ thuộc partner, chia margin                   |
| Dùng khi      | Sản phẩm tự giải thích, giá thấp | Sản phẩm phức tạp, giá cao         | Có platform phù hợp và**quan hệ thật** |

### "Vùng chết ARPU–CAC" — cái gì có nguồn, cái gì là folklore

> ⚠️ Không có nguồn công bố nào chuẩn hoá một "vùng chết $50–$1.000". Đây là heuristic thực chiến, không phải benchmark có dữ liệu. Người duy nhất kiểm chứng công khai là Tomasz Tunguz (2017) — ông đối chiếu ACV-tại-IPO của toàn bộ công ty phần mềm đại chúng và bác bỏ nó: không có khoảng trống, mà là một dải liên tục từ $87 đến $780.000. Cụm "dead zone" trên SaaStr thực ra nói về tốc độ tăng trưởng, không phải ACV.

Dùng ba công cụ có nguồn thay cho con số nghe kể lại:

Công cụ 1 — Ngưỡng sàn của Inside Sales (Tunguz, 2016). ACV tối thiểu ~$3.000 để nuôi được inside sales team.

```
Số deal 1 AE phải chốt / năm  =  Quota năm / ACV
```

Chép

Giả định gốc: quota $500k/năm · fully-loaded 1 AE $100k · tỷ lệ đạt quota 75%.

* ACV $3.000 → 167 deal/năm ≈ 0,7 deal / ngày làm việc → khả thi
* ACV $500 → 1.000 deal/năm ≈ 4 deal / ngày → bất khả thi

Công cụ 2 — Channel Affordability Test. Kênh chỉ khả thi khi ngân sách CAC bạn có ≥ CAC thực tế của kênh đó.

```
Ngân sách CAC  =  ARPU_tháng × Gross Margin × Số tháng payback cho phép
```

Chép

| Phân khúc | CAC Payback cho phép |
| ----------- | --------------------- |
| SMB         | < 12 tháng           |
| Mid-market  | < 18 tháng           |
| Enterprise  | < 24 tháng           |

Và LTV : CAC ≥ 3× (David Skok, cập nhật 06/2026; payback tốt nhất 5–7 tháng). Ví dụ: ARPU $200/tháng, GM 60%, SMB → Ngân sách CAC = 200 × 0,6 × 12 = $1.440 / khách.

Công cụ 3 — Đối chiếu chi phí thật của motion có sales (ICONIQ 2026).

| Chỉ số (theo phân khúc khách hàng) | Mức                                  |
| ---------------------------------------- | ------------------------------------- |
| Cost per lead                            | $500 · $600 · $800                  |
| **Cost per opportunity**           | **$6.300 · $8.000 · $11.200** |

Ghép Công cụ 2 và 3: win rate 25% → CAC thực tế ≈ $25.200 – $44.800 / khách. So với ngân sách CAC $1.440 → lệch 18–31 lần. Kết luận: ở ARPU $200/tháng, motion có sales rep là không khả thi — và lần này bạn chứng minh được bằng số.

3 lối thoát khi rơi vào vùng đó:

| Lối thoát                 | Làm gì                                                              | Đánh đổi                       |
| --------------------------- | --------------------------------------------------------------------- | ---------------------------------- |
| **Giảm giá xuống** | Bỏ bớt feature, đơn giản hoá đến mức khách tự mua được  | Mất phân khúc lớn              |
| **Tăng giá lên**   | Bán kèm service / triển khai / support để có margin nuôi sales | Chậm hơn, cần đội triển khai |
| **Đi Partner-Led**   | Cắm vào platform đã có khách →**bypass bài toán CAC**  | Chia margin, phụ thuộc partner   |

> ⚠️ Partner-Led nghe đẹp nhưng dễ thành mơ. Nếu chọn Partner-Led, One-Pager phải ghi tên công ty partner cụ thể, bạn mang lại giá trị gì cho họ, và bạn đã nói chuyện với họ chưa. "Sẽ tích hợp với các nền tảng trong ngành" = chưa có kênh.

Các bước:

* 1. (5') Mở 4_Channel_Fit. Nhập ARPU/tháng và phân khúc → ra ngân sách CAC (GM tự lấy từ Tab 2).
* 2. (4') Nhập quota AE/năm và ACV → ra số deal/AE/ngày làm việc. ≤ ~1 deal/ngày → Sales-Led khả thi; > 1 deal/ngày → 🟥 không chạy được, dù tuyển sales giỏi đến đâu.
* 3. (4') Tính CAC thực tế ≈ Cost per opportunity / Win rate, so với ngân sách CAC ở bước 1. Lệch bao nhiêu lần? Ghi con số đó lại — đây là bằng chứng chọn kênh của bạn.
* 4. (4') Chấm điểm 3 kênh trong scorecard (6 tiêu chí × 1–5). Chốt 1 kênh duy nhất.
* 5. (3') Nếu rơi vào vùng không khả thi, chọn 1 trong 3 lối thoát ở trên và viết 1 câu bạn sẽ làm gì cụ thể trong 30 ngày tới.

Kết quả mong đợi:

```
[ ] Ngân sách CAC / khách        = ______
[ ] Số deal / AE / ngày          = ______
[ ] CAC thực tế ước tính         = ______   → lệch ______ lần
[ ] Kênh đã chốt                 = PLG / Sales-Led / Partner-Led
[ ] Nếu Partner-Led: TÊN partner = ______   · Đã nói chuyện chưa? ______
```

Chép

🔁 Nếu bí: chạy Prompt 4.7.3 (Channel Reality Check).

### 4.5 Trạm 5 — Pain Moment & 90-Day Plan · 15 phút

Mục tiêu. Biến kênh trừu tượng thành một điểm chạm cụ thể, và một kế hoạch 90 ngày ai đọc cũng làm theo được.

Build thì dễ, grow mới khó. Sản phẩm AI thắng lớn không tạo thói quen mới — chúng nhúng vào thói quen cũ.

```
✗ Đừng bắt khách mở tab mới     ✗ Đừng bắt tải app mới     ✗ Đừng bắt học workflow mới
```

Chép

| Sản phẩm               | Nhúng vào đâu                               | Vì sao zero friction                                                                  |
| ------------------------ | ----------------------------------------------- | -------------------------------------------------------------------------------------- |
| **GitHub Copilot** | Ngay trong VSCode/IDE                           | Gợi ý hiện ra khi đang gõ — không mở website nào                              |
| **Notion AI**      | Ngay trong trang đang viết                    | Bấm Space là có — không rời Notion                                               |
| **Intercom Fin**   | Ngay trong helpdesk**khách đang dùng** | Không bắt khách đổi helpdesk — và bỏ luôn phí seat để không có rào cản |
| **Clay**           | Trong bảng dữ liệu, không giới hạn seat   | Cả team dùng chung, không ai phải xin license                                      |

Chú ý case Intercom Fin: để nhúng được vào workflow của khách, họ chấp nhận từ bỏ doanh thu seat.

```
Pain Moment  =  MẤY GIỜ  +  ĐANG LÀM GÌ  +  DÙNG APP NÀO
```

Chép

Ví dụ đọc được: "23h, dev push PR, không ai review → tắc nghẽn. Lúc đó họ đang ở GitHub → sản phẩm phải là một GitHub App, không phải một website riêng." Ví dụ không đọc được: "Khách cần AI khi làm việc."

> Pain Moment quyết định kênh phân phối — không phải ngược lại.

Các bước:

* 1. (5') Viết Pain Moment theo đúng công thức 3 phần ở trên.
* 2. (3') Từ Pain Moment suy ra điểm nhúng: sản phẩm của bạn phải nằm ở đâu để có mặt đúng lúc đó? (một app trong marketplace? một số hotline? một plugin? một webhook?) — không phải một website riêng, trừ khi bạn chứng minh được khách sẽ mở nó đúng lúc đau.
* 3. (7') Điền 90-Day Plan trong 5_90Day_Plan, 3 giai đoạn:

| Giai đoạn           | Mục tiêu điển hình                                | Phải ghi rõ                                            |
| --------------------- | ------------------------------------------------------ | -------------------------------------------------------- |
| **Tháng 1**    | Hiểu sâu — số ít khách, làm tận tay            | Bao nhiêu khách? Ai đi gặp? Thu được data gì?    |
| **Tháng 2–3** | Đòn bẩy — nhân rộng qua kênh đã chọn         | Cơ chế nhân rộng là gì? KPI nào?                  |
| **Tháng 4+**   | Mở rộng — chỉ sau khi thắng tuyệt đối 1 ngách | Ngách tiếp theo là gì? Dựa trên bằng chứng nào? |

> Đừng cố scale ngay. Chọn 1 ngách hẹp, thắng tuyệt đối, rồi mới mở rộng. Kế hoạch "tháng 1 lấy 200 khách ở 5 ngành" là kế hoạch chưa ai từng thử làm.

Kết quả mong đợi: Pain Moment đủ 3 phần + điểm nhúng cụ thể + 90-Day Plan có số và có tên người chịu trách nhiệm.

### 4.6 Trạm 6 — Evidence Pack & ráp One-Pager · 20 phút

Mục tiêu. Gói 5 trạm trước thành 1 trang đưa được cho người lạ.

```
Bán bằng DEMO:     Demo → Vỗ tay → ✗ Procurement chặn
Bán bằng EVIDENCE: Evidence → Trust → ✓ Ký hợp đồng
```

Chép

Khách vỗ tay ở buổi demo. Nhưng về công ty, phòng mua hàng và IT sẽ hỏi 3 câu, và bạn không có mặt để trả lời:

* 1. AI này có hallucinate không? Nếu nó nói sai với khách của tôi thì sao?
* 2. Data của tôi có bị dùng để train model không?
* 3. Startup này chết thì data của tôi ở đâu?

Không trả lời được bằng văn bản → deal chết ở Procurement. Không phải vì sản phẩm dở — vì không ai dám ký.

Evidence Pack còn có tác dụng thứ hai ít người để ý: nó là điều kiện để bạn được phép bán Outcome.

```
Evals → Attribution → được phép chọn Outcome → giá cao hơn
        ↓
Procurement Q&A → Procurement duyệt → deal ký được
```

Các bước:

(7') Điền checklist Evidence Pack trong 5_90Day_Plan:
Tài sản	Nếu đã có thì ghi	Nếu chưa có thì ghi
Eval Results	Con số cụ thể: "xử lý đúng __%; __% còn lại chuyển người"	Ai làm · deadline · đo bằng bộ eval nào
Procurement Q&A	3 câu trả lời cho 3 câu hỏi của Procurement ở trên	Ai làm · deadline
Pilot Report	"__ tuần, __ lượt, tỷ lệ thành công __%, tiết kiệm __"	Pilot với ai · bắt đầu khi nào
Ô nào chưa có thì ghi deadline, đừng bỏ trống. Một Evidence Pack thành thật về khoảng trống tốt hơn một Evidence Pack bịa.

(10') Ráp One-Pager — mở day28_one_pager_template.docx, chép các con số đã chốt từ Excel sang. 3 khối:
Khối	Nội dung

1. Pricing	Ngân sách khách · Value Metric + lý do · Cost/Job · Giá đề xuất · GM · Breakeven containment · cách neo giá
2. GTM	Kênh + số chứng minh (ngân sách CAC vs CAC thực tế) · Pain Moment 3 phần · 90-day plan
3. Evidence	3 tài sản — đã có gì, thiếu gì, deadline
   (3') Tự kiểm tra bằng "bài test người lạ". Đưa One-Pager cho một nhóm khác đọc trong 2 phút. Họ phải trả lời được không cần hỏi lại quá 3 câu:
   Bạn bán cái gì, cho ai, tính tiền theo đơn vị nào?

Bạn có lãi trên mỗi đơn vị không, và con số nào chứng minh?

Bạn tiếp cận khách qua đâu, và vì sao là kênh đó?

Kết quả mong đợi: One-Pager đầy đủ 3 khối, mọi con số đều truy được về một ô trong Excel.

### 4.7 Bộ prompt phản biện (English-only)

> Tất cả prompt giữ tiếng Anh 100% để AI giữ ngữ nghĩa ổn định. Thảo luận kết quả bằng tiếng Việt. AI là công cụ phản biện, không phải tác giả. Với mỗi điểm AI nêu: accept (sửa theo), reject (giữ, có lý do), hoặc partial. Mọi sửa đổi phải do bạn viết lại — không copy nguyên văn từ AI.

### 4.7.1 Cost/Job Stress Test Prompt — prompt chính

Dùng sau khi điền xong Tab 1.

Act as a ruthless CFO and a skeptical infrastructure engineer.
Review the Cost/Job model for an AI product below.

Do NOT rewrite my numbers. Perform a stress test:

1. MISSING COST CATEGORIES
   List every cost category I have omitted or underestimated for
   THIS specific product type. Focus especially on:

   - Retry and timeout costs
   - Human-in-the-loop cost, and WHO actually bears it
   - Observability, logging, eval running costs
   - Egress, storage, vector DB growth over time
     For each, estimate a realistic value at my stated volume.
2. DENOMINATOR CHECK
   Am I dividing by jobs ATTEMPTED or jobs COMPLETED?
   Recalculate Cost/Job using completed jobs only.
   State the difference as a percentage.
3. TOKEN MATH AUDIT
   Recompute my LLM cost per job from the token counts and
   list prices I provided. Flag any arithmetic error.
   Tell me how much prompt caching and batch pricing would save,
   and whether batch is viable for my latency requirement.
4. PRICE VOLATILITY
   Which of the prices I used are promotional or likely to change
   within 6 months? Recompute Cost/Job at list price.
5. BREAKEVEN SENSITIVITY
   Solve for the minimum success/containment rate required to hit
   60% gross margin at my proposed price. Show the algebra.
   Then show Cost/Job and gross margin at success rates of
   50%, 60%, 70%, 80%, 90%.
6. THE ONE NUMBER THAT KILLS ME
   Identify the single input that, if wrong by 2x, breaks the model.

Be direct and highly critical. Use my numbers, show your arithmetic.
Do not be encouraging. Find the problems before my investors do.

[Paste your Tab 1 assumptions here]


4.7.2 Value Metric Challenger Prompt
Dùng khi phân vân giữa Seat / Usage / Outcome / Hybrid.

My AI product:

- What it does: [describe the job it completes]
- Definition of one job: [your exact definition]
- Target buyer: [B2B SME / Enterprise / prosumer, industry, geography]
- Whose budget it comes from: [software / headcount / operations]
- Autonomy today: [does it complete jobs end-to-end, or need a human?]
- Attribution today: [can I measure that the outcome was caused by
  my AI? what evidence do I have — eval results, logs, A/B?]
- Cost/Job: [number] · Proposed price: [number]

Task:

1. Recommend a value metric (Seat / Usage / Outcome / Hybrid)
   and explain the reasoning against the Attribution x Autonomy
   framework.
2. ATTACK my own preferred choice, which is [your choice].
   Give me the three hardest questions a skeptical buyer or
   investor would ask about it. Include the refund/dispute
   scenario if I chose Outcome.
3. Name 3 REAL products that charge for a similar job. For each,
   state their value metric and current published price, and give
   the URL. If you are not confident a price is current, say so
   explicitly rather than guessing.
4. Describe the exact customer behaviour that would make my
   chosen metric lose money, and how a hybrid structure would
   cap that downside.

Be specific about numbers. Flag any figure you are unsure about.
Chép
4.7.3 Channel Reality Check Prompt
I am choosing ONE go-to-market channel for the next 90 days.

My numbers:

- ARPU: [number] per month · Gross margin: [%]
- Segment: [SMB / mid-market / enterprise]
- CAC budget I computed: [number] per customer
- Chosen channel: [PLG / Sales-Led / Partner-Led]
- Pain moment: [time + what the customer is doing + which app]

Task:

1. Compute how many deals one AE must close per year and per
   working day at my ACV, assuming a realistic quota and
   fully-loaded cost for my market. State your assumptions.
   Tell me plainly whether a sales-led motion is arithmetically
   possible at my price point.
2. Compare my CAC budget against realistic cost-per-opportunity
   benchmarks for a rep-driven motion. State the gap as a multiple.
3. Stress test my pain moment. Is it specific enough to name an
   integration surface? If it is vague, tell me exactly which
   detail is missing.
4. If my chosen channel is Partner-Led: what must be true about
   the partner's incentives for this to work? What is the single
   fastest way to falsify this plan within 2 weeks?
5. Give me the strongest argument AGAINST my chosen channel.

Be blunt. I would rather be wrong now than in month 3.
Chép
4.7.4 Procurement Objection Simulator
Dùng để chuẩn bị Evidence Pack.

Act as the IT security lead and the procurement manager of a
mid-sized company evaluating my AI vendor. You are risk-averse,
you have been burned before, and you have no incentive to say yes.

My product: [describe]
My Evidence Pack currently contains: [list what you actually have]

Task:

1. Write the 10 questions you would send me before approving
   this purchase. Order them by how likely they are to kill
   the deal.
2. For each question, tell me what a SATISFACTORY written answer
   looks like — the format and the level of specificity, not the
   content.
3. Identify which of my 10 answers is currently weakest and
   would stall the deal.
4. Tell me the minimum viable Evidence Pack that would get this
   through your review, given I am an early-stage company and
   cannot yet have SOC 2.

Do not soften anything. Write as you would internally.
Chép
4.7.5 One-Pager Defensibility Check
Dùng trước khi nộp.

I wrote this Monetization One-Pager for an AI product:

"[paste your one-pager]"

Evaluate it the way a Series A investor reading 50 decks today
would:

1. For each number in the document, can you tell WHY it is that
   number and not another? List any number that appears
   unjustified.
2. Which claim would you push back on first, and what exactly
   would you ask?
3. Could a competing founder write this same one-pager with
   their own numbers swapped in? If yes, it is too generic —
   point to the specific sentences that make it generic.
4. Is the pricing internally consistent — does the Cost/Job,
   the price, the gross margin and the breakeven success rate
   actually agree with each other arithmetically? Check the math.
5. Does the chosen channel follow logically from the ARPU and
   the pain moment, or were they decided independently?
6. Suggest ONE specific edit that would make this 10x more
   defensible.

Be brutally honest. I would rather hear it from you now.
Chép
4.7.6 Price-Change Watch Prompt (dùng sau buổi học)
Below are the AI infrastructure prices I used to build my
Cost/Job model, with the date I recorded them.

[paste your 6_Benchmarks rows: item, vendor, price, unit, date]

Task:

1. For each line, tell me whether this price is likely to have
   changed, and what would drive the change.
2. Flag which lines were promotional pricing with an expiry.
3. Recompute my Cost/Job under a scenario where every
   promotional price reverts to list.
4. Tell me which single vendor switch would most reduce my
   Cost/Job, and what I would trade away to get it.

If you cannot verify a current price, say so — do not guess.


# Kiểm tra kết quả

Sáu mốc, tương ứng 6 trạm. Ở mỗi mốc, tự kiểm bằng Pass / Fail signals dưới đây.

### 5.1 Checkpoint 1 — Ngân sách & Job (phút 15)

Output: 1 câu định vị · tên ngân sách · định nghĩa Job.

| ✅ Pass                                                                                   | ❌ Fail                                                                    |
| ----------------------------------------------------------------------------------------- | -------------------------------------------------------------------------- |
| Câu định vị nói được**công việc bị thay thế** , không chỉ công nghệ | "Nền tảng AI cho…" — rơi vào ngân sách phần mềm mà không biết |
| Nêu được**ai ký duyệt**ngân sách đó                                       | Không biết ai ra quyết định mua                                       |
| Job định nghĩa theo**giá trị**và**đếm được**                       | Job = "1 request" hoặc "1 khách hài lòng"                              |

### 5.2 Checkpoint 2 — Value Metric (phút 35)

Output: Value Metric + Decision Note 3 câu + 2 benchmark có link.

| ✅ Pass                                                                                          | ❌ Fail                                 |
| ------------------------------------------------------------------------------------------------ | --------------------------------------- |
| Lựa chọn khớp ma trận Attribution × Autonomy, hoặc**lệch có lý do thị trường** | Chọn theo cảm tính                   |
| Nếu chọn Outcome: có**số liệu eval**làm bằng chứng Attribution                     | Chọn Outcome mà chưa đo được gì |
| Có 2 sản phẩm thật + link nguồn                                                             | Benchmark nhớ mang máng, không link  |

### 5.3 Checkpoint 3 — Cost/Job & Giá (phút 65) ⭐

Output: 5 con số ở Trạm 3.

| ✅ Pass                                                                             | ❌ Fail                                       |
| ----------------------------------------------------------------------------------- | --------------------------------------------- |
| Đủ 5 thành phần chi phí,**không ô nào bằng 0 mà không có lý do** | Chỉ có token API                            |
| Mẫu số là**job hoàn thành**                                              | Chia cho job thử                             |
| Gross Margin ≥ 60% 🟩 (hoặc 50–60% 🟨 + kế hoạch cải thiện)                  | GM < 50% mà không có kế hoạch            |
| **Breakeven containment**đã tính và đối chiếu với eval                | Không biết mô hình gãy ở ngưỡng nào  |
| Ghi ngày kiểm tra giá API                                                        | Dùng giá khuyến mại như giá vĩnh viễn |

> Nếu GM quá cao (> 85%) — cũng là Fail. Gần như chắc chắn bạn đang quên chi phí, vì benchmark AI-native 2026 chỉ ~52–53%.

### 5.4 Checkpoint 4 — Kênh (phút 85)

Output: 4 con số + kênh đã chốt.

| ✅ Pass                                                         | ❌ Fail                                          |
| --------------------------------------------------------------- | ------------------------------------------------ |
| **Đúng 1 kênh**cho 90 ngày                            | Chọn 2–3 kênh "để linh hoạt"               |
| Có ngân sách CAC tính từ công thức                       | CAC lấy từ công ty khác                      |
| Có số deal/AE/ngày và kết luận khả thi hay không        | Nói "sẽ tuyển sales" mà không kiểm tra số |
| Partner-Led: có**tên công ty**+ trạng thái liên hệ | "Các nền tảng trong ngành"                   |

### 5.5 Checkpoint 5 — Pain Moment & Plan (phút 100)

| ✅ Pass                                                                   | ❌ Fail                              |
| ------------------------------------------------------------------------- | ------------------------------------ |
| Pain Moment đủ**3 phần** : giờ + việc + app                    | "Khi khách cần hỗ trợ"           |
| Điểm nhúng là một**bề mặt tích hợp cụ thể**              | "Website của chúng tôi"           |
| 90-day plan có**số**và**tên người chịu trách nhiệm** | Toàn động từ, không có số     |
| Tháng 1 là giai đoạn**học** , không phải scale               | Tháng 1 đặt mục tiêu 200 khách |

### 5.6 Checkpoint 6 — One-Pager (phút 120)

| ✅ Pass                                                         | ❌ Fail                                   |
| --------------------------------------------------------------- | ----------------------------------------- |
| Mọi con số truy được về một ô trong Excel               | Số trong One-Pager khác số trong Excel |
| Qua được**bài test người lạ**(≤ 3 câu hỏi lại) | Người đọc phải hỏi lại 5–6 câu   |
| Evidence Pack: ô nào chưa có thì**có deadline**     | Ô trống                                 |

### 5.7 Lỗi thường gặp theo trạm

Trạm 1

* Chọn phiên bản "nền tảng" vì nghe sang → rơi vào ngân sách phần mềm đông đúc.
* Định nghĩa Job theo kỹ thuật ("1 request") thay vì theo giá trị ("1 ticket resolved").
* Định nghĩa Job quá rộng ("1 khách hàng hài lòng") → không đếm được → không tính được Cost/Job.

Trạm 2

* Chọn Outcome khi chưa đo được Attribution. Câu hỏi giết bạn: "Khách nói 'tôi mua vì quảng cáo, không phải vì AI' — bạn refund à?"
* Chọn Seat cho một AI agent chạy liên tục. Chính GitHub đã phải bỏ mô hình cũ vì lý do này (06/2026).
* Chọn Hybrid nhưng không nói được phí nền mua cái gì. Phí nền phải đổi lấy một thứ cụ thể (chỗ ngồi, dung lượng gói, SLA).

Trạm 3

| Bẫy                                                             | Dấu hiệu                             | Sửa thế nào                                                     |
| ---------------------------------------------------------------- | -------------------------------------- | ------------------------------------------------------------------ |
| Chỉ tính token API                                             | Cost/Job đẹp bất thường, GM > 85% | Điền đủ Infra + Retry + HITL                                   |
| Chia cho số job**thử**thay vì job**hoàn thành** | Cost/Job thấp hơn thực tế          | Mẫu số phải là job completed (S2)                              |
| Retry = 0%                                                       | Ô S6 trống                           | Dùng 5–10%, ghi là ước tính                                  |
| Quên HITL vì "AI tự chạy hết"                               | S7 trống ở Biến thể B              | Không AI nào đúng 100%. 5% sai cũng phải có người xử lý |
| Dùng giá khuyến mại như giá vĩnh viễn                    | Không có ngày kiểm tra             | Ghi ngày + tính thêm kịch bản giá list                       |
| Neo giá theo ChatGPT $20                                        | Giá trần thấp bất thường         | Neo theo**lương**hoặc**chi phí outsource**         |

Trạm 4

* Chọn 2–3 kênh cùng lúc. 90 ngày, đội nhỏ → chỉ đủ sức làm sâu 1 kênh.
* Partner-Led không tên. Phải có tên công ty và trạng thái liên hệ.
* PLG với ARPU cao. Ai tự bỏ vài trăm đô/tháng mà không nói chuyện với ai?
* Lấy CAC của công ty khác làm CAC của mình. Dùng công thức, đừng dùng con số nghe kể.

Trạm 5

* Pain Moment chung chung → chưa hiểu khách đủ sâu.
* Kế hoạch không có số ("tăng trưởng khách hàng") → không đo được → không biết khi nào cần đổi hướng.
* Tháng 1 đã đặt mục tiêu scale → bỏ mất giai đoạn học.

### 5.8 Final Checklist — trước khi nộp

```
[ ]  1. Tab 1 — đủ 5 thành phần chi phí, không ô nào trống vô lý
[ ]  2. Tab 1 — mẫu số là JOB HOÀN THÀNH, không phải job thử
[ ]  3. Tab 2 — Giá bán ≥ 3 × Cost/Job, Gross Margin ≥ 60%
[ ]  4. Tab 2 — Breakeven containment đã tính, đã so với eval
[ ]  5. Tab 3 — Value Metric + Decision Note + 2 benchmark có link
[ ]  6. Tab 4 — Ngân sách CAC, deal/AE/ngày, 1 kênh duy nhất
[ ]  7. Tab 5 — 90-day plan có số; Evidence Pack có deadline
[ ]  8. Ghi ngày kiểm tra giá API ở tab 6_Benchmarks
[ ]  9. One-Pager — 3 khối, mọi số khớp Excel
[ ] 10. Đã chạy ít nhất 2 prompt ở §4.7 và ghi lại accept/reject
```

Chép

Minimum bar: một người lạ mở file Excel và đọc One-Pager phải hiểu được — bạn bán gì, cho ai, tính tiền thế nào, có lãi trên mỗi đơn vị không, và tiếp cận khách qua đâu — mà không cần hỏi lại quá 3 câu.

Nộp bài
6.1 Artefact cần nộp
Mục	Yêu cầu
Nộp khi nào	Trước buổi tiếp theo
Nộp ở đâu	Submit link repo chứa bài làm trực tiếp trên Vlearn
Tên repo	Track1_Day22_MHV_HoVaTen
Tên file	[Tên]_Day22_model.xlsx và [Tên]_Day22_onepager.pdf
Bao gồm	(1) File Excel 5 tab hoàn chỉnh · (2) Monetization One-Pager (PDF hoặc DOCX)
6.2 Rubric — 100 điểm, 5 tiêu chí

# Tiêu chí	Điểm

1	Cost/Job Rigor	30
2	Value Metric Justification	25
3	Channel Evidence	20
4	Pain Moment & 90-Day Plan	15
5	Evidence Pack Readiness	10
Tiêu chí 1 — Cost/Job Rigor (30 điểm). Chấm độ chặt của mô hình chi phí. Có đủ 5 thành phần (API, Infra, HITL, Retry, Overhead) không? Có phân biệt đúng Biến thể A/B của HITL không? Mẫu số có phải job hoàn thành không? Có tính breakeven containment và đối chiếu với eval không? Giá API có ngày kiểm tra không?

Điểm cao nhất dành cho bài trả lời được: "Mô hình của tôi gãy khi biến nào vượt ngưỡng nào." HITL hoặc Retry = 0 mà không có lý do → mất tối thiểu 10 điểm. GM > 85% mà không giải thích được → coi như thiếu chi phí, trừ như bỏ sót.

Tiêu chí 2 — Value Metric Justification (25 điểm). Chấm chất lượng lập luận, không chấm lựa chọn. Seat, Usage, Outcome hay Hybrid đều có thể đạt điểm tối đa — miễn là lý do đứng vững. Có dùng ma trận Attribution × Autonomy không? Nếu lệch khỏi gợi ý, lý do thị trường có cụ thể không? Có 2 benchmark thật kèm link không?

Chọn Outcome mà không có bằng chứng Attribution → tối đa 10/25.

Tiêu chí 3 — Channel Evidence (20 điểm). Chấm việc chứng minh bằng số, không chấm việc chọn kênh nào. Có tính ngân sách CAC từ công thức không? Có kiểm tra số deal/AE/ngày không? Có đối chiếu với cost-per-opportunity không? Chốt đúng 1 kênh chưa?

Partner-Led mà không có tên công ty cụ thể → tối đa 8/20. Chọn nhiều hơn 1 kênh → trừ 5 điểm.

Tiêu chí 4 — Pain Moment & 90-Day Plan (15 điểm). Pain Moment có đủ 3 phần (giờ + việc + app) không? Điểm nhúng có phải một bề mặt tích hợp cụ thể không? Plan có số, có người chịu trách nhiệm, và Tháng 1 có phải giai đoạn học không?

Pain Moment chung chung ("khi khách cần AI") → 0 điểm phần này.

Tiêu chí 5 — Evidence Pack Readiness (10 điểm). Ba tài sản có được liệt kê với nội dung cụ thể hoặc deadline rõ ràng không? Có nối được với Evals và Procurement Q&A không?

Ô trống, không nội dung không deadline → 0 điểm dòng đó. Thành thật về khoảng trống được điểm cao hơn bịa nội dung.

6.3 Grade bands
Band	Điểm	Ý nghĩa
Outstanding	90–100	Có thể đem One-Pager này đi gặp khách hoặc nhà đầu tư mà không cần sửa
Strong	75–89	Đủ chất để trình sếp, cần làm rõ 1–2 điểm
Pass	60–74	Hiểu khái niệm nhưng số liệu còn yếu, cần revise trước khi defend
Needs rework	40–59	Sai một khái niệm core (ví dụ: chia cho job thử, quên HITL)
Fail	< 40	Chưa đạt minimum bar
6.4 Lưu ý khi làm bài
Đừng bịa số đẹp. GM 90% với Cost/Job chỉ có token API = bị phát hiện ngay.
HITL và Retry ≠ 0. Hai cục hay quên nhất, và cũng là hai cục được chấm nặng nhất.
Chia cho job hoàn thành, không phải job thử. Sai chỗ này là sai toàn bộ mô hình.
Value Metric nào cũng được — lý do mới quan trọng.
Ghi ngày cho mọi con số giá. Một mô hình không ghi ngày là một mô hình không tin được.
AI là công cụ critique, không phải author. Quyết định cuối cùng là của bạn, và phải do bạn viết lại bằng lời của mình.
6.5 Câu chốt mang theo
Câu chốt
"Chạy được là bài toán kỹ thuật. Bán được là bài toán sinh tồn."
"Chi phí biên của AI không bao giờ về 0 — Microsoft chịu được, startup thì không."
"Giá là Value Metric, không phải con số. Chọn sai đơn vị thì mọi mức giá đều sai."
"Không có Eval thì không có Attribution. Không có Attribution thì đừng bán Outcome."
"Định nghĩa 1 job phải trùng với thứ khách coi là giá trị."
"Chia cho job HOÀN THÀNH, không phải job đã thử."
"HITL và Retry là hai cục hay quên nhất — và là hai cục giết biên lợi nhuận."
"GM 85% ở mô hình AI thường không phải bạn giỏi — mà là bạn quên một khoản."
"Đừng để khách so bạn với ChatGPT $20. Hãy để họ so với lương nhân viên."
"Kênh phân phối = Thói quen. Đừng bắt khách mở thêm một tab mới."
"Pain Moment quyết định kênh phân phối — không phải ngược lại."
"Partner-Led không có tên công ty thì chưa phải là kênh."
"90 ngày, đội nhỏ — chỉ đủ sức làm sâu 1 kênh."
"Procurement mua sự an tâm, không mua demo."
"Pricing là giả thuyết, không phải chân lý. Không dám đổi giá mới là thất bại."
"Đừng bán AI. Hãy bán kết quả."
Chúc các bạn bán được hàng. From a product that runs → to a product that sells.
