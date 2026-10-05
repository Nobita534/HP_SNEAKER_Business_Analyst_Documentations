# **Functional Requirements Document (FRD) — HP Sneakers Store**

## **1\. Introduction**

### **1.1. Purpose**

Tài liệu này mô tả các yêu cầu chức năng của hệ thống **HP Sneakers Store** dựa trên các business requirements đã được xác định trong BRD.

FRD tập trung mô tả **hệ thống phải thực hiện những chức năng gì và hoạt động như thế nào** để đáp ứng các nhu cầu nghiệp vụ về:

* Product Management  
* Inventory Management  
* Inventory Import  
* Order Management  
* Payment  
* Customer Purchase Flow

### **1.2. Scope**

FRD tập trung vào các chức năng chính của hệ thống:

* Khách hàng xem, tìm kiếm và lọc sản phẩm.  
* Khách hàng quản lý giỏ hàng và tạo đơn hàng.  
* Khách hàng thực hiện thanh toán.  
* Hệ thống xử lý và cập nhật kết quả thanh toán.  
* Admin quản lý sản phẩm.  
* Admin import dữ liệu inventory từ Excel/CSV.  
* Hệ thống validation và preview dữ liệu inventory trước khi cập nhật.  
* Hệ thống quản lý goods receipt và cập nhật inventory.  
* Hệ thống kiểm soát inventory consistency trong quá trình đặt hàng.  
* Hệ thống quản lý order lifecycle.  
* Hệ thống xử lý các trường hợp payment failure và duplicate callback.

## **2\. Functional Requirements Overview**

Phần này tổng hợp các Functional Requirements chính của HP\_SNEAKERS được phân rã từ các Business Requirements trong BRD. Mỗi Functional Requirement mô tả một hành vi hoặc chức năng mà hệ thống cần cung cấp để đáp ứng yêu cầu nghiệp vụ.

&nbsp;

| FR ID | Functional Requirement | Related BR | Priority |
| ----- | ----- | ----- | ----- |
| **FR-01** | Quản lý thông tin sản phẩm, bao gồm các thuộc tính sản phẩm và số lượng tồn kho | BR-01, BR-02 | P0 |
| **FR-02** | Tìm kiếm và lọc sản phẩm | BR-03 | P1 |
| **FR-03** | Upload file Excel/CSV nhập kho | BR-04 | P0 |
| **FR-04** | Validate file và dữ liệu nhập kho | BR-05 | P0 |
| **FR-05** | Preview dữ liệu trước khi nhập kho | BR-06 | P0 |
| **FR-06** | Xác nhận và cập nhật inventory | BR-07 | P0 |
| **FR-07** | Ghi nhận lịch sử nhập và cập nhật inventory | BR-08 | P1 |
| **FR-08** | Tạo đơn hàng từ giỏ hàng | BR-09 | P0 |
| **FR-09** | Quản lý vòng đời đơn hàng | BR-10 | P0 |
| **FR-10** | Kiểm soát chuyển đổi trạng thái đơn hàng | BR-11 | P0 |
| **FR-11** | Khởi tạo thanh toán trực tuyến | BR-12 | P0 |
| **FR-12** | Xác minh kết quả thanh toán | BR-13 | P0 |
| **FR-13** | Xử lý duplicate payment callback | BR-14 | P0 |
| **FR-14** | Xử lý thanh toán thất bại | BR-15 | P0 |
| **FR-15** | Kiểm tra tồn kho trước khi xác nhận đơn hàng | BR-16 | P0 |
| **FR-16** | Kiểm soát concurrent inventory update | BR-17 | P0 |
| **FR-17** | Đảm bảo consistency giữa Order và Inventory | BR-18 | P0 |
| **FR-18** | Hệ thống phải hỗ trợ kiểm thử các business-critical workflows để đảm bảo chức năng hoạt động đúng sau khi thay đổi.&nbsp; | BR-19 | P0 |
| **FR-19** | Hệ thống phải hỗ trợ quy trình triển khai nhất quán giữa các môi trường.&nbsp; | BR-20 | P1 |

### **2.1.  Functional Requirement Groups**

**Product Management**

* **FR-01:** Quản lý thông tin sản phẩm, bao gồm thuộc tính và tồn kho.  
* **FR-02:** Tìm kiếm và lọc sản phẩm.

**Inventory Management**

* **FR-03:** Upload file Excel/CSV.  
* **FR-04:** Validation dữ liệu.  
* **FR-05:** Preview dữ liệu.  
* **FR-06:** Xác nhận và cập nhật inventory.  
* **FR-07:** Ghi nhận lịch sử inventory.

**Order Management**

* **FR-08:** Tạo đơn hàng.  
* **FR-09:** Quản lý order lifecycle.  
* **FR-10:** Kiểm soát order state transition.

**Payment**

* **FR-11:** Khởi tạo payment.  
* **FR-12:** Payment verification.  
* **FR-13:** Idempotent payment processing.  
* **FR-14:** Payment failure handling.

**Order & Inventory Consistency**

* **FR-15:** Stock availability check.  
* **FR-16:** Concurrent inventory control.  
* **FR-17:** Order–Inventory consistency.

**Testing & Deployment**

* **FR-18:** Kiểm thử các business-critical workflows.  
* **FR-19:** Hỗ trợ deployment nhất quán giữa các môi trường.

## **3\. Functional Requirements**

