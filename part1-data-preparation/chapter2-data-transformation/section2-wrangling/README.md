Dưới đây là bản dịch sang tiếng Việt cho nội dung của bạn, các thuật ngữ chuyên ngành được giữ nguyên hoặc giải thích rõ để đảm bảo tính chính xác trong bối cảnh AWS/Data Engineering:

# Biến đổi Dữ liệu (Data Wrangling)

## AWS Glue Studio

Ngoài việc là công cụ lập danh mục dữ liệu (cataloging), Glue còn cung cấp chức năng ETL (Trích xuất, Biến đổi, Tải). Dịch vụ này sử dụng Spark bên dưới, nhưng vì đây là dịch vụ serverless (không máy chủ), nên nó không yêu cầu người dùng phải tự quản lý cụm (cluster) Spark.

* **Glue Studio:** giao diện trực quan dựa trên DAG UI dành cho các quy trình làm việc ETL (đây là lựa chọn ưu tiên - *go-to choice* để lên lịch và quản lý việc thực thi các tác vụ xử lý dữ liệu).
* **Glue Data Quality:** tính năng mới nhất của Glue Studio cho phép người dùng xác định các quy tắc tùy chỉnh bằng ngôn ngữ DQDL để phát hiện bất kỳ luồng dữ liệu kém chất lượng nào (kết quả này được sử dụng để làm thất bại tác vụ hoặc báo cáo lên CloudWatch).
* **Glue DataBrew:** giao diện người dùng (UI) dùng để tiền xử lý các tập dữ liệu lớn từ S3 hoặc Kho dữ liệu - DW (có sẵn 250 phép biến đổi - *transformation*). Người dùng tạo các "công thức" (recipes) biến đổi, có thể được lưu lại dưới dạng các tác vụ (jobs) để tái sử dụng.

## SageMaker Data Wrangler

Đây là một quy trình ETL được tích hợp sẵn (*baked in*) bên trong SageMaker Studio. Giống như Glue DataBrew, công cụ này đã có sẵn hơn 300 phép biến đổi dùng cho việc xử lý dữ liệu. Hoặc, người dùng có thể tự định nghĩa các hàm tùy chỉnh bằng pandas, pyspark, v.v. Lưu ý rằng, nó chỉ cung cấp mã code cho phép biến đổi: nó không thực sự trực tiếp thực thi việc biến đổi dữ liệu. Các mã code này được dự định để tích hợp vào các quy trình SageMaker tiếp theo như Processing (Xử lý), Pipelines (Quy trình), v.v.

## EMR

*(Phần này đang trống, EMR là viết tắt của Amazon Elastic MapReduce - dịch vụ đám mây của AWS dùng để xử lý lượng dữ liệu lớn bằng các framework như Apache Spark, Hadoop, v.v.)*
