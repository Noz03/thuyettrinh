# KỊCH BẢN THUYẾT TRÌNH GIỚI THIỆU SẢN PHẨM AI-GRADING-IOT

**Tên sản phẩm:** AI-Grading-IoT – Hệ thống hỗ trợ đánh giá bài thực hành IoT ứng dụng trí tuệ nhân tạo

**Mã số đề tài:** 21 – SV – 2026 – VT1

**Phong cách:** Giới thiệu sản phẩm kết hợp trình diễn công nghệ

---

## PHẦN I. GIỚI THIỆU VẤN ĐỀ VÀ GIẢI PHÁP

### Slide 1. Giới thiệu sản phẩm

**Nội dung trên slide:**
- Tên sản phẩm: AI-Grading-IoT
- Thông điệp: *Ứng dụng trí tuệ nhân tạo vào đánh giá bài thực hành IoT*
- Hình ảnh giao diện hệ thống và bộ kit thực hành IoT

**Lời thuyết trình:**

Kính chào quý thầy cô và các bạn.

Trong những năm gần đây, Internet of Things, hay IoT, ngày càng được ứng dụng rộng rãi trong nhiều lĩnh vực như nhà thông minh, công nghiệp, y tế và giáo dục.

Đi cùng với sự phát triển đó, nhu cầu đào tạo các kỹ năng lập trình, thiết kế mạch và triển khai hệ thống IoT cũng ngày càng tăng.

Tuy nhiên, việc đánh giá một bài thực hành IoT không đơn thuần là kiểm tra một đoạn mã nguồn. Giảng viên còn phải xem xét phần cứng, các linh kiện được sử dụng, cách triển khai và kết quả hoạt động của hệ thống.

Xuất phát từ thực tế này, nhóm đã nghiên cứu và phát triển **AI-Grading-IoT – một nền tảng web ứng dụng trí tuệ nhân tạo nhằm hỗ trợ giảng viên quản lý, kiểm tra và đánh giá bài thực hành IoT**.

Trong phần trình bày hôm nay, em sẽ giới thiệu bài toán mà sản phẩm hướng đến, giải pháp được đề xuất, kiến trúc hệ thống, các công nghệ AI được sử dụng và cuối cùng là phần demo thực tế.

### Slide 2. Đặt vấn đề

**Nội dung trên slide:**
- Việc chấm bài thực hành IoT cần kiểm tra nhiều thành phần.
- Chấm thủ công tốn thời gian.
- Khó bảo đảm sự thống nhất giữa các lần đánh giá.
- Thiếu công cụ đánh giá kết hợp mã nguồn và phần cứng.

**Lời thuyết trình:**

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

**Nội dung trên slide:**
- Một nền tảng web quản lý toàn bộ quy trình thực hành.
- AI hỗ trợ xây dựng tiêu chí đánh giá.
- AI hỗ trợ phân tích mã nguồn.
- Computer Vision nhận diện linh kiện trong hình ảnh.
- Kết quả đánh giá có điểm số và nhận xét.

**Lời thuyết trình:**

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

**Nội dung trên slide:**
- Xây dựng nền tảng quản lý bài thực hành.
- Phát triển AI Code Grader.
- Xây dựng dataset linh kiện IoT.
- Fine-tuning mô hình YOLO11.
- Tích hợp và hoàn thiện Vision Grader.

**Lời thuyết trình:**

Để hiện thực hóa giải pháp, nhóm lựa chọn cách triển khai theo từng giai đoạn, ưu tiên xây dựng một sản phẩm MVP, hay Minimum Viable Product, trước khi mở rộng các tính năng nâng cao.

Ở giai đoạn đầu, nhóm tập trung thiết kế các cấu trúc dữ liệu cốt lõi như bài thực hành, tiêu chí chấm điểm, bài nộp của sinh viên và kết quả đánh giá.

Tiếp theo, nhóm phát triển hệ thống backend, các API và tích hợp mô hình ngôn ngữ lớn để hỗ trợ đánh giá mã nguồn.

Sau khi xây dựng các chức năng cơ bản, nhóm tiếp tục triển khai giao diện web dành cho giảng viên và sinh viên.

Song song với quá trình phát triển phần mềm, nhóm thực hiện xây dựng bộ dữ liệu hình ảnh các linh kiện trên bộ kit IoT và tiến hành fine-tuning mô hình YOLO11.

