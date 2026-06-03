# Moni Tài Chính — AI Spending Insight Prototype

> **Track D — Personal Finance** | Product/app: MoMo  
> ⚠️ Đây là prototype học thuật, KHÔNG phải sản phẩm chính thức của MoMo. Không sử dụng dữ liệu thật, không kết nối API tài chính thật.

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

## How to Run

```bash
cd 02-group-spec/moni-prototype
npm install
npm run dev
```

Mở `http://localhost:5173` trên trình duyệt.

## Prototype Flow

```
Home MoMo → Lịch sử giao dịch → Hỏi Moni
  ├── Phân tích tháng này (Happy)
  ├── Phân loại chi tiêu (Low-confidence)
  ├── Tìm giao dịch lạ
  ├── Giải thích khoản Google (Failure → Correction)
  └── Nhắc thanh toán Viettel (Reminder)
```

## Folder Structure

```
moni-prototype/
├── docs/
│   ├── thin-spec.md
│   ├── evidence-pack.md
│   ├── ux-test.md
│   ├── demo-script.md
│   └── individual-reflection-template.md
├── src/
│   ├── data/transactions.ts
│   ├── components/
│   │   ├── PhoneFrame.tsx
│   │   ├── HomeScreen.tsx
│   │   ├── TransactionScreen.tsx
│   │   ├── MoniChat.tsx
│   │   ├── InsightCard.tsx
│   │   ├── TransactionItem.tsx
│   │   └── BottomNav.tsx
│   ├── App.tsx
│   └── App.css
└── README.md
```

## Auto/Aug Decision

**Augmentation** — AI chỉ gợi ý, phân loại nháp, giải thích, cảnh báo. User là người quyết cuối. AI KHÔNG tự thanh toán, tự kết luận gian lận, hoặc tự thay đổi dữ liệu.

## Team Roles

| Thành viên | Vai trò |
|---|---|
| Thân Văn Hoàng | Research / Evidence |
| Phạm Quang Dũng | Test / Failure Path |
| Đặng Trần Đạt | SPEC & Prototype |
| Hoàng Anh Thư | Demo script / Repo |

## Disclaimer

> Đây là prototype phục vụ học tập trong khuôn khổ khóa AI Product Labs. Không phải sản phẩm chính thức của MoMo. Không kết nối tài khoản thật, không dùng dữ liệu thật, không gọi API tài chính thật. Mọi dữ liệu giao dịch đều là mock data mô phỏng.
