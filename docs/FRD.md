# **FUNCTIONAL REQUIREMENTS DOCUMENT (FRD) — HP\_SNEAKER Store**

Draft v2.0 \| 26/10/2026 \| Cấu trúc tinh gọn cho pet project.

Nguồn: FRD hiện hành và BA Register v1.2; các AC đã duyệt tại baseline mới có hiệu lực ưu tiên khi phần mô tả khái quát cũ khác nhau. BA Approved không có nghĩa đã implemented, test passed hoặc deployed.

# **1\. Introduction & Scope**

### **Purpose**

Tài liệu này mô tả các yêu cầu chức năng của hệ thống HP Sneakers Store dựa trên các business requirements đã được xác định trong BRD.

FRD tập trung mô tả hệ thống phải thực hiện những chức năng gì và hoạt động như thế nào để đáp ứng các nhu cầu nghiệp vụ về:

Product Management

Inventory Management

Inventory Import

Order Management

Payment

Customer Purchase Flow

### **Scope**

FRD tập trung vào các chức năng chính của hệ thống:

Khách hàng xem, tìm kiếm và lọc sản phẩm.

Khách hàng quản lý giỏ hàng và tạo đơn hàng.

Khách hàng thực hiện thanh toán.

Hệ thống xử lý và cập nhật kết quả thanh toán.

Admin quản lý sản phẩm.

Admin import dữ liệu inventory từ Excel/CSV.

Hệ thống validation và preview dữ liệu inventory trước khi cập nhật.

Hệ thống quản lý goods receipt và cập nhật inventory.

Hệ thống kiểm soát inventory consistency trong quá trình đặt hàng.

Hệ thống quản lý order lifecycle.

Hệ thống xử lý các trường hợp payment failure và duplicate callback.

# **2\. Functional Requirements Overview**

Phần này tổng hợp các Functional Requirements chính của HP\_SNEAKERS được phân rã từ các Business Requirements trong BRD. Mỗi Functional Requirement mô tả một hành vi hoặc chức năng mà hệ thống cần cung cấp để đáp ứng yêu cầu nghiệp vụ.

| FR ID | Functional Requirement | Related BR | Priority |
| :---- | :---- | :---- | :---- |
| **FR-01** | **Quản lý thông tin sản phẩm, bao gồm các thuộc tính sản phẩm và số lượng tồn kho** | **BR-01, BR-02** | **P0** |
| **FR-02** | **Tìm kiếm và lọc sản phẩm** | **BR-03** | **P1** |
| **FR-03** | **Upload file Excel/CSV nhập kho** | **BR-04** | **P0** |
| **FR-04** | **Validate file và dữ liệu nhập kho** | **BR-05** | **P0** |
| **FR-05** | **Preview dữ liệu trước khi nhập kho** | **BR-06** | **P0** |
| **FR-06** | **Xác nhận và cập nhật inventory** | **BR-07** | **P0** |
| **FR-07** | **Ghi nhận lịch sử nhập và cập nhật inventory** | **BR-08** | **P1** |
| **FR-08** | **Tạo đơn hàng từ giỏ hàng** | **BR-09** | **P0** |
| **FR-09** | **Quản lý vòng đời đơn hàng** | **BR-10** | **P0** |
| **FR-10** | **Kiểm soát chuyển đổi trạng thái đơn hàng** | **BR-11** | **P0** |
| **FR-11** | **Khởi tạo thanh toán trực tuyến** | **BR-12** | **P0** |
| **FR-12** | **Xác minh kết quả thanh toán** | **BR-13** | **P0** |
| **FR-13** | **Xử lý duplicate payment callback** | **BR-14** | **P0** |
| **FR-14** | **Xử lý thanh toán thất bại** | **BR-15** | **P0** |
| **FR-15** | **Kiểm tra tồn kho trước khi xác nhận đơn hàng** | **BR-16** | **P0** |
| **FR-16** | **Kiểm soát concurrent inventory update** | **BR-17** | **P0** |
| **FR-17** | **Đảm bảo consistency giữa Order và Inventory** | **BR-18** | **P0** |
| **FR-18** | **Hệ thống phải hỗ trợ kiểm thử các business-critical workflows để đảm bảo chức năng hoạt động đúng sau khi thay đổi.** | **BR-19** | **P0** |
| **FR-19** | **Hệ thống phải hỗ trợ quy trình triển khai nhất quán giữa các môi trường.** | **BR-20** | **P1** |

### **Functional Requirement Groups**

Product Management

FR-01: Quản lý thông tin sản phẩm, bao gồm thuộc tính và tồn kho.

FR-02: Tìm kiếm và lọc sản phẩm.

Inventory Management

FR-03: Upload file Excel/CSV.

FR-04: Validation dữ liệu.

FR-05: Preview dữ liệu.

FR-06: Xác nhận và cập nhật inventory.

FR-07: Ghi nhận lịch sử inventory.

Order Management

FR-08: Tạo đơn hàng.

FR-09: Quản lý order lifecycle.

FR-10: Kiểm soát order state transition.

Payment

FR-11: Khởi tạo payment.

FR-12: Payment verification.

FR-13: Idempotent payment processing.

FR-14: Payment failure handling.

Order & Inventory Consistency

FR-15: Stock availability check.

FR-16: Concurrent inventory control.

FR-17: Order–Inventory consistency.

Testing & Deployment

FR-18: Kiểm thử các business-critical workflows.

FR-19: Hỗ trợ deployment nhất quán giữa các môi trường.

# **3\. Detailed Functional Requirements (FR-01–FR-19)**

Mỗi FR chứa mô tả/Actor/Preconditions/Input/Main Flow/Output từ đặc tả gốc và AC đã được phê duyệt từ phụ lục v1.2; mã FR/AC được giữ nguyên.

## **3.1 Product Management**

### **3.1 FR-01 — Quản lý thông tin sản phẩm**

| Field | Value |
| :---- | :---- |
| **FR ID** | FR-01 |
| **Actor** | Admin |
| **Related BR** | BR-01 — BR-02 |
| **Priority** | P0 |

Description;  Hệ thống cho phép Admin quản lý thông tin sản phẩm và các thuộc tính liên quan, bao gồm thông tin cơ bản, size và số lượng tồn kho.

Preconditions

Admin đã đăng nhập.

Admin có quyền quản lý sản phẩm.

Input

Thông tin sản phẩm.

Các thuộc tính sản phẩm.

Size.

Số lượng tồn kho.

Main Flow

| Step | Action / Expected Behavior |
| :---- | :---- |
| **1** | Admin truy cập chức năng quản lý sản phẩm. |
| **2** | Hệ thống hiển thị danh sách sản phẩm. |
| **3** | Admin tạo mới hoặc cập nhật thông tin sản phẩm. |
| **4** | Admin nhập các thuộc tính cần thiết. |
| **5** | Hệ thống kiểm tra dữ liệu. |
| **6** | Hệ thống lưu thông tin sản phẩm. |

Output

Thông tin sản phẩm được tạo hoặc cập nhật thành công.

Inventory tương ứng được ghi nhận trong hệ thống.

Business Rules

Thông tin bắt buộc của sản phẩm phải được cung cấp.

Số lượng tồn kho không được nhận giá trị không hợp lệ.

Exception / Alternative Flow

| Condition | System Behavior |
| :---- | :---- |
| Dữ liệu không hợp lệ | hệ thống thông báo lỗi và yêu cầu Admin chỉnh sửa. |
| Sản phẩm không tồn tại khi cập nhật | hệ thống thông báo lỗi. |

### **Acceptance Criteria — Approved (BA Register v1.2)**

| AC ID | Acceptance Criterion |
| :---- | :---- |
| **V-AC01** | Variant từng có Order/Import/Movement không bị xóa hay đổi size trực tiếp. |
| **V-AC02** | Size 42 và 42.0 tương đương; 42.5 hợp lệ; 42.25 không hợp lệ. |
| **V-AC03** | Size hợp lệ ngoài 35–45 vẫn được tạo. |
| **V-AC04** | Không tạo Variant mới trùng size chuẩn hóa với Variant Active/Retired cùng Product. |
| **V-AC05** | Reactivate Variant Retired giữ nguyên ID, quantity, WAC và lịch sử. |
| **V-AC06** | Order tạo hợp lệ trước khi Retire không tự hủy và tiếp tục theo Order Lifecycle. |
| **V-AC07** | Checkout/Import mới vào Variant Retired bị chặn; CartItem cũ giữ để hiển thị cảnh báo. |

### **3.2 FR-02 — Tìm kiếm và lọc sản phẩm**

| Field | Value |
| :---- | :---- |
| **FR ID** | FR-02 |
| **Actor** | Customer |
| **Related BR** | BR-03 |
| **Priority** | P1 |

Description;  Hệ thống cho phép khách hàng tìm kiếm và lọc sản phẩm dựa trên các tiêu chí phù hợp.