Mô hình sau huấn luyện sẽ được sử dụng trong nhánh xử lý hình ảnh của hệ thống.

Cách tiếp cận này giúp nhóm kiểm thử từng thành phần tương đối độc lập, hạn chế sự phụ thuộc giữa các module và thuận lợi hơn khi cần nâng cấp sản phẩm.

### Slide 5. Công nghệ sử dụng

**Nội dung trên slide:**

| Thành phần | Công nghệ |
|---|---|
| Frontend | React |
| Backend | Python, FastAPI |
| API và Validation | REST API, Pydantic |
| AI Code Grading | LLM thông qua Groq API |
| Object Detection | YOLO11, Ultralytics |
| Vision Reasoning | Qwen3-VL (định hướng tích hợp) |
| Testing | Pytest, Swagger UI |

**Lời thuyết trình:**

Về mặt công nghệ, hệ thống sử dụng React để xây dựng giao diện web, giúp tạo ra trải nghiệm tương tác phù hợp với cả giảng viên và sinh viên.

Ở phía backend, nhóm lựa chọn Python kết hợp với FastAPI.

FastAPI hỗ trợ xây dựng các REST API, xử lý dữ liệu và tích hợp các thành phần trí tuệ nhân tạo một cách thuận tiện.

Bên cạnh đó, Pydantic được sử dụng để định nghĩa schema và kiểm tra tính hợp lệ của dữ liệu, bảo đảm các thành phần trong hệ thống có cấu trúc nhất quán.

Đối với AI Code Grading, nhóm sử dụng mô hình ngôn ngữ lớn thông qua Groq API. Cách tiếp cận này cho phép khai thác khả năng phân tích mã nguồn của LLM mà không cần tự triển khai một mô hình ngôn ngữ lớn trên máy chủ riêng.

Đối với xử lý hình ảnh, nhóm lựa chọn YOLO11 để nhận diện các linh kiện trên bộ kit IoT.

Ngoài ra, kiến trúc Vision Grader còn định hướng kết hợp với Qwen3-VL, một mô hình ngôn ngữ thị giác, để hỗ trợ phân tích ngữ cảnh của hình ảnh.

Như vậy, sản phẩm kết hợp hai hướng AI: **LLM để phân tích và đánh giá nội dung, Computer Vision để nhận diện các thành phần phần cứng**.

### Slide 6. Kiến trúc tổng thể hệ thống

**Nội dung trên slide:**

Sơ đồ kiến trúc gồm các tầng:

1. Presentation Layer – React Web Application.
2. API Layer – FastAPI.
3. Business Logic – Practice, Rubric, Submission, Grading Services.
4. AI Layer – Groq LLM và Vision Pipeline.
5. Data Layer – Database và lưu trữ bài nộp.

**Lời thuyết trình:**

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

**Nội dung trên slide:**
- Quản lý người dùng theo vai trò.
- Quản lý khóa học và bài thực hành.
- Xây dựng và chỉnh sửa rubric.
- Nộp bài thực hành.
- AI đánh giá mã nguồn.
- Quản lý và hiển thị kết quả.
- Nhận diện linh kiện IoT bằng AI.

**Lời thuyết trình:**

Từ kiến trúc vừa trình bày, nhóm đã tổ chức sản phẩm thành các nhóm chức năng chính, tương ứng với quy trình sử dụng thực tế.

Với giảng viên, hệ thống hỗ trợ quản lý khóa học, tạo bài thực hành và thiết lập các tiêu chí đánh giá.

Với sinh viên, hệ thống cung cấp giao diện để xem yêu cầu, gửi mã nguồn và tải lên những hình ảnh cần thiết cho bài thực hành.

Sau khi bài nộp được gửi lên, AI Grading Service thực hiện phân tích những thành phần được hỗ trợ và trả về kết quả.

Kết quả không chỉ là một điểm số, mà còn hướng đến việc thể hiện nhận xét và mức độ đáp ứng các tiêu chí đánh giá.

Ngoài ra, nhóm đang mở rộng khả năng phân tích hình ảnh phần cứng trên cơ sở mô hình YOLO11 đã được fine-tune.

Sau đây, em xin trình bày cụ thể hơn về luồng sử dụng của giảng viên, sinh viên và quá trình AI xử lý bài nộp.

