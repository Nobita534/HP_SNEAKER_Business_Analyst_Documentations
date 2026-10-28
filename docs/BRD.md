# **BUSINESS REQUIREMENTS DOCUMENT (BRD) — HP\_SNEAKER Store**

Draft v2.0 \| 26/10/2026 \| Cấu trúc tinh gọn cho pet project.

Nguồn nội dung: BRD hiện hành, BA Decision & Acceptance Criteria Register v1.2 (26/10/2026). Giữ nguyên nội dung nghiệp vụ, mã tham chiếu và trạng thái Approved/Pending; không xác nhận implemented/tested.

# **1\. Project Overview & Business Objectives**

## **1.1 Giới thiệu và bối cảnh**

Xác định các yêu cầu nghiệp vụ của HP Sneakers Store phiên bản cải tiến.

### **Business Context**

Hệ thống là nền tảng bán giày trực tuyến.

Hỗ trợ khách hàng mua hàng và thanh toán.

Hỗ trợ Admin quản lý sản phẩm, đơn hàng và tồn kho.

Phiên bản cải tiến tập trung vào inventory accuracy, order reliability và payment reliability.

## **1.2 Business Objectives**

| BO ID | Mục tiêu nghiệp vụ | Mô tả |
| :---- | :---- | :---- |
| BO-01 | Giảm manual effort trong nhập kho | Giảm việc nhập từng sản phẩm/thông tin tồn kho thủ công. |
| BO-02 | Cải thiện inventory accuracy | Đảm bảo dữ liệu tồn kho chính xác và nhất quán. |
| BO-03 | Hạn chế overselling | Đảm bảo không bán vượt quá số lượng tồn thực tế. |
| BO-04 | Kiểm soát order lifecycle | Đảm bảo đơn hàng chỉ chuyển qua các trạng thái hợp lệ. |
| BO-05 | Tăng payment reliability | Đảm bảo kết quả thanh toán được xử lý chính xác và an toàn. |
| BO-06 | Cải thiện product discovery | Giúp khách hàng tìm và lựa chọn sản phẩm dễ dàng hơn. |
| BO-07 | Tăng khả năng kiểm thử business-critical workflows | Giảm rủi ro khi thay đổi các nghiệp vụ quan trọng. |
| BO-08 | Chuẩn hóa development và deployment | Đảm bảo hệ thống có môi trường phát triển và quy trình triển khai nhất quán. |

ID

## **1.3 Expected Business Outcomes & Success Criteria**

### **Inventory**

Giảm thời gian nhập dữ liệu hàng hóa.

Giảm lỗi do nhập dữ liệu thủ công.

Tăng độ chính xác của inventory.

Có khả năng kiểm tra dữ liệu trước khi cập nhật.

### **Order**

Order lifecycle rõ ràng hơn.

Giảm tình trạng order chuyển trạng thái không hợp lệ.

Tăng tính nhất quán giữa order và inventory.

### **Payment**

Giảm rủi ro xử lý payment không chính xác.

Hạn chế ảnh hưởng của duplicate payment callback.

Có khả năng xử lý payment failure rõ ràng hơn.

### **Customer Experience**

Khách hàng tìm kiếm sản phẩm dễ dàng hơn.

Giảm khả năng đặt mua sản phẩm không còn tồn.

Trải nghiệm checkout và payment đáng tin cậy hơn.

### **Operations**

Admin giảm manual effort.

Quy trình nhập kho có thể kiểm soát và audit tốt hơn.

Các nghiệp vụ quan trọng có cơ sở để automated testing.

### **Testing & Deployment**

Giảm rủi ro regression khi thay đổi các nghiệp vụ quan trọng.

Đảm bảo các business-critical workflows được kiểm tra tự động.

Giảm lỗi phát sinh do khác biệt giữa các môi trường.

Tăng tính nhất quán và reliability của quá trình deployment.

### **Success Criteria**

| SC ID | Success Criterion | Objective |
| :---- | :---- | :---- |
| SC-01 | Admin có thể import dữ liệu inventory từ file và kiểm tra dữ liệu trước khi xác nhận. | BO-01 |
| SC-02 | Dữ liệu không hợp lệ không được cập nhật vào inventory. | BO-02 |
| SC-03 | Inventory phản ánh chính xác các giao dịch nhập/xuất được hệ thống ghi nhận. | BO-02 |
| SC-04 | Hệ thống không cho phép xác nhận mua vượt quá số lượng tồn thực tế. | BO-03 |
| SC-05 | Order chỉ có thể chuyển qua các trạng thái hợp lệ. | BO-04 |
| SC-06 | Payment result được xác minh trước khi ghi nhận thanh toán thành công. | BO-05 |
| SC-07 | Duplicate payment result không tạo ra duplicate business effect. | BO-05 |
| SC-08 | Customer có thể tìm kiếm và lọc sản phẩm. | BO-06 |
| SC-09 | Các business-critical workflows có automated tests tương ứng. | BO-07 |
| SC-10 | Các business-critical workflows có automated tests và được kiểm tra khi hệ thống thay đổi. | BO-07 |
| SC-11 | Hệ thống có quy trình deployment nhất quán giữa các môi trường. | BO-08 |

