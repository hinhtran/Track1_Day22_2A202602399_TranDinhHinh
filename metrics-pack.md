# METRICS PACK: P-149 (DATA-17) AI AGENT — ENTITY RESOLUTION & MASTER DATA MANAGEMENT

> **Họ và tên:** Trần Đình Hinh  
> **Mã học viên:** 2A202602399  
> **Dự án:** P-149 (DATA-17) Multi-Agent Entity Resolution & Master Data Management  
> **Vai trò:** Product Manager (PM)  

---

## 00 — Dự án, Persona, Core Job

### 1. Dự án
**P-149 (DATA-17):** Hệ thống Multi-Agent Entity Resolution & Master Data Management kết hợp Machine Learning và cơ chế Human-In-The-Loop (HITL). Hệ thống tự động khử trùng lặp và hợp nhất các hồ sơ khách hàng bị phân mảnh từ nhiều nguồn (Web, Mobile App, Referral Partner) để kiến tạo **Golden Record (Single Customer View)** chuẩn xác, chống nguy cơ gộp nhầm (False Merge) và sót trùng lặp (False Separate).

### 2. Persona (Chọn 1 duy nhất)
* **Đối tượng:** **Data Steward (Chuyên viên Quản trị Chất lượng Dữ liệu Master Data)**.
* **Bối cảnh:** Làm việc tại khối VSF / Data Management, chịu trách nhiệm thẩm định các trường hợp nghi trùng lặp ở "vùng xám" (độ tương đồng $0.55 \le \text{Score} < 0.85$) do AI Agent chuyển giao, đồng thời chịu trách nhiệm về tính toàn vẹn của danh tính khách hàng trong toàn doanh nghiệp.

### 3. Core Job (JTBD — Viết bằng tiếng lòng của user)
> *"Tôi mất quá nhiều thời gian rà soát thủ công hàng nghìn bản ghi khách hàng nghi ngờ trùng lặp từ Web, App, Đối tác, và tôi luôn nơm nớp lo sợ bấm gộp nhầm hai người khác nhau (False Merge) làm sai lệch lịch sử giao dịch và rủi ro bảo mật."*

---

## 01 — Core Action Card (+ Kết quả tự kiểm 5 tiêu chí)

### 1. Bảng phân biệt 4 khái niệm cốt lõi

| Khái niệm | Câu hỏi then chốt | Ứng dụng trong dự án P-149 |
| :--- | :--- | :--- |
| **Core Job** | User đang cố hoàn thành việc gì? | Hợp nhất chính xác hồ sơ khách hàng trùng lặp mà không gộp nhầm người |
| **Core Action** | User làm gì trong sản phẩm để tiến tới giá trị? | **Thẩm định và xác nhận quyết định xử lý một cặp hồ sơ nghi trùng (Resolve Candidate Pair: Approve Merge hoặc Confirm Reject)** |
| **Core Value** | User nhận được lợi ích gì? | Giải phóng backlog hàng đợi, hình thành Golden Record chuẩn sạch, an tâm về dữ liệu |
| **Core Value Event** | Sự kiện nào chứng minh value đã xảy ra? | `candidate_pair_resolved` (và `golden_record_merged` thành công) |

* **Lập luận vì sao KHÔNG chọn thao tác UI / Output hệ thống:**
  * *Không chọn "Đăng nhập" hay "Mở app":* Đây chỉ là thao tác truy cập giao diện, không tạo ra bất kỳ giá trị dữ liệu nào.
  * *Không chọn "Upload file dữ liệu" hay "Xem báo cáo":* Đây chỉ là bước chuẩn bị kỹ thuật.
  * *Không chọn "Output AI Agent gợi ý":* AI tạo ra điểm số matching chỉ là output thuật toán. Nếu Data Steward chưa thẩm định và xác nhận, hồ sơ chưa được cập nhật an toàn vào cơ sở dữ liệu Master Data.

---

### 2. Core Action Card

