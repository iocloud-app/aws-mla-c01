Dưới đây là bản dịch song ngữ Anh - Việt cho tài liệu về Amazon S3:

# S3 Topics / Các chủ đề về S3

* *Objects* are stored into a *bucket* / *Đối tượng (Object)* được lưu trữ trong một *Bucket*
* Although *bucket* is created in certain region, its name has to be unique globally / Mặc dù *bucket* được tạo ở một vùng (region) nhất định, tên của nó phải là duy nhất trên toàn cầu
* Single *object* can be up to 5TB. However, if single *object* is larger than 5GB, *multi part upload* must be used / Một *đối tượng* đơn lẻ có thể có dung lượng lên tới 5TB. Tuy nhiên, nếu *đối tượng* đơn lẻ lớn hơn 5GB, bắt buộc phải sử dụng *tải lên đa phần (multi-part upload)*
* Object can include metadata such as tags and version ID. / Đối tượng có thể bao gồm siêu dữ liệu (metadata) như thẻ (tags) và ID phiên bản.

1. [Backup / Sao lưu](#2-data-backup)
    * [Versioning / Quản lý phiên bản](#versioning) : (1) Bucket level / Cấp bucket (2) Version ID / ID phiên bản (3) Delete marker / Đánh dấu xóa
    * [Replication / Sao chép](#replication) : (1) Bucket region / Vùng bucket (2) Optional delete marker replication / Sao chép đánh dấu xóa tùy chọn (3) New objects only / Chỉ đối tượng mới
1. [Optimizations / Tối ưu hóa](#3-optimizations)
    * [Cost Optimization / Tối ưu hóa chi phí](#cost-optimization) : (1) storage classes / lớp lưu trữ (2) intelligent tiering / phân lớp thông minh (3) lifecycle rule / quy tắc vòng đời
    * [Performance Optimization / Tối ưu hóa hiệu suất](#performance-optimization) : (1) multi-part upload / tải lên đa phần (2) transfer acceleration / tăng tốc truyền tải (3) byte-range fetches / truy xuất theo phạm vi byte
1. [Security / Bảo mật](#1-security)
    * [JSON Policy Format / Định dạng Chính sách JSON](#json-policy-format) : REAP(Resource, Effect, Action, Principal) / (Tài nguyên, Hiệu ứng, Hành động, Chủ thể)
    * [Encryption / Mã hóa](#encryption) : (1) SSE-S3, KMS, C (2) Client-side encryption / Mã hóa phía client (3) aws:SecureTransport
1. [Utilities / Tiện ích](#4-utilities)
    * [Event Notifications / Thông báo sự kiện](#event-notifications) : (1) SNS, SQS, Lambda trigger / Kích hoạt SNS, SQS, Lambda (2) AWS EventBridge
    * [Access Points / Điểm truy cập](#access-points) : (1) point-by-point policy, DNS name / chính sách từng điểm, tên DNS (2) Object lambda / Lambda đối tượng

## 1. Backup / Sao lưu

Protecting data from potential loss is essential for compliance, so this topic shold not be neglected. / Việc bảo vệ dữ liệu khỏi nguy cơ mất mát là điều bắt buộc để tuân thủ quy định, do đó chủ đề này không nên bị bỏ qua.

### Versioning / Quản lý phiên bản

Provides easy way to rollback deletion, so prevents unintended data loss. / Cung cấp cách dễ dàng để khôi phục (rollback) việc xóa, do đó ngăn chặn việc mất dữ liệu vô ý.

* **Bucket level / Cấp bucket** : Enabling versioning object by object (X) Every object in the bucket is versioned (O) / Bật quản lý phiên bản cho từng đối tượng (Sai) Mọi đối tượng trong bucket đều được quản lý phiên bản (Đúng)
* **Version ID / ID phiên bản** : When uploading an object with same key, existing object is not deleted by overwriting. Instead, version ID for existing object is created(fyi, initial version ID for any object is null) / Khi tải lên một đối tượng có cùng khóa (key), đối tượng hiện có không bị xóa bởi việc ghi đè. Thay vào đó, ID phiên bản cho đối tượng hiện có sẽ được tạo (lưu ý, ID phiên bản ban đầu của bất kỳ đối tượng nào đều là null)
* **Delete marker / Đánh dấu xóa** : If you delete an object, it just leaves delete marker. Deletion can be undone by deleting this delete marker. However, if you delete a version, this is permanent delete. / Nếu bạn xóa một đối tượng, nó chỉ để lại đánh dấu xóa. Việc xóa có thể được hoàn tác bằng cách xóa đánh dấu xóa này. Tuy nhiên, nếu bạn xóa một phiên bản, đây là thao tác xóa vĩnh viễn.

### Replication / Sao chép

Must enable versioning in both source and target buckets. Target bucket can be a bucket of different account. Replication happens asynchronously(i.e. it takes a few seconds) / Phải bật quản lý phiên bản ở cả bucket nguồn và bucket đích. Bucket đích có thể là bucket của một tài khoản khác. Việc sao chép diễn ra không đồng bộ (ví dụ: mất vài giây)

* **Bucket region / Vùng bucket** : If region of source and target buckets are different, it's cross region replication(CRR), otherwise same region replication / Nếu vùng của bucket nguồn và đích khác nhau, đó là sao chép giữa các vùng (CRR), ngược lại là sao chép cùng vùng (SRR)
    * **CRR** : make copy of a data to prevent potential data loss occur in a region(such fault tolerance is essential for compliance) / Tạo bản sao dữ liệu để ngăn chặn nguy cơ mất dữ liệu xảy ra tại một vùng (khả năng chịu lỗi như vậy là điều bắt buộc để tuân thủ quy định)
    * **SRR** : create a sandbox environment(i.e. dev, staging environment) / Tạo môi trường sandbox (ví dụ: môi trường phát triển, staging)
* **Optional delete marker replication / Sao chép đánh dấu xóa tùy chọn** : Default replication behavior is to ignore any deletion from source bucket. Delete marker can be replicated, but permanent delete is not replicated. / Hành vi sao chép mặc định là bỏ qua mọi lệnh xóa từ bucket nguồn. Đánh dấu xóa có thể được sao chép, nhưng việc xóa vĩnh viễn thì không được sao chép.
* **New objects only / Chỉ đối tượng mới** : Replication starts from the time of replication rule definition. To replicate existing objects in source bucket, use *S3 Batch replication* service. / Việc sao chép bắt đầu từ thời điểm quy tắc sao chép được xác lập. Để sao chép các đối tượng hiện có trong bucket nguồn, hãy sử dụng dịch vụ *S3 Batch replication*.

## 2. Optimizations / Tối ưu hóa

Reliability of S3 storage can be expressed by following measures / Độ tin cậy của lưu trữ S3 có thể được thể hiện qua các chỉ số sau

* Durability : chance of object stored in S3 preserved(11 9s guranteed; 99.999...99%) / Độ bền: xác suất đối tượng được lưu trữ trong S3 được bảo toàn (đảm bảo 11 số 9; 99.999...99%)
* Availability : measure of readiness of service(differs across different storage classes) / Tính sẵn sàng: thước đo mức độ sẵn sàng của dịch vụ (khác nhau tùy thuộc vào các lớp lưu trữ khác nhau)

### Cost Optimization / Tối ưu hóa chi phí

Depending on access frequency, user can select object-by-object storage class among following table. / Tùy thuộc vào tần suất truy cập, người dùng có thể chọn lớp lưu trữ cho từng đối tượng trong bảng sau.

|                | standard | standard-IA | Glacier Instant Retrieval | Glacier Flexible Retrieval                   | Glacier Deep Archive                       |
|----------------|----------|-------------|---------------------------|----------------------------------------------|--------------------------------------------|
| Storage Cost / Chi phí lưu trữ   | 1        | 0.5         | 0.17                      | 0.17                                         | 0.04                                       |
| Retrieval Cost / Chi phí truy xuất | 1        | 25          | 250                       | 1                                            | 1                                          |
| Retrieval Time / Thời gian truy xuất | 0        | 0           | 0                         | Expedited(<=5m), Standard(<=5h), Bulk(<=12h) / Nhanh(<=5p), Tiêu chuẩn(<=5h), Hàng loạt(<=12h) | Expedited(x), Standard(<=12h), Bulk(<=48h) / Nhanh(không hỗ trợ), Tiêu chuẩn(<=12h), Hàng loạt(<=48h) | 

* Combined with versioning, glacier classes can be optimal choice for noncurrent level version storage. / Kết hợp với quản lý phiên bản, các lớp Glacier có thể là lựa chọn tối ưu cho việc lưu trữ các phiên bản không còn hiện hành (noncurrent).
* **Intelligent tiering / Phân lớp thông minh** : Used to automatically shift storage class from standard -> IA(30 days) -> glacier(90 days) based on *latest access date*, with additional cost for object monitoring / Được sử dụng để tự động chuyển đổi lớp lưu trữ từ standard -> IA (30 ngày) -> glacier (90 ngày) dựa trên *ngày truy cập gần nhất*, kèm theo chi phí bổ sung cho việc giám sát đối tượng
* **Lifecycle rule / Quy tắc vòng đời** : Manual rule applied on every object in bucket to shift storage class or delete object based on object *creation date*. / Quy tắc thủ công được áp dụng cho mọi đối tượng trong bucket để chuyển đổi lớp lưu trữ hoặc xóa đối tượng dựa trên *ngày tạo* đối tượng.

### Performance Optimization / Tối ưu hóa hiệu suất

* S3 Gurantees at least 3,500 PUT/POST/DELETE/COPY API response per second for each prefix / S3 Đảm bảo ít nhất 3,500 phản hồi API PUT/POST/DELETE/COPY mỗi giây cho mỗi tiền tố (prefix)
    * **multi part upload / tải lên đa phần** : divide big file into small sub-parts and upload each in parallel(recommended for 100MB+ file, required for 5GB+ file) / chia tệp lớn thành các phần nhỏ hơn và tải lên từng phần một cách song song (khuyến nghị cho tệp từ 100MB+, bắt buộc cho tệp từ 5GB+)
    * **transfer acceleration / tăng tốc truyền tải** : useful when uploading file to bucket in different region(compatible with multi part upload); first upload file into edge location in the same region, then transfer the data to target bucket using private AWS network / hữu ích khi tải tệp lên bucket ở vùng khác (tương thích với tải lên đa phần); đầu tiên tải tệp lên vị trí edge (edge location) trong cùng vùng, sau đó truyền dữ liệu đến bucket đích bằng mạng riêng của AWS
* S3 Gurantees at least 5,500 GET/HEAD API response per second for each prefix / S3 Đảm bảo ít nhất 5,500 phản hồi API GET/HEAD mỗi giây cho mỗi tiền tố
    * **Byte range fetches / Truy xuất theo phạm vi byte** : parallelize GET request by requesting specific byte range in parallel / Song song hóa yêu cầu GET bằng cách yêu cầu các phạm vi byte cụ thể cùng lúc

## 3. Security / Bảo mật

Regardless of security settings enabled by bucket policy, if *Block public access* is enabled, any publicly opened roles or polices are ignored. This is a additional safety layer that prevents potential data leak which can be caused by mistakes. / Bất kể các cài đặt bảo mật được bật bởi bucket policy, nếu tính năng *Block public access* (Chặn truy cập công khai) được bật, mọi vai trò hoặc chính sách mở công khai đều sẽ bị bỏ qua. Đây là một lớp bảo vệ bổ sung ngăn chặn rò rỉ dữ liệu tiềm ẩn có thể do sai sót gây ra.

### JSON Policy Format / Định dạng Chính sách JSON

[AWS Policy generator](https://awspolicygen.s3.amazonaws.com/policygen.html) can be used to generate JSON string that contains following items to specify permitted IAM user, role and corresponding actions. / [Trình tạo chính sách AWS](https://awspolicygen.s3.amazonaws.com/policygen.html) có thể được sử dụng để tạo chuỗi JSON chứa các mục sau để chỉ định người dùng IAM, vai trò và các hành động tương ứng được phép.

* **Resource / Tài nguyên** : bucket or objects specified by ARN / bucket hoặc các đối tượng được chỉ định bởi ARN
* **Effect / Hiệu ứng** : Allow / Deny / Cho phép / Từ chối
* **Action / Hành động** : API action to allow or deny(ex. s3:GetObject) / Hành động API để cho phép hoặc từ chối (ví dụ: s3:GetObject)
* **Principal / Chủ thể** : account or user to apply policy / tài khoản hoặc người dùng để áp dụng chính sách

### Encryption / Mã hóa

Can be categorized into two items depending on who does the encryption and decryption: Server-side(SSE) and client side. Can set IAM policy to enforce certain encryption/decryption option(ex. [SSE-KMS policy](https://docs.aws.amazon.com/AmazonS3/latest/userguide/UsingKMSEncryption.html#require-sse-kms)). / Có thể được phân thành hai loại tùy thuộc vào ai thực hiện mã hóa và giải mã: Phía máy chủ (SSE) và phía máy khách (client-side). Có thể đặt chính sách IAM để thực thi bắt buộc một tùy chọn mã hóa/giải mã nhất định (ví dụ: [Chính sách SSE-KMS](https://docs.aws.amazon.com/AmazonS3/latest/userguide/UsingKMSEncryption.html#require-sse-kms)).

* **SSE-S3** : (Default) Uses encryption key that AWS owns and manages / (Mặc định) Sử dụng khóa mã hóa do AWS sở hữu và quản lý
* **SSE-KMS**
    * Encryption key stored in KMS(Key MAnagement Service) is used for encryption/decryption. / Khóa mã hóa được lưu trữ trong KMS (Dịch vụ Quản lý Khóa) được sử dụng để mã hóa/giải mã.
    * KMS has its own usage limit for each region, so usage can be limited by this quota. / KMS có giới hạn sử dụng riêng cho từng vùng, do đó mức sử dụng có thể bị giới hạn bởi hạn mức này.
* **SSE-C**
    * Encryption key is delievered within PUT request header, and never stored in AWS. / Khóa mã hóa được gửi đi trong header của yêu cầu PUT, và không bao giờ được lưu trữ trên AWS.
    * Same key has to be provided when sending GET request to the S3. / Cùng một khóa đó phải được cung cấp khi gửi yêu cầu GET tới S3.
* **Client-side Encryption / Mã hóa phía Client** : User encrypts/decrypts the data by himself and upload/download the data to/from S3 / Người dùng tự mã hóa/giải mã dữ liệu và tự tải lên/tải xuống dữ liệu tới/từ S3
* **aws:SecureTransport** : User can set IAM policy to deny PUT request that uses unsafe HTTP protocol. / Người dùng có thể đặt chính sách IAM để từ chối yêu cầu PUT sử dụng giao thức HTTP không an toàn.

## 4. Utilities / Tiện ích

### Event Notifications / Thông báo sự kiện

Used to trigger designated operation when certain S3 API is called(ex. s3:ObjectCreated). / Được sử dụng để kích hoạt một thao tác được chỉ định khi một API S3 nhất định được gọi (ví dụ: s3:ObjectCreated).

* Takes few seconds to minutes for notice to be generated. / Mất từ vài giây đến vài phút để thông báo được tạo ra.
* Setting proper IAM SNS, SQS, Lambda resource policy on S3 bucket is required. / Yêu cầu thiết lập chính sách tài nguyên IAM phù hợp cho SNS, SQS, Lambda trên bucket S3.
* Or, user can utilize AWS EventBridge to trigger 18+ AWS service as destination. / Hoặc, người dùng có thể sử dụng AWS EventBridge để kích hoạt hơn 18 dịch vụ AWS làm đích đến.

### Access Points / Điểm truy cập

Unlike IAM policy which is basically subject(principal) based statement, access point and corresponding access point policy enables object(resource) based control. This enables more granular control over the objects, which would have only possible by creating multiple buckets. / Khác với chính sách IAM về cơ bản là một câu lệnh dựa trên chủ thể (principal), điểm truy cập và chính sách điểm truy cập tương ứng cho phép kiểm soát dựa trên đối tượng (tài nguyên). Điều này cho phép kiểm soát chi tiết hơn đối với các đối tượng, điều mà trước đây chỉ có thể thực hiện được bằng cách tạo nhiều bucket.

* **point by point policy / chính sách từng điểm** : Similar to bucket policy, access point policy sets the list of allowed action on the object with the designated policy. / Tương tự như bucket policy, chính sách điểm truy cập thiết lập danh sách các hành động được phép thực hiện trên đối tượng với chính sách được chỉ định.
* **point by point DNS name / tên DNS từng điểm** : Data with certain prefix can only be accessed through specific network paths, therefore provides more isolated S3 environment for each team or workflow. / Dữ liệu với một tiền tố nhất định chỉ có thể được truy cập thông qua các đường dẫn mạng cụ thể, do đó cung cấp môi trường S3 cách ly hơn cho từng nhóm hoặc quy trình làm việc.
* **object lambda / lambda đối tượng** : Define AWS Lambda function to transform the object before it is retrieved by the caller application, while application retrieves the transformed object from Lambda access point. / Xác định hàm AWS Lambda để chuyển đổi đối tượng trước khi nó được truy xuất bởi ứng dụng gọi, trong khi ứng dụng truy xuất đối tượng đã được chuyển đổi từ điểm truy cập Lambda.