## **3.1. Product Management**

### **FR-01 — Quản lý thông tin sản phẩm**

* **Description**  
  &nbsp;Hệ thống cho phép Admin quản lý thông tin sản phẩm và các thuộc tính liên quan, bao gồm thông tin cơ bản, size và số lượng tồn kho.  
* **Actor**  
  &nbsp;Admin  
* **Preconditions**  
  * Admin đã đăng nhập.  
  * Admin có quyền quản lý sản phẩm.  
* **Input**  
  * Thông tin sản phẩm.  
  * Các thuộc tính sản phẩm.  
  * Size.  
  * Số lượng tồn kho.  
* **Main Flow**  
  * Admin truy cập chức năng quản lý sản phẩm.  
  * Hệ thống hiển thị danh sách sản phẩm.  
  * Admin tạo mới hoặc cập nhật thông tin sản phẩm.  
  * Admin nhập các thuộc tính cần thiết.  
  * Hệ thống kiểm tra dữ liệu.  
  * Hệ thống lưu thông tin sản phẩm.  
* **Output**  
  * Thông tin sản phẩm được tạo hoặc cập nhật thành công.  
  * Inventory tương ứng được ghi nhận trong hệ thống.  
* **Business Rules**  
  * Thông tin bắt buộc của sản phẩm phải được cung cấp.  
  * Số lượng tồn kho không được nhận giá trị không hợp lệ.  
* **Exception / Alternative Flow**  
  * Dữ liệu không hợp lệ → hệ thống thông báo lỗi và yêu cầu Admin chỉnh sửa.  
  * Sản phẩm không tồn tại khi cập nhật → hệ thống thông báo lỗi.  
* **Related BR**  
  * BR-01  
  * BR-02

### **FR-02 — Tìm kiếm và lọc sản phẩm**

* **Description**  
  &nbsp;Hệ thống cho phép khách hàng tìm kiếm và lọc sản phẩm dựa trên các tiêu chí phù hợp.  
* **Actor**  
  &nbsp;Customer  
* **Preconditions**  
  * Hệ thống có dữ liệu sản phẩm.  
* **Input**  
  * Từ khóa tìm kiếm.  
  * Tiêu chí lọc sản phẩm.  
* **Main Flow**  
  * Customer truy cập danh sách sản phẩm.  
  * Customer nhập từ khóa hoặc lựa chọn tiêu chí lọc.  
  * Hệ thống xử lý yêu cầu.  
  * Hệ thống trả về các sản phẩm phù hợp.  
* **Output**  
  * Danh sách sản phẩm phù hợp với điều kiện tìm kiếm/lọc.  
* **Business Rules**  
  * Chỉ hiển thị các sản phẩm phù hợp với tiêu chí tìm kiếm/lọc.  
* **Exception / Alternative Flow**  
  * Không tìm thấy sản phẩm → hệ thống hiển thị kết quả rỗng và thông báo phù hợp.  
* **Related BR**  
  * BR-03

# **3.2. Inventory Management**

### **FR-03 — Upload file Excel/CSV**

* **Description**  
  &nbsp;Hệ thống cho phép Admin upload file chứa dữ liệu hàng hóa để thực hiện nhập kho hàng loạt.  
* **Actor**  
  &nbsp;Admin  
* **Preconditions**  
  * Admin đã đăng nhập.  
  * Admin có quyền nhập kho.  
  * File dữ liệu đã được chuẩn bị.  
* **Input**  
  * File Excel/CSV.  
* **Main Flow**  
  * Admin truy cập chức năng nhập kho.  
  * Admin chọn file Excel/CSV.  
  * Hệ thống tiếp nhận file.  
  * Hệ thống đọc dữ liệu từ file.  
  * Hệ thống chuyển dữ liệu sang bước validation.  
* **Output**  
  * Dữ liệu từ file được hệ thống tiếp nhận để kiểm tra.  
* **Business Rules**  
  * File phải thuộc định dạng được hệ thống hỗ trợ.  
  * File phải chứa các trường dữ liệu cần thiết.  
* **Exception / Alternative Flow**  
  * File không đúng định dạng → từ chối upload.  
  * File không thể đọc → thông báo lỗi.  
  * File thiếu trường bắt buộc → chuyển sang trạng thái invalid.  
* **Related BR**  
  * BR-04

### **FR-04 — Validation dữ liệu nhập kho**

* **Description**  
  &nbsp;Hệ thống kiểm tra tính hợp lệ của file và từng record trước khi dữ liệu được sử dụng để cập nhật inventory.  
* **Actor**  
  &nbsp;Admin / System  
* **Preconditions**  
  * File đã được upload thành công.  
* **Input**  
  * Dữ liệu từ file Excel/CSV.  
* **Main Flow**  
  * Hệ thống đọc từng record.  
  * Hệ thống kiểm tra các trường bắt buộc.  
  * Hệ thống kiểm tra format và giá trị dữ liệu.  
  * Hệ thống xác định các record hợp lệ và không hợp lệ.  
  * Hệ thống trả kết quả validation cho Admin.  
* **Output**  
  * Danh sách record hợp lệ.  
  * Danh sách record không hợp lệ.  
  * Thông tin lỗi tương ứng.  
* **Business Rules**  
  * Record không hợp lệ không được cập nhật inventory.  
  * Dữ liệu bắt buộc phải đáp ứng các điều kiện nghiệp vụ.  
