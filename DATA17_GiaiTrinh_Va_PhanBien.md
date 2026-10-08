# DATA-17 — Bài Lab 22 Monetization và GTM

**Trần Đình Hinh · 2A202602399 · P-149 · 08/10/2026**

## 1. Quyết định và cơ sở số liệu

DATA-17 tập trung vào **Data Steward khối VSF**, bán trực tiếp cho **Head of Data/CDO** bằng **Sales-Led do Founder thực hiện**. Chọn **Hybrid**: phí nền duy trì pipeline và audit, usage chỉ áp dụng sau khi chứng minh khả năng tự xử lý và chất lượng. Hiện tại nên thương lượng pilot phí cố định theo phạm vi; chưa đủ bằng chứng bán Outcome.

Nguồn chính là **metrics-pack.md**, yêu cầu là **HD_Lab22.md**. Metrics pack chứa thiết kế và mục tiêu; chưa có raw events, hóa đơn API, holdout có nhãn hoặc kết quả pilot. Thư mục cũng chưa có backend để xác nhận tính năng đã triển khai. Không kết luận sản phẩm đã chạy hoặc chưa chạy chỉ từ mô tả.

| Loại dữ liệu | Nội dung | Cách dùng |
|---|---|---|
| Thiết kế trong metrics pack | Steward VSF; vùng xám 0.55≤score<0.85; Approve/Reject; batch tuần; quality window14 ngày | Nền cho user, workflow, tracking và pilot |
| Mục tiêu trong metrics pack | Activation5 ca/48h; review60→25s; NSM+30%/4tuần; evidence≥80%; revert≤1%; LLM≤$0.02/ca | Ghi là mục tiêu; baseline60s cũng cần đo |
| Giả định economics | 100k attempts; clean auto-containment80%; infra$0.005; QA1%; retry5%; lương$10/h; overhead$3k | Dự toán tương lai, không gọi là eval |
| Giá đã kiểm tra | Gemini3.7 Flash Standard paid; AWS ER; cách tính giá Tamr | Nguồn chính thức, ngày08/10/2026 |
| Chưa có bằng chứng | False merge, retention, khách trả phí, win rate, cost/job thực tế | Không tự đánh dấu đạt |

**80% không phải số đo trong metrics pack.** Giữ làm kịch bản thương mại có điều kiện để tính Cost/Job và stress test theo lab. Kịch bản100k tính trên toàn bộ candidate pipeline, gồm ca ngoài vùng xám; không giả định80% riêng queue vùng xám được AI tự quyết định. Nếu sản phẩm chỉ hỗ trợ Steward, chưa áp dụng auto-usage này; dùng pilot/seat và đo economics hỗ trợ người trước.

## 2. Định vị, ngân sách và job

**Định vị:** DATA-17 giảm việc đối chiếu hồ sơ nghi trùng từ Web, App và Referral bằng Evidence Card và Approve/Reject có audit, giúp Steward chốt mẻ dữ liệu tuần và cập nhật Golden Record an toàn hơn.

User là Data Steward; champion dự kiến là Head of Data; CDO/IT phê duyệt từ ngân sách **Data Operations/quản trị chất lượng dữ liệu**, Procurement và Security đồng duyệt. Buyer và budget line là giả thuyết discovery, chưa có khách xác nhận.

Phân biệt hai loại job:

1. **Job hiện tại trong metrics pack:** một pair được Steward thẩm định, xác nhận Approve/Reject, commit merge/cannot-link và không revert14 ngày. Đây là core action cho activation, retention và NSM của Steward.
2. **Job dự toán auto-usage:** một pair do AI tự quyết định và commit, không cần người quyết định, không revert14 ngày. Chỉ loại này là mẫu số80k và usage$0.04 trong Excel. Ca Steward quyết định không thu usage tự động hóa.

Reject phải lưu quyết định không gộp/cannot-link; không bắt buộc tạo merge. HTTP200 không đủ chứng minh quyết định đúng; cần commit và audit state. Không revert14 ngày là quality proxy, không chứng minh tuyệt đối chính xác; vẫn phải đối chiếu nhãn và theo dõi lỗi muộn.

Hóa đơn deduplicate theo **tenant_id + canonical_pair_id + decision_version**, với idempotency key tạo một lần và tái sử dụng khi retry. Không tạo timestamp mới trong key cho mỗi click. Pair sai phát hiện sau14 ngày vẫn phải có dispute/credit/rollback. Mô hình là tháng **steady state**; tháng đầu usage chờ cohort đủ tuổi, không dự báo thu ngay.

