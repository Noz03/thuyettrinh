# KỊCH BẢN THUYẾT TRÌNH GIỚI THIỆU SẢN PHẨM AI-GRADING-IOT

**Tên sản phẩm:** AI-Grading-IoT – Hệ thống hỗ trợ đánh giá bài thực hành IoT ứng dụng trí tuệ nhân tạo

**Mã số đề tài:** 21 – SV – 2026 – VT1

**Phong cách:** Giới thiệu sản phẩm kết hợp trình diễn công nghệ

---

## PHẦN I. GIỚI THIỆU VẤN ĐỀ VÀ GIẢI PHÁP

### Slide 1. Giới thiệu sản phẩm


Kính chào quý thầy cô và các bạn.

Trong những năm gần đây, Internet of Things, hay IoT, ngày càng được ứng dụng rộng rãi trong nhiều lĩnh vực như nhà thông minh, công nghiệp, y tế và giáo dục.

Đi cùng với sự phát triển đó, nhu cầu đào tạo các kỹ năng lập trình, thiết kế mạch và triển khai hệ thống IoT cũng ngày càng tăng.

Tuy nhiên, việc đánh giá một bài thực hành IoT không đơn thuần là kiểm tra một đoạn mã nguồn. Giảng viên còn phải xem xét phần cứng, các linh kiện được sử dụng, cách triển khai và kết quả hoạt động của hệ thống.

Xuất phát từ thực tế này, nhóm đã nghiên cứu và phát triển **AI-Grading-IoT – một nền tảng web ứng dụng trí tuệ nhân tạo nhằm hỗ trợ giảng viên quản lý, kiểm tra và đánh giá bài thực hành IoT**.

Trong phần trình bày hôm nay, em sẽ giới thiệu bài toán mà sản phẩm hướng đến, giải pháp được đề xuất, kiến trúc hệ thống, các công nghệ AI được sử dụng và cuối cùng là phần demo thực tế.

### Slide 2. Đặt vấn đề


Để hiểu rõ hơn về vấn đề, chúng ta có thể xem xét một bài thực hành IoT đơn giản: sinh viên sử dụng vi điều khiển ESP32 để đọc dữ liệu từ cảm biến và hiển thị thông tin ra Serial Monitor.

Đối với bài thực hành này, giảng viên cần kiểm tra nhiều yếu tố.

Thứ nhất, mã nguồn của sinh viên có đúng yêu cầu hay không? Việc khởi tạo thiết bị, đọc cảm biến và xử lý dữ liệu đã phù hợp chưa?

Thứ hai, sinh viên có sử dụng đúng các linh kiện cần thiết không? Bộ vi điều khiển và cảm biến có xuất hiện trong mô hình phần cứng hay không?

Và thứ ba, kết quả thực hiện có đáp ứng được yêu cầu của bài thực hành hay không?

Trong phương pháp truyền thống, những công việc này thường cần giảng viên kiểm tra thủ công.

Khi số lượng bài nộp tăng lên, quá trình đánh giá sẽ tiêu tốn nhiều thời gian. Đồng thời, việc duy trì tính nhất quán trong cách cho điểm và phản hồi cũng trở nên khó khăn hơn.

Một số công cụ có thể hỗ trợ kiểm tra mã nguồn, nhưng chưa giải quyết toàn diện việc đánh giá bài thực hành có kết hợp giữa phần mềm và thiết bị vật lý.

Đây chính là vấn đề mà nhóm mong muốn giải quyết.

### Slide 3. Phương án giải quyết


Từ những vấn đề vừa nêu, nhóm đề xuất xây dựng một nền tảng có khả năng kết hợp quản lý bài thực hành và đánh giá tự động bằng trí tuệ nhân tạo.

Thay vì chỉ tập trung vào việc kiểm tra mã nguồn, hệ thống được thiết kế để tiếp nhận nhiều loại thông tin từ bài nộp của sinh viên.

Giải pháp được chia thành ba hướng đánh giá chính.

Hướng thứ nhất là **Code Grading**, sử dụng mô hình ngôn ngữ lớn để phân tích mã nguồn, đối chiếu với yêu cầu bài thực hành và đưa ra nhận xét.

Hướng thứ hai là **Data Grading**, hướng đến việc đánh giá dữ liệu hoặc kết quả đầu ra của thiết bị, chẳng hạn dữ liệu cảm biến được ghi nhận trong quá trình thực hành.

Hướng thứ ba là **Vision Grading**, sử dụng thị giác máy tính để phân tích hình ảnh bộ kit IoT, nhận diện các linh kiện và hỗ trợ kiểm tra phần cứng.

Điểm quan trọng ở đây là hệ thống không được thiết kế để thay thế hoàn toàn giảng viên.

Thay vào đó, AI đóng vai trò công cụ hỗ trợ, còn giảng viên vẫn có quyền xây dựng yêu cầu, điều chỉnh tiêu chí và kiểm tra kết quả đánh giá.

Với định hướng này, AI-Grading-IoT hướng đến một quy trình chấm bài nhanh hơn, có cấu trúc rõ ràng hơn và dễ mở rộng trong tương lai.

---

## PHẦN II. HƯỚNG TRIỂN KHAI VÀ CÔNG NGHỆ

### Slide 4. Hướng triển khai sản phẩm


Để hiện thực hóa giải pháp, nhóm lựa chọn cách triển khai theo từng giai đoạn, ưu tiên xây dựng một sản phẩm MVP, hay Minimum Viable Product, trước khi mở rộng các tính năng nâng cao.

Ở giai đoạn đầu, nhóm tập trung thiết kế các cấu trúc dữ liệu cốt lõi như bài thực hành, tiêu chí chấm điểm, bài nộp của sinh viên và kết quả đánh giá.

Tiếp theo, nhóm phát triển hệ thống backend, các API và tích hợp mô hình ngôn ngữ lớn để hỗ trợ đánh giá mã nguồn.

Sau khi xây dựng các chức năng cơ bản, nhóm tiếp tục triển khai giao diện web dành cho giảng viên và sinh viên.

Song song với quá trình phát triển phần mềm, nhóm thực hiện xây dựng bộ dữ liệu hình ảnh các linh kiện trên bộ kit IoT và tiến hành fine-tuning mô hình YOLO11.

Mô hình sau huấn luyện sẽ được sử dụng trong nhánh xử lý hình ảnh của hệ thống.

Cách tiếp cận này giúp nhóm kiểm thử từng thành phần tương đối độc lập, hạn chế sự phụ thuộc giữa các module và thuận lợi hơn khi cần nâng cấp sản phẩm.

### Slide 5. Công nghệ sử dụng


Về mặt công nghệ, hệ thống sử dụng React để xây dựng giao diện web, giúp tạo ra trải nghiệm tương tác phù hợp với cả giảng viên và sinh viên.

Ở phía backend, nhóm lựa chọn Python kết hợp với FastAPI.

FastAPI hỗ trợ xây dựng các REST API, xử lý dữ liệu và tích hợp các thành phần trí tuệ nhân tạo một cách thuận tiện.

Bên cạnh đó, Pydantic được sử dụng để định nghĩa schema và kiểm tra tính hợp lệ của dữ liệu, bảo đảm các thành phần trong hệ thống có cấu trúc nhất quán.

Đối với AI Code Grading, nhóm sử dụng mô hình ngôn ngữ lớn thông qua Groq API. Cách tiếp cận này cho phép khai thác khả năng phân tích mã nguồn của LLM mà không cần tự triển khai một mô hình ngôn ngữ lớn trên máy chủ riêng.

Đối với xử lý hình ảnh, nhóm lựa chọn YOLO11 để nhận diện các linh kiện trên bộ kit IoT.

Ngoài ra, kiến trúc Vision Grader còn định hướng kết hợp với Qwen3-VL, một mô hình ngôn ngữ thị giác, để hỗ trợ phân tích ngữ cảnh của hình ảnh.

Như vậy, sản phẩm kết hợp hai hướng AI: **LLM để phân tích và đánh giá nội dung, Computer Vision để nhận diện các thành phần phần cứng**.

### Slide 6. Kiến trúc tổng thể hệ thống



Về kiến trúc, AI-Grading-IoT được thiết kế theo hướng phân chia hệ thống thành các tầng chức năng.

Đầu tiên là Presentation Layer, nơi người dùng tương tác với hệ thống thông qua giao diện web React.

Các yêu cầu từ frontend được gửi đến API Layer được xây dựng bằng FastAPI.

Sau đó, yêu cầu được chuyển đến Business Logic Layer, nơi thực hiện các nghiệp vụ như quản lý bài thực hành, tạo rubric, tiếp nhận bài nộp và xử lý yêu cầu chấm điểm.

Đối với các tác vụ yêu cầu trí tuệ nhân tạo, hệ thống tiếp tục chuyển dữ liệu đến AI Layer.

Tại đây, những yêu cầu liên quan đến mã nguồn được xử lý bởi Code Grader thông qua LLM API.

Đối với hình ảnh, nhánh Vision Grader được thiết kế để thực hiện nhận diện linh kiện bằng YOLO11, sau đó kết hợp kết quả nhận diện với các mô hình thị giác – ngôn ngữ để phục vụ quá trình đánh giá.

Cuối cùng là Data Layer, phụ trách lưu trữ thông tin người dùng, bài thực hành, tiêu chí, bài nộp và kết quả.

Ưu điểm của kiến trúc này là khả năng tách biệt các module. Chẳng hạn, khi muốn thay đổi mô hình AI, nhóm có thể điều chỉnh AI Service mà không phải xây dựng lại toàn bộ frontend và các chức năng quản lý.