* **Exception / Alternative Flow**  
  * File không có record hợp lệ → không cho phép tiếp tục nhập kho.  
  * Có record lỗi → Admin phải xử lý hoặc loại bỏ record lỗi trước khi xác nhận.  
* **Related BR**  
  * BR-05

### **FR-05 — Preview dữ liệu nhập kho**

* **Description**  
  &nbsp;Hệ thống cho phép Admin xem trước dữ liệu đã được xử lý và kết quả validation trước khi xác nhận nhập kho.  
* **Actor**  
  &nbsp;Admin  
* **Preconditions**  
  * File đã được upload.  
  * Validation đã được thực hiện.  
* **Input**  
  * Kết quả validation.  
  * Dữ liệu hợp lệ từ file.  
* **Main Flow**  
  * Hệ thống hiển thị dữ liệu preview.  
  * Hệ thống phân biệt record hợp lệ và không hợp lệ.  
  * Admin kiểm tra dữ liệu.  
  * Admin lựa chọn tiếp tục hoặc quay lại xử lý dữ liệu.  
* **Output**  
  * Admin có thể xác nhận dữ liệu trước khi nhập kho.  
* **Business Rules**  
  * Inventory chưa được cập nhật trong bước preview.  
* **Exception / Alternative Flow**  
  * Admin phát hiện dữ liệu sai → không xác nhận và thực hiện điều chỉnh.  
  * Dữ liệu không hợp lệ → hệ thống hiển thị lỗi tương ứng.  
* **Related BR**  
  * BR-06

### **FR-06 — Xác nhận và cập nhật inventory**

* **Description**  
  &nbsp;Hệ thống cập nhật inventory dựa trên dữ liệu đã được Admin kiểm tra và xác nhận.  
* **Actor**  
  &nbsp;Admin / System  
* **Preconditions**  
  * Dữ liệu đã hoàn thành validation.  
  * Admin đã kiểm tra và xác nhận dữ liệu.  
* **Input**  
  * Dữ liệu nhập kho hợp lệ.  
  * Xác nhận của Admin.  
* **Main Flow**  
  * Admin xác nhận dữ liệu.  
  * Hệ thống tạo thông tin nhập kho.  
  * Hệ thống cập nhật inventory.  
  * Hệ thống hoàn tất giao dịch.  
  * Hệ thống thông báo kết quả cho Admin.  
* **Output**  
  * Inventory được cập nhật.  
  * Thông tin nhập kho được ghi nhận.  
* **Business Rules**  
  * Inventory chỉ được cập nhật sau khi Admin xác nhận dữ liệu hợp lệ.  
  * Việc cập nhật phải đảm bảo tính nhất quán dữ liệu.  
* **Exception / Alternative Flow**  
  * Transaction thất bại → hệ thống không ghi nhận cập nhật không hoàn chỉnh.  
  * Dữ liệu không còn hợp lệ tại thời điểm xác nhận → từ chối cập nhật.  
* **Related BR**  
  * BR-07

### **FR-07 — Ghi nhận lịch sử inventory**

* **Description**  
  &nbsp;Hệ thống ghi nhận các thay đổi liên quan đến việc nhập và cập nhật inventory.  
* **Actor**  
  &nbsp;System  
* **Preconditions**  
  * Có giao dịch nhập hoặc cập nhật inventory được thực hiện.  
* **Input**  
  * Thông tin giao dịch inventory.  
  * Dữ liệu sản phẩm.  
  * Số lượng thay đổi.  
* **Main Flow**  
  * Hệ thống hoàn tất giao dịch inventory.  
  * Hệ thống ghi nhận thông tin thay đổi.  
  * Hệ thống lưu lịch sử inventory.  
* **Output**  
  * Lịch sử thay đổi inventory được lưu lại.  
* **Business Rules**  
  * Các thay đổi inventory phải có thông tin nguồn gốc giao dịch.  
* **Exception / Alternative Flow**  
  * Không thể ghi nhận lịch sử → giao dịch inventory không được coi là hoàn tất nếu việc ghi nhận là bắt buộc.  
* **Related BR**  
  * BR-08

# **3.3. Order Management**

### **FR-08 — Tạo đơn hàng**

* **Description**  
  &nbsp;Hệ thống cho phép Customer tạo đơn hàng từ các sản phẩm đã thêm vào giỏ hàng.  
* **Actor**  
  &nbsp;Customer  
* **Preconditions**  
  * Customer có sản phẩm trong giỏ hàng.  
  * Sản phẩm có thông tin hợp lệ.  
* **Input**  
  * Danh sách sản phẩm.  
  * Số lượng.  
  * Thông tin giao hàng.  
  * Thông tin thanh toán.  
* **Main Flow**  
  * Customer truy cập checkout.  
  * Hệ thống kiểm tra thông tin giỏ hàng.  
  * Hệ thống kiểm tra khả năng đáp ứng tồn kho.  
  * Customer xác nhận đặt hàng.  
  * Hệ thống tạo Order.  
  * Order được đưa vào trạng thái phù hợp.  
* **Output**  
  * Order được tạo thành công.  
  * Customer được chuyển sang bước thanh toán nếu cần.  
* **Business Rules**  
  * Order chỉ được tạo khi thông tin cần thiết hợp lệ.  
  * Số lượng sản phẩm phải đáp ứng tồn kho.  
