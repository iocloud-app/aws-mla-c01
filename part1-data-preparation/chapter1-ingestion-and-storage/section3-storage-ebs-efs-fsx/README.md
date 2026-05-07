Dưới đây là bản dịch song ngữ Anh - Việt cho nội dung của bạn:

# EBS, EFS, FSx topics / Các chủ đề về EBS, EFS, FSx

Unlike S3 which is basically gigantic AWS-managed key-value container of objects, EBS, EFS and FSx can be thought of as a independent file system that user can manage with relatively higher cost.
Khác với S3 về cơ bản là một container key-value khổng lồ do AWS quản lý chứa các đối tượng, EBS, EFS và FSx có thể được coi là một hệ thống tệp độc lập mà người dùng có thể quản lý với chi phí tương đối cao hơn.

1. [AWS EBS](#ebselastic-block-storage)
    * (1) single instance in single AZ (2) persistance (3) provisioned
    * (1) một instance duy nhất trong một AZ (2) tính bền vững (3) được cấp phát
1. [AWS EFS](#efselastic-file-system)
    * (1) multiple instances in multiple AZ (2) storage classes (3) throughput modes
    * (1) nhiều instance trong nhiều AZ (2) các lớp lưu trữ (3) các chế độ thông lượng
1. [AWS FSx](#fsxfile-server)
    * Third party file system integration
    * Tích hợp hệ thống tệp của bên thứ ba


## EBS(Elastic Block Storage)

Think of it as hard drive, because this analogy mostly fits the property of this storage. *Fast detach and attach*. Note that using the attached volume within EC2 instance is another topic covered in [this page](https://docs.aws.amazon.com/ebs/latest/userguide/ebs-using-volumes.html).
Hãy tưởng tượng nó như một ổ cứng, bởi vì sự so sánh này phần lớn phù hợp với đặc tính của loại lưu trữ này. *Gắn và tháo nhanh*. Lưu ý rằng việc sử dụng volume đã gắn trong instance EC2 là một chủ đề khác được đề cập trong [trang này](https://docs.aws.amazon.com/ebs/latest/userguide/ebs-using-volumes.html).

* **single instance in single AZ** : Disk is attached only in one computer, and does not move easily. To move data from EBS to another region, take snapshot of it. However, as of computer, attaching multiple EBS volumes on single instance is available.
* **một instance duy nhất trong một AZ** : Ổ đĩa chỉ được gắn vào một máy tính và không dễ dàng di chuyển. Để chuyển dữ liệu từ EBS sang một region (vùng) khác, hãy tạo ảnh chụp nhanh (snapshot) của nó. Tuy nhiên, giống như máy tính, việc gắn nhiều volume EBS vào một instance duy nhất là có thể.

* **persists after termination** : Disk does not disappear after computer is turned off. However, such persistence can be optionally turned off using *delete on termination* option. This option is applied by default on root EBS but not on additionally attached EBS.
* **bền vững sau khi chấm dứt** : Ổ đĩa không biến mất sau khi máy tính bị tắt. Tuy nhiên, tính bền vững này có thể được tùy chọn tắt đi bằng tùy chọn *xóa khi chấm dứt (delete on termination)*. Tùy chọn này được áp dụng theo mặc định trên EBS gốc (root) nhưng không áp dụng cho các EBS được gắn thêm.

* **provisioned capacity** : Disk on computer have its own fixed capacity in terms of volume and velocity. However, its type can be modified on the fly(i.e. when instance is running), but not in volume-decreasing way. These are types of EBS provisioned by AWS.
* **dung lượng được cấp phát** : Ổ đĩa trên máy tính có dung lượng cố định riêng về cả thể tích và tốc độ. Tuy nhiên, loại của nó có thể được sửa đổi ngay lập tức (tức là khi instance đang chạy), nhưng không thể giảm dung lượng. Dưới đây là các loại EBS do AWS cấp phát.

| name | disk type | performance | use-case |
|------|----------------------|-------------------------|----------------------------------------------|
| tên | loại ổ đĩa | hiệu suất | trường hợp sử dụng |
| io2 | provisioned iops SSD | throughput 16, i/o 16 | mission critical database service |
| io2 | provisioned iops SSD | thông lượng 16, i/o 16 | dịch vụ cơ sở dữ liệu nhiệm vụ trọng yếu |
| io1 | provisioned iops SSD | throughput 4, i/o 4 | i/o intensive NoSQL database |
| io1 | provisioned iops SSD | thông lượng 4, i/o 4 | cơ sở dữ liệu NoSQL chuyên xử lý i/o |
| gp3 | general purpose SSD | throughput 4, i/o 1 | middle-sized RDBMS in production environment |
| gp3 | general purpose SSD | thông lượng 4, i/o 1 | RDBMS quy mô vừa trong môi trường sản xuất |
| gp2 | general purpose SSD | throughput 1, i/o 1 | staging environment transaction workloads |
| gp2 | general purpose SSD | thông lượng 1, i/o 1 | khối lượng công việc giao dịch trong môi trường staging |
| st1 | HDD | throughput 2, i/o 0.03 | large log data ETL server |
| st1 | HDD | thông lượng 2, i/o 0.03 | máy chủ ETL dữ liệu nhật ký lớn |
| sc1 | HDD | throughput 1, i/o 0.015 | cold data storage |
| sc1 | HDD | thông lượng 1, i/o 0.015 | lưu trữ dữ liệu lạnh (cold data) |

## EFS(Elastic File System)

Imagine a hard drive that is lives in the word wide web and its files transferred via network. This system would used Linux's mount functionality using NFS protocol, so *only be used in Linux instance*. Also, as data will be stored in wide wide web, *storage size would scale automatically*. Other properties would include:
Hãy tưởng tượng một ổ cứng tồn tại trên mạng internet toàn cầu và các tệp của nó được truyền tải qua mạng. Hệ thống này sẽ sử dụng chức năng mount (gắn kết) của Linux bằng giao thức NFS, vì vậy *chỉ có thể được sử dụng trong instance Linux*. Ngoài ra, vì dữ liệu sẽ được lưu trữ trên mạng lưới rộng lớn, *kích thước lưu trữ sẽ tự động mở rộng (scale)*. Các thuộc tính khác bao gồm:

* **multiple instances in multiple AZs** : In default *regional* option, after configuring the mount target at specific AZ with appropriate *security group setting(mandatory)*, EFS can be directly connected and mounted to 100+ instances in various AZs. Nevertheless, *one-zone option is also available*.
* **nhiều instance trong nhiều AZ** : Trong tùy chọn *theo region (vùng)* mặc định, sau khi cấu hình mount target (điểm gắn kết) tại một AZ cụ thể với *cài đặt security group (nhóm bảo mật) phù hợp (bắt buộc)*, EFS có thể được kết nối trực tiếp và gắn kết với hơn 100 instance trong nhiều AZ khác nhau. Tuy nhiên, *tùy chọn one-zone (một vùng) cũng có sẵn*.

* **storage classes** : Files in the system can have 3 different storage classes; *standard, EFS-IA* and *Archive*. User can set *lifecycle policy* based on latest access date, so files not accessed for a long time can be moved to cheaper storage classes for efficient budget use.
* **các lớp lưu trữ** : Các tệp trong hệ thống có thể có 3 lớp lưu trữ khác nhau; *standard (tiêu chuẩn), EFS-IA (truy cập không thường xuyên)* và *Archive (lưu trữ)*. Người dùng có thể đặt *chính sách vòng đời (lifecycle policy)* dựa trên ngày truy cập gần nhất, do đó các tệp không được truy cập trong thời gian dài có thể được chuyển sang các lớp lưu trữ rẻ hơn để sử dụng ngân sách hiệu quả.

* **throughput mode** : Since every data is transferred through network, there are three performance optimization options that user can choose based on expected usage.
* **chế độ thông lượng** : Vì mọi dữ liệu đều được truyền qua mạng, có ba tùy chọn tối ưu hóa hiệu suất mà người dùng có thể chọn dựa trên mức sử dụng dự kiến.
    * **Bursting** : if data in the system enlarges, there are more chance for larger throughput will be needed. This mode scales throughput based on the size of data the system holds.
    * **Bursting (Đột biến)** : nếu dữ liệu trong hệ thống phình to, sẽ có nhiều khả năng cần đến thông lượng lớn hơn. Chế độ này mở rộng quy mô thông lượng dựa trên kích thước dữ liệu mà hệ thống giữ.
    * **Enhanced** : In this setting, user can either fix the throughput that user needs(*Provisioned*) or let the system determine the range of throughput the system would need(*Elastic*; default).
    * **Enhanced (Nâng cao)** : Trong cài đặt này, người dùng có thể cố định thông lượng mà mình cần (*Provisioned (Được cấp phát)*) hoặc để hệ thống tự xác định phạm vi thông lượng mà hệ thống cần (*Elastic (Đàn hồi)*; mặc định).

## FSx(File Server)

x in FSx seem to mean unknown variable literally, since this is service for third party file system in AWS. List of supported services and their comparisons is well explained in [this page](https://aws.amazon.com/fsx/when-to-choose-fsx/).
Chữ x trong FSx có vẻ mang nghĩa đen là một biến số ẩn, vì đây là dịch vụ dành cho hệ thống tệp của bên thứ ba trên AWS. Danh sách các dịch vụ được hỗ trợ và so sánh giữa chúng được giải thích rõ ràng trong [trang này](https://aws.amazon.com/fsx/when-to-choose-fsx/).
