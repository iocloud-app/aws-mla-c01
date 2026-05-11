Dưới đây là bản dịch song ngữ Anh - Việt, giữ nguyên cấu trúc để bạn dễ dàng đối chiếu:

# Data Wrangling
# Biến đổi Dữ liệu (Data Wrangling)

## AWS Glue Studio

Other than being cataloging tools, Glue it also provides ETL functionality. This uses Spark under the hood, but as this is serverless service, it doesn't require Spark cluster to be managed by user.
Ngoài việc là công cụ lập danh mục dữ liệu (cataloging), Glue còn cung cấp chức năng ETL. Dịch vụ này sử dụng Spark bên dưới, nhưng vì đây là dịch vụ serverless, nên nó không yêu cầu người dùng phải tự quản lý cụm (cluster) Spark.

* Glue Studio : visual interface based on DAG UI for ETL workflows(go-to choice for scheduling and managing the execution of data processing jobs)
* **Glue Studio:** giao diện trực quan dựa trên DAG UI dành cho các quy trình làm việc ETL (đây là lựa chọn ưu tiên - *go-to choice* để lên lịch và quản lý việc thực thi các tác vụ xử lý dữ liệu).

* Glue Data Quality : latest feature of Glue studio that lets users define custom rule using DQDL to detect any low quality data flows in(this result is used to fail the job or report to CloudWatch)
* **Glue Data Quality:** tính năng mới nhất của Glue Studio cho phép người dùng xác định các quy tắc tùy chỉnh bằng ngôn ngữ DQDL để phát hiện bất kỳ luồng dữ liệu kém chất lượng nào (kết quả này được sử dụng để làm thất bại tác vụ hoặc báo cáo lên CloudWatch).

* Glue DataBrew : UI for preprocessing large datasets from S3 or DW(250 ready-made transformation exists). user creates recipes of transformation that can be saved as jobs to be reused.
* **Glue DataBrew:** giao diện người dùng (UI) dùng để tiền xử lý các tập dữ liệu lớn từ S3 hoặc Kho dữ liệu - DW (có sẵn 250 phép biến đổi). Người dùng tạo các "công thức" (recipes) biến đổi, có thể được lưu lại dưới dạng các tác vụ (jobs) để tái sử dụng.

## SageMaker Data Wrangler

It is a ETL pipeline baked in Sagemaker Studio. Like Glue DataBrew, there are already 300+ built-in transformations for data wrangling. Or, user can define own custom functions with pandas, pyspark etc. Note that it is just code of transformation it provides: it does not actually do the data transformation. These codes intended to be implemented into further Sagemaker processes like Processing, Pipelines etc.
Đây là một quy trình ETL được tích hợp sẵn bên trong SageMaker Studio. Giống như Glue DataBrew, công cụ này đã có sẵn hơn 300 phép biến đổi dùng cho việc xử lý dữ liệu. Hoặc, người dùng có thể tự định nghĩa các hàm tùy chỉnh bằng pandas, pyspark, v.v. Lưu ý rằng, nó chỉ cung cấp mã code cho phép biến đổi: nó không thực sự trực tiếp thực thi việc biến đổi dữ liệu. Các mã code này được dự định để tích hợp vào các quy trình SageMaker tiếp theo như Processing, Pipelines, v.v.

## EMR