---

## PHẦN III. CÁC TÍNH NĂNG VÀ LUỒNG HOẠT ĐỘNG

### Slide 7. Tổng quan các tính năng


Từ kiến trúc vừa trình bày, nhóm đã tổ chức sản phẩm thành các nhóm chức năng chính, tương ứng với quy trình sử dụng thực tế.

Với giảng viên, hệ thống hỗ trợ quản lý khóa học, tạo bài thực hành và thiết lập các tiêu chí đánh giá.

Với sinh viên, hệ thống cung cấp giao diện để xem yêu cầu, gửi mã nguồn và tải lên những hình ảnh cần thiết cho bài thực hành.

Sau khi bài nộp được gửi lên, AI Grading Service thực hiện phân tích những thành phần được hỗ trợ và trả về kết quả.

Kết quả không chỉ là một điểm số, mà còn hướng đến việc thể hiện nhận xét và mức độ đáp ứng các tiêu chí đánh giá.

Ngoài ra, nhóm đang mở rộng khả năng phân tích hình ảnh phần cứng trên cơ sở mô hình YOLO11 đã được fine-tune.

Sau đây, em xin trình bày cụ thể hơn về luồng sử dụng của giảng viên, sinh viên và quá trình AI xử lý bài nộp.

### Slide 8. Luồng chức năng dành cho giảng viên


Đối với giảng viên, quy trình bắt đầu bằng việc tạo một bài thực hành mới.

Khi tạo Practice, giảng viên có thể khai báo những thông tin quan trọng như mục tiêu bài học, danh sách nhiệm vụ cần thực hiện, các ràng buộc kỹ thuật và môi trường phần cứng sử dụng.

Ví dụ, một bài thực hành có thể yêu cầu sinh viên sử dụng ESP32, đọc dữ liệu cảm biến và gửi kết quả ra Serial Monitor theo một chu kỳ xác định.

Thông thường, để chấm một bài thực hành như vậy, giảng viên cần tự xây dựng bộ tiêu chí chi tiết.

Trong AI-Grading-IoT, hệ thống được thiết kế để hỗ trợ quá trình này bằng cách tự động đề xuất các rubric dựa trên nội dung Practice.

Rubric được chia thành ba nhóm: Code Rubric, Data Rubric và Vision Rubric.

Giảng viên có thể xem lại, điều chỉnh tiêu chí và trọng số trước khi sử dụng để chấm điểm.

Tính năng này hướng đến việc giảm thời gian xây dựng rubric từ đầu, đồng thời giúp những bài thực hành khác nhau có cấu trúc đánh giá rõ ràng và có thể kiểm soát được.

### Slide 9. Luồng chức năng dành cho sinh viên


Đối với sinh viên, quy trình sử dụng được thiết kế tương đối đơn giản.

Sau khi đăng nhập, sinh viên lựa chọn bài thực hành được giảng viên công bố.

Tại đây, sinh viên có thể xem mục tiêu, các nhiệm vụ cần thực hiện và những yêu cầu kỹ thuật tương ứng.

Sau khi hoàn thành bài thực hành, sinh viên tạo bài nộp, cung cấp mã nguồn và các hình ảnh liên quan đến bộ kit phần cứng.

Bài nộp sẽ được lưu trữ trong hệ thống và liên kết với bài thực hành tương ứng.

Khi kích hoạt chức năng chấm điểm, AI Grading Service sẽ lấy thông tin bài nộp, yêu cầu bài thực hành và rubric để thực hiện đánh giá.

Sau khi hoàn thành, sinh viên có thể xem kết quả chấm, nhận xét và những nội dung cần cải thiện.

Trong phiên bản MVP, chức năng chấm lần đầu được giới hạn theo bài nộp nhằm tránh việc gửi yêu cầu đánh giá lặp lại không cần thiết.

### Slide 10. Luồng AI Code Grading


Một trong những tính năng trọng tâm của sản phẩm là AI Code Grading.

Đây cũng là thành phần được nhóm ưu tiên triển khai trong giai đoạn đầu.

Khi sinh viên yêu cầu chấm điểm, hệ thống thu thập ba nhóm thông tin.

Thứ nhất là mô tả bài thực hành, giúp AI hiểu mục tiêu và yêu cầu mà sinh viên cần thực hiện.

Thứ hai là Code Rubric, chứa các tiêu chí đánh giá và trọng số tương ứng.

Thứ ba là mã nguồn do sinh viên gửi lên.

Các thông tin này được tổng hợp thành một prompt có cấu trúc và gửi đến mô hình ngôn ngữ lớn thông qua Groq API.

LLM phân tích mã nguồn dựa trên những tiêu chí đã được cung cấp, từ đó đưa ra đánh giá và nhận xét.

Kết quả trả về tiếp tục được backend xử lý, kiểm tra cấu trúc và tính hợp lệ trước khi lưu trữ.

Một điểm quan trọng trong thiết kế là việc sử dụng rubric làm cơ sở chấm điểm.

Điều này giúp AI không chỉ đưa ra nhận xét một cách chung chung, mà phải đối chiếu với những yêu cầu cụ thể do giảng viên quy định.

Tuy nhiên, việc phân tích mã nguồn bằng LLM không đồng nghĩa với việc đã kiểm chứng chương trình chạy đúng trên thiết bị thật. Đây là giới hạn mà hệ thống cần phân biệt rõ trong kết quả đánh giá.

### Slide 11. Luồng AI Vision Grading


Bên cạnh mã nguồn, một bài thực hành IoT còn có thành phần rất quan trọng là phần cứng.

Vì vậy, nhóm đã nghiên cứu và phát triển hướng đánh giá hình ảnh sử dụng Computer Vision.

Trong kiến trúc đề xuất, khi sinh viên tải ảnh bộ kit lên, hệ thống sẽ đưa hình ảnh qua mô hình YOLO11.

Nhiệm vụ của YOLO11 là xác định những linh kiện xuất hiện trong ảnh, vị trí của chúng và độ tin cậy của từng kết quả nhận diện.

Chẳng hạn, mô hình có thể nhận diện vi điều khiển Arduino, màn hình LCD, keypad hoặc các loại cảm biến trên bộ kit.

Tuy nhiên, việc xác định có những linh kiện nào mới chỉ là bước đầu.

Để đánh giá một bài thực hành, hệ thống còn cần hiểu mối liên hệ giữa các linh kiện và đối chiếu với yêu cầu của giảng viên.

Vì vậy, nhóm định hướng bổ sung Qwen3-VL để phân tích hình ảnh trong ngữ cảnh của bài thực hành.

Kết quả từ YOLO11 sẽ đóng vai trò thông tin bổ trợ cho mô hình thị giác – ngôn ngữ.

Chẳng hạn, nếu bài yêu cầu sử dụng Arduino, LCD và cảm biến nhiệt độ, hệ thống có thể đối chiếu danh sách linh kiện nhận diện được với danh sách linh kiện yêu cầu.

Đối với các vấn đề phức tạp hơn như xác định kết nối dây dẫn hoặc kiểm tra thiết bị có hoạt động chính xác hay không, hệ thống cần thêm bằng chứng và các bước kiểm chứng bổ sung.

Ở thời điểm hiện tại, mô hình YOLO11 đã được fine-tune, còn quá trình tích hợp hoàn chỉnh Vision Grader với Qwen3-VL là hướng phát triển tiếp theo.

---

## PHẦN IV. DATASET VÀ FINE-TUNING YOLO11

### Slide 12. Xây dựng Dataset IoT


Để có thể nhận diện các linh kiện trong bộ kit của dự án, nhóm cần một mô hình được huấn luyện phù hợp với dữ liệu thực tế.

Mặc dù YOLO11 là mô hình phát hiện vật thể đã được huấn luyện trước, các lớp đối tượng phổ biến trong bộ dữ liệu gốc không đáp ứng trực tiếp nhu cầu nhận diện những linh kiện cụ thể trên bộ kit IoT của nhóm.

Vì vậy, nhóm tiến hành xây dựng một bộ dữ liệu riêng.

Đầu tiên, nhóm phân tích cấu trúc của bộ kit và xác định danh sách những linh kiện cần nhận diện.

Bộ dữ liệu bao gồm các nhóm linh kiện như module nguồn, vi điều khiển, thiết bị hiển thị, cảm biến và các module ngoại vi.

Một số ví dụ là Arduino, STM32, LCD, keypad, cảm biến DHT, cảm biến khí gas, cảm biến ánh sáng và các thành phần trên module nguồn.

Sau khi xác định danh sách class, nhóm tiến hành thu thập hình ảnh và thực hiện annotation.

Với mỗi linh kiện trong ảnh, nhóm tạo một bounding box và gán class tương ứng.

Một nguyên tắc quan trọng là duy trì tính nhất quán trong việc gán nhãn. Những linh kiện cùng loại phải được chú thích theo cùng một quy tắc để hạn chế nhiễu trong quá trình huấn luyện.

Sau khi hoàn thành, dữ liệu được tổ chức theo định dạng YOLO và sử dụng để fine-tune mô hình.

### Slide 13. Fine-tuning mô hình YOLO11


Sau khi xây dựng dataset, nhóm tiếp tục tiến hành fine-tuning mô hình YOLO11.

Thay vì huấn luyện toàn bộ mô hình từ đầu, nhóm tận dụng trọng số đã được pretrained của YOLO11.