# **2\. Stakeholders & Business Scope**

## 

| Stakeholder | Role / Responsibility |
| :---- | :---- |
| **Customer** | **Tìm kiếm, mua sản phẩm và thanh toán.** |
| **Admin** | **Quản lý sản phẩm, tồn kho và đơn hàng.** |
| **Supplier** | **Cung cấp hàng hóa và thông tin nhập kho.** |
| **Payment Gateway** | **Xử lý giao dịch thanh toán trực tuyến.** |
| **Development Team** | **Xây dựng, kiểm thử và duy trì hệ thống.** |

## **2.1 Stakeholders và người dùng**

### **Stakeholders**

Stakeholder

Vai trò / Mối quan tâm

Customer

Tìm kiếm, mua sản phẩm và thanh toán.

Admin

Quản lý sản phẩm, tồn kho và đơn hàng.

Supplier

Cung cấp hàng hóa và thông tin nhập kho.

Payment Gateway

Xử lý giao dịch thanh toán trực tuyến.

Development Team

Xây dựng, kiểm thử và duy trì hệ thống.

### **Users**

### **Customer**

Nhu cầu chính:

Xem và tìm kiếm sản phẩm.

Quản lý giỏ hàng.

Đặt hàng.

Thanh toán.

Theo dõi đơn hàng.

### **Admin**

Nhu cầu chính:

Quản lý sản phẩm.

Nhập và cập nhật tồn kho.

Quản lý đơn hàng.

Theo dõi trạng thái xử lý đơn hàng.

## **2.2 In Scope**

Phạm vi gồm:

Product Management

Inventory Management

Inventory Import

Order Management

Payment

Customer Purchase Flow

## **2.3 Out of Scope**

Không đi sâu vào:

Database design

API design

Laravel implementation

Frontend implementation

Infrastructure implementation

# **3\. Business Requirements (BR-01–BR-20)**

Các BR cấp Business Requirement giữ nguyên ID; nội dung chi tiết ở Chương 4 là baseline đã duyệt v1.2 nếu có khác biệt với mô tả tổng quát.

Đây là phần trọng tâm nhất của BRD.

### **Product Management**

| BR ID | Business Requirement |
| :---- | :---- |
| BR-01 | Hệ thống phải cho phép quản lý thông tin sản phẩm. |
| BR-02 | Hệ thống phải hỗ trợ quản lý các thuộc tính cần thiết của sản phẩm, bao gồm size và số lượng tồn. |
| BR-03 | Khách hàng phải có khả năng tìm kiếm và lọc sản phẩm. |

### **Inventory Management**

| BR ID | Business Requirement |
| :---- | :---- |
| BR-04 | Hệ thống phải cho phép Admin nhập dữ liệu hàng hóa từ file. |
| BR-05 | Hệ thống phải kiểm tra dữ liệu trước khi cập nhật tồn kho. |
| BR-06 | Hệ thống phải cho phép Admin xem trước dữ liệu trước khi xác nhận nhập kho. |
| BR-07 | Hệ thống chỉ được cập nhật tồn kho sau khi dữ liệu nhập kho được xác nhận hợp lệ. |
| BR-08 | Hệ thống phải ghi nhận các thay đổi liên quan đến nhập và cập nhật tồn kho. |

### **Order Management**

| BR ID | Business Requirement |
| :---- | :---- |
| BR-09 | Hệ thống phải cho phép khách hàng tạo đơn hàng từ giỏ hàng. |
| BR-10 | Hệ thống phải quản lý vòng đời đơn hàng theo các trạng thái nghiệp vụ hợp lệ. |
| BR-11 | Hệ thống phải ngăn chặn các chuyển đổi trạng thái đơn hàng không hợp lệ. |

### **Payment**

| BR ID | Business Requirement |
| :---- | :---- |
| BR-12 | Hệ thống phải hỗ trợ thanh toán trực tuyến. |
| BR-13 | Hệ thống phải xác minh kết quả thanh toán trước khi cập nhật trạng thái giao dịch. |
| BR-14 | Hệ thống phải đảm bảo một kết quả thanh toán không gây ra nhiều business effects ngoài mong muốn. |
| BR-15 | Hệ thống phải xử lý trường hợp thanh toán thất bại hoặc không hoàn tất. |

### **Inventory & Order Consistency**