## 3. Cost/Job đủ năm thành phần

Đã điền5 tab làm việc, giữ7 tab và toàn bộ công thức gốc. Thêm đối soát Hybrid vì mẫu chỉ tính giá/job; nguồn/giả định thêm dưới bảng tham chiếu. Benchmarks hàng1–43 giữ nguyên, không coi là giá đang dùng.

[Gemini API pricing](https://ai.google.dev/gemini-api/docs/pricing), kiểm tra08/10/2026: Standard paid input$0.75/output$3.75 mỗi1M tokens, tăng lên$1.50/$7.50 từ01/01/2027. Không cache/batch trong base. Budget1,000input+200output/attempt là tổng mọi agent/call, kể cả thinking, chưa phải log. Một lượt tương đương trong mẫu dùng để cộng chi phí, không khẳng định kiến trúc chỉ có một agent.

| Thành phần | Cách tính | USD/khách/tháng | Ô Excel |
|---|---|---:|---|
| API LLM |100k×(1,000×0.75+200×3.75)/1M |150.00 |1_Cost_Job!B72 |
| Infra |100k×0.005 |500.00 |1_Cost_Job!B74 |
| Retry |5%×150 |7.50 |1_Cost_Job!B75 |
| HITL QA |100k×1%×1/60×$10 |166.67 |1_Cost_Job!B76 |
| Overhead |R&D/Sales phân bổ giả định |3,000.00 |1_Cost_Job!B77 |
| COGS |API+Infra+Retry+QA |**824.17** |2_Pricing!B60 |
| Tổng có overhead |824.17+3,000 |**3,824.17** |1_Cost_Job!B65 |

**HITL A:** khách trả lương20k ca escalate; bằng0 trong COGS nhà cung cấp nhưng không bằng0 đối với khách. QA nội bộ vẫn tính. Speech/telephony0 vì không thoại. Retry chỉ gồm API theo công thức mẫu; nếu rerun pipeline cần thêm infra retry.

Infra$500 là dự toán: graph/vector100, compute200, storage/egress50, logging/eval50, support100. Chưa có hóa đơn. Custom connector, onboarding, nhãn eval ban đầu, security/legal một lần cần scope và báo giá/phân bổ riêng; không coi đã được bao hết. Overhead$3k là phân bổ/khách giả định, không tổng toàn công ty.

- Completed =100,000×80%=80,000 clean auto-resolves.
- Cost/job COGS =824.166667/80,000=**$0.010302083**.
- Cost/job có overhead =3,824.166667/80,000=**$0.047802083**.
- Chia attempts cho ra$0.008241667, thấp hơn cost đúng20%; đổi sang completed làm cost tăng25%.

FX26,000VND/USD là giả định minh họa kế thừa mẫu, chưa phải tỷ giá ngân hàng. Mọi quyết định giá/CAC dùng USD.

## 4. Giá và ngưỡng hòa vốn

Giá usage$0.04 trên sàn3×cost=$0.03090625. Base$1,000/tháng gồm5 seats, **zero free usage quota**. Base là mức thử nhằm bao footprint vận hành giả định$500 cùng dự phòng; chưa có WTP. Cap$5k phải có phê duyệt tăng ngân sách trước xử lý vượt, không xử lý vô hạn miễn phí.

Giá trị thời gian giả định80k×1phút×$10/h=$13,333.33/tháng. Trần25% là$0.0416667/pair; usage nằm dưới, nhưng giá hiệu dụng cả gói$0.0525 chiếm31.5%. Không nói cả gói dưới trần25%; cần kiểm tra WTP hoặc neo theo nhân công50–70%. Thời gian tiết kiệm chưa chắc chuyển thành giảm lương.

| Chỉ số | Kết quả | Ô Excel |
|---|---:|---|
| Revenue usage |$3,200 |2_Pricing!B58 |
| Revenue Hybrid |$4,200 |2_Pricing!B59 |
| Usage GM |74.2448% |2_Pricing!B21 |
| Hybrid GM |80.3770% |2_Pricing!B61 |
| Còn sau overhead |$375.83/tháng |2_Pricing!B62 |

GM không phải lợi nhuận ròng. $375.83 chưa gồm thuế/chi phí một lần chưa xác minh. Sales trong overhead là phân bổ vận hành; CAC đánh giá chi phí thu hút khách/payback, không cộng lần nữa vào lợi nhuận tháng. Chưa có forecast cash flow thu CAC upfront.

Đặt **c=v+q=$0.008241667/attempt**, e là cost escalate nhà cung cấp chịu, R clean auto-containment, P=$0.04:

- Cost/job=[c+e(1−R)]/R.
- GM=1−[c+e(1−R)]/(R×P).
- Để GM≥g: **R≥(c+e)/[P(1−g)+e]**.

| Ngưỡng khi HITL A, e=0 | R | Ô Excel |
|---|---:|---|
| Hòa vốn usage GM0 |20.6042% |2_Pricing!B63 |
| Usage GM50% |41.2083% |2_Pricing!B64 |
| Usage GM60% |51.5104% |2_Pricing!B33 |
| Hybrid hòa vốn sau overhead ở100k attempts |70.6042% |2_Pricing!B68 |

Ô mẫuB33 tên “Breakeven containment” thực chất là ngưỡng **GM mục tiêu60%**, không GM0. Chưa có eval để so thực tế: chỉ so scenario80% với51.51%, không claim chất lượng đã đạt.

| R | Cost/job | Usage GM |
|---:|---:|---:|
|50% |$0.0164833 |58.79% |
|60% |$0.0137361 |65.66% |
|70% |$0.0117738 |70.57% |
|80% |$0.0103021 |74.24% |
|90% |$0.0091574 |77.11% |

## 5. Value Metric và benchmark

Attribution **2/10** chỉ từ định nghĩa job/billing; chưa có log, eval, khách đồng ý và bằng chứng phân actor. Autonomy **0/10** do chưa có bằng chứng job tự chạy end-to-end. Điểm0 là chưa đủ bằng chứng, không kết luận code không có tính năng. Model gợi ý SEAT/HYBRID; chọnHybrid phù hợp. Decision Note3 câu ở3_Value_Metric!B32:B34.

Hai benchmark kiểm tra08/10/2026:

- [Tamr](https://www.tamr.com/pricing): subscription + volume golden records đầu ra; không công bố đơn giá, ghi “liên hệ báo giá”.
- [AWS Entity Resolution](https://aws.amazon.com/entity-resolution/pricing/): rule/ML$0.25/1,000 input records processed, cả non-match. Pair/record/golden record khác đơn vị; không so đơn giá trực tiếp.

AWS phản biện giá matching cơ bản rất thấp. DATA-17 phải chứng minh thêm Evidence Card, audit/rollback, cannot-link và thời gian Steward; “multi-agent” chưa đủ lý do tính giá cao.

## 6. Sales-Led và CAC

| Phép tính | Kết quả |
|---|---:|
|ARPU; ACV |$4,200; $50,400 |
|CAC cap template: ARPU×GMusage×24th |$74,838.75 |
|CAC giả định: CPO$8k/win25% |$32,000 |
|CAC/cap |0.427586× |
|Quota$500k/ACV |9.920635deals/năm |
|Deals/250ngày |0.039683/ngày |
|Payback Hybrid:32k/(4,200−824.166667) |9.4791tháng |
|Cap nội bộ12th theo Hybrid gross profit |$40,510 |
|CPO cap nội bộ tạiwin25% |$10,127.50 |

Tab4 giữ công thức GM usage, nên cap template bảo thủ hơn HybridGM; quyết định nội bộ chặt hơn với **payback12th**, không chi theo ceiling24th.

CPO8k giả định32k all-in/4qualified opportunities; win25%, quota500k,250days chưa đo, không gọi là benchmark mới đã kiểm chứng. Chưa churn/lifetime nên không bịaLTV:CAC≥3.

Scorecard PLG13, Sales-Led20, Partner-Led9 (4_Channel_Fit!B34:D34). Buyer khác user, dữ liệu nhạy cảm và connector cần discovery, nên founder bán trực tiếp. Năng lực đội/điểm nhúng chỉ chấm2–3 vì chưa xác nhận. Khả thi số học ở giá **sau pilot**, chưa chứng minh chốt được deal. Một hợp đồng90ngày là mục tiêu; chu kỳ procurement có thể dài hơn.

## 7. Pain Moment và kế hoạch90ngày

**09:00 thứ Hai**, Steward VSF rà backlog sau weekly batch trong console MDM, Review Queue/Evidence Card DATA-17. Nhúng bằng chứng và Approve/Reject ngay queue, API nối Web/App/Referral pipeline. Giờ09:00 và app hiện tại cần discovery; đây là bề mặt thiết kế, chưa khẳng định khách đang dùng DATA-17. Không tự nhận Airflow/Salesforce đã triển khai.

| Giai đoạn | Hành động/KPI mục tiêu | Owner |
|---|---|---|
|08/10–07/11/2026 |30accounts,10interviews,4opportunities,2LOI; holdout10kpairs,1connector |Hinh; DE/ML cần bố trí |
|08/11–06/01/2027 |2pilot×4tuần×10kpairs; dự kiến16/11–13/12, cohort cuối đủ14d ngày27/12; report06/01;1hợp đồng |Hinh, Steward khách; Security/ML cần bố trí |
|Từ07/01/2027 |Mục tiêu3khách cùng ngách; scale khi≥2mẻ đủ14d, quality đạt, usageGM≥60%, payback≤12th và pipeline thật |Hinh; Sales/CS khi có ngân sách |

Review60→25s giảm **58.33%**, không70%. NSM+30%/4tuần cần so cùng cohort/input volume/độ khó, không coi tăng do nạp thêm dữ liệu là hiệu quảAI. Weekly retention85% là mục tiêu thiết kế chưa có nguồn benchmark. R80% và falsemerge<0.1% là gate tự động hóa bổ sung, không thành tích hiện có.

## 8. Phản biện AI theo§4.7

Đã rà theo hai prompt Cost Auditor và Channel Reality Check, đối chiếu phép tính bằng Excel và tính độc lập. Đây là phản biện của bài làm, không phải review khách hay kết quả pilot. Prompt tiếng Anh, quyết định tiếng Việt.

### Prompt1 — Cost/Job Stress Test

> Act as a ruthless CFO and a skeptical infrastructure engineer. Stress-test DATA-17 with 100,000 attempts/month, an assumed 80% clean autonomous completion after14 days, HITL A, 1,000 total input and200 total output tokens across all agents, prices$0.75/$3.75 per1M tokens, infrastructure$0.005/attempt, 5% API retry, 1% internal QA at one minute and$10/hour, and$3,000 monthly allocated overhead. Usage is$0.04 and base is$1,000, with no free usage quota. Audit missing costs, attempted versus completed denominators, token math, caching/batch, API prices doubling on January1 2027, GM threshold algebra, R=50/60/70/80/90%, and the input whose2x error breaks the model. Do not treat assumptions as measured eval results.

| Phản biện | Quyết định | Kết quả/lý do |
|---|---|---|
|Chia100k vì pipeline chạy100k |Reject |Cost phát sinh trên100k nhưng mẫu số80k completed |
|HITL A nên bỏQA |Reject |QA vẫn$166.67; khách chỉ chịu escalate |
|Infra2× |Accept |COGS$1,324.17; usageGM58.62%; sau overhead−$124.17 |
|API2× từ01/01/2027 |Accept |COGS$981.67; cost$0.0122708; usageGM69.32%; sau overhead$218.33 |
|Managed outcome giữA |Reject |B thêm$3,333.33; cost$0.05196875; usageGM−29.92% |
|Bậtbatch ngay vì workflowtuần |Partial |ThửSLA trước; nếu50%LLM+retry, COGS$745.42, GM76.71%; chưa là tiết kiệm đã đạt |
|Caching giảm90%toàn cost |Reject |Chưa cachehit/TTL; base khôngcache |
|Retry có cảinfra |Accept |Rerun5%infra thêm$25; COGS$849.17, usageGM73.46%; phải đo loại retry |
|GMcao đảm bảo lãi |Reject |Sau overhead chỉ$375.83; chi phí một lần chưa xác minh |

**Infra** là biến2× làm gãy ở scenario này. HITL đổi người chịu cũng làm economics đổi mạnh. Giữ R80% và thành phần khác: usageGM<50% khi infra>$0.01275833/attempt; mất phần còn sau overhead khi infra>$0.00875833/attempt.

### Prompt2 — Channel Reality Check

> I am choosing one GTM channel for DATA-17 for the next90 days. Forecast ARPU is$4,200/month, ACV$50,400, usage GM74.2448%, hybrid GM80.3770%, enterprise CAC cap$74,838.75, and an internal12-month payback limit. Assume CPO$8,000, win25%, AE quota$500,000/year and250 working days. Choose founder-led Sales-Led. The hypothesized pain moment is Monday09:00: a VSF Data Steward reviewing the weekly duplicate backlog in an MDM review queue. Calculate CAC, deal throughput, affordability and payback. Challenge assumptions, the integration surface and closing within90 days. State the strongest objection and conditions before scaling.

| Phản biện | Quyết định | Kết quả/lý do |
|---|---|---|
|Sales-Led nuôi được |Partial |Khả thi số học, gap0.428×; chưa pipeline/win/CPO thực tế |
|Chi theo CAC cap24th |Reject |Cap nội bộ12th=$40,510 |
|Thuê AE ngay vì0.04deal/ngày |Reject |Founder discovery/pilot trước; chưa demand |
|Ghi Airflow/Salesforce đã triển khai |Reject |Không có trong metrics pack; xác minh app trước |
|1hợp đồng90ngày guaranteed |Reject |Mục tiêu; security/procurement có thể trễ |
|Pain Moment tháng lệch cadence |Accept |Đổi weeklybatch theo nguồn |
|Partner-Led bỏCAC |Reject |Chưa partner cụ thể/đã nói chuyện; chốtSales-Led |

Phản biện mạnh nhất: chưa chứng minh tự xử lý sạch nên chưa chắc đạt ARPU saupilot.90ngày kiểm chứng quality, WTP và buyer, không coi scorecard là thị trường.

## 9. Evidence Pack và tracking cần triển khai

**DATA17_Evidence_Pack.md** chứa Eval plan, Procurement Q&A draft và Pilot Report template, với owner/deadline. Không bịa khách hoặc kết quả.

Các sửa thiết kế tracking cần thực hiện:

- Thêm actor_type, decision_version, committed_at, recommended_decision, review_started_at, evidence_attached, token/cost và reverted_at.
- golden_record_merged chỉ choApprove, không làm mẫu sốEvidence Attachment của cảApprove+Reject.
- recommendation_viewed không chứng minh acceptance; so recommendation với finaldecision trên ca có recommendation.
- NSM Steward lọcactorSTEWARD/cohort đủ14d; cleanautoR/billing lọcAI. Không trộn âm thầm.
- Activation5ca/48h từqueueassigned; retentionW0 là tuần đạtactivation đó, không bỏ điều kiện48h.
- RevertRate tính trênmerge đủwindow; falsemerge theo nhãn trênautoapprove là chỉ sốkhác. Khôngrevert không đồng nghĩa khôngfalsemerge.
- Idempotency key tái sử dụng, không đổi timestamp mỗi retry.

## 10. Đối chiếu yêu cầu nộp

| Yêu cầu | Trạng thái |
|---|---|
|5tab + Cost5thành phần + HITL A/B |Đã điền, phân ai chịu chi phí |
|Completed, giá sàn/trần, GM, stress |Đã tính;80k là scenario sạch tương lai |
|Breakeven soeval |Đã tính; chưa eval, chỉ so giả định |
|ValueMetric/3câu/2benchmark |CóTab3 vàTamr/AWS |
|CAC/dealsngày/1kênh |CóTab4, Sales-Led |
|Pain Moment/90ngày/owner |CóTab5, nhịp tuần |
|Evidence thiếu códeadline |07/11,14/11,06/01 và tài liệu dự thảo |
|Giá API cóngày |MởGoogle08/10/2026, ghiTab6 |
|OnePager1trang khớpExcel |PDF1trang, mappingô và kiểm tra ảnh |
|≥2prompt/acceptreject |Cóphần8 |
|Người lạđọc2phút/hỏi≤3 |Chưa thực hiện; không tự đánh dấu |
|Submitrepo/Vlearn |Chưa gửi; người học nộp link repo |

Dự án đang phát triển được lab cho phép dùng ước tính có giải thích. Bộ bài hoàn thành phần mô hình dự toán và kế hoạch; không tự chấm100/100 khi chưa có kiểm chứng ngoài bài viết.

## 11. Kiểm tra tệp và nguồn

Nguồn nội bộ: metrics-pack.md§§00–06, HD_Lab22.md§§2.2,3.2,4.1–4.7,6.1–6.2. Nguồn giá là ba trang chính thức đã dẫn, ngày08/10/2026.

Excel tính/render bằngArtifactTool; giữ nguyêntext công thức gốc, làm mới cache mẫu cũ; đối soát tổngcost và sensitivity. Kiểm tra inputR, API2×, infra2×, HITL A/B trong bảnmemory khônglưu. Không claim testMicrosoftExcelnative. Mẫu hiển thịcost0 khiR0; ghi rõ đó không phải costmiễn phí.

PDF một trang đã render/kiểmtra chữ Việt và bố cục. Wordrenderer khôngchạy do GLIBC/GLIBCXX hệ thống không tương thích. PDF là bản nộp chính; Markdown là nội dung sửa được. DOCX cũ cất trong thư mục làm việc, không đưa làm bản nộp đã kiểmtra.
