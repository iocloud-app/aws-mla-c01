Dưới đây là bản dịch song ngữ Anh/Việt cho tài liệu của bạn:

# AWS Athena on top of AWS Glue
# AWS Athena chạy trên nền tảng AWS Glue

Glue is for table definitions and ETL. Serverless system that manages table definition(schema) automatically. Using Glue, you can give structure to unstructured data. Then, AWS Athena, the serverless SQL query engine for S3 data can be utilized to analyze it. So, it provides good pipeline for ad-hoc queries before actual ETL job definition.
Glue dùng để định nghĩa bảng và ETL (Trích xuất, Chuyển đổi, Tải). Đây là hệ thống serverless (không máy chủ) tự động quản lý định nghĩa bảng (schema). Bằng cách sử dụng Glue, bạn có thể tạo cấu trúc cho dữ liệu phi cấu trúc. Sau đó, AWS Athena - công cụ truy vấn SQL serverless dành cho dữ liệu S3 - có thể được sử dụng để phân tích dữ liệu đó. Vì vậy, nó cung cấp một quy trình (pipeline) tốt cho các truy vấn ad-hoc (thường xuyên/bất thường) trước khi định nghĩa công việc ETL thực tế.

## AWS Glue Crawler
## AWS Glue Crawler (Trình thu thập dữ liệu Glue)

How the data is organized(or, how it is partitioned; which feature will be primarily used to query data) will determine the efficiency of Glue crawling job.
Cách dữ liệu được tổ chức (hoặc cách nó được phân vùng; tính năng nào sẽ được sử dụng chủ yếu để truy vấn dữ liệu) sẽ quyết định hiệu quả của tác vụ thu thập (crawling) của Glue.

* Glue cralwer : scans data in S3 and creates schema
* Glue crawler : quét dữ liệu trong S3 và tạo schema (lược đồ)
* Glue Data Catalog : stores table definition
* Glue Data Catalog : lưu trữ định nghĩa bảng

## Amazon Athena
## Amazon Athena

uses presto under the hood
sử dụng presto bên trong (dưới nền tảng)

* no charge for DDL(e.g. data definition like CREATE, ALTER, DROP), and cost applied by mass of scanned data
* Không tính phí cho DDL (ví dụ: định nghĩa dữ liệu như CREATE, ALTER, DROP), chi phí được tính dựa trên khối lượng dữ liệu được quét
* no need to copy or load data; it stays in S3 in any data format(ex. parquet).
* không cần sao chép hoặc tải dữ liệu; dữ liệu vẫn nằm trong S3 ở bất kỳ định dạng nào (ví dụ: parquet).
* to boost performance...
* để tăng cường hiệu suất...
    * just use columnar storage data like ORC or parquet to save money and get better performance
    * chỉ cần sử dụng dữ liệu lưu trữ theo cột (columnar storage) như ORC hoặc parquet để tiết kiệm chi phí và đạt hiệu suất tốt hơn
    * also, partition data well(small number of large files is better than large number of small files)
    * đồng thời, phân vùng dữ liệu cho tốt (số lượng ít tệp lớn sẽ tốt hơn số lượng lớn các tệp nhỏ)
    * `MSCK REPAIR TABLE` : adding partitions after the data is stored
    * `MSCK REPAIR TABLE` : thêm các phân vùng sau khi dữ liệu đã được lưu trữ
* it sees glue data catalog(including partition), and it automatically creates a table from it so data can be un/semi-structured.
* nó đọc Glue Data Catalog (bao gồm cả phân vùng), và tự động tạo bảng từ đó nên dữ liệu có thể là dạng phi cấu trúc/bán cấu trúc (un/semi-structured).

## Athena Services 
## Các Dịch vụ của Athena

* Athena workgroups : organizes users/teams/apps and control query access and track costs.
* Athena workgroups (nhóm làm việc) : tổ chức người dùng/nhóm/ứng dụng, kiểm soát quyền truy cập truy vấn và theo dõi chi phí.
* CTAS syntax in Athena : CREATE TABLE __ WITH (location, format, compression etc) AS SELECT ...
* Cú pháp CTAS trong Athena : CREATE TABLE __ WITH (vị trí, định dạng, nén, v.v.) AS SELECT ...
* ACID transactions : add `table_type = 'ICEBERG'` to make Athena act as if it is strict RDS
* Giao dịch ACID : thêm `table_type = 'ICEBERG'` để khiến Athena hoạt động như thể nó là một RDS nguyên tắc (strict RDS)
    * using this feature, user can periodically compact data by using `OPTIMIZE table REWRITE DATA USING BIN_PACK` query and improve performance of ACID transactions
    * sử dụng tính năng này, người dùng có thể nén dữ liệu định kỳ bằng cách sử dụng truy vấn `OPTIMIZE table REWRITE DATA USING BIN_PACK` và cải thiện hiệu suất của các giao dịch ACID