* **Exception / Alternative Flow**  
  * Sản phẩm hết hàng → không tạo order.  
  * Số lượng không đủ → yêu cầu Customer điều chỉnh số lượng.  
  * Thông tin checkout không hợp lệ → yêu cầu nhập lại.  
* **Related BR**  
  * BR-09  
  * BR-16

### **FR-09 — Quản lý Order Lifecycle**

* **Description**  
  &nbsp;Hệ thống quản lý vòng đời của Order thông qua các trạng thái nghiệp vụ được định nghĩa.  
* **Actor**  
  &nbsp;System / Admin  
* **Preconditions**  
  * Order đã được tạo.  
* **Input**  
  * Order.  
  * Sự kiện nghiệp vụ liên quan đến Order.  
* **Main Flow**  
  * Order được tạo.  
  * Hệ thống xác định trạng thái hiện tại.  
  * Khi có sự kiện phù hợp, hệ thống cập nhật trạng thái.  
  * Hệ thống ghi nhận trạng thái mới.  
  * Order tiếp tục workflow cho đến khi hoàn tất.  
* **Output**  
  * Order có trạng thái phản ánh đúng lifecycle.  
* **Business Rules**  
  * Order phải tuân thủ lifecycle đã định nghĩa.  
  * Chỉ các trạng thái hợp lệ mới được sử dụng.  
* **Exception / Alternative Flow**  
  * Sự kiện không phù hợp với trạng thái hiện tại → không cập nhật Order.  
  * Order đã ở trạng thái cuối → không cho phép chuyển tiếp trái quy định.  
* **Related BR**  
  * BR-10

### **FR-10 — Kiểm soát Order State Transition**

* **Description**  
  &nbsp;Hệ thống kiểm tra tính hợp lệ của việc chuyển trạng thái Order trước khi thực hiện cập nhật.  
* **Actor**  
  &nbsp;System / Admin  
* **Preconditions**  
  * Order tồn tại.  
  * Order có trạng thái hiện tại.  
* **Input**  
  * Trạng thái hiện tại.  
  * Trạng thái yêu cầu chuyển đến.  
* **Main Flow**  
  * Hệ thống nhận yêu cầu thay đổi trạng thái.  
  * Hệ thống xác định trạng thái hiện tại.  
  * Hệ thống kiểm tra transition được phép.  
  * Nếu hợp lệ, hệ thống cập nhật trạng thái.  
  * Hệ thống ghi nhận kết quả.  
* **Output**  
  * Order được chuyển sang trạng thái hợp lệ.  
* **Business Rules**  
  * Không cho phép chuyển sang trạng thái không được định nghĩa.  
  * Không cho phép chuyển ngược lifecycle nếu business rule không cho phép.  
* **Exception / Alternative Flow**  
  * Transition không hợp lệ → từ chối yêu cầu và giữ nguyên trạng thái hiện tại.  
* **Related BR**  
  * BR-11

# **3.4. Payment**

### **FR-11 — Khởi tạo Payment**

* **Description**  
  &nbsp;Hệ thống cho phép Customer khởi tạo thanh toán trực tuyến cho Order.  
* **Actor**  
  &nbsp;Customer / System  
* **Preconditions**  
  * Order đã được tạo.  
  * Order ở trạng thái cho phép thanh toán.  
* **Input**  
  * Order information.  
  * Payment amount.  
  * Payment method.  
* **Main Flow**  
  * Customer chọn phương thức thanh toán.  
  * Hệ thống tạo payment request.  
  * Hệ thống chuyển Customer đến payment gateway.  
  * Customer thực hiện thanh toán.  
* **Output**  
  * Payment request được khởi tạo.  
  * Customer được chuyển đến payment gateway.  
* **Business Rules**  
  * Payment phải gắn với một Order hợp lệ.  
  * Số tiền thanh toán phải tương ứng với Order.  
* **Exception / Alternative Flow**  
  * Không thể tạo payment request → thông báo lỗi cho Customer.  
  * Payment gateway không phản hồi → payment chưa được ghi nhận thành công.  
* **Related BR**  
  * BR-12

### **FR-12 — Payment Verification**

* **Description**  
  &nbsp;Hệ thống xác minh kết quả thanh toán trước khi ghi nhận payment thành công.  
* **Actor**  
  &nbsp;System / Payment Gateway  
* **Preconditions**  
  * Payment request đã được tạo.  
  * Hệ thống nhận được payment result.  
* **Input**  
  * Payment callback/result.  
  * Payment information.  
* **Main Flow**  
  * Hệ thống nhận payment result.  
  * Hệ thống kiểm tra thông tin giao dịch.  
  * Hệ thống xác minh kết quả thanh toán.  
  * Nếu hợp lệ, hệ thống ghi nhận payment.  
  * Hệ thống cập nhật Order tương ứng.  
* **Output**  
  * Payment được xác định là thành công hoặc thất bại.  
  * Order được cập nhật theo kết quả payment.  
* **Business Rules**  
  * Payment result phải được xác minh trước khi ghi nhận thành công.  
* **Exception / Alternative Flow**  
  * Payment result không hợp lệ → không ghi nhận payment thành công.  
  * Signature verification thất bại → từ chối callback.  
* **Related BR**  
  * BR-13

### **FR-13 — Idempotent Payment Processing**

* **Description**  
  &nbsp;Hệ thống xử lý payment result nhiều lần mà không tạo ra các business effect trùng lặp.  
