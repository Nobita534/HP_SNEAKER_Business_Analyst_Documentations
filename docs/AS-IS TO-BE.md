# **System Analysis & Improvement Proposal — HP Sneakers Store**

## **1\. Current System (As-Is)**

### **1.1. Tổng quan hệ thống**

HP Sneakers Store là hệ thống thương mại điện tử cho phép khách hàng xem sản phẩm, quản lý giỏ hàng, đặt hàng và thanh toán. Admin có thể quản lý sản phẩm, đơn hàng và tồn kho.

Hệ thống hiện tại gồm:

* Laravel Backend: xử lý business logic, authentication và các request của hệ thống.  
* Blade Web Application: giao diện cho khách hàng và Admin.  
* MySQL: lưu trữ dữ liệu sản phẩm, đơn hàng, inventory và người dùng.  
* VNPay: cung cấp dịch vụ thanh toán trực tuyến.

### **1.2. Quy trình bán hàng hiện tại**

Customer  
&nbsp;↓  
&nbsp;Product  
&nbsp;↓  
&nbsp;Cart  
&nbsp;↓  
&nbsp;Checkout  
&nbsp;↓  
&nbsp;Create Order  
&nbsp;↓  
&nbsp;VNPay  
&nbsp;↓  
&nbsp;Payment Result  
&nbsp;↓  
&nbsp;Order

### **1.3. Quy trình nhập kho hiện tại**

Admin  
&nbsp;↓  
&nbsp;Nhập thông tin hàng hóa  
&nbsp;↓  
&nbsp;Tạo phiếu nhập kho  
&nbsp;↓  
&nbsp;Cập nhật Inventory

---

# **2\. Problems & Business Impact**

### **2.1. Các vấn đề hiện tại**

| ID | Vấn đề | Ảnh hưởng |
| ----- | ----- | ----- |
| P1 | Nhập dữ liệu inventory thủ công | Tốn thời gian và dễ xảy ra sai sót |
| P2 | Quy trình nhập kho chưa có bước validation rõ ràng | Dữ liệu không hợp lệ có thể ảnh hưởng inventory |
| P3 | Cập nhật tồn kho chưa thể hiện rõ cơ chế đảm bảo consistency khi có nhiều giao dịch đồng thời | Có nguy cơ inventory inconsistency hoặc overselling |
| P4 | Order lifecycle chưa được kiểm soát chặt | Có thể xảy ra trạng thái đơn hàng không hợp lệ |
| P5 | Payment workflow phụ thuộc external payment gateway | Có thể xảy ra callback lỗi hoặc xử lý duplicate |
| P6 | Search/filter sản phẩm còn đơn giản | Khó tìm sản phẩm phù hợp khi catalog tăng |
| P7 | Testing cho các critical business workflow chưa được thể hiện rõ | Khó đảm bảo hệ thống hoạt động ổn định khi thay đổi |
| P8 | Development/deployment environment chưa được chuẩn hóa hoàn toàn | Khó đảm bảo consistency giữa các môi trường |

---

### **2.2. Tác động nghiệp vụ**

Các vấn đề trên có thể dẫn đến:

* **Inventory inconsistency:** tồn kho không phản ánh chính xác trạng thái thực tế.  
* **Overselling:** nhiều khách hàng có thể cùng mua sản phẩm có số lượng tồn thấp.  
* **Order inconsistency:** trạng thái đơn hàng có thể chuyển không đúng workflow.  
* **Payment reliability issues:** callback từ VNPay có thể gây cập nhật payment/order không chính xác nếu không được xử lý phù hợp.  
* **Manual effort:** Admin phải nhập dữ liệu inventory thủ công.  
* **Poor maintainability:** business logic khó kiểm thử và kiểm soát khi hệ thống phát triển.  
* **Deployment risk:** môi trường development và production có thể khác nhau.

**Giữ nguyên về bản chất.**

---