Preconditions

Hệ thống có dữ liệu sản phẩm.

Input

Từ khóa tìm kiếm.

Tiêu chí lọc sản phẩm.

Main Flow

| Step | Action / Expected Behavior |
| :---- | :---- |
| **1** | Customer truy cập danh sách sản phẩm. |
| **2** | Customer nhập từ khóa hoặc lựa chọn tiêu chí lọc. |
| **3** | Hệ thống xử lý yêu cầu. |
| **4** | Hệ thống trả về các sản phẩm phù hợp. |

Output

Danh sách sản phẩm phù hợp với điều kiện tìm kiếm/lọc.

Business Rules

Chỉ hiển thị các sản phẩm phù hợp với tiêu chí tìm kiếm/lọc.

Exception / Alternative Flow

| Condition | System Behavior |
| :---- | :---- |
| Không tìm thấy sản phẩm | hệ thống hiển thị kết quả rỗng và thông báo phù hợp. |

### **Acceptance Criteria — Approved (BA Register v1.2)**

| AC ID | Acceptance Criterion |
| :---- | :---- |
| **S-AC01** | nike/Nike/NIKE cho kết quả tương đương. |
| **S-AC02** | Exact được ưu tiên trước contains rồi approximate khi sort relevance. |
| **S-AC03** | Nhóm lọc kết hợp đúng, không hiển thị ngoài điều kiện. |
| **S-AC04** | Giá niêm yết 2.200.000, giảm còn 1.800.000 xuất hiện khi lọc 1–2 triệu. |
| **S-AC05** | Product Active size 42 hết hàng vẫn xuất hiện khi lọc size 42, kèm thông báo. |
| **S-AC06** | Product Inactive và Variant Retired không hiển thị như |
| **S-AC07** | Đổi trang/sắp xếp vẫn giữ điều kiện lọc; không kết quả hiển thị empty state. |

## **3.2 Inventory Management**

### **3.3 FR-03 — Upload file Excel/CSV**

| Field | Value |
| :---- | :---- |
| **FR ID** | FR-03 |
| **Actor** | Admin |
| **Related BR** | BR-04 |
| **Priority** | P0 |

Description;  Hệ thống cho phép Admin upload file chứa dữ liệu hàng hóa để thực hiện nhập kho hàng loạt.

Preconditions

Admin đã đăng nhập.

Admin có quyền nhập kho.

File dữ liệu đã được chuẩn bị.

Input

File Excel/CSV.

Main Flow

| Step | Action / Expected Behavior |
| :---- | :---- |
| **1** | Admin truy cập chức năng nhập kho. |
| **2** | Admin chọn file Excel/CSV. |
| **3** | Hệ thống tiếp nhận file. |
| **4** | Hệ thống đọc dữ liệu từ file. |
| **5** | Hệ thống chuyển dữ liệu sang bước validation. |

Output

Dữ liệu từ file được hệ thống tiếp nhận để kiểm tra.

Business Rules

File phải thuộc định dạng được hệ thống hỗ trợ.

File phải chứa các trường dữ liệu cần thiết.

Exception / Alternative Flow

| Condition | System Behavior |
| :---- | :---- |
| File không đúng định dạng | từ chối upload. |
| File không thể đọc | thông báo lỗi. |
| File thiếu trường bắt buộc | chuyển sang trạng thái invalid. |

### **Acceptance Criteria — Approved (BA Register v1.2)**

| AC ID | Acceptance Criterion |
| :---- | :---- |
| **016-AC01** | File đúng template Excel/CSV, metadata bắt buộc và giới hạn; tạo Draft xem trước. |
| **016-AC02** | File \>5 MB hoặc \>1.000 dòng lúc Upload bị từ chối; sau chỉnh Preview vẫn không được \>1.000 dòng. |
| **016-AC03** | Thiếu cột, sai cấu trúc hoặc file không đọc được yêu cầu upload lại, không Confirm. |
| **016-AC14** | Draft đúng cấu trúc được cấp Import Code duy nhất, giữ đến Confirm và không tái sử dụng sau hủy/hết hạn. |
| **016-AC16** | File gốc trùng byte-for-byte hoặc dữ liệu nghiệp vụ cuối chuẩn hóa trùng với phiếu Confirm phải cảnh báo và xác nhận rõ; không tự chặn, lưu bằng chứng. |

### **3.4 FR-04 — Validation dữ liệu nhập kho**

| Field | Value |
| :---- | :---- |
| **FR ID** | FR-04 |
| **Actor** | Admin / System |
| **Related BR** | BR-05 |
| **Priority** | P0 |

Description;  Hệ thống kiểm tra tính hợp lệ của file và từng record trước khi dữ liệu được sử dụng để cập nhật inventory.

Preconditions

File đã được upload thành công.

Input

Dữ liệu từ file Excel/CSV.

Main Flow

| Step | Action / Expected Behavior |
| :---- | :---- |
| **1** | Hệ thống đọc từng record. |
| **2** | Hệ thống kiểm tra các trường bắt buộc. |
| **3** | Hệ thống kiểm tra format và giá trị dữ liệu. |
| **4** | Hệ thống xác định các record hợp lệ và không hợp lệ. |
| **5** | Hệ thống trả kết quả validation cho Admin. |

Output

Danh sách record hợp lệ.

Danh sách record không hợp lệ.

Thông tin lỗi tương ứng.

Business Rules

Record không hợp lệ không được cập nhật inventory.

Dữ liệu bắt buộc phải đáp ứng các điều kiện nghiệp vụ.

Exception / Alternative Flow

| Condition | System Behavior |
| :---- | :---- |
| File không có record hợp lệ | không cho phép tiếp tục nhập kho. |
| Có record lỗi | Admin phải xử lý hoặc loại bỏ record lỗi trước khi xác nhận. |

### **Acceptance Criteria — Approved (BA Register v1.2)**

| AC ID | Acceptance Criterion |
| :---- | :---- |
| **016-AC03** | Thiếu cột, sai cấu trúc hoặc file không đọc được yêu cầu upload lại, không Confirm. |
| **016-AC04** | SKU/Product/Variant không tồn tại hoặc không hợp lệ, Product Inactive hoặc Variant Inactive/Retired đều bị lỗi và chặn Confirm; không tự tạo/Reactivate; kiểm tra lại trạng thái lúc Confirm. |
| **016-AC05** | Quantity không nguyên dương, Import Price âm hoặc định dạng số mơ hồ bị từ chối. |
| **016-AC06** | Dòng trùng SKU/size chuẩn hóa trong cùng phiếu phải đánh dấu, chặn Confirm cho đến khi sửa. |
| **016-AC08** | Người tạo thêm/xóa dòng; kiểm tra giới hạn 1.000 dòng và revalidate. |
| **016-AC17** | Quantity, WAC hoặc trạng thái Product/Variant đổi sau Preview làm xác nhận cũ hết hiệu lực; refresh/revalidate, Inactive/Retired chặn, Admin xác nhận lại. |

### **3.5 FR-05 — Preview dữ liệu nhập kho**

| Field | Value |
| :---- | :---- |
| **FR ID** | FR-05 |
| **Actor** | Admin |
| **Related BR** | BR-06 |
| **Priority** | P0 |

Description;  Hệ thống cho phép Admin xem trước dữ liệu đã được xử lý và kết quả validation trước khi xác nhận nhập kho.

Preconditions

File đã được upload.

Validation đã được thực hiện.

Input

Kết quả validation.

Dữ liệu hợp lệ từ file.

Main Flow

| Step | Action / Expected Behavior |
| :---- | :---- |
| **1** | Hệ thống hiển thị dữ liệu preview. |
| **2** | Hệ thống phân biệt record hợp lệ và không hợp lệ. |
| **3** | Admin kiểm tra dữ liệu. |
| **4** | Admin lựa chọn tiếp tục hoặc quay lại xử lý dữ liệu. |

Output

Admin có thể xác nhận dữ liệu trước khi nhập kho.

Business Rules

Inventory chưa được cập nhật trong bước preview.

Exception / Alternative Flow

| Condition | System Behavior |
| :---- | :---- |
| Admin phát hiện dữ liệu sai | không xác nhận và thực hiện điều chỉnh. |
| Dữ liệu không hợp lệ | hệ thống hiển thị lỗi tương ứng. |

### **Acceptance Criteria — Approved (BA Register v1.2)**

