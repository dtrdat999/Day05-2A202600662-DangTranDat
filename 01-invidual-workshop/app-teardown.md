# Workshop — Mổ App AI Thật

**Họ và tên:** Đặng Trần Đạt  
**Mã học viên:** 2A202600662  

**Thời gian:** 35-45 phút  
**Hình thức:** cá nhân trước, chia sẻ theo nhóm sau  
**Output:** finding note + sketch `as-is / to-be`

Mục tiêu không phải chấm "UI đẹp hay xấu". Mục tiêu là dùng sản phẩm thật như một bài needfinding: tìm chỗ product gãy trong workflow thật, rồi viết finding đó thành quyết định product.

---

## 1. Chọn một sản phẩm để dùng thử

| Sản phẩm | AI feature | Cách truy cập |
|---|---|---|
| MoMo — Moni | Trợ thủ tài chính, phân tích chi tiêu, chatbot | App MoMo |
| Vietnam Airlines — NEO | Chatbot hỗ trợ vé, hành lý, khiếu nại | Website/Zalo VNA |
| **V-App — V-AI** ✅ | Trợ lý voice/text, gợi ý theo ngữ cảnh | App V-App |

**Sản phẩm được chọn: V-App — V-AI**

---

## 2. Dùng thử: promise vs reality

- **Product hứa gì?**  
  V-AI đóng vai trò trợ lý ảo thông minh, có khả năng trả lời câu hỏi tự nhiên, hỗ trợ điều hướng và cung cấp thông tin theo ngữ cảnh người dùng ngay trong hệ sinh thái V-App.

- **User nào được hứa sẽ được giúp?**  
  Người dùng V-App muốn tra cứu thông tin nhanh và hỏi đáp tự nhiên như nói chuyện với một trợ lý hiểu ngữ cảnh — thay vì phải tìm kiếm thủ công.

- **Bạn kỳ vọng AI làm được task nào?**  
  Khi hỏi *"Mối quan hệ của Phạm Nhật Vượng và Elon Musk"*, kỳ vọng AI sẽ:
  - Hiểu đây là câu hỏi **quan hệ** giữa hai người (relational query), không phải tra profile một người.
  - Trả lời đúng: hai người không có quan hệ thân tộc, chỉ là hai tỷ phú hoạt động trong lĩnh vực khác nhau.
  - Ở lượt follow-up *"Vậy Elon Musk - có phải anh của ông Phạm Nhật Vượng không?"* — AI phải nhận ra tiền đề **sai hoàn toàn** và phủ nhận rõ ràng thay vì tiếp tục trả lời như bình thường.

- **Khi dùng thật, điểm gãy xuất hiện ở đâu?**  
  V-AI trả về **cùng một đoạn văn boilerplate** mô tả ông Phạm Nhật Vượng và Vingroup cho **cả hai câu hỏi** hoàn toàn khác nhau. Kể cả khi user hỏi một câu phi lý ("Elon Musk có phải anh của ông Phạm Nhật Vượng không?"), bot không nhận diện intent, không phủ nhận sai lầm, không điều chỉnh theo context câu trước — chỉ copy-paste lại y nguyên kết quả cũ.

**Evidence:**

- [x] Screenshot đã chụp: 2 lượt chat liên tiếp, cùng một response boilerplate, không có context carry-over.
- **Prompt/input đã thử:**
  1. `"Mối quan hệ của Phạm Nhật Vượng và Elon Musk"`
  2. `"Vậy Elon Musk - có phải anh của ông Phạm Nhật Vượng không"`
- **Hành vi quan sát được:** Cả 2 câu hỏi nhận về cùng đoạn text: *"Ông Phạm Nhật Vượng là nhà sáng lập kiêm Chủ tịch Tập đoàn Vingroup…"* — không hỏi lại, không phủ nhận tiền đề sai, không có nút sửa.

---

## 3. Vẽ 4 paths

