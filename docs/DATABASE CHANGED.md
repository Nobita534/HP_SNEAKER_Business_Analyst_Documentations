# **DATABASE CHANGE SPECIFICATION (DBCS) — HP\_SNEAKER Store**

Draft v2.0 \| 26/10/2026 \| Pet project. Source of truth: DBCS hiện hành, BA Register v1.2, FRD. Schema PROPOSED / Technical Review Pending; không phải implementation.

# **1\. Introduction & Scope**

DATABASE CHANGED \| HP Sneakers Store \| Bản đặc tả thay đổi cơ sở dữ liệu (đề xuất)

Trạng thái: PROPOSED SCHEMA DESIGN — Chờ Technical Review. Business Rules/AC đã phê duyệt vẫn là nguồn yêu cầu; tên bảng/cột dưới đây là phương án thiết kế, chưa phải quyết định implementation.

Nguồn đánh giá: SQL dump sneakershop(2).sql (05/10/2026, MariaDB 10.4.32), FRD FR-01–FR-19 và BA Decision Register v1.2 (26/10/2026), bao gồm ISSUE-011/WAC, ISSUE-016 Import, VN-01B–VN-04A, T-01–T-08 và D-01–D-09. Phần 1–3 mô tả database design candidates; phần 4 đồng bộ các quyết định mới. Mọi bảng/cột vẫn chờ Technical Review.

Phạm vi triển khai: KHÔNG đặt yêu cầu migrate/backfill/đối soát dữ liệu lịch sử trong SQL dump; tập trung thiết kế schema TO-BE cho dữ liệu nghiệp vụ phát sinh về sau. Tuy vậy, hệ thống sau nâng cấp vẫn phải bảo toàn lịch sử nghiệp vụ của chính nó. Không thực thi SQL, tạo migration, chỉnh mã nguồn hoặc sửa các tab AS-IS TO-BE / BRD / FRD.

# **2\. Current Database Assessment**

## **Kết luận tổng quan**

Database có bộ bảng nền tảng cho Catalog, Cart, Order, VNPay và nhập kho (19 bảng trong dump). Tuy nhiên, cấu trúc hiện tại chưa thể biểu diễn đầy đủ các workflow và dữ liệu chứng cứ mà BA đã chốt: ProductSize lifecycle, phiếu nhập có Header/Lines/Draft, toàn bộ stock movements, WAC replay, Cancellation Restock, Payment Reconciliation và Refund.

Ba structural gaps ưu tiên:

| Gap ID | Problem / Business Impact |
| :---- | :---- |
| GAP-01 | Inventory history thiếu loại nghiệp vụ: inventory\_transactions.type hiện chỉ có import và adjustment; không đủ cho sale, cancellation restock, customer/supplier return và cost correction/revaluation. |
| GAP-02 | Thiếu Variant FK: cart\_items, order\_items và inventory\_transactions dùng product\_id \+ size; chưa có product\_size\_id trực tiếp để bảo đảm ProductSize identity bất biến xuyên suốt. |
| GAP-03 | Rủi ro mất lịch sử qua FK: nhiều quan hệ nghiệp vụ có ON DELETE CASCADE (ví dụ products → product\_sizes/order\_items/inventory\_transactions; users → orders), trái nguyên tắc giữ chứng từ/Order lịch sử khi đã phát sinh. |

GAP-01 — Inventory history thiếu loại nghiệp vụ: inventory\_transactions.type hiện chỉ có import và adjustment; không đủ cho sale, cancellation restock, customer/supplier return và cost correction/revaluation.

GAP-02 — Thiếu Variant FK: cart\_items, order\_items và inventory\_transactions dùng product\_id \+ size; chưa có product\_size\_id trực tiếp để bảo đảm ProductSize identity bất biến xuyên suốt.

GAP-03 — Rủi ro mất lịch sử qua FK: nhiều quan hệ nghiệp vụ có ON DELETE CASCADE (ví dụ products → product\_sizes/order\_items/inventory\_transactions; users → orders), trái nguyên tắc giữ chứng từ/Order lịch sử khi đã phát sinh.

## **Mức độ ưu tiên và ranh giới**

P0 — Cần cho tính đúng đắn nghiệp vụ: Variant identity/lifecycle; tồn, giá vốn và sổ movement; Order stock deduction/restock; Payment Request/reconciliation/refund; idempotency/concurrency; FK bảo toàn chứng từ.

P1 — Cần khi hoàn thiện chức năng đã duyệt: Goods Receipt Header/Lines, Import Draft/Preview/evidence và Cost Correction/Inventory Count audit; một số thành phần có thể triển khai đồng thời với P0 theo thứ tự phụ thuộc.

P2 — Tối ưu: bổ sung index tìm kiếm/lọc sau khi có truy vấn và kiểm thử hiệu năng thực tế.

PENDING — Chưa tự đưa vào scope triển khai hoàn chỉnh: Customer/Supplier Return workflow, refund một phần, quyền Admin chi tiết và các chính sách chưa được BA phê duyệt. WAC-costing policy đã duyệt không có nghĩa toàn bộ Return Management được duyệt.

# **3\. Database Change Specifications (DB-001–DB-008)**

## 

| DB ID | Database Change |
| :---- | :---- |
| **DB-001** | **ProductSize Lifecycle & Referential Identity** |
| **DB-002** | **Search & Filter Support** |
| **DB-003** | **Goods Receipt Header/Lines & Import Draft** |
| **DB-004** | **Inventory Movement Ledger, WAC & Cost Correction** |
| **DB-005** | **OrderItem Variant/Cost Snapshot & Cancellation History** |
| **DB-006** | **Payment Request, Reconciliation, Idempotency & Refund** |
| **DB-007** | **Stock Concurrency, Source Linking & Idempotency** |
| **DB-008** | **Customer Return, Supplier Return, Stock Count** |

## **Product Management — FR-01, FR-02**

## **DB-001 — ProductSize Lifecycle & Referential Identity \| FR-01 \| ISSUE-002/009/011 \| P0**

Problem: ProductSize đang được định danh bằng id trong product\_sizes nhưng các nghiệp vụ khác phần lớn chỉ lưu product\_id \+ size; chưa có trạng thái retired riêng. Xóa hoặc tái tạo size có thể làm mất mối liên hệ lịch sử.

Business Impact: Không truy vết chắc chắn được Variant từng bán/nhập; Cart/Order có thể tham chiếu size đã ngừng bán; dữ liệu lịch sử dễ bị xóa dây chuyền.

Current Schema: product\_sizes(id, product\_id, size, quantity, total\_imported, total\_sold, average\_cost\_per\_size); UNIQUE(product\_id,size); products.is\_active. order\_items/cart\_items/inventory\_transactions chưa có FK tới product\_sizes.id.

Example Scenario: Size 42 từng phát sinh Order được Retire dù tồn vẫn dương; khi Reactivate phải sử dụng lại chính ProductSize ID, không tạo dòng size mới.

Options & Trade-offs: (A) Gắn lifecycle cho ProductSize và FK ổn định — cần chỉnh schema nhưng bảo toàn lịch sử; (B) Xóa/tạo lại size — đơn giản tức thời nhưng phá identity/history. Business policy chọn A.

Proposed Database Change: Bổ sung product\_sizes.lifecycle\_status (active/retired, hoặc cấu trúc tương đương), có thể kèm status\_changed\_at/actor nếu cần audit; dùng product\_size\_id tại các record giao dịch mới và giữ các snapshot tên/size ở chứng từ để hiển thị lịch sử. Không cho Product Form tự thay đổi số tồn. Rà soát CASCADE trên quan hệ chứng từ.

Decision & Rationale: Giữ ProductSize ID và history theo BD-011-01/05/06, ISSUE-009 A+C; ngừng bán không đồng nghĩa xóa dữ liệu.

How to Verify: Retire/Reactivate giữ nguyên ID và stock; Order/Receipt cũ vẫn truy xuất đúng variant; hard delete bị ngăn khi có giao dịch lịch sử.

Dependencies / Status: FR-01/07/08/15/17, ISSUE-002/009/011/016. BA APPROVED: Variant có lịch sử giữ ID và không đổi size trực tiếp/hard-delete; size hợp lệ là số nguyên dương hoặc .5, chuẩn hóa 42=42.0, dải 35–45 chỉ là mặc định; mỗi Product duy nhất một size chuẩn hóa kể cả Retired, Reactivate dùng ID cũ. Schema/DB constraints vẫn PROPOSED.

## **DB-002 — Search & Filter Support \| FR-02 \| ISSUE-012 \| P2**

Problem: Catalog tăng sẽ khiến tìm theo tên/thương hiệu hoặc lọc giá/danh mục/size không đủ nhanh nếu truy vấn thiếu index.

Business Impact: Trải nghiệm tìm kiếm chậm hoặc sai thứ tự; active vs out-of-stock có thể hiển thị nhầm.

Current Schema: products có category\_id, brand, price, sale\_price, is\_active; product\_sizes có product\_id, size, quantity; hiện chưa cần bảng riêng cho Search.

Example Scenario: Lọc Nike \+ Nam \+ size 42 \+ khoảng giá, Product Active có size hết hàng vẫn hiển thị nhãn, không cho checkout size đó.

Options & Trade-offs: (A) Dùng schema sẵn có, index/query tối ưu khi cần — chi phí thấp; (B) Xây search engine/bảng index mới ngay — tăng chi phí vận hành. Đề xuất A.

Proposed Database Change: Chỉ bổ sung index phù hợp workload thực tế (ví dụ products trên is\_active/category\_id/brand và product\_sizes trên product\_id/size/quantity); không tự tạo Search schema mới. Giá lọc thực tế và quy tắc size hết hàng còn theo phê duyệt FR-02.

Decision & Rationale: Ưu tiên schema đơn giản ở giai đoạn hiện tại, nhưng kiểm chứng bằng EXPLAIN/index usage.

How to Verify: Kết quả lọc đúng chức năng, không lộ Product Inactive; phân trang ổn định và query đáp ứng performance target khi được chốt.

Dependencies / Status: FR-02, FR-15. BA APPROVED S-01/S-02/S-03 và S-BR01–04/S-AC01–07: kết hợp bộ lọc, giá bán thực tế sau ưu đãi, Active hết hàng vẫn thấy nhãn, Retired không là size có thể bán. Query/index/EXPLAIN tối ưu là Technical P2.

## **Inventory Management — FR-03 đến FR-07**

## **DB-003 — Goods Receipt Header/Lines & Import Draft \| FR-03/04/05/06/07 \| ISSUE-016 \| P1**

Problem: inventory\_transactions hiện trộn mã phiếu với từng dòng; UNIQUE(import\_code,product\_id,size) chỉ unique theo dòng, không định danh Header duy nhất. Chưa có cấu trúc Draft, file gốc, Preview đã sửa, quyền Confirm hoặc bằng chứng cảnh báo duplicate.

Business Impact: Không thể thực thi trọn workflow Upload→Validate→Preview/Edit→Confirm→Import/Audit theo All-or-Nothing; thiếu audit và idempotency.

Current Schema: inventory\_transactions(import\_code, product\_id, size, quantity, import\_price, total\_cost, type, note, user\_id, imported\_at); không có header riêng.

Example Scenario: File gồm 3 dòng cho một Import Code; Admin chỉnh Preview; mã tài liệu nguồn/file trùng chỉ cảnh báo, yêu cầu xác nhận; double Confirm không tạo phiếu thứ hai.

Options & Trade-offs: (A) Tách chứng từ Header/Lines và Draft — rõ ràng, dễ audit; (B) Tiếp tục mã phiếu lặp trên từng transaction — ít bảng hơn nhưng khó quản lý trạng thái chứng từ. Đề xuất A.

Proposed Database Change: goods\_receipts(id, import\_code UNIQUE, source\_doc\_code, confirmed\_by, confirmed\_at, status, total\_quantity, total\_amount, source\_file\_ref, final\_file\_ref, created\_at); goods\_receipt\_lines(id, receipt\_id FK, product\_size\_id FK, quantity, unit\_cost, line\_total); import\_drafts(id, created\_by, created\_at, expires\_at, status, source\_file\_ref, normalized\_data\_ref, validation\_version, confirm\_holder); có thể tách draft\_lines và import\_warning\_confirmations với actor/timestamp/duplicate evidence. Mã và bảng là design candidates.

Decision & Rationale: 19 I16-BR/24 AC v0.9 yêu cầu 1 file=1 phiếu, 4 cột, 5MB/1000 dòng, Draft 7 ngày, Confirm delegation và duplicate warning hai lớp.

How to Verify: Tối đa một Receipt/stock effect cho cùng Draft Confirm; dữ liệu phiếu xác nhận và file gốc bất biến; refresh/revalidate stale Preview; giữ bằng chứng duplicate confirmation.

Dependencies / Status: FR-01/03–07/16/17; ISSUE-003/009/011/016. Business rules Approved; permission matrix, file parser/canonicalization, storage/retention kỹ thuật Pending.

## **DB-004 — Inventory Movement Ledger, WAC & Cost Correction \| FR-06/07 \| ISSUE-011 \| P0**

Problem: Số tồn và WAC hiện thời không thay thế được lịch sử stock/cost. inventory\_transactions chỉ import/adjustment, chưa thể replay Sale, Restock, Return hoặc Cost Correction có variance.

Business Impact: Cost Correction sai, WAC chưa xác định bị nhầm với WAC=0, không đối soát được chênh lệch giữa tồn, COGS và variance.

Current Schema: product\_sizes.average\_cost\_per\_size DECIMAL(10,2) NOT NULL DEFAULT 0; inventory\_transactions.type ENUM('import','adjustment'); products.average\_cost là mức tổng hợp, không thay thế WAC từng size.

Example Scenario: Sửa giá nhập sau nhiều giao dịch: hệ thống replay giả định chuỗi movements, phân bổ delta vào giá trị tồn hiện tại và Cost Correction Variance, không sửa COGS của Sale cũ.

Options & Trade-offs: (A) Append-only ledger \+ cost adjustment document — đủ audit/replay; (B) chỉ cập nhật quantity/WAC mới nhất — ít storage nhưng không truy vết được. Đề xuất A.

Proposed Database Change: inventory\_movements(id, product\_size\_id, movement\_type, quantity\_delta, quantity\_before/after, unit\_cost\_snapshot, wac\_before/after, stock\_value\_delta, source\_type/source\_id, idempotency\_key, occurred\_at, recorded\_at, actor\_id); cost\_adjustments(id, source\_receipt\_line\_id, original\_cost, corrected\_cost, inventory\_value\_delta, cost\_correction\_variance, rounding\_residual, reason, actor\_id, created\_at); có thể bổ sung inventory\_valuations hoặc accounting entry nếu cần tách giá trị ra khỏi movements. Không ép Cost Correction giá trị-only thành giao dịch thay đổi quantity.

Proposed Data Semantics: product\_sizes WAC cần phân biệt zero hợp lệ với chưa xác định (NULL hoặc valuation\_status); WAC 2 decimal ROUND\_HALF\_UP; nếu stock trước lần Stock Import \=0 thì dùng cost của lô nhập mới; Sale thường không đổi WAC; Customer Return/Restock có rule định giá riêng.

Decision & Rationale: 16 WAC-BR/34 WAC-AC đã duyệt yêu cầu ledger, replay, variance riêng và xử lý stock-zero theo size.

How to Verify: Import/Sale/Restock/Return/Adjustment không lost movement; duplicate key không ghi hai effect; WAC=0 có chứng cứ không bị hiểu là missing; replay giữ COGS cũ; variance/residual đối soát được.

Dependencies / Status: FR-06/07/09/16/17; ISSUE-003/008/011/014/016. Policy Approved; naming/model Proposed; accounting granularity và rounding totals cần Technical/BA verification.

## **Order Management — FR-08, FR-09, FR-10**

## **DB-005 — OrderItem Variant/Cost Snapshot & Cancellation History \| FR-08/09/10 \| ISSUE-004/007/011 \| P0**

Problem: order\_items hiện lưu product\_id \+ size \+ selling price, nhưng không có ProductSize FK, COGS snapshot hay liên kết stock deduction/restock. orders chỉ giữ state hiện tại, chưa đủ dấu vết chuyển trạng thái/cancellation/refund obligations.

Business Impact: Cancellation Restock có thể tăng stock đúng lượng nhưng sai giá vốn, hoặc tăng lặp; khó kiểm chứng đã ngừng fulfillment.

Current Schema: order\_items(order\_id,product\_id,size,quantity,price,total,...); orders(status,payment\_status,cancelled\_at,cancel\_reason,...).

Example Scenario: Order xuất 4 đôi lúc WAC=100.000, sau đó nhập 4 đôi giá 150.000; hủy hợp lệ hoàn 4 đôi theo cost snapshot 100.000 và tính lại WAC.

Options & Trade-offs: (A) Snapshot hoặc liên kết bất biến với stock deduction; (B) lấy WAC hiện tại lúc hủy. Business policy đã chọn A.

Proposed Database Change: order\_items.product\_size\_id; tham chiếu inventory\_movements stock deduction với cost snapshot bất biến, hoặc cost\_snapshot\_unit/value trên OrderItem (cần chọn nguồn sự thật, tránh dữ liệu mâu thuẫn); order\_status\_history(id, order\_id, from\_status, to\_status, actor, reason, timestamp); cancellation\_cases / refund\_obligations nếu cần theo dõi independent lifecycle, stopped\_fulfillment\_evidence, restock\_status và source movement key.

Decision & Rationale: WAC-BR15, ISSUE-004/007 yêu cầu hoàn theo cost lúc xuất, đúng một lần và không đồng nhất cancel/return/refund.

How to Verify: Order/OrderItem có variant FK; mỗi stock deduction có nhiều nhất một restock cho lượng được phép; PAID cancel theo policy, giữ nghĩa vụ refund riêng.

Dependencies / Status: FR-08–10/16/17 và Payment FR-11–14. BA APPROVED: toàn đơn mới được Cancellation; PAID/PROCESSING chưa SHIPPED chỉ Admin có quyền xác nhận dừng fulfillment, lưu actor/time/reason; CANCELLATION\_IN\_PROGRESS chỉ hoàn tất khi stock/refund obligation được xử lý. Refund Completed cần chứng cứ hợp lệ. Partial Cancellation không thuộc scope; schema/fulfillment workflow kỹ thuật vẫn PENDING.

## **Payment — FR-11 đến FR-14**

## **DB-006 — Payment Request, Reconciliation, Idempotency & Refund \| FR-11/12/13/14 \| ISSUE-004/005/006/007 \| P0**

Problem: vnpay\_transactions hiện chỉ có trạng thái pending/success/failed; thiếu lớp Payment Request expiry, UNKNOWN/reconciliation, refund obligation và callback-processing history rõ ràng.

Business Impact: Timeout có thể bị xem nhầm FAILED dù tiền đã được thu; callback lặp có thể tạo nhiều effects; Cancelled Order không đồng nghĩa đã REFUNDED.

Current Schema: vnpay\_transactions(txn\_ref UNIQUE, amount DECIMAL(15,2), status ENUM('pending','success','failed'), order\_id nullable, paid\_at, response\_data,...); orders.total DECIMAL(10,2) và payment\_status varchar.

Example Scenario: Payment Request được tạo thành công lúc 10:00, deadline mục tiêu 10:05 (VN-01B, chờ VNPay Sandbox/merchant xác minh). Nếu chưa có verified final IPN, đến expiry hệ thống kích hoạt QueryDR theo sự kiện, không polling. QueryDR còn UNKNOWN/error 94 \=\> chuyển Admin ngay, không tự FAILED/Cancel/Restock; late SUCCESS xử lý idempotent/exception theo Order state.

Options & Trade-offs: (A) Payment Request \+ Attempts/Events/Reconciliation/Refund entity — nhiều bảng hơn, truy vết tốt; (B) một status field — khó thể hiện UNKNOWN và refund. Đề xuất A.

Proposed Database Change: payment\_requests(id, order\_id, txn\_ref UNIQUE, requested\_amount, request\_created\_at, expires\_at, state); payment\_events/attempts(id, payment\_request\_id, gateway\_ref, result\_code, verified, payload\_ref, received\_at, idempotency\_key); payment\_reconciliations(id, payment\_request\_id, attempted\_at, result, next\_action, actor); refunds(id, order\_id, payment\_request\_id, amount, obligation\_status, provider\_status, provider\_ref, resolved\_at) và refund\_attempts/events nếu cần. Chỉ mở rộng vnpay\_transactions hoặc tách mới sau Technical Design.

Proposed Value Changes: chuẩn hóa precision các amount/order totals đủ lớn (ví dụ DECIMAL(18,2), phải kiểm tra VNPay VND unit/scale); PaymentRequest gắn chính xác Order, amount và identifiers; không khẳng định API VNPay refund đã hỗ trợ.

Decision & Rationale: BA APPROVED ISSUE-004/007 và VN-01B/VN-02A/VN-03A/VN-04A: expiry mục tiêu 5 phút từ Payment Request tạo thành công; QueryDR theo sự kiện khi đến hạn mà chưa có kết quả cuối, không polling; UNKNOWN chuyển Admin ngay; mã 94 là lỗi duplicate QueryDR, không phải Payment FAILED. Restock chỉ khi an toàn payment; Refund FAILED/UNKNOWN giữ nghĩa vụ; late SUCCESS xử lý ngoại lệ. 5 phút chưa verified với VNPay Sandbox.

How to Verify: Retry/callback trùng không tạo double-PAID; UNKNOWN không tự FAILED; Order CANCELLED \+ Refund Pending lưu được hai trạng thái độc lập; reconciliation history audit.

Dependencies / Status: FR-09/10/11–14/17. Business Payment/Refund/Reconciliation policies APPROVED; technical P0 PENDING: VNPay chấp nhận vnp\_ExpireDate=+5 phút, event trigger đáng tin cậy, QueryDR signature/limits/code94, IPN idempotency, Refund provider/manual evidence, merchant capability; partial refund chưa duyệt scope.

## **Order & Inventory Consistency — FR-15, FR-16, FR-17**

## **DB-007 — Stock Concurrency, Source Linking & Idempotency \| FR-15/16/17 \| ISSUE-003/004/007/011 \| P0**

Problem: Đồng thời nhiều checkout/import/restock/return/adjustment có thể cập nhật quantity/WAC dựa trên số đọc cũ.

Business Impact: Overselling, lost update, double restock, trạng thái Order khác Inventory.

Current Schema: product\_sizes.quantity là counter; hiện chưa có ledger/outbound identity để theo dõi hiệu ứng mỗi action. Condition UPDATE ứng dụng có thể hỗ trợ nhưng không đủ thay thế transaction nhiều bảng.

Example Scenario: Size 42 còn 1 đôi, hai khách Checkout đồng thời; tối đa 1 Order được xác nhận và stock không âm.

Options & Trade-offs: (A) Ràng buộc DB \+ transaction/locking/versioning \+ idempotency; (B) chỉ kiểm tra ở UI/Preview. Policy chọn A.

Proposed Database Change: UNIQUE(idempotency\_key) hoặc source\_effect unique trên ledger; FK chứng từ ↔ movement; CHECK(quantity\>=0) và positive transaction quantity; optional row\_version phù hợp optimistic concurrency; FK Variant và required timestamps. Atomic Order/Inventory tại commit phải được bảo đảm trong Application & DB Transaction.

Decision & Rationale: Bảo đảm stock/current valuation, Order và chứng từ thay đổi nhất quán; không để duplicate request làm business effect lặp.

How to Verify: Test cùng ProductSize đồng thời trên MariaDB mục tiêu; không negative stock, stale adjustment reject, double Confirm/Restock không nhân đôi movement.

Dependencies / Status: FR-15/16/17/18/19, ISSUE-003/008/011/014/016. BA principle Approved; locking pattern/chỉ số version Technical Review.

## **Các chức năng mở rộng chưa có FR ID riêng**

## **DB-008 — Customer Return, Supplier Return, Stock Count \| Candidate FR \| ISSUE-011 \| P1 / Pending**

Problem: WAC policy đã chốt quy tắc giá vốn Customer Return/Supplier Return/Stock Count nhưng chưa có chứng từ nghiệp vụ end-to-end được duyệt cho toàn bộ Return Management.

Business Impact: Không thể triển khai đúng kiểm hàng trước khi bán lại, supplier credit variance hoặc inventory counting có audit nếu chỉ tăng/giảm số tồn.

Current Schema: inventory\_transactions chỉ import/adjustment, không có riêng Receipt/Return/Count entities.

Example Scenario: Customer Return phải qua quality check trước khi tăng saleable stock; Supplier Return giảm tồn theo WAC hiện tại và lưu variance; Stock Count nhập target quantity chứ không nhập delta tùy ý.

Options & Trade-offs: (A) Evidence/transaction riêng từng nghiệp vụ — rõ workflow; (B) dùng note trong InventoryTransaction — khó enforce. Đề xuất A khi scope được duyệt.

Proposed Database Change: customer\_returns / return\_lines với OrderItem/source sale movement, inspection\_status và cost source; supplier\_returns / return\_lines với credit\_document, supplier\_variance, cost snapshot; stock\_counts / stock\_count\_lines với target, observed, delta, reason, actor, before/after và stale check. Có thể chuẩn hóa chứng từ tổng quát nhưng không gộp các trạng thái nghiệp vụ khác nhau thành một enum thiếu nghĩa.

Decision & Rationale: WAC-BR09–14/AC16–28 chỉ phê duyệt costing/điều kiện cập nhật stock; full returns workflow là Candidate.

How to Verify: Return chưa đạt kiểm hàng không tăng saleable stock; không tự thay COGS thiếu bằng WAC hiện tại; supplier variance có dấu; count stale reject.

Dependencies / Status: FR-06/07/09/16/17, ISSUE-011; RETURN eligibility/partial/damaged/refund, Supplier Credit pending chưa phê duyệt.

# **4\. Data Model & Integrity Constraints**

## **Danh sách**

| Constraint ID | Constraint / Integrity Rule |
| :---- | :---- |
| **C-01** | **Referential integrity lịch sử: Không dùng ON DELETE CASCADE để xóa Order, OrderItem, Goods Receipt hoặc Inventory Movement chỉ vì User/Product/Variant bị xóa/ẩn. Đề xuất RESTRICT/NO ACTION hoặc thiết kế lưu dấu vết phù hợp. CASCADE có thể hợp lệ ở dữ liệu phụ thuộc tạm thời như CartItem, nhưng phải thẩm định từng quan hệ.** |
| **C-02** | **Variant unique & identity: Theo V-BR03/04 phải unique trên (product\_id, normalized\_size), kể cả Retired; normalize size nguyên/.5 trước khi so trùng. ProductSize ID bất biến khi retire/reactivate. Variant có lịch sử không được đổi size trực tiếp; nếu thay size phải Retire và tạo ID mới stock=0. Biểu diễn và index cụ thể chờ Technical Design.** |
| **C-03** | **Nonnegative stock & quantities: CHECK(product\_sizes.quantity\>=0), stock absolute/count không âm, sale/import quantity hợp lệ theo loại chứng từ. Return/stock movement dùng signed delta có rule riêng.** |
| **C-04** | **Value validity: unit\_cost/import\_price/amount phải không âm, đủ DECIMAL precision; WAC=0 có thể hợp lệ khi có chứng từ, NULL/invalid phải phân biệt; không tự tính 0 khi thiếu giá vốn.** |
| **C-05** | **Receipt identity: goods\_receipts.import\_code UNIQUE toàn hệ thống; goods\_receipt\_lines không trùng Variant trong một phiếu khi rule ISSUE-016 yêu cầu; cùng một Draft Confirm không phát sinh hai Receipt/Movement.** |
| **C-06** | **Inventory idempotency: Source effect của Checkout Deduction/Restock/Return/Import chỉ ghi một lần theo business key ổn định; index UNIQUE cần phản ánh 'cùng effect', không chặn các giao dịch khác có cùng SKU/Size.** |
| **C-07** | **Audit and immutability: Phiếu nhập Confirm và các movement lịch sử không ghi đè/xóa; Cost Correction là chứng từ mới tham chiếu nguồn. Actor/time/reason ghi theo nghiệp vụ; thiếu audit bắt buộc thì giao dịch không hoàn tất.** |
| **C-08** | **Order lifecycle: Order state và refund/payment state có phạm vi hợp lệ, không tự suy diễn từ một state field; transition validation nằm ở Application, DB lưu đủ timestamps/events để audit.** |
| **C-09** | **Payment identity: txn\_ref UNIQUE cho từng Payment Request, liên kết đúng Order/amount, giữ nhiều event/attempt khi cần; gateway callback duplicate không tạo nhiều effects; không đồng nhất Order CANCELLED với Refund hoàn tất.** |
| **C-10** | **Monetary precision: orders.total hiện DECIMAL(10,2) còn vnpay\_transactions.amount DECIMAL(15,2); đề xuất chuẩn hóa đủ phạm vi tiền hợp lệ, ví dụ DECIMAL(18,2), với conversion VNPay VND và ROUND\_HALF\_UP policy được kiểm tra riêng.** |
| **C-11** | **Concurrency consistency: FK/unique/check là tuyến bảo vệ dữ liệu; mọi flow Import/Checkout/Adjustment/Restock phải cập nhật source document, movement, quantity và cost trong transaction nhất quán; chống stale reads.** |
| **C-12** | **Search indexes: Index cho trạng thái, danh mục, brand, price và size chỉ bổ sung theo query workload/EXPLAIN; không sử dụng index thay thế validation Product/Variant lifecycle.** |
| **C-13** | **Return traceability: Customer Return trỏ tới OrderItem/sale movement gốc; Supplier Return trỏ chứng từ xuất và supplier credit; Return qty không vượt lượng hợp lệ còn có thể trả; không double-restock với Cancellation.** |
| **C-14** | **Permissions & data protection: Chỉ Admin được ủy quyền Confirm/Adjust/Cost Correct; retention riêng cho file gốc, file cuối và audit; khóa truy cập bằng quyền ứng dụng và storage riêng tư, không lộ payment secrets.** |

##  **ràng buộc cần rà soát**

C-01 \| Referential integrity lịch sử: Không dùng ON DELETE CASCADE để xóa Order, OrderItem, Goods Receipt hoặc Inventory Movement chỉ vì User/Product/Variant bị xóa/ẩn. Đề xuất RESTRICT/NO ACTION hoặc thiết kế lưu dấu vết phù hợp. CASCADE có thể hợp lệ ở dữ liệu phụ thuộc tạm thời như CartItem, nhưng phải thẩm định từng quan hệ.

C-02 \| Variant unique & identity: Theo V-BR03/04 phải unique trên (product\_id, normalized\_size), kể cả Retired; normalize size nguyên/.5 trước khi so trùng. ProductSize ID bất biến khi retire/reactivate. Variant có lịch sử không được đổi size trực tiếp; nếu thay size phải Retire và tạo ID mới stock=0. Biểu diễn và index cụ thể chờ Technical Design.

C-03 \| Nonnegative stock & quantities: CHECK(product\_sizes.quantity\>=0), stock absolute/count không âm, sale/import quantity hợp lệ theo loại chứng từ. Return/stock movement dùng signed delta có rule riêng.

C-04 \| Value validity: unit\_cost/import\_price/amount phải không âm, đủ DECIMAL precision; WAC=0 có thể hợp lệ khi có chứng từ, NULL/invalid phải phân biệt; không tự tính 0 khi thiếu giá vốn.

C-05 \| Receipt identity: goods\_receipts.import\_code UNIQUE toàn hệ thống; goods\_receipt\_lines không trùng Variant trong một phiếu khi rule ISSUE-016 yêu cầu; cùng một Draft Confirm không phát sinh hai Receipt/Movement.

C-06 \| Inventory idempotency: Source effect của Checkout Deduction/Restock/Return/Import chỉ ghi một lần theo business key ổn định; index UNIQUE cần phản ánh 'cùng effect', không chặn các giao dịch khác có cùng SKU/Size.

C-07 \| Audit and immutability: Phiếu nhập Confirm và các movement lịch sử không ghi đè/xóa; Cost Correction là chứng từ mới tham chiếu nguồn. Actor/time/reason ghi theo nghiệp vụ; thiếu audit bắt buộc thì giao dịch không hoàn tất.

C-08 \| Order lifecycle: Order state và refund/payment state có phạm vi hợp lệ, không tự suy diễn từ một state field; transition validation nằm ở Application, DB lưu đủ timestamps/events để audit.

C-09 \| Payment identity: txn\_ref UNIQUE cho từng Payment Request, liên kết đúng Order/amount, giữ nhiều event/attempt khi cần; gateway callback duplicate không tạo nhiều effects; không đồng nhất Order CANCELLED với Refund hoàn tất.

C-10 \| Monetary precision: orders.total hiện DECIMAL(10,2) còn vnpay\_transactions.amount DECIMAL(15,2); đề xuất chuẩn hóa đủ phạm vi tiền hợp lệ, ví dụ DECIMAL(18,2), với conversion VNPay VND và ROUND\_HALF\_UP policy được kiểm tra riêng.

C-11 \| Concurrency consistency: FK/unique/check là tuyến bảo vệ dữ liệu; mọi flow Import/Checkout/Adjustment/Restock phải cập nhật source document, movement, quantity và cost trong transaction nhất quán; chống stale reads.

C-12 \| Search indexes: Index cho trạng thái, danh mục, brand, price và size chỉ bổ sung theo query workload/EXPLAIN; không sử dụng index thay thế validation Product/Variant lifecycle.

C-13 \| Return traceability: Customer Return trỏ tới OrderItem/sale movement gốc; Supplier Return trỏ chứng từ xuất và supplier credit; Return qty không vượt lượng hợp lệ còn có thể trả; không double-restock với Cancellation.

C-14 \| Permissions & data protection: Chỉ Admin được ủy quyền Confirm/Adjust/Cost Correct; retention riêng cho file gốc, file cuối và audit; khóa truy cập bằng quyền ứng dụng và storage riêng tư, không lộ payment secrets.

## **Nguyên tắc triển khai ràng buộc**

Không tự suy ra rằng mọi constraint vừa đề xuất đã tồn tại trong DB hoặc Laravel. Database constraints chỉ bảo vệ bất biến dữ liệu, không thay thế authorization, validation, state-machine và business transaction.

Vì không quan tâm dữ liệu cũ trong bản dump, DBCS không đưa yêu cầu backfill hoặc sửa từng record cũ. Nếu triển khai lên một môi trường có dữ liệu thật cần giữ, phải lập kế hoạch riêng được phê duyệt trước khi drop/alter/restore; không sử dụng migrate:fresh lên môi trường cần bảo toàn dữ liệu.

Mọi tên bảng/cột/kiểu dữ liệu mới trong mục 2–3 đều là schema design đề xuất, có thể thay bằng thiết kế tương đương nếu bảo đảm các FR/BR/AC. Không tạo prompt Codex, migration, SQL DDL hoặc sửa các tài liệu BA khác ở bước này.

# **5\. Migration, Deployment & Verification**

Các mục dưới đây giữ nội dung được phê duyệt và các gate kỹ thuật trong DBCS hiện hành; mọi migration/schema/hosting detail vẫn được đánh dấu Proposed hoặc Technical Pending.

CHECK nonnegative quantity khi saleable stock; transaction \+ row locks/optimistic version phù hợp trên DB engine thực tế; consistent FK ProductSize; UNIQUE business effect/idempotency per Receipt Confirm, Order Deduction, Restock, Payment verification; immutable movement history. Monetary DECIMAL precision và WAC rounding không được sử dụng FLOAT. Không hủy Order hoặc hoàn kho từ payment UNKNOWN.

DBCS không khẳng định SQL dump cũ phải backfill. Nếu về sau có production data thật, recovery/migration phải bảo toàn tất cả Order, Payment, Receipt và Movements mới phát sinh.

## **FR-18/FR-19 — Testing, Vercel & Deployment Dependencies**

Approved FR-18: P0 automated tests theo AC/risk phải PASS trước release; P1 ảnh hưởng chức năng thay đổi/an toàn dữ liệu phải sửa; P2 chỉ được ngoại lệ với risk assessment và phê duyệt. Unit \+ Feature/Integration \+ concurrency, E2E smoke; CI mock IPN/QueryDR/Refund, Sandbox thực trước release Payment đầu tiên và mỗi thay đổi Payment workflow; isolated test DB/seed/factory; test evidence theo Test Case ↔ AC ↔ FR ↔ commit/release.

Approved FR-19: CI \+ manual deployment runbook; Local/CI Test/Vercel Preview-Staging/Production khi có. Pin release tag/commit SHA và dependency lockfile; separate environment configs/secrets; migration tương thích theo bước, backup và isolated restore test trước thay đổi có rủi ro; không restore DB làm mất giao dịch mới; smoke/health/log/migration verification trước Successful.

Vercel D-09: đã chọn Vercel cho web deployment; Laravel/PHP full-stack trên community runtime CHƯA xác minh khả thi. Database giao dịch và persistent storage phải độc lập khỏi Vercel Function ephemeral filesystem. Import original/confirmed files, history và audit phải được giữ bền vững với private access. Payment expiry event trigger trong khoảng 5 phút không được phụ thuộc lịch cron không đáp ứng timing.

Technical feasibility gates VG-01 PHP/Laravel runtime/Composer/routes; VG-02 external DB/transactions/connections; VG-03 persistent private object storage và evidence; VG-04 public VNPay IPN, expiry event, QueryDR/Refund; VG-05 background jobs/queues; VG-06 CI/manual promotion/preview/rollback; VG-07 plan/usage eligibility và giới hạn gói dịch vụ.

Architecture pending: cần kiểm thử toàn bộ Laravel trên Vercel; không tự phê duyệt tách frontend/backend, không áp đặt nhà cung cấp DB, object storage hay queue khi chưa có quyết định.

# **6\. Traceability & Pending Technical Decisions**

DB-001→FR-01 V-BR/AC; DB-002→FR-02 S-BR/AC; DB-003→FR-03–07 I16-BR01–19/016-AC01–24 v0.9; DB-004→WAC-BR01–16/WAC-AC01–34; DB-005→FR-08–10 C/O-BR/AC, WAC-AC29–31; DB-006→FR-11–14 PY/PV/PI/PF BR/AC và VN-01–04; DB-007→FR-15–17 ST-BR/AC, WAC-AC32–34; FR-18→BR-19/T18-BR01–07/T18-AC01–13; FR-19→BR-20/D19-BR01–07/D19-AC01–13.

Status: BA policies Approved; schema entities, constraints, migrations, code, VNPay Sandbox, Vercel runtime, test evidence và production deployment vẫn Pending Technical Verification. Full Returns workflow và 016-AC24 wording BA Pending. Không tạo hoặc chạy database migration/SQL, không sửa code, GitHub hoặc các tab ngoài DBCS trong bước này.

## **6.1 Approved Baseline v1.2 — Clarifications**

Trạng thái: BUSINESS POLICIES APPROVED / SCHEMA DESIGN PROPOSED / TECHNICAL VERIFICATION PENDING. Các mục DB-001–DB-008 và C-01–C-14 là phương án thiết kế, không phải migration đã phê duyệt hoặc đã triển khai. Phần 4 có hiệu lực cập nhật các phát biểu nghiệp vụ đã lỗi thời trong phần 1–3.

## **DB-001/002 — Variant, Search, Price**

Approved: Variant có chứng từ lâu dài (OrderItem, Receipt, Movement, Adjustment, Return/Cost Correction) không được hard-delete hoặc sửa size trực tiếp. Size nguyên dương hoặc bước 0,5; 42 và 42.0 là một size; unique per Product kể cả Retired. Reactivate giữ nguyên Variant ID, tồn/WAC/lịch sử. Product Inactive và Variant Retired chặn Checkout/Import mới nhưng không hủy Order đã tạo hợp lệ. Catalog chỉ hiển thị Product Active; lọc giá theo selling price sau ưu đãi; Active hết hàng còn hiển thị nhãn.

Design pending: normalized\_size representation và unique index, lifecycle\_status, FK snapshot, price-effective expression, indexes theo truy vấn thực tế. Không tự mở rộng full-text search engine.

## **DB-003/004 — Import, Ledger, WAC**

Approved: Import một file .csv/.xlsx \= một Goods Receipt, 4 cột SKU/Size/Quantity/Import Price; 5 MB và 1.000 dòng, Draft 7 ngày không gia hạn. Preview chỉnh sửa/validate lại; trùng SKU+size trong cùng phiếu phải chặn; trùng raw file hoặc dữ liệu nghiệp vụ cuối đã Confirm chỉ cảnh báo và yêu cầu Admin xác nhận, lưu evidence. Delegation chỉ chuyển quyền Confirm có reason/audit, quyền Edit vẫn thuộc creator. Confirm idempotent, atomic với Receipt, stock, WAC và movement.

Approved WAC: WAC theo ProductSize, 2 số thập phân ROUND\_HALF\_UP; 0 hợp lệ có chứng cứ khác NULL/missing. Zero stock và inbound mới đặt WAC theo cost mới. Sale không thay WAC; cost correction/replay tạo variance/residual, không hồi tố COGS đã ghi; cancellation restock tham chiếu original deduction cost snapshot đúng một lần. Stock Count, Customer/Supplier Return có costing policy nhưng full Return Management chưa Approved.

Business pending: 016-AC24 hiện vẫn dùng câu chữ Inventory Adjustment cho sai sót sau Confirm; đề xuất phân biệt qty adjustment và cost correction chưa được duyệt, không áp dụng ngầm. Xử lý nhầm Variant sau Confirm chưa có workflow phê duyệt.

Design pending: Goods Receipt/Draft/Line, normalized duplicate comparison, receipt source audit, persistent original/final file refs, append-only inventory movements, WAC cost replay precision và DB locking/unique idempotency.

## **DB-005 — Order State, Cancellation, Refund Obligation**

Approved Order State: CREATED → PAYMENT\_PENDING → PAID → PROCESSING → SHIPPED → DELIVERED; cancellation CREATED/PAYMENT\_PENDING chỉ khi payment an toàn; PAID/PROCESSING trước SHIPPED chỉ Admin có thẩm quyền chuyển CANCELLATION\_IN\_PROGRESS sau stop-fulfillment evidence; chỉ hoàn tất CANCELLED khi stock/restock xử lý đúng và nghĩa vụ Refund đã giải quyết. Không partial cancel. Customer không tự cancel PAID. Order status, payment status và refund status cần bảo toàn độc lập về ngữ nghĩa.

Approved: Order và stock deduction trong một atomic commit trước redirect VNPay; không tạo Order từ Return URL. OrderItem lưu ProductSize reference và COGS/sale snapshot hoặc tham chiếu immutable source movement. Cancellation restock theo cost snapshot gốc tối đa một lần; refund chờ xử lý không cản việc restock đã đủ điều kiện nhưng vẫn cản Cancellation Completed.

Design pending: order\_status\_events/cancellation workflow, payment/refund link, actor/time/reason/stopped\_fulfillment\_evidence, restock source effect uniqueness, event state mapping; không bắt buộc biểu diễn CANCELLATION\_IN\_PROGRESS bằng riêng orders.status nếu workflow entity tương đương.

## **DB-006 — Payment Request, IPN, QueryDR, Refund**

VN-01B Approved/Technical Pending: request\_created\_at và expires\_at với mục tiêu \+5 phút kể từ Payment Request tạo thành công, truyền VNPay vnp\_ExpireDate GMT+7; chưa xác minh môi trường Sandbox/merchant chấp nhận 5 phút. Không tự fallback về 15 phút.

VN-02A Approved: ưu tiên IPN xác minh; tại expiry chưa có verified final result thì trigger QueryDR một lần theo sự kiện, không polling theo chu kỳ 7–15–20. Nếu verified final result đã có thì không gửi truy vấn không cần thiết.

VN-03A/VN-04A Approved: QueryDR không xác minh được \=\> UNKNOWN/reconciliation exception và chuyển Admin ngay; không auto-FAILED, auto-Cancel, auto-Restock hay tự retry theo lịch. QueryDR response code 94 liên quan duplicate request/API window, không chứng minh Payment FAILED. QueryDR response status phải phân biệt transaction status.

Data design candidates: payment\_requests(order\_id,txn\_ref,amount,created\_at,expires\_at,state); verified payment\_events/idempotency identity; reconciliation attempts/event reason, trigger source/time/result/admin assignee; refund obligations, provider/manual evidence, amount, reference, verified\_at, actor; late IPN exceptions. Không lưu payment secrets hoặc raw sensitive payment data không cần thiết.

Required verification: exact VNPay timestamp/transaction identity, chữ ký của PAY/IPN/QueryDR/Refund, response\_code vs transaction\_status, merchant acceptance expiry 5 phút, API 94 frequency, concurrency/late success, Refund Completed evidence. Return URL chỉ phục vụ UI và không là nguồn xác nhận duy nhất.

BẢNG THAM CHIẾU NHANH — DBCS

| DB ID | Database Change |
| :---- | :---- |
| DB-001 | ProductSize Lifecycle & Referential Identity |
| DB-002 | Search & Filter Support |
| DB-003 | Goods Receipt Header/Lines & Import Draft |
| DB-004 | Inventory Movement Ledger, WAC & Cost Correction |
| DB-005 | OrderItem Variant/Cost Snapshot & Cancellation History |
| DB-006 | Payment Request, Reconciliation, Idempotency & Refund |
| DB-007 | Stock Concurrency, Source Linking & Idempotency |
| DB-008 | Customer Return, Supplier Return, Stock Count |