| AC ID | Acceptance Criterion |
| :---- | :---- |
| **016-AC07** | Người tạo sửa được SKU, Size, Quantity, Import Price trong Preview, bắt buộc validate lại. |
| **016-AC08** | Người tạo thêm/xóa dòng; kiểm tra giới hạn 1.000 dòng và revalidate. |
| **016-AC09** | Audit mọi sửa Preview gồm actor, thời gian, trường, trước/sau. |
| **016-AC10** | Không phải chủ Draft thì không được sửa; chỉ chủ thể hiện có quyền Confirm được Confirm. Admin có thẩm quyền chuyển riêng quyền Confirm có lý do và audit; không tự chuyển quyền sửa. |
| **016-AC11** | Draft mở lại trong 7 ngày từ lúc tạo, chỉnh sửa không kéo dài hạn. |
| **016-AC12** | Draft hết hạn không Confirm, không ảnh hưởng Inventory. |
| **016-AC13** | Sửa Preview không chỉnh sửa/ghi đè file gốc. |
| **016-AC15** | Mã chứng từ nguồn trùng cảnh báo rõ phiếu liên quan nếu có quyền; Admin xác nhận trước tiếp tục. |
| **016-AC16** | File gốc trùng byte-for-byte hoặc dữ liệu nghiệp vụ cuối chuẩn hóa trùng với phiếu Confirm phải cảnh báo và xác nhận rõ; không tự chặn, lưu bằng chứng. |
| **016-AC17** | Quantity, WAC hoặc trạng thái Product/Variant đổi sau Preview làm xác nhận cũ hết hiệu lực; refresh/revalidate, Inactive/Retired chặn, Admin xác nhận lại. |
| **016-AC18** | Confirm hiển thị Import Code, số dòng, tổng Quantity, tổng giá trị nhập và cảnh báo. |

### **3.6 FR-06 — Xác nhận và cập nhật inventory**

| Field | Value |
| :---- | :---- |
| **FR ID** | FR-06 |
| **Actor** | Admin / System |
| **Related BR** | BR-07 |
| **Priority** | P0 |

Description;  Hệ thống cập nhật inventory dựa trên dữ liệu đã được Admin kiểm tra và xác nhận.

Preconditions

Dữ liệu đã hoàn thành validation.

Admin đã kiểm tra và xác nhận dữ liệu.

Input

Dữ liệu nhập kho hợp lệ.

Xác nhận của Admin.

Main Flow

| Step | Action / Expected Behavior |
| :---- | :---- |
| **1** | Admin xác nhận dữ liệu. |
| **2** | Hệ thống tạo thông tin nhập kho. |
| **3** | Hệ thống cập nhật inventory. |
| **4** | Hệ thống hoàn tất giao dịch. |
| **5** | Hệ thống thông báo kết quả cho Admin. |

Output

Inventory được cập nhật.

Thông tin nhập kho được ghi nhận.

Business Rules

Inventory chỉ được cập nhật sau khi Admin xác nhận dữ liệu hợp lệ.

Việc cập nhật phải đảm bảo tính nhất quán dữ liệu.

Exception / Alternative Flow

| Condition | System Behavior |
| :---- | :---- |
| Transaction thất bại | hệ thống không ghi nhận cập nhật không hoàn chỉnh. |
| Dữ liệu không còn hợp lệ tại thời điểm xác nhận | từ chối cập nhật. |

### **Acceptance Criteria — Approved (BA Register v1.2)**

| AC ID | Acceptance Criterion |
| :---- | :---- |
| **016-AC14** | Draft đúng cấu trúc được cấp Import Code duy nhất, giữ đến Confirm và không tái sử dụng sau hủy/hết hạn. |
| **016-AC17** | Quantity, WAC hoặc trạng thái Product/Variant đổi sau Preview làm xác nhận cũ hết hiệu lực; refresh/revalidate, Inactive/Retired chặn, Admin xác nhận lại. |
| **016-AC18** | Confirm hiển thị Import Code, số dòng, tổng Quantity, tổng giá trị nhập và cảnh báo. |
| **016-AC19** | Một dòng sai hoặc update thất bại không được nhập kho một phần. |
| **016-AC20** | Double-click, request trùng hoặc Confirm đồng thời không tạo phiếu/tăng tồn lặp; chỉ người đang giữ quyền Confirm hợp lệ thực hiện; atomicity và idempotency. |
| **016-AC21** | Confirm thành công cập nhật stock/WAC theo WAC policy, không ghi đè thay đổi đồng thời. |
| **WAC-AC01** | Nhập vào ProductSize tồn dương/WAC hợp lệ phải tính đúng WAC bình quân. |
| **WAC-AC02** | Tồn trước nhập bằng 0 tạo WAC bằng giá nhập mới. |
| **WAC-AC03** | Bán thường giảm tồn, không tự đổi WAC; lưu cost snapshot khi trừ. |
| **WAC-AC04** | WAC=0 có chứng từ hợp lệ; NULL/thiếu/invalid báo lỗi hoặc ngoại lệ. |
| **WAC-AC05** | WAC ROUND\_HALF\_UP hai chữ số; truy xuất được rounding residual. |
| **WAC-AC06** | Quantity/price không hợp lệ không tạo cập nhật tồn một phần. |

### **3.7 FR-07 — Ghi nhận lịch sử inventory**

| Field | Value |
| :---- | :---- |
| **FR ID** | FR-07 |
| **Actor** | System |
| **Related BR** | BR-08 |
| **Priority** | P1 |

Description;  Hệ thống ghi nhận các thay đổi liên quan đến việc nhập và cập nhật inventory.

Preconditions

Có giao dịch nhập hoặc cập nhật inventory được thực hiện.

Input

Thông tin giao dịch inventory.

Dữ liệu sản phẩm.

Số lượng thay đổi.

Main Flow

| Step | Action / Expected Behavior |
| :---- | :---- |
| **1** | Hệ thống hoàn tất giao dịch inventory. |
| **2** | Hệ thống ghi nhận thông tin thay đổi. |
| **3** | Hệ thống lưu lịch sử inventory. |

Output

Lịch sử thay đổi inventory được lưu lại.

Business Rules

Các thay đổi inventory phải có thông tin nguồn gốc giao dịch.

Exception / Alternative Flow

| Condition | System Behavior |
| :---- | :---- |
| Không thể ghi nhận lịch sử | giao dịch inventory không được coi là hoàn tất nếu việc ghi nhận là bắt buộc. |

### **Acceptance Criteria — Approved (BA Register v1.2)**

| AC ID | Acceptance Criterion |
| :---- | :---- |
| **016-AC09** | Audit mọi sửa Preview gồm actor, thời gian, trường, trước/sau. |
| **016-AC13** | Sửa Preview không chỉnh sửa/ghi đè file gốc. |
| **016-AC22** | Xem được hồ sơ phiếu Confirm: file gốc, dữ liệu cuối, lịch sử sửa, kết quả, người và thời điểm Confirm. |
| **016-AC23** | Chỉ Admin có quyền xem import history truy xuất hồ sơ; trái quyền bị từ chối. |
| **016-AC24** | Phiếu đã Confirm không sửa/xóa trực tiếp; sai sót xử lý theo quy trình Inventory Adjustment có audit. \[Câu chữ hiện hành v0.9; đề xuất tách Cost Correction vẫn BA PENDING.\] |
| **WAC-AC07** | Cost Correction tạo chứng từ tham chiếu nguồn, phiếu gốc bất biến. |
| **WAC-AC08** | Replay đúng thứ tự Import/Sale/Return/Restock/Inventory Adjustment/Cost Adjustment. |
| **WAC-AC09** | Replay không sửa COGS lịch sử. |
| **WAC-AC10** | Sửa tăng giá nhập phân bổ đúng chênh tồn và variance có dấu. |
| **WAC-AC11** | Sửa giảm giá nhập phân bổ đúng chênh âm, không tạo tồn/giá trị tồn invalid. |
| **WAC-AC12** | Chi phí chu kỳ tồn kết thúc không tự nhập WAC chu kỳ mới. |
| **WAC-AC13** | Sửa cùng chứng từ nhiều lần xét các adjustment trước, không cộng trùng. |
| **WAC-AC14** | Đối soát chênh nhập \= chênh tồn \+ variance \+ rounding residual theo quy tắc phê duyệt. |
| **WAC-AC15** | Thiếu lịch sử replay thiết yếu thì chuyển ngoại lệ, không suy đoán từ WAC hiện tại. |

## **3.3 Order Management**

### **3.8 FR-08 — Tạo đơn hàng**

| Field | Value |
| :---- | :---- |
| **FR ID** | FR-08 |
| **Actor** | Customer |
| **Related BR** | BR-09 — BR-16 |
| **Priority** | P0 |

Description;  Hệ thống cho phép Customer tạo đơn hàng từ các sản phẩm đã thêm vào giỏ hàng.

Preconditions

Customer có sản phẩm trong giỏ hàng.

Sản phẩm có thông tin hợp lệ.

Input

Danh sách sản phẩm.

Số lượng.

Thông tin giao hàng.

Thông tin thanh toán.

Main Flow