| Path | Câu hỏi cần trả lời | Quan sát thực tế trên V-AI |
|---|---|---|
| **Happy** | Khi AI đúng và tự tin, user thấy gì? | ✅ Tồn tại với các câu hỏi profile đơn giản (ví dụ "Phạm Nhật Vượng là ai?") — bot trả về thông tin cơ bản đúng. |
| **Low-confidence** | Khi AI không chắc, hệ thống có hỏi lại, show options hoặc chuyển người không? | ❌ **Không tồn tại.** Bot không bao giờ hỏi lại, kể cả khi câu hỏi mơ hồ hoặc có nhiều cách hiểu. Nó tự chọn keyword và trả lời ngay. |
| **Failure** | Khi AI sai, user biết bằng cách nào và sửa thế nào? | ❌ **Không có cơ chế báo lỗi.** Bot không nhận ra mình đang sai (kể cả khi xác nhận ngầm tiền đề phi lý). User chỉ phát hiện lỗi nhờ kiến thức bản thân. Không có nút "Sửa lại" hay "Báo cáo không chính xác". |
| **Correction** | Khi user sửa, correction có được lưu/log/học lại không hay biến mất? | ❌ **Biến mất hoàn toàn.** Câu hỏi thứ 2 là user đang làm rõ intent, nhưng bot trả về y hệt câu đầu — không memory, không context, mỗi lượt là fresh start. |

> **Nhận xét:** Trong 4 paths, V-AI chỉ có **Happy path** ở mức cơ bản. Ba paths Low-confidence, Failure, và Correction hoàn toàn vắng mặt.

---

## 4. Viết finding thành quyết định

### Finding 1 — Intent parser không tồn tại, chỉ match keyword

```text
Khi user hỏi câu quan hệ nhiều chiều ("mối quan hệ giữa A và B"),
V-AI bỏ qua động từ quan hệ, chỉ trích xuất entity nổi tiếng hơn (A)
và trả về profile boilerplate của entity đó,
hậu quả là user nhận thông tin không liên quan đến câu hỏi thực sự,
buộc phải gõ lại từ đầu hoặc bỏ cuộc.
Lỗi thuộc layer: Intent (không parse được relational query).
Nên sửa bằng: Intent classifier — nhận diện dạng câu "mối quan hệ giữa X và Y"
thành một query type riêng biệt với "profile của X".
Test case: Input "quan hệ giữa A và B" → Output phải đề cập đến cả A lẫn B.
```

### Finding 2 — Không có safety filter cho tiền đề sai/phi lý

```text
Khi user đặt câu hỏi với tiền đề sai hoàn toàn
("Elon Musk có phải anh của ông Phạm Nhật Vượng không"),
V-AI không phủ nhận tiền đề, không cảnh báo thông tin sai lệch,
mà tiếp tục trả về profile mô tả ông Phạm Nhật Vượng như bình thường,
hậu quả là user có thể hiểu bot đang xác nhận tiền đề sai,
gây hiểu nhầm nghiêm trọng về sự kiện thực tế.
Lỗi thuộc layer: Safety / Factual Grounding.
Nên sửa bằng: Fact-checking layer — khi câu hỏi chứa assertion sai
("A là anh/em/vợ của B" trong khi thực tế không phải),
bot phải phủ nhận rõ ràng trước, sau đó mới cung cấp thông tin đúng.
Test case: "X có phải [quan hệ thân tộc] của Y không?"
với X, Y là người không có quan hệ → bot phải trả lời "Không, ..."
```

### Finding 3 — Không có session memory, mỗi lượt là reset hoàn toàn

```text
Khi user gửi câu hỏi follow-up dùng từ dẫn chiếu ("Vậy Elon Musk..."),
V-AI xử lý câu đó độc lập như một session mới hoàn toàn,
hậu quả là bot không hiểu "Vậy" dẫn chiếu đến ngữ cảnh câu trước,
user phải lặp lại toàn bộ context trong mỗi câu hỏi — trải nghiệm không khác gì Google Search.
Lỗi thuộc layer: UX Recovery + Promise
(hứa "trợ lý ảo thông minh" nhưng thiếu memory cơ bản).
Nên sửa bằng: Short-term session context (tối thiểu 10 lượt) —
giữ nguyên topic và entities đã nhắc trong cùng cuộc hội thoại.
```