| Thành phần | Chi tiết cho P-149 |
| :--- | :--- |
| **Target user** | Data Steward (Chuyên viên Quản trị Chất lượng Dữ liệu). |
| **Core job** | Thẩm định và xử lý dứt điểm các ca hồ sơ nghi trùng lặp trong hàng đợi review. |
| **Core action** | **Thẩm định và xác nhận quyết định xử lý cặp hồ sơ (Resolve Duplicate Candidate)**. |
| **Object** | Cặp hồ sơ khách hàng nghi trùng (`Candidate Pair` trong `Review Queue`). |
| **Preconditions** | Pipeline Blocking & Matching đã chạy xong, phân loại cặp hồ sơ vào vùng `REVIEW_NEEDED` ($0.55 \le \text{Score} < 0.85$). |
| **Completion rule** | Data Steward chọn `Approve Merge` hoặc `Reject / Cannot-Link`, hệ thống ghi nhận quyết định vào database và cập nhật đồ thị danh tính Golden Record thành công. |
| **Core value** | Làm sạch dữ liệu khách hàng, giải phóng hàng đợi, ngăn chặn tuyệt đối rủi ro gộp nhầm (False Merge). |
| **Evidence of value**| Cặp hồ sơ chuyển trạng thái `Resolved`, được đưa vào Golden Record và **không bị Revert / Unmerge trong 14 ngày tiếp theo**. |
| **Candidate event** | `candidate_pair_resolved` |

---

### 3. Tự kiểm 5 tiêu chí Core Action (Gate 1 Evaluation)

1. **Gần core value (5/5):** **ĐẠT.** Mỗi lần xác nhận quyết định là một cặp dữ liệu rác/phân mảnh được giải quyết, đưa hệ thống tiến trực tiếp tới trạng thái Single Customer View.
2. **Có thể lặp lại (5/5):** **ĐẠT.** Data Steward thực hiện lặp đi lặp lại hành động này cho từng ca nghi ngờ trong mỗi đợt đồng bộ dữ liệu.
3. **Có thể quan sát (5/5):** **ĐẠT.** Hệ thống bắt được chính xác thời điểm bản ghi quyết định được lưu thành công (`HTTP 200` từ API `/api/v1/candidates/{id}/decision`).
4. **Có ý nghĩa (5/5):** **ĐẠT.** Số lượng candidate được resolve chất lượng tăng lên đồng nghĩa với việc tồn đọng dữ liệu bẩn giảm xuống và mô hình học được nhiều case chuẩn hơn.
5. **Có thể tác động (5/5):** **ĐẠT.** Đội ngũ phát triển có thể nâng cấp giao diện so sánh bằng chứng (Evidence Card), đưa ra gợi ý giải thích từ AI Agent để Steward ra quyết định nhanh hơn và chính xác hơn.

> **ĐÁNH GIÁ GATE 1:** **ĐẠT (5/5 tiêu chí).** Core action có đầy đủ Actor, Object, Completion Rule rõ ràng, không lẫn với thao tác UI.

---

## 02 — Action Nature Card & Kết luận Cadence

### 1. Action Nature Card (8 thuộc tính bản chất)

| Thuộc tính | Phân tích bản chất hành vi trong P-149 |
| :--- | :--- |
| **Actor** | Data Steward (Người dùng nội bộ khối Data Management). |
| **Intent** | Giải quyết các ca dữ liệu nghi trùng cần sự can thiệp của con người để hoàn tất kỳ đối soát và làm sạch dữ liệu. |
| **Trigger** | Dữ liệu mới được nạp vào theo đợt từ các nguồn (Web, Mobile, Đối tác) kích hoạt pipeline so khớp định kỳ. |
| **Effort** | Mức độ trung bình: Cần 15 – 45 giây để quan sát họ tên, số điện thoại, địa chỉ, lịch sử giao dịch và lý do AI giải thích. |
| **Value timing** | Giá trị tích lũy: Càng xử lý nhiều ca thì chất lượng Master Data càng cao và tập nhãn huấn luyện cho AI Agent càng phong phú. |
| **State** | Dữ liệu được ghi nhận: Quyết định (Approve/Reject), Ghi chú lý do, Cập nhật trạng thái cụm Golden Record (Graph Edge / Cannot-link constraint). |
| **Dependency** | Phụ thuộc vào dữ liệu nguồn đầu vào (Batch Sync) và độ trễ xử lý của Pipeline Blocking/Matching. |
| **Repeat condition**| Nhu cầu lặp lại khi có mẻ dữ liệu mới cần thẩm định hoặc định kỳ hàng tuần trước khi chốt báo cáo Master Data. |

---

### 2. Kết luận Cadence (Template chuẩn)

> **"Đối với Data Steward, core action thẩm định và xác nhận quyết định xử lý cặp hồ sơ nghi trùng (Resolve Duplicate Candidate) thường xuất hiện theo chu kỳ hàng tuần (Weekly Batch Workflow) vì dữ liệu từ các hệ thống nguồn (Web, Mobile, Đối tác) được đồng bộ và xử lý khử trùng theo mẻ định kỳ hàng tuần phục vụ chốt kỳ đối soát dữ liệu doanh nghiệp. Do đó, nhịp đo phù hợp là Weekly ở cấp User (và Team Data Quality)."**

