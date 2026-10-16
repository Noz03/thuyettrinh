# Kịch bản thuyết trình và thực nghiệm Knowledge Graph kết hợp RAG

**Người trình bày:** Duy Hoàng  
**Phạm vi:** 12 slide của phần 7 và phần 8; kịch bản demo sản phẩm đặt ở cuối buổi.  
**Hệ thống:** Diabetes Knowledge Graph + RAG với MedQuAD, Neo4j, FAISS, E5, Groq và Streamlit.

## Cách dùng kịch bản

- **LỜI NÓI:** có thể đọc hoặc diễn đạt lại khi thuyết trình.
- **THAO TÁC:** việc thực hiện trên ứng dụng, Neo4j Browser hoặc notebook.
- **QUAN SÁT:** thông tin cần chỉ cho người nghe.
- **KẾT LUẬN:** điều được phép rút ra từ thao tác vừa thực hiện.
- **DỰ PHÒNG:** lời dẫn khi kết quả khác dự kiến.

Tài liệu này hướng dẫn thao tác; các thử nghiệm mới trong tài liệu **chưa được chạy trong phiên biên soạn**. Những số liệu lịch sử được ghi rõ là kết quả đã lưu. Các cell “bổ sung để trình diễn” được viết mới nhằm gọi hàm có sẵn hoặc đọc artifact, không phải mã gốc đã xuất hiện trong notebook. Không ghi đè artifact gốc khi chạy các cell bổ sung.

Có thể trình bày theo hai nhịp:

| Phương án | Nội dung | Thời lượng gợi ý |
|---|---|---|
| Đầy đủ | 12 slide, một số thực nghiệm ngắn, demo cuối | 35–45 phút |
| Rút gọn | 12 slide, chỉ trình diễn số nút và Top-K, demo cuối | 20–25 phút |

Nếu chỉ có 15 phút, mỗi slide nói khoảng 35–45 giây; dành 4–5 phút cho demo cuối. Các thử nghiệm RRF, LLM thuần và kiểm tra nguồn chi tiết chuyển sang phần hỏi đáp.

## 1. Chuẩn bị trước buổi thuyết trình

### 1.1. Những cửa sổ nên mở sẵn

1. Bài trình chiếu: 6 hình phần 7, tiếp theo là 6 hình phần 8.
2. Streamlit: tab **AI Chat**.
3. Neo4j Browser: tab truy vấn chỉ đọc.
4. Jupyter: notebook `05_hybrid_retrieval.ipynb`, đã khởi tạo các hàm truy xuất.
5. Notebook `06_rag_groq.ipynb` nếu định trình diễn gọi LLM hoặc baseline bổ sung.
6. Các tệp kết quả lịch sử, để chuyển sang khi mạng/API không sẵn sàng.

Các hình được sinh bằng AI trong bộ slide là hình minh họa. Riêng hình “Demo hệ thống Streamlit” không phải bằng chứng ứng dụng đã chạy. Khi bắt đầu demo, chuyển sang cửa sổ ứng dụng thật.

### 1.2. Khởi động trong thư mục dự án

Các lệnh dưới đây là hướng dẫn cho người trình bày, không phải nhật ký đã chạy lại:

```bash
cd /home/nozoe/Documents/Semester1_Year4/IntelligentSystem/Assigment7/diabetes-kg-rag
docker compose up -d neo4j
uv run streamlit run app/streamlit_app.py
```

Mở địa chỉ Streamlit thực tế được terminal thông báo. Với cổng mặc định, thường là `http://localhost:8501`. Neo4j Browser theo cấu hình compose của dự án ở `http://localhost:7474`.

Nếu `uv` hoặc môi trường Python chưa hoạt động, sửa và chạy thử từ trước buổi trình bày. Không dành phần thuyết trình để cài lại môi trường. Không mở `.env` trên màn hình trình chiếu; dùng tài khoản và cấu hình đã chuẩn bị.

### 1.3. Kiểm tra các artifact bắt buộc

- `data/processed/medical_documents.csv`.
- `data/processed/vector_index/medquad_e5.faiss`.
- `data/processed/vector_index/chunks.csv`.
- `data/processed/vector_index/index_config.json`.
- `data/processed/knowledge_graph/nodes.csv` và `edges.csv`.
- Neo4j có dữ liệu đã nhập; chỉ khởi động container không tự tạo lại KG.
- Có `GROQ_API_KEY` nếu muốn sinh câu trả lời thật.

Trong mã hiện tại, việc tạo dịch vụ hỏi đáp luôn khởi tạo Neo4j, kể cả khi chọn Vector RAG. Vì vậy cần giữ Neo4j hoạt động ở cả hai chế độ. Chỉ báo FAISS trên sidebar kiểm tra sự tồn tại của thư mục; chưa chứng minh cả ba tệp chỉ mục đều hợp lệ.

### 1.4. Giới hạn giao diện cần biết trước

| Thành phần | Thao tác thật sự có sẵn | Điều không nên hứa khi trình bày |
|---|---|---|
| Sidebar | Vector/Hybrid; Top-K từ 1 đến 8, mặc định 4 | Không có thanh chỉnh trọng số RRF hoặc kích thước chunk |
| Sources | Mã `[S#]`, document_id, source, topic, score, tối đa 1.000 ký tự đoạn hiển thị | Không phải toàn bộ tài liệu gốc |
| Chi tiết truy xuất | Chế độ, số evidence, document_ids, latency_seconds, citation_check | Không hiển thị trực tiếp toàn bộ vector_rank, graph_rank, graph_reasons |
| Knowledge Graph | Bộ lọc Topic, Concept; đồ thị và bảng dữ liệu | Tab này hiện không hiển thị cạnh FROM_SOURCE |
| Lịch sử chat | Hiển thị lại các lượt đã hỏi | Lịch sử chưa được đưa vào prompt để suy luận nhiều lượt |
| System Information | Mô tả cơ chế bằng văn bản | Không phải bảng đo tự động toàn bộ thông số đang chạy |

**Hai lưu ý riêng cho phiên bản mã hiện tại:**

- Chọn **Topic hoặc Concept riêng lẻ**. Khi cả hai đều khác rỗng, truy vấn hiện tại yêu cầu cùng một cạnh đồng thời là ABOUT_TOPIC và MENTIONS nên trả rỗng. Trước khi chuyển sang lọc Concept, trả Topic về lựa chọn rỗng; làm tương tự khi đổi ngược lại.
- Nhãn Score trong lịch sử lấy theo lựa chọn chế độ hiện tại của sidebar. Sau khi đổi mode, câu trả lời cũ có thể bị gắn nhãn mode mới. Khi so sánh, ghi lại `retrieval_mode` trong **Chi tiết truy xuất** của đúng lượt hoặc dùng hai cửa sổ riêng; không dựa vào nhãn Score của lịch sử cũ.

### 1.5. Chạy làm nóng trước

1. Mở ứng dụng, gửi một câu hỏi thử ở Vector, Top-K = 4.
2. Chờ E5 tải/khởi tạo xong; bảo đảm model đã có trong bộ nhớ đệm.
3. Mở Sources và Chi tiết truy xuất, kiểm tra có kết quả thật.
4. Chạy một lượt Hybrid và một bộ lọc graph.
5. Ghi lại một số document_id để chuẩn bị truy vấn minh họa.
6. Sau đó bấm **Cuộc trò chuyện mới** để màn hình sạch trước khi trình bày.

Lượt đầu có thể gồm thời gian khởi tạo model và kết nối. Không dùng nó để tuyên bố độ trễ đại diện. Mỗi lần gửi câu hỏi có thể phát sinh lượt gọi Groq; phần thử truy xuất bằng notebook Step 5 không gọi LLM.

## 2. Bộ câu hỏi dùng xuyên suốt

### Câu hỏi chính bằng tiếng Việt

```text
Làm thế nào để phòng ngừa bệnh tiểu đường type 2?
```

Dùng nguyên văn cho các lần đổi mode và Top-K. Không hỏi “Còn cách nào khác?” vì hệ thống hiện chưa đưa lịch sử vào prompt.

### Câu hỏi đối chiếu bằng tiếng Anh

```text
How can type 2 diabetes be prevented?
```

Đây là câu đã có trong kết quả Step 5 và Step 6. Dùng để đọc lại kết quả lịch sử hoặc quan sát riêng tác động của ngôn ngữ truy vấn. Không đồng thời đổi cả ngôn ngữ và mode rồi quy kết khác biệt chỉ do mode.

### Câu hỏi dự phòng

```text
What are common symptoms of type 2 diabetes?
```

```text
What are diabetic neuropathies?
```

### Câu ngoài phạm vi để quan sát cách xử lý

```text
Kho tài liệu này có thông tin về kết quả giải bóng đá tối qua không?
```

Ứng dụng hiện chưa có ngưỡng liên quan bảo đảm từ chối truy xuất. FAISS vẫn có thể trả tài liệu tiểu đường cho câu ngoài phạm vi. Mục đích thử là quan sát phản hồi và nhận ra giới hạn, không hứa hệ thống chắc chắn từ chối.

# PHẦN 7 — KẾT HỢP KNOWLEDGE GRAPH VÀ RAG

## Slide 1 — Lý do kết hợp Knowledge Graph và RAG