| Step | Action / Expected Behavior |
| :---- | :---- |
| **1** | Customer truy cập checkout. |
| **2** | Hệ thống kiểm tra thông tin giỏ hàng. |
| **3** | Hệ thống kiểm tra khả năng đáp ứng tồn kho. |
| **4** | Customer xác nhận đặt hàng. |
| **5** | Hệ thống tạo Order. |
| **6** | Order được đưa vào trạng thái phù hợp. |

Output

Order được tạo thành công.

Customer được chuyển sang bước thanh toán nếu cần.

Business Rules

Order chỉ được tạo khi thông tin cần thiết hợp lệ.

Số lượng sản phẩm phải đáp ứng tồn kho.

Exception / Alternative Flow

| Condition | System Behavior |
| :---- | :---- |
| Sản phẩm hết hàng | không tạo order. |
| Số lượng không đủ | yêu cầu Customer điều chỉnh số lượng. |
| Thông tin checkout không hợp lệ | yêu cầu nhập lại. |

### **Acceptance Criteria — Approved (BA Register v1.2)**

| AC ID | Acceptance Criterion |
| :---- | :---- |
| **C-AC01** | Guest Cart Merge đúng User Cart sau đăng nhập, giữ những dòng hợp lệ. |
| **C-AC02** | Hai dòng cùng ProductSize được hợp nhất; Merge lặp không cộng quantity lần hai. |
| **C-AC03** | Quantity sau Merge vượt stock không trừ tồn/tạo Order; áp FR-15 tại Checkout. |
| **C-AC04** | Product Inactive/Variant Retired giữ CartItem để cảnh báo, không thành lựa chọn mua hợp lệ. |
| **C-AC05** | Guest chưa đăng nhập không tạo Order; sau đăng nhập tiếp tục Cart/Checkout phù hợp. |
| **C-AC06** | Giá/khuyến mãi thay đổi thể hiện trong tổng tiền mới, yêu cầu xác nhận lại trước Order. |
| **C-AC07** | Không có Buy Now độc lập bypass kiểm tra Checkout |

### **3.9 FR-09 — Quản lý Order Lifecycle**

| Field | Value |
| :---- | :---- |
| **FR ID** | FR-09 |
| **Actor** | System / Admin |
| **Related BR** | BR-10 |
| **Priority** | P0 |

Description;  Hệ thống quản lý vòng đời của Order thông qua các trạng thái nghiệp vụ được định nghĩa.

Preconditions

Order đã được tạo.

Input

Order.

Sự kiện nghiệp vụ liên quan đến Order.

Main Flow

| Step | Action / Expected Behavior |
| :---- | :---- |
| **1** | Order được tạo. |
| **2** | Hệ thống xác định trạng thái hiện tại. |
| **3** | Khi có sự kiện phù hợp, hệ thống cập nhật trạng thái. |
| **4** | Hệ thống ghi nhận trạng thái mới. |
| **5** | Order tiếp tục workflow cho đến khi hoàn tất. |

Output

Order có trạng thái phản ánh đúng lifecycle.

Business Rules

Order phải tuân thủ lifecycle đã định nghĩa.

Chỉ các trạng thái hợp lệ mới được sử dụng.

Exception / Alternative Flow

| Condition | System Behavior |
| :---- | :---- |
| Sự kiện không phù hợp với trạng thái hiện tại | không cập nhật Order. |
| Order đã ở trạng thái cuối | không cho phép chuyển tiếp trái quy định. |

### **Acceptance Criteria — Approved (BA Register v1.2)**

| AC ID | Acceptance Criterion |
| :---- | :---- |
| **O-AC01** | Không có thao tác hủy một OrderItem riêng trong Cancellation hiện hành. |
| **O-AC02** | SHIPPED/DELIVERED không được hủy theo flow thông thường. |
| **O-AC03** | PAID chưa SHIPPED chỉ Admin có quyền duyệt theo policy. |
| **O-AC04** | Chưa xác nhận dừng fulfillment thì không Cancellation Restock. |
| **O-AC05** | Truy xuất Admin/time/reason, fulfillment status và Stock Restock liên quan. |
| **O-AC06** | Hủy lặp hoặc đồng thời không double-restock/duplicate refund obligation. |
| **O-AC07** | Refund PENDING/FAILED/UNKNOWN thì PAID Cancellation chưa Completed. |
| **O-AC08** | Retire Variant sau khi tạo Order không tự Cancel, |
| **WAC-AC29** | Cancellation chưa đủ điều kiện không restock; đủ điều kiện hoàn tối đa một lần. |
| **WAC-AC30** | Cancellation Restock theo cost snapshot gốc, tính WAC lại khi có movement xen giữa. |
| **WAC-AC31** | Refund Pending/Unknown/Failed không tự đổi WAC hoặc restock lặp; không double qua Customer Return. |

### **3.10 FR-10 — Kiểm soát Order State Transition**

| Field | Value |
| :---- | :---- |
| **FR ID** | FR-10 |
| **Actor** | System / Admin |
| **Related BR** | BR-11 |
| **Priority** | P0 |

Description;  Hệ thống kiểm tra tính hợp lệ của việc chuyển trạng thái Order trước khi thực hiện cập nhật.

Preconditions

Order tồn tại.

Order có trạng thái hiện tại.

Input

Trạng thái hiện tại.

Trạng thái yêu cầu chuyển đến.

Main Flow

| Step | Action / Expected Behavior |
| :---- | :---- |
| **1** | Hệ thống nhận yêu cầu thay đổi trạng thái. |
| **2** | Hệ thống xác định trạng thái hiện tại. |
| **3** | Hệ thống kiểm tra transition được phép. |
| **4** | Nếu hợp lệ, hệ thống cập nhật trạng thái. |
| **5** | Hệ thống ghi nhận kết quả. |

Output

Order được chuyển sang trạng thái hợp lệ.

Business Rules

Không cho phép chuyển sang trạng thái không được định nghĩa.

Không cho phép chuyển ngược lifecycle nếu business rule không cho phép.

Exception / Alternative Flow

| Condition | System Behavior |
| :---- | :---- |
| Transition không hợp lệ | từ chối yêu cầu và giữ nguyên trạng thái hiện tại. |

### **Acceptance Criteria — Approved (BA Register v1.2)**

| AC ID | Acceptance Criterion |
| :---- | :---- |
| **O-AC02** | SHIPPED/DELIVERED không được hủy theo flow thông thường. |
| **O-AC03** | PAID chưa SHIPPED chỉ Admin có quyền duyệt theo policy. |
| **O-AC04** | Chưa xác nhận dừng fulfillment thì không Cancellation Restock. |
| **O-AC05** | Truy xuất Admin/time/reason, fulfillment status và Stock Restock liên quan. |
| **O-AC06** | Hủy lặp hoặc đồng thời không double-restock/duplicate refund obligation. |
| **O-AC07** | Refund PENDING/FAILED/UNKNOWN thì PAID Cancellation chưa Completed. |
| **O-AC08** | Retire Variant sau khi tạo Order không tự Cancel, |

## **3.4 Payment Management**

### **3.11 FR-11 — Khởi tạo Payment**

| Field | Value |
| :---- | :---- |
| **FR ID** | FR-11 |
| **Actor** | Customer / System |
| **Related BR** | BR-12 |
| **Priority** | P0 |

Description;  Hệ thống cho phép Customer khởi tạo thanh toán trực tuyến cho Order.

Preconditions

Order đã được tạo.

Order ở trạng thái cho phép thanh toán.

Input

Order information.

Payment amount.

Payment method.

Main Flow

| Step | Action / Expected Behavior |
| :---- | :---- |
| **1** | Customer chọn phương thức thanh toán. |
| **2** | Hệ thống tạo payment request. |
| **3** | Hệ thống chuyển Customer đến payment gateway. |
| **4** | Customer thực hiện thanh toán. |

Output

Payment request được khởi tạo.

Customer được chuyển đến payment gateway.

Business Rules

Payment phải gắn với một Order hợp lệ.

Số tiền thanh toán phải tương ứng với Order.

Exception / Alternative Flow

| Condition | System Behavior |
| :---- | :---- |
| Không thể tạo payment request | thông báo lỗi cho Customer. |
| Payment gateway không phản hồi | payment chưa được ghi nhận thành công. |

### **Acceptance Criteria — Approved (BA Register v1.2)**

| AC ID | Acceptance Criterion |
| :---- | :---- |
| **PY-AC01** | Order/Inventory thất bại không phát sinh Payment Request thành công. |
| **PY-AC02** | Payment Request lưu Order, amount, reference, timestamp. |
| **PY-AC03** | Deadline từ khi Request tạo thành công; giá trị hiện hành là \+5 phút theo §19.4, Technical Verification Pending; thay giá trị \+15 phút của bản đã duyệt trước. |
| **PY-AC04** | Không rõ gateway đã nhận request thì không tự Cancel/Restock. |

### **3.12 FR-12 — Payment Verification**

