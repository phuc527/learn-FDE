# FDE Log

Nhật ký học FDE hằng ngày. Mỗi ngày một mục, cùng format.

---

## DAY 1 · Phân loại ticket CSKH

**Tình huống:** team CSKH nhận hàng trăm ticket mỗi ngày, cần gán nhóm và độ khẩn cấp tự động.

**Prompt sai:**
```
Phân loại ticket này: "Tôi bị trừ tiền 2 lần cho đơn #8812, làm ơn xử lý gấp"
```

**Vì sao sai:** không có schema nên output là văn xuôi, không parse được. Không định nghĩa nhóm hợp lệ, model có thể tự tạo nhóm mới.

**Prompt đúng:**
```
Bạn là hệ thống phân loại ticket. Chỉ trả về JSON đúng schema, không giải thích.
Schema: {"category": "billing|bug|shipping|other", "urgent": true|false}

Ví dụ:
- "Bị trừ tiền 2 lần" -> {"category": "billing", "urgent": true}
- "App crash khi mở màn hình thanh toán" -> {"category": "bug", "urgent": true}
- "Muốn đổi địa chỉ giao hàng" -> {"category": "shipping", "urgent": false}

Ticket: "{ticket}"
```

**Lưu ý:** luôn định nghĩa giá trị hợp lệ cho từng trường. Nếu không, model tự đặt tên nhóm mới.

**Kiểm tra:** chạy 10 ticket, đếm JSON hợp lệ, category đúng, urgent đúng.

### Kết quả (chạy trên claude.ai, mỗi ticket một chat mới)

| # | Ticket | Nhãn đúng | Output | JSON hợp lệ | Category đúng | Urgent đúng |
|---|---|---|---|---|---|---|
| 1 | Đặt hàng rồi mà app báo lỗi, tiền vẫn bị trừ | billing, true | billing, true | Có | Có | Có |
| 2 | Đơn chưa tới, mà hôm qua shop nói đã giao | shipping, true | shipping, false | Có | Có | Không |
| 3 | Nút thanh toán không bấm được | bug, true | bug, true | Có | Có | Có |
| 4 | cho hỏi phí ship về Đà Nẵng bao nhiêu | shipping, false | shipping, false | Có | Có | Có |
| 5 | Tôi muốn hủy đơn | other, false | other, false | Có | Có | Có |
| 6 | app hay bị đơ, lúc nào cũng vậy | bug, false | bug, false | Có | Có | Có |
| 7 | Tài khoản bị khóa, không đăng nhập được, gấp quá | other, true | other, true | Có | Có | Có |
| 8 | Mã giảm giá không dùng được | billing, false | billing, false | Có | Có | Có |
| 9 | Hàng bị hư, muốn đổi trả | shipping, false | shipping, false | Có | Có | Có |
| 10 | Cảm ơn shop, hàng rất ổn | other, false | other, false | Có | Có | Có |

**Điểm:** JSON hợp lệ 10/10 · Category đúng 10/10 · Urgent đúng 9/10 · Mục tiêu (≥ 9/10 JSON hợp lệ) đạt.

### Ghi chú cuối ngày

1. **Prompt sai hỏng ở đâu?** Chưa chạy prompt sai trên claude.ai, nên chưa có output thật để so sánh. Lỗi dự kiến là văn xuôi, không parse được.
2. **Prompt đúng còn lỗi gì?** Ticket 2 ("đơn chưa tới, hôm qua shop nói đã giao") bị gán urgent=false, nhãn đúng là true. Có thể model coi đây là câu hỏi thường, không phải sự cố cần xử lý gấp. Lần sau nên thêm ví dụ kiểu "đơn chưa nhận được, khách đang bực" có urgent=true.
3. **Lần sau đổi gì?** Định nghĩa rõ urgent là "cần xử lý trong ngày" và thêm 1 ví dụ shipping với urgent=true để model không mặc định urgent=false cho mọi câu hỏi về giao hàng.

---

## DAY 2 · Trích xuất đơn hàng từ email

**Tình huống:** email khách hàng chứa thông tin đơn hàng, cần đưa vào hệ thống dưới dạng JSON.

**Prompt sai:** `Lấy thông tin đơn hàng từ email này`

**Vì sao sai:** không nói rõ trường nào cần lấy, không nói khi thiếu thì làm gì. Model có thể tự bịa.

**Prompt đúng:** schema cố định, quy tắc `null` khi thiếu, định dạng ngày `YYYY-MM-DD`, số tiền là integer (xem prompt đầy đủ trong bản ghi chạy ngày 2).

### Kết quả (claude.ai)

| Email | JSON hợp lệ | Không bịa trường | Đúng số tiền | Đúng item và số lượng |
|---|---|---|---|---|
| A (đủ thông tin) | Có | Có | Có | Có |
| B (thiếu thông tin) | Có | Có | — | — |
| C (2 đơn, lộn xộn) | Có | Có | Không (null, thiếu 350.000 và 4.200.000) | Một phần (2 item nhưng gộp thành 1 đơn, mất mã 8812 và 8813) |

**Điểm:** email A và B đạt đầy đủ. Email C lỗi cấu trúc: schema chỉ có một object nên model gộp 2 đơn thành 1, mất `order_id` và `total_vnd`.

### Ghi chú cuối ngày

1. **Prompt sai hỏng ở đâu?** Chưa chạy prompt sai trên claude.ai. Dự kiến lỗi là output văn xuôi, không có cấu trúc.
2. **Model bịa dữ liệu hoặc sai số tiền?** Không bịa ở email A và B. Ở email C, model không bịa nhưng bỏ mất dữ liệu: 2 mã đơn và 2 số tiền.
3. **Lần sau đổi gì khi gặp nhiều đơn?** Đổi schema thành `{"orders": [{order_id, items, total_vnd}], "customer_name": ...}`. Thêm quy tắc: "mỗi đơn hàng là một phần tử trong orders, không gộp". Cần thêm ví dụ email nhiều đơn vào prompt.

### Chạy lại email C với schema `orders` (claude.ai)

| Kiểm tra | Kết quả |
|---|---|
| Tách đúng 2 đơn | Có (8812, 8813) |
| Item và số lượng từng đơn | Đúng (chuột x1; màn hình x1) |
| total_vnd là integer | Đúng (350000; 4200000) |
| customer_name | Đúng ("Trần Hải") |

**Kết luận:** đạt. Lỗi của email C nằm ở schema một đơn, không phải ở model. Sau khi đổi sang mảng `orders`, model trích xuất đúng cả 2 đơn.

**Bài học FDE:** khi dữ liệu đầu vào có thể có nhiều bản ghi, schema phải là mảng ngay từ đầu. Đừng thiết kế schema cho trường hợp đẹp nhất.

---

## DAY 3 · Trả lời khách từ chính sách

_(chưa làm)_

**Tình huống:** khách hỏi về đổi trả, cần trả lời dựa trên chính sách nội bộ.

**Prompt sai:** `Trả lời khách hàng hỏi về đổi trả`, kèm chính sách.

**Vì sao sai:** không ràng buộc phạm vi trả lời, không yêu cầu nói "không biết" khi thiếu thông tin.