Cách tiếp cận này được gọi là Transfer Learning, cho phép tận dụng các đặc trưng hình ảnh mà mô hình đã học trước đó để thích nghi với bộ dữ liệu IoT mới.

Trong quá trình huấn luyện, nhóm cấu hình bộ dữ liệu, lựa chọn mô hình nền và thiết lập các tham số huấn luyện phù hợp.

Mô hình sau đó học cách phân biệt các linh kiện dựa trên hình dạng, màu sắc, kích thước và đặc trưng thị giác.

Trong quá trình này, các chỉ số như loss, precision, recall và mAP được sử dụng để theo dõi và đánh giá chất lượng mô hình.

Kết thúc quá trình huấn luyện, nhóm thu được file trọng số `best.pt`.

Đây là mô hình được lựa chọn theo kết quả đánh giá trên tập validation trong quá trình training.

File này có thể được nạp vào chương trình Python để thực hiện inference trên những hình ảnh mới.

Kết quả đầu ra bao gồm tên class, tọa độ bounding box và confidence score.

Đây chính là thành phần AI nhận diện linh kiện mà nhóm dự kiến tích hợp trực tiếp vào hệ thống Vision Grading.

#### Phần bổ sung giữa Slide 13 và Slide 14 – Giải thích quá trình fine-tuning, định nghĩa các chỉ số và đọc kết quả YOLO11

*Vị trí trình bày: sau lời nói ở Slide 13 và trước Slide 14. Đoạn dưới đây được viết để có thể đọc trực tiếp khi thuyết trình. Các con số về cấu hình, dữ liệu và kết quả đánh giá lấy từ notebook `notebook399d3887ca.ipynb`; ví dụ tính toán dùng để giải thích khái niệm, không phải số đo của mô hình.*

**1. Mục tiêu của bài toán và dữ liệu đầu vào**

Trước khi xem kết quả, em xin giải thích cụ thể mô hình YOLO11 đã được huấn luyện như thế nào và các chỉ số ở biểu đồ có ý nghĩa gì.

Bài toán nhóm giải quyết là **Object Detection**, hay *phát hiện đối tượng trong ảnh*. Một mô hình Object Detection không chỉ dự đoán tên linh kiện mà còn tìm vị trí linh kiện đó. Chẳng hạn, với một ảnh có Arduino, LCD và cảm biến nhiệt độ, đầu ra mong muốn là ba **class**, tức *nhãn loại linh kiện*, cùng ba **bounding box**, tức *khung chữ nhật bao quanh vùng chứa linh kiện*. Vị trí này được biểu diễn bằng tọa độ để máy tính có thể vẽ và xử lý tiếp.

Để dạy mô hình, nhóm dùng **dataset**, nghĩa là tập dữ liệu phục vụ huấn luyện, gồm ảnh và **annotation**. Annotation là thông tin do người gán nhãn cung cấp, cho biết một linh kiện thuộc class nào và khung bao của nó nằm ở đâu. Trong notebook, dataset được lấy từ Roboflow, phiên bản 2 của dự án AI-Grading-IoT, ở định dạng `yolov8` tương thích với thư viện Ultralytics và có **33 class**. Số class là số *loại linh kiện cần phân biệt*, không phải số linh kiện xuất hiện trong một ảnh.

Dữ liệu được chia thành **tập train**, tức nhóm ảnh mô hình dùng để học và cập nhật tham số, và **tập validation**, tức nhóm ảnh tách ra để kiểm tra chất lượng trong quá trình huấn luyện. Nhật ký ghi **1.173 ảnh train** và **112 ảnh validation với 841 instance**. *Instance* là một đối tượng được gán nhãn cụ thể; ví dụ, một ảnh có 1 Arduino và 2 nút bấm thì có 3 instance. Vì thế, 112 ảnh có thể chứa 841 đối tượng. Notebook chưa trình bày kết quả đánh giá trên một **tập test độc lập**, tức tập chỉ dùng để kiểm tra cuối cùng, không dùng trong lựa chọn mô hình.

**2. Vì sao dùng YOLO11n và fine-tuning?**

Nhóm sử dụng mô hình **YOLO11n** với tệp pretrained `yolo11n.pt`. *Pretrained* có nghĩa mô hình đã được huấn luyện trước trên dữ liệu khác để học các đặc trưng thị giác cơ bản, như cạnh, hình dạng và họa tiết. Ký tự **n** viết tắt của **nano**, tức biến thể gọn của họ YOLO11. YOLO là họ mô hình được thiết kế để thực hiện Object Detection nhanh trên ảnh.

Thay vì huấn luyện từ trọng số ngẫu nhiên, nhóm thực hiện **fine-tuning**, tức tiếp tục huấn luyện mô hình có sẵn trên ảnh linh kiện IoT. Đây là một hình thức **Transfer Learning**, nghĩa là chuyển kiến thức đã học từ bài toán trước sang bài toán mới. Trong quá trình này, mô hình học cách phân biệt 33 class của bộ kit, chẳng hạn LCD khác keypad như thế nào. Lựa chọn nano thuận lợi khi cần chạy trên phần cứng có tài nguyên hạn chế, nhưng **chưa thể kết luận đây là biến thể tối ưu nhất** nếu chưa so sánh thực nghiệm với các kích thước mô hình khác.

**3. Cấu hình huấn luyện – định nghĩa từng tham số**

Notebook huấn luyện bằng Ultralytics trên Kaggle, sử dụng GPU **NVIDIA Tesla T4**. **GPU**, viết tắt của *Graphics Processing Unit*, là bộ xử lý có khả năng thực hiện nhiều phép tính song song, giúp tăng tốc việc huấn luyện mạng nơ-ron. Thư viện **Ultralytics** là công cụ cung cấp mã nguồn và API để huấn luyện, đánh giá và chạy inference với YOLO.

Cấu hình gồm `epochs=50`, `imgsz=640`, `batch=16`, `device=0`. **Epoch** được định nghĩa là *một lượt mô hình đi qua toàn bộ tập train*; 50 epoch nghĩa là quá trình học diễn ra qua tập train 50 lượt, chứ không phải học 50 ảnh. **Batch** là một nhóm ảnh được xử lý cùng lúc để tính bước cập nhật mô hình; **batch size** là số ảnh của nhóm đó. Với batch size 16 và 1.173 ảnh, mỗi epoch có khoảng **74 batch**, đúng với log huấn luyện. **Image size**, hay `imgsz=640`, là kích thước đầu vào do pipeline xử lý ảnh của YOLO sử dụng, thường tương ứng 640 × 640 pixel sau tiền xử lý; nó không có nghĩa mọi ảnh gốc đều được chụp ở độ phân giải này. `device=0` chọn GPU đầu tiên trong môi trường Kaggle.

Một thuật ngữ quan trọng nữa là **weight**, hay *trọng số mô hình*: các tham số số học mà mạng nơ-ron điều chỉnh trong quá trình học. Khi mô hình dự đoán sai, thuật toán **optimizer**, tức bộ tối ưu, dùng thông tin từ hàm mất mát và gradient để cập nhật trọng số. Trong log, bộ tối ưu được lựa chọn tự động là **AdamW**; vì vậy ta không nên nói nhóm tự tay chọn từng tham số tối ưu nếu notebook không thể hiện điều đó.

**4. Training Loss – ba đường biểu đồ phản ánh điều gì?**

Trong bước training, mô hình xem ảnh có nhãn, dự đoán class và khung bao, sau đó so sánh với đáp án. Mức sai lệch được biểu diễn qua **loss**, hay *hàm mất mát*. Loss là giá trị mà thuật toán cố gắng giảm xuống trong quá trình học. Nó **không phải** phần trăm dự đoán sai và cũng **không phải** độ chính xác của mô hình.

Notebook vẽ ba đường trên tập train:

- **Box Loss** là *mức sai lệch khi xác định vị trí khung bao*. Ví dụ, mô hình phát hiện đúng Arduino nhưng vẽ khung bao lệch nhiều khỏi vị trí được gán nhãn, thì thành phần sai số định vị có thể lớn. Box Loss giảm cho thấy việc tối ưu vị trí bounding box đang tốt dần trên tập train.
- **Classification Loss**, thường viết `cls_loss`, là *mức sai lệch khi dự đoán class*. Ví dụ, mô hình nhầm một module LCD thành một thiết bị khác sẽ bị phạt ở thành phần này. Khi Class Loss giảm, mô hình đang học cách phân biệt nhãn linh kiện tốt hơn trên dữ liệu huấn luyện.
- **DFL Loss**, viết đầy đủ là *Distribution Focal Loss*, là thành phần loss hỗ trợ **hồi quy vị trí khung bao** bằng cách học phân phối cho các khoảng cách đến cạnh hộp. Nói đơn giản, DFL giúp tinh chỉnh cách mô hình xác định tọa độ của bounding box. DFL **không phải** điểm confidence hay một chỉ số phần trăm chính xác.

Khi xem các biểu đồ theo **epoch**, trục ngang biểu diễn số lượt học; trục dọc là giá trị loss. Một xu hướng thường được mong đợi là các đường loss giảm rồi ổn định. Tuy nhiên, loss train giảm không chứng minh mô hình sẽ tốt trên những ảnh ngoài tập train. Đó là lý do phải quan sát validation.

**5. Validation Loss, IoU và các thước đo đánh giá**