* **Actor**  
  &nbsp;System / Payment Gateway  
* **Preconditions**  
  * Payment result đã được gửi đến hệ thống.  
  * Payment có transaction/reference phù hợp.  
* **Input**  
  * Payment callback/result.  
* **Main Flow**  
  * Hệ thống nhận callback.  
  * Hệ thống xác định payment transaction.  
  * Hệ thống kiểm tra payment đã được xử lý hay chưa.  
  * Nếu chưa xử lý, hệ thống thực hiện business effect.  
  * Nếu đã xử lý, hệ thống không thực hiện lại business effect.  
* **Output**  
  * Một payment result chỉ tạo ra business effect cần thiết.  
* **Business Rules**  
  * Duplicate payment result không được tạo duplicate business effect.  
* **Exception / Alternative Flow**  
  * Callback được gửi lại → hệ thống nhận diện và bỏ qua xử lý lặp.  
  * Không xác định được payment transaction → callback bị từ chối hoặc đưa vào trạng thái xử lý lỗi.  
* **Related BR**  
  * BR-14

### **FR-14 — Payment Failure Handling**

* **Description**  
  &nbsp;Hệ thống xử lý trường hợp thanh toán thất bại, bị hủy hoặc không hoàn tất.  
* **Actor**  
  &nbsp;Customer / System / Payment Gateway  
* **Preconditions**  
  * Order đang trong quá trình thanh toán.  
  * Payment request đã được tạo.  
* **Input**  
  * Payment failure result.  
  * Payment status.  
* **Main Flow**  
  * Hệ thống nhận kết quả thanh toán.  
  * Hệ thống xác định payment không thành công.  
  * Hệ thống cập nhật payment status.  
  * Hệ thống giữ hoặc chuyển Order sang trạng thái phù hợp.  
  * Hệ thống thông báo kết quả cho Customer.  
* **Output**  
  * Payment được ghi nhận là thất bại/chưa hoàn tất.  
  * Order không được ghi nhận là đã thanh toán.  
* **Business Rules**  
  * Payment thất bại không được làm Order chuyển sang trạng thái PAID.  
* **Exception / Alternative Flow**  
  * Không nhận được payment result → Order tiếp tục ở trạng thái chờ xử lý theo business rule.  
* **Related BR**  
  * BR-15

# **3.5. Order & Inventory Consistency**

### **FR-15 — Stock Availability Check**

* **Description**  
  &nbsp;Hệ thống kiểm tra số lượng tồn kho trước khi xác nhận việc mua sản phẩm.  
* **Actor**  
  &nbsp;Customer / System  
* **Preconditions**  
  * Customer đang thực hiện checkout.  
  * Sản phẩm tồn tại.  
* **Input**  
  * Product ID.  
  * Size/variant.  
  * Requested quantity.  
* **Main Flow**  
  * Customer xác nhận checkout.  
  * Hệ thống lấy số lượng tồn hiện tại.  
  * Hệ thống so sánh với số lượng Customer yêu cầu.  
  * Nếu đủ hàng, workflow tiếp tục.  
  * Nếu không đủ hàng, hệ thống từ chối việc mua.  
* **Output**  
  * Kết quả kiểm tra stock availability.  
* **Business Rules**  
  * Không được xác nhận mua vượt quá số lượng tồn thực tế.  
* **Exception / Alternative Flow**  
  * Không đủ hàng → hiển thị số lượng có thể mua hoặc yêu cầu Customer điều chỉnh.  
  * Sản phẩm không tồn tại → không cho phép tiếp tục checkout.  
* **Related BR**  
  * BR-16

### **FR-16 — Concurrent Inventory Control**

* **Description**  
  &nbsp;Hệ thống kiểm soát việc nhiều Customer đồng thời mua cùng một sản phẩm có số lượng tồn thấp.  
* **Actor**  
  &nbsp;System  
* **Preconditions**  
  * Có nhiều request mua cùng một sản phẩm/variant.  
  * Số lượng tồn có giới hạn.  
* **Input**  
  * Product/variant.  
  * Requested quantity.  
  * Concurrent purchase requests.  
* **Main Flow**  
  * Customer A và Customer B cùng thực hiện checkout.  
  * Hệ thống xử lý các yêu cầu cập nhật inventory.  
  * Hệ thống kiểm tra stock tại thời điểm cập nhật.  
  * Request đủ điều kiện được xử lý thành công.  
  * Request vượt quá stock còn lại bị từ chối.  
* **Output**  
  * Inventory không bị giảm xuống giá trị không hợp lệ.  
  * Một hoặc nhiều Customer có thể nhận kết quả Out of Stock tùy stock thực tế.  
* **Business Rules**  
  * Tổng số lượng được bán không được vượt quá inventory khả dụng.  
  * Concurrent requests phải được xử lý theo cơ chế đảm bảo consistency.  
* **Exception / Alternative Flow**  
  * Stock không còn đủ tại thời điểm cập nhật → request bị từ chối.  
  * Cập nhật inventory thất bại → giao dịch không được ghi nhận không hoàn chỉnh.  
* **Related BR**  
  * BR-17

### **FR-17 — Order–Inventory Consistency**

* **Description**  
  &nbsp;Hệ thống đảm bảo các thay đổi liên quan đến Order và Inventory được thực hiện nhất quán.  
* **Actor**  
  &nbsp;System  