**Thời lượng:** khoảng 2 phút.  
**Mục tiêu:** người nghe hiểu hai cơ chế bổ sung nhau ở khâu truy xuất, không nhầm KG là mô hình chẩn đoán.

### LỜI NÓI

“Ở các phần trước, chúng ta đã thấy hai cách tổ chức và khai thác tri thức: biểu diễn bằng đồ thị và truy xuất tài liệu theo ngữ nghĩa. Trong phần này, em trình bày cách nhóm kết hợp hai cơ chế đó vào hệ thống hỏi đáp về bệnh tiểu đường.

Nếu chỉ lưu tài liệu dạng văn bản, thông tin có thể nằm rải rác ở nhiều câu hỏi và câu trả lời. Tìm kiếm vector giúp xác định các đoạn có ý nghĩa gần với câu hỏi của người dùng, kể cả khi cách diễn đạt không hoàn toàn giống nhau. Tuy nhiên, một số tài liệu có liên quan thông qua cùng chủ đề hoặc khái niệm chưa chắc nằm ở những vị trí đầu của kết quả vector.

Knowledge Graph bổ sung một cách nhìn khác: tài liệu này thuộc chủ đề nào, nhắc đến khái niệm nào và đến từ nguồn nào. Khi đã có vài tài liệu phù hợp làm điểm xuất phát, hệ thống có thể dùng các liên kết đó để tìm thêm ứng viên.

Vì vậy, mục tiêu của việc kết hợp là hỗ trợ lựa chọn bằng chứng và giúp kiểm tra được nguồn. Đây là mục tiêu thiết kế. Để chứng minh chất lượng trả lời được cải thiện bao nhiêu, nhóm vẫn cần một bộ đánh giá độc lập.”

### THAO TÁC

Không cần thực nghiệm ở slide này. Dùng con trỏ theo thứ tự: tài liệu → tìm kiếm ngữ nghĩa → liên kết tri thức → bằng chứng.

### QUAN SÁT VÀ KẾT LUẬN

- Truy xuất vector dựa trên tương đồng ngữ nghĩa.
- Đồ thị bổ sung liên kết giữa tài liệu với Topic/Concept.
- Không nói “KG loại bỏ hoàn toàn hallucination” hoặc “kết hợp chắc chắn chính xác hơn”.

### CÂU CHUYỂN

“Để thấy hai cơ chế này phối hợp như thế nào, em chuyển sang kiến trúc cụ thể của hệ thống đã triển khai.”

## Slide 2 — Kiến trúc hệ thống

**Thời lượng:** 2–3 phút.  
**Mục tiêu:** giải thích rõ hai nhánh lưu trữ và điểm giao nhau ở document_id.

### LỜI NÓI

“Nguồn tri thức của hệ thống gồm 230 tài liệu hỏi đáp về bệnh tiểu đường được lọc từ MedQuAD. Nhóm cũng xử lý một bộ dữ liệu bảng phục vụ dự đoán bệnh, nhưng bộ dữ liệu bệnh nhân đó không được dùng làm tài liệu hỏi đáp và không được gửi cho Groq.

Từ cùng một kho MedQuAD, nhóm xây dựng hai nhánh. Nhánh thứ nhất tạo các nút Document, Topic, Concept và Source, sau đó lưu vào Neo4j. Nhánh thứ hai chia văn bản thành 435 đoạn, tạo embedding 768 chiều bằng multilingual-e5-base rồi đưa vào FAISS.

Hai nhánh liên kết bằng document_id. Điều này giúp một tài liệu tìm được trong FAISS có thể trở thành điểm xuất phát để truy vấn Neo4j; tài liệu mở rộng từ Neo4j cũng có thể ánh xạ trở lại các đoạn văn bản trong kho vector.

Khi hỏi đáp, hệ thống chọn bằng chứng từ kết quả truy xuất, xây dựng prompt và gọi openai/gpt-oss-20b thông qua Groq. Streamlit là giao diện để người dùng nhập câu hỏi, xem nguồn và khám phá đồ thị.”

### THỰC NGHIỆM NGẮN — Xem cấu hình thật

**Nơi thực hiện:** mở tệp `data/processed/vector_index/index_config.json` trong trình soạn thảo. Không sửa tệp.

**Chỉ vào các trường:**

```json
{
  "embedding_model": "intfloat/multilingual-e5-base",
  "max_tokens": 320,
  "overlap_tokens": 48,
  "index_type": "IndexFlatIP",
  "normalized_embeddings": true,
  "embedding_dimension": 768,
  "vector_count": 435
}
```

Đây là các trường trích từ cấu hình đã lưu; tệp thực tế còn các trường khác.

**Nói trong lúc thao tác:**

“Các thông số trên slide được đối chiếu từ artifact của hệ thống. Ví dụ, 435 là số vector đã lưu và 768 là số chiều của từng vector. Kích thước payload của mỗi chunk được cấu hình tối đa 320 token, chồng lặp 48 token.”

**KẾT LUẬN:** cấu hình đã lưu xác nhận cách lập chỉ mục; nó không phải phép đo chất lượng. Muốn đổi chunk size, phải tạo lại chunk và embedding; sửa con số trong JSON không tạo ra chỉ mục mới hợp lệ.

### CÂU CHUYỂN

“Sau khi chuẩn bị hai kho dữ liệu, câu hỏi của người dùng sẽ đi qua các bước xử lý sau.”

## Slide 3 — Quy trình xử lý câu hỏi

**Thời lượng:** 2–3 phút, có thể thêm 2 phút thử Top-K.  
**Mục tiêu:** phân biệt Vector và Hybrid, giải thích Top-K đúng ý nghĩa.

### LỜI NÓI

“Đầu tiên, câu hỏi được chuẩn hóa khoảng trắng và mã hóa bằng E5 với tiền tố query. Vector câu hỏi được so sánh với các vector trong FAISS. Các kết quả chunk được gộp theo document_id để mỗi tài liệu giữ đoạn có điểm tốt nhất.

Nếu chọn Vector RAG, hệ thống lấy các kết quả vector để tạo bằng chứng. Nếu chọn Hybrid RAG, các tài liệu vector đứng đầu được dùng làm seed, Neo4j mở rộng tài liệu có chung Concept hoặc Topic, rồi hệ thống hợp nhất thứ hạng bằng RRF.

Sau khi chọn được các đoạn văn, hệ thống gắn mã S1, S2 và đưa vào prompt. LLM tổng hợp câu trả lời bằng tiếng Việt. Cuối cùng, giao diện hiển thị câu trả lời, nguồn và thông tin truy xuất.

Top-K ở giao diện là số nguồn mong muốn lấy vào bước tạo bằng chứng. Nó không có nghĩa là chỉ so sánh đúng K vector trong toàn bộ chỉ mục.”

### THỰC NGHIỆM — Đổi Top-K từ 2 lên 4

**Biến thay đổi:** Top-K.  
**Giữ nguyên:** câu hỏi tiếng Việt, Vector RAG, corpus và model.

1. Chọn **Vector RAG**; Top-K = **2**; bấm **Cuộc trò chuyện mới**.
2. Gửi nguyên văn câu hỏi chính.
3. Mở **Sources** và **Chi tiết truy xuất**; ghi số nguồn thực tế, document_ids và latency_seconds.
4. Chuyển Top-K = **4**, bắt đầu cuộc trò chuyện mới và gửi lại đúng câu hỏi.
5. Ghi thông tin tương tự, đặt hai kết quả cạnh nhau bằng ghi chú hoặc hai cửa sổ.

**Nói trong lúc chờ:**

“Ở đây em chỉ thay đổi số nguồn yêu cầu. Khi tăng Top-K, hệ thống có thể đưa thêm đoạn vào ngữ cảnh. Điều cần quan sát là số nguồn thực sự được sử dụng và nguồn mới có liên quan không, chứ không mặc định nhiều nguồn hơn thì tốt hơn.”

**QUAN SÁT:** `retrieved_chunks` là tên trường giao diện, nhưng giá trị được tính bằng số phần tử evidence sau gộp tài liệu và giới hạn ngữ cảnh. Với truy xuất hiện tại, mỗi evidence đại diện một chunk tốt nhất của một document.

**KẾT LUẬN:** thay Top-K làm thay đổi ngân sách bằng chứng; chưa chứng minh độ đúng tăng. Số evidence có thể nhỏ hơn K vì giới hạn 11.000 ký tự, cắt tối đa 4.000 ký tự mỗi đoạn và điều kiện dừng khi phần còn lại dưới 200 ký tự.

**DỰ PHÒNG:** nếu số nguồn không tăng, nói: “K là giới hạn yêu cầu, còn số đưa vào prompt chịu ràng buộc nội dung và dung lượng ngữ cảnh. Em ghi nhận kết quả thực tế thay vì ép kết quả phải bằng K.”

### CÂU CHUYỂN

“Nhánh tạo ra khác biệt của Hybrid nằm ở bước mở rộng đồ thị. Em sẽ minh họa trực tiếp phần đó.”

## Slide 4 — Neo4j cung cấp graph context

**Thời lượng:** 3–4 phút, gồm truy vấn.  
**Mục tiêu:** chứng minh cách mở rộng có thể truy vết bằng liên kết thật.

### LỜI NÓI