| BR ID | Business Requirement |
| :---- | :---- |
| BR-16 | Hệ thống phải đảm bảo số lượng tồn kho được kiểm tra trước khi xác nhận mua hàng. |
| BR-17 | Hệ thống phải ngăn chặn việc nhiều khách hàng cùng mua vượt quá số lượng tồn thực tế. |
| BR-18 | Các cập nhật liên quan đến order và inventory phải đảm bảo tính nhất quán dữ liệu. |

### **Testing & Deployment**

| BR ID | Business Requirement |
| :---- | :---- |
| BR-19 | Hệ thống phải có khả năng kiểm thử các business-critical workflows để đảm bảo các chức năng quan trọng hoạt động đúng sau khi thay đổi. |
| BR-20 | Hệ thống phải đảm bảo quá trình triển khai được thực hiện nhất quán giữa các môi trường. |

# **4\. Business Rules & Policies**

Business Rules Approved theo BA Register v1.2; mã I16/WAC/V/S/C/O/PY/PV/PI/PF/ST/T18/D19 là trace IDs, không tự tạo BR mới.

## **4.1 Product & Variant Lifecycle**

| Rule ID | Business Rule |
| :---- | :---- |
| V-BR01 | Variant có lịch sử nếu đã liên kết OrderItem, phiếu nhập đã xác nhận, Inventory Movement, Adjustment, Return, Cost Correction hoặc chứng từ nghiệp vụ lâu dài tương đương. |
| V-BR02 | Variant có lịch sử không được sửa size trực tiếp hoặc hard delete; đổi size phải Retire cũ và tạo Variant mới hợp lệ với quantity ban đầu \= 0\. |
| V-BR03 | Hỗ trợ size số nguyên dương và bước 0,5; chuẩn hóa size trước kiểm tra trùng. Dải 35–45 chỉ là dải khởi tạo mặc định. |
| V-BR04 | Trong một Product chỉ có một ProductSize cho mỗi size chuẩn hóa, kể cả Retired; muốn mở bán cùng size phải Reactivate ID cũ. |
| V-BR05 | Retire/Inactive chặn Checkout và Stock Import mới; không tự hủy Order hợp lệ đã tạo trước đó; Order cũ xử lý theo Order Lifecycle/Payment. |

## **4.2 Search & Filter**

| Rule ID | Business Rule |
| :---- | :---- |
| S-BR01 | Chỉ Product Active xuất hiện công khai; tìm tên/brand không phân biệt chữ hoa thường, ranking exact → contains → approximate. |
| S-BR02 | Tìm/lọc kết hợp keyword, brand, category, size, price; AND giữa nhóm, OR giữa các giá trị cùng nhóm nếu UI hỗ trợ. |
| S-BR03 | Lọc/sắp xếp giá theo giá bán thực tế đang hiệu lực, thống nhất giá hiển thị. |
| S-BR04 | Product Active có size Active hết hàng vẫn thấy kèm nhãn; Retired không được coi là size lựa chọn bán hàng; giữ điều kiện tìm kiếm khi phân trang. |

## **4.3 Inventory Import**

| Rule ID | Business Rule |
| :---- | :---- |
| I16-BR01 | Một file CSV/Excel tương ứng một phiếu nhập gồm nhiều dòng ProductSize. |
| I16-BR02 | Template gồm SKU, Size, Quantity, Import Price; ngày chứng từ, ghi chú và mã chứng từ nguồn tùy chọn nhập tại UI. |
| I16-BR03 | Giới hạn file 5 MB và 1.000 dòng dữ liệu tại Upload và sau sửa Preview; bỏ qua header, dòng trống hoàn toàn. |
| I16-BR04 | Sai cấu trúc phải upload lại; lỗi dữ liệu có thể sửa Preview nhưng bắt buộc validate lại. |
| I16-BR05 | Quantity là số nguyên dương; Import Price không âm, định dạng VND không mơ hồ. |
| I16-BR06 | SKU, Product và ProductSize phải tồn tại, hợp lệ, Active/được phép nhập; Inactive/Retired phải được Reactivate hợp lệ trước Import; không tự tạo hoặc Reactivate. |
| I16-BR07 | Không cho hai dòng trùng SKU và Size trong cùng phiếu trước Confirm. |
| I16-BR08 | File đúng cấu trúc tạo Import Draft dù còn lỗi giá trị để Admin xem và chỉnh Preview. |
| I16-BR09 | Admin được sửa 4 trường, thêm/xóa dòng trên Preview; lưu lịch sử thay đổi và validate lại. |
| I16-BR10 | Draft tồn tại 7 ngày từ lúc tạo, không gia hạn; người tạo sửa và mặc định Confirm; Admin có thẩm quyền chuyển riêng quyền Confirm có lý do và audit, không tự chuyển quyền sửa. |
| I16-BR11 | File gốc lưu riêng, không bị ghi đè do sửa Preview. |
| I16-BR12 | Confirm chỉ khi tất cả dòng hợp lệ; All-or-Nothing, không ghi nhập kho một phần. |
| I16-BR13 | Import Code duy nhất cấp khi Draft đúng cấu trúc, giữ đến Confirm và không tái sử dụng; Confirm idempotent. |
| I16-BR14 | Trùng mã chứng từ nguồn hoặc trùng file gốc/dữ liệu nghiệp vụ cuối chuẩn hóa chỉ cảnh báo, Admin xác nhận rõ; không tự chặn, lưu bằng chứng. |
| I16-BR15 | Quantity hoặc WAC đổi sau Preview thì làm mới dữ liệu tính toán và yêu cầu xác nhận lại. |
| I16-BR16 | Trước Confirm hiển thị Import Code, số dòng, tổng Quantity, tổng giá trị và cảnh báo; Admin xác nhận rõ. |
| I16-BR17 | Confirm ghi phiếu, tồn, WAC và lịch sử nhất quán trong giao dịch atomic; thất bại thì không có phần nhập dở. |
| I16-BR18 | Sau Import lưu file gốc, dữ liệu cuối, lịch sử sửa, người xác nhận và kết quả. |
| I16-BR19 | Hồ sơ Confirm giữ đến khi có retention policy, phân quyền xem, không sửa/xóa trực tiếp; sai sót xử lý giao dịch điều chỉnh có lịch sử. |