| Field | Value |
| :---- | :---- |
| **FR ID** | FR-12 |
| **Actor** | System / Payment Gateway |
| **Related BR** | BR-13 |
| **Priority** | P0 |

Description;  Hệ thống xác minh kết quả thanh toán trước khi ghi nhận payment thành công.

Preconditions

Payment request đã được tạo.

Hệ thống nhận được payment result.

Input

Payment callback/result.

Payment information.

Main Flow

| Step | Action / Expected Behavior |
| :---- | :---- |
| **1** | Hệ thống nhận payment result. |
| **2** | Hệ thống kiểm tra thông tin giao dịch. |
| **3** | Hệ thống xác minh kết quả thanh toán. |
| **4** | Nếu hợp lệ, hệ thống ghi nhận payment. |
| **5** | Hệ thống cập nhật Order tương ứng. |

Output

Payment được xác định là thành công hoặc thất bại.

Order được cập nhật theo kết quả payment.

Business Rules

Payment result phải được xác minh trước khi ghi nhận thành công.

Exception / Alternative Flow

| Condition | System Behavior |
| :---- | :---- |
| Payment result không hợp lệ | không ghi nhận payment thành công. |
| Signature verification thất bại | từ chối callback. |

### **Acceptance Criteria — Approved (BA Register v1.2)**

| AC ID | Acceptance Criterion |
| :---- | :---- |
| **PV-AC01** | Callback sai chữ ký/reference/amount không ghi PAID. |
| **PV-AC02** | SUCCESS hợp lệ trên Order còn hợp lệ ghi PAID đúng một lần. |
| **PV-AC03** | Timeout không có kết quả cuối không tự UNKNOWN → FAILED. |
| **PV-AC04** | Reconciliation không xác minh được phải giữ exception và bằng chứng chuyển Admin. |

### **3.13 FR-13 — Idempotent Payment Processing**

| Field | Value |
| :---- | :---- |
| **FR ID** | FR-13 |
| **Actor** | System / Payment Gateway |
| **Related BR** | BR-14 |
| **Priority** | P0 |

Description;  Hệ thống xử lý payment result nhiều lần mà không tạo ra các business effect trùng lặp.

Preconditions

Payment result đã được gửi đến hệ thống.

Payment có transaction/reference phù hợp.

Input

Payment callback/result.

Main Flow

| Step | Action / Expected Behavior |
| :---- | :---- |
| **1** | Hệ thống nhận callback. |
| **2** | Hệ thống xác định payment transaction. |
| **3** | Hệ thống kiểm tra payment đã được xử lý hay chưa. |
| **4** | Nếu chưa xử lý, hệ thống thực hiện business effect. |
| **5** | Nếu đã xử lý, hệ thống không thực hiện lại business effect. |

Output

Một payment result chỉ tạo ra business effect cần thiết.

Business Rules

Duplicate payment result không được tạo duplicate business effect.

Exception / Alternative Flow

| Condition | System Behavior |
| :---- | :---- |
| Callback được gửi lại | hệ thống nhận diện và bỏ qua xử lý lặp. |
| Không xác định được payment transaction | callback bị từ chối hoặc đưa vào trạng thái xử lý lỗi. |

### **Acceptance Criteria — Approved (BA Register v1.2)**

| AC ID | Acceptance Criterion |
| :---- | :---- |
| **PI-AC01** | Cùng SUCCESS nhiều lần chỉ có một business effect. |
| **PI-AC02** | Hai callback đồng thời không duplicate PAID. |
| **PI-AC03** | SUCCESS đến muộn trên Order hợp lệ chưa Restock được xét theo policy. |
| **PI-AC04** | SUCCESS sau Cancel/Restock được ghi exception; không tự khôi phục Order/trừ stock. |

### **3.14 FR-14 — Payment Failure Handling**

| Field | Value |
| :---- | :---- |
| **FR ID** | FR-14 |
| **Actor** | Customer / System / Payment Gateway |
| **Related BR** | BR-15 |
| **Priority** | P0 |

Description;  Hệ thống xử lý trường hợp thanh toán thất bại, bị hủy hoặc không hoàn tất.

Preconditions

Order đang trong quá trình thanh toán.

Payment request đã được tạo.

Input

Payment failure result.

Payment status.

Main Flow

| Step | Action / Expected Behavior |
| :---- | :---- |
| **1** | Hệ thống nhận kết quả thanh toán. |
| **2** | Hệ thống xác định payment không thành công. |
| **3** | Hệ thống cập nhật payment status. |
| **4** | Hệ thống giữ hoặc chuyển Order sang trạng thái phù hợp. |
| **5** | Hệ thống thông báo kết quả cho Customer. |

Output

Payment được ghi nhận là thất bại/chưa hoàn tất.

Order không được ghi nhận là đã thanh toán.

Business Rules

Payment thất bại không được làm Order chuyển sang trạng thái PAID.

Exception / Alternative Flow

| Condition | System Behavior |
| :---- | :---- |
| Không nhận được payment result | Order tiếp tục ở trạng thái chờ xử lý theo business rule. |

### **Acceptance Criteria — Approved (BA Register v1.2)**

| AC ID | Acceptance Criterion |
| :---- | :---- |
| **PF-AC01** | FAILED cuối an toàn, không còn attempt mơ hồ: Cancel/Restock đúng một lần. |
| **PF-AC02** | Một attempt UNKNOWN còn tồn tại thì không Cancel/Restock chỉ vì attempt khác FAILED. |
| **PF-AC03** | Order Cancelled do FAILED muốn mua tiếp phải Checkout mới. |
| **PF-AC04** | Refund Completed có bằng chứng xác minh được amount, Order/Refund, reference, timestamp. |
| **PF-AC05** | Refund Gateway FAILED/UNKNOWN không tự Completed. |
| **PF-AC06** | Refund thủ công chỉ Completed sau Admin đủ quyền xác minh chứng từ hợp lệ. |
| **PF-AC07** | PAID Order chỉ Completed Cancellation khi fulfillment ngừng, stock đúng policy, refund obligation đã giải quyết. |

## **3.5 Stock & Order Consistency**

### **3.15 FR-15 — Stock Availability Check**

| Field | Value |
| :---- | :---- |
| **FR ID** | FR-15 |
| **Actor** | Customer / System |
| **Related BR** | BR-16 |
| **Priority** | P0 |

Description;  Hệ thống kiểm tra số lượng tồn kho trước khi xác nhận việc mua sản phẩm.

Preconditions

Customer đang thực hiện checkout.

Sản phẩm tồn tại.

Input

Product ID.

Size/variant.

Requested quantity.

Main Flow

| Step | Action / Expected Behavior |
| :---- | :---- |
| **1** | Customer xác nhận checkout. |
| **2** | Hệ thống lấy số lượng tồn hiện tại. |
| **3** | Hệ thống so sánh với số lượng Customer yêu cầu. |
| **4** | Nếu đủ hàng, workflow tiếp tục. |
| **5** | Nếu không đủ hàng, hệ thống từ chối việc mua. |

Output

Kết quả kiểm tra stock availability.

Business Rules

Không được xác nhận mua vượt quá số lượng tồn thực tế.

Exception / Alternative Flow

| Condition | System Behavior |
| :---- | :---- |
| Không đủ hàng | hiển thị số lượng có thể mua hoặc yêu cầu Customer điều chỉnh. |
| Sản phẩm không tồn tại | không cho phép tiếp tục checkout. |

### **Acceptance Criteria — Approved (BA Register v1.2)**

| AC ID | Acceptance Criterion |
| :---- | :---- |
| **ST-AC01** | Add to Cart không trừ/giữ stock. |
| **ST-AC02** | Yêu cầu 3, stock 2: đề xuất 2 và chưa tạo Order. |
| **ST-AC03** | Stock=0 đề xuất bỏ dòng; không Checkout nếu không còn dòng khả dụng. |
| **ST-AC04** | Khách từ chối chỉnh không tạo Order/Payment Request theo đề xuất. |
| **ST-AC05** | Giảm qty khiến mất discount thì tổng mới hiển thị để xác nhận. |
| **ST-AC06** | Stock đổi trước commit phải revalidate, không dùng snapshot cũ oversell. |
| **ST-AC07** | Checkout đa dòng Order/deduction/cost snapshot nhất quán; lỗi 1 dòng không tạo order thành công một phần. |

### **3.16 FR-16 — Concurrent Inventory Control**

| Field | Value |
| :---- | :---- |
| **FR ID** | FR-16 |
| **Actor** | System |
| **Related BR** | BR-17 |
| **Priority** | P0 |

Description;  Hệ thống kiểm soát việc nhiều Customer đồng thời mua cùng một sản phẩm có số lượng tồn thấp.

Preconditions

Có nhiều request mua cùng một sản phẩm/variant.

Số lượng tồn có giới hạn.

Input

Product/variant.

Requested quantity.

Concurrent purchase requests.

Main Flow