* **Preconditions**  
  * Customer đang thực hiện purchase workflow.  
  * Order và inventory cần được cập nhật.  
* **Input**  
  * Order information.  
  * Product/variant.  
  * Quantity.  
* **Main Flow**  
  * Hệ thống xác nhận điều kiện mua hàng.  
  * Hệ thống xử lý Order.  
  * Hệ thống cập nhật inventory tương ứng.  
  * Hệ thống hoàn tất giao dịch.  
  * Hệ thống trả kết quả cho Customer.  
* **Output**  
  * Order và Inventory phản ánh cùng một business transaction.  
  * Không xảy ra trường hợp Order được ghi nhận nhưng Inventory không được cập nhật hoặc ngược lại.  
* **Business Rules**  
  * Các cập nhật liên quan phải đảm bảo consistency.  
  * Không được để lại trạng thái dữ liệu không hoàn chỉnh.  
* **Exception / Alternative Flow**  
  * Một bước trong giao dịch thất bại → hệ thống không ghi nhận trạng thái không nhất quán.  
  * Inventory không thể cập nhật → Order không được hoàn tất theo workflow yêu cầu.  
* **Related BR**  
  * BR-18

# **3.6. Testing & Deployment**

### **FR-18 — Kiểm thử Business-Critical Workflows**

* **Description**  
  &nbsp;Hệ thống phải có khả năng kiểm thử các workflow quan trọng để đảm bảo business logic tiếp tục hoạt động đúng sau khi có thay đổi.  
* **Actor**  
  &nbsp;Developer / System  
* **Preconditions**  
  * Các business-critical workflows đã được xác định.  
  * Có các điều kiện kiểm thử tương ứng.  
* **Input**  
  * Test cases.  
  * Test data.  
  * Business-critical workflows.  
* **Main Flow**  
  * Developer thực hiện thay đổi hệ thống.  
  * Các automated tests được thực thi.  
  * Hệ thống kiểm tra kết quả thực tế với expected result.  
  * Hệ thống xác định test pass/fail.  
  * Developer xử lý các lỗi nếu test thất bại.  
* **Output**  
  * Kết quả kiểm thử.  
  * Các workflow quan trọng được xác nhận hoạt động đúng hoặc được phát hiện lỗi.  
* **Business Rules**  
  * Các business-critical workflows phải có automated tests tương ứng.  
  * Thay đổi không được làm phá vỡ các workflow đã được kiểm thử.  
* **Exception / Alternative Flow**  
  * Test thất bại → thay đổi chưa được coi là đạt yêu cầu.  
  * Phát hiện regression → Developer phải xử lý trước khi tiếp tục deployment.  
* **Related BR**  
  * BR-19

### **FR-19 — Deployment Nhất Quán Giữa Các Môi Trường**

* **Description**  
  &nbsp;Hệ thống phải hỗ trợ quy trình triển khai nhất quán giữa các môi trường nhằm giảm rủi ro do khác biệt môi trường.  
* **Actor**  
  &nbsp;Developer / System  
* **Preconditions**  
  * Phiên bản hệ thống đã được chuẩn bị để deployment.  
  * Môi trường deployment đã được cấu hình.  
* **Input**  
  * Application version.  
  * Configuration cần thiết.  
  * Deployment package.  
* **Main Flow**  
  * Developer chuẩn bị phiên bản hệ thống.  
  * Hệ thống thực hiện các bước kiểm tra cần thiết.  
  * Phiên bản được triển khai theo quy trình đã định nghĩa.  
  * Hệ thống khởi động phiên bản mới.  
  * Developer kiểm tra kết quả deployment.  
* **Output**  
  * Phiên bản hệ thống được triển khai nhất quán.  
  * Môi trường chạy phiên bản tương ứng với release.  
* **Business Rules**  
  * Deployment phải tuân theo quy trình thống nhất.  
  * Chỉ phiên bản đáp ứng điều kiện kiểm tra mới được triển khai.  
* **Exception / Alternative Flow**  
  * Deployment thất bại → hệ thống không được coi là đã release thành công.  
  * Phát hiện lỗi sau deployment → thực hiện quy trình xử lý/recovery phù hợp.  
* **Related BR**  
  * BR-20

# **4\. Functional Flow Summary**

Phần này tổng hợp các workflow chính của HP\_SNEAKERS dựa trên các Functional Requirements đã được xác định ở Phần 3\.

## **4.1. Inventory Import**

Workflow cho phép Admin nhập dữ liệu hàng hóa từ file Excel/CSV và cập nhật inventory sau khi dữ liệu được kiểm tra và xác nhận.

**Flow:**

Admin  
&nbsp;↓  
&nbsp;Upload Excel/CSV  
&nbsp;↓  
&nbsp;Validate Data  
&nbsp;↓  
&nbsp;Preview  
&nbsp;↓  
&nbsp;Admin Confirm  
&nbsp;↓  
&nbsp;Create/Record Inventory Transaction  
&nbsp;↓  
&nbsp;Update Inventory

**Functional Requirements:** FR-03, FR-04, FR-05, FR-06, FR-07.

---

## **4.2. Order Processing**

Workflow xử lý quá trình Customer mua sản phẩm từ giỏ hàng đến khi Order được tạo, đồng thời đảm bảo số lượng tồn kho được kiểm tra và cập nhật nhất quán.

**Flow:**

