# Các chủ đề Kinesis

Có ba dịch vụ liên quan đến sản phẩm xử lý dữ liệu luồng này. Nhìn tổng thể, dữ liệu được gửi bởi *producer* (nhà sản xuất) sẽ được lưu trữ tạm thời trong *Kinesis Data Stream* và sau đó gửi đến *consumer* (người tiêu dùng) như *Data Firehose* hoặc *Managed Service for Apache Flink*.

1. Kinesis Data Stream
    * (1) producer, consumer (2) không xóa, chỉ hết hạn (3) dành cho dữ liệu thời gian thực nhỏ
    * [Chế độ dung lượng](#capacity-mode) : (1) Provisioned (Cấp phát trước) (2) On-demand (Theo yêu cầu)
    * [Xử lý sự cố](#troubleshooting) 
1. Amazon Data Firehose
    * (1) không phát lại (2) dung sai lỗi (3) các loại dữ liệu được hỗ trợ
1. Amazon Managed Service for Apache Flink
    * (1) ETL trên luồng dữ liệu (2) tạo chỉ số liên tục
 
## Kinesis Data Stream

Sử dụng các *shard* (mảnh) trong *stream* (luồng) để thu thập dữ liệu thời gian thực từ *producer* và giữ lại để *consumer* có thể tiêu thụ. Dữ liệu được mã hóa bằng base64 ngay trên đường truyền (on the fly). Mỗi dữ liệu đều có *partition ID* (ID phân vùng), và dữ liệu có chung ID được đảm bảo thứ tự xử lý.

* **producer, consumer** : dữ liệu -> *producer* -> stream -> *consumer*
    * Producer bao gồm Kinesis Agent, các ứng dụng được triển khai bằng AWS SDK hoặc *AWS KPL* (Kinesis Producer Library; bộ công cụ để xây dựng ứng dụng producer tối ưu)
    * Consumer bao gồm *Kinesis Data Firehose*, *Kinesis Data Analytics*, *AWS Lambda function* và các ứng dụng được triển khai bằng AWS SDK hoặc *AWS KCL* (Kinesis Consumer Library; bộ công cụ để xây dựng ứng dụng consumer tối ưu)
* **không xóa, chỉ hết hạn** : Các consumer của stream có thể tiêu thụ lại dữ liệu trong stream trong một khoảng thời gian nhất định (tối đa 1 năm). Để đảm bảo điều này, dữ liệu sẽ không bị xóa trong khung thời gian này; không xóa, chỉ hết hạn.
* **dành cho dữ liệu thời gian thực nhỏ** : Kích thước tối đa của dữ liệu gửi đến shard trong một stream là 1MB. Để xử lý dữ liệu lớn hơn mức này, hãy sử dụng *AWS Managed Service for Kafka (MSK)*.

### Chế độ dung lượng (Capacity mode)

* **Chế độ Provisioned** : Chọn số lượng shard trong stream theo cách thủ công và trả phí theo mỗi shard mỗi giờ (tham khảo; mỗi shard có吞吐 lượng 1MB/s đọc vào và 2MB/s ghi ra)
* **Chế độ On-demand** : AWS giám sát lịch sử dung lượng trong 30 ngày và thiết lập吞吐 lượng tối ưu để tự động mở rộng (trả phí theo mỗi stream mỗi giờ và dữ liệu I/O tính bằng GB)

### Xử lý sự cố (Troubleshooting)

Tăng hiệu suất có thể được thực hiện một cách đơn giản bằng cách tăng số lượng shard trong stream, nhưng để xử lý sự cố chi tiết hơn, vui lòng tham khảo tài liệu chính thức về phía [producer](https://docs.aws.amazon.com/streams/latest/dev/troubleshooting-producers.html), [consumer](https://docs.aws.amazon.com/streams/latest/dev/troubleshooting-consumers.html).

## Amazon Data Firehose

Dịch vụ được quản lý hoàn toàn dùng để xếp dữ liệu gửi từ bất kỳ producer nào lên *buffer* (bộ đệm) và *flush* (xả) nó đến đích mong muốn (ví dụ: S3, OpenSearch, Redshift, HTTP endpoint) như một quy trình xử lý batch *gần thời gian thực* (near real-time). 

* **không phát lại** : Không có khả năng lưu trữ dữ liệu; nếu dữ liệu đã được xử lý và flush đến đích, dữ liệu này sẽ không còn hiển thị từ Firehose nữa.
* **dung sai lỗi** : Trong trường hợp xảy ra lỗi trong quá trình xử lý, Firehose có thể gửi dữ liệu lỗi này đến S3, do bản thân Firehose không có khả năng lưu trữ.
* **các loại dữ liệu được hỗ trợ** : Dữ liệu đã xếp có thể được chuyển đổi thành CSV, JSON, Avro, Parquet. Hoặc, việc chuyển đổi dữ liệu tùy chỉnh bằng AWS Lambda function cũng có thể thực hiện được.

## Amazon Managed Service for Apache Flink

Dịch vụ dùng để truy vấn hoặc xử lý dữ liệu trên stream. Cụ thể, dữ liệu chảy theo hướng source (ví dụ: KDS) -> MSAF -> sink (ví dụ: S3, Firehose). Người dùng có thể tự phát triển ứng dụng Flink của riêng họ và tải lên MSAF thông qua S3. Các trường hợp sử dụng bao gồm (1) ETL trên dữ liệu luồng (2) tạo chỉ số liên tục và (3) phân tích dữ liệu phản hồi nhanh (responsive).
