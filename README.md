# Lab22 — DATA-17 Monetization và GTM

**Trần Đình Hinh · MHV2A202602399 · P-149 · 08/10/2026**

DATA-17 hỗ trợ Data Steward thẩm định và xử lý hồ sơ khách hàng nghi trùng từ Web, Mobile App và Referral. Bài làm bám **metrics-pack.md** và yêu cầu **HD_Lab22.md**.

## Bộ bài nộp

1. [Excel Monetization Model](outputs/data17/TranDinhHinh_Day22_model.xlsx):5 tab làm việc hoàn chỉnh cùng2 tab tham chiếu; Cost/Job, pricing, Hybrid reconciliation, Value Metric, CAC, channel và90-day plan.
2. [Monetization One-Pager PDF](outputs/data17/TranDinhHinh_Day22_onepager.pdf): đúng1 trang, có ô Excel tham chiếu cho các số.
3. [Phân tích và phản biện](outputs/data17/DATA17_GiaiTrinh_Va_PhanBien.md): định nghĩa job, giả định, kiểm toán cost, sensitivity,2 prompt/accept-reject và đối chiếu yêu cầu.
4. [Evidence Pack](outputs/data17/DATA17_Evidence_Pack.md): Eval plan, Procurement Q&A draft, Pilot Report template.
5. [One-Pager sửa được](outputs/data17/TranDinhHinh_Day22_onepager.md): nội dung Markdown tương ứng PDF.

Hai file bắt buộc theo hướng dẫn là Excel và PDF. Các fileMarkdown bổ sung giải thích và kế hoạch kiểm chứng.

## Trạng thái số liệu

Metrics pack là thiết kế/mục tiêu, chưa có raw eval hoặc pilot. Kịch bản100k attempts/tháng, clean auto-containment80% và economics là **dự toán tự động hóa sau pilot**, không phải kết quả thực đo. Ca vùng xám hiện vẫn do Steward thẩm định; pilot trước kiểm chứng dùng phí cố định theo phạm vi. Không tự nhận đã có khách hoặc bán Outcome.

Base scenario: Cost/job$0.010302; usage$0.04; base$1,000/tháng; GMusage74.24%; GMHybrid80.38%; còn$375.83 sau overhead giả định. Xem giải trình trước khi dùng các số để quyết định thương mại.

Giá API được mở kiểm tra08/10/2026 từ [Google Gemini](https://ai.google.dev/gemini-api/docs/pricing); benchmark metric từ [Tamr](https://www.tamr.com/pricing) và [AWS Entity Resolution](https://aws.amazon.com/entity-resolution/pricing/). Nguồn/giả định ghi trong Excel, Benchmarks hàng45–63.

## Nộp bài

Repo theo yêu cầu: **Track1_Day22_2A202602399_TranDinhHinh**. Đưa link repo chứa bộ bài lên Vlearn trước buổi tiếp theo. Chưa gửi Vlearn hoặc push remote trong phiên này.

Test người lạ cần người đọc độc lập đọcPDF2 phút và ghi số câu hỏi≤3; chưa thực hiện, giữ trạng thái chưa kiểm tra trong Tab5. Eval, pilot và security signoff cũng là việc triển khai tiếp theo, không được bịa thành đã hoàn thành.

Các mẫu đầu vào ở thư mục gốc giữ nguyên. File có sẵn trước phiên nằm ở bản nháp/đầu vào; dùng các bản trong **outputs/data17/** được liên kết phía trên để nộp. PDF là bản One-Pager đã kiểm tra; không dùng DOCX cũ chưa render.
