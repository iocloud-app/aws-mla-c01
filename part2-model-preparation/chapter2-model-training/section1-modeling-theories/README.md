Dưới đây là bản dịch song ngữ Anh - Việt của tài liệu:

# Modeling Theories / Các Lý thuyết Mô hình hóa

Since deep learning approaches took over ML in practice, this section is also mostly about deep learning architecture components and related metrics.
Kể từ khi các phương pháp học sâu (deep learning) chiếm lĩnh Học máy (ML) trong thực tế, phần này cũng chủ yếu nói về các thành phần kiến trúc của học sâu và các chỉ số liên quan.

1. [Activation Functions](#activation-functions) / [Hàm kích hoạt](#activation-functions) : anti choices and preferred choices / các lựa chọn chống chỉ định và các lựa chọn ưu tiên
1. [Convolutional Neural Network](#convolutional-neural-network) / [Mạng Nơ-ron Tích chập](#convolutional-neural-network) : what convolution means and famous architectures / ý nghĩa của tích chập và các kiến trúc nổi tiếng
1. [Recurrent Neural network](#recurrent-neural-network) / [Mạng Nơ-ron Hồi quy](#recurrent-neural-network) : vanishing gradient problem and recurrent cell / vấn đề gradient biến mất và tế bào hồi quy
1. [Evaluation Metrics](#evaluation-metrics) / [Chỉ số Đánh giá](#evaluation-metrics) : widely used metrics for classification and regression / các chỉ số được sử dụng rộng rãi cho bài toán phân loại và hồi quy

---

## Activation Functions / Hàm kích hoạt

Function used to calculate final input of next layer.
Hàm được sử dụng để tính toán đầu vào cuối cùng của lớp tiếp theo.

* anti-choices : just remind why following functions should not be utilized
* các lựa chọn chống chỉ định : chỉ để nhắc nhở tại sao các hàm sau không nên được sử dụng
    * **linear / tuyến tính** : dummy function which makes neural network a linear function of input / hàm giả (vô dụng) khiến mạng nơ-ron trở thành một hàm tuyến tính của đầu vào
    * **step function / hàm bước** : not differentiable, so gradient cannot be calculated / không khả vi, do đó không thể tính được gradient
    * **sigmoid, logistic** : gradient is nearly zero in most domain values(vanishing gradient) / gradient gần bằng không ở hầu hết các giá trị miền (gradient biến mất)
    * **tanh** : requires relatively expensive computation compared to upcoming ReLU variants(but, used in RNN) / đòi hỏi tính toán khá tốn kém so với các biến thể ReLU ra đời sau (nhưng được sử dụng trong RNN)
* preffered choices : try ReLU -> Leaky ReLU -> PReLU, ELU, SiLU for performance optimization
* các lựa chọn ưu tiên : thử ReLU -> Leaky ReLU -> PReLU, ELU, SiLU để tối ưu hóa hiệu năng
    * **ReLU** : easy computation, but linear in negative domain(dying ReLU problem) / tính toán dễ dàng, nhưng mang tính tuyến tính trong miền âm (vấn đề ReLU chết)
    * **Leaky ReLU** : introduce small negative slope in negative domain / đưa vào một độ dốc âm nhỏ trong miền âm
    * **PReLU**(Parametric ReLU) / (ReLU tham số) : Leaky ReLU whose slope is also learnable parameter / Leaky ReLU có độ dốc cũng là một tham số có thể học được
    * **ELU**(Exponential Linear Unit) / (Đơn vị Tuyến tính Mũ) : linear on positive domain, exponential function on negative domain / tuyến tính trên miền dương, hàm mũ trên miền âm
    * **SiLU**(Sigmoid Linear Unit) / (Đơn vị Tuyến tính Sigmoid) : f(x) = x * sigmoid(x), a.k.a. swish function / f(x) = x * sigmoid(x), hay còn gọi là hàm swish

---

## Convolutional Neural Network / Mạng Nơ-ron Tích chập (CNN)

* *Feature location invariant* network used to find things in data that is might not be in exactly where expected to be. 
* Mạng có tính *bất biến vị trí đặc trưng* được sử dụng để tìm kiếm các đối tượng trong dữ liệu mà có thể không nằm đúng ở vị trí mong đợi.
* It scans the data and find the pattern of interest; word 'Convolution' is a fancy term for a process of breaking a data into little chunks, processing them individually and assembling into a bigger picture.
* Nó quét dữ liệu và tìm ra mẫu quan tâm; từ 'Tích chập' là một thuật ngữ trang trọng cho quá trình chia nhỏ dữ liệu thành nhiều mảnh, xử lý chúng riêng lẻ và lắp ghép lại thành một bức tranh tổng thể.
* This network recognizes edges on the image to notice specific object by repeating Convolution, Pooling, Dropout, Flatten, Dense operations in a model-specific order.
* Mạng này nhận diện các cạnh trong ảnh để chú ý đến đối tượng cụ thể bằng cách lặp đi lặp lại các phép toán Tích chập (Convolution), Gộp (Pooling), Bỏ qua (Dropout), Làm phẳng (Flatten), và Dày đặc (Dense) theo một thứ tự đặc trưng của mô hình.

### some famous architectures / một số kiến trúc nổi tiếng

* **LeNet-5** : got famous through MNIST handwriting recognition challenge / nổi tiếng qua thử thách nhận dạng chữ viết tay MNIST
* **AlexNet** : got famous through ImageNet image classification challenge / nổi tiếng qua thử thách phân loại ảnh ImageNet
* **GoogLeNet** : introduces *inception modules*(which is basically groups of convolution layers) / giới thiệu *mô-đun inception* (về cơ bản là các nhóm lớp tích chập)
* **ResNet** : deeper architecture achieved by *skip-connection* / kiến trúc sâu hơn đạt được nhờ *kết nối tắt (skip-connection)*

---

## Recurrent Neural Network / Mạng Nơ-ron Hồi quy (RNN)

* Operation(or, cell) to project current portion of data into hidden state vector using previous hidden state vector is repeated.
* Phép toán (hoặc tế bào) chiếu phần dữ liệu hiện tại vào vectơ trạng thái ẩn bằng cách sử dụng vectơ trạng thái ẩn trước đó được lặp đi lặp lại.
* Recent behavior tends to have more influence on current time step.
* Hành vi gần đây có xu hướng có nhiều ảnh hưởng hơn đến bước thời gian hiện tại.
* vanishing gradient : occurs when sequence gets longer(even happens with deep CNNs), so RNNs from LSTM adopted *memory cell* to propagate message to next step.
* gradient biến mất : xảy ra khi chuỗi dài hơn (thậm chí còn xảy ra với các CNN sâu), do đó các RNN bắt đầu từ LSTM đã áp dụng *tế bào trí nhớ (memory cell)* để truyền thông điệp sang bước tiếp theo.

### some famous RNN cells / một số tế bào RNN nổi tiếng

* **LSTM Cell / Tế bào LSTM** : maintains separate ideas of short-term and log-term states / duy trì các khái niệm tách biệt về trạng thái ngắn hạn và dài hạn
* **GRU Cell / Tế bào GRU** : simplified LSTM cell that performs as well / tế bào LSTM đơn giản hóa nhưng hoạt động hiệu quả tương đương

---

## Evaluation Metrics / Chỉ số Đánh giá

Essential for monitoring the training process and setting early stopping criteria
Không thể thiếu để theo dõi quá trình huấn luyện và thiết lập tiêu chí dừng sớm (early stopping)

* classification / phân loại
    * accuracy / độ chính xác = (TP + TF) / all / (TP + TF) / tất cả
    * precision / độ chính xác (tìm ra) = TP / (TP + FP)
    * recall / độ thu hồi = TP / (TP + FN)
    * F1 score / điểm F1 = 2 * TP / (2 * TP + FP + FN)
    * AUC = area under the ROC curve(x = FP rate, y = TP rate in various threshold level) / diện tích dưới đường cong ROC (x = tỷ lệ FP, y = tỷ lệ TP ở nhiều mức ngưỡng khác nhau)
    * PR curve(curve of recall(y) by precision(x); area under the curve is used for metric) / đường cong PR (đường cong của recall(y) theo precision(x); diện tích dưới đường cong được dùng làm chỉ số)
* regression / hồi quy
    * RMSE = root mean squared error(average of square root of (y-yhat)^2) / căn bậc hai sai số bình phương trung bình (trung bình của căn bậc hai của (y-yhat)^2)*
    * MAE = mean absolute error(average of |y-yhat|) / sai số tuyệt đối trung bình (trung bình của |y-yhat|)

*(Ghi chú bản dịch: Công thức RMSE trong bản gốc viết là "average of square root of (y-yhat)^2" là một cách diễn đạt không chính xác về mặt toán học. Bản chất RMSE là căn bậc hai của trung bình bình phương sai số, tức sqrt(average of (y-yhat)^2). Bản dịch vẫn giữ nguyên theo bản gốc nhưng lưu ý thêm điểm này).*