**Validation** là quá trình đưa tập ảnh không dùng để cập nhật trọng số vào mô hình để đánh giá mức độ mô hình áp dụng kiến thức đã học. **Validation Loss** được tính trên tập này. Nếu Training Loss giảm nhưng Validation Loss tăng kéo dài, có thể xuất hiện **overfitting**, hay *quá khớp*: mô hình học rất sát ảnh train nhưng hoạt động kém hơn trên ảnh mới. Đây chỉ là một dấu hiệu cần xem xét cùng các chỉ số khác, không phải kết luận tự động.

Khi đo độ đúng của **bounding box**, công cụ sử dụng **IoU**, viết tắt của *Intersection over Union*, nghĩa là **tỷ lệ diện tích giao trên diện tích hợp** giữa khung dự đoán và khung thật. Ví dụ, nếu diện tích phần giao là 80 đơn vị và phần hợp là 100 đơn vị thì **IoU = 80 / 100 = 0,80**. IoU bằng 1 khi hai khung trùng khít và bằng 0 khi không chồng lấp. Ở ngưỡng **IoU 0,50**, một khung có IoU 0,80 đạt yêu cầu về độ chồng lấp; ở ngưỡng 0,90, chính khung đó không đạt. Đó là lý do một mô hình có thể có kết quả khác nhau khi thay tiêu chuẩn đánh giá vị trí.

Để tính kết quả detection, ta cần phân biệt ba loại kết quả: **True Positive – TP**, tức một phát hiện đúng class và đáp ứng quy tắc khớp đối tượng; **False Positive – FP**, tức phát hiện sai hoặc phát hiện thừa; và **False Negative – FN**, tức một đối tượng có thật nhưng bị bỏ sót. Ví dụ *minh họa*, ảnh có 10 linh kiện, mô hình nhận đúng 8, bỏ sót 2, đồng thời báo thêm 1 linh kiện không có thật, thì TP = 8, FN = 2 và FP = 1.

**6. Precision, Recall, AP và mAP – giải thích số liệu thật**

Từ TP, FP và FN, hai chỉ số cơ bản được định nghĩa như sau:

- **Precision**, hay *độ chính xác trong số các đối tượng mà mô hình đã phát hiện*, được tính bằng **TP / (TP + FP)**. Nó trả lời câu hỏi: “Trong các linh kiện mà mô hình báo có, bao nhiêu phát hiện là đúng?”. Với ví dụ TP = 8 và FP = 1, Precision = 8/9 ≈ 88,9%. Precision cao cho thấy ít nhận nhầm hoặc ít báo thừa.
- **Recall**, hay *độ bao phủ những đối tượng thực sự tồn tại*, được tính bằng **TP / (TP + FN)**. Nó trả lời câu hỏi: “Trong các linh kiện thật sự xuất hiện, mô hình tìm được bao nhiêu?”. Với ví dụ TP = 8 và FN = 2, Recall = 8/10 = 80%. Recall cao cho thấy mô hình bỏ sót ít linh kiện hơn.

Hai chỉ số này có thể thay đổi khi điều chỉnh **confidence threshold**, tức mức điểm tự tin tối thiểu để giữ lại một dự đoán. Hạ ngưỡng có thể giúp phát hiện nhiều vật thể hơn nhưng cũng tăng báo nhầm; tăng ngưỡng có thể giảm báo nhầm nhưng tăng bỏ sót. Vì vậy, một chỉ số cao riêng lẻ chưa nói lên toàn bộ chất lượng detection.

Để đánh giá tổng hợp, ta dùng **AP – Average Precision**, nghĩa là *diện tích dưới đường Precision–Recall* cho một class tại một mức IoU xác định. Đường Precision–Recall cho thấy sự đánh đổi giữa hai chỉ số khi thay ngưỡng confidence. **mAP – Mean Average Precision** là *trung bình AP của các class được đưa vào đánh giá*. Khác với Precision, mAP không phải tỉ lệ ảnh được dự đoán đúng mà là chỉ số tổng hợp chất lượng phát hiện đối tượng theo nhiều ngưỡng confidence.

Hai giá trị thường xuất hiện trên slide có nghĩa là:

- **mAP@50** là mAP tính ở mức **IoU = 0,50**. Tức là khi đánh giá việc khung dự đoán có khớp với vật thể thật hay không, ta dùng ngưỡng chồng lấp 50%.
- **mAP@50–95** là trung bình của mAP tại **10 ngưỡng IoU: 0,50; 0,55; 0,60; ...; 0,95**. Chỉ số này khắt khe hơn về vị trí khung bao, nên thường thấp hơn mAP@50.

Trong notebook, khi đánh giá lại file `best.pt` trên **112 ảnh validation chứa 841 đối tượng**, kết quả được ghi nhận là:

| Chỉ số | Định nghĩa ngắn | Kết quả từ notebook |
|---|---|---:|
| Precision | Tỷ lệ phát hiện đúng trong số phát hiện được tính | **0,924 = 92,4%** |
| Recall | Tỷ lệ đối tượng thật được tìm thấy | **0,892 = 89,2%** |
| mAP@50 | Trung bình AP các class tại IoU 0,50 | **0,897 = 89,7%** |
| mAP@50–95 | Trung bình mAP qua IoU 0,50 đến 0,95 | **0,654 = 65,4%** |

Nói cách khác, kết quả cho thấy mô hình đạt chất lượng phát hiện tương đối tốt trên **tập validation được báo cáo**, nhưng khi tăng yêu cầu về mức chồng lấp khung bao thì hiệu năng tổng hợp thấp hơn. Đặc biệt, **mAP@50 = 89,7% không có nghĩa 89,7% hình ảnh được nhận diện hoàn toàn đúng, và càng không có nghĩa hệ thống chấm đúng 89,7% bài thực hành IoT**. Đây là một chỉ số đo chất lượng Object Detection, không phải điểm chấm bài của Vision Grader.

**7. Đánh giá từng class và ý nghĩa với sản phẩm**

Bên cạnh kết quả toàn bộ dataset, notebook cho thấy hiệu năng theo từng **class**. Chỉ số **per-class mAP@50** là AP ở IoU 0,50 của riêng class đó, từ đó cho thấy những linh kiện nào dễ hay khó nhận diện hơn. Chẳng hạn, Arduino đạt **0,995**, LCD đạt **0,984**, DHT sensor đạt **0,995**. Ngược lại, buzzer đạt **0,417**, `power_ack_led` đạt **0,637** và `power_vcc_led` đạt **0,627**. Đặc biệt, `lora_module` có **1 instance trong tập validation** và mAP@50 **0,124**; mẫu validation quá ít nên không thể kết luận chắc chắn mô hình sẽ hoạt động như thế nào với loại linh kiện này ở môi trường thực tế.

Nhóm cũng có thể dùng **Confusion Matrix – ma trận nhầm lẫn**, là bảng đặt nhãn đúng cạnh nhãn mô hình dự đoán để xem cặp class nào dễ bị nhận nhầm. Đối với bộ kit IoT, những kết quả này giúp nhóm biết cần chụp thêm ảnh cho class nào, bổ sung góc nhìn hay kiểm tra lại annotation ở đâu.

**8. Từ `best.pt` đến Slide 14 – nhận diện trên ảnh mới**

Cuối cùng, trong quá trình training, hệ thống lưu **checkpoint**, tức bản lưu trạng thái trọng số tại một thời điểm. Tệp **`best.pt`** là checkpoint được lựa chọn tốt nhất theo tiêu chí validation của trình huấn luyện; **`last.pt`** thường là trọng số ở epoch cuối. Notebook có `runs/detect/train/weights/best.pt`, dung lượng khoảng **5,5 MB**, được dùng để đánh giá lại trên validation.

Khi nạp `best.pt` để thực hiện **inference**, nghĩa là *cho mô hình đã huấn luyện dự đoán trên ảnh đầu vào mà không tiếp tục cập nhật trọng số*, mô hình sẽ trả về class, bounding box và **confidence score**. Confidence score là *điểm tự tin mô hình gán cho một dự đoán*, ví dụ `lcd: 0.91`; số này không nên hiểu máy móc là “91% xác suất thực tế chắc chắn đúng” nếu chưa kiểm định hiệu chỉnh xác suất. Từ các dự đoán này, phần Vision Grader có thể đối chiếu với các linh kiện bắt buộc của Practice và Vision Rubric, nhưng YOLO11 **không tự kiểm chứng được mạch đấu dây đúng, cảm biến hoạt động đúng hay toàn bộ bài thực hành đạt yêu cầu**.

Như vậy, nhóm đã đi qua chuỗi xử lý: **dataset có annotation → YOLO11n pretrained → fine-tuning 50 epoch → theo dõi Training Loss và Validation Metrics → chọn `best.pt` → inference**. Điểm cần nhớ là **các con số vừa trình bày là kết quả validation của notebook, chưa phải kết quả kiểm thử độc lập trên mọi môi trường và chưa phải độ chính xác của hệ thống chấm bài**.

**Tiếp theo, ở Slide 14, em xin minh họa ảnh đầu vào và kết quả phát hiện đối tượng để thấy cụ thể bounding box, class và confidence score được hiển thị như thế nào.**

### Slide 14. Kết quả thử nghiệm YOLO11


Sau khi fine-tuning, nhóm thực hiện inference để kiểm tra khả năng nhận diện của mô hình trên những hình ảnh bộ kit.

Trên màn hình là hình ảnh đầu vào và kết quả xử lý tương ứng.

Có thể quan sát thấy mô hình trả về các bounding box bao quanh những linh kiện mà nó phát hiện, cùng với tên class và độ tin cậy dự đoán.

Điều này cho thấy mô hình đã có thể thực hiện bài toán phát hiện linh kiện theo danh sách class được xây dựng riêng cho dự án.

