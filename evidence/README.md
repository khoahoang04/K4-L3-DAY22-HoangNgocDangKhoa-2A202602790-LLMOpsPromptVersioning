# Báo cáo đánh giá RAGAS - So sánh Prompt V1 và Prompt V2

## 1. Bảng điểm tổng hợp

| Chỉ số | Prompt V1 (Ngắn gọn) | Prompt V2 (Phân tích cấu trúc) | Winner |
|---|:---:|:---:|:---:|
| **faithfulness** | **0.9538** | 0.9323 | **V1** |
| **answer_relevancy** | **0.9187** | 0.8831 | **V1** |
| **context_recall** | **1.0000** | **1.0000** | Hòa (1.0) |
| **context_precision** | 0.9417 | **0.9450** | **V2** |

- **Mục tiêu rubric**: Faithfulness >= 0.8 cho ít nhất 1 phiên bản: **ĐẠT** (Cả 2 phiên bản đều đạt >= 0.93, đủ điều kiện nhận điểm thưởng).

---

## 2. Phân tích chi tiết kết quả

### 2.1. Faithfulness (Độ trung thực với ngữ cảnh): V1 (0.9538) > V2 (0.9323)
- **Prompt V1** chỉ thị rõ: *"Trả lời ngắn gọn (2-4 câu), chỉ dựa trên context. Nếu không có thông tin, hãy nói thẳng là không biết."* Phong cách trả lời trực diện, ngắn gọn giúp LLM tập trung trích xuất chính xác các ý từ context mà không diễn giải thêm, hạn chế tối đa việc hallucinate hoặc thêm thắt ngữ nghĩa ngoài tài liệu.
- **Prompt V2** yêu cầu: *"Chuyên gia phân tích thông tin... đọc kỹ context, xác định các facts liên quan, rồi viết câu trả lời rõ ràng, có tổ chức (3-5 câu)."* Vì phải tổ chức lập luận và viết dài hơn, LLM có xu hướng sử dụng thêm các từ liên kết, khái quát hóa vấn đề, khiến evaluator đôi lúc đánh giá có thông tin diễn đạt vượt ngoài câu chữ gốc của context.

### 2.2. Answer Relevancy (Độ liên quan câu trả lời): V1 (0.9187) > V2 (0.8831)
- **V1** thắng ở chỉ số này vì câu trả lời đi thẳng vào trọng tâm câu hỏi mà người dùng đưa ra, không chứa thông tin phụ.
- **V2** do cố gắng phân tích cấu trúc facts nên thường mở rộng thêm ngữ cảnh nền hoặc diễn giải các khía cạnh liên quan, làm giảm nhẹ độ cô đọng tập trung so với câu hỏi gốc.

### 2.3. Context Recall & Context Precision
- **Context Recall (1.0000 ở cả V1 và V2)**: Đạt điểm tối đa tuyệt đối, cho thấy pipeline retrieval với `k = 3` và chiến lược chunking đã bao phủ hoàn toàn các thông tin cần thiết có trong đáp án chuẩn (`reference`).
- **Context Precision (0.9417 vs 0.9450)**: Điểm số của cả hai tương đương nhau và ở mức rất cao, chứng minh các tài liệu được retriever xếp hạng ở top đầu đều thực sự liên quan đến câu hỏi.

---

## 3. Kết luận
- **Prompt V1** tối ưu hơn cho các bài toán tra cứu thông tin nhanh, trợ lý FAQ nơi tính chuẩn xác cao và câu trả lời ngắn gọn là ưu tiên hàng đầu.
- **Prompt V2** thích hợp cho bài toán báo cáo tóm tắt, nơi người dùng cần các câu trả lời có cấu trúc và phân tích chi tiết.
