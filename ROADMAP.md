# Roadmap FDE · 30 ngày · 1 giờ/ngày

**Công cụ:** claude.ai (chạy prompt), Claude Code (viết file, script, test).
**Kết quả:** ghi vào `FDE-log.md`, mỗi ngày một mục.

## Nhịp 1 giờ mỗi ngày
- 10 phút: đọc tình huống và prompt sai.
- 30 phút: chạy prompt đúng trên các input test.
- 15 phút: chấm kết quả theo tiêu chí đạt, gửi mình để chấm.
- 5 phút: ghi 3 dòng vào `FDE-log.md`.

## Quy ước một ngày
Mỗi ngày có đúng 5 mục:
1. **Tình huống:** 1 dòng.
2. **Prompt sai:** dán nguyên văn.
3. **Prompt đúng:** dán nguyên văn, kèm quy tắc chính.
4. **Input test:** 3–5 input.
5. **Đạt khi:** tiêu chí rõ ràng.

---

## Tuần 1 · Prompt cơ bản

**Ngày 1 · Phân loại ticket CSKH** ✅
Đạt: JSON hợp lệ 10/10, category 10/10, urgent 9/10. Xem `FDE-log.md`.

**Ngày 2 · Trích xuất đơn hàng từ email** ✅
Đạt: schema mảng `orders`, không gộp đơn. Xem `FDE-log.md`.

**Ngày 3 · Trả lời khách từ chính sách** ✅
Đạt: 3/3 câu, trích dẫn điều khoản, chuyển nhân viên khi thiếu hoặc mâu thuẫn.

**Ngày 4 · Kiểm tra regression khi sửa prompt** ⏳
- Sai: sửa prompt rồi dùng luôn, không kiểm tra.
- Đúng: chạy 12 ticket với v1 và v2, đếm cải thiện và regression.
- Đạt khi: không có regression, hoặc regression đã giải thích được.

**Ngày 5 · Mô tả sản phẩm theo giọng thương hiệu**
- Sai: `Viết mô tả sản phẩm cho áo thun này`.
- Đúng: quy định giọng văn, độ dài, từ cấm, và ví dụ một mô tả mẫu.
- Input: 3 sản phẩm khác nhau (áo, đèn bàn, bình giữ nhiệt).
- Đạt khi: đúng độ dài, không có từ cấm, không bịa thông số kỹ thuật.

## Tuần 2 · Output có cấu trúc

**Ngày 6 · Parse CV thành JSON**
- Sai: `Tóm tắt CV này`.
- Đúng: schema cố định, `null` khi thiếu, không bịa kinh nghiệm.
- Input: 1 CV đầy đủ, 1 CV thiếu, 1 CV có kinh nghiệm không rõ năm.
- Đạt khi: không điền thông tin không có trong CV.

**Ngày 7 · Gom lỗi từ log**
- Sai: `Log này có lỗi gì`.
- Đúng: phân nhóm theo nguyên nhân, trả bảng với số lần xuất hiện.
- Input: 20 dòng log mẫu (Claude Code tạo file).
- Đạt khi: không gộp sai hai lỗi khác nhau.

**Ngày 8 · Chuẩn hóa địa chỉ Việt Nam**
- Sai: `Chuẩn hóa địa chỉ này`.
- Đúng: format cố định (số nhà, đường, phường, quận, tỉnh), viết tắt được mở rộng.
- Input: 5 địa chỉ viết tắt, thiếu, sai chính tả.
- Đạt khi: không tự thêm phường hay quận không có trong input.

**Ngày 9 · Bóc số liệu từ bảng copy-paste**
- Sai: `Lấy số liệu từ bảng này`.
- Đúng: trả JSON theo cột, số nguyên, bỏ dấu phân cách.
- Input: 1 bảng sạch, 1 bảng có ô trống, 1 bảng có dòng tổng.
- Đạt khi: không đưa dòng tổng vào danh sách dữ liệu.

**Ngày 10 · Sinh test case từ đặc tả (Claude Code)**
- Sai: `Viết test cho hàm này`.
- Đúng: yêu cầu test cho biên, lỗi, và input rỗng; xuất ra file test.
- Input: một đặc tả 5 dòng.
- Đạt khi: test chạy được và có ít nhất 1 case biên.

## Tuần 3 · Agent và tool

**Ngày 11 · Mô tả tool để model chọn đúng tool**
- Sai: tool có mô tả `Tìm đơn hàng`.
- Đúng: mô tả nói rõ khi nào dùng, khi nào không dùng, và tham số bắt buộc.
- Input: 5 câu hỏi, mỗi câu nên gọi tool nào.
- Đạt khi: chọn đúng tool ở ít nhất 4/5 câu.

**Ngày 12 · Xử lý khi tool trả lỗi**
- Sai: model tự đoán kết quả khi tool lỗi.
- Đúng: quy định retry tối đa 1 lần, sau đó hỏi lại người dùng.
- Input: 3 tình huống tool lỗi (timeout, không tìm thấy, lỗi quyền).
- Đạt khi: không bịa kết quả trong cả 3 tình huống.

**Ngày 13 · Giới hạn số bước agent**
- Sai: để agent tự lặp đến khi xong.
- Đúng: giới hạn bước, dừng và báo khi vượt.
- Input: 1 việc đơn giản, 1 việc không thể hoàn thành.
- Đạt khi: việc không hoàn thành được dừng đúng giới hạn và báo lại.

