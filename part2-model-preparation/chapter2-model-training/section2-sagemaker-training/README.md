Dưới đây là bản dịch song ngữ Anh - Việt cho tài liệu của bạn:

# Model Training on SageMaker / Huấn luyện Mô hình trên SageMaker

Successful model training consists of Preprocess, Training and Tuning jobs. Sagemaker provides management tools for these jobs, whose workflow can be summarized as below.
Việc huấn luyện mô hình thành công bao gồm các tác vụ Tiền xử lý (Preprocess), Huấn luyện (Training) và Tinh chỉnh (Tuning). SageMaker cung cấp các công cụ quản lý cho các tác vụ này, quy trình của chúng có thể được tóm tắt như sau.

1. Raw data in S3 + Processing code image in ECR -> S3 URL of converted training data(ex. protobuf)
1. Dữ liệu thô trong S3 + Image mã tiền xử lý trong ECR -> URL S3 của dữ liệu huấn luyện đã chuyển đổi (vd: protobuf)

2. Training data in S3 + Training code image in ECR -> Sagemaker Training(model artifact saved in S3)
2. Dữ liệu huấn luyện trong S3 + Image mã huấn luyện trong ECR -> Huấn luyện SageMaker (các thành phần mô hình được lưu trong S3)

3. Model artifact in S3 + persistent endpoint -> make individual predictions
3. Thành phần mô hình trong S3 + endpoint cố định (persistent endpoint) -> đưa ra các dự đoán riêng lẻ

4. Model artifact in S3 + Sagemaker Batch Transform -> batch inference
4. Thành phần mô hình trong S3 + Sagemaker Batch Transform -> suy luận theo lô (batch inference)

This section covers list of service user can use with different complexity level.
Phần này bao gồm danh sách các dịch vụ người dùng có thể sử dụng với các mức độ phức tạp khác nhau.

