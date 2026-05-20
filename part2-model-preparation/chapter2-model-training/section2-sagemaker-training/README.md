# Model Training on SageMaker | # Đào tạo mô hình trên SageMaker
Successful model training consists of Preprocess, Training and Tuning jobs. Sagemaker provides management tools for these jobs, whose workflow can be summarized as below. | Quá trình đào tạo mô hình thành công bao gồm các công việc Tiền xử lý, Đào tạo và Tinh chỉnh. Sagemaker cung cấp các công cụ quản lý cho các công việc này, quy trình làm việc có thể được tóm tắt như sau.
1. Raw data in S3 + Processing code image in ECR -> S3 URL of converted training data(ex. protobuf) | 1. Dữ liệu thô trong S3 + hình ảnh mã tiền xử lý trong ECR -> URL S3 của dữ liệu đào tạo đã chuyển đổi (ví dụ: protobuf)
1. Training data in S3 + Training code image in ECR -> Sagemaker Training(model artifact saved in S3) | 1. Dữ liệu đào tạo trong S3 + hình ảnh mã đào tạo trong ECR -> Đào tạo Sagemaker (tác phẩm mô hình được lưu trong S3)
1. Model artifact in S3 + persistent endpoint -> make individual predictions | 1. Tác phẩm mô hình trong S3 + điểm cuối cố định -> đưa ra dự đoán riêng lẻ
1. Model artifact in S3 + Sagemaker Batch Transform -> batch inference | 1. Tác phẩm mô hình trong S3 + Chuyển đổi hàng loạt Sagemaker -> suy luận hàng loạt
This section covers list of service user can use with different complexity level. | Phần này bao gồm danh sách các dịch vụ mà người dùng có thể sử dụng với các mức độ phức tạp khác nhau.
1. [SageMaker Canvas](#sagemaker-canvas) : modeling by drag-and-drop | 1. [SageMaker Canvas](#sagemaker-canvas) : mô hình hóa bằng cách kéo thả
1. [SageMaker Autopilot](#sagemaker-autopilot) : automatic training/tuning for tabular data | 1. [SageMaker Autopilot](#sagemaker-autopilot) : đào tạo/tinh chỉnh tự động cho dữ liệu dạng bảng
1. [SageMaker Jumpstart](#sageaker-jumpstart) : pretrained/foundation model aggregator | 1. [SageMaker Jumpstart](#sageaker-jumpstart) : bộ tổng hợp mô hình nền tảng/được huấn luyện trước
1. [SageMaker Training Job](#sagemaker-training-job) : most flexible way to train a custom model | 1. [SageMaker Training Job](#sagemaker-training-job) : cách linh hoạt nhất để đào tạo một mô hình tùy chỉnh
    * [SageMaker Input Mode](#sagemaker-input-mode) |     * [Chế độ đầu vào SageMaker](#sagemaker-input-mode)
    * [SageMaker Model Registry](#sagemaker-model-registry) |     * [Sổ đăng ký mô hình SageMaker](#sagemaker-model-registry)
    * [Automatic Model Tuning](#automatic-model-tuningamt) |     * [Tinh chỉnh mô hình tự động](#automatic-model-tuningamt)
    * [Monitoring](#monitoring) |     * [Giám sát](#monitoring)
    * [Distributed Training](#distributed-training) |     * [Đào tạo phân tán](#distributed-training)
## SageMaker Canvas | ## SageMaker Canvas
No-code interface for building ML models targeting non-technical users including business analysts to return prediction value. | Giao diện không-code để xây dựng mô hình ML dành cho người dùng phi kỹ thuật, bao gồm các nhà phân tích kinh doanh để trả về giá trị dự đoán.
* **highest simplicity** : user can train a model only by drag-and-drop | * **đơn giản nhất** : người dùng có thể đào tạo mô hình chỉ bằng cách kéo thả
* **lowest flexibility** : no room for customization or parameter tuning(once result returned, that's it) | * **linh hoạt nhất** : không có chỗ cho tùy chỉnh hoặc tinh chỉnh tham số (khi kết quả được trả về, thế là xong)
* **supported tasks** : regression, single/multi class classification on tabular data, forecasting and text, image classification | * **nhiệm vụ được hỗ trợ** : hồi quy, phân loại một/nhiều lớp trên dữ liệu dạng bảng, dự báo và văn bản, phân loại hình ảnh
## SageMaker Autopilot | ## SageMaker Autopilot
This is AutoML tool which figures out the best model that most generalizes data by itself. It is applicable when  | Đây là công cụ AutoML tự động tìm ra mô hình tốt nhất tổng quát hóa dữ liệu tốt nhất. Nó được áp dụng khi
1. problem to solve is either classification or regression | 1. vấn đề cần giải quyết là phân loại hoặc hồi quy
1. data is in tabular format(therefore, it is stored in S3 as CSV or Parquet format) | 1. dữ liệu ở dạng bảng (do đó, nó được lưu trữ trong S3 dưới dạng CSV hoặc Parquet)
1. model choice can be restricted to Linear Learner, XGBoost, Deep Learning, Ensemble. | 1. lựa chọn mô hình có thể bị giới hạn ở Linear Learner, XGBoost, Deep Learning, Ensemble.
Then, given single target value, it finds best model and hyperparameter set and returns: | Sau đó, cho một giá trị mục tiêu duy nhất, nó tìm ra mô hình tốt nhất và bộ siêu tham số và trả về:
1. model notebook : used for further model refination by data scientists or ML engineer | 1. sổ tay mô hình : được sử dụng để tinh chỉnh mô hình thêm bởi nhà khoa học dữ liệu hoặc kỹ sư ML
1. model leaderboard : to provide information on how the model is selected compared to other alternatives | 1. bảng xếp hạng mô hình : cung cấp thông tin về cách mô hình được chọn so với các phương án khác
This solution is integrated with *SageMaker Clarify* which provides feature importance report using SHAP value. | Giải pháp này được tích hợp với *SageMaker Clarify* cung cấp báo cáo mức độ quan trọng của đặc trưng bằng giá trị SHAP.
## SageMaker Jumpstart | ## SageMaker Jumpstart
Next simplest way to train a model is to use notebook environment. Using *SageMaker Studio*, user can select instance type, install everything inside the notebook(safe to say that this is Google colab equivalent of AWS). Combined with *SageMaker Jumpstart*, which provide suite of pretrained (foundational) models for various task including text (encoding/generation/summarization) and image (encoding/segmentation), user can either use the pretrained model as is or tune the model seamlessly. Model comes from various source including Huggingface, Stability AI and Amazon(ex. Titan, Alexa). | Cách đơn giản tiếp theo để đào tạo mô hình là sử dụng môi trường sổ tay. Sử dụng *SageMaker Studio*, người dùng có thể chọn loại instance, cài đặt mọi thứ bên trong sổ tay (có thể nói đây là bản tương đương Google colab của AWS). Kết hợp với *SageMaker Jumpstart*, cung cấp bộ mô hình (nền tảng) được huấn luyện trước cho nhiều tác vụ khác nhau bao gồm văn bản (mã hóa/tạo/tóm tắt) và hình ảnh (mã hóa/phân đoạn), người dùng có thể sử dụng mô hình được huấn luyện trước nguyên bản hoặc tinh chỉnh mô hình một cách liền mạch. Mô hình đến từ nhiều nguồn khác nhau bao gồm Huggingface, Stability AI và Amazon (ví dụ: Titan, Alexa).
## SageMaker Training Job | ## SageMaker Training Job
Lowest level for model training is to | Mức thấp nhất để đào tạo mô hình là
1. configure running environment using Docker image | 1. cấu hình môi trường chạy bằng cách sử dụng hình ảnh Docker
1. let SageMaker run the code within configured type of instance by creating Training job. | 1. cho phép SageMaker chạy mã trong loại instance đã cấu hình bằng cách tạo Training job.
Since this provides most flexible way to train a model, there are various options to consider. | Vì điều này cung cấp cách linh hoạt nhất để đào tạo một mô hình, có nhiều tùy chọn để xem xét.
### Sagemaker Input mode | ### Chế độ đầu vào Sagemaker
There are several options to feed data during training job. | Có một số tùy chọn để cung cấp dữ liệu trong quá trình đào tạo.
* S3 based : can optimize performance using *S3 express one zone*. | * Dựa trên S3 : có thể tối ưu hóa hiệu suất bằng cách sử dụng *S3 express one zone*.
    * **S3 File Mode** : (default) copies data into Docker container(has to wait for copy process, and container image gets heavy) |     * **Chế độ tệp S3** : (mặc định) sao chép dữ liệu vào vùng chứa Docker (phải chờ quá trình sao chép và hình ảnh vùng chứa trở nên nặng)
    * **S3 Fast File Mode** : no copy but feed data as stream(works best with sequential data access; not random access) |     * **Chế độ tệp nhanh S3** : không sao chép nhưng cung cấp dữ liệu dưới dạng luồng (hoạt động tốt nhất với truy cập dữ liệu tuần tự; không phải truy cập ngẫu nhiên)
* Network storage based : requires VPC for data transfer | * Dựa trên lưu trữ mạng : yêu cầu VPC để truyền dữ liệu
    * **FSx Lustre** : suitable for training job that requires low latency with 100GB+ throughput in a single AZ |     * **FSx Lustre** : phù hợp cho công việc đào tạo yêu cầu độ trễ thấp với thông lượng 100GB+ trong một AZ duy nhất
    * **EFS** : when data is stored in EFS |     * **EFS** : khi dữ liệu được lưu trữ trong EFS
### SageMaker Model Registry | ### Sổ đăng ký mô hình SageMaker
Used to generate catalog of models and manage their versions(i.e. associate metadata with models). This is especially useful for workflow to centralize the model management workplace and share, deploy(with CI/CD) models easily. Can be integrated with MLFlow. | Được sử dụng để tạo danh mục mô hình và quản lý phiên bản của chúng (tức là liên kết siêu dữ liệu với mô hình). Điều này đặc biệt hữu ích cho quy trình làm việc để tập trung hóa nơi làm việc quản lý mô hình và chia sẻ, triển khai (với CI/CD) mô hình một cách dễ dàng. Có thể được tích hợp với MLFlow.
* ex) trained model -> model registry -> event bridge -> lambda function(email notification to administrator) -> model approved -> API gateway -> lambda function(update parameter store) -> inference pipeline enabled with the new model | * ví dụ) mô hình đã đào tạo -> sổ đăng ký mô hình -> event bridge -> hàm lambda (thông báo email cho quản trị viên) -> mô hình được phê duyệt -> API gateway -> hàm lambda (cập nhật cửa hàng tham số) -> đường ống suy luận được bật với mô hình mới
### Automatic Model Tuning(AMT) | ### Tinh chỉnh mô hình tự động (AMT)
Given list of hyperparameters and grid of its possible values, this searches the best combination using one of following approaches: | Cho danh sách siêu tham số và lưới các giá trị có thể có, việc này tìm kiếm kết hợp tốt nhất bằng cách sử dụng một trong các cách tiếp cận sau:
* **grid search** : limited to categorical parameters(brute forces every possible combination; worst solution) | * **tìm kiếm lưới** : giới hạn ở các tham số phân loại (ép buộc mọi kết hợp có thể; giải pháp tồi nhất)
* **random search** : among possible choices, select random parameter set | * **tìm kiếm ngẫu nhiên** : trong số các lựa chọn có thể, chọn bộ tham số ngẫu nhiên
* **bayesian optimization** : regression problem on metric to optimize(learns as it goes, so parallel learning is disabled) | * **tối ưu hóa bayes** : bài toán hồi quy trên số liệu để tối ưu hóa (học khi đi, vì vậy học song song bị vô hiệu hóa)
* **hyperband** : appropriate for algorithms that learns iteratively along number of epochs(i.e most DL models). faster than BO or RS | * **hyperband** : phù hợp với các thuật toán học lặp đi lặp lại theo số lượng epochs (tức là hầu hết các mô hình DL). Nhanh hơn BO hoặc RS
Few tips to keep in mind: | Một số mẹo cần ghi nhớ:
* Don't optimize too many hypermarameters at once(magnitude of trial to expolit grows exponentially) | * Đừng tối ưu hóa quá nhiều siêu tham số cùng một lúc (số lượng thử nghiệm để khai thác tăng theo cấp số nhân)
* Don't run too many tuning jobs concurrently, because learning happens sequentially. | * Đừng chạy quá nhiều công việc tinh chỉnh đồng thời, vì việc học xảy ra tuần tự.
* Limit the possible value ranges to as small(or, realistic) as possible to avoid unnecessary trial | * Giới hạn các phạm vi giá trị có thể càng nhỏ (hoặc thực tế) càng tốt để tránh thử nghiệm không cần thiết
### Monitoring | ### Giám sát
* **health checking** : enabled by default when using `ml.p` or `ml.g` instance types to ensure NCCL works properly in every instance within cluster | * **kiểm tra sức khỏe** : được bật theo mặc định khi sử dụng các loại instance `ml.p` hoặc `ml.g` để đảm bảo NCCL hoạt động đúng cách trong mọi instance trong cụm
* **Sagemaker Debugger** : `smdebug` library provides integrated APIs to define conditions to make alarm via CloudWatch | * **Sagemaker Debugger** : thư viện `smdebug` cung cấp các API tích hợp để xác định các điều kiện tạo báo động qua CloudWatch
* **Tensorboard Integration** : requires some modification within training script as [written](https://docs.aws.amazon.com/sagemaker/latest/dg/tensorboard-on-sagemaker.html) | * **Tích hợp Tensorboard** : yêu cầu một số sửa đổi trong tập lệnh đào tạo như đã [viết](https://docs.aws.amazon.com/sagemaker/latest/dg/tensorboard-on-sagemaker.html)
### Distributed Training | ### Đào tạo phân tán
Note: before moving out to creating cluster of multiple instances, always max out number of gpus in single instance first to prevent overhead from inter-instance communication | Lưu ý: trước khi chuyển sang tạo cụm nhiều instance, luôn tối đa hóa số lượng gpu trong một instance duy nhất trước để tránh chi phí phát sinh từ giao tiếp liên instance
* job parallelism : running multiple jobs in parallel | * song song công việc : chạy nhiều công việc song song
* data parallelism : to distribute training data into multiple instances within cluster | * song song dữ liệu : phân phối dữ liệu đào tạo vào nhiều instance trong cụm
    * SageMaker Distributed Data Parallelism library implements inter-instance communication |     * Thư viện Song song dữ liệu phân tán SageMaker triển khai giao tiếp liên instance
    * set backend of torch.distributed.init_process_group as 'smddp' |     * đặt backend của torch.distributed.init_process_group thành 'smddp'
    * however, working with other distributed training libraries are just fine(ex. pytorch ddp, torchrun, deepspeed etc) |     * tuy nhiên, làm việc với các thư viện đào tạo phân tán khác cũng hoàn toàn ổn (ví dụ: pytorch ddp, torchrun, deepspeed, v.v.)
* model parallelism : when model doesn't fit into single instance so has to be loaded on aggregated resources | * song song mô hình : khi mô hình không vừa trong một instance duy nhất nên phải được tải trên các tài nguyên được tổng hợp
    * Sagemaker MPP library for Pytorch provides functionalities including |     * Thư viện MPP Sagemaker cho Pytorch cung cấp các chức năng bao gồm
        * optimization state sharding : weights, gradients anything that stateful optimizer(ex. Adam) holds are sharded |         * phân mảnh trạng thái tối ưu hóa : trọng số, gradient bất cứ thứ gì mà bộ tối ưu hóa có trạng thái (ví dụ: Adam) giữ được đều được phân mảnh
        * activation checkpointing : trick to reduce memory usage by clearning recorded activation value of some layers, and recomputing the during a backward process |         * điểm kiểm tra kích hoạt : thủ thuật để giảm mức sử dụng bộ nhớ bằng cách xóa giá trị kích hoạt đã ghi của một số lớp và tính toán lại chúng trong quá trình lùi
        * activation offloading : swaps checkpointed activations in a microbatch to/from the cpu |         * đổ kích hoạt : hoán đổi các kích hoạt đã kiểm tra trong một lô vi mô đến/từ cpu
    * for more information, see [documents](https://sagemaker.readthedocs.io/en/stable/api/training/sdp_versions/latest/smd_data_parallel_pytorch.html) |     * để biết thêm thông tin, xem [tài liệu](https://sagemaker.readthedocs.io/en/stable/api/training/sdp_versions/latest/smd_data_parallel_pytorch.html)
### Other extra keywords  | ### Các từ khóa bổ sung khác
* warm pool : maintain training resources to reduce provisioning time | * nhóm ấm : duy trì tài nguyên đào tạo để giảm thời gian cấp phát
    * can be turned on when defining sagemaker training job(configure *KeepAlivePeriodInSeconds*) |     * có thể được bật khi xác định công việc đào tạo sagemaker (cấu hình *KeepAlivePeriodInSeconds*)
    * requires service limit increase request in advance -> then use persistent cache to store data across training jobs in combination |     * yêu cầu yêu cầu tăng giới hạn dịch vụ trước -> sau đó sử dụng bộ nhớ đệm cố định để lưu trữ dữ liệu trên các công việc đào tạo kết hợp
* Elastic Fabric Adapter(EFA) : network device attached to the sagemaker GPU instances by making better use of the bandwidth     | * Bộ chuyển đổi Fabric linh hoạt (EFA) : thiết bị mạng được gắn vào các instance GPU sagemaker bằng cách sử dụng tốt hơn băng thông