“Đồ thị của nhóm gồm 311 nút và 1.217 quan hệ theo lần dựng đã lưu. Các nút gồm 230 tài liệu, 59 chủ đề, 15 khái niệm và 7 nguồn. Ba loại quan hệ là MENTIONS, ABOUT_TOPIC và FROM_SOURCE.

Trong bước Hybrid, hệ thống dùng MENTIONS và ABOUT_TOPIC để tìm tài liệu có chung thực thể với các tài liệu seed. FROM_SOURCE hỗ trợ quản lý xuất xứ, nhưng không được dùng để mở rộng ứng viên trong hàm graph_expand hiện tại.

Điểm cần lưu ý là MENTIONS chỉ thể hiện một thuật ngữ xuất hiện trong tài liệu theo quy tắc trích xuất. Nó không tự chứng minh bệnh gây triệu chứng hay thuốc điều trị bệnh. Graph context ở đây hỗ trợ chọn tài liệu và giải thích vì sao tài liệu được mở rộng. Nội dung gửi cho LLM vẫn là các đoạn văn bản.”

### THỰC NGHIỆM A — Xem đúng schema

Mở Neo4j Browser, chạy truy vấn chỉ đọc:

```cypher
MATCH (d:Document)-[r:MENTIONS|ABOUT_TOPIC|FROM_SOURCE]->(e)
RETURN d, r, e
LIMIT 30;
```

**QUAN SÁT:** hướng cạnh đi từ Document đến Concept, Topic hoặc Source. Không dùng `MATCH (n) RETURN n` với toàn bộ graph khi trình chiếu vì dễ rối màn hình.

**Nói:** “Ở đây là dữ liệu đồ thị thật. Em giới hạn số cạnh để hình đủ rõ, do đó số nút đang thấy trên màn hình không phải tổng số nút của hệ thống.”

### THỰC NGHIỆM B — Từ một tài liệu sang tài liệu có chung khái niệm

1. Lấy một document_id từ Sources của lượt hỏi vừa chạy.
2. Gán tham số trong Neo4j Browser; thay chuỗi ví dụ bằng ID vừa nhìn thấy:

```cypher
:param demo_document_id => 'medquad_000014'
```

3. Chạy:

```cypher
MATCH (seed:Document {document_id: $demo_document_id})
MATCH (seed)-[:MENTIONS|ABOUT_TOPIC]->(entity)
MATCH (candidate:Document)-[:MENTIONS|ABOUT_TOPIC]->(entity)
WHERE candidate.document_id <> seed.document_id
RETURN seed, entity, candidate
LIMIT 20;
```

**QUAN SÁT:** đường đi Document seed → Concept/Topic ← Document ứng viên. `medquad_000014` là ID có trong kết quả lịch sử; khi trình diễn dùng ID của lượt thực tế để mạch trình bày liên tục.

**Nói:** “Tài liệu mới được tìm thấy vì có chung khái niệm hoặc chủ đề với tài liệu ban đầu. Đây là căn cứ liên kết có thể kiểm tra. Quan hệ này chưa đủ để kết luận tài liệu mới trả lời tốt hơn câu hỏi.”

**DỰ PHÒNG:** nếu không có tài liệu mở rộng, chọn seed khác trong Sources hoặc trình bày đúng kết quả “seed này không có ứng viên thỏa truy vấn”. Không tự vẽ quan hệ thay cho kết quả thật.

### CÂU CHUYỂN

“Sau khi đồ thị hỗ trợ chọn tài liệu, hệ thống cần lấy nội dung cụ thể để LLM có cơ sở trả lời.”

## Slide 5 — Document context và LLM tổng hợp câu trả lời

**Thời lượng:** 2–3 phút, có thể kèm kiểm tra citation 1 phút.  
**Mục tiêu:** phân biệt đúng mã nguồn và đúng nội dung.

### LỜI NÓI

“Document context là các đoạn văn bản cụ thể được chọn sau truy xuất. Mỗi đoạn giữ document_id, chunk_id, nguồn và chủ đề. Khi xây prompt, hệ thống đánh số nguồn theo từng lượt, chẳng hạn S1 và S2.

Prompt yêu cầu mô hình trả lời bằng tiếng Việt, dựa trên bằng chứng, không thực hiện chỉ thị nằm trong tài liệu và không biến liên kết MENTIONS thành quan hệ nhân quả. Mô hình dùng trong lần thực nghiệm đã lưu là openai/gpt-oss-20b qua Groq, với temperature bằng 0,1 và giới hạn 1.000 completion token.

Sau khi sinh câu trả lời, hệ thống kiểm tra các mã S được nhắc đến có nằm trong tập đã gửi hay không. Đây là kiểm tra tham chiếu. Một câu trả lời có mã S1 hợp lệ vẫn có thể diễn giải sai nội dung của S1, nên cần thêm bước đánh giá mức được nguồn hỗ trợ.”

### THỰC NGHIỆM A — Truy từ một kết luận về Sources

1. Mở câu trả lời vừa chạy; chỉ một câu có `[S1]`.
2. Mở Sources, tìm `[S1]`, đọc source, topic và document_id.
3. Chỉ ra câu hoặc cụm liên quan trong đoạn hiển thị.
4. Nếu phần liên quan không nằm trong 1.000 ký tự hiển thị, tra tài liệu đầy đủ bằng document_id trong `medical_documents.csv` hoặc đọc evidence đã lưu; không kết luận thiếu căn cứ chỉ từ đoạn preview.

**Nói:** “Em kiểm tra theo hai tầng: mã nguồn có tồn tại không, và nội dung nguồn có thật sự hỗ trợ kết luận không. Giao diện hỗ trợ tầng thứ nhất và cung cấp dữ liệu để người dùng rà soát tầng thứ hai.”

### THỰC NGHIỆM B — Kiểm tra mã trích dẫn không gọi Groq

**Cell bổ sung trong notebook, dùng môi trường dự án; chỉ kiểm tra hàm đã có:**

```python
from diabetes_kg_rag.rag.prompt_builder import verify_citations

demo_evidence = [{"id": "S1"}]
for demo_answer in [
    "Thông tin có nguồn [S1].",
    "Thông tin gắn nguồn ngoài tập [S9].",
    "Câu trả lời không có mã nguồn."
]:
    print(demo_answer)
    print(verify_citations(demo_answer, demo_evidence))
```

Đây là dữ liệu kiểm tra phần mềm giả lập, không phải câu trả lời y khoa hay dữ liệu MedQuAD. Nếu notebook chưa import được package, bảo đảm mở bằng môi trường dự án; không cài đặt giữa buổi.

**QUAN SÁT:**

- `[S1]`: invalid rỗng, has_citation true.
- `[S9]`: invalid có S9.
- Không có mã: has_citation false nhưng citation_ids_valid vẫn true, vì không có mã ngoài tập.

**KẾT LUẬN:** phải đọc cả `has_citation`, không chỉ nhìn `citation_ids_valid`. Không nói true đồng nghĩa “đã xác nhận độ đúng y khoa”.

### CÂU CHUYỂN

“Các thành phần này đã được ghép vào ứng dụng. Slide tiếp theo giới thiệu những điểm em sẽ trình diễn ở demo cuối.”

## Slide 6 — Demo hoàn chỉnh

**Thời lượng giới thiệu:** khoảng 1 phút. Demo thực tế dùng kịch bản cuối tài liệu.  
**Mục tiêu:** giới thiệu trình tự quan sát, tránh trình diễn hai lần cùng nội dung.

### LỜI NÓI

“Đây là hình minh họa các thành phần giao diện của sản phẩm. Phần demo trên ứng dụng thật em sẽ thực hiện ở cuối, sau khi trình bày các tiêu chí đánh giá.

Demo đi theo ba câu hỏi: hệ thống trả lời được như thế nào, người dùng có kiểm tra được nguồn không, và Knowledge Graph tham gia ở đâu. Em sẽ nhập cùng một câu hỏi cho Vector và Hybrid, mở Sources để so sánh bằng chứng, rồi kiểm tra liên kết trong đồ thị.

Em cũng sẽ thay đổi Top-K để minh họa cách điều chỉnh lượng bằng chứng. Những quan sát này cho thấy hệ thống vận hành và truy vết ra sao; chúng chưa thay thế đánh giá trên tập câu hỏi có nhãn.”

### THAO TÁC

Chỉ trỏ các thành phần trên hình; chưa cần mở nhiều cửa sổ. Nếu lịch thuyết trình yêu cầu demo ngay tại slide 6, dùng nguyên **Mục 5 — Demo sản phẩm cuối cùng**, rồi trở lại slide 7. Không chạy lại toàn bộ demo ở cuối.

### CÂU CHUYỂN

“Sau khi mô tả cách kết hợp, em chuyển sang đánh giá những gì hệ thống đã đạt được và những gì chưa thể kết luận.”

# PHẦN 8 — ĐÁNH GIÁ VÀ THẢO LUẬN

## Slide 7 — Đánh giá Knowledge Graph

**Thời lượng:** 2–3 phút.  
**Mục tiêu:** tách quy mô, tính nhất quán và chất lượng ngữ nghĩa.

### LỜI NÓI