---

## 5. Sketch as-is / to-be

### AS-IS — Flow hiện tại (đánh dấu điểm gãy)

```
User gõ câu hỏi
        │
        ▼
[V-AI nhận input]
        │
        ▼
[Trích xuất keyword nổi tiếng nhất]
        │        ↑
        │        ❌ Bỏ qua verb/relation/intent
        │        ❌ Bỏ qua tiền đề đúng/sai
        ▼
[Tra database → Profile của entity đó]
        │
        ▼
[Hiển thị response boilerplate]
        │
        ▼
User gõ câu follow-up ("Vậy...")
        │        ↑
        │        ❌ Bot reset — không có context
        ▼
[Trích xuất keyword → Cùng entity → Cùng response]
        │
        ▼
[User bỏ cuộc / thoát app]
        ↑
        ❌ Không có recovery path, không có nút feedback
```

---

### TO-BE — Flow đề xuất (đánh dấu path đã sửa)

```
User gõ câu hỏi
        │
        ▼
[Intent Classifier]  ✅
        │
        ├── Relational query ("mối quan hệ giữa A và B")
        │         └──► [Relation Engine → Trả lời về cả A lẫn B]  ✅
        │
        ├── Profile query ("A là ai?")
        │         └──► [Profile DB]  ✅
        │
        ├── Ambiguous / Confidence thấp
        │         └──► [Hỏi lại user: "Bạn muốn biết về...?"]  ✅
        │
        └── Tiền đề sai ("A có phải X của B?")
                  └──► [Fact-check → Phủ nhận + Giải thích đúng]  ✅
                              │
                              ▼
                    [Session Context — giữ entities trong 10 lượt]  ✅
                              │
                              ▼
                    [Response phù hợp với intent]
                              │
                              ▼
                    [Nút feedback: 👍 Đúng / ✏️ Sửa lại]  ✅
                              │
                    ┌─────────┴──────────┐
                    ▼                    ▼
              [Log correction]    [User gõ tiếp trong context]
              [Cải thiện model]   [Bot nhớ ngữ cảnh câu trước]  ✅
```

---

## 6. Tự kiểm trước khi nộp

- [x] Có ít nhất 1 screenshot hoặc observation cụ thể.  
  → Screenshot 2 lượt chat liên tiếp, cùng response boilerplate; tiền đề sai không bị phủ nhận.

- [x] Có đủ 4 paths hoặc nói rõ path nào chưa có trong product.  
  → Happy path: Có (ở mức cơ bản). Low-confidence, Failure Recovery, Correction: Không tồn tại trong sản phẩm hiện tại.

- [x] Finding được viết thành product decision, không chỉ là nhận xét.  
  → 3 findings với format chuẩn: trigger → failure → impact → layer lỗi → hướng sửa + test case.

- [x] Sketch có as-is và to-be.  
  → ASCII flow diagram: AS-IS đánh dấu ❌ điểm gãy, TO-BE đánh dấu ✅ điểm sửa.

- [x] Có một câu nói rõ finding này sẽ đổi gì trong SPEC.

> **SPEC Change Required:**  
> Bổ sung vào Acceptance Criteria của V-AI module:  
> 1. Bot **phải** parse relational query ("mối quan hệ giữa X và Y") thành intent riêng — output phải đề cập đến cả X lẫn Y, không phải profile đơn lẻ.  
> 2. Bot **phải** phát hiện và phủ nhận tiền đề sai trước khi cung cấp thông tin bổ sung.  
> 3. Session context **phải** được duy trì tối thiểu 10 lượt trong cùng cuộc hội thoại.  
> 4. Mỗi response **phải** có nút feedback để user báo lỗi hoặc yêu cầu sửa.

---