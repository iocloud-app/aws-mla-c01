Dưới đây là bản dịch song ngữ Anh/Vie cho văn bản của bạn:


# Feature Engineering Techniques / Các kỹ thuật Kỹ nghệ đặc trưng

Feature engineering can be defined as process of applying domain knowledge on data by transforming data or adding/deleting/selecting attributes. This process is essential, since it is not wise to pour everything into the piepline and expect somehow good results to come out. As a result, this is an important discriminator of proficient and plain practitioner.
Kỹ nghệ đặc trưng có thể được định nghĩa là quá trình áp dụng kiến thức chuyên môn vào dữ liệu bằng cách biến đổi dữ liệu hoặc thêm/xóa/lựa chọn các thuộc tính. Quá trình này là cực kỳ cần thiết, vì sẽ rất khôn ngoan khi đổ mọi thứ vào pipeline và kỳ vọng kết quả tốt sẽ tự động xuất hiện (nguyên văn: sẽ không khôn ngoan khi đổ mọi thứ...). Do đó, đây là yếu tố phân biệt quan trọng giữa một người thực hành xuất sắc và một người thực hành tầm thường.

## The curse of dimensionality / Lời nguyền chiều dữ liệu

Any row of data can be represented as a real vector. As dimension of this vector increases, the vector becomes more flexible and can represent more diverse pattern. However, this implies that the case where ML algorithm has to seek for the pattern would also increase. In such sparse data, most ML algorithm tends to fit suboptimal pattern, therefore it is always wise to boil down the features to the set which matters the most.
Bất kỳ hàng dữ liệu nào cũng có thể được biểu diễn dưới dạng một vector thực. Khi chiều của vector này tăng lên, vector trở nên linh hoạt hơn và có thể biểu diễn nhiều mẫu đa dạng hơn. Tuy nhiên, điều này cũng ngụ ý rằng không gian mà thuật toán Học máy (ML) phải tìm kiếm mẫu cũng sẽ tăng lên. Trong các dữ liệu thưa thớt như vậy, hầu hết các thuật toán ML có xu hướng khớp với các mẫu chưa tối ưu (suboptimal), do đó luôn là khôn ngoan nếu ta rút gọn các đặc trưng xuống tập hợp những đặc trưng quan trọng nhất.

## Imputinig missing data / Quy điển (điền khuyết) dữ liệu bị thiếu

* mean replacement
    * pros : fast and easy(but obvious not the best)
    * cons : distorts the distribution of the column(reduces variance) and disregards the correlation between column
* Thay thế bằng giá trị trung bình (mean replacement)
    * ưu điểm : nhanh và dễ (nhưng rõ ràng là không phải tốt nhất)
    * nhược điểm : làm sai lệch phân phối của cột (làm giảm phương sai) và bỏ qua mối tương quan giữa các cột

* dropping
    * pros : reasonable when the ratio of missing data is low
    * cons : if 'missing' property contains some meaning, it can cause the loss of information
* Xóa bỏ (dropping)
    * ưu điểm : hợp lý khi tỷ lệ dữ liệu bị thiếu thấp
    * nhược điểm : nếu thuộc tính 'bị thiếu' chứa một ý nghĩa nào đó, nó có thể gây mất mát thông tin

* Preferred alternatives
    * use ML techniques(ex. KNN, MICE, train model where that column is target)
    * just collect more data
* Các lựa chọn thay thế được ưu tiên
    * sử dụng các kỹ thuật ML (ví dụ: KNN, MICE, huấn luyện mô hình với cột đó làm mục tiêu/target)
    * chỉ cần thu thập thêm dữ liệu

## Dealing with unbalanced data / Xử lý dữ liệu mất cân bằng

* threshold modification : reduce false positive and (possibly) increase false negative
* Điều chỉnh ngưỡng (threshold modification) : giảm dương tính giả (false positive) và (có thể) tăng âm tính giả (false negative)

* sampling
    * undersampling : dropping majoirity case
    * oversampling : augmenting minority case
* Lấy mẫu (sampling)
    * lấy mẫu dưới (undersampling) : loại bỏ các trường hợp thuộc nhóm đa số
    * lấy mẫu trên (oversampling) : tăng cường các trường hợp thuộc nhóm thiểu số

* SMOTE(Synthetic Minority Over-sampling TEchnique)
    * yes, it is oversampling technique with machine learning(specifically, KNN)
    * hybrid approach of the two sampling technique above, so more preferred
    * it both generates new samples and undersamples the majority case
* SMOTE (Kỹ thuật Lấy mẫu trên Thiểu số Tổng hợp)
    * đúng vậy, đây là kỹ thuật lấy mẫu trên sử dụng học máy (cụ thể là KNN)
    * phương pháp kết hợp (hybrid) của hai kỹ thuật lấy mẫu ở trên, nên được ưu tiên hơn
    * nó vừa tạo ra các mẫu mới vừa lấy mẫu dưới nhóm đa số

## Other Topics / Các chủ đề khác

* outlier detection : Defined as data whose pattern deviates from the normal pattern significantly(ex. user that reacts extraordinarily diverse item in collaborative filtering). Use standard deviation or box plot to detect outliers.
* Phát hiện giá trị ngoại lai (outlier detection) : Được định nghĩa là dữ liệu có mẫu lệch đáng kể so với mẫu bình thường (ví dụ: người dùng có tương tác với các mục đa dạng bất thường trong lọc cộng tác - collaborative filtering). Sử dụng độ lệch chuẩn hoặc biểu đồ hộp (box plot) để phát hiện giá trị ngoại lai.

* binning : converting numerical data into categorical data representing the interval of that value(effective when there can be innate measurement error; care is required since it drops information)
* Phân nhóm (binning) : chuyển đổi dữ liệu số thành dữ liệu phân loại biểu diễn khoảng của giá trị đó (hiệu quả khi có thể có sai số đo lường cố hữu; cần cẩn thận vì phương pháp này làm mất thông tin)

* transforming : apply functions(ex. square root or logarithm) on numerical data to augment the signal from the feature
* Biến đổi (transforming) : áp dụng các hàm (ví dụ: căn bậc hai hoặc logarit) lên dữ liệu số để khuếch đại tín hiệu từ đặc trưng

* encoding : what deep learing does best
* Mã hóa (encoding) : điều mà học sâu (deep learning) làm tốt nhất

* scaling, normalization, shuffling : you know...
* Chuyển đổi tỷ lệ (scaling), chuẩn hóa (normalization), xáo trộn (shuffling) : bạn biết rồi...
```

*Một số lưu ý nhỏ về thuật ngữ:*
- *Quy điển (Imputation): Trong thống kê, imputation là việc thay thế dữ liệu bị thiếu bằng các giá trị thay thế. Từ "quy điển" hoặc "điền khuyết" đều được dùng trong tiếng Việt.*
- *Lời nguyền chiều dữ liệu (Curse of dimensionality): Đây là một thuật ngữ chuẩn trong Machine Learning.*
- *Lấy mẫu trên/dưới (Oversampling/Undersampling): Cũng là các thuật ngữ chuẩn được dịch trực tiếp trong lĩnh vực xử lý dữ liệu mất cân bằng.*
