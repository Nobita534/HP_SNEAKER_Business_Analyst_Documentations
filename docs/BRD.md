# **BRD — HP Sneakers Store**

## **1\. Introduction**

### **1.1. Purpose**

* Xác định các yêu cầu nghiệp vụ của HP Sneakers Store phiên bản cải tiến.  
* Làm cơ sở để chuyển từ AS-IS → TO-BE sang Functional Requirements và System Design.

### **1.2. Business Context**

* Hệ thống là nền tảng bán giày trực tuyến.  
* Hỗ trợ khách hàng mua hàng và thanh toán.  
* Hỗ trợ Admin quản lý sản phẩm, đơn hàng và tồn kho.  
* Phiên bản cải tiến tập trung vào **inventory accuracy, order reliability và payment reliability**.

### **1.3. Business Scope**

Phạm vi gồm:

* Product Management  
* Inventory Management  
* Inventory Import  
* Order Management  
* Payment  
* Customer Purchase Flow

### **1.4. Out of Scope**

Không đi sâu vào:

* Database design  
* API design  
* Laravel implementation  
* Frontend implementation  
* Infrastructure implementation

---

# **2\. Business Objectives**

| ID | Mục tiêu nghiệp vụ | Mô tả |
| ----- | ----- | ----- |
| BO-01 | Giảm manual effort trong nhập kho | Giảm việc nhập từng sản phẩm/thông tin tồn kho thủ công. |
| BO-02 | Cải thiện inventory accuracy | Đảm bảo dữ liệu tồn kho chính xác và nhất quán. |
| BO-03 | Hạn chế overselling | Đảm bảo không bán vượt quá số lượng tồn thực tế. |
| BO-04 | Kiểm soát order lifecycle | Đảm bảo đơn hàng chỉ chuyển qua các trạng thái hợp lệ. |
| BO-05 | Tăng payment reliability | Đảm bảo kết quả thanh toán được xử lý chính xác và an toàn. |
| BO-06 | Cải thiện product discovery | Giúp khách hàng tìm và lựa chọn sản phẩm dễ dàng hơn. |
| BO-07 | Tăng khả năng kiểm thử business-critical workflows | Giảm rủi ro khi thay đổi các nghiệp vụ quan trọng. |
| BO-08 | Chuẩn hóa development và deployment | Đảm bảo hệ thống có môi trường phát triển và quy trình triển khai nhất quán. |

---

# **3\. Stakeholders & Users**

## **3.1. Stakeholders**

| Stakeholder | Vai trò / Mối quan tâm |
| ----- | ----- |
| Customer | Tìm kiếm, mua sản phẩm và thanh toán. |
| Admin | Quản lý sản phẩm, tồn kho và đơn hàng. |
| Supplier | Cung cấp hàng hóa và thông tin nhập kho. |
| Payment Gateway | Xử lý giao dịch thanh toán trực tuyến. |
| Development Team | Xây dựng, kiểm thử và duy trì hệ thống. |

## **3.2. Users**

### **Customer**

Nhu cầu chính:

* Xem và tìm kiếm sản phẩm.  
* Quản lý giỏ hàng.  
* Đặt hàng.  
* Thanh toán.  
* Theo dõi đơn hàng.

### **Admin**

Nhu cầu chính:

* Quản lý sản phẩm.  
* Nhập và cập nhật tồn kho.  
* Quản lý đơn hàng.  
* Theo dõi trạng thái xử lý đơn hàng.

---

# **4\. Business Requirements**

Đây là **phần trọng tâm nhất** của BRD.

## **4.1. Product Management**

| ID | Business Requirement |
| ----- | ----- |
| BR-01 | Hệ thống phải cho phép quản lý thông tin sản phẩm. |
| BR-02 | Hệ thống phải hỗ trợ quản lý các thuộc tính cần thiết của sản phẩm, bao gồm size và số lượng tồn. |
| BR-03 | Khách hàng phải có khả năng tìm kiếm và lọc sản phẩm. |

## **4.2. Inventory Management**