“Khi đánh giá Knowledge Graph, nhóm xem xét ba tầng. Tầng thứ nhất là quy mô: số nút, số cạnh và phân bố theo loại. Tầng thứ hai là cấu trúc và khả năng truy vết: các cạnh có đầu mút hợp lệ, có tài liệu chứng cứ và có thể truy ngược về nguồn hay không. Tầng thứ ba là chất lượng ngữ nghĩa: nhận diện có đúng và có đủ khái niệm trong văn bản không.

Dữ liệu đã lưu cho thấy đồ thị có 311 nút, gồm 230 Document, 59 Topic, 15 Concept và 7 Source. Có 757 cạnh MENTIONS, 230 ABOUT_TOPIC và 230 FROM_SOURCE, tổng cộng 1.217 quan hệ.

Nhóm có các bước kiểm tra cấu trúc và nguồn trong notebook. Tuy nhiên, chưa có tập gán nhãn độc lập để đo precision hoặc recall của trích xuất khái niệm. Vì vậy, kết quả hiện tại xác nhận quy mô và cách tổ chức đồ thị, chưa xác nhận mức độ đầy đủ của tri thức y khoa.”

### THỰC NGHIỆM — Đếm theo phạm vi KG của dự án

Trong Neo4j Browser:

```cypher
MATCH (n:KGNode)
RETURN count(n) AS total_kg_nodes;
```

```cypher
MATCH (n:KGNode)
UNWIND [label IN labels(n) WHERE label <> 'KGNode'] AS node_type
RETURN node_type, count(*) AS node_count
ORDER BY node_count DESC;
```

```cypher
MATCH (a:KGNode)-[r:MENTIONS|ABOUT_TOPIC|FROM_SOURCE]->(b:KGNode)
RETURN type(r) AS relation_type, count(*) AS relation_count
ORDER BY relation_count DESC;
```

**QUAN SÁT:** nếu dữ liệu chưa thay đổi và không chứa dữ liệu bổ sung cùng schema, các giá trị sẽ tương ứng lần chạy đã lưu. Tránh đếm mọi nút trong một database có nhiều dự án rồi gọi đó là số liệu của riêng dự án này.

**DỰ PHÒNG KHI SỐ KHÁC:**

“Kết quả trực tiếp hiện tại khác artifact đã lưu. Em sẽ phân biệt hai mốc: slide mô tả lần thực nghiệm trong báo cáo, còn màn hình là trạng thái cơ sở dữ liệu hiện tại. Cần kiểm tra việc nạp thêm dữ liệu hoặc thay đổi phiên bản trước khi đối chiếu tiếp.”

**Không xóa dữ liệu hoặc chạy lại cell nhập graph ngay giữa buổi để ép số về 311.**

### CÂU CHUYỂN

“Đối với RAG, nhóm kiểm tra cả dữ liệu vector, khả năng tìm tài liệu và khả năng gọi mô hình sinh câu trả lời.”

## Slide 8 — Đánh giá RAG

**Thời lượng:** 3 phút.  
**Mục tiêu:** giải thích đúng Hit@5 và số lượt gọi API.

### LỜI NÓI

“Ở nhánh RAG, 230 tài liệu được chia thành 435 chunk và tạo embedding 768 chiều. Chỉ mục FAISS được lưu cùng metadata để ánh xạ mỗi vector về đoạn văn bản gốc.

Trong phép kiểm tra tự truy xuất, nhóm lấy 20 câu hỏi từ chính bộ dữ liệu rồi kiểm tra tài liệu nguồn có xuất hiện trong Top 5 hay không. Kết quả đã lưu là 15 trên 20, tương ứng Hit@5 bằng 0,75. Con số này không có nghĩa 75 phần trăm câu trả lời y khoa là đúng; nó chỉ mô tả một phép thử tìm lại tài liệu trên câu hỏi nguồn.

Step 5 lưu 50 dòng kết quả cho 5 câu hỏi, hai phương pháp và Top 5. Đây là 50 dòng thứ hạng, không phải 50 câu hỏi độc lập. Step 6 có ba lượt gọi Groq thành công, nhưng chỉ có hai câu hỏi phân biệt: một câu về triệu chứng và một câu phòng ngừa được chạy ở hai chế độ.

Không có ví dụ nào trong ba lượt dùng mã nguồn ngoài tập bằng chứng. Dù vậy, số mẫu rất nhỏ và phép kiểm tra mã nguồn chưa xác nhận độ đúng của câu trả lời.”

### THỰC NGHIỆM — Tính lại số liệu từ CSV đã lưu

**Cell bổ sung trong notebook; chỉ đọc kết quả lịch sử, không tải model hoặc gọi API:**

```python
from pathlib import Path
import pandas as pd

# Chạy notebook từ thư mục dự án hoặc thư mục con của dự án.
demo_root = next(
    p for p in [Path.cwd(), *Path.cwd().parents]
    if (p / "pyproject.toml").exists()
)
demo_smoke = pd.read_csv(
    demo_root / "output/results/semantic_search_smoke_test.csv"
)
demo_found = (
    demo_smoke["found_top5"].astype(str).str.strip().str.lower().eq("true")
)
print("Số câu hỏi:", len(demo_smoke))
print("Tìm được tài liệu nguồn trong Top 5:", int(demo_found.sum()))
print("Self-retrieval Hit@5:", float(demo_found.mean()))
display(demo_smoke[["document_id", "question", "found_top5"]])
```

**Nói:** “Em đang tính lại từ kết quả đã lưu, không phải vừa chạy lại truy xuất trên model. Cách này giúp đối chiếu trực tiếp con số trên slide với các bản ghi nguồn.”

**KẾT LUẬN:** xác nhận phép tổng hợp từ artifact; không gọi phép đọc CSV này là benchmark mới.

### CÂU CHUYỂN

“Những số liệu đó cho phép mô tả hệ thống đã vận hành, nhưng khi so sánh ba kiến trúc cần phân biệt thêm giữa cơ chế và chất lượng đo được.”

## Slide 9 — So sánh LLM thuần, RAG và KG + RAG

**Thời lượng:** 3 phút; thực nghiệm RRF tùy chọn 2–3 phút.  
**Mục tiêu:** so sánh công bằng và tránh kết luận từ một câu trả lời.

### LỜI NÓI

“LLM thuần tạo câu trả lời từ kiến thức nằm trong tham số mô hình mà không truy xuất kho MedQuAD của dự án. RAG bổ sung các đoạn văn bản lấy từ truy xuất vector. KG cộng RAG thêm bước mở rộng ứng viên bằng liên kết trong Neo4j trước khi chọn bằng chứng.

Về khả năng truy vết, RAG có mã tài liệu và mã nguồn; Hybrid có thêm dữ liệu giải thích liên kết mở rộng. Tuy nhiên, phần giải thích graph chi tiết hiện có trong kết quả truy xuất của mã nguồn và notebook, chưa được hiển thị đầy đủ trong giao diện chat.

Bảng trên slide là so sánh cơ chế. Dự án chưa có baseline LLM thuần được đo trên cùng tập câu hỏi và tiêu chí. Ngay cả khi một câu trả lời Hybrid có vẻ đầy đủ hơn, chúng ta vẫn phải kiểm tra liệu các ý bổ sung có được nguồn hỗ trợ hay không.

Muốn kết luận phương án nào tốt hơn, cần giữ cùng câu hỏi, corpus, model, ngân sách ngữ cảnh và cách chấm, rồi đánh giá trên nhiều mẫu độc lập.”

### THỰC NGHIỆM CHÍNH — So sánh truy xuất, chưa gọi LLM

**Nơi:** notebook Step 5, sau khi đã chạy các cell khởi tạo và định nghĩa hàm đến cell 18. Nếu driver đã đóng bởi cell cuối, khởi tạo lại kết nối trước. Không cần chạy lại các cell ghi báo cáo gốc.

**Cell bổ sung gọi các hàm có sẵn:**

```python
demo_question = "How can type 2 diabetes be prevented?"
demo_vector = vector_retrieve(demo_question, k=5)
demo_hybrid = hybrid_retrieve(demo_question, k=5)

demo_rows = []
for demo_method, demo_hits in [
    ("vector", demo_vector), ("hybrid", demo_hybrid)
]:
    for demo_rank, demo_hit in enumerate(demo_hits, start=1):
        demo_rows.append({
            "method": demo_method,
            "rank": demo_rank,
            "document_id": demo_hit["document_id"],
            "source": demo_hit["source"],
            "cosine": demo_hit.get("vector_score", demo_hit.get("cosine")),
            "rrf_score": demo_hit.get("rrf_score"),
        })
display(pd.DataFrame(demo_rows))
```

**QUAN SÁT:** ID xuất hiện, thứ hạng, cosine và RRF. Không so trực tiếp 0,8 cosine với 0,02 RRF để nói cosine “tốt hơn”; đây là hai đại lượng khác nhau.

**Nói nếu nguồn thay đổi:** “Graph expansion và RRF đã làm thay đổi thứ hạng ứng viên. Để gọi đây là cải thiện, cần chấm mức liên quan của từng tài liệu.”

**Nói nếu nguồn giống nhau:** “Với câu hỏi này, hai cấu hình chọn được các nguồn tương tự. Kết hợp graph không nhất thiết tạo ra khác biệt ở mọi câu hỏi.”

### THỰC NGHIỆM TÙY CHỌN — Thay graph_weight trong notebook