## **4.4 Stock, WAC & Cost Correction**

| Rule ID | Business Rule |
| :---- | :---- |
| WAC-BR01 | WAC theo ProductSize; nhập kho tính bình quân gia quyền theo số lượng và giá vốn; bán thông thường không tự đổi WAC. |
| WAC-BR02 | Tồn trước nhập bằng 0: WAC bằng giá nhập mới, không pha WAC chu kỳ cũ; Customer Return/Cancellation Restock có rule riêng. |
| WAC-BR03 | WAC lưu 2 chữ số thập phân, ROUND\_HALF\_UP; sai số làm tròn phải đối soát được. |
| WAC-BR04 | WAC \= 0 hợp lệ khi có chứng cứ; WAC NULL/thiếu/không hợp lệ không mặc định thành 0\. |
| WAC-BR05 | Không sửa hoặc xóa phiếu nhập đã Confirm; Cost Correction tạo chứng từ mới tham chiếu nguồn. |
| WAC-BR06 | Replay chuỗi giao dịch có thứ tự và các adjustment trước đó để mô phỏng tác động, không ghi đè lịch sử. |
| WAC-BR07 | Phân bổ chênh lệch có dấu giữa giá trị tồn hiện tại và Cost Correction Variance; không hồi tố COGS bán cũ, không chuyển chi phí chu kỳ cũ sang lô mới. |
| WAC-BR08 | Cost Adjustment lưu chứng từ nguồn, trước/sau, chênh lệch tồn, variance, rounding residual, người thực hiện, thời điểm; không ghi effect trùng. |
| WAC-BR09 | Inventory Count dùng target quantity; tăng tồn với WAC hợp lệ dùng WAC hiện hành, có lý do và audit. |
| WAC-BR10 | Stock=0/WAC thiếu: Admin nhập đơn giá mỗi đơn vị và lý do để khởi tạo WAC; stock\>0/WAC thiếu chặn đến khi định giá tồn cũ. |
| WAC-BR11 | Customer Return liên kết sale gốc; chỉ hàng qua kiểm tra và được chấp nhận bán lại mới tăng tồn bán được, theo COGS gốc. |
| WAC-BR12 | Customer Return thiếu COGS hoặc chưa đủ điều kiện tiếp nhận đưa vào ngoại lệ; không tự tăng tồn, dùng WAC hiện tại hoặc Reactivate variant. |
| WAC-BR13 | Supplier Return giảm tồn và giá trị theo WAC hiện tại; chênh Supplier Credit với giá trị xuất ghi Supplier Return Variance riêng, WAC phần còn lại không tự đổi. |
| WAC-BR14 | Supplier Return không vượt tồn; nếu trả về stock=0 thì lần nhập tiếp theo sử dụng WAC giá nhập mới. |
| WAC-BR15 | Cancellation Restock cần đủ điều kiện ISSUE-004/007 và ngừng fulfillment; hoàn theo cost snapshot lúc trừ kho tối đa một lần, tính WAC lại nếu có giao dịch xen giữa; refund độc lập, không hoàn trùng Customer Return. |
| WAC-BR16 | Mọi thao tác stock/cost phải atomic, concurrency-safe, idempotent; không tự Reactivate hoặc vượt chính sách lifecycle. |

## **4.5 Cart & Checkout**