* **Lập luận phản biện bẫy Dashboard:**
  * Không chọn **Daily (DAU)** vì dữ liệu nguồn của doanh nghiệp không đổ về theo từng phút để Steward phải túc trực 24/7. Ép nhịp Daily sẽ tạo ra thói quen ảo hoặc khiến Steward mở app chỉ để "điểm danh".
  * Không chọn **Monthly** vì chu kỳ tháng quá dài, khiến tồn đọng hàng đợi quá lớn, làm tắc nghẽn các hệ thống CRM/Marketing hạ nguồn cần dùng Golden Record.

> **ĐÁNH GIÁ GATE 2:** **ĐẠT.** Cadence xuất phát từ bản chất quy trình nghiệp vụ (Nature), có lý do logic thuyết phục, không chạy theo thói quen đo lường DAU/MAU.

---

## 03 — Metric System

### 1. Activation Metric (Đầy đủ 3 thành phần)
* **Start event:** `steward_queue_assigned` (Lần đầu tiên Data Steward được phân công danh sách hồ sơ cần thẩm định).
* **Activation event:** `candidate_pair_resolved` (Thực hiện thành công việc thẩm định và đóng **5 ca đầu tiên** đạt chuẩn).
* **Time window:** Trong vòng **48 giờ** kể từ khi nhận phân công ca làm việc đầu tiên.
* *Lý do:* Không lấy việc "Đăng nhập" hay "Xem hết video giới thiệu" làm activation. Chỉ khi Steward tự tay thẩm định và đóng thành công 5 ca đầu tiên thì họ mới thực sự chạm tới giá trị của hệ thống.

---

### 2. Engagement Metric (Đo lường theo Depth & Frequency)
* **Frequency (Tần suất):** Số lượng ca `candidate_pair_resolved` trung bình trên mỗi Data Steward trong một tuần làm việc (Weekly Resolves per Steward).
* **Depth (Độ sâu / Chất lượng):** Tỉ lệ quyết định có kèm theo gắn nhãn bằng chứng / lý do nghiệp vụ (Evidence Attachment Rate $\ge 80\%$).

---

### 3. North Star Metric (NSM) — Đúng công thức chuẩn 3 thành phần

$$\text{NSM} = \text{Unit of Value} + \text{Quality Threshold} + \text{Frequency}$$

> **North Star Metric của P-149:**  
> **"Số lượng cặp hồ sơ nghi trùng được thẩm định thành công KHÔNG BỊ UNMERGE / HOÀN TÁC trong vòng 14 ngày, tính theo tuần (Weekly Confirmed Clean Resolves)."**

* **Unit of Value:** Cặp hồ sơ nghi trùng được giải quyết dứt điểm (`Resolved Candidate Pair`).
* **Quality Threshold:** Quyết định chính xác, tuyệt đối không bị người khác hoặc kiểm toán dữ liệu Unmerge/Khiếu nại sau 14 ngày (`No-unmerge within 14 days`).
* **Frequency:** Tính theo chu kỳ tuần (`Weekly`).
* *Tại sao NSM này không thể bị "game"?* Nếu Data Steward bấm bừa Approve để đạt chỉ tiêu số lượng, các ca gộp ẩu sẽ lập tức bị phát hiện và unmerge trong 14 ngày, khiến ca đó bị loại khỏi NSM và kích hoạt cảnh báo rủi ro.

---

### 4. Leading Indicators (Tối đa 3 chỉ số dự báo)
1. **Median Review Time per Candidate:** Thời gian trung vị để hoàn tất thẩm định 1 ca (Mục tiêu: giảm từ 60s xuống còn 25s nhờ AI gợi ý bằng chứng). Thời gian giảm chứng tỏ công cụ trực quan và hiệu quả.
2. **AI Recommendation Acceptance Rate:** Tỉ lệ Steward đồng thuận với nhãn dự đoán của AI Agent. Tỉ lệ này cao chứng tỏ AI ngày càng thông minh và bám sát nghiệp vụ thực tế.
3. **Queue Clearance Rate (Tỉ lệ giải phóng hàng đợi trong 3 ngày đầu tuần):** Dự báo mức độ gắn kết với luồng công việc định kỳ.

---

### 5. Counter-metric (Chống suy thoái chất lượng & chi phí)
1. **Unmerge / Revert Rate (% số ca gộp bị hủy sau đó):** Ngưỡng cảnh báo: $\le 1.0\%$. Nếu chỉ số này tăng, chứng tỏ Steward đang duyệt ẩu hoặc thuật toán AI đề xuất sai lệch, đe dọa trực tiếp tính toàn vẹn của Master Data.
2. **LLM Cost per Resolved Candidate:** Chi phí token LLM trung bình để giải thích và hỗ trợ giải quyết một ca không được vượt quá **$0.02 / ca**.

