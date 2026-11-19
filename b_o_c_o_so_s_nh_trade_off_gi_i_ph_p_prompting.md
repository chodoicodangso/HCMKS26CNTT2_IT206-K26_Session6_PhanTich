# BÁO CÁO SO SÁNH TRADE-OFF GIẢI PHÁP PROMPTING: GIA SƯ FEYNMAN & CHẤM ĐIỂM RUBRIC

## BƯỚC 1: ĐỀ XUẤT 2 PROMPT MẪU

### Giải pháp 1: Prompt đơn giản, ngắn gọn (Zero-shot cơ bản)

```
Đóng vai gia sư Feynman và giám khảo chấm thi. Hãy chấm điểm bài làm dưới đây dựa trên tiêu chí (nếu trống thì tự chấm thang 10) và nhận xét ngắn gọn xem học viên hiểu đúng chỗ nào, sai chỗ nào, kèm 1 câu hỏi gợi mở. Nếu bài làm trống hoặc lạc đề thì báo lỗi và cho 0 điểm.

- Chủ đề: [Nhập chủ đề]
- Trình độ: [Nhập trình độ]
- Bài làm: [Nhập bài làm]
- Tiêu chí chấm: [Nhập tiêu chí]

```

### Giải pháp 2: Prompt chi tiết, có ràng buộc chặt chẽ (Structured & Guardrails)

```
Hãy đóng vai một gia sư áp dụng phương pháp Feynman (giải thích đơn giản, dễ hiểu) kết hợp với giám khảo chấm thi để đánh giá bài làm của học viên dưới đây.

Thông tin đầu vào:
- Chủ đề: [Nhập chủ đề]
- Trình độ học viên: [Nhập trình độ]
- Bài làm của học viên: [Nhập bài làm]
- Tiêu chí chấm (Rubric): [Nhập tiêu chí]

Trước khi chấm, hãy kiểm tra kỹ dữ liệu theo các quy tắc sau:
1. Nếu phần Chủ đề hoặc Bài làm bị trống, dưới 10 từ, hoặc toàn ký tự vô nghĩa: Dừng lại ngay và chỉ báo đúng một dòng "[ERROR_01]: Dữ liệu không hợp lệ hoặc bị trống."
2. Nếu Bài làm viết lạc đề hoàn toàn so với Chủ đề: Dừng lại và chỉ báo "[ERROR_02]: Bài làm lạc đề. Điểm: 0/10."
3. Nếu phần Tiêu chí chấm bị bỏ trống: Hãy tự động dùng thang điểm 10 mặc định gồm 3 phần: Chính xác (4đ), Diễn đạt đơn giản (4đ), và Ví dụ thực tế (2đ).

Nếu dữ liệu hợp lệ, hãy chấm điểm và nhận xét bằng ngôn ngữ phù hợp với trình độ của học viên. Lưu ý trừ một nửa số điểm phần Diễn đạt đơn giản nếu thấy bài làm chép nguyên văn định nghĩa trong sách vở. Cuối cùng, không giải bài hộ mà chỉ đặt 1-2 câu hỏi gợi mở để học viên tự sửa.

Trình bày kết quả ngắn gọn theo đúng 2 mục dưới đây (không cần lời chào hỏi):
1. Bảng điểm chi tiết (từng tiêu chí, điểm số, nhận xét ngắn) và Tổng điểm.
2. Nhận xét Feynman gồm: Ý đã làm tốt, Lỗ hổng kiến thức, và Câu hỏi gợi mở.

```

## BƯỚC 2: BẢNG SO SÁNH TRADE-OFF