| Step | Action / Expected Behavior |
| :---- | :---- |
| **1** | Customer A và Customer B cùng thực hiện checkout. |
| **2** | Hệ thống xử lý các yêu cầu cập nhật inventory. |
| **3** | Hệ thống kiểm tra stock tại thời điểm cập nhật. |
| **4** | Request đủ điều kiện được xử lý thành công. |
| **5** | Request vượt quá stock còn lại bị từ chối. |

Output

Inventory không bị giảm xuống giá trị không hợp lệ.

Một hoặc nhiều Customer có thể nhận kết quả Out of Stock tùy stock thực tế.

Business Rules

Tổng số lượng được bán không được vượt quá inventory khả dụng.

Concurrent requests phải được xử lý theo cơ chế đảm bảo consistency.

Exception / Alternative Flow

| Condition | System Behavior |
| :---- | :---- |
| Stock không còn đủ tại thời điểm cập nhật | request bị từ chối. |
| Cập nhật inventory thất bại | giao dịch không được ghi nhận không hoàn chỉnh. |

### **Acceptance Criteria — Approved (BA Register v1.2)**

| AC ID | Acceptance Criterion |
| :---- | :---- |
| **WAC-AC32** | Concurrency Import/Checkout/Return/Restock/Adjustment không lost update hoặc stock âm. |
| **WAC-AC33** | Inactive/Retired không tự Reactivate hay vào tồn bán được trái policy. |
| **WAC-AC34** | Stale Preview/Cost snapshot phải revalidate; giao dịch lỗi không cập nhật một phần. |
| **016-AC20** | Double-click, request trùng hoặc Confirm đồng thời không tạo phiếu/tăng tồn lặp; chỉ người đang giữ quyền Confirm hợp lệ thực hiện; atomicity và idempotency. |
| **016-AC21** | Confirm thành công cập nhật stock/WAC theo WAC policy, không ghi đè thay đổi đồng thời. |

### **3.17 FR-17 — Order–Inventory Consistency**

| Field | Value |
| :---- | :---- |
| **FR ID** | FR-17 |
| **Actor** | System |
| **Related BR** | BR-18 |
| **Priority** | P0 |

Description;  Hệ thống đảm bảo các thay đổi liên quan đến Order và Inventory được thực hiện nhất quán.

Preconditions

Customer đang thực hiện purchase workflow.

Order và inventory cần được cập nhật.

Input

Order information.

Product/variant.

Quantity.

Main Flow

| Step | Action / Expected Behavior |
| :---- | :---- |
| **1** | Hệ thống xác nhận điều kiện mua hàng. |
| **2** | Hệ thống xử lý Order. |
| **3** | Hệ thống cập nhật inventory tương ứng. |
| **4** | Hệ thống hoàn tất giao dịch. |
| **5** | Hệ thống trả kết quả cho Customer. |

Output

Order và Inventory phản ánh cùng một business transaction.

Không xảy ra trường hợp Order được ghi nhận nhưng Inventory không được cập nhật hoặc ngược lại.

Business Rules

Các cập nhật liên quan phải đảm bảo consistency.

Không được để lại trạng thái dữ liệu không hoàn chỉnh.

Exception / Alternative Flow

| Condition | System Behavior |
| :---- | :---- |
| Một bước trong giao dịch thất bại | hệ thống không ghi nhận trạng thái không nhất quán. |
| Inventory không thể cập nhật | Order không được hoàn tất theo workflow yêu cầu. |

### **Acceptance Criteria — Approved (BA Register v1.2)**

| AC ID | Acceptance Criterion |
| :---- | :---- |
| **WAC-AC29** | Cancellation chưa đủ điều kiện không restock; đủ điều kiện hoàn tối đa một lần. |
| **WAC-AC30** | Cancellation Restock theo cost snapshot gốc, tính WAC lại khi có movement xen giữa. |
| **WAC-AC31** | Refund Pending/Unknown/Failed không tự đổi WAC hoặc restock lặp; không double qua Customer Return. |
| **WAC-AC32** | Concurrency Import/Checkout/Return/Restock/Adjustment không lost update hoặc stock âm. |
| **WAC-AC33** | Inactive/Retired không tự Reactivate hay vào tồn bán được trái policy. |
| **WAC-AC34** | Stale Preview/Cost snapshot phải revalidate; giao dịch lỗi không cập nhật một phần. |
| **ST-AC07** | Checkout đa dòng Order/deduction/cost snapshot nhất quán; lỗi 1 dòng không tạo order thành công một phần. |

## **3.6 Testing & Deployment**

### **3.18 FR-18 — Kiểm thử Business-Critical Workflows**

| Field | Value |
| :---- | :---- |
| **FR ID** | FR-18 |
| **Actor** | Developer / System |
| **Related BR** | BR-19 |
| **Priority** | P0 |

Description;  Hệ thống phải có khả năng kiểm thử các workflow quan trọng để đảm bảo business logic tiếp tục hoạt động đúng sau khi có thay đổi.

Preconditions

Các business-critical workflows đã được xác định.

Có các điều kiện kiểm thử tương ứng.

Input

Test cases.

Test data.

Business-critical workflows.

Main Flow

| Step | Action / Expected Behavior |
| :---- | :---- |
| **1** | Developer thực hiện thay đổi hệ thống. |
| **2** | Các automated tests được thực thi. |
| **3** | Hệ thống kiểm tra kết quả thực tế với expected result. |
| **4** | Hệ thống xác định test pass/fail. |
| **5** | Developer xử lý các lỗi nếu test thất bại. |

Output

Kết quả kiểm thử.

Các workflow quan trọng được xác nhận hoạt động đúng hoặc được phát hiện lỗi.

Business Rules

Các business-critical workflows phải có automated tests tương ứng.

Thay đổi không được làm phá vỡ các workflow đã được kiểm thử.

Exception / Alternative Flow

| Condition | System Behavior |
| :---- | :---- |
| Test thất bại | thay đổi chưa được coi là đạt yêu cầu. |
| Phát hiện regression | Developer phải xử lý trước khi tiếp tục deployment. |

### **Acceptance Criteria — Approved (BA Register v1.2)**

| AC ID | Acceptance Criterion |
| :---- | :---- |
| **T18-AC01** | Mọi AC P0 thuộc release scope có test case liên kết và priority rõ ràng. |
| **T18-AC02** | CI chạy P0 automated tests; bất kỳ P0 test bắt buộc FAILED hoặc NOT RUN đều chặn release. |
| **T18-AC03** | Import tests bao phủ validation, Draft/Preview, duplicate warnings, All-or-Nothing, stale Preview, double Confirm và authorization. |
| **T18-AC04** | WAC tests bao phủ stock-zero, weighted average, rounding, Cost Correction, cost snapshot, replay và các quy tắc Return đã duyệt; không tự yêu cầu full Return workflows chưa duyệt. |
| **T18-AC05** | Payment mock tests bao phủ SUCCESS, FAILED, UNKNOWN, invalid signature, duplicate IPN, late SUCCESS, QueryDR code 94, Refund states và idempotency. |
| **T18-AC06** | Order tests bao phủ invalid transitions, Customer không tự Cancel PAID, Admin stop-fulfillment, single restock và refund obligation. |
| **T18-AC07** | Concurrency/authorization tests kiểm chứng không oversell, lost update, duplicate business effects hoặc unauthorized access. |
| **T18-AC08** | Test reports có kết quả rõ Unit, Feature/Integration và concurrency tests cùng danh sách failed cases. |
| **T18-AC09** | E2E smoke trên môi trường phù hợp xác nhận các luồng cốt lõi trước release thực tế; không phát sinh giao dịch Production thử nghiệm thiếu kiểm soát. |
| **T18-AC10** | Tests có thể tái chạy từ seed/factory/fixtures độc lập, không phụ thuộc state tồn dư giữa các tests. |
| **T18-AC11** | Release Payment đầu tiên hoặc thay đổi workflow Payment có Sandbox evidence. Mốc Payment Request \+5 phút chưa được xác minh thì không đánh dấu Payment integration gate PASS. |
| **T18-AC12** | P0/P1 blocking defects được sửa và retest; P2 exception có risk description, người phê duyệt và kế hoạch theo dõi. |
| **T18-AC13** | Release test report gắn tag/commit SHA, test failures, FR/AC liên quan và kết luận quality gate. |

### **3.19 FR-19 — Deployment Nhất Quán Giữa Các Môi Trường**

| Field | Value |
| :---- | :---- |
| **FR ID** | FR-19 |
| **Actor** | Developer / System |
| **Related BR** | BR-20 |
| **Priority** | P1 |

Description;  Hệ thống phải hỗ trợ quy trình triển khai nhất quán giữa các môi trường nhằm giảm rủi ro do khác biệt môi trường.

Preconditions

Phiên bản hệ thống đã được chuẩn bị để deployment.