Tuy nhiên, chất lượng nhận diện không đồng đều trong mọi điều kiện.

Một số yếu tố như kích thước linh kiện nhỏ, góc chụp, ánh sáng, các linh kiện bị che khuất hoặc số lượng ảnh huấn luyện hạn chế có thể ảnh hưởng đến kết quả.

Vì vậy, bên cạnh những hình ảnh mà mô hình nhận diện tốt, nhóm cũng xem xét các trường hợp nhận diện sai hoặc bỏ sót.

Đây là cơ sở để tiếp tục bổ sung dữ liệu và cải thiện mô hình trong tương lai.

Ở giai đoạn hiện tại, mục tiêu quan trọng của nhóm là chứng minh khả năng sử dụng mô hình YOLO11 đã fine-tune để nhận diện các linh kiện phục vụ hệ thống AI-Grading-IoT.

---

## PHẦN V. DEMO SẢN PHẨM

### Slide 15. Demo hệ thống AI-Grading-IoT

**Nội dung trên slide:**

Demo 1 – Tạo bài thực hành và Rubric.

Demo 2 – Sinh viên nộp bài và AI đánh giá mã nguồn.

Demo 3 – YOLO11 nhận diện linh kiện IoT.

**Kịch bản thuyết trình và thao tác:**

**Bước 1. Giới thiệu giao diện web**

Tiếp theo, em xin chuyển sang phần demo thực tế để minh họa cách sản phẩm hoạt động.

Đây là giao diện của hệ thống AI-Grading-IoT.

Hệ thống được xây dựng với các chức năng khác nhau tùy theo vai trò người dùng, trong đó hai vai trò chính là giảng viên và sinh viên.

Đầu tiên, em sẽ đăng nhập bằng tài khoản giảng viên để thực hiện quy trình tạo bài thực hành.

*[Thao tác: Mở trang web, đăng nhập, truy cập trang quản lý khóa học và bài thực hành.]*

**Bước 2. Tạo bài thực hành**

Ở đây, giảng viên có thể tạo một bài thực hành mới.

Trong ví dụ này, em sẽ sử dụng một bài thực hành ESP32 với yêu cầu đọc dữ liệu cảm biến và hiển thị kết quả lên Serial Monitor.

Hệ thống cho phép khai báo mục tiêu bài thực hành, danh sách nhiệm vụ, những ràng buộc kỹ thuật và môi trường phát triển.

*[Thao tác: Mở Practice đã chuẩn bị hoặc tạo một Practice mới với các yêu cầu cụ thể.]*

Sau khi tạo bài thực hành, hệ thống có chức năng hỗ trợ xây dựng rubric bằng AI.

Phần Code Rubric thể hiện các tiêu chí liên quan đến mã nguồn, chẳng hạn khởi tạo Serial, đọc dữ liệu cảm biến và xử lý vòng lặp chương trình.

Giảng viên có thể xem lại và chỉnh sửa những tiêu chí này trước khi sử dụng.

*[Thao tác: Mở Rubric và trình bày các tiêu chí đã được tạo.]*

**Bước 3. Sinh viên nộp bài**

Tiếp theo, em chuyển sang tài khoản sinh viên.

Tại đây, sinh viên có thể truy cập bài thực hành và xem các yêu cầu mà giảng viên đã công bố.

Sau khi hoàn thành, sinh viên có thể gửi mã nguồn cùng với hình ảnh bộ kit.

*[Thao tác: Đăng nhập bằng tài khoản sinh viên, mở Practice và tạo Submission.]*

Trong ví dụ này, em đã chuẩn bị một chương trình đơn giản dành cho ESP32.

Chương trình thực hiện khởi tạo Serial, đọc giá trị đầu vào analog và xuất dữ liệu ra Serial Monitor.

Sau khi tải mã nguồn lên, em sẽ tiến hành nộp bài.

**Bước 4. AI Code Grading**

Sau khi bài nộp được lưu, sinh viên có thể kích hoạt chức năng AI Grading.

Lúc này, backend sẽ thu thập yêu cầu bài thực hành, rubric và mã nguồn, sau đó gửi đến LLM thông qua Groq API.

*[Thao tác: Nhấn chấm điểm và mở trang kết quả.]*

Ở giao diện kết quả, hệ thống thể hiện điểm đánh giá mã nguồn và phần nhận xét tương ứng.

Các tiêu chí đánh giá được đối chiếu với nội dung mà giảng viên đã xây dựng trước đó.

Nhờ vậy, sinh viên có thể biết những phần nào trong mã nguồn đáp ứng yêu cầu và những phần nào cần cải thiện.

Ngoài ra, giảng viên cũng có thể truy cập các bài nộp trong khóa học để kiểm tra kết quả.

**Bước 5. Demo nhận diện linh kiện bằng YOLO11**

Ở phần tiếp theo, em xin giới thiệu kết quả của nhánh Computer Vision.

Đây là mô hình YOLO11 mà nhóm đã fine-tune trên bộ dữ liệu linh kiện IoT.

*[Thao tác: Mở chương trình inference hoặc các hình ảnh thử nghiệm đã chuẩn bị.]*

Đầu tiên, em đưa vào một hình ảnh bộ kit.

Sau khi xử lý, mô hình trả về các linh kiện phát hiện được cùng với bounding box và confidence score tương ứng.

*[Thao tác: Chạy inference bằng file `best.pt`, trình bày ảnh kết quả.]*

Các kết quả này là đầu vào dự kiến cho Vision Grader khi hoàn thiện quá trình tích hợp với hệ thống web.

Trong kiến trúc tổng thể, YOLO11 phụ trách nhận diện linh kiện, còn mô hình thị giác – ngôn ngữ được định hướng sử dụng để phân tích sâu hơn và đối chiếu với yêu cầu bài thực hành.

Như vậy, phần demo vừa rồi minh họa hai thành phần chính: hệ thống web hỗ trợ quy trình quản lý và chấm mã nguồn, cùng với mô hình AI nhận diện linh kiện IoT đã được fine-tune.

---

## PHẦN VI. ĐÁNH GIÁ VÀ KẾT LUẬN

### Slide 16. Kết quả đạt được và định hướng phát triển

**Nội dung trên slide:**

**Kết quả:**
- Xây dựng nền tảng web quản lý bài thực hành.
- Triển khai AI Code Grading thông qua LLM API.
- Xây dựng cấu trúc Practice, Rubric, Submission và GradingResult.
- Xây dựng dataset linh kiện IoT.
- Fine-tuning YOLO11 và thu được mô hình `best.pt`.

**Định hướng:**
- Hoàn thiện Data Grading.
- Tích hợp YOLO11 và Qwen3-VL vào Vision Grader.
- Kiểm thử và đánh giá mô hình trên dữ liệu thực tế.
- Nâng cao chất lượng rubric và kết quả chấm.

**Lời thuyết trình:**

Qua quá trình nghiên cứu và triển khai, nhóm đã xây dựng được nền tảng ban đầu cho một hệ thống hỗ trợ đánh giá bài thực hành IoT ứng dụng trí tuệ nhân tạo.

Về phía phần mềm, nhóm đã phát triển các thành phần quản lý bài thực hành, rubric, bài nộp và kết quả đánh giá, đồng thời tích hợp LLM API phục vụ quá trình chấm mã nguồn.

Về phía Computer Vision, nhóm đã xây dựng bộ dữ liệu các linh kiện IoT, tiến hành fine-tuning YOLO11 và thu được mô hình có thể sử dụng để inference.

Những kết quả này tạo nền tảng để tiếp tục phát triển hệ thống đánh giá kết hợp giữa mã nguồn, dữ liệu và hình ảnh phần cứng.

Tất nhiên, sản phẩm vẫn còn những giới hạn.

Trước hết, khả năng đánh giá hình ảnh hiện mới tập trung vào bài toán nhận diện linh kiện. Việc kiểm tra chính xác kết nối dây dẫn và chức năng hoạt động của phần cứng là một bài toán phức tạp hơn.

Bên cạnh đó, việc sử dụng LLM để chấm mã nguồn vẫn cần được kiểm chứng thông qua các bộ benchmark và kết quả đánh giá từ giảng viên.

Trong thời gian tới, nhóm định hướng hoàn thiện Vision Grader, mở rộng Data Grading và xây dựng quy trình kiểm thử nhằm đo lường độ chính xác của hệ thống.

**Kết thúc:**

Thông qua AI-Grading-IoT, nhóm mong muốn cho thấy tiềm năng của việc kết hợp trí tuệ nhân tạo với một nền tảng quản lý và đánh giá bài thực hành trong giáo dục kỹ thuật.

Điểm cốt lõi của sản phẩm không chỉ là ứng dụng một mô hình AI để cho điểm, mà là xây dựng một quy trình có thể kết hợp yêu cầu của giảng viên, mã nguồn của sinh viên, dữ liệu thực hành và bằng chứng hình ảnh phần cứng.

Đây sẽ là cơ sở để từng bước hình thành một hệ thống đánh giá bài thực hành IoT toàn diện hơn trong tương lai.

Em xin cảm ơn quý thầy cô và các bạn đã lắng nghe phần giới thiệu sản phẩm AI-Grading-IoT của nhóm.

Em rất mong nhận được những câu hỏi và góp ý để tiếp tục hoàn thiện sản phẩm.

---


---

## PHỤ LỤC. ĐỊNH NGHĨA CHI TIẾT CÁC THUẬT NGỮ VÀ CHỈ SỐ YOLO11