Giao diện Streamlit **không có** nút chỉnh trọng số. Hàm `hybrid_retrieve` trong notebook Step 5 có tham số này; class HybridRetriever của ứng dụng dùng trọng số cố định trong mã.

```python
demo_weight_rows = []
for demo_weight in [0.0, 0.3, 0.6, 1.0]:
    demo_hits = hybrid_retrieve(
        demo_question, k=5,
        seed_k=5, vector_k=20, graph_limit=40,
        rrf_c=60, vector_weight=1.0, graph_weight=demo_weight,
    )
    for demo_rank, demo_hit in enumerate(demo_hits, start=1):
        demo_weight_rows.append({
            "graph_weight": demo_weight,
            "rank": demo_rank,
            "document_id": demo_hit["document_id"],
            "vector_rank": demo_hit.get("vector_rank"),
            "graph_rank": demo_hit.get("graph_rank"),
            "rrf_score": demo_hit.get("rrf_score"),
        })
display(pd.DataFrame(demo_weight_rows))
```

**Giải thích:** “Trọng số càng lớn thì đóng góp thứ hạng của graph càng lớn. Đây là phân tích độ nhạy của cấu hình, chưa phải tìm được trọng số tối ưu.”

Ở graph_weight = 0, hàm vẫn chạy graph_expand và giữ hợp ứng viên. Vì vậy không gọi cấu hình này là tắt hoàn toàn graph hoặc lấy thời gian của nó làm baseline Vector thuần. Dùng `vector_retrieve` riêng làm baseline truy xuất vector.

### THỰC NGHIỆM LLM THUẦN TÙY CHỌN

Chỉ thực hiện khi còn thời gian và đã thử trước. Giao diện không có chế độ LLM thuần; dùng cell bổ sung ở **Phụ lục B**. Kết quả mới phải ghi nhận riêng, không sửa các số liệu lịch sử 3/3 lượt gọi API.

### CÂU CHUYỂN

“Từ cơ chế kết hợp và kết quả hiện có, em tổng hợp những ưu điểm thực tế cùng các giới hạn cần cải thiện.”

## Slide 10 — Ưu điểm và hạn chế

**Thời lượng:** 2 phút.  
**Mục tiêu:** thảo luận cả lợi ích và chi phí của kiến trúc.

### LỜI NÓI

“Ưu điểm đầu tiên là hệ thống kết hợp được tìm kiếm ngữ nghĩa với liên kết có cấu trúc. Ưu điểm thứ hai là khả năng truy vết: người dùng có thể đi từ mã S trong câu trả lời về đoạn văn và tài liệu cụ thể. Việc quản lý tri thức bên ngoài LLM cũng cho phép cập nhật corpus và tạo lại các artifact mà không phải huấn luyện lại mô hình sinh.

Tuy nhiên, chất lượng hệ thống vẫn bị giới hạn bởi nguồn dữ liệu. Nếu nguồn thiếu hoặc không phù hợp, việc tổng hợp có thể thiếu căn cứ. Graph expansion cũng có thể đưa thêm tài liệu ít liên quan, đặc biệt khi nhiều tài liệu cùng gắn với một khái niệm rộng.

Một hạn chế khác là chi phí vận hành: phải đồng bộ FAISS với Neo4j, giữ model embedding sẵn sàng và phụ thuộc Groq khi sinh câu trả lời. Mô hình vẫn có thể diễn đạt sai, gộp ý quá mức hoặc đưa ra kết luận không được nguồn hỗ trợ.

Do đó, có trích dẫn là điều kiện giúp kiểm tra, không phải chứng nhận tự động rằng nội dung đã đúng.”

### THỬ NGHIỆM TÙY CHỌN — Câu hỏi ngoài phạm vi

1. Vector RAG, Top-K = 4, cuộc trò chuyện mới.
2. Gửi câu hỏi ngoài phạm vi đã chuẩn bị.
3. Quan sát câu trả lời và Sources, không đọc đáp án dự kiến trước.

**Nếu mô hình thông báo thiếu thông tin:** “Ở ví dụ này, mô hình đã nhận ra bằng chứng không đủ. Cần nhiều mẫu hơn để đánh giá tính ổn định.”

**Nếu vẫn trả lời hoặc nhận tài liệu không liên quan:** “Ví dụ này cho thấy hạn chế của truy xuất Top-K khi chưa có cơ chế ngưỡng liên quan. Đây là một tình huống cần bổ sung kiểm thử và cải tiến.”

Không cố tình nhập dữ liệu cá nhân, không trình diễn như một công cụ chẩn đoán, không cần chứng minh bằng một tình huống y khoa thật của người nghe.

### CÂU CHUYỂN

“Dù còn các giới hạn đó, nhóm đã hoàn thành được những thành phần cụ thể sau.”

## Slide 11 — Kết quả đạt được

**Thời lượng:** 2 phút.  
**Mục tiêu:** tổng kết đúng phạm vi đã hiện thực.

### LỜI NÓI

“Kết quả đầu tiên là pipeline tiền xử lý tách biệt dữ liệu dự đoán với tài liệu tri thức, tạo ra 230 tài liệu MedQuAD phục vụ hỏi đáp.

Từ dữ liệu đó, nhóm xây dựng đồ thị Neo4j với 311 nút và 1.217 quan hệ, đồng thời tạo 435 vector 768 chiều trong FAISS. Hai nhánh liên kết bằng document_id và được sử dụng trong hai chiến lược truy xuất Vector và Hybrid RRF.

Ở bước sinh câu trả lời, nhóm đã tích hợp Groq và lưu các ví dụ câu trả lời tiếng Việt có mã nguồn. Mã nguồn Streamlit có ba tab để hỏi đáp, xem đồ thị và đọc thông tin hệ thống.

Nhóm cũng lưu các artifact, cấu hình và báo cáo để kiểm tra và tái lập. Tuy nhiên, việc đã có mã nguồn ứng dụng cần được phân biệt với kiểm thử đầy đủ: hiện chưa có báo cáo kiểm thử đầu cuối đủ để kết luận mọi tình huống giao diện và dịch vụ đều hoạt động ổn định.”

### THAO TÁC NGẮN

Mở thư mục `output/results`, chỉ các tệp:

- `preprocessing_summary.json`.
- `kg_build_summary.json`.
- `vector_retrieval_summary.json`.
- `hybrid_retrieval_comparison.csv`.
- `rag_groq_summary.json`.

**Nói:** “Mỗi bước đều có dữ liệu đầu ra để đối chiếu; các con số trên slide có thể truy lại về tệp kết quả, không chỉ xuất hiện trong phần mô tả.”

Không chạy lại toàn bộ notebook chỉ để chứng minh có artifact. Chỉ ra artifact là kiểm tra khả năng truy nguyên, không phải xác nhận mọi thử nghiệm có thể tái lập ngay trên môi trường hiện tại.

### CÂU CHUYỂN

“Bước phát triển tiếp theo là chuyển từ việc chứng minh pipeline hoạt động sang đánh giá hiệu quả có kiểm soát.”

## Slide 12 — Hướng phát triển

**Thời lượng:** 2 phút.  
**Mục tiêu:** trình bày kế hoạch đánh giá và cải tiến có thứ tự ưu tiên.

### LỜI NÓI

“Nhóm đề xuất hai hướng phát triển song song. Hướng thứ nhất là đánh giá độc lập. Cần xây dựng bộ câu hỏi nằm ngoài câu hỏi nguồn, gán nhãn tài liệu liên quan và chuẩn bị đáp án tham khảo. Sau đó, chạy LLM thuần, Vector RAG và KG cộng RAG trên cùng điều kiện để so sánh.

Đối với truy xuất, có thể đo Recall@K, MRR hoặc nDCG khi có nhãn phù hợp. Đối với câu trả lời, cần đánh giá mức được bằng chứng hỗ trợ, tính đầy đủ và độ đúng, thay vì chỉ kiểm tra mã S có hợp lệ không.

Hướng thứ hai là cải tiến hệ thống: mở rộng corpus có kiểm tra nguồn, cải thiện nhận diện thực thể, bổ sung reranking và đánh giá độ nhạy của RRF. Cần đánh giá riêng câu hỏi tiếng Việt vì kho tài liệu chủ yếu bằng tiếng Anh, đồng thời hoàn thiện kiểm thử đầu cuối và đo độ trễ qua nhiều lượt.

Tóm lại về kết quả nghiên cứu, nhóm đã hiện thực được pipeline kết hợp. Hiệu quả nâng cao chất lượng cần tiếp tục được kiểm chứng. Sau đây em chuyển sang sản phẩm thật để minh họa toàn bộ luồng sử dụng.”

### THAO TÁC

Không thực nghiệm các tính năng chưa có như thể đã hoàn thành. Chuyển thẳng sang cửa sổ Streamlit đã chuẩn bị.

# 3. Bảng thông số và cách thực nghiệm nhanh