Môi trường deployment đã được cấu hình.

Input

Application version.

Configuration cần thiết.

Deployment package.

Main Flow

| Step | Action / Expected Behavior |
| :---- | :---- |
| **1** | Developer chuẩn bị phiên bản hệ thống. |
| **2** | Hệ thống thực hiện các bước kiểm tra cần thiết. |
| **3** | Phiên bản được triển khai theo quy trình đã định nghĩa. |
| **4** | Hệ thống khởi động phiên bản mới. |
| **5** | Developer kiểm tra kết quả deployment. |

Output

Phiên bản hệ thống được triển khai nhất quán.

Môi trường chạy phiên bản tương ứng với release.

Business Rules

Deployment phải tuân theo quy trình thống nhất.

Chỉ phiên bản đáp ứng điều kiện kiểm tra mới được triển khai.

Exception / Alternative Flow

| Condition | System Behavior |
| :---- | :---- |
| Deployment thất bại | hệ thống không được coi là đã release thành công. |
| Phát hiện lỗi sau deployment | thực hiện quy trình xử lý/recovery phù hợp. |

### **Acceptance Criteria — Approved (BA Register v1.2)**

| AC ID | Acceptance Criterion |
| :---- | :---- |
| **D19-AC01** | CI ghi nhận test results theo release tag/commit SHA cụ thể. |
| **D19-AC02** | Vercel Preview/Staging và Production dùng phiên bản source/dependency xác định; xác minh PHP/Laravel runtime và các dependency trước promotion. |
| **D19-AC03** | Các môi trường có environment variables, database config và secrets riêng; thiếu/sai cấu hình bắt buộc không âm thầm đi vào release. |
| **D19-AC04** | Runbook mô tả CI verification, Vercel Preview deploy, manual promotion, migration, post-deploy verification, recovery và người chịu trách nhiệm. |
| **D19-AC05** | P0/P1 blocking tests hoặc integration gates thuộc release scope chưa đạt thì không promote lên Production. |
| **D19-AC06** | Database migration được thử trên môi trường phù hợp; bất tương thích schema có plan chuyển đổi/recovery duyệt trước. |
| **D19-AC07** | Deploy dữ liệu/schema rủi ro có backup và evidence restore verification ở môi trường cách ly. |
| **D19-AC08** | Không rollback schema hoặc restore DB làm mất transaction mới khi chưa đánh giá tác động và xác định cách bảo toàn/đối soát. |
| **D19-AC09** | Production chỉ nhận phiên bản đã đạt Preview/Staging verification và được promote có kiểm soát. |
| **D19-AC10** | Smoke/health/log checks xác nhận core endpoints, DB connection, nghiệp vụ web và integration liên quan trên môi trường triển khai, không tạo giao dịch Production thử nghiệm thiếu kiểm soát. |
| **D19-AC11** | Deploy/build/runtime/post-deploy verification thất bại: không đánh dấu SUCCESSFUL; thực hiện xử lý hoặc recovery theo runbook. |
| **D19-AC12** | Recovery khi có transaction mới phải bảo toàn/đối soát Order, Payment, Inventory Movement, Import Receipt; không tùy tiện restore DB về backup cũ. |
| **D19-AC13** | Release record lưu tag/commit, Vercel deployment ID/URL, người thực hiện, CI, migration, backup, smoke tests và trạng thái Successful/Failed/Recovered. |

# **4\. Cross-Feature Workflows**

## **4.1 Business-Critical Shared Policies — Current Approved Baseline**

| Policy Area | Approved Policy / Qualification |
| :---- | :---- |
| Inventory | file CSV/XLSX 5 MB/1.000 dòng, một phiếu/file, Draft 7 ngày, All-or-Nothing, duplicate-warning liên phiếu, Confirm idempotent; Product/Variant Inactive/Retired bị chặn; WAC per ProductSize, ROUND\_HALF\_UP 2 chữ số; immutable evidence. |
| Checkout | Guest Cart merge một lần; phải login trước tạo Order; xác nhận lại quantity/price/promo/freight thay đổi. Atomic stock check và order deduction tại checkout; Cart không reserve stock. |
| Payment | tạo Order/trừ tồn trước phát hành VNPay URL. VN-01B: ExpireDate \+5 phút sau Payment Request thành công, SANDBOX TECHNICAL PENDING, không fallback 15 phút. VN-02A: QueryDR một lần theo sự kiện expiry nếu chưa có kết quả cuối, không polling. VN-03A: UNKNOWN sau QueryDR chuyển Admin ngay; VN-04A: mã 94 chỉ là query duplicate, không phải payment failed. IPN xác thực và idempotency là nguồn xử lý payment; Return URL chỉ UI. |
| Order State Matrix | CREATED → PAYMENT\_PENDING → PAID → PROCESSING → SHIPPED → DELIVERED; CREATED/PAYMENT\_PENDING → CANCELLED chỉ nếu payment an toàn. PAID/PROCESSING (chưa shipped) → CANCELLATION\_IN\_PROGRESS với Admin có quyền và evidence stop-fulfillment; → CANCELLED chỉ sau khi dừng giao hàng, xử lý stock và nghĩa vụ refund. UNKNOWN không tự cancel/restock. Customer không tự cancel PAID; late SUCCESS sau Cancel/Restock tạo exception. |
| Deployment | CI \+ manual runbook; P0 automated tests PASS, P1 blocking resolved, P2 approved exceptions; local/CI/Preview-Staging/Production. Vercel được chọn làm platform web nhưng Laravel PHP/runtime, DB/storage/background jobs cần technical feasibility gate. Recovery không làm mất giao dịch mới. |
| BA PENDING | làm rõ 016-AC24 (quantity vs Cost Correction) và workflow sai Variant sau Confirm; Return workflow end-to-end chưa được duyệt. Candidate flows WAC Return chỉ áp dụng policy đã duyệt. |

## **4.2 Functional Flow Summaries**

Phần này tổng hợp các workflow chính của HP\_SNEAKERS dựa trên các Functional Requirements đã được xác định ở Phần 3\.

### **Inventory Import**

Workflow cho phép Admin nhập dữ liệu hàng hóa từ file Excel/CSV và cập nhật inventory sau khi dữ liệu được kiểm tra và xác nhận.

Flow:

Admin;  ↓;  Upload Excel/CSV;  ↓;  Validate Data;  ↓;  Preview;  ↓;  Admin Confirm;  ↓;  Create/Record Inventory Transaction;  ↓;  Update Inventory

Functional Requirements: FR-03, FR-04, FR-05, FR-06, FR-07.

### **Order Processing**

Workflow xử lý quá trình Customer mua sản phẩm từ giỏ hàng đến khi Order được tạo, đồng thời đảm bảo số lượng tồn kho được kiểm tra và cập nhật nhất quán.

Flow:

Customer;  ↓;  Cart;  ↓;  Checkout;  ↓;  Check Stock;  ↓;  Create Order;  ↓;  Update Inventory;  ↓;  Order Created

Nếu không đủ stock:

Checkout;  ↓;  Check Stock;  ↓;  Insufficient Stock;  ↓;  Reject Purchase

Functional Requirements: FR-08, FR-09, FR-10, FR-15, FR-16, FR-17.

### **Payment Processing**

Workflow xử lý thanh toán trực tuyến thông qua payment gateway, bao gồm xác minh kết quả, xử lý callback trùng lặp và payment failure.

Flow:

Customer;  ↓;  Checkout;  ↓;  Create Payment;  ↓;  VNPay;  ↓;  Payment Callback;  ↓;  Verify Payment;  ↓;  Check Idempotency;  ↓;  Update Payment;  ↓;  Update Order

Nếu thanh toán thất bại:

Payment Callback;  ↓;  Payment Failed;  ↓;  Update Payment Status;  ↓;  Order không được ghi nhận là PAID

Functional Requirements: FR-11, FR-12, FR-13, FR-14.

### **Product Management & Search**

Workflow quản lý và tìm kiếm sản phẩm bao gồm hai nhóm người dùng chính.

Admin:

Admin;  ↓;  Product Management;  ↓;  Create / Update Product;  ↓;  Validate Data;  ↓;  Save Product

Customer:

Customer;  ↓;  Product Search;  ↓;  Apply Filter;  ↓;  Display Matching Products

Functional Requirements: FR-01, FR-02.

### **Testing & Deployment**

Workflow hỗ trợ kiểm tra các business-critical workflows và triển khai phiên bản hệ thống theo quy trình nhất quán.

Testing:

Code Change;  ↓;  Automated Tests;  ↓;  Pass / Fail;  ↓;  Nếu Pass → Continue

Nếu Fail → Fix → Test Again

Deployment:

Validated Version;  ↓;  Deployment Process;  ↓;  Environment;  ↓;  Deployment Verification;  ↓;  Release

Functional Requirements: FR-18, FR-19.

# **5\. Non-Functional Requirements**

