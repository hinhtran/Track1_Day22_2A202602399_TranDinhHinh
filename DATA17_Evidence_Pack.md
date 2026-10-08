# DATA-17 — Evidence Pack cho Lab22

Trần Đình Hinh · 2A202602399 · 08/10/2026

**Trạng thái:** kế hoạch và dự thảo cho dự án đang triển khai. Chưa có raw eval, pilot, khách ký hợp đồng hoặc security review. Không sử dụng các tài liệu này như chứng nhận kết quả đã đạt.

## 1. Eval Results — kế hoạch kiểm định

**Accountable:** Trần Đình Hinh; vai trò ML/DE cần bố trí. **Deadline:** 07/11/2026.

**Dataset:** holdout10,000 pairs đại diện theo nguồn Web/App/Referral và độ khó. Nếu thêm safety set có chọn lọc các ca khó, báo riêng; không dùng tập đó để ước tính containment sản xuất.

### Phương pháp

1. Gán nhãn same-person/different-person/uncertain. Hai Steward thẩm định, adjudication khi bất đồng. Mask PII khi kiểm thử.
2. Split theo entity/connected cluster, tránh cùng người nằm cả train và test qua các pair khác nhau. Giữ test riêng và freeze trước tuning threshold.
3. Chạy shadow mode trước auto-merge; ghi model/rule version, recommendation, quyết định người, attempt, token/cost và latency.
4. Cost tính cả fail/timeout/retry; quality window14 ngày áp dụng cho clean containment. Báo kết quả đúng nhãn riêng với proxy không revert trong pilot.
5. Safety cases: tên/điện thoại lỗi, số dùng chung, thiếu PII, xung đột cannot-link, nguồn mới, prompt injection trong text. Kiểm tra rollback không phá lineage/transitive clusters.

| Metric | Định nghĩa | Mục tiêu/điều kiện | Kết quả |
|---|---|---|---|
| Pairwise precision/recall |TP/(TP+FP); TP/(TP+FN) theo nhãn độc lập |Báo theo nguồn/độ khó; ngưỡng duyệt với Steward |Chưa đo |
| Clean auto-containment14d |Auto-committed pair không revert14d / unique attempts cùng cohort |80% là giả định Excel cần xác nhận |Chưa đo |
| Auto false merge |Wrong auto-approve / all auto-approve trên nhãn độc lập |Gate bổ sung<0.1%; báo CI và mẫu số |Chưa đo |
| False separate |Wrong reject / actual duplicate pairs |Báo riêng, không che bằng containment |Chưa đo |
| RevertRate14d |Merge bị revert / merge đủ14d |≤1% theo metrics pack; không thay false merge |Chưa đo |
| LLM cost/resolved |Tổng LLM cả fail/retry / completed đúng loại job |≤$0.02 theo metrics pack |Chưa đo |
| Total cost/resolved |API+infra+HITL+retry / completed |Thay giả định Excel bằng cost đo |Chưa đo |
| Latency/error |p50/p95, failure/timeout/retry |Chốt SLA trước chọn batch |Chưa đo |

Không có lỗi trên mẫu nhỏ không chứng minh false merge<0.1%. Ví dụ0 lỗi/3,000 auto-approve có upper bound binomial95% **một phía** là 1−0.05^(1/3000)≈0.09981%; trên1,000 ca≈0.2991%, chưa đủ. Đây là minh họa thiết kế cỡ mẫu, không phải kết quả eval. Nếu yêu cầu CI hai phía phải dùng đúng CI hai phía và tăng mẫu. Các pair cùng entity có thể phụ thuộc; không mặc định mọi pair độc lập.

**Gate tự động hóa:** đủ mẫu, đạt chất lượng được phê duyệt; log actor/commit/version; rollback qua nghiệm thu; đủ audit. Score≥0.85 riêng lẻ không đủ đảm bảo an toàn. Chưa qua gate giữ human review. Không tự huấn luyện từ nhãn production bỏ qua regression eval/phê duyệt version.

**Báo cáo cuối phải gắn:** dataset manifest/size, split IDs, label guideline, thresholds, model/commit version, confusion matrix, segmented metrics, CI/mẫu số, cost raw log, lỗi và signoff. Hiện chưa có các tệp này.