| Thông số | Hiện tại | Chỉnh ở đâu | Cách thử | Kết luận được phép |
|---|---|---|---|---|
| Chế độ truy xuất | Vector/Hybrid | Sidebar | Cùng câu hỏi, cùng K | Nguồn hoặc thứ hạng có thể đổi |
| Top-K | 1–8, mặc định 4 | Sidebar | 2 → 4; tùy chọn 8 | Ngân sách và số evidence thay đổi |
| seed_k | 5 | Hàm Step 5 hoặc class retrieval | 1 → 3 → 5 | Mức mở rộng phụ thuộc seed |
| vector_k | 20 | Hàm Step 5 | 10 → 20, giữ tham số khác | Hợp ứng viên có thể đổi |
| graph_limit | 40 | Hàm Step 5 | 10 → 40 | Số ứng viên graph được giới hạn khác nhau |
| graph_weight | 0,6 | Hàm notebook Step 5 | 0 → 0,3 → 0,6 → 1 | Thứ hạng nhạy với trọng số |
| rrf_c | 60 | Hàm notebook Step 5 | Chỉ thử sau khi hiểu công thức | Thang điểm và thứ hạng có thể đổi |
| Chunk payload | 320 token | Notebook Step 4 | Tạo bộ chỉ mục mới riêng | Cần đánh giá lại, không đổi trực tiếp trong app |
| Overlap | 48 token | Notebook Step 4 | Tạo bộ chỉ mục mới riêng | Số chunk và phân đoạn có thể đổi |
| Embedding | E5, 768 chiều | Bước lập chỉ mục và truy vấn | Phải tương thích hai phía | Không đổi model query độc lập với index |
| Nhiệt độ LLM | 0,1 | Mã gọi Groq | Không đổi trong so sánh chính | Giảm nguồn biến thiên cấu hình |
| Giới hạn đầu ra | 1.000 completion token | Mã gọi Groq | Giữ cố định | Không đồng nghĩa độ dài thực tế là 1.000 |
| Ngữ cảnh | 11.000 ký tự văn bản | evidence builder | Giữ nguyên ở demo | Không phải giới hạn 11.000 token |

Các tham số có trong notebook chưa chắc có trong giao diện hoặc class ứng dụng. Đặc biệt graph_weight và rrf_c là tham số của hàm notebook; class hiện tại ghi trực tiếp hệ số 0,6 và hằng số 60 trong công thức.

# 4. Mẫu ghi kết quả ngay khi trình diễn

Chuẩn bị bảng này trong ghi chú, không điền sẵn số dự kiến như kết quả đã chạy:

| Lượt | Câu hỏi nguyên văn | Mode | Top-K | Số evidence | document_ids theo thứ tự | Latency | Có citation? | Mã ngoài tập? | Nhận xét nguồn |
|---|---|---|---:|---:|---|---|---|---|---|
| 1 |  | Vector | 2 |  |  |  |  |  |  |
| 2 |  | Vector | 4 |  |  |  |  |  |  |
| 3 |  | Hybrid | 4 |  |  |  |  |  |  |

**Cách đọc latency:** trường giao diện đo thời gian từ trước khi load_service đến sau khi tạo câu trả lời. Tùy trạng thái cache, nó có thể gồm khởi tạo tài nguyên, truy xuất, gọi API và retry. Không phải độ trễ riêng của FAISS hoặc Neo4j. Một vài lượt demo không đủ để kết luận phương pháp nào nhanh hơn.

**Cách đọc score:** Vector dùng điểm cosine của vector chuẩn hóa; Hybrid dùng RRF ở lớp ứng dụng. Notebook Step 6 lưu trường score của evidence theo cosine, trong khi ứng dụng Streamlit lấy hit.score và Hybrid hit.score là RRF. Không đối chiếu hai tệp hoặc hai giao diện như thể cùng định nghĩa điểm.

# 5. Kịch bản demo sản phẩm cuối cùng

## 5.1. Mục tiêu và bố trí

**Thời lượng:** khoảng 8–10 phút, tùy thời gian phản hồi API.  
**Câu hỏi duy nhất dùng cho phần so sánh chính:** “Làm thế nào để phòng ngừa bệnh tiểu đường type 2?”

| Chặng | Mục đích | Thời gian gợi ý |
|---|---|---|
| Mở sản phẩm | Giới thiệu phạm vi | 30 giây |
| Vector, K=4 | Chạy toàn bộ luồng hỏi đáp | 1–2 phút |
| Kiểm tra nguồn | Đi từ câu trả lời về evidence | 1 phút |
| Hybrid, K=4 | Quan sát thay đổi nguồn | 1–2 phút |
| Top-K | Điều chỉnh một tham số | 1 phút |
| Graph và cấu hình | Minh họa cấu trúc và giải thích hệ thống | 1–2 phút |
| Kết thúc | Chốt kết quả và giới hạn | 30–45 giây |

Dùng cửa sổ A cho Vector và B cho Hybrid nếu có thể. Khi cả hai dùng chung một tab Streamlit, chụp/ghi lượt A trước khi đổi mode; bắt đầu cuộc trò chuyện mới cho lượt B để tránh nhãn Score của lịch sử bị đổi theo sidebar.

## 5.2. Chặng 1 — Mở sản phẩm

**THAO TÁC:** mở Streamlit, đưa về tab AI Chat, sidebar ở Vector, Top-K = 4. Thu gọn terminal và các cửa sổ không liên quan.

**LỜI NÓI:**

“Đây là ứng dụng Diabetes Knowledge Graph cộng RAG của nhóm. Ứng dụng phục vụ hỏi đáp giáo dục dựa trên kho tài liệu MedQuAD; không thay thế tư vấn chuyên môn và không phải công cụ dự đoán bệnh từ chỉ số cá nhân.

Bên trái là lựa chọn chiến lược truy xuất và số nguồn Top-K. Phía trên có ba tab: hỏi đáp, khám phá đồ thị và thông tin hệ thống. Em sẽ bắt đầu bằng một câu hỏi và kiểm tra nguồn của câu trả lời.”

## 5.3. Chặng 2 — Hỏi bằng Vector RAG

**THAO TÁC:** chọn Vector RAG, K=4; cuộc trò chuyện mới; dán câu hỏi và gửi.

**LỜI NÓI TRONG LÚC CHỜ:**

“Với chế độ Vector, câu hỏi được E5 mã hóa rồi tìm trong FAISS. Hệ thống chọn những đoạn phù hợp, gắn mã nguồn và gửi chúng cùng câu hỏi cho Groq. Model trả lời bằng tiếng Việt dù tài liệu đầu vào chủ yếu là tiếng Anh.”

**KHI CÓ KẾT QUẢ:**

“Ở đây là đầu ra của lần chạy trực tiếp. Em không đọc toàn bộ câu trả lời mà sẽ chọn một kết luận để kiểm tra căn cứ. Các mã S1, S2 là mã nguồn theo riêng lượt hỏi này, không phải mã cố định của tài liệu trong toàn hệ thống.”

**QUAN SÁT:** câu trả lời không rỗng; có mã nguồn hay không; có cảnh báo lỗi không. Không đọc trước các khuyến nghị y khoa như thể hệ thống chắc chắn sẽ sinh đúng cùng một nội dung.

## 5.4. Chặng 3 — Kiểm tra Sources và chi tiết

**THAO TÁC:** mở Sources và Chi tiết truy xuất.

**LỜI NÓI:**

“Mỗi nguồn ở đây có document_id để truy về tài liệu, tên nguồn và chủ đề. Điểm hiển thị ở chế độ Vector thể hiện tương đồng ngữ nghĩa của đoạn được chọn, không phải xác suất câu trả lời đúng.

Em chọn câu có S1 và kiểm tra phần văn bản của S1 có hỗ trợ kết luận không. Nếu cần đọc đầy đủ, có thể tra tài liệu bằng document_id. Khung chi tiết cho biết số evidence thực tế, danh sách tài liệu, thời gian của lượt xử lý và kết quả kiểm tra mã trích dẫn.”

**ĐỌC ĐÚNG TRƯỜNG:**

- `retrieval_mode`: xác định chế độ của lượt thực tế.
- `retrieved_chunks`: số evidence sau các bước lựa chọn.
- `document_ids`: ghi lại thứ tự.
- `latency_seconds`: chỉ báo của một lượt, không tổng quát hóa.
- `citation_check.used`, `invalid`, `has_citation`, `citation_ids_valid`: đọc kết hợp.

**Nếu citation_ids_valid true nhưng has_citation false:** “Ở lượt này không có mã ngoài tập, nhưng mô hình cũng chưa trích mã nguồn; đây là hai tiêu chí khác nhau.”

## 5.5. Chặng 4 — Chạy Hybrid với cùng điều kiện

**THAO TÁC:** lưu ghi chú lượt Vector; sang cửa sổ B hoặc cuộc trò chuyện mới; chọn Hybrid RAG, K=4; gửi lại đúng câu hỏi.

**LỜI NÓI TRONG LÚC CHỜ:**

“Lần này em chỉ thay đổi chiến lược truy xuất. Hệ thống vẫn bắt đầu từ FAISS, nhưng dùng các tài liệu đứng đầu làm seed để mở rộng qua Concept hoặc Topic trong Neo4j. RRF hợp nhất hai thứ hạng trước khi chọn đoạn làm bằng chứng.”

**KHI CÓ KẾT QUẢ:** mở Sources và Chi tiết, đối chiếu danh sách document_id.

**Nếu khác nguồn hoặc thứ hạng:**

“Ở câu hỏi này, cơ chế mở rộng và hợp nhất đã thay đổi tập bằng chứng. Đây là khác biệt về kết quả truy xuất. Muốn nói tập mới tốt hơn, cần đánh giá mức liên quan và mức hỗ trợ câu trả lời.”