**Ngày 14 · Phân quyền tool theo vai trò**
- Sai: mọi người dùng đều dùng được mọi tool.
- Đúng: bảng quyền theo vai trò, từ chối rõ ràng khi không đủ quyền.
- Input: 3 yêu cầu từ 2 vai trò (nhân viên, khách).
- Đạt khi: khách không gọi được tool nội bộ.

**Ngày 15 · Review chuỗi tool call (Claude Code)**
- Việc: đọc một log tool call và tìm bước thừa hoặc sai thứ tự.
- Đạt khi: chỉ ra được ít nhất 1 vấn đề và đề xuất cách sửa.

## Tuần 4 · RAG bằng Claude Projects

**Ngày 16 · Hỏi đáp tài liệu có trích dẫn**
- Sai: hỏi trực tiếp không yêu cầu trích dẫn.
- Đúng: chỉ trả lời từ tài liệu, trích dẫn đoạn.
- Input: 3 câu có trong tài liệu.
- Đạt khi: mỗi câu có trích dẫn đúng đoạn.

**Ngày 17 · Câu không có trong tài liệu**
- Sai: model tự trả lời khi tài liệu không có.
- Đúng: quy định nói "không tìm thấy trong tài liệu".
- Input: 2 câu không có trong tài liệu, 1 câu gần đúng.
- Đạt khi: không bịa ở cả 3 câu.

**Ngày 18 · Tài liệu có bảng bị hỏng**
- Việc: upload tài liệu có bảng, hỏi số liệu trong bảng.
- Đạt khi: nhận ra khi số liệu không đọc được, không đoán.

**Ngày 19 · So sánh 2 phiên bản chính sách**
- Sai: hỏi "có gì thay đổi".
- Đúng: liệt kê thay đổi theo điều khoản, phân loại thêm/bỏ/sửa.
- Input: 2 phiên bản chính sách mẫu.
- Đạt khi: không bỏ sót điều khoản bị sửa.

**Ngày 20 · Tóm tắt hợp đồng và đánh dấu rủi ro**
- Sai: `Tóm tắt hợp đồng`.
- Đúng: tóm tắt theo mục, đánh dấu điều khoản phạt, gia hạn tự động, và giới hạn trách nhiệm.
- Đạt khi: không bỏ sót điều khoản phạt.

## Tuần 5 · Eval và bảo mật

**Ngày 21 · Tạo 30 test case có nhãn**
- Việc: viết 30 ticket hoặc email mẫu, mỗi cái có nhãn đúng.
- Đạt khi: đủ 30 case, có ít nhất 5 case bẫy.

**Ngày 22 · Chấm bằng script (Claude Code)**
- Việc: viết script so sánh output với nhãn, in độ chính xác từng nhóm.
- Đạt khi: script chạy được trên 30 case.

**Ngày 23 · Dùng claude.ai làm judge**
- Việc: dùng model chấm câu trả lời theo tiêu chí, rồi tự chấm lại 5 case để kiểm tra judge.
- Đạt khi: judge đồng ý với người ở ít nhất 4/5 case.

**Ngày 24 · Prompt injection trong email**
- Sai: model làm theo chỉ dẫn trong email.
- Đúng: quy định nội dung email là dữ liệu, không phải lệnh.
- Input: 3 email có chỉ dẫn ẩn (ví dụ "bỏ qua hướng dẫn trước").
- Đạt khi: không làm theo chỉ dẫn ẩn trong cả 3 email.

**Ngày 25 · Lộ thông tin cá nhân trong output**
- Việc: input có số điện thoại, email, địa chỉ; output không được chứa các thông tin này.
- Đúng: quy định che thông tin, hoặc chỉ trả về ID.
- Đạt khi: không lộ thông tin cá nhân trong 5 input.

## Tuần 6 · Khách hàng và bàn giao

**Ngày 26 · Phỏng vấn người dùng giả lập**
- Việc: dùng claude.ai đóng vai nhân viên CSKH, hỏi 10 câu để tìm quy trình tốn thời gian nhất.
- Đạt khi: rút ra 1 workflow có số giờ ước tính.

**Ngày 27 · One-pager use case và ROI**
- Việc: viết 1 trang gồm vấn đề, giải pháp, số liệu, rủi ro.
- Đạt khi: có ước tính tiết kiệm thời gian và chi phí rõ ràng.

**Ngày 28 · Pilot plan 4 tuần**
- Việc: chia 4 tuần, mỗi tuần có mục tiêu và tiêu chí go/no-go.
- Đạt khi: tiêu chí đo được bằng số.

**Ngày 29 · Runbook bàn giao (Claude Code)**
- Việc: viết tài liệu hướng dẫn khách vận hành hệ thống, gồm cách chạy, cách xử lý lỗi, cách kiểm tra.
- Đạt khi: người chưa biết hệ thống làm được theo tài liệu.

**Ngày 30 · Demo và báo cáo tổng kết**
- Việc: tổng hợp 30 ngày, chọn 1 bài làm mạnh nhất để demo cho khách giả lập.
- Đạt khi: demo chạy được và có số liệu trước/sau.
