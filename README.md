# Moni Tài Chính — AI Spending Insight Prototype

> **Track D — Personal Finance** | Product/app: MoMo

## Problem

User trẻ dùng MoMo gặp khó khi xem lại lịch sử giao dịch: các khoản hiển thị rời rạc, có khoản chưa phân loại hoặc phân loại sai, dẫn tới không hiểu vì sao chi tiêu tăng, khoản nào cần kiểm tra, và khoản nào nên nhắc thanh toán.

## Solution

**Moni** — AI action layer trên màn hình Lịch sử giao dịch của MoMo, giúp user:
1. **Phân loại chi tiêu** — tự động gán category, hỏi lại khi không chắc
2. **Phát hiện giao dịch lạ** — cảnh báo khoản cần kiểm tra, không khẳng định tuyệt đối
3. **Giải thích phí/giao dịch** — mô tả khoản phí có thể đến từ đâu
4. **Nhắc thanh toán** — phát hiện giao dịch định kỳ và gợi ý tạo reminder

## Four Paths

| Path | Thể hiện trong prototype |
|---|---|
| ✅ Happy | "Phân tích tháng này" → Moni trả lời đúng tổng chi, khoản lớn nhất, giao dịch cần kiểm tra |
| ⚠️ Low-confidence | Moni không chắc giao dịch "Nguyễn Đông Anh" → hỏi user chọn category |
| ❌ Failure | Moni phân loại "Thanh toán Google" là Giải trí (có thể sai → thực chất là công cụ học tập) |
| 🔄 Correction | User sửa category Google → Moni cập nhật báo cáo + ghi nhớ cho lần sau |

## Auto/Aug Decision

**Augmentation** — AI chỉ gợi ý, phân loại nháp, giải thích, cảnh báo. User là người quyết cuối. AI KHÔNG tự thanh toán, tự kết luận gian lận, hoặc tự thay đổi dữ liệu.

## Team Roles

| Thành viên | Vai trò |
|---|---|
| Thân Văn Hoàng | Research / Evidence |
| Phạm Quang Dũng | Test / Failure Path |
| Đặng Trần Đạt | SPEC & Prototype |
| Hoàng Anh Thư | Demo script / Repo |