**Nếu trùng nguồn:**

“Với cấu hình và câu hỏi này, hai cơ chế chọn nguồn tương tự. Điều đó cho thấy Hybrid không nhất thiết tạo khác biệt trong mọi trường hợp; tác động cần được đánh giá trên nhiều câu hỏi.”

**Giải thích score:**

“Điểm ở chế độ Hybrid là điểm RRF, nên không so trực tiếp với số cosine của lượt Vector. Hai con số đều phục vụ xếp hạng nhưng có cách tính và thang giá trị khác nhau.”

Không hứa sẽ thấy graph_rank hoặc graph_reasons trong khung Chi tiết của ứng dụng. Nếu được hỏi, chuyển sang notebook Step 5 để hiển thị chi tiết đã hướng dẫn ở slide 9.

## 5.6. Chặng 5 — Điều chỉnh Top-K

**THAO TÁC:** giữ nguyên Hybrid và câu hỏi; đổi K từ 4 xuống 2; cuộc trò chuyện mới; gửi lại. Có thể bỏ chặng này nếu đã thử trực tiếp ở slide 3 hoặc thời gian còn ít.

**LỜI NÓI:**

“Ở bước này em chỉ giảm số nguồn yêu cầu để minh họa cách điều chỉnh ngữ cảnh. Các tham số nội bộ như số seed, giới hạn graph và trọng số RRF không thay đổi theo slider này.

Điều em ghi nhận là số evidence thực tế và tài liệu nào được giữ lại. Ít nguồn hơn có thể làm câu trả lời ngắn hoặc bỏ bớt thông tin; nhiều nguồn hơn cũng có thể bổ sung nội dung ít liên quan. Đây là tham số cần đánh giá chứ không phải càng lớn càng tốt.”

**KẾT LUẬN:** chỉ mô tả kết quả thấy được; không chấm độ đúng bằng độ dài câu trả lời.

## 5.7. Chặng 6 — Khám phá Knowledge Graph

**THAO TÁC:** sang tab Knowledge Graph.

1. Để cả hai bộ lọc rỗng để xem đồ thị con.
2. Chọn một **Concept có trong danh sách thực tế**, để Topic rỗng.
3. Chỉ các nút tài liệu nối với Concept; xem bảng nodes/edges.
4. Muốn thử Topic, trả Concept về rỗng rồi chọn Topic.

**LỜI NÓI:**

“Đây là một phần đồ thị lấy trực tiếp từ Neo4j. Bộ lọc giúp quan sát những tài liệu liên kết với cùng khái niệm hoặc chủ đề. Tab hiện tại giới hạn số bản ghi cạnh để hình dễ đọc; đây không phải toàn bộ 311 nút.

Các cạnh MENTIONS và ABOUT_TOPIC là liên kết tài liệu. Khi truy xuất Hybrid, hệ thống khai thác kiểu liên kết này để tìm thêm tài liệu ứng viên. Tab khám phá này độc lập với lượt chat; nó không tự hiển thị chính xác đường đi đã dùng cho câu hỏi vừa chạy.”

**Nếu cần minh họa FROM_SOURCE:** mở Neo4j Browser bằng truy vấn ở slide 4. Không nói mọi loại cạnh đều có trên tab Streamlit.

## 5.8. Chặng 7 — System Information và kết thúc

**THAO TÁC:** mở tab System Information; quay lại AI Chat nếu muốn kết thúc bằng sản phẩm.

**LỜI NÓI:**

“Tab này giải thích cách hai chiến lược hoạt động và nguồn dữ liệu gửi cho Groq. Nó cũng nhắc rằng mã nguồn hợp lệ không đồng nghĩa câu trả lời đã được xác nhận đúng.

Qua demo, em đã minh họa toàn bộ luồng: nhận câu hỏi, truy xuất bằng chứng, sinh câu trả lời tiếng Việt, kiểm tra nguồn và khám phá cấu trúc đồ thị. Điểm nhóm đã hiện thực là pipeline và khả năng truy vết. Phần cần tiếp tục là đánh giá trên tập câu hỏi độc lập, cải thiện truy xuất và hoàn thiện kiểm thử toàn hệ thống. Em xin kết thúc phần trình bày và sẵn sàng trao đổi thêm.”

# 6. Kịch bản dự phòng khi demo gặp lỗi

## 6.1. Groq không phản hồi hoặc báo lỗi

**Lời nói:**

“Ở lượt trực tiếp này, dịch vụ sinh câu trả lời chưa phản hồi thành công. Em sẽ tách kiểm tra truy xuất khỏi dịch vụ LLM, rồi mở kết quả đã lưu để minh họa đầu ra của lần thực nghiệm trước.”

**Thao tác:**

- Không gửi lặp liên tục hoặc để lộ API key.
- Dùng Step 5 để chạy truy xuất mà không gọi Groq nếu Neo4j và model vẫn hoạt động.
- Mở `rag_groq_results.json` hoặc `rag_groq_comparison.csv`.
- Nói rõ “kết quả đã lưu”, không gọi đây là phản hồi trực tiếp.

Client được cấu hình timeout và retry; không hứa 45 giây là tổng thời gian tối đa vì có thể có nhiều lần thử.

## 6.2. Neo4j không kết nối

**Lời nói:**

“Ứng dụng hiện phụ thuộc kết nối Neo4j khi khởi tạo dịch vụ, kể cả với Vector RAG. Em chuyển sang artifact đã lưu để tiếp tục trình bày, thay vì coi lỗi kết nối là kết quả đánh giá truy xuất.”

Mở `kg_build_summary.json`, `nodes.csv`, `edges.csv`; nếu notebook vector độc lập đã sẵn sàng có thể dùng Step 4. Không nói graph đang truy vấn trực tiếp nếu chỉ đọc CSV.

## 6.3. Model embedding tải lâu

**Lời nói:**

“Đây là thời gian khởi tạo hoặc tải mô hình, chưa phải thời gian truy xuất khi hệ thống đã được làm nóng. Em dùng kết quả đã chuẩn bị để giữ nhịp trình bày.”

Chuyển sang ảnh chụp thật hoặc CSV đã lưu. Không đổi model query tùy tiện vì phải tương thích với FAISS đã lập chỉ mục.

## 6.4. Graph rỗng

- Kiểm tra có đang chọn đồng thời Topic và Concept không.
- Đưa một bộ lọc về rỗng.
- Nếu vẫn rỗng, quan sát truy vấn Neo4j chỉ đọc, không tạo dữ liệu giả.

**Lời nói:** “Em sẽ kiểm tra lại điều kiện lọc và dữ liệu đang có. Không có kết quả cho điều kiện này không đồng nghĩa toàn bộ đồ thị bị mất.”

## 6.5. Câu trả lời có lỗi diễn đạt hoặc thiếu nguồn

**Lời nói:**

“Đây là đầu ra thực tế cho thấy giới hạn của bước sinh ngôn ngữ. Việc pipeline chạy và có tài liệu truy xuất chưa bảo đảm mọi câu trả lời đạt chất lượng. Đây cũng là lý do cần đánh giá mức được nguồn hỗ trợ và có bộ câu hỏi độc lập.”

Không xóa hoặc che lỗi rồi nói hệ thống luôn đúng. Nếu đọc kết quả lịch sử, có thể nhắc rằng một đầu ra từng có cách diễn đạt chưa phù hợp; kiểm tra citation chưa đủ phát hiện loại lỗi này.

# PHỤ LỤC A — Một cell đọc nhanh các số liệu lịch sử

Cell bổ sung chỉ đọc tệp, không gọi Neo4j hoặc Groq và không ghi đè dữ liệu:

```python
from pathlib import Path
import json
import pandas as pd

demo_root = next(
    p for p in [Path.cwd(), *Path.cwd().parents]
    if (p / "pyproject.toml").exists()
)
for demo_name in [
    "preprocessing_summary.json",
    "kg_build_summary.json",
    "vector_retrieval_summary.json",
    "hybrid_retrieval_summary.json",
    "rag_groq_summary.json",
]:
    demo_path = demo_root / "output/results" / demo_name
    print("\nTệp:", demo_name)
    print(json.dumps(json.loads(demo_path.read_text()), ensure_ascii=False, indent=2))
```

Chỉ chạy trên màn hình phù hợp; không in `.env`, cấu hình bí mật hoặc đối tượng client.

# PHỤ LỤC B — So sánh LLM thuần tùy chọn

## Điều kiện

- Không có sẵn chế độ LLM thuần trong ứng dụng.
- Đây là **thử nghiệm bổ sung được đề xuất**, chưa có kết quả trong báo cáo.
- Chỉ chạy trong notebook Step 6 sau khi đã khởi tạo `client`, `GROQ_MODEL`, `generate_answer` và các hàm truy xuất, còn kết nối Neo4j hoạt động.
- Chạy tạo ba request thật: một LLM thuần, một Vector, một Hybrid. Có thể tính vào quota.
- Không ghi đè `rag_groq_results.json` hoặc thay đổi số liệu lịch sử.

## Cell bổ sung