| Rule ID | Business Rule |
| :---- | :---- |
| C-BR01 | Guest Cart của session hiện tại được Merge vào User Cart một lần sau đăng nhập; dòng trùng ProductSize cộng quantity, không giữ/trừ stock. |
| C-BR02 | Guest có thể dùng Cart nhưng phải xác thực trước tạo Order; thao tác sửa Cart kiểm tra ownership. |
| C-BR03 | Checkout kiểm tra lại giá, ưu đãi, phí và tính hợp lệ hiện hành; thay đổi ảnh hưởng Order phải được khách xác nhận trước Order/Payment Request. |
| C-BR04 | Không triển khai Buy Now tạo Order độc lập trong scope hiện tại; nếu có nút Buy Now không được bypass Cart Checkout. |

## **4.6 Order & Cancellation**

| Rule ID | Business Rule |
| :---- | :---- |
| O-BR01 | Cancellation hiện tại chỉ áp dụng toàn bộ Order, không Partial Cancellation. |
| O-BR02 | Admin xác minh chưa SHIPPED, ghi bằng chứng dừng fulfillment, người, thời điểm, lý do và kết quả trước Restock. |
| O-BR03 | Cancellation tuân theo Payment: CREATED chỉ khi payment an toàn; PAYMENT\_PENDING/UNKNOWN theo Payment Policy; PAID chưa SHIPPED chỉ Admin được duyệt. |
| O-BR04 | PAID cancellation đủ điều kiện có thể hoàn tồn mà không chờ Refund success; chỉ hoàn tất Cancellation khi fulfillment, inventory và refund obligation đều giải quyết hợp lệ. |

## **4.7 Payment & Refund**

| Rule ID | Business Rule |
| :---- | :---- |
| PY-BR01 | Payment Request chỉ sau Order/Inventory hợp lệ; amount và reference gắn đúng Order. Deadline tính từ khi Payment Request được tạo thành công. \[Deadline 5 phút theo §19.4 mới có hiệu lực ưu tiên, thay 15 phút ban đầu.\] |
| PY-BR02 | Chắc chắn không phát sinh request/giao dịch có thể hủy Order và Restock theo policy; nếu không chắc gateway đã tiếp nhận phải Reconcile trước. |
| PV-BR01 | Chỉ Payment được xác minh chữ ký, Order/reference/amount và trạng thái mới tác động business status. |
| PV-BR02 | UNKNOWN không tự FAILED; đối soát có giới hạn, chuyển Admin khi chưa xác minh. \[Theo §19.4: QueryDR theo sự kiện hết hạn, chưa rõ kết quả thì Admin ngay, không polling định kỳ.\] |
| PI-BR01 | Payment Result trùng không tạo thêm payment/order/inventory effect, truy vết bằng transaction identity. |
| PI-BR02 | SUCCESS muộn: Order hợp lệ/chưa hoàn tồn có thể PAID; đã Cancel/Restock phải vào exception không tự đảo trạng thái. |
| PF-BR01 | Order chưa PAID chỉ Cancel/Restock do FAILED khi tất cả payment attempts liên quan an toàn và không còn SUCCESS/UNKNOWN chưa giải quyết; Restock tối đa một lần. |
| PF-BR02 | Sau FAILED và Order Cancelled, mua tiếp phải Checkout mới, không tạo Payment Request mới trên Order Cancelled. |
| PF-BR03 | Refund Completed chỉ khi có bằng chứng hoàn tiền thành công được xác minh (gateway hoặc chuyển khoản thủ công hợp lệ); FAILED/UNKNOWN tiếp tục là nghĩa vụ và không retry thiếu kiểm soát. |

## **4.8 Stock Availability**

| Rule ID | Business Rule |
| :---- | :---- |
| ST-BR01 | Cart không giữ stock; Checkout kiểm tra ProductSize/stock và cập nhật Order–Inventory nguyên tử. |
| ST-BR02 | Thiếu stock thì đề xuất quantity khả dụng; stock=0 đề xuất bỏ dòng khỏi Checkout; cần xác nhận khách. |
| ST-BR03 | Mọi thay đổi qty/giá/phí/ưu đãi phải tính lại, hiển thị và xác nhận; thay đổi trước commit phải revalidate, không tạo Payment Request trên phương án cũ. |

## **4.9 Business-Critical Testing**

| Rule ID | Business Rule |
| :---- | :---- |
| T18-BR01 | Các AC quan trọng thuộc release scope phải có test case liên kết và mức độ rủi ro. Automated P0 tests phải PASS trước khi release. |
| T18-BR02 | P0 coverage bao gồm các workflow rủi ro cao đã duyệt: Import, WAC, Order, Stock, VNPay, Cancellation/Refund, authorization, concurrency và idempotency. Không tự mở rộng sang workflow chưa được BA phê duyệt. |
| T18-BR03 | Áp dụng Unit, Feature/Integration và concurrency tests phù hợp rủi ro, cùng E2E smoke cho luồng cốt lõi. E2E smoke không thay thế P0 automated tests. |
| T18-BR04 | CI kiểm thử IPN, QueryDR, Refund bằng mock/stub. VNPay Sandbox verification bắt buộc trước release Payment đầu tiên và khi thay đổi Payment workflow. |
| T18-BR05 | Tests sử dụng database độc lập và dữ liệu tái lập, làm sạch/reset phù hợp giữa tests; không dùng dữ liệu Production thật làm dữ liệu kiểm thử thông thường. |
| T18-BR06 | P0 thất bại hoặc không được chạy phải chặn release; P1 ảnh hưởng chức năng thay đổi/an toàn dữ liệu phải sửa; P2 chỉ được chấp nhận khi có ghi nhận, phân tích rủi ro và phê duyệt. |
| T18-BR07 | Mỗi release lưu evidence truy vết Test Case ↔ ↔AC FR ↔ version, gồm kết quả, failures và quyết định xử lý ngoại lệ. |