*Phụ lục dùng để ôn tập và trả lời câu hỏi sau thuyết trình; lời nói chính đã giải thích ngắn gọn các thuật ngữ trước khi đọc kết quả.*

## I. Các khái niệm nền tảng về mô hình và dữ liệu

### 1. AI, Machine Learning và Deep Learning

- **Định nghĩa:** **Artificial Intelligence (AI)** là lĩnh vực xây dựng hệ thống thực hiện các nhiệm vụ cần khả năng xử lý thông minh. **Machine Learning (ML)** là hướng tiếp cận trong đó mô hình học quy luật từ dữ liệu. **Deep Learning (DL)** là một nhóm phương pháp ML sử dụng mạng nơ-ron nhiều lớp.
- **Trong dự án:** YOLO11 thuộc Deep Learning; mục tiêu là học đặc điểm hình ảnh của linh kiện IoT để nhận diện chúng.

### 2. Computer Vision

- **Định nghĩa:** *Thị giác máy tính* là lĩnh vực nghiên cứu cách máy tính xử lý và suy ra thông tin từ ảnh hoặc video.
- **Trong dự án:** Vision Grader sử dụng Computer Vision để có bằng chứng về linh kiện xuất hiện trong ảnh bộ kit.

### 3. Object Detection

- **Định nghĩa:** *Phát hiện đối tượng* là bài toán dự đoán cả **loại đối tượng** và **vị trí** của đối tượng trong ảnh.
- **Phân biệt:** *Image classification* chỉ phân loại ảnh, ví dụ “ảnh có Arduino”, còn Object Detection trả về “Arduino ở khung tọa độ này”, và có thể tìm nhiều linh kiện trong cùng ảnh.

### 4. YOLO và YOLO11n

- **Định nghĩa:** **YOLO (You Only Look Once)** là họ mô hình Object Detection thực hiện dự đoán vị trí và class từ ảnh. **YOLO11** là một thế hệ mô hình thuộc họ này. Ký tự **n (nano)** chỉ biến thể quy mô nhỏ.
- **Trong notebook:** `yolo11n.pt` được dùng làm trọng số pretrained trước fine-tuning. Kích thước nano có ưu thế gọn, nhưng muốn chứng minh nhanh/chính xác hơn bản khác phải benchmark trên cùng điều kiện.

### 5. Dataset và class

- **Định nghĩa:** **Dataset** là tập mẫu dùng để huấn luyện hoặc đánh giá; **class** là một nhãn loại đối tượng mà mô hình cần phân biệt.
- **Trong notebook:** Cấu hình dùng **33 class** linh kiện IoT. “33 class” không có nghĩa một ảnh chứa 33 linh kiện, cũng không bảo đảm mỗi class có số ảnh ngang nhau.

### 6. Instance

- **Định nghĩa:** *Một instance* là một vật thể được gán nhãn riêng trong ảnh. Nhiều instance có thể thuộc cùng một class.
- **Ví dụ:** Một ảnh có một Arduino và hai switch button thì có ba instance nhưng chỉ hai class xuất hiện.
- **Trong notebook:** **112 ảnh validation** chứa **841 instance**.

### 7. Annotation, Ground Truth và Bounding Box

- **Định nghĩa:** **Annotation** là dữ liệu gán nhãn do người chuẩn bị dataset cung cấp. **Ground Truth** là nhãn được dùng làm đáp án tham chiếu trong huấn luyện/đánh giá. **Bounding Box (bbox)** là khung chữ nhật bao quanh một đối tượng.
- **Ví dụ:** Một ô chữ nhật bao quanh Arduino cùng tên class `arduino` là annotation để dạy mô hình vị trí và loại linh kiện.
- **Lưu ý:** Độ chính xác nhãn thủ công ảnh hưởng trực tiếp đến chất lượng mô hình.

### 8. Định dạng nhãn YOLO và `data.yaml`

- **Định nghĩa:** Định dạng phát hiện đối tượng của YOLO thường lưu mỗi vật thể trên một dòng: `class_id x_center y_center width height`, với tọa độ tỷ lệ hóa theo kích thước ảnh (giá trị trong khoảng 0–1). `data.yaml` là tệp cấu hình đường dẫn dữ liệu và danh sách tên class.
- **Trong notebook:** Dataset được tải với định dạng Roboflow `yolov8`, sau đó tham chiếu tệp `data.yaml` để train YOLO11; tên định dạng `yolov8` không có nghĩa mô hình huấn luyện là YOLOv8.

### 9. Train, Validation, Test

- **Train:** Tập dùng để tính gradient và cập nhật trọng số.
- **Validation:** Tập không dùng cập nhật trọng số, được dùng theo dõi hiệu năng và lựa chọn mô hình/cấu hình.
- **Test:** Tập độc lập nếu có, dành cho đánh giá cuối cùng, không dùng ra quyết định trong huấn luyện.
- **Notebook:** **1.173 train images**, **112 validation images**, và **841 instance validation**. Notebook **không cung cấp kết quả đánh giá trên một tập test độc lập**.
- **Vì sao phải phân biệt:** Chỉ số validation có thể lạc quan hơn đánh giá trên ảnh thực tế hoàn toàn mới.

### 10. Roboflow và phiên bản dataset

- **Định nghĩa:** Roboflow là nền tảng hỗ trợ tổ chức dữ liệu ảnh, gán nhãn, xuất dataset và quản lý phiên bản.
- **Trong notebook:** Mã tải **version 2** của dataset AI-Grading-IoT. Đây là nguồn dữ liệu trong notebook, không phải bằng chứng rằng số ảnh hoặc nhãn của mọi phiên bản khác đều giống nhau.

## II. Thuật ngữ huấn luyện – từ đầu vào đến trọng số

### 11. Pretrained, Transfer Learning, Fine-tuning

- **Pretrained:** Mô hình đã được huấn luyện trước để có trọng số khởi tạo hữu ích.
- **Transfer Learning (học chuyển giao):** Tận dụng kiến thức từ một nhiệm vụ/dataset nguồn cho nhiệm vụ/dataset đích.
- **Fine-tuning (tinh chỉnh):** Tiếp tục cập nhật trọng số pretrained bằng dataset của bài toán mới.
- **Trong dự án:** Khởi tạo `yolo11n.pt` rồi fine-tune để nhận diện linh kiện trong bộ kit.

### 12. Weight, Parameter, Gradient, Optimizer

- **Weight / Parameter (trọng số / tham số):** Các giá trị số mà mô hình học và lưu để biến đổi đầu vào thành dự đoán.
- **Gradient (đạo hàm):** Mức độ và chiều hướng loss thay đổi khi thay đổi một tham số; được dùng để quyết định hướng cập nhật.
- **Optimizer (bộ tối ưu):** Thuật toán cập nhật tham số dựa trên gradient nhằm giảm loss. **AdamW** là một thuật toán optimizer.
- **Trong notebook:** Log thể hiện `optimizer=auto` và trình huấn luyện đã chọn **AdamW**. Vì vậy nên nói “optimizer được Ultralytics tự động lựa chọn”, không nói nhóm đã tự điều chỉnh thủ công nếu không có bằng chứng.

### 13. Epoch và iteration/step

- **Epoch:** Một lượt đi hết tập ảnh train.
- **Iteration / training step:** Một lần xử lý batch và thực hiện bước cập nhật mô hình.
- **Trong notebook:** `epochs=50` nghĩa là huấn luyện **50 epoch**. Một epoch có khoảng 74 batch nên tổng số bước xử lý batch khoảng `50 × 74 = 3.700`, chỉ là phép tính khái quát từ số batch trong log.

### 14. Batch và batch size

- **Batch:** Một nhóm mẫu ảnh được xử lý cùng nhau trong một bước huấn luyện.
- **Batch size:** Số mẫu dự kiến của một batch.
- **Trong notebook:** `batch=16` với 1.173 ảnh. Tính `ceil(1173 / 16) = 74` batch/epoch, khớp nhật ký `74/74`. Batch cuối có thể ít ảnh hơn.
- **Đánh đổi:** Batch lớn dùng nhiều bộ nhớ hơn; batch nhỏ có thể làm gradient nhiễu hơn, tùy bài toán và cài đặt.

### 15. Image size, pixel và preprocessing

- **Pixel:** Điểm ảnh; ảnh số được biểu diễn bằng các pixel.
- **Image size / `imgsz`:** Kích thước đầu vào mà pipeline mô hình sử dụng khi xử lý ảnh.
- **Preprocessing (tiền xử lý):** Biến đổi đầu vào để phù hợp yêu cầu mô hình, chẳng hạn resize và padding.
- **Trong notebook:** `imgsz=640`. Không được suy ra mọi ảnh chụp gốc có độ phân giải chính xác 640 × 640.

### 16. GPU, CUDA và `device=0`

- **GPU:** Bộ xử lý song song thường dùng để tăng tốc tính toán Deep Learning.
- **CUDA:** Nền tảng tính toán trên GPU NVIDIA được các framework sử dụng.
- **`device=0`:** Chọn GPU đánh số 0 trong môi trường đang chạy.
- **Trong notebook:** Môi trường Kaggle hiển thị **NVIDIA Tesla T4**, không có nghĩa chạy web inference bắt buộc dùng Tesla T4.

### 17. Data Augmentation