# **3\. Improvement Objectives**

| ID | Mục tiêu |
| ----- | ----- |
| OBJ-01 | Giảm manual effort trong quá trình nhập kho |
| OBJ-02 | Đảm bảo dữ liệu inventory hợp lệ trước khi cập nhật |
| OBJ-03 | Đảm bảo inventory consistency và hạn chế overselling |
| OBJ-04 | Kiểm soát rõ vòng đời của Order |
| OBJ-05 | Tăng reliability của Payment workflow |
| OBJ-06 | Cải thiện khả năng tìm kiếm sản phẩm |
| OBJ-07 | Tăng khả năng kiểm thử các business-critical workflow |
| OBJ-08 | Chuẩn hóa development và deployment environment |

**Không cần chỉnh.**

Các Objective đang nói về **kết quả cần đạt**, không nói cụ thể phải dùng technology nào.

---

# **4\. Proposed System (To-Be)**

### **4.1. Tổng quan hệ thống đề xuất**

HP Sneakers Store V2 tiếp tục sử dụng hệ thống Web-based commerce hiện tại, nhưng cải thiện các workflow liên quan đến inventory, order, payment và product discovery.

Hệ thống mới tập trung vào bốn nhóm capability chính:

* **Inventory Management:** giảm manual effort và tăng tính chính xác trong quá trình nhập và cập nhật tồn kho.  
* **Order & Payment Reliability:** kiểm soát vòng đời đơn hàng và đảm bảo payment workflow nhất quán.  
* **Product Discovery:** cải thiện khả năng tìm kiếm và lọc sản phẩm.  
* **System Quality & Reliability:** tăng khả năng kiểm thử và chuẩn hóa quá trình development/deployment.

Kiến trúc high-level:

Customer / Admin  
&nbsp;↓  
&nbsp;Web Application  
&nbsp;↓  
&nbsp;Laravel Backend  
&nbsp;↓  
&nbsp;Business Logic  
&nbsp;↓  
&nbsp;MySQL  
&nbsp;↓  
&nbsp;External Services  
&nbsp;├── Payment Gateway  
&nbsp;└── Supporting Services

Không cần đưa **Database Transaction, Row-level Locking, Redis, Queue, Docker...** vào sơ đồ này.

---

### **4.2. Inventory Management**

Hệ thống mới chuyển từ quy trình nhập inventory thủ công sang một quy trình nhập dữ liệu có kiểm soát.

Workflow mục tiêu:

Supplier / Inventory Data  
&nbsp;↓  
&nbsp;Inventory Import  
&nbsp;↓  
&nbsp;Validation  
&nbsp;↓  
&nbsp;Review  
&nbsp;↓  
&nbsp;Confirmation  
&nbsp;↓  
&nbsp;Inventory Update

Mục tiêu là đảm bảo dữ liệu được kiểm tra trước khi ảnh hưởng đến inventory và giảm thao tác nhập liệu thủ công của Admin.

Chi tiết về file format, validation rules, transaction và implementation sẽ được xác định trong các tài liệu Requirements và Technical Design.

---

### **4.3. Order Management**

Hệ thống mới kiểm soát vòng đời Order theo một workflow rõ ràng, đảm bảo đơn hàng chỉ có thể chuyển giữa các trạng thái hợp lệ.

Workflow mục tiêu:

Order Created  
&nbsp;↓  
&nbsp;Payment Pending  
&nbsp;↓  
&nbsp;Paid  
&nbsp;↓  
&nbsp;Processing  
&nbsp;↓  
&nbsp;Shipped  
&nbsp;↓  
&nbsp;Delivered

Các transition không hợp lệ sẽ bị hệ thống từ chối.

Chi tiết về state transition rules sẽ được đặc tả trong **Business Rules / Functional Requirements**.

---

### **4.4. Payment Management**

