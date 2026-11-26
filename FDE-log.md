# FDE Log · Roadmap 30 ngày

Theo `ROADMAP.md`. Nhật ký cũ (70 ngày, format cũ) lưu tại `FDE-log-cu.md`.

**Tiến độ:** 0/30 ngày

---

## Ngày 1 · Phân loại ticket CSKH

**Tình huống:** team CSKH nhận hàng trăm ticket mỗi ngày, cần gán nhóm và độ khẩn cấp tự động.

**Prompt sai:**
```
Phân loại ticket này: "Tôi bị trừ tiền 2 lần cho đơn #8812, làm ơn xử lý gấp"
```

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

**Input test:** 10 ticket trong `ROADMAP.md` (Ngày 1), chạy mỗi ticket một chat mới.

**Đạt khi:** JSON hợp lệ ≥ 9/10, category ≥ 9/10, urgent ≥ 9/10.

### Kết quả
_(chưa chạy)_

### Ghi chú cuối ngày
1. Prompt sai hỏng ở đâu?
2. Prompt đúng còn lỗi gì, ticket nào?
3. Lần sau đổi gì?