1. [SageMaker Canvas](#sagemaker-canvas) : modeling by drag-and-drop
1. [SageMaker Canvas](#sagemaker-canvas) : tạo mô hình bằng thao tác kéo-thả

2. [SageMaker Autopilot](#sagemaker-autopilot) : automatic training/tuning for tabular data
2. [SageMaker Autopilot](#sagemaker-autopilot) : tự động huấn luyện/tinh chỉnh cho dữ liệu dạng bảng

3. [SageMaker Jumpstart](#sagemaker-jumpstart) : pretrained/foundation model aggregator
3. [SageMaker Jumpstart](#sagemaker-jumpstart) : trình tổng hợp các mô hình được huấn luyện trước/mô hình nền tảng

4. [SageMaker Training Job](#sagemaker-training-job) : most flexible way to train a custom model
4. [SageMaker Training Job](#sagemaker-training-job) : cách linh hoạt nhất để huấn luyện một mô hình tùy chỉnh

    * [SageMaker Input Mode](#sagemaker-input-mode)
    * [Chế độ đầu vào SageMaker](#sagemaker-input-mode)

    * [SageMaker Model Registry](#sagemaker-model-registry)
    * [Sổ đăng ký mô hình SageMaker](#sagemaker-model-registry)

    * [Automatic Model Tuning](#automatic-model-tuningamt)
    * [Tinh chỉnh Mô hình Tự động](#automatic-model-tuningamt)

    * [Monitoring](#monitoring)
    * [Giám sát](#monitoring)

    * [Distributed Training](#distributed-training)
    * [Huấn luyện Phân tán](#distributed-training)

---

## SageMaker Canvas

No-code interface for building ML models targeting non-technical users including business analysts to return prediction value.
Giao diện không cần mã (no-code) để xây dựng các mô hình ML dành cho người dùng phi kỹ thuật, bao gồm cả các nhà phân tích kinh doanh nhằm trả về giá trị dự đoán.

* **highest simplicity** : user can train a model only by drag-and-drop
* **Đơn giản nhất** : người dùng có thể huấn luyện mô hình chỉ bằng thao tác kéo-thả

* **lowest flexibility** : no room for customization or parameter tuning(once result returned, that's it)
* **Ít linh hoạt nhất** : không có không gian để tùy chỉnh hoặc tinh chỉnh tham số (một khi kết quả đã trả về, thì như vậy thôi)

* **supported tasks** : regression, single/multi class classification on tabular data, forecasting and text, image classification
* **Các tác vụ được hỗ trợ** : hồi quy, phân loại đơn/đa lớp trên dữ liệu dạng bảng, dự báo và phân loại văn bản, hình ảnh

---

## SageMaker Autopilot

This is AutoML tool which figures out the best model that most generalizes data by itself. It is applicable when 
Đây là công cụ AutoML tự tìm ra mô hình tốt nhất có khả năng tổng hợp hóa dữ liệu nhiều nhất. Nó có thể áp dụng khi

1. problem to solve is either classification or regression
1. bài toán cần giải quyết là phân loại hoặc hồi quy

2. data is in tabular format(therefore, it is stored in S3 as CSV or Parquet format)
2. dữ liệu ở định dạng bảng (do đó, nó được lưu trữ trong S3 dưới định dạng CSV hoặc Parquet)

3. model choice can be restricted to Linear Learner, XGBoost, Deep Learning, Ensemble.
3. lựa chọn mô hình có thể bị giới hạn ở Linear Learner, XGBoost, Deep Learning, Ensemble.

Then, given single target value, it finds best model and hyperparameter set and returns:
Sau đó, khi cung cấp một giá trị mục tiêu đơn lẻ, nó sẽ tìm mô hình và bộ siêu tham số tốt nhất và trả về:

1. model notebook : used for further model refination by data scientists or ML engineer
1. model notebook : được sử dụng để tinh chỉnh mô hình thêm bởi các nhà khoa học dữ liệu hoặc kỹ sư ML

2. model leaderboard : to provide information on how the model is selected compared to other alternatives
2. bảng xếp hạng mô hình (model leaderboard) : cung cấp thông tin về cách mô hình được chọn so với các phương án thay thế khác

This solution is integrated with *SageMaker Clarify* which provides feature importance report using SHAP value.
Giải pháp này được tích hợp với *SageMaker Clarify*, công cụ cung cấp báo cáo mức độ quan trọng của đặc trưng (feature importance) bằng cách sử dụng giá trị SHAP.

---

## SageMaker Jumpstart

Next simplest way to train a model is to use notebook environment. Using *SageMaker Studio*, user can select instance type, install everything inside the notebook(safe to say that this is Google colab equivalent of AWS). Combined with *SageMaker Jumpstart*, which provide suite of pretrained (foundational) models for various task including text (encoding/generation/summarization) and image (encoding/segmentation), user can either use the pretrained model as is or tune the model seamlessly. Model comes from various source including Huggingface, Stability AI and Amazon(ex. Titan, Alexa).
Cách đơn giản tiếp theo để huấn luyện mô hình là sử dụng môi trường notebook. Sử dụng *SageMaker Studio*, người dùng có thể chọn loại instance, cài đặt mọi thứ bên trong notebook (có thể nói đây là phiên bản tương đương Google Colab của AWS). Kết hợp với *SageMaker Jumpstart*, nơi cung cấp một bộ các mô hình được huấn luyện trước (mô hình nền tảng) cho nhiều tác vụ khác nhau bao gồm văn bản (mã hóa/tạo bản tóm tắt) và hình ảnh (mã hóa/phân vùng), người dùng có thể sử dụng mô hình được huấn luyện trước như hiện tại hoặc tinh chỉnh mô hình một cách liền mạch. Các mô hình đến từ nhiều nguồn khác nhau bao gồm Huggingface, Stability AI và Amazon (vd: Titan, Alexa).

---

## SageMaker Training Job

Lowest level for model training is to
Cấp độ thấp nhất để huấn luyện mô hình là

1. configure running environment using Docker image
1. cấu hình môi trường chạy bằng Docker image

2. let SageMaker run the code within configured type of instance by creating Training job. 
2. để SageMaker chạy mã bên trong loại instance đã cấu hình bằng cách tạo Training job.

Since this provides most flexible way to train a model, there are various options to consider.
Vì cách này cung cấp phương pháp linh hoạt nhất để huấn luyện mô hình, nên có nhiều tùy chọn khác nhau cần xem xét.

### Sagemaker Input mode

There are several options to feed data during training job.
Có một số tùy chọn để nạp dữ liệu trong quá trình huấn luyện.

* S3 based : can optimize performance using *S3 express one zone*.
* Dựa trên S3 : có thể tối ưu hóa hiệu suất bằng cách sử dụng *S3 express one zone*.

    * **S3 File Mode** : (default) copies data into Docker container(has to wait for copy process, and container image gets heavy)
    * **Chế độ Tệp S3 (S3 File Mode)** : (mặc định) sao chép dữ liệu vào Docker container (phải đợi quá trình sao chép, và image container sẽ trở nên nặng hơn)

    * **S3 Fast File Mode** : no copy but feed data as stream(works best with sequential data access; not random access)
    * **Chế độ Tệp Nhanh S3 (S3 Fast File Mode)** : không sao chép mà nạp dữ liệu dưới dạng luồng (hoạt động tốt nhất với truy cập dữ liệu tuần tự; không phải truy cập ngẫu nhiên)

* Network storage based : requires VPC for data transfer
* Dựa trên bộ nhớ mạng : yêu cầu VPC để truyền dữ liệu

    * **FSx Lustre** : suitable for training job that requires low latency with 100GB+ throughput in a single AZ
    * **FSx Lustre** : phù hợp với tác vụ huấn luyện yêu cầu độ trễ thấp với thông lượng hơn 100GB trong một AZ (vùng sẵn sàng) duy nhất

    * **EFS** : when data is stored in EFS
    * **EFS** : khi dữ liệu được lưu trữ trong EFS

### SageMaker Model Registry

Used to generate catalog of models and manage their versions(i.e. associate metadata with models). This is especially useful for workflow to centralize the model management workplace and share, deploy(with CI/CD) models easily. Can be integrated with MLFlow.
Được sử dụng để tạo danh mục mô hình và quản lý các phiên bản của chúng (tức là liên kết siêu dữ liệu với các mô hình). Điều này đặc biệt hữu ích cho quy trình làm việc nhằm tập trung hóa không gian quản lý mô hình và chia sẻ, triển khai (với CI/CD) các mô hình một cách dễ dàng. Có thể tích hợp với MLFlow.

* ex) trained model -> model registry -> event bridge -> lambda function(email notification to administrator) -> model approved -> API gateway -> lambda function(update parameter store) -> inference pipeline enabled with the new model
* vd) mô hình đã huấn luyện -> sổ đăng ký mô hình -> event bridge -> hàm lambda (thông báo email cho quản trị viên) -> mô hình được phê duyệt -> API gateway -> hàm lambda (cập nhật parameter store) -> quy trình suy luận được kích hoạt với mô hình mới

### Automatic Model Tuning(AMT)

Given list of hyperparameters and grid of its possible values, this searches the best combination using one of following approaches:
Đưa ra danh sách các siêu tham số và lưới các giá trị có thể có, công cụ này tìm kiếm sự kết hợp tốt nhất bằng một trong các cách tiếp cận sau:

* **grid search** : limited to categorical parameters(brute forces every possible combination; worst solution)
* **tìm kiếm theo lưới (grid search)** : giới hạn ở các tham số phân loại (thử toàn lực mọi kết hợp có thể; giải pháp tệ nhất)

* **random search** : among possible choices, select random parameter set
* **tìm kiếm ngẫu nhiên (random search)** : trong số các lựa chọn có thể, chọn ngẫu nhiên bộ tham số

* **bayesian optimization** : regression problem on metric to optimize(learns as it goes, so parallel learning is disabled)
* **tối ưu hóa Bayes (bayesian optimization)** : bài toán hồi quy trên chỉ số cần tối ưu hóa (học theo thời gian, do đó việc học song song bị vô hiệu hóa)

* **hyperband** : appropriate for algorithms that learns iteratively along number of epochs(i.e most DL models). faster than BO or RS
* **hyperband** : phù hợp với các thuật toán học lặp đi lặp lại theo số epoch (tức là hầu hết các mô hình DL). nhanh hơn BO hoặc RS

Few tips to keep in mind:
Một vài mẹo cần lưu ý:

* Don't optimize too many hypermarameters at once(magnitude of trial to expolit grows exponentially)
* Đừng tối ưu hóa quá nhiều siêu tham số cùng một lúc (quy mô các lần thử nghiệm để khai thác tăng theo cấp số nhân)

* Don't run too many tuning jobs concurrently, because learning happens sequentially.
* Đừng chạy quá nhiều tác vụ tinh chỉnh cùng lúc, vì quá trình học diễn ra theo trình tự.

* Limit the possible value ranges to as small(or, realistic) as possible to avoid unnecessary trial
* Giới hạn phạm vi giá trị có thể ở mức nhỏ nhất (hoặc thực tế) có thể để tránh các lần thử không cần thiết

### Monitoring

* **health checking** : enabled by default when using `ml.p` or `ml.g` instance types to ensure NCCL works properly in every instance within cluster
* **kiểm tra sức khỏe (health checking)** : được bật theo mặc định khi sử dụng các loại instance `ml.p` hoặc `ml.g` để đảm bảo NCCL hoạt động bình thường trong mọi instance thuộc cụm

* **Sagemaker Debugger** : `smdebug` library provides integrated APIs to define conditions to make alarm via CloudWatch
* **Sagemaker Debugger** : thư viện `smdebug` cung cấp các API tích hợp để xác định các điều kiện nhằm tạo cảnh báo qua CloudWatch

* **Tensorboard Integration** : requires some modification within training script as [written](https://docs.aws.amazon.com/sagemaker/latest/dg/tensorboard-on-sagemaker.html)
* **Tích hợp Tensorboard** : đòi hỏi một số chỉnh sửa trong tập lệnh huấn luyện như đã [viết](https://docs.aws.amazon.com/sagemaker/latest/dg/tensorboard-on-sagemaker.html)

### Distributed Training

Note: before moving out to creating cluster of multiple instances, always max out number of gpus in single instance first to prevent overhead from inter-instance communication
Lưu ý: trước khi chuyển sang việc tạo cụm gồm nhiều instance, hãy luôn tối đa hóa số lượng gpu trong một instance đơn lẻ trước để ngăn chặn độ trễ (overhead) từ giao tiếp giữa các instance

* job parallelism : running multiple jobs in parallel
* tính song song hóa tác vụ (job parallelism) : chạy nhiều tác vụ song song

* data parallelism : to distribute training data into multiple instances within cluster
* tính song song hóa dữ liệu (data parallelism) : phân phối dữ liệu huấn luyện vào nhiều instance trong cụm

    * SageMaker Distributed Data Parallelism library implements inter-instance communication
    * Thư viện SageMaker Distributed Data Parallelism triển khai giao tiếp giữa các instance

    * set backend of torch.distributed.init_process_group as 'smddp'
    * đặt backend của torch.distributed.init_process_group thành 'smddp'

    * however, working with other distributed training libraries are just fine(ex. pytorch ddp, torchrun, deepspeed etc)
    * tuy nhiên, việc làm việc với các thư viện huấn luyện phân tán khác cũng hoàn toàn ổn (vd: pytorch ddp, torchrun, deepspeed, v.v.)

* model parallelism : when model doesn't fit into single instance so has to be loaded on aggregated resources
* tính song song hóa mô hình (model parallelism) : khi mô hình không vừa với một instance đơn lẻ nên phải được tải trên các tài nguyên tổng hợp

    * Sagemaker MPP library for Pytorch provides functionalities including
    * Thư viện Sagemaker MPP cho Pytorch cung cấp các chức năng bao gồm

        * optimization state sharding : weights, gradients anything that stateful optimizer(ex. Adam) holds are sharded
        * phân mảnh trạng thái tối ưu hóa (optimization state sharding) : trọng số, gradient, bất cứ thứ gì mà trình tối ưu hóa có trạng thái (vd: Adam) nắm giữ đều được phân mảnh

        * activation checkpointing : trick to reduce memory usage by clearning recorded activation value of some layers, and recomputing the during a backward process
        * kiểm tra điểm kích hoạt (activation checkpointing) : thủ thuật giảm mức sử dụng bộ nhớ bằng cách xóa giá trị kích hoạt đã ghi của một số lớp, và tính toán lại chúng trong quá trình backward

        * activation offloading : swaps checkpointed activations in a microbatch to/from the cpu
        * dỡ bỏ kích hoạt (activation offloading) : hoán đổi các kích hoạt đã được kiểm tra điểm (checkpointed) trong một microbatch đến/từ cpu

    * for more information, see [documents](https://sagemaker.readthedocs.io/en/stable/api/training/sdp_versions/latest/smd_data_parallel_pytorch.html)
    * để biết thêm thông tin, xem [tài liệu](https://sagemaker.readthedocs.io/en/stable/api/training/sdp_versions/latest/smd_data_parallel_pytorch.html)

### Other extra keywords / Các từ khóa bổ sung khác

* warm pool : maintain training resources to reduce provisioning time
* warm pool (nhóm giữ nhiệt) : duy trì tài nguyên huấn luyện để giảm thời gian cấp phát

    * can be turned on when defining sagemaker training job(configure *KeepAlivePeriodInSeconds*)
    * có thể được bật khi xác định tác vụ huấn luyện sagemaker (cấu hình *KeepAlivePeriodInSeconds*)

    * requires service limit increase request in advance -> then use persistent cache to store data across training jobs in combination
    * yêu cầu yêu cầu tăng giới hạn dịch vụ trước -> sau đó sử dụng bộ đệm cố định (persistent cache) để lưu trữ dữ liệu xuyên suốt các tác vụ huấn luyện kết hợp

* Elastic Fabric Adapter(EFA) : network device attached to the sagemaker GPU instances by making better use of the bandwidth
* Elastic Fabric Adapter(EFA) : thiết bị mạng gắn liền với các instance GPU của sagemaker bằng cách tận dụng băng thông tốt hơn