- **Định nghĩa:** Tạo biến thể của dữ liệu huấn luyện bằng phép biến đổi như đổi độ sáng, lật hoặc phóng to thu nhỏ nhằm giúp mô hình học đa dạng hơn.
- **Giới hạn:** Một số phép biến đổi có thể không phù hợp với bài toán xác định trạng thái LED hoặc đọc ký tự LCD. Đây là kiến thức bổ trợ; chỉ nên khẳng định phép biến đổi nào đã chạy khi có log và cấu hình tương ứng.

### 18. Loss (hàm mất mát)

- **Định nghĩa:** Đại lượng số mô tả độ sai lệch giữa dự đoán và đáp án theo mục tiêu tối ưu trong huấn luyện.
- **Cách đọc:** Với cùng loại loss và cùng điều kiện, xu hướng giảm thường cho thấy quá trình tối ưu đang có hiệu quả.
- **Không phải:** Loss = 0,2 không có nghĩa mô hình sai 20% ảnh; loss của hai thành phần khác loại cũng không so sánh trực tiếp để kết luận thành phần nào “đúng hơn”.

### 19. Box Loss

- **Định nghĩa:** Thành phần loss phản ánh sai lệch trong định vị Bounding Box.
- **Ví dụ:** Dự đoán đúng tên `arduino` nhưng khung bao lệch vị trí thật thì Box Loss có thể cao.
- **Đọc biểu đồ:** `train/box_loss` và `val/box_loss` theo epoch; loss giảm cho thấy tối ưu vị trí tốt hơn ở tập đang xét.

### 20. Classification Loss / Cls Loss

- **Định nghĩa:** Thành phần loss phạt việc mô hình gán class sai cho đối tượng.
- **Ví dụ:** Nhầm `lcd` thành `power_display`.
- **Đọc biểu đồ:** `train/cls_loss` và `val/cls_loss`; giảm thường cho thấy mô hình phân biệt nhãn tốt hơn trên tập liên quan.

### 21. Distribution Focal Loss / DFL Loss

- **Định nghĩa:** Thành phần loss giúp mô hình học **phân phối khoảng cách từ một vị trí tham chiếu đến các cạnh Bounding Box**, qua đó hỗ trợ hồi quy tọa độ tinh hơn.
- **Ví dụ trực quan:** Không chỉ đoán “cạnh phải cách khoảng 20 pixel” mà học sự phân bố xác suất trên các vị trí lân cận để suy ra vị trí phù hợp.
- **Đọc biểu đồ:** `train/dfl_loss` và `val/dfl_loss`.
- **Không phải:** DFL không phải confidence, accuracy hoặc phần trăm nhận diện đúng.

### 22. Training Loss, Validation Loss, Overfitting, Generalization

- **Training Loss:** Loss tính trên tập huấn luyện.
- **Validation Loss:** Loss tính trên tập validation.
- **Overfitting (quá khớp):** Mô hình quá phù hợp ảnh train nhưng kém trên ảnh khác điều kiện hoặc chưa thấy.
- **Generalization (khả năng tổng quát hóa):** Khả năng áp dụng kiến thức đã học cho ảnh mới.
- **Ví dụ:** Nếu train loss giảm lâu dài nhưng val loss tăng và mAP validation xấu đi, nên kiểm tra khả năng overfitting. Không kết luận chỉ từ một đường loss.

## III. Định nghĩa công thức của các chỉ số đánh giá

### 23. IoU – Intersection over Union

- **Định nghĩa:** Thước đo mức độ chồng lấp giữa Bounding Box dự đoán và Bounding Box gán nhãn thật.
- **Công thức:** `IoU = Diện tích phần giao / Diện tích phần hợp`.
- **Ví dụ:** Giao 80 pixel², hợp 100 pixel² → IoU = 0,80.
- **Ý nghĩa:** IoU = 1 nghĩa là khung trùng khớp hoàn toàn; IoU = 0 nghĩa không giao nhau. Ở ngưỡng 0,50 ví dụ trên đạt tiêu chí vị trí, còn ở 0,90 thì không đạt.

### 24. TP – True Positive

- **Định nghĩa:** Một đối tượng được phát hiện **đúng class** và được ghép đúng với Ground Truth theo quy tắc đánh giá (bao gồm ngưỡng IoU).
- **Ví dụ:** Có Arduino thật và dự đoán `arduino` với box phù hợp.

### 25. FP – False Positive

- **Định nghĩa:** Phát hiện **không được xem là đúng**, có thể do nhầm class, dự đoán thêm vật thể không tồn tại hoặc box không khớp đủ tốt.
- **Ví dụ:** Mô hình báo có `lcd` ở vùng không có LCD.

### 26. FN – False Negative

- **Định nghĩa:** Một vật thể Ground Truth tồn tại nhưng không được phát hiện thành công.
- **Ví dụ:** Có cảm biến DHT thật nhưng mô hình bỏ sót.

### 27. Precision

- **Tên tiếng Việt:** Độ chính xác của các phát hiện được đưa ra.
- **Định nghĩa:** Trong số dự đoán dương tính được tính, tỷ lệ dự đoán đúng.
- **Công thức:** `Precision = TP / (TP + FP)`.
- **Ví dụ minh họa:** TP = 8, FP = 1 → Precision = 8/9 ≈ 88,9%.
- **Kết quả notebook:** **Precision = 0,924 (92,4%)** trên tập validation.
- **Không nên nói:** “92,4% tất cả ảnh đã được phát hiện hoàn toàn đúng.”

### 28. Recall

- **Tên tiếng Việt:** Độ bao phủ / khả năng tìm được các đối tượng thực tế.
- **Định nghĩa:** Trong số vật thể thật, tỷ lệ đã được phát hiện thành công.
- **Công thức:** `Recall = TP / (TP + FN)`.
- **Ví dụ minh họa:** TP = 8, FN = 2 → Recall = 8/10 = 80%.
- **Kết quả notebook:** **Recall = 0,892 (89,2%)** trên tập validation.
- **Không nên nói:** Recall là tỷ lệ dự đoán đã đưa ra mà đúng; đó là Precision.

### 29. Confidence Score và Confidence Threshold

- **Confidence Score:** Điểm tin cậy mà mô hình gán cho mỗi Bounding Box dự đoán.
- **Confidence Threshold:** Ngưỡng tối thiểu để giữ lại một phát hiện trong quá trình lọc kết quả.
- **Ví dụ:** Nếu threshold = 0,50 (ví dụ minh họa) thì dự đoán có confidence 0,72 được giữ; dự đoán 0,30 bị loại.
- **Lưu ý:** Confidence 0,91 không tự động tương đương xác suất thực tế 91% đúng vì mô hình có thể chưa được hiệu chỉnh xác suất.

### 30. Precision–Recall Curve và Trade-off

- **Precision–Recall Curve:** Đường biểu diễn sự thay đổi của Precision theo Recall khi thay đổi ngưỡng confidence.
- **Trade-off:** Đánh đổi giữa ít báo nhầm (Precision cao) và ít bỏ sót (Recall cao).
- **Ví dụ:** Hạ threshold thường khiến phát hiện thêm vật thể, có thể tăng Recall nhưng tăng FP, từ đó giảm Precision. Không phải một quy luật tuyệt đối ở mọi ảnh.

### 31. AP – Average Precision

- **Định nghĩa:** Chỉ số tổng hợp Precision–Recall cho **một class** tại **một ngưỡng IoU**, được tính từ diện tích xấp xỉ dưới đường Precision–Recall theo quy trình của bộ đánh giá.
- **Ý nghĩa:** AP cao cho thấy mô hình phân loại/định vị class đó tốt trong nhiều ngưỡng confidence.
- **Không phải:** Tỷ lệ số ảnh chứa class đó mà mô hình nhận đúng một cách đơn giản.

### 32. mAP – Mean Average Precision

- **Định nghĩa:** Giá trị **trung bình của AP qua các class** được đánh giá, tại ngưỡng IoU tương ứng.
- **Công thức khái quát:** `mAP = (AP_1 + AP_2 + ... + AP_C) / C` với C là số class được tính vào đánh giá.
- **Ý nghĩa:** So sánh hiệu năng Object Detection tổng hợp, không phải điểm thực hành IoT.

### 33. mAP@50

- **Định nghĩa:** mAP tính với điều kiện ghép Bounding Box ở **ngưỡng IoU = 0,50**.
- **Kết quả notebook:** **0,897 = 89,7%**.
- **Cách nói đúng:** “Mô hình đạt mAP@50 bằng 0,897 trên tập validation.”
- **Cách nói sai:** “Mô hình chấm đúng 89,7% bài thực hành.”

### 34. mAP@50–95

- **Định nghĩa:** Trung bình mAP tại 10 ngưỡng IoU **0,50; 0,55; 0,60; ...; 0,95**.
- **Kết quả notebook:** **0,654 = 65,4%**.
- **Vì sao thấp hơn mAP@50:** Khi yêu cầu khung bao chính xác hơn, nhiều dự đoán không còn được tính là phát hiện đúng. Hiệu năng khác nhau giữa các class cũng ảnh hưởng chỉ số chung.

### 35. F1-score (kiến thức bổ trợ)

- **Định nghĩa:** Trung bình điều hòa của Precision và Recall, dùng để cân bằng hai khía cạnh báo nhầm và bỏ sót.
- **Công thức:** `F1 = 2 × Precision × Recall / (Precision + Recall)` khi mẫu số khác 0.
- **Lưu ý:** Đây là định nghĩa bổ trợ; notebook đang xét không báo F1 như chỉ số tổng hợp chính của thí nghiệm. Không đưa giá trị F1 tự tính vào slide như chỉ số đã được log ghi nhận.

