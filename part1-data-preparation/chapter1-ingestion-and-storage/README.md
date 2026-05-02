Dưới đây là bản dịch song ngữ Anh - Việt, giữ nguyên cấu trúc Markdown và các thuật ngữ chuyên ngành để bạn dễ dàng đối chiếu:


# Data Ingestion and Storage / Nhập và Lưu trữ Dữ liệu

Before diving into specific services, basic concepts to define properties of data and therefore determine right format of data to hold information and storage system to use are discussed.
Trước khi đi sâu vào các dịch vụ cụ thể, chúng ta sẽ thảo luận về các khái niệm cơ bản nhằm xác định các thuộc tính của dữ liệu, từ đó quyết định định dạng dữ liệu phù hợp để lưu trữ thông tin và hệ thống lưu trữ cần sử dụng.

1. [Properties of Data / Các Thuộc tính của Dữ liệu](#1-properties-of-data)
1. [Common Data Formats / Các Định dạng Dữ liệu Phổ biến](#2-common-data-formats)
    1. [text-based formats / định dạng dựa trên văn bản](#1-text-based-formats)
    1. [binary formats / định dạng nhị phân](#2-binary-formats)
1. [Storage Systems / Hệ thống Lưu trữ](#3-storage-systems)
    1. [Required Jobs: Extract, Transform, Load / Các Công việc Cần thiết: Trích xuất, Chuyển đổi, Tải](#extract-transform-load)
    1. [Comparing Data Warehouse and Data Lake / So sánh Kho dữ liệu và Hồ dữ liệu](#data-warehousedw-data-lakedl)

## 1. Properties of Data / 1. Các Thuộc tính của Dữ liệu

Three keywords to describe basic characteristic of certain dataset
Ba từ khóa để mô tả đặc điểm cơ bản của một tập dữ liệu nhất định

* **Volume / Khối lượng**: size of data / kích thước của dữ liệu
* **Velocity / Tốc độ**: speed of new data being generated, collected and processed / tốc độ dữ liệu mới được tạo ra, thu thập và xử lý
    * ex. need real-time processing <-> batch processing is enough / ví dụ: cần xử lý theo thời gian thực <-> xử lý theo lô (batch) là đủ
* **Variety / Đa dạng**: type of data / loại dữ liệu
    * *structured / có cấu trúc*: usually refers to tabular data with predefined schema(ex. CSV, RDBMS table) / thường chỉ dữ liệu dạng bảng với lược đồ (schema) được xác định trước (ví dụ: CSV, bảng RDBMS)
    * *semi-structured / bán cấu trúc*: not as organized as structured, so requires some parsing to pull out information from it(ex. XML, JSON) / không có tổ chức chặt chẽ như dữ liệu có cấu trúc, do đó cần phân tích cú pháp (parsing) để trích xuất thông tin (ví dụ: XML, JSON)
    * *unstructured / phi cấu trúc*: unorganized data without any predefined schema or parsable structure(ex. image, text) / dữ liệu không có tổ chức, không có lược đồ xác định trước hoặc cấu trúc có thể phân tích cú pháp (ví dụ: hình ảnh, văn bản)

## 2. Common Data Formats / 2. Các Định dạng Dữ liệu Phổ biến

Classified into two categories
Được phân loại thành hai danh mục

### (1) text-based formats / định dạng dựa trên văn bản

Human readable, editable and system independent(because any system that can read, write text data can access to the information)
Con người có thể đọc được, có thể chỉnh sửa và độc lập với hệ thống (vì bất kỳ hệ thống nào có thể đọc, ghi dữ liệu văn bản đều có thể truy cập thông tin)

* **CSV** : delimeter separated format / định dạng phân tách bằng dấu phân cách (delimiter)
    * shares same abstraction design with RDBMS table(i.e. *row-column structure*) / chia sẻ cùng thiết kế trừu tượng với bảng RDBMS (tức là *cấu trúc hàng-cột*)
    * therefore, source and target is usually *small to medium sized RDBMS table* / do đó, nguồn và đích thường là *bảng RDBMS quy mô từ nhỏ đến trung bình*
* **JSON** : key-value format / định dạng cặp khóa-giá trị (key-value)
    * unlike CSV, also suitable for *semi-structured data* because its key-value structure allows nested data structure / không giống như CSV, cũng phù hợp cho *dữ liệu bán cấu trúc* vì cấu trúc khóa-giá trị cho phép cấu trúc dữ liệu lồng nhau
    * suitable for data whose *schema is flexible*(i.e. each row can have different keys), so fits well to NoSQL DB like MongoDB / phù hợp với dữ liệu có *lược đồ linh hoạt* (tức là mỗi hàng có thể có các khóa khác nhau), nên rất hợp với cơ sở dữ liệu NoSQL như MongoDB

### (2) binary formats / định dạng nhị phân

Not human readable, nor editable
Con người không thể đọc được, cũng không thể chỉnh sửa được

* **Avro**: binary data with its schema embedded / dữ liệu nhị phân có lược đồ nhúng bên trong
    * suitable for system where *schema changes frequently*(information in the data will be preserved even when the schema changes) / phù hợp với hệ thống mà *lược đồ thay đổi thường xuyên* (thông tin trong dữ liệu sẽ được bảo toàn ngay cả khi lược đồ thay đổi)
    * its binary nature makes serialization more efficient, so *good choice for data transportation*(frequently used in real-time big data processing system like Kafka, Spark, Flink etc) / bản chất nhị phân giúp tuần tự hóa (serialization) hiệu quả hơn, do đó là *lựa chọn tốt để vận chuyển dữ liệu* (thường được sử dụng trong hệ thống xử lý dữ liệu lớn thời gian thực như Kafka, Spark, Flink, v.v.)
* **Parquet**: column-based binary data with its schema embedded / dữ liệu nhị phân theo cột có lược đồ nhúng bên trong
    * good choice to *store data in distributed system*(column A in this node while B in that node) where efficient compression comes in column by column / lựa chọn tốt để *lưu trữ dữ liệu trong hệ thống phân tán* (cột A ở nút này trong khi cột B ở nút khác) nơi nén dữ liệu hiệu quả diễn ra theo từng cột
    * *optimizes large scale data analytics*, since only subset of columns is required in most cases / *tối ưu hóa phân tích dữ liệu quy mô lớn*, vì trong hầu hết các trường hợp chỉ cần một tập con của các cột

## 3. Storage Systems / 3. Hệ thống Lưu trữ

On jobs that those systems should do and when to choose what
Về các công việc mà các hệ thống đó nên thực hiện và khi nào nên chọn cái nào

### Extract, Transform, Load / Trích xuất, Chuyển đổi, Tải (ETL)

These processes must be automated in reliable way, and how to implement it using AWS services will be covered in following sections.
Các quy trình này phải được tự động hóa một cách đáng tin cậy, và cách triển khai nó bằng các dịch vụ AWS sẽ được đề cập trong các phần sau.

* Extract / Trích xuất: retrieve raw data from *external source systems* including / truy xuất dữ liệu thô từ *các hệ thống nguồn bên ngoài*, bao gồm
    * JDBC(or, ODBC): common interface for connecting external DB / giao diện phổ biến để kết nối với CSDL bên ngoài
    * raw log files: stored in S3, parsed according to specific schema / các tệp nhật ký thô: được lưu trữ trong S3, phân tích cú pháp theo một lược đồ cụ thể
    * external APIs: response, request in JSON format is extracted and stored / API bên ngoài: phản hồi, yêu cầu ở định dạng JSON được trích xuất và lưu trữ
    * data stream: when real-time consumption takes place / luồng dữ liệu: khi quá trình tiêu thụ theo thời gian thực diễn ra
* Transform / Chuyển đổi: Convert data into a suitable format / Chuyển đổi dữ liệu sang định dạng phù hợp
* Load / Tải: Move (transformed) data into a target storage / Di chuyển (dữ liệu đã chuyển đổi) vào kho lưu trữ đích

### Data Warehouse(DW), Data Lake(DL) / Kho dữ liệu (DW), Hồ dữ liệu (DL)

Note that these are gradually considered as legacy approach, since hybrid of two(Data Lakehouse) is becoming a norm for ML data storage system(ex. Redshift Spectrum on S3 objects).
Lưu ý rằng những cách tiếp cận này đang dần được coi là phương pháp cũ (legacy), vì sự kết hợp của cả hai (Data Lakehouse / Hồ nhà kho dữ liệu) đang trở thành tiêu chuẩn cho hệ thống lưu trữ dữ liệu ML (ví dụ: Redshift Spectrum trên các đối tượng S3).

* DW: centralized storage for OLAP / lưu trữ tập trung cho OLAP
* DL: raw-data native format storage(loaded as-is) / lưu trữ định dạng gốc của dữ liệu thô (được tải nguyên trạng)

||Data Warehouse / Kho dữ liệu|Data Lake / Hồ dữ liệu|
|-|-|-|
|schema / lược đồ|schema-on-write(predefined schema exists at the time of loading) / lược đồ-ghi (lược đồ xác định trước tồn tại tại thời điểm tải)|schema-on-read(data consumer defines schema on raw data) / lược đồ-đọc (người tiêu dùng dữ liệu xác định lược đồ trên dữ liệu thô)|
|data type / loại dữ liệu|structured / có cấu trúc|anything / bất kỳ loại nào|
|agility / tính linh hoạt|less agile due to predefined schema / ít linh hoạt hơn do lược đồ được xác định trước|adaptable to schema change afterwards / có thể thích ứng với các thay đổi lược đồ sau này|
|processing / xử lý|ETL|EL(T)|
|use-case / trường hợp sử dụng|complex queries on large data(optimized for such queries, so suitable for BI tasks) / truy vấn phức tạp trên dữ liệu lớn (được tối ưu hóa cho các truy vấn như vậy, nên phù hợp cho các tác vụ BI)|cost-effective, scalable storage for data whose usage is not yet determined / lưu trữ tiết kiệm chi phí, có khả năng mở rộng cho dữ liệu mà mục đích sử dụng chưa được xác định|
|etc / khác|data integration from different sources is essential(otherwise, it's just database) / tích hợp dữ liệu từ các nguồn khác nhau là điều cần thiết (nếu không, nó chỉ là cơ sở dữ liệu)|costs can rise when used to process large amounts of data / chi phí có thể tăng lên khi được sử dụng để xử lý lượng lớn dữ liệu|