Customer  
&nbsp;↓  
&nbsp;Cart  
&nbsp;↓  
&nbsp;Checkout  
&nbsp;↓  
&nbsp;Check Stock  
&nbsp;↓  
&nbsp;Create Order  
&nbsp;↓  
&nbsp;Update Inventory  
&nbsp;↓  
&nbsp;Order Created

Nếu không đủ stock:

Checkout  
&nbsp;↓  
&nbsp;Check Stock  
&nbsp;↓  
&nbsp;Insufficient Stock  
&nbsp;↓  
&nbsp;Reject Purchase

**Functional Requirements:** FR-08, FR-09, FR-10, FR-15, FR-16, FR-17.

---

## **4.3. Payment Processing**

Workflow xử lý thanh toán trực tuyến thông qua payment gateway, bao gồm xác minh kết quả, xử lý callback trùng lặp và payment failure.

**Flow:**

Customer  
&nbsp;↓  
&nbsp;Checkout  
&nbsp;↓  
&nbsp;Create Payment  
&nbsp;↓  
&nbsp;VNPay  
&nbsp;↓  
&nbsp;Payment Callback  
&nbsp;↓  
&nbsp;Verify Payment  
&nbsp;↓  
&nbsp;Check Idempotency  
&nbsp;↓  
&nbsp;Update Payment  
&nbsp;↓  
&nbsp;Update Order

Nếu thanh toán thất bại:

Payment Callback  
&nbsp;↓  
&nbsp;Payment Failed  
&nbsp;↓  
&nbsp;Update Payment Status  
&nbsp;↓  
&nbsp;Order không được ghi nhận là PAID

**Functional Requirements:** FR-11, FR-12, FR-13, FR-14.

---

## **4.4. Product Management & Search**

Workflow quản lý và tìm kiếm sản phẩm bao gồm hai nhóm người dùng chính.

**Admin:**

Admin  
&nbsp;↓  
&nbsp;Product Management  
&nbsp;↓  
&nbsp;Create / Update Product  
&nbsp;↓  
&nbsp;Validate Data  
&nbsp;↓  
&nbsp;Save Product

**Customer:**

Customer  
&nbsp;↓  
&nbsp;Product Search  
&nbsp;↓  
&nbsp;Apply Filter  
&nbsp;↓  
&nbsp;Display Matching Products

**Functional Requirements:** FR-01, FR-02.

---

## **4.5. Testing & Deployment**

Workflow hỗ trợ kiểm tra các business-critical workflows và triển khai phiên bản hệ thống theo quy trình nhất quán.

**Testing:**

Code Change  
&nbsp;↓  
&nbsp;Automated Tests  
&nbsp;↓  
&nbsp;Pass / Fail  
&nbsp;↓  
&nbsp;Nếu Pass → Continue

Nếu Fail → Fix → Test Again

**Deployment:**

Validated Version  
&nbsp;↓  
&nbsp;Deployment Process  
&nbsp;↓  
&nbsp;Environment  
&nbsp;↓  
&nbsp;Deployment Verification  
&nbsp;↓  
&nbsp;Release

**Functional Requirements:** FR-18, FR-19.

# **5\. Non-Functional Requirements**

Phần này xác định các yêu cầu về chất lượng và khả năng vận hành của HP\_SNEAKERS. Các yêu cầu được giới hạn ở những tiêu chí thực sự cần thiết đối với phạm vi của project.

## **5.1. Performance**

| ID | Requirement |
| ----- | ----- |
| NFR-01 | Hệ thống phải phản hồi Product Search và Filtering trong thời gian phù hợp để đảm bảo trải nghiệm người dùng. |
| NFR-02 | Hệ thống phải xử lý các thao tác Checkout và Order Creation mà không gây ra độ trễ không cần thiết cho người dùng. |

## **5.2. Security**

| ID | Requirement |
| ----- | ----- |
| NFR-03 | Hệ thống phải kiểm soát quyền truy cập theo vai trò của người dùng. |
| NFR-04 | Chỉ người dùng có quyền phù hợp mới được thực hiện các chức năng quản lý sản phẩm, inventory và order. |
| NFR-05 | Thông tin liên quan đến authentication và payment phải được xử lý và bảo vệ an toàn. |

## **5.3. Reliability**

| ID | Requirement |
| ----- | ----- |
| NFR-06 | Hệ thống phải đảm bảo dữ liệu Order và Inventory nhất quán khi thực hiện các giao dịch mua hàng. |
| NFR-07 | Hệ thống phải xử lý Payment Callback theo cách không tạo ra nhiều business effects ngoài mong muốn khi callback được gửi lặp lại. |
| NFR-08 | Hệ thống phải xử lý các trường hợp lỗi trong Payment workflow mà không làm Order chuyển sang trạng thái thanh toán thành công không hợp lệ. |
| NFR-09 | Hệ thống phải đảm bảo dữ liệu inventory không bị cập nhật một phần khi quá trình nhập kho hoặc cập nhật tồn kho xảy ra lỗi. |

## **5.4. Availability**

| ID | Requirement |
| ----- | ----- |
| NFR-10 | Các chức năng chính của hệ thống phải sẵn sàng để Customer và Admin sử dụng trong thời gian hệ thống được vận hành. |
| NFR-11 | Lỗi của một thao tác hoặc workflow không được làm ảnh hưởng không cần thiết đến khả năng sử dụng các chức năng khác của hệ thống. |