## **4.10 Deployment**

| Rule ID | Business Rule |
| :---- | :---- |
| D19-BR01 | Vercel là nền tảng deployment web mục tiêu. Phân tách Local, CI Test, Vercel Preview/Staging tối giản và Production khi triển khai thực tế. |
| D19-BR02 | Release xác định bằng tag/commit SHA, dependency lockfiles và cấu hình riêng theo môi trường; secrets không hardcode hoặc lưu công khai trong source. |
| D19-BR03 | CI thực hiện kiểm tra; Production promotion phải có kiểm soát thủ công theo runbook. Cấu hình Vercel không được tự đưa phiên bản chưa qua release gate vào Production chỉ vì push code. |
| D19-BR04 | Release phải vượt quality gates áp dụng: P0/P1 blocking tests, secrets/config, migrations, backup/recovery readiness, Vercel runtime compatibility và integration verification. |
| D19-BR05 | Database migrations ưu tiên backward compatibility và triển khai có kiểm soát. Rollback/recovery không được tự làm mất Order, Payment, Inventory hoặc chứng từ mới; cần forward-fix/đối soát khi rollback không an toàn. |
| D19-BR06 | Deploy rủi ro dữ liệu phải có backup và restore verification. Import Evidence, Order, Payment và Inventory data không phụ thuộc filesystem tạm của Vercel Functions. |
| D19-BR07 | Release chỉ SUCCESSFUL sau deployment, migration verification, smoke tests, health/log checks và lưu evidence. Lỗi nghiêm trọng kích hoạt recovery theo runbook. |

# **5\. Business Policy Matrices**

Chính sách xuyên suốt đã phê duyệt, áp dụng ưu tiên khi mô tả trước đó mâu thuẫn. Các chi tiết kiểm thử được dẫn chiếu trong FRD.

| Policy Area | Approved Policy / Notes |
| :---- | :---- |
| Order/Inventory | tạo Order, trừ tồn và lưu cost snapshot nguyên tử trước phát hành VNPay Payment Request; không tạo Order lại từ Return URL. Không giữ tồn tại Cart. Guest Cart merge một lần sau login; khách xác nhận lại thay đổi quantity/tổng tiền. |
| Payment | deadline mục tiêu 5 phút tính từ Payment Request thành công (VN-01B), phải xác minh Sandbox; QueryDR theo sự kiện khi hết hạn mà chưa có kết quả cuối (VN-02A); không polling. UNKNOWN sau QueryDR chuyển Admin ngay (VN-03A); mã QueryDR 94 không phải Payment FAILED (VN-04A). Không tự chuyển FAILED/hủy/hoàn tồn khi còn payment attempt chưa an toàn. IPN/late SUCCESS xử lý idempotent. |
| Cancellation | hủy toàn bộ Order (không partial). PAID trước SHIPPED chỉ Admin có quyền xét hủy, kiểm tra dừng fulfillment; hoàn tồn tối đa một lần theo cost snapshot gốc. Refund Pending/Failed/Unknown giữ nghĩa vụ; CANCELLATION\_IN\_PROGRESS kết thúc CANCELLED chỉ khi dừng fulfillment, tồn và nghĩa vụ refund đều được xử lý. Không tự hủy Order SHIPPED/DELIVERED. |
| Import | 19 I16-BR/24 016-AC hiệu lực v0.9, đã áp dụng thay thế I16-BR06/10/14 và 016-AC04/10/16/17/20. Câu chữ sửa 016-AC24 (tách Quantity Adjustment/Cost Correction) CHƯA PHÊ DUYỆT; giữ AC24 cũ và đánh dấu BA Pending. |
| Returns | chỉ các quy tắc định giá/kiểm soát đã duyệt; chưa phê duyệt toàn bộ workflow Customer/Supplier Return hay Supplier Credit settlement. Vercel đã được chọn làm platform web mục tiêu nhưng Laravel/PHP runtime, DB, storage, background processing chưa được xác minh. |

## **5.1 Quy tắc nghiệp vụ tổng quát từ BRD gốc**

Các BRULE dưới đây là mô tả tổng quát, không thay thế quy tắc cụ thể tại Chương 4\.