> **ĐÁNH GIÁ GATE 3:** **ĐẠT.** Activation có đủ start/activation event + time window; NSM chuẩn công thức 3 thành phần có rào chắn chất lượng; có Counter-metric chống False Merge rõ ràng.

---

## 04 — Retention Definition (Đầy đủ 6 thành phần)

Bảng định nghĩa Retention khớp chặt với nhịp Weekly của P-149:

| Thành phần | Định nghĩa chi tiết trong dự án P-149 |
| :--- | :--- |
| **1. Unit** | **Data Steward (User cá nhân)** |
| **2. Cohort entry** | Tuần đầu tiên Steward đạt chuẩn Activation (thực hiện $\ge 5$ ca `candidate_pair_resolved` thành công trong tuần W0). |
| **3. Return event** | Thực hiện hành động `candidate_pair_resolved` (Core action). |
| **4. Window** | **Weekly (W1, W2, W3, W4...)** — Hoàn toàn khớp với Cadence đã kết luận ở Phase 2. |
| **5. Threshold** | Đạt tối thiểu **$\ge 10$ ca thẩm định hoàn tất** trong tuần theo dõi. |
| **6. Segment** | Data Stewards phụ trách tập dữ liệu Master Customer khối VSF. |

* **Lập luận so sánh 3 mốc (S34 bài giảng):**
  * So với *Natural cycle:* Khớp hoàn toàn với chu kỳ chốt mẻ dữ liệu tuần của bộ phận IT/Data.
  * So với *Cohort segment:* Tách riêng nhóm Steward mới vào nghề và Steward lâu năm để đánh giá đường cong thích ứng.
  * So với *Category benchmark:* Các công cụ B2B Data Stewardship chuyên nghiệp yêu cầu tỷ lệ Weekly Retention duy trì ổn định $\ge 85\%$.

---

## 05 — Product Loop (2 chu kỳ & Metric Hypothesis)

### 1. Thiết kế Product Loop: HITL Data Flywheel Loop

Loop của P-149 không dựa vào Notification mà dựa trên **vòng lặp tiến độ công việc và tích lũy tri thức**:

```text
[CHU KỲ 1: Thẩm định thủ công & Tích lũy tri thức]
Trigger tự nhiên: Mẻ dữ liệu tuần mới nạp vào tạo ra hàng đợi Review Queue
  ➜ Core Action: Steward mở Evidence Card, thẩm định và xác nhận quyết định (Approve / Reject)
  ➜ Immediate Value: Ca nghi ngờ được đóng lại, giải phóng hàng đợi, Golden Record được cập nhật sạch
  ➜ Saved State / Investment: Quyết định của Steward cùng lý do được lưu vào DB làm nhãn chuẩn (Active Learning Feedback)

                              ⬇ (Tích lũy & Huấn luyện)

[CHU KỲ 2: AI Agent thông minh hơn & Hiệu suất tăng tốc]
Next Natural Trigger: Đợt dữ liệu tiếp theo đổ về, hệ thống tái huấn luyện / điều chỉnh rule từ nhãn tuần trước
  ➜ Core Action tiếp theo: Steward thẩm định các ca mới với độ chính xác gợi ý cao hơn, bằng chứng nổi bật rõ ràng hơn
  ➜ Repeat Value: Thời gian duyệt mỗi ca giảm 50%, tỉ lệ ca phải duyệt tay giảm (do AI tự tin Auto-Match), Steward hoàn thành chỉ tiêu nhẹ nhàng hơn
```

* **Reason to return ngoài Notification:**
  * Nhu cầu nội tại của Data Steward: Phải giải tỏa hàng tồn đọng trước kỳ chốt báo cáo để bảo đảm KPI vận hành.
  * Động lực tích lũy: Hệ thống lưu giữ lịch sử và uy tín thẩm định (Steward Audit Log), giúp giảm dần khối lượng việc thủ công qua từng tuần.

### 2. Metric Hypothesis (Câu giả thuyết bắt buộc)
> **"Nếu vòng lặp HITL Data Flywheel này hoạt động hiệu quả, metric Weekly Confirmed Clean Resolves sẽ tăng 30% và Median Review Time per Candidate sẽ giảm 40% trong vòng 4 tuần, vì mô hình AI Agent đã học được các mẫu gõ nhầm và quy luật số điện thoại từ các quyết định trước đó của Steward, giúp giảm bớt việc tra cứu thủ công."**