| ID | Business Requirement |
| ----- | ----- |
| BR-04 | Hệ thống phải cho phép Admin nhập dữ liệu hàng hóa từ file. |
| BR-05 | Hệ thống phải kiểm tra dữ liệu trước khi cập nhật tồn kho. |
| BR-06 | Hệ thống phải cho phép Admin xem trước dữ liệu trước khi xác nhận nhập kho. |
| BR-07 | Hệ thống chỉ được cập nhật tồn kho sau khi dữ liệu nhập kho được xác nhận hợp lệ. |
| BR-08 | Hệ thống phải ghi nhận các thay đổi liên quan đến nhập và cập nhật tồn kho. |

## **4.3. Order Management**

| ID | Business Requirement |
| ----- | ----- |
| BR-09 | Hệ thống phải cho phép khách hàng tạo đơn hàng từ giỏ hàng. |
| BR-10 | Hệ thống phải quản lý vòng đời đơn hàng theo các trạng thái nghiệp vụ hợp lệ. |
| BR-11 | Hệ thống phải ngăn chặn các chuyển đổi trạng thái đơn hàng không hợp lệ. |

## **4.4. Payment**

| ID | Business Requirement |
| ----- | ----- |
| BR-12 | Hệ thống phải hỗ trợ thanh toán trực tuyến. |
| BR-13 | Hệ thống phải xác minh kết quả thanh toán trước khi cập nhật trạng thái giao dịch. |
| BR-14 | Hệ thống phải đảm bảo một kết quả thanh toán không gây ra nhiều business effects ngoài mong muốn. |
| BR-15 | Hệ thống phải xử lý trường hợp thanh toán thất bại hoặc không hoàn tất. |

## **4.5. Inventory & Order Consistency**

| ID | Business Requirement |
| ----- | ----- |
| BR-16 | Hệ thống phải đảm bảo số lượng tồn kho được kiểm tra trước khi xác nhận mua hàng. |
| BR-17 | Hệ thống phải ngăn chặn việc nhiều khách hàng cùng mua vượt quá số lượng tồn thực tế. |
| BR-18 | Các cập nhật liên quan đến order và inventory phải đảm bảo tính nhất quán dữ liệu. |

## **4.6. Testing & Deployment**

| ID | Business Requirement |
| ----- | ----- |
| **BR-19** | Hệ thống phải có khả năng kiểm thử các business-critical workflows để đảm bảo các chức năng quan trọng hoạt động đúng sau khi thay đổi. |
| **BR-20** | Hệ thống phải đảm bảo quá trình triển khai được thực hiện nhất quán giữa các môi trường. |

&nbsp;

---

# **5\. Business Rules & Constraints**

## **5.1. Inventory Rules**

**BRULE-01 — Inventory Validation**

Dữ liệu nhập kho phải đáp ứng các điều kiện bắt buộc trước khi được cập nhật vào inventory.

**BRULE-02 — Import Confirmation**

Dữ liệu chỉ được cập nhật vào inventory sau khi Admin kiểm tra và xác nhận.

**BRULE-03 — Stock Availability**

Sản phẩm chỉ được xác nhận mua khi số lượng tồn đáp ứng số lượng khách hàng yêu cầu.

**BRULE-04 — Stock Consistency**

Số lượng tồn kho không được giảm xuống giá trị không hợp lệ do một giao dịch mua hàng.

---

## **5.2. Order Rules**

**BRULE-05 — Valid Order Transition**

Order chỉ được chuyển sang các trạng thái được phép trong lifecycle.

Ví dụ:

CREATED

&nbsp;&nbsp;&nbsp;↓

PAYMENT\_PENDING

&nbsp;&nbsp;&nbsp;↓

PAID

&nbsp;&nbsp;&nbsp;↓

PROCESSING

&nbsp;&nbsp;&nbsp;↓

SHIPPED

&nbsp;&nbsp;&nbsp;↓

DELIVERED

Một trạng thái không thể chuyển ngược hoặc chuyển sang trạng thái không được định nghĩa.

---

## **5.3. Payment Rules**

**BRULE-06 — Payment Verification**

Kết quả thanh toán phải được xác minh trước khi hệ thống ghi nhận giao dịch thành công.

**BRULE-07 — Duplicate Payment Processing**

Một payment result được gửi nhiều lần không được tạo ra nhiều business effects.

**BRULE-08 — Failed Payment**

Thanh toán thất bại không được làm cho đơn hàng được ghi nhận là đã thanh toán.

---

## **5.4. Import Rules**