### **Inventory Rules**

| Rule ID | Business Rule |
| :---- | :---- |
| BRULE-01 | Inventory Validation — Dữ liệu nhập kho phải đáp ứng các điều kiện bắt buộc trước khi được cập nhật vào inventory. |
| BRULE-02 | Import Confirmation — Dữ liệu chỉ được cập nhật vào inventory sau khi Admin kiểm tra và xác nhận. |
| BRULE-03 | Stock Availability — Sản phẩm chỉ được xác nhận mua khi số lượng tồn đáp ứng số lượng khách hàng yêu cầu. |
| BRULE-04 | Stock Consistency — Số lượng tồn kho không được giảm xuống giá trị không hợp lệ do một giao dịch mua hàng. |

BRULE-01 — Inventory Validation

Dữ liệu nhập kho phải đáp ứng các điều kiện bắt buộc trước khi được cập nhật vào inventory.

BRULE-02 — Import Confirmation

Dữ liệu chỉ được cập nhật vào inventory sau khi Admin kiểm tra và xác nhận.

BRULE-03 — Stock Availability

Sản phẩm chỉ được xác nhận mua khi số lượng tồn đáp ứng số lượng khách hàng yêu cầu.

BRULE-04 — Stock Consistency

Số lượng tồn kho không được giảm xuống giá trị không hợp lệ do một giao dịch mua hàng.

### **Order Rules**

| Rule ID | Business Rule |
| :---- | :---- |
| BRULE-05 | Valid Order Transition — Order chỉ được chuyển sang các trạng thái được phép trong lifecycle. |

BRULE-05 — Valid Order Transition

Order chỉ được chuyển sang các trạng thái được phép trong lifecycle.

Ví dụ:

CREATED

↓

PAYMENT\_PENDING

↓

PAID

↓

PROCESSING

↓

SHIPPED

↓

DELIVERED

Một trạng thái không thể chuyển ngược hoặc chuyển sang trạng thái không được định nghĩa.

### **Payment Rules**

| Rule ID | Business Rule |
| :---- | :---- |
| BRULE-06 | Payment Verification — Kết quả thanh toán phải được xác minh trước khi hệ thống ghi nhận giao dịch thành công. |
| BRULE-07 | Duplicate Payment Processing — Một payment result được gửi nhiều lần không được tạo ra nhiều business effects. |
| BRULE-08 | Failed Payment — Thanh toán thất bại không được làm cho đơn hàng được ghi nhận là đã thanh toán. |

BRULE-06 — Payment Verification

Kết quả thanh toán phải được xác minh trước khi hệ thống ghi nhận giao dịch thành công.

BRULE-07 — Duplicate Payment Processing

Một payment result được gửi nhiều lần không được tạo ra nhiều business effects.

BRULE-08 — Failed Payment

Thanh toán thất bại không được làm cho đơn hàng được ghi nhận là đã thanh toán.

### **Import Rules**

| Rule ID | Business Rule |
| :---- | :---- |
| BRULE-09 | File Validation — File import phải đáp ứng format và các trường dữ liệu bắt buộc. |
| BRULE-10 | Invalid Records — Các record không hợp lệ phải được xác định để Admin có thể kiểm tra và xử lý. |

BRULE-09 — File Validation

File import phải đáp ứng format và các trường dữ liệu bắt buộc.

BRULE-10 — Invalid Records

Các record không hợp lệ phải được xác định để Admin có thể kiểm tra và xử lý.

### **Testing Rules**

| Rule ID | Business Rule |
| :---- | :---- |
| BRULE-11 | Critical Workflow Testing;  Các business-critical workflows phải có automated tests để đảm bảo chức năng hoạt động đúng sau khi có thay đổi. — Các business-critical workflows phải có automated tests để đảm bảo chức năng hoạt động đúng sau khi có thay đổi. |
| BRULE-12 | Regression Protection;  Các thay đổi hệ thống không được làm ảnh hưởng đến các business-critical workflows đã được kiểm thử. — Các thay đổi hệ thống không được làm ảnh hưởng đến các business-critical workflows đã được kiểm thử. |

BRULE-11 — Critical Workflow Testing;  Các business-critical workflows phải có automated tests để đảm bảo chức năng hoạt động đúng sau khi có thay đổi.

BRULE-12 — Regression Protection;  Các thay đổi hệ thống không được làm ảnh hưởng đến các business-critical workflows đã được kiểm thử.

### **Constraints**

Inventory ban đầu được giả định là được cung cấp thông qua file Excel/CSV.

Hệ thống phụ thuộc vào external payment gateway.

Các nghiệp vụ inventory và payment yêu cầu tính nhất quán dữ liệu cao.

Các quyết định về database transaction, locking, queue... sẽ được xác định ở Technical Design.

