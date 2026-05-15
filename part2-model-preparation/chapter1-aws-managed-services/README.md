Dịch song ngữ Anh/Việt:

# AWS Managed AI Services / Các dịch vụ AI được quản lý trên AWS

When model development from scratch is overkill, managed AI services can be a good start. It's *token-based pricing* so there comes a tipping point where overall cost for developing model from scratch and then serve it on custom server gets cheaper. Still, since it's fully managed service by AWS, their *performance(both in hardware and algorithm) is optimized* at a decent level and available in many availability zones.

Khi phát triển mô hình từ đầu là quá mức cần thiết, các dịch vụ AI được quản lý sẵn có thể là một điểm khởi đầu tốt. Vì đây là *mô hình định giá dựa trên token* nên sẽ có một điểm bùng phát, tại đó tổng chi phí cho việc tự phát triển mô hình từ đầu và sau đó triển khai trên máy chủ riêng sẽ trở nên rẻ hơn. Dù vậy, vì đây là dịch vụ được quản lý hoàn toàn bởi AWS, *hiệu suất (cả về phần cứng lẫn thuật toán) của họ được tối ưu hóa* ở mức độ rất tốt và có sẵn tại nhiều vùng khả dụng (availability zones).

* Text
    * **Comprehend** : supervised tasks like NER, sentiment analysis or categorization
    * **Translate** : language translation for content localization
    * **Kendra** : document search service where its source can be S3, Google Drive etc(incremental learning enabled)
    * **Lex** : chatbot creator with integration with Lambda(like RAG), Comprehend, Kendra

* Văn bản
    * **Comprehend**: các tác vụ học có giám sát (supervised) như nhận dạng thực thể (NER), phân tích cảm xúc (sentiment analysis) hoặc phân loại
    * **Translate**: dịch ngôn ngữ dùng cho việc nội địa hóa nội dung (content localization)
    * **Kendra**: dịch vụ tìm kiếm tài liệu với nguồn dữ liệu có thể là S3, Google Drive, v.v. (hỗ trợ học tăng trưởng - incremental learning)
    * **Lex**: công cụ tạo chatbot có khả năng tích hợp với Lambda (như kiến trúc RAG), Comprehend, Kendra

* Vision
    * **Textract** : extract text from image, PDF
    * **Rekognition** : object detection(and labeling), facial search
    * **A2I**(Augmented AI) : enable human evaluation on low confidence result from Textract and Rekognition
    * **Lookout** : detectors monitor datasets to find anomalies by optimizing configured measure

* Thị giác (Computer Vision)
    * **Textract**: trích xuất văn bản từ hình ảnh, PDF
    * **Rekognition**: phát hiện đối tượng (và gán nhãn), tìm kiếm khuôn mặt
    * **A2I** (Augmented AI - AI tăng cường): cho phép con người đánh giá lại các kết quả có độ tin cậy thấp từ Textract và Rekognition
    * **Lookout**: các bộ phát hiện theo dõi tập dữ liệu để tìm ra sự bất thường bằng cách tối ưu hóa các chỉ số đã được cấu hình

* Speech
    * **Polly** : text to speech with custom vocabulary reinforcement(i.e. how certain term should be pronounced)
    * **Transcribe** : speech to text with PII redaction, automatic language identification and custom vocabulary reinforcement

* Giọng nói
    * **Polly**: chuyển văn bản thành giọng nói (text-to-speech) có hỗ trợ củng cố từ vựng tùy chỉnh (ví dụ: một thuật ngữ cụ thể nên được phát âm như thế nào)
    * **Transcribe**: chuyển giọng nói thành văn bản (speech-to-text) với tính năng ẩn thông tin nhận dạng cá nhân (PII redaction), tự động nhận diện ngôn ngữ và củng cố từ vựng tùy chỉnh

* Recommendations
    * **Personalize** : collection of recipes for different recommendation purpose(ex. POPULAR_ITEMS, RELATED_ITEMS)

* Hệ thống gợi ý
    * **Personalize**: tập hợp các công thức (thuật toán mẫu) cho các mục đích gợi ý khác nhau (ví dụ: POPULAR_ITEMS - Các mặt hàng phổ biến, RELATED_ITEMS - Các mặt hàng liên quan)
