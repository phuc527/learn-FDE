# FDE Log

Nhật ký học FDE hằng ngày. Mỗi ngày một mục, cùng format.

---

## DAY 1 · Prompt phân loại ticket CSKH

**Tình huống:** team CSKH nhận hàng trăm ticket mỗi ngày, cần gán nhóm và độ khẩn cấp tự động.

**Công cụ:** claude.ai (chat mới cho mỗi lần thử)

**Thời lượng:** 60–90 phút

### 1. Prompt sai

```
Phân loại ticket này: "Tôi bị trừ tiền 2 lần cho đơn #8812, làm ơn xử lý gấp"
```

**Output thực tế:** _(điền)_

### 2. Prompt đã sửa

```
Bạn là hệ thống phân loại ticket. Chỉ trả về JSON đúng schema, không giải thích.
Schema: {"category": "billing|bug|shipping|other", "urgent": true|false}

Ví dụ:
- "Bị trừ tiền 2 lần" -> {"category": "billing", "urgent": true}
- "App crash khi mở màn hình thanh toán" -> {"category": "bug", "urgent": true}
- "Muốn đổi địa chỉ giao hàng" -> {"category": "shipping", "urgent": false}

Ticket: "{ticket}"
```

### 3. Test 10 ticket khó

| # | Ticket | Nhãn đúng | JSON hợp lệ | Category đúng | Urgent đúng |
|---|---|---|---|---|---|
| 1 | Đặt hàng rồi mà app báo lỗi, tiền vẫn bị trừ | billing, true | | | |
| 2 | Đơn chưa tới, mà hôm qua shop nói đã giao | shipping, true | | | |
| 3 | Nút thanh toán không bấm được | bug, true | | | |
| 4 | cho hỏi phí ship về Đà Nẵng bao nhiêu | shipping, false | | | |
| 5 | Tôi muốn hủy đơn | other, false | | | |
| 6 | app hay bị đơ, lúc nào cũng vậy | bug, false | | | |
| 7 | Tài khoản bị khóa, không đăng nhập được, gấp quá | other, true | | | |
| 8 | Mã giảm giá không dùng được | billing, false | | | |
| 9 | Hàng bị hư, muốn đổi trả | shipping, false | | | |
| 10 | Cảm ơn shop, hàng rất ổn | other, false | | | |

### 4. Kết quả

- JSON hợp lệ: ___/10
- Category đúng: ___/10
- Urgent đúng: ___/10

### 5. Ghi chú cuối ngày

1. Prompt sai hỏng ở đâu?
2. Prompt đã sửa còn lỗi gì? Ticket nào sai?
3. Lần sau sẽ đổi gì trong prompt?

**Mục tiêu:** JSON hợp lệ ≥ 9/10, có ít nhất 1 ticket sai và giải thích được vì sao.

---

## DAY 2

_(chưa làm)_