**BRULE-09 — File Validation**

File import phải đáp ứng format và các trường dữ liệu bắt buộc.

**BRULE-10 — Invalid Records**

Các record không hợp lệ phải được xác định để Admin có thể kiểm tra và xử lý.

---

## **5.5. Testing Rules**

**BRULE-11 — Critical Workflow Testing**  
&nbsp;Các business-critical workflows phải có automated tests để đảm bảo chức năng hoạt động đúng sau khi có thay đổi.

**BRULE-12 — Regression Protection**  
&nbsp;Các thay đổi hệ thống không được làm ảnh hưởng đến các business-critical workflows đã được kiểm thử.

&nbsp;

---

## **5.6. Constraints**

* Inventory ban đầu được giả định là được cung cấp thông qua file Excel/CSV.  
* Hệ thống phụ thuộc vào external payment gateway.  
* Các nghiệp vụ inventory và payment yêu cầu tính nhất quán dữ liệu cao.  
* Các quyết định về database transaction, locking, queue... sẽ được xác định ở Technical Design.  
* Môi trường development và deployment phải được chuẩn hóa để giảm khác biệt giữa các môi trường.  
* Chi tiết về công cụ và quy trình CI/CD sẽ được xác định ở Technical Design.

---

# **6\. Expected Business Outcomes**

## **6.1. Inventory**

* Giảm thời gian nhập dữ liệu hàng hóa.  
* Giảm lỗi do nhập dữ liệu thủ công.  
* Tăng độ chính xác của inventory.  
* Có khả năng kiểm tra dữ liệu trước khi cập nhật.

## **6.2. Order**

* Order lifecycle rõ ràng hơn.  
* Giảm tình trạng order chuyển trạng thái không hợp lệ.  
* Tăng tính nhất quán giữa order và inventory.

## **6.3. Payment**

* Giảm rủi ro xử lý payment không chính xác.  
* Hạn chế ảnh hưởng của duplicate payment callback.  
* Có khả năng xử lý payment failure rõ ràng hơn.

## **6.4. Customer Experience**

* Khách hàng tìm kiếm sản phẩm dễ dàng hơn.  
* Giảm khả năng đặt mua sản phẩm không còn tồn.  
* Trải nghiệm checkout và payment đáng tin cậy hơn.

## **6.5. Operations**

* Admin giảm manual effort.  
* Quy trình nhập kho có thể kiểm soát và audit tốt hơn.  
* Các nghiệp vụ quan trọng có cơ sở để automated testing.

## **6.6. Testing & Deployment**

* Giảm rủi ro regression khi thay đổi các nghiệp vụ quan trọng.  
* Đảm bảo các business-critical workflows được kiểm tra tự động.  
* Giảm lỗi phát sinh do khác biệt giữa các môi trường.  
* Tăng tính nhất quán và reliability của quá trình deployment.

&nbsp;

---

# **7\. Success Criteria**

| ID | Success Criteria | Objective |
| ----- | ----- | ----- |
| SC-01 | Admin có thể import dữ liệu inventory từ file và kiểm tra dữ liệu trước khi xác nhận. | BO-01 |
| SC-02 | Dữ liệu không hợp lệ không được cập nhật vào inventory. | BO-02 |
| SC-03 | Inventory phản ánh chính xác các giao dịch nhập/xuất được hệ thống ghi nhận. | BO-02 |
| SC-04 | Hệ thống không cho phép xác nhận mua vượt quá số lượng tồn thực tế. | BO-03 |
| SC-05 | Order chỉ có thể chuyển qua các trạng thái hợp lệ. | BO-04 |
| SC-06 | Payment result được xác minh trước khi ghi nhận thanh toán thành công. | BO-05 |
| SC-07 | Duplicate payment result không tạo ra duplicate business effect. | BO-05 |
| SC-08 | Customer có thể tìm kiếm và lọc sản phẩm. | BO-06 |
| SC-09 | Các business-critical workflows có automated tests tương ứng. | BO-07 |
| SC-10 | Các business-critical workflows có automated tests và được kiểm tra khi hệ thống thay đổi.&nbsp; | BO-07 |
| SC-11 | Hệ thống có quy trình deployment nhất quán giữa các môi trường. | BO-08&nbsp; |

&nbsp;

&nbsp;