Môi trường development và deployment phải được chuẩn hóa để giảm khác biệt giữa các môi trường.

Chi tiết về công cụ và quy trình CI/CD sẽ được xác định ở Technical Design.

# **6\. Risks, Dependencies & Pending Decisions**

| Issue | Status | Dependency / Notes |
| :---- | :---- | :---- |
| Technical Pending | Pending | VNPay merchant chấp nhận expiry \+5 phút, mã QueryDR 94/callback/IPN, concurrency, database schema/ledger, role matrix, test evidence, Vercel PHP community runtime, persistent storage, queues, manual promotion, backup/recovery. |
| BA Pending | Pending | sửa câu chữ 016-AC24; full Return workflows, quy trình xử lý sai Variant sau Confirm. Không tự ghi nhận các đề xuất này là Approved. |

Traceability: BR-19 ↔ FR-18 ↔ T18-BR01–07/T18-AC01–13; BR-20 ↔ FR-19 ↔ D19-BR01–07/D19-AC01–13; FR-03–07 ↔ I16-BR01–19/016-AC01–24; FR-06/07/09/10/16/17 ↔ WAC-BR/AC.

# **7\. Requirement Traceability**

BO → BR → FR. Mọi BR giữ ID gốc; dùng mã Business Rule, FR và AC để kiểm soát thay đổi theo Register v1.2.

| Related requirements | Existing references |
| :---- | :---- |
| BR-19 | FR-18 ↔ T18-BR01–07/T18-AC01–13 |
| BR-20 | FR-19 ↔ D19-BR01–07/D19-AC01–13 |
| FR-03–07 | I16-BR01–19/016-AC01–24 |
| FR-06/07/09/10/16/17 | WAC-BR/AC. |

Traceability: BR-19 ↔ FR-18 ↔ T18-BR01–07/T18-AC01–13; BR-20 ↔ FR-19 ↔ D19-BR01–07/D19-AC01–13; FR-03–07 ↔ I16-BR01–19/016-AC01–24; FR-06/07/09/10/16/17 ↔ WAC-BR/AC.

Tài liệu FRD v2.0 tiếp nhận FR-01–FR-19 và các AC liên quan; DBCS v2.0 lưu các DB-001–DB-008 ở trạng thái schema proposed.

BẢNG THAM CHIẾU NHANH — BRD

| BR ID | Business Requirement |
| :---- | :---- |
| BR-01 | Hệ thống phải cho phép quản lý thông tin sản phẩm. |
| BR-02 | Hệ thống phải hỗ trợ quản lý các thuộc tính cần thiết của sản phẩm, bao gồm size và số lượng tồn. |
| BR-03 | Khách hàng phải có khả năng tìm kiếm và lọc sản phẩm. |
| BR-04 | Hệ thống phải cho phép Admin nhập dữ liệu hàng hóa từ file. |
| BR-05 | Hệ thống phải kiểm tra dữ liệu trước khi cập nhật tồn kho. |
| BR-06 | Hệ thống phải cho phép Admin xem trước dữ liệu trước khi xác nhận nhập kho. |
| BR-07 | Hệ thống chỉ được cập nhật tồn kho sau khi dữ liệu nhập kho được xác nhận hợp lệ. |
| BR-08 | Hệ thống phải ghi nhận các thay đổi liên quan đến nhập và cập nhật tồn kho. |
| BR-09 | Hệ thống phải cho phép khách hàng tạo đơn hàng từ giỏ hàng. |
| BR-10 | Hệ thống phải quản lý vòng đời đơn hàng theo các trạng thái nghiệp vụ hợp lệ. |
| BR-11 | Hệ thống phải ngăn chặn các chuyển đổi trạng thái đơn hàng không hợp lệ. |
| BR-12 | Hệ thống phải hỗ trợ thanh toán trực tuyến. |
| BR-13 | Hệ thống phải xác minh kết quả thanh toán trước khi cập nhật trạng thái giao dịch. |
| BR-14 | Hệ thống phải đảm bảo một kết quả thanh toán không gây ra nhiều business effects ngoài mong muốn. |
| BR-15 | Hệ thống phải xử lý trường hợp thanh toán thất bại hoặc không hoàn tất. |
| BR-16 | Hệ thống phải đảm bảo số lượng tồn kho được kiểm tra trước khi xác nhận mua hàng. |
| BR-17 | Hệ thống phải ngăn chặn việc nhiều khách hàng cùng mua vượt quá số lượng tồn thực tế. |
| BR-18 | Các cập nhật liên quan đến order và inventory phải đảm bảo tính nhất quán dữ liệu. |
| BR-19 | Hệ thống phải có khả năng kiểm thử các business-critical workflows để đảm bảo các chức năng quan trọng hoạt động đúng sau khi thay đổi. |
| BR-20 | Hệ thống phải đảm bảo quá trình triển khai được thực hiện nhất quán giữa các môi trường. |