```python
demo_question = "How can type 2 diabetes be prevented?"

# Baseline bổ sung: không đưa evidence của MedQuAD vào prompt.
# Hướng dẫn khác ở chỗ không ép chỉ dùng evidence không tồn tại.
# Sự khác biệt prompt này cần được báo cáo trong phương pháp đánh giá.
demo_plain_response = client.chat.completions.create(
    model=GROQ_MODEL,
    messages=[
        {
            "role": "system",
            "content": (
                "Bạn là trợ lý giáo dục về bệnh tiểu đường. "
                "Trả lời bằng tiếng Việt; nói rõ khi không chắc chắn. "
                "Không chẩn đoán cá nhân và không kê thuốc. "
                "Không tạo mã trích dẫn hoặc nguồn tham khảo không có căn cứ."
            ),
        },
        {"role": "user", "content": demo_question},
    ],
    temperature=0.1,
    max_completion_tokens=1000,
)
print("LLM thuần — không truy xuất kho MedQuAD:")
print(demo_plain_response.choices[0].message.content or "")

# Gọi các hàm Step 6 có sẵn, không thay đổi định nghĩa gốc.
demo_vector_result = generate_answer(demo_question, mode="vector", k=4)
demo_hybrid_result = generate_answer(demo_question, mode="hybrid", k=4)
for demo_label, demo_result in [
    ("Vector RAG", demo_vector_result),
    ("Hybrid RAG", demo_hybrid_result),
]:
    print("\n", demo_label, demo_result["api_status"])
    print(demo_result["answer"])
    print(demo_result["citation_check"])
```

## Lời dẫn khi so sánh

“Em bổ sung một lượt không truy xuất corpus để minh họa khác biệt giữa kiến thức sẵn có của LLM và câu trả lời có ngữ cảnh từ MedQuAD. Đây là ba ví dụ quan sát, chưa phải benchmark. Prompt của baseline phải được mô tả rõ vì nó không nhận evidence như hai phương án RAG.”

Chấm từng kết luận có được hỗ trợ, có nguồn, có bổ sung ý ngoài tài liệu hay không. Không chọn phương án thắng chỉ từ cách viết lưu loát hoặc số lượng ý. Nếu muốn làm nghiên cứu chính thức, cần thiết kế prompt so sánh, bộ câu hỏi và rubric trước khi chạy nhiều mẫu.

# PHỤ LỤC C — Câu hỏi phản biện và gợi ý trả lời

## 1. Vì sao gọi là KG + RAG nếu cuối cùng LLM vẫn nhận văn bản?

“Knowledge Graph tham gia ở khâu lựa chọn và mở rộng ứng viên. Các tài liệu được tìm thông qua liên kết graph được ánh xạ về đoạn văn để tạo evidence. Thiết kế hiện tại không gửi toàn bộ đồ thị cho LLM, nhưng graph vẫn tác động đến tập bằng chứng được chọn.”

## 2. 311 nút có chứng minh graph tốt không?

“Không. Đây là chỉ tiêu quy mô. Chất lượng cần đánh giá thêm độ đúng của trích xuất, mức bao phủ và hiệu quả với truy xuất.”

## 3. Hit@5 bằng 0,75 có phải độ chính xác của hệ thống không?

“Không. Đó là tỷ lệ tìm lại được tài liệu nguồn trong Top 5 của 20 câu hỏi lấy từ corpus. Nó không đánh giá trực tiếp độ đúng câu trả lời.”

## 4. Vì sao không kết luận Hybrid tốt hơn?

“Nhóm chưa có bộ câu hỏi độc lập và nhãn liên quan để so sánh có kiểm soát. Thay đổi thứ hạng chưa tự chứng minh cải thiện.”

## 5. Tại sao dùng E5 đa ngôn ngữ?

“Thiết kế cần truy xuất kho tiếng Anh bằng câu hỏi có thể là tiếng Việt. Model đa ngôn ngữ phù hợp mục tiêu đó, nhưng chất lượng truy xuất tiếng Việt vẫn cần đo riêng.”

## 6. Vì sao có hai loại score?

“Vector sử dụng cosine trên vector chuẩn hóa. Hybrid hợp nhất thứ hạng bằng RRF. Chúng không phải xác suất và không cùng thang đo.”

## 7. S1 có phải tài liệu cố định không?

“Không. S1 là nhãn gán cho evidence đầu tiên trong từng lượt. document_id mới là định danh tài liệu để đối chiếu giữa các lượt.”

## 8. Có thể dùng để chẩn đoán bệnh không?

“Phạm vi hiện tại là hỏi đáp giáo dục dựa trên kho tri thức. Hệ thống không được đánh giá như công cụ chẩn đoán hoặc kê đơn.”

## 9. Top-K bằng 8 thì có chắc trả 8 nguồn không?

“Không chắc. Đây là số lượng yêu cầu; evidence còn chịu giới hạn dung lượng ngữ cảnh và số kết quả khả dụng.”

## 10. Nếu muốn chỉnh trọng số RRF ngay trên ứng dụng?

“Giao diện hiện chưa có điều khiển đó. Có thể thực nghiệm bằng tham số hàm ở notebook Step 5. Muốn điều chỉnh trên Streamlit cần bổ sung giao diện và truyền tham số qua dịch vụ.”

## 11. Có lưu hội thoại thì đã có khả năng hiểu câu hỏi tiếp nối chưa?

“Lịch sử hiện phục vụ hiển thị. Dịch vụ sinh câu trả lời nhận câu hỏi hiện tại, chưa đưa toàn bộ lịch sử vào prompt. Vì vậy demo dùng câu hỏi đầy đủ thay vì câu hỏi tiếp nối mơ hồ.”

## 12. Trọng số 0,6 có phải tối ưu không?

“Đó là cấu hình hiện có, chưa có thực nghiệm tối ưu hóa trên tập validation. Cần đánh giá nhiều cấu hình với nhãn phù hợp trước khi kết luận.”

# PHỤ LỤC D — Kiểm tra trước khi bấm trình chiếu

- [ ] Neo4j có dữ liệu và kết nối được.
- [ ] FAISS, metadata và cấu hình có đủ, dùng đúng model.
- [ ] E5 đã được làm nóng.
- [ ] Một lượt Vector và một lượt Hybrid đã chạy thử thành công hoặc có phương án dự phòng.
- [ ] Chọn Topic/Concept riêng lẻ đã được thử trước.
- [ ] Có câu hỏi chính để sao chép, tránh gõ khác giữa các lượt.
- [ ] Có bảng ghi số nguồn, ID, mode và latency.
- [ ] Các tệp kết quả lịch sử đã mở sẵn.
- [ ] Không chiếu `.env`, API key hoặc mật khẩu.
- [ ] Không dùng hình minh họa AI như ảnh chụp kết quả thật.
- [ ] Không trình bày phép thử mới như kết quả đã có trong báo cáo.
- [ ] Chỉ chạy các cell đọc/truy xuất cần thiết, không chạy lại cell ghi artifact gốc.

# Nguồn đối chiếu nội bộ

Kịch bản đối chiếu báo cáo Word, nội dung 12 slide và mã nguồn trong thư mục dự án:

```text
/home/nozoe/Documents/Semester1_Year4/IntelligentSystem/Assigment7/diabetes-kg-rag
```

Các đường dẫn dưới đây tính từ thư mục dự án:

- `app/streamlit_app.py`: tên tab, Top-K, trạng thái, cách lưu lịch sử và thông tin được hiển thị.
- `src/diabetes_kg_rag/retrieval/vector_retriever.py`: encode query, tìm chunk và gộp theo tài liệu.
- `src/diabetes_kg_rag/retrieval/graph_retriever.py`: mở rộng seed và điều kiện lọc đồ thị.
- `src/diabetes_kg_rag/retrieval/hybrid_retriever.py`: tham số mặc định và công thức RRF của ứng dụng.
- `src/diabetes_kg_rag/rag/prompt_builder.py`: ngân sách ký tự, evidence và kiểm tra mã nguồn.
- `src/diabetes_kg_rag/rag/rag_service.py`: gọi Groq, xử lý thiếu evidence và lỗi dịch vụ.
- `src/diabetes_kg_rag/config.py`: cấu hình đường dẫn và môi trường.
- `notebook/05_hybrid_retrieval.ipynb`: các hàm truy xuất có tham số phục vụ thực nghiệm.
- `notebook/06_rag_groq.ipynb`: pipeline Groq và kết quả minh họa.
- `data/processed/vector_index/index_config.json`: cấu hình chỉ mục đã lưu.
- `output/results/semantic_search_smoke_test.csv`: 20 kết quả tự truy xuất.
- `output/results/preprocessing_summary.json`, `kg_build_summary.json`, `vector_retrieval_summary.json`, `hybrid_retrieval_summary.json`, `rag_groq_summary.json`: số liệu lịch sử.
- `output/results/hybrid_retrieval_comparison.csv`, `rag_groq_results.json`, `rag_groq_comparison.csv`: chi tiết kết quả đã lưu.
- `docker-compose.yml`, `pyproject.toml`: cấu trúc khởi động và phụ thuộc dự án.

**Lưu ý phiên bản:** các giới hạn giao diện và điều kiện lọc trong kịch bản phản ánh mã đã đọc khi biên soạn. Nếu sửa ứng dụng sau đó, cần thử lại thao tác trước khi dùng kịch bản trình bày.