Payment workflow được cải thiện nhằm đảm bảo kết quả thanh toán được xử lý nhất quán khi giao tiếp với external payment gateway.

Workflow mục tiêu:

Checkout  
&nbsp;↓  
&nbsp;Create Order  
&nbsp;↓  
&nbsp;Payment Processing  
&nbsp;↓  
&nbsp;Payment Gateway  
&nbsp;↓  
&nbsp;Payment Result  
&nbsp;↓  
&nbsp;Payment Verification  
&nbsp;↓  
&nbsp;Update Payment / Order

Hệ thống cần có khả năng xử lý các trường hợp như payment failure hoặc duplicate payment notification mà không gây ra trạng thái không nhất quán.

Cơ chế kỹ thuật cụ thể sẽ được xác định trong Technical Design.

---

### **4.5. Inventory Consistency**

Khi nhiều khách hàng cùng thao tác với một sản phẩm có số lượng tồn thấp, hệ thống phải đảm bảo inventory được cập nhật nhất quán.

Ví dụ:

Stock \= 1

Customer A ──┐  
&nbsp;├── Purchase  
&nbsp;Customer B ──┘

Kết quả mong muốn:

Customer A → Success  
&nbsp;Customer B → Out of Stock

Thay vì:

Customer A → Success  
&nbsp;Customer B → Success  
&nbsp;Stock → Invalid

Các cơ chế kỹ thuật để đảm bảo concurrency và consistency sẽ được xác định ở Technical Design.

---

# **5\. Gap Analysis**

| AS-IS | Gap | TO-BE |
| ----- | ----- | ----- |
| Admin nhập inventory thủ công | Manual effort cao | Controlled Inventory Import |
| Nhập dữ liệu trực tiếp | Chưa có validation workflow rõ ràng | Validated Inventory Import |
| Quy trình nhập kho đơn giản | Chưa có bước review/confirmation rõ ràng | Controlled Goods Receipt Workflow |
| Inventory update chưa đảm bảo rõ ràng trong concurrent transactions | Nguy cơ inventory inconsistency/overselling | Consistent Inventory Management |
| Order status chưa được kiểm soát chặt | Có thể xảy ra transition không hợp lệ | Controlled Order Lifecycle |
| Payment phụ thuộc external gateway | Có nguy cơ failure/duplicate notification | Reliable Payment Processing |
| Product search còn cơ bản | Product discovery hạn chế | Improved Product Search & Filtering |
| Critical workflows chưa được kiểm thử rõ ràng | Khó phát hiện regression | Business-Critical Test Coverage |
| Environment setup chưa chuẩn hóa | Environment inconsistency | Standardized Development Environment |
| Deployment còn phụ thuộc thao tác thủ công | Delivery chưa nhất quán | Standardized Software Delivery |

Đây là phần **thay thế cho Gap / Change Analysis cũ**, nhưng được chuẩn hóa theo cùng cách của TechByte.

---

# **6\. Scope của phiên bản cải tiến**

### **6.1. In Scope**

HP Sneakers Store V2 tập trung vào:

* Inventory Import và Inventory Management.  
* Data Validation trong inventory workflow.  
* Goods Receipt workflow.  
* Inventory consistency và overselling prevention.  
* Order lifecycle management.  
* Payment reliability.  
* Product Search & Filtering.  
* Business-critical testing.  
* Development/deployment standardization.

### **6.2. Out of Scope**

Các hướng sau chưa thuộc phạm vi phiên bản hiện tại vì chưa có business justification đủ mạnh:

* Microservices Architecture.  
* Kafka.  
* Kubernetes.  
* Distributed ETL/ELT Pipeline.  
* Các hệ thống event-driven phức tạp.  
* AI features ở mức phức tạp nếu chưa có use case rõ ràng.

Các công nghệ này chỉ nên được xem xét khi quy mô hệ thống hoặc business requirements thực sự cần đến.

&nbsp;

&nbsp;

&nbsp;