## 2. Procurement Q&A — bản trả lời dự thảo

**Accountable:** Hinh; Security/Legal là vai trò cần bố trí. **Deadline:**14/11/2026. Chưa được IT/khách phê duyệt.

### Câu1: Nếu AI gộp nhầm thì sao?

DATA-17 thiết kế Evidence Card, quyền duyệt/chặn merge, cannot-link và audit. Vùng xám do Steward quyết định. Chỉ bật auto sau holdout/pilot qua quality gate; không cam kết “không có rủi ro gộp nhầm”. Mọi quyết định phải lưu version, evidence và lineage để điều tra, revert/unmerge và credit nếu sai ở usage. Chưa có kết quả eval/rollback test để chứng minh đã đáp ứng.

**Bằng chứng cần:** Eval Results; incident/rollback runbook; trách nhiệm SLA; workflow dispute khách phê duyệt. **Còn thiếu:** rollback test độc lập, SLA đã ký.

### Câu2: Dữ liệu có bị dùng train model không? PII đi đâu?

[Gemini API pricing](https://ai.google.dev/gemini-api/docs/pricing), mở08/10/2026, ghi paid tier không dùng để improve products. Điều này **không tự chứng minh zero retention, data residency hoặc dữ liệu chỉ ở VPC khách**. Dự kiến mask/tokenize PII, gửi tối thiểu thông tin, không log raw PII; cần thẩm định DPA, retention, vùng xử lý và subprocessor trước gửi PII thật.

**Bằng chứng cần:** data flow, field allowlist, masking tests, provider plan/DPA/retention/residency, RBAC, log access policy. **Còn thiếu:** DPA/điều kiện cụ thể và kiểm tra PII. Trước phê duyệt dùng dữ liệu giả lập/ẩn danh được cấp quyền.

### Câu3: Startup ngừng hoạt động thì data ở đâu?

Thiết kế exit cho phép xuất Golden Record, source IDs, cannot-link, quyết định/evidence/audit và rules version qua JSON/CSV. Đích lưu/bản sao ở khách, thời điểm export, truyền giao và xóa phải ghi trong hợp đồng. Không hứa đã có VPC/backup/export khi chưa có kiểm thử.

**Bằng chứng cần:** exit runbook, export manifest/schema, backup/restore test, quyền sở hữu dữ liệu, retention/xóa/subprocessor và owner. **Còn thiếu:** export/restore validated và thỏa thuận dịch vụ.

### Mười câu IT/Procurement cần chốt

| Câu hỏi | Bằng chứng cần | Trạng thái |
|---|---|---|
|PII gửi API qua đường nào? |Data flow/allowlist/masking test |Chưa xác minh |
|Auto-merge sai xử lý thế nào? |Eval + rollback test + incident owner |Chưa kết quả |
|Raw PII giữ bao lâu/vùng nào? |DPA, retention/residency, subprocessors |Chưa thỏa thuận |
|Ai quản trị từng tenant? |RBAC, isolation test, audit access |Chưa kiểm thử |
|Token/billable event đếm trùng không? |Idempotency + billing reconciliation |Có contract; chưa backend test |
|Nhãn sai làm model học sai không? |Adjudication/version/regression gate |Có kế hoạch; chưa kiểm thử |
|SLA/incident response thế nào? |SLA/SLO theo phạm vi |Chưa ký |
|Backup/restore/export dùng được không? |Restore/export drill và manifest |Chưa kiểm thử |
|Dispute/credit và cap xử lý sao? |Billing policy + audit/manual approval |Dự thảo |
|Connector custom/setup tính sao? |Scope/price/acceptance contract |Cần discovery |

## 3. Pilot Report — mẫu và kế hoạch

**Accountable:** Hinh; Steward và Head of Data khách xác nhận. **Khách:**2 design partners dự kiến, chưa có tên/đồng ý.

**Chạy dự kiến:**16/11–13/12/2026; mỗi pilot4 tuần/10,000 pairs. **Cohort cuối đủ14 ngày:**27/12. **Report:**06/01/2027.

**Câu hỏi:** AI assistance giảm review time/cải thiện quality queue không? Có thể chuyển ca đủ an toàn thành auto-completion không? Buyer chấp nhận hóa đơn và ROI toàn gói không?

### Phương pháp

- Manual baseline vs AI-assisted, cân đối cùng Steward/nguồn/độ khó; thống nhất đo active review time, loại idle.
- Kiểm tra autonomy bằng shadow và nhãn độc lập; recommendation acceptance không phải containment.
- Effort gồm ca khó, QA, correction và rollback, không chỉ đo ca dễ.
- NSM giữ định nghĩa Steward/cùng cohort/window14d; báo auto-clean throughput riêng. Reject theo decision revert, không chỉ cluster unmerge.
- Retention chỉ tính users có cơ hội batch phù hợp; báo eligible users/jobs.
- Pilot phí cố định theo phạm vi đàm phán; không mặc định hóa đơn$4,200 khi còn human assistance.

| Metric | Baseline | Có AI | Mục tiêu | Dữ liệu |
|---|---|---|---|---|
|Median active review time |Chưa đo;60s là ước tính |Chưa đo |≤25s, giảm58.33% nếu baseline60s |Review start/resolved timestamps |
|Weekly clean Steward resolves |Chưa đo |Chưa đo |+30%/4 tuần, cohort/input có thể so |Resolved actorSTEWARD, revert14d |
|Evidence attachment |Chưa đo |Chưa đo |≥80% |Evidence attached trên decision |
|Activation |Chưa đo |Chưa đo |5 ca/48h từ assigned |Queue assigned + Steward resolves |
|Weekly retention |Chưa đo |Chưa đo |≥10 ca/tuần;85% chỉ mục tiêu chưa benchmark |Cohort activation và returns |
|Revert/false merge |Chưa đo |Chưa đo |Revert≤1%; autoFM<0.1% gate bổ sung |Nhãn độc lập + reverted decision |
|Clean auto-containment |Không suy ra từ manual |Chưa đo |80% giả định scenario |ActorAI, attempts, quality14d |
|Token/cost/job |Chưa đo |Chưa đo |LLM≤$0.02/đúng loại resolved |Token ledger/invoices/cost allocation |
|WTP/budget/price |Chưa xác nhận |Chưa xác nhận |Buyer duyệt toàn gói hoặc điều chỉnh |Interview + written quote |

### ROI cần báo cáo

**Labor value saved** = (manual active minutes − total assisted active minutes) × hourly rate fully-loaded được khách xác nhận /60.

**Net customer value** = labor value saved − platform/usage fees − incremental setup/training/QA/correction costs.

Manual minutes và assisted minutes phải cùng khối lượng/độ khó. Không cộng thêm “auto80%×1min” vào phần đã tính tổng tiết kiệm, tránh đếm hai lần. Báo thời gian tiết kiệm riêng với headcount reduction/thực thu. Nếu quy đổi tiền cần FX/date được khách xác nhận.

**Kết luận hiện tại:** chưa kết quả để quyết định mở usage. Go/No-go sau khi quality, ROI, WTP, security và billing verification hoàn thành. Thiếu bằng chứng thì tiếp tục human assistance/pilot hoặc đổi phạm vi, không tự scale.

**Xác nhận cần:** Head of Data, Steward đánh giá, Hinh; chưa cung cấp tên/ngày/signoff. **Attachments cần:** event log ẩn PII, dataset manifest, config/version, baseline comparison, incidents, cost ledger, written approval; chưa có.

## 4. Event contract tối thiểu

candidate_pair_resolved phát sau commit, kèm tenant_id, canonical_pair_id, actor_type, decision_version, decision, model_version, rule_version, recommended_decision, review_started_at, committed_at, evidence_attached, trace_id.

decision_reverted trỏ đúng decision_version và reason; cluster_unmerged là subtype cho merge.

agent_attempt_completed/token ledger ghi cả success/fail/timeout, input/output/thinking/cached tokens, price version, latency, retry và infra allocation. Không bỏ cost failed attempts. Đây là contract đề xuất, chưa xác nhận backend đã có.

Chỉ tính quality cohort đủ14d; deduplicate tenant/pair/version; tách AI/STEWARD. Một idempotency key tái sử dụng cho cùng intent, không đổi timestamp mỗi retry. HTTP200 nhưng commit failed, thiếu actor/audit hoặc version trùng thì không phát billing event.