# **6\. Requirement Traceability**

Phần này xác định mối liên hệ giữa Business Objective, Business Requirement, Functional Requirement và Use Case, nhằm đảm bảo các chức năng được xây dựng đều có nguồn gốc từ nhu cầu nghiệp vụ.

## **6.1. Requirement Traceability Matrix**

| Business Objective | Business Requirement | Functional Requirement | Use Case |
| ----- | ----- | ----- | ----- |
| BO-01 — Giảm manual effort trong nhập kho | BR-04 — Cho phép Admin nhập dữ liệu hàng hóa từ file | FR-03 — Import Inventory Data | UC-01 — Import Inventory |
| BO-01 — Giảm manual effort trong nhập kho | BR-04 — Cho phép Admin nhập dữ liệu hàng hóa từ file | FR-04 — Validate Import Data | UC-01 — Import Inventory |
| BO-02 — Cải thiện inventory accuracy | BR-05 — Kiểm tra dữ liệu trước khi cập nhật inventory | FR-04 — Validate Import Data | UC-01 — Import Inventory |
| BO-02 — Cải thiện inventory accuracy | BR-06 — Cho phép Admin xem trước dữ liệu | FR-05 — Preview Import Data | UC-01 — Import Inventory |
| BO-02 — Cải thiện inventory accuracy | BR-07 — Chỉ cập nhật inventory sau khi xác nhận hợp lệ | FR-06 — Confirm Inventory Import | UC-01 — Import Inventory |
| BO-02 — Cải thiện inventory accuracy | BR-08 — Ghi nhận thay đổi inventory | FR-07 — Record Inventory Transaction | UC-01 — Import Inventory |
| BO-03 — Hạn chế overselling | BR-16 — Kiểm tra stock trước khi xác nhận mua | FR-15 — Check Stock Availability | UC-02 — Process Customer Purchase |
| BO-03 — Hạn chế overselling | BR-17 — Ngăn chặn mua vượt tồn kho | FR-16 — Handle Concurrent Inventory Purchase | UC-02 — Process Customer Purchase |
| BO-03 — Hạn chế overselling | BR-18 — Đảm bảo consistency giữa order và inventory | FR-17 — Maintain Order-Inventory Consistency | UC-02 — Process Customer Purchase |
| BO-04 — Kiểm soát order lifecycle | BR-10 — Quản lý vòng đời Order | FR-09 — Manage Order Lifecycle | UC-03 — Manage Order |
| BO-04 — Kiểm soát order lifecycle | BR-11 — Ngăn chặn transition không hợp lệ | FR-10 — Validate Order State Transition | UC-03 — Manage Order |
| BO-05 — Tăng payment reliability | BR-12 — Hỗ trợ thanh toán trực tuyến | FR-11 — Initiate Payment | UC-04 — Process Payment |
| BO-05 — Tăng payment reliability | BR-13 — Xác minh kết quả thanh toán | FR-12 — Verify Payment Result | UC-04 — Process Payment |
| BO-05 — Tăng payment reliability | BR-14 — Không tạo duplicate business effect | FR-13 — Handle Duplicate Payment Callback | UC-04 — Process Payment |
| BO-05 — Tăng payment reliability | BR-15 — Xử lý payment failure | FR-14 — Handle Payment Failure | UC-04 — Process Payment |
| BO-06 — Cải thiện product discovery | BR-01 — Quản lý thông tin sản phẩm | FR-01 — Manage Product Information | UC-05 — Manage Product |
| BO-06 — Cải thiện product discovery | BR-03 — Cho phép Customer tìm kiếm và lọc sản phẩm | FR-02 — Search & Filter Products | UC-06 — Search Products |
| BO-07 — Tăng khả năng kiểm thử business-critical workflows | — | FR-18 — Automated Testing | UC-07 — Execute Automated Tests |
| BO-07 — Tăng khả năng kiểm thử business-critical workflows | — | FR-19 — Deployment Process | UC-08 — Deploy Application |

## **6.2. Traceability Flow**

Các requirement chính của HP\_SNEAKERS được liên kết theo chuỗi:

Business Objective

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;↓

Business Requirement

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;↓

Functional Requirement

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;↓

Use Case

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;↓

Activity / BPMN / Sequence Diagram

Ví dụ đối với Inventory:

BO-02

Cải thiện inventory accuracy

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;↓

BR-05 / BR-06 / BR-07 / BR-08

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;↓

FR-04 / FR-05 / FR-06 / FR-07

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;↓

UC-01 — Import Inventory

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;↓

Activity — Inventory Import Workflow

Ví dụ đối với Payment:

BO-05

Tăng payment reliability

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;↓

BR-13 / BR-14 / BR-15

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;↓

FR-12 / FR-13 / FR-14

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;↓

UC-04 — Process Payment

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;↓

Sequence — Payment Processing

## **6.3. Traceability Principle**

Requirement Traceability của HP\_SNEAKERS đảm bảo:

* Mỗi Functional Requirement quan trọng có nguồn gốc từ Business Requirement hoặc Business Objective.  
* Các Functional Requirement được nhóm thành các Use Case tương ứng.  
* Các Use Case có thể tiếp tục được sử dụng làm cơ sở cho System Modeling.  
* Requirement không có justification rõ ràng không nên được bổ sung chỉ vì mục đích sử dụng thêm technology.

&nbsp;

&nbsp;