### Slide 8. Luồng chức năng dành cho giảng viên

**Nội dung trên slide:**

Đăng nhập → Chọn khóa học → Tạo Practice → AI đề xuất Rubric → Giảng viên kiểm tra → Công bố bài thực hành.

**Lời thuyết trình:**

Đối với giảng viên, quy trình bắt đầu bằng việc tạo một bài thực hành mới.

Khi tạo Practice, giảng viên có thể khai báo những thông tin quan trọng như mục tiêu bài học, danh sách nhiệm vụ cần thực hiện, các ràng buộc kỹ thuật và môi trường phần cứng sử dụng.

Ví dụ, một bài thực hành có thể yêu cầu sinh viên sử dụng ESP32, đọc dữ liệu cảm biến và gửi kết quả ra Serial Monitor theo một chu kỳ xác định.

Thông thường, để chấm một bài thực hành như vậy, giảng viên cần tự xây dựng bộ tiêu chí chi tiết.

Trong AI-Grading-IoT, hệ thống được thiết kế để hỗ trợ quá trình này bằng cách tự động đề xuất các rubric dựa trên nội dung Practice.

Rubric được chia thành ba nhóm: Code Rubric, Data Rubric và Vision Rubric.

Giảng viên có thể xem lại, điều chỉnh tiêu chí và trọng số trước khi sử dụng để chấm điểm.

Tính năng này hướng đến việc giảm thời gian xây dựng rubric từ đầu, đồng thời giúp những bài thực hành khác nhau có cấu trúc đánh giá rõ ràng và có thể kiểm soát được.

### Slide 9. Luồng chức năng dành cho sinh viên

**Nội dung trên slide:**

Đăng nhập → Chọn Practice → Xem yêu cầu → Nộp mã nguồn và hình ảnh → AI Grading → Xem kết quả.

**Lời thuyết trình:**

Đối với sinh viên, quy trình sử dụng được thiết kế tương đối đơn giản.

Sau khi đăng nhập, sinh viên lựa chọn bài thực hành được giảng viên công bố.

Tại đây, sinh viên có thể xem mục tiêu, các nhiệm vụ cần thực hiện và những yêu cầu kỹ thuật tương ứng.

Sau khi hoàn thành bài thực hành, sinh viên tạo bài nộp, cung cấp mã nguồn và các hình ảnh liên quan đến bộ kit phần cứng.

Bài nộp sẽ được lưu trữ trong hệ thống và liên kết với bài thực hành tương ứng.

Khi kích hoạt chức năng chấm điểm, AI Grading Service sẽ lấy thông tin bài nộp, yêu cầu bài thực hành và rubric để thực hiện đánh giá.

Sau khi hoàn thành, sinh viên có thể xem kết quả chấm, nhận xét và những nội dung cần cải thiện.

Trong phiên bản MVP, chức năng chấm lần đầu được giới hạn theo bài nộp nhằm tránh việc gửi yêu cầu đánh giá lặp lại không cần thiết.

### Slide 10. Luồng AI Code Grading

**Nội dung trên slide:**

Practice + Code Rubric + Student Code → Prompt Builder → Groq LLM API → Validate Response → Grading Result.

**Lời thuyết trình:**

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

**Nội dung trên slide:**

Hình ảnh bộ kit → Tiền xử lý → YOLO11 → Danh sách linh kiện → Qwen3-VL → Đối chiếu Vision Rubric → Kết quả.

**Lời thuyết trình:**

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

**Nội dung trên slide:**
- Thu thập hình ảnh bộ kit thực hành.
- Xác định các class linh kiện.
- Gán nhãn Bounding Box.
- Tổ chức tập dữ liệu theo định dạng YOLO.
- Phân chia dữ liệu để huấn luyện và đánh giá.

**Lời thuyết trình:**

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

**Nội dung trên slide:**

Pretrained YOLO11 → Custom IoT Dataset → Training → Validation → `best.pt` → Inference.

**Lời thuyết trình:**

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

### Slide 14. Kết quả thử nghiệm YOLO11

**Nội dung trên slide:**
- Hình ảnh đầu vào.
- Hình ảnh sau khi thực hiện Object Detection.
- Bounding Box và Class Name.
- Confidence Score.
- Các trường hợp mô hình nhận diện chưa tốt.

**Lời thuyết trình:**

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