### 36. Confusion Matrix – Ma trận nhầm lẫn

- **Định nghĩa:** Bảng so sánh lớp đúng với lớp dự đoán, giúp xem lớp nào bị nhầm với lớp nào.
- **Trong Object Detection:** Có thể có hàng/cột nền hoặc cơ chế ghép bbox tùy cách bộ đánh giá tạo ma trận.
- **Trong notebook:** Có phần mở `runs/detect/train/confusion_matrix.png` để trình bày ma trận của quá trình training.

## IV. Đọc biểu đồ, mô hình và kết quả thực nghiệm

### 37. `results.csv` và ba biểu đồ Training Loss

- **`results.csv`:** Tệp ghi các thống kê theo epoch từ trình huấn luyện.
- **Notebook dùng các cột:** `train/box_loss`, `train/cls_loss`, `train/dfl_loss` để vẽ ba biểu đồ Training Loss.
- **Cách đọc:** Trục ngang là epoch, trục dọc là giá trị loss. Cần quan sát xu hướng qua nhiều epoch, không suy ra hiệu năng ngoài thực tế chỉ từ một điểm loss.

### 38. Biểu đồ Validation Loss và Validation Accuracy (mAP)

- **Validation Loss:** Notebook vẽ `val/box_loss`, `val/cls_loss`, `val/dfl_loss`.
- **Biểu đồ mAP:** Notebook vẽ `metrics/mAP50(B)` và `metrics/mAP50-95(B)`. Ký hiệu `(B)` thể hiện nhánh đánh giá Bounding Box (object detection).
- **Cách đọc:** mAP tăng thường là tín hiệu tốt; validation loss giảm thường là tín hiệu tốt trong điều kiện so sánh phù hợp. Khi hai nhóm chỉ số có xu hướng trái ngược, cần kiểm tra thêm dữ liệu và hình dự đoán thực tế.

### 39. Checkpoint, `best.pt` và `last.pt`

- **Checkpoint:** Tệp lưu trạng thái mô hình tại một thời điểm trong huấn luyện.
- **`best.pt`:** Tệp trọng số được chọn theo tiêu chí validation của trainer, không nhất thiết là epoch cuối.
- **`last.pt`:** Tệp trọng số ở epoch cuối.
- **Notebook:** `best.pt` được lưu tại `runs/detect/train/weights/best.pt`, có dung lượng khoảng **5,5 MB** sau khi bỏ optimizer state. Đây là trọng số để đưa vào inference.

### 40. Inference, Postprocessing và NMS

- **Inference:** Dùng mô hình đã huấn luyện để tạo dự đoán từ ảnh đầu vào, không huấn luyện lại.
- **Postprocessing:** Các bước xử lý kết quả sau khi mạng dự đoán, chẳng hạn lọc bbox theo ngưỡng confidence.
- **NMS – Non-Maximum Suppression:** Kỹ thuật loại nhiều Bounding Box dự đoán chồng lấn quá mức trên cùng một vật thể, thường giữ bbox tự tin hơn.
- **Trong dự án:** Từ ảnh bộ kit, hệ thống có thể lấy class, bbox và confidence để gửi sang thành phần đánh giá bằng Rubric.
- **Lưu ý:** NMS là giải thích cơ chế phổ biến; notebook không trình bày một thí nghiệm riêng để đo hiệu quả NMS.

### 41. Per-class mAP và độ không cân bằng dữ liệu

- **Per-class mAP:** Kết quả AP/mAP được tính riêng theo từng class, giúp tìm lớp khó.
- **Notebook:** Arduino `mAP@50 = 0,995`, LCD `0,984`, DHT sensor `0,995`; buzzer `0,417`, power_ack_led `0,637`, power_vcc_led `0,627`.
- **Cảnh báo mẫu nhỏ:** `lora_module` chỉ có **1 instance validation**, `mAP@50 = 0,124`. Chỉ một đối tượng không đủ để đưa ra kết luận tin cậy về mọi ảnh LoRa trong thực tế.

### 42. Vision Grader và Vision Rubric

- **Vision Rubric:** Bộ tiêu chí giảng viên dùng để đánh giá bằng chứng hình ảnh, ví dụ phải có một bo mạch và một cảm biến cụ thể.
- **Vision Grader:** Bộ phận hệ thống xử lý ảnh và đối chiếu kết quả với tiêu chí.
- **Vai trò YOLO11:** Nhận diện linh kiện và vị trí. Không tự xác minh điện áp, dây đấu đúng hay chức năng đang chạy của vi điều khiển.

## V. Kết quả notebook – bảng tra cứu trước khi thuyết trình

| Mục | Giá trị | Định nghĩa / ý nghĩa |
|---|---:|---|
| Số class | 33 | 33 loại linh kiện được cấu hình để phân biệt |
| Train images | 1.173 | Số ảnh dùng cập nhật trọng số |
| Validation images | 112 | Số ảnh dùng đánh giá trong huấn luyện |
| Validation instances | 841 | Tổng số đối tượng được gán nhãn trong 112 ảnh validation |
| Model nền | `yolo11n.pt` | YOLO11 nano đã pretrained |
| Epochs | 50 | 50 lượt qua tập train |
| Batch size | 16 | Số ảnh mục tiêu trong một batch |
| Image size | 640 | Kích thước ảnh đầu vào mô hình |
| GPU | Tesla T4 | Phần cứng tính toán phục vụ training trên Kaggle |
| Precision | 0,924 | TP / (TP + FP) theo cách đánh giá |
| Recall | 0,892 | TP / (TP + FN) theo cách đánh giá |
| mAP@50 | 0,897 | Trung bình AP tại IoU = 0,50 |
| mAP@50–95 | 0,654 | Trung bình mAP tại IoU 0,50–0,95 |
| Tệp mô hình | `best.pt` | Checkpoint được chọn theo validation; khoảng 5,5 MB |

**Quan trọng:** Bảng thể hiện kết quả **validation được log trong notebook**, không phải thông số mà người biên soạn tự tạo. Các giá trị này không chứng minh độ chính xác chấm điểm bài thực hành end-to-end.

## VI. Câu hỏi thường gặp khi thuyết trình

**1. Precision và Recall đều đo chất lượng, vậy khác nhau thế nào?**

Precision bắt đầu từ tập **dự đoán mà mô hình báo có**, hỏi bao nhiêu phát hiện là đúng. Recall bắt đầu từ tập **linh kiện có thật trong ảnh**, hỏi mô hình tìm được bao nhiêu. Công thức lần lượt là `TP/(TP+FP)` và `TP/(TP+FN)`.

**2. Tại sao mAP@50 = 89,7% còn mAP@50–95 = 65,4%?**

mAP@50 chỉ xét tiêu chí ghép vị trí ở IoU 0,50; mAP@50–95 tổng hợp cả những ngưỡng IoU khắt khe hơn đến 0,95. Vì thế chỉ số thứ hai thường thấp hơn. Tuy nhiên cần xem ảnh và per-class metrics nếu muốn biết chính xác nguyên nhân của khoảng cách trong từng class.

**3. Loss thấp có nghĩa accuracy cao không?**

Không. Loss đo mục tiêu tối ưu trên dữ liệu nhất định, còn Precision, Recall, AP và mAP đo kết quả Object Detection theo quy tắc đánh giá. Cần xem đồng thời train loss, validation loss và metrics.

**4. Vì sao cần validation nếu đã train đủ 50 epoch?**

Vì mô hình có thể học rất tốt trên ảnh train nhưng kém trên ảnh chưa được dùng để cập nhật trọng số. Validation giúp lựa chọn checkpoint và quan sát nguy cơ overfitting.

**5. Vì sao nhóm dùng YOLO11n?**

Vì đây là biến thể nhỏ, dễ thử nghiệm và triển khai hơn trên thiết bị tài nguyên hạn chế. Chưa thể khẳng định bản nano tối ưu hơn tất cả biến thể khác khi chưa benchmark trực tiếp.

**6. mAP@50 bằng 0,897 có nghĩa 89,7% bài thực hành được AI chấm đúng không?**

Không. Đây là thước đo chất lượng phát hiện đối tượng của YOLO11 trên validation. Chấm bài cần so sánh với yêu cầu, Vision Rubric và có thể cần thêm bằng chứng hoạt động của phần cứng.

**7. Nếu giảng viên hỏi vì sao một class có kết quả thấp?**

Có thể do kích thước vật thể, góc chụp, ánh sáng, chất lượng annotation, ít mẫu hoặc sự giống nhau giữa class; nhưng để kết luận nguyên nhân phải xem ảnh, phân bố dữ liệu và lỗi thực tế của từng class.

**8. 112 ảnh và 841 đối tượng có mâu thuẫn không?**

Không. Một ảnh có thể có nhiều linh kiện được gán nhãn. Số ảnh đếm file ảnh; số instance đếm đối tượng cụ thể.

**9. Có thể dùng kết quả validation để tuyên bố mô hình hoạt động tốt trên mọi điều kiện không?**

Không. Muốn khẳng định khả năng tổng quát hóa, cần đánh giá thêm ảnh chưa xuất hiện trong quá trình lựa chọn mô hình, ở nhiều điều kiện chụp và bố trí phần cứng.

**10. Confidence 0,95 nghĩa là xác suất đúng chắc chắn 95% phải không?**

Không nhất thiết. Nó là điểm tự tin do mô hình tạo ra. Chỉ khi được kiểm tra hiệu chỉnh xác suất phù hợp mới có thể diễn giải gần với xác suất thực tế.