Phần này xác định các yêu cầu về chất lượng và khả năng vận hành của HP\_SNEAKERS. Các yêu cầu được giới hạn ở những tiêu chí thực sự cần thiết đối với phạm vi của project.

### **Performance**

| NFR ID | Requirement |
| :---- | :---- |
| **NFR-01** | Hệ thống phải phản hồi Product Search và Filtering trong thời gian phù hợp để đảm bảo trải nghiệm người dùng. |
| **NFR-02** | Hệ thống phải xử lý các thao tác Checkout và Order Creation mà không gây ra độ trễ không cần thiết cho người dùng. |

### **Security**

| NFR ID | Requirement |
| :---- | :---- |
| **NFR-03** | Hệ thống phải kiểm soát quyền truy cập theo vai trò của người dùng. |
| **NFR-04** | Chỉ người dùng có quyền phù hợp mới được thực hiện các chức năng quản lý sản phẩm, inventory và order. |
| **NFR-05** | Thông tin liên quan đến authentication và payment phải được xử lý và bảo vệ an toàn. |

### **Reliability**

| NFR ID | Requirement |
| :---- | :---- |
| **NFR-06** | Hệ thống phải đảm bảo dữ liệu Order và Inventory nhất quán khi thực hiện các giao dịch mua hàng. |
| **NFR-07** | Hệ thống phải xử lý Payment Callback theo cách không tạo ra nhiều business effects ngoài mong muốn khi callback được gửi lặp lại. |
| **NFR-08** | Hệ thống phải xử lý các trường hợp lỗi trong Payment workflow mà không làm Order chuyển sang trạng thái thanh toán thành công không hợp lệ. |
| **NFR-09** | Hệ thống phải đảm bảo dữ liệu inventory không bị cập nhật một phần khi quá trình nhập kho hoặc cập nhật tồn kho xảy ra lỗi. |

### **Availability**

| NFR ID | Requirement |
| :---- | :---- |
| **NFR-10** | Các chức năng chính của hệ thống phải sẵn sàng để Customer và Admin sử dụng trong thời gian hệ thống được vận hành. |
| **NFR-11** | Lỗi của một thao tác hoặc workflow không được làm ảnh hưởng không cần thiết đến khả năng sử dụng các chức năng khác của hệ thống. |

# **6\. Testing & Acceptance**

| Verification Area | Requirement / Gate |
| :---- | :---- |
| Release tests | P0 automated tests bắt buộc PASS; P1 ảnh hưởng chức năng thay đổi/an toàn dữ liệu phải sửa trước release; P2 chỉ được chấp nhận với risk assessment và phê duyệt. Phân biệt Unit, Feature/Integration, concurrency và E2E smoke. |
| Payment verification | Mock IPN/QueryDR/Refund trong CI; Payment Sandbox bắt buộc trước lần release Payment đầu tiên và khi Payment workflow thay đổi. Tách test database và dữ liệu seed/factory; lưu Test Case ↔ AC ↔ FR ↔ version. |
| Traceability | FR-18 có T18-AC01–13; FR-19 có D19-AC01–13. Chi tiết ở Chương 3, không sao chép lần thứ hai. |

# **7\. Traceability & Pending Items**

## **7.1 Requirement Traceability — Original**

Phần này xác định mối liên hệ giữa Business Objective, Business Requirement, Functional Requirement và Use Case, nhằm đảm bảo các chức năng được xây dựng đều có nguồn gốc từ nhu cầu nghiệp vụ.

### **Requirement Traceability Matrix**

| Business Objective | Business Requirement | Functional Requirement | Use Case |
| :---- | :---- | :---- | :---- |
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

### **Traceability Flow**

Các requirement chính của HP\_SNEAKERS được liên kết theo chuỗi:

Business Objective

↓

Business Requirement

↓

Functional Requirement

↓

Use Case

↓

Activity / BPMN / Sequence Diagram

Ví dụ đối với Inventory:

BO-02

Cải thiện inventory accuracy

↓

BR-05 / BR-06 / BR-07 / BR-08

↓

FR-04 / FR-05 / FR-06 / FR-07

↓

UC-01 — Import Inventory

↓

Activity — Inventory Import Workflow

Ví dụ đối với Payment:

BO-05

Tăng payment reliability

↓

BR-13 / BR-14 / BR-15

↓

FR-12 / FR-13 / FR-14

↓

UC-04 — Process Payment

↓

Sequence — Payment Processing

### **Traceability Principle**

Requirement Traceability của HP\_SNEAKERS đảm bảo:

Mỗi Functional Requirement quan trọng có nguồn gốc từ Business Requirement hoặc Business Objective.

Các Functional Requirement được nhóm thành các Use Case tương ứng.

Các Use Case có thể tiếp tục được sử dụng làm cơ sở cho System Modeling.

Requirement không có justification rõ ràng không nên được bổ sung chỉ vì mục đích sử dụng thêm technology.

## **7.2 FR–BR–AC Traceability & Release Gates**

| Related requirements | BR / FR / AC / Release Gate |
| :---- | :---- |
| BR-01/02 | FR-01 → V-BR01–05/V-AC01–07. BR-03 → FR-02 → S-BR01–04/S-AC01–07. BR-04–08 → FR-03–07 → I16-BR01–19/016-AC01–24 (v0.9 effective) \+ WAC-BR/AC. |
| BR-09/16 | FR-08/15 → C-BR01–04/C-AC01–07, ST-BR01–03/ST-AC01–07. BR-10/11 → FR-09/10 → O-BR01–04/O-AC01–08 \+ State Matrix/WAC-AC29–31. |
| BR-12–15 | FR-11–14 → PY/PV/PI/PF BR/AC, VN-01B/02A/03A/04A. BR-17/18 → FR-16/17 → WAC-AC32–34 \+ relevant Import/Order AC. |
| BR-19 | FR-18 → T18-BR01–07/T18-AC01–13. BR-20 → FR-19 → D19-BR01–07/D19-AC01–13; Vercel D-09 and VG-01–VG-07 technical pending. |

## **7.3 Pending & Technical Verification**

| Pending Area | Dependency / Status |
| :---- | :---- |
| P0 | VNPay Sandbox expiry 5 phút chưa xác minh; QueryDR vnp\_ResponseCode vs vnp\_TransactionStatus, 94/5-min API; IPN validity/security; payment controller refactor; admin cancellation; safe refund/reconciliation. |
| Technical | MariaDB transaction/locking WAC/stock, Import file parser/security, authorization matrix, Vercel full Laravel/PHP runtime compatibility, external DB/object storage, background/expiry trigger, staging/manual promotion, backups/restore. P0/P1 test evidence và production deploy chưa PASS. |
| Business pending | 016-AC24 revision not approved; full Customer/Supplier Returns/credit settlement, wrong Variant after Confirm not approved workflow. |

## **7.4 Approved WAC Costing AC Without an Approved Full Return/Stock-Count FR**

Các AC dưới đây đã duyệt về costing/control theo Register v1.2 nhưng chưa được tự gán thành FR workflow Return/Stock Count mới; không khẳng định full workflow đã duyệt.

| AC ID | Acceptance Criterion |
| :---- | :---- |
| **WAC-AC16** | Kiểm kê tăng WAC hợp lệ tính delta/cost, lưu trước/sau và lý do. |
| **WAC-AC17** | Stock=0/WAC thiếu: Admin nhập đơn giá \+ lý do, khởi tạo WAC. |
| **WAC-AC18** | Stock\>0/WAC thiếu: chặn Confirm tăng tồn đến khi định giá tồn cũ. |
| **WAC-AC19** | Stale Inventory Count snapshot bị từ chối, yêu cầu reload. |
| **WAC-AC20** | WAC=0 hợp lệ không bị nhầm WAC missing. |
| **WAC-AC21** | Customer Return chưa đủ điều kiện không tăng tồn bán được. |
| **WAC-AC22** | Customer Return chấp nhận dùng COGS gốc và tính lại WAC khi cần. |
| **WAC-AC23** | Missing COGS gốc chuyển ngoại lệ, không tự định giá thay thế. |
| **WAC-AC24** | Return qty không vượt lượng đủ điều kiện chưa hoàn từ OrderItem/movement gốc. |
| **WAC-AC25** | Supplier Return lưu quantity, WAC xuất, giá trị xuất, Supplier Credit reference. |
| **WAC-AC26** | Supplier variance hỗ trợ dương, âm và bằng 0\. |
| **WAC-AC27** | Supplier Return vượt tồn hoặc xác nhận lặp không giảm kho/cost hai lần. |
| **WAC-AC28** | Supplier Return về stock=0 thì Import mới dùng WAC lô mới. |

Trạng thái: Approved costing rules / Return & Stock Count full workflow chưa xác định FR độc lập, thuộc BA Pending hoặc Candidate Scope. Giữ nguyên mã AC.