| Tiêu chí đánh giá | Giải pháp 1 (Đơn giản) | Giải pháp 2 (Chi tiết) | Phân tích sự đánh đổi (Trade-off) | 
| ----- | ----- | ----- | ----- | 
| **Độ dễ hiểu & Bảo trì** | Cao | Trung bình | Giải pháp 1 viết cực ngắn trong 1 đoạn. Giải pháp 2 chia nhiều ý điều kiện nên dài và tốn công đọc hơn. | 
| **Chi phí Token** | Ít (\~70 tokens) | Nhiều hơn (\~240 tokens) | Giải pháp 2 tốn lượng token đầu vào gấp \~3.4 lần do viết rõ từng kịch bản bắt lỗi và bố cục đầu ra. | 
| **Chặn lỗi biên (Edge cases)** | Trung bình | Cực tốt | Giải pháp 1 dễ bị AI bỏ qua lỗi hoặc tự ý chấm điểm khi dữ liệu rác. Giải pháp 2 chặn đứng lỗi bằng mã quy ước rõ ràng. | 
| **Tính nhất quán của Output** | Thấp | Rất cao | Giải pháp 1 mỗi lần chạy sẽ ra một kiểu trình bày khác nhau. Giải pháp 2 ép AI trả về đúng bố cục bảng cố định. | 
| **Chất lượng sư phạm (Feynman)** | Trung bình | Cao | Giải pháp 2 kiểm soát được việc trừ điểm chép văn mẫu và cấm AI làm bài hộ học viên. | 

## BƯỚC 3: LỰA CHỌN PHƯƠNG ÁN TỐI ƯU VÀ CHẠY THỬ

### 1. Lựa chọn & Lý do

* **Phương án được chọn:** **Giải pháp 2 (Chi tiết, nhiều ràng buộc)**.

* **Lý do chọn:** Trong bài toán tự động hóa hệ thống gia sư và chấm điểm hàng loạt, **độ chính xác, tính nhất quán và khả năng bắt lỗi dữ liệu** quan trọng hơn việc tiết kiệm một lượng nhỏ token đầu vào. Giải pháp 2 giúp loại bỏ hoàn toàn tình trạng AI chấm điểm sai khi học viên nộp bài trống/lạc đề, đồng thời giữ cho câu chữ tự nhiên, dễ chỉnh sửa mà vẫn ép được định dạng đầu ra chuẩn xác.

### 2. Chạy thử thực tế với Giải pháp 2

**Dữ liệu nạp vào (Input):**

* **Chủ đề:** Cơ chế hoạt động của RAM máy tính

* **Trình độ học viên:** Sinh viên năm nhất ngành CNTT

* **Bài làm của học viên:** *"RAM là bộ nhớ truy cập ngẫu nhiên, cho phép đọc và ghi dữ liệu với tốc độ cao. Dữ liệu trên RAM mang tính chất khả biến (volatile), sẽ bị mất khi ngắt nguồn điện. Ví dụ như khi mở nhiều tab trình duyệt cùng lúc."*

* **Tiêu chí chấm (Rubric):** *(Để trống - Kiểm tra cơ chế tự động áp dụng Rubric mặc định)*

**Phản hồi thực tế từ AI (Output):**

> **1. Bảng điểm chi tiết:**
>
> | Tiêu chí | Điểm | Nhận xét | 
> | ----- | ----- | ----- | 
> | Chính xác | 4/4 | Nắm đúng đặc tính tốc độ cao và tính chất mất dữ liệu khi tắt nguồn. | 
> | Diễn đạt đơn giản | 2/4 | Dùng từ chuyên ngành ("truy cập ngẫu nhiên", "khả biến") giống định nghĩa giáo trình. | 
> | Ví dụ thực tế | 2/2 | Ví dụ mở nhiều tab trình duyệt rất thực tế và đúng bản chất. | 
> | **TỔNG ĐIỂM** | **8/10** | **Khá** | 
>
> **2. Nhận xét Feynman:**
>
> * **Ý đã làm tốt:** Bạn hiểu chính xác vai trò lưu trữ tạm thời của RAM khi chạy đa nhiệm (mở nhiều tab) và đặc điểm mất dữ liệu khi mất điện.
>
> * **Lỗ hổng kiến thức:** Bạn đang dùng lại các thuật ngữ kỹ thuật cứng nhắc ("bộ nhớ truy cập ngẫu nhiên", "khả biến") thay vì diễn đạt lại bằng ngôn ngữ đời thường để chứng tỏ mình thực sự hiểu sâu.
>
> * **Câu hỏi gợi mở:** Nếu giải thích cho một người không rành công nghệ, bạn sẽ ví von RAM và ổ cứng giống như những đồ vật gì trên một bàn làm việc? Tại sao máy tính không dùng luôn ổ cứng để chạy ứng dụng cho đỡ tốn tiền mua RAM?