> **ĐÁNH GIÁ GATE 4:** **ĐẠT.** Loop có tối thiểu 2 chu kỳ, reason to return không dựa vào notification spam, có Metric Hypothesis định lượng trỏ thẳng về NSM ở Phase 3.

---

## 06 — Tracking Nhanh (5 Core Events & 2 Acceptance Criteria)

### 1. Bảng 5 Core Events chuẩn `object_action`

| Tên Event | Ý nghĩa (Hành vi/Value đại diện) | Thời điểm ghi nhận chính xác | Metric sử dụng ở Phase 3 |
| :--- | :--- | :--- | :--- |
| `steward_queue_assigned` | Phân công thành công một danh sách ca nghi ngờ cho Steward | Ngay khi Batch Queue được gán quyền cho Steward trong DB | Start event cho Activation |
| `candidate_pair_resolved` | Steward đã thẩm định và xác nhận xong quyết định xử lý một cặp | Khi API `/api/v1/candidates/{id}/decision` trả về HTTP 200 | Activation, Engagement, Retention, NSM |
| `golden_record_merged` | Cụm Golden Record được hợp nhất và cập nhật vào Master View | Ngay sau khi thuật toán Graph/DSU ghi nhận thành công | Depth Metric, Core Value Event |
| `cluster_unmerged` | Một hồ sơ bị tách ngược lại do phát hiện nhầm lẫn (False Merge) | Khi Lead Steward hoặc kiểm toán bấm xác nhận hủy gộp | **Counter-metric (Unmerge Rate)** |
| `agent_recommendation_viewed`| Steward mở xem chi tiết bằng chứng và lý do AI giải thích | Khi component `EvidenceCard` được render trên viewport màn hình | Leading Indicator (AI Assist Rate) |

---

### 2. Hai tiêu chí nghiệm thu (Acceptance Criteria)

* **Tiêu chí 1 (Chỉ bắn event khi nghiệp vụ hoàn tất thực tế):**
  * Với mỗi cặp `pair_id`, hệ thống chỉ được bắn event `candidate_pair_resolved` khi backend đã xử lý xong và commit giao dịch vào cơ sở dữ liệu (HTTP 200). 
  * *Ràng buộc:* Hành động click nút trên giao diện của Steward tuyệt đối không được kích hoạt event tracking nếu API chưa trả về thành công.

* **Tiêu chí 2 (Chống trùng lặp do reload / retry / mạng lag):**
  * Mỗi hành động thẩm định phải đi kèm một mã định danh duy nhất (`idempotency_key = steward_id + pair_id + decision_timestamp`). 
  * Nếu người dùng bấm liên tục nhiều lần do mạng chậm hoặc F5 tải lại trang, hệ thống backend và analytics engine chỉ được ghi nhận duy nhất 1 event `candidate_pair_resolved`. Các lần bấm lặp phải bị khử trùng lặp (deduplicated).

> **ĐÁNH GIÁ GATE 5:** **ĐẠT.** Tất cả các event đều map về chỉ số ở Phase 3, có 2 tiêu chí nghiệm thu kỹ thuật cụ thể chống bẫy ghi nhận sai lệch.

---

## 07 — Bản tự kiểm 7 câu hỏi kinh điển trước khi nộp

1. *Core action không phải thao tác giao diện hay output hệ thống?* 👉 **ĐÚNG.** Là hành vi thẩm định và ra quyết định xử lý cặp hồ sơ (`candidate_pair_resolved`).
2. *Activation không phải "xem hết hướng dẫn" hay "đăng nhập"?* 👉 **ĐÚNG.** Là hoàn tất 5 ca thẩm định thực tế đầu tiên trong vòng 48h.
3. *Frequency không cao hơn nhu cầu thật?* 👉 **ĐÚNG.** Đo theo nhịp Weekly khớp mẻ dữ liệu doanh nghiệp, không ép nhịp Daily.
4. *Loop có reason to return ngoài notification?* 👉 **ĐÚNG.** Dựa vào áp lực chốt kỳ dữ liệu và giá trị tích lũy AI thông minh hơn qua từng tuần.
5. *Retention không dùng chung một window cho mọi cadence?* 👉 **ĐÚNG.** Sử dụng khung đo Weekly (W1, W2, W3...).
6. *Mọi event đều map về một metric?* 👉 **ĐÚNG.** Cả 5 event đều gắn trực tiếp với Activation, Retention, NSM, Leading hoặc Counter-metric.
7. *Metric nào cũng có event để tính nó?* 👉 **ĐÚNG.** Tất cả chỉ số đều có nguồn event cụ thể tương ứng.
