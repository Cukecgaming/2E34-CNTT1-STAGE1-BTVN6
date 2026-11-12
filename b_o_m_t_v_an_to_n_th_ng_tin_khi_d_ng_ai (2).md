Cuốn cẩm nang này được xây dựng bằng kỹ thuật **chia nhỏ yêu cầu (Prompt Chaining)**, tích hợp toàn diện các nguyên lý cốt lõi: hiểu bản chất mô hình ngôn ngữ lớn (LLM), quản lý Token, phòng chống ảo giác (Hallucination) và thiết kế câu lệnh chuẩn khung **CREATE**.

## Phần 1: Bản chất của AI — Cách LLM "đoán từ" và quản lý Token

### 1. LLM hoạt động như thế nào?

Các mô hình ngôn ngữ lớn (như ChatGPT, Gemini, Claude) **không có tư duy logic hay ý thức** như con người. Về bản chất, chúng là các "cỗ máy dự đoán xác suất thống kê":

* Khi bạn nhập một câu hỏi, AI sẽ phân tích ngữ cảnh và tính toán xem **từ (Token) tiếp theo nào có xác suất xuất hiện hợp lý nhất** dựa trên hàng tỷ văn bản đã học.

* **Ví dụ:** Khi thấy cụm từ *"Hôm nay trời mưa, tôi quên mang..."*, AI sẽ tính toán xác suất và chọn từ *"ô"* hoặc *"áo mưa"* chứ không thực sự hiểu cảm giác bị ướt.

### 2. Kiểm soát Token & Cửa sổ ngữ cảnh (Context Window)

* **Token là gì?** Là đơn vị dữ liệu nhỏ nhất mà AI đọc hiểu (1 từ tiếng Anh $\approx 1$ token; 1 từ tiếng Việt có dấu thường tốn $2 - 3$ tokens).

* **Context Window (Bộ nhớ RAM tạm thời):** Là giới hạn tối đa lượng thông tin AI có thể ghi nhớ trong một phiên trò chuyện. Nếu bạn dán đoạn code quá dài hoặc chat quá nhiều lượt, thông tin cũ ở đầu phiên sẽ bị đẩy ra ngoài, khiến AI **"quên" yêu cầu ban đầu**.

* **Mẹo cho tân sinh viên:** Hãy chia nhỏ bài toán (Prompt Chaining), viết câu lệnh ngắn gọn, súc tích và mở phiên chat mới khi chuyển sang chủ đề/bài tập khác.

## Phần 2: 3 cách nhận diện và phòng chống "Ảo giác" (Hallucination) của AI

Vì hoạt động theo cơ chế đoán từ hợp lý, đôi khi AI sẽ **tự tin bịa ra thông tin sai sự thật** (ảo giác) để làm hài lòng người hỏi. Dưới đây là 3 dấu hiệu nhận diện khi học lập trình:

1. **Bịa đặt thư viện, hàm (Function) hoặc API không tồn tại:**

   * *Dấu hiệu:* AI gợi ý đoạn code sử dụng một thư viện nghe rất hợp lý nhưng khi chạy (`import`) thì báo lỗi `ModuleNotFoundError` hoặc hàm không tồn tại trong tài liệu chính thức.

   * *Cách khắc phục:* Luôn chạy thử code trên môi trường thực tế (IDE) và đối chiếu với tài liệu gốc (Official Docs).

2. **Sai lệch số liệu, mốc lịch sử và trích dẫn nguồn:**

   * *Dấu hiệu:* AI đưa ra đường link tham khảo bị lỗi 404, hoặc nhầm lẫn năm ra đời, tác giả của các ngôn ngữ lập trình/công nghệ.

   * *Cách khắc phục:* Áp dụng kỹ thuật **RAG Grounding** (cung cấp trực tiếp tài liệu giáo trình của trường cho AI đọc trước khi hỏi) và **Cross-check** (kiểm tra chéo câu trả lời trên 2 mô hình AI khác nhau).

3. **Lập luận vòng vo, tự mâu thuẫn khi bị hỏi vặn:**

   * *Dấu hiệu:* Khi bạn chỉ ra lỗi sai trong đoạn code AI vừa viết, AI lập tức xin lỗi và sửa lại nhưng lại sinh ra một lỗi logic khác tương tự.

   * *Cách khắc phục:* Thêm câu ràng buộc cứng vào Prompt: *"Nếu không chắc chắn 100%, hãy trả lời tôi không biết, tuyệt đối không tự suy đoán"*.

> **Nguyên tắc vàng:** Luôn đặt con người làm trung tâm kiểm duyệt cuối cùng (**Human-in-the-Loop**) và **tuyệt đối không dán mật khẩu, API Key thật** vào khung chat.

## Phần 3: Cấu trúc Prompt mẫu tối ưu theo khung CREATE

Thay vì viết câu lệnh sơ sài (*"Garbage In, Garbage Out"*), tân sinh viên hãy áp dụng công thức **CREATE** để AI đóng vai gia sư lập trình chuẩn xác ngay từ lần đầu:

* **C – Context (Bối cảnh & Vai trò):** Định vị vai trò cho AI và nêu rõ trình độ hiện tại của bạn.

* **R – Request (Yêu cầu cụ thể):** Nêu rõ nhiệm vụ muốn AI thực hiện (giải thích lỗi, tối ưu code, gợi ý thuật toán...).

* **E – Explanation (Giải thích & Ràng buộc):** Đưa ra các quy tắc cấm (không giải hộ bài tập từ A-Z, chỉ gợi ý hướng tư duy, không bịa đặt).

* **A – Audience (Đối tượng mục tiêu):** Xác định người đọc là sinh viên năm nhất mới làm quen với lập trình.

* **T – Tone (Giọng văn):** Thân thiện, sư phạm, khuyến khích tư duy.

* **E – Example / Extras (Định dạng mẫu):** Quy định cách trình bày đầu ra (Markdown, chia từng bước, có chú thích từng dòng code).

### Mẫu Prompt chuẩn áp dụng thực tế (Copy & Dùng ngay):

> **\[C - Context\]:** Bạn là một giảng viên đại học tận tâm chuyên dạy môn Nhập môn Lập trình Python. Tôi là sinh viên năm nhất mới học lập trình được 2 tuần.
>
> **\[R - Request\]:** Hãy giúp tôi tìm lỗi logic trong đoạn code tính tổng các số chẵn dưới đây và hướng dẫn cách sửa:
>
> ```
> n = 10
> total = 0
> for i in range(n):
>     if i % 2 = 0:
>         total += i
> print(total)
> 
> ```
>
> **\[E - Explanation\]:**
>
> * Chỉ ra chính xác dòng bị lỗi cú pháp/logic và giải thích nguyên nhân.
>
> * **Ràng buộc:** Không viết lại toàn bộ code hoàn chỉnh ngay lập tức, hãy đưa ra gợi ý để tôi tự sửa. Nếu đoạn code thiếu ngữ cảnh, hãy hỏi lại tôi chứ không tự suy đoán.
>
> **\[A - Audience\]:** Sinh viên mới bắt đầu, chưa rành các thuật ngữ chuyên sâu phức tạp.
>
> **\[T - Tone\]:** Dễ hiểu, kiên nhẫn, mang tính định hướng sư phạm.
>
> **\[E - Extras\]:** Trình bày câu trả lời bằng định dạng Markdown gồm 3 mục ngắn gọn: `1. Vị trí lỗi`, `2. Giải thích nguyên nhân`, và `3. Gợi ý cách sửa`.