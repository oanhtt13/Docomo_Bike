SRS Documentation
======================

Introduction
--------------

Phát triển hệ thống bao gồm AI Unit và Ứng dụng Android cho phép phát hiện vị trí xe đạp đang di chuyển, từ đó đứa ra cảnh báo người dùng, từ đó giúp nâng cao ý thức người sử dụng xe đạp.

Hệ thống bao gồm:

* Ứng dụng được phát triển trên thiết bị do khách hàng cung cấp.
* Ứng dụng điện thoại Android.

Scope
------

In-Scope
^^^^^^^^^^^^^^
Phát triển ứng dụng trên thiết bị là điện thoại, cho phép phát cảnh báo tới người dùng khi phát hiện người dùng đang di chuyển trên vỉa hè.
Phase 2 tập trung phát triển ứng dụng Android và tích hợp với AI Unit đã có.

Các tính năng chính:

* Xác định vị trí xe đang di chuyển, phát cảnh báo đi chậm.
* Kiểm tra connect giữa thiết bị và điện thoại
* Gửi ảnh từ AI unit sang ứng dụng điện thoại.
* Xóa thông tin nhạy cảm trong ảnh.
* Mã hóa bản tin gửi/nhận giữa ứng dụng điện thoại và AI Unit.

Out-of-Scope
^^^^^^^^^^^^^^^^

* Không hỗ trợ ứng dụng Ios.
* Không hỗ trợ lunching app lên App Store.
* Ứng dụng không hỗ trợ chạy nền.
* Không optimize model AI đã phát triển tại phase 1.

Overall Description
---------------------

Contraints
^^^^^^^^^^^^^^

* Phát triển trên thiết bị phần cứng thiết bị do khách hàng cung cấp.
* Model phát triển phải convert sang định dạng RKNN để có thể implement được lên thiết bị phần cứng.

Security

* Phát triển tính năng xác thực ứng dụng: chỉ ứng dụng do HBLAB phát triển mới có thể nhận được bản tin từ AI Unit.
* Mã hóa thông tin gửi từ AI Unit sang ứng dụng, từ đó cho phép chỉ ứng dụng mới có thể đọc được thông tin thiết bị gửi về.

Functional Requirements
-----------------------------

FR-DA: Device Application
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. list-table:: **FR-DA**
   :widths: 15 10
   :header-rows: 1

   * - Content
     - Detail
   * - Description
     - Phát triển ứng dụng trên thiết bị phần cứng do khách hàng cung cấp.
   * - Input
     - Ảnh do camera thu được.
   * - Output
     - Log ứng dụng.

       Bản tin gửi cho ứng dụng điện thoại.
   * - Preconditions
     - Thiết bị được cấp nguồn đầy đủ.
       
       Bộ nhớ trong chưa đầy.

       Thiết bị được liên kết với camera.
   * - Postconditions
     - Lưu log chương trình.

.. list-table:: **Business Flow**
   :widths: 15 30 30
   :header-rows: 1

   * - Step
     - Desciption
     - Business Logic Acceptance Criteria
   * - 1. Khởi động chương trình
     - Cấp nguồn cho thiết bị.
     - Thiết bị tự động chạy ngay khi được cấp nguồn.
   * - 2. Connect AI Unit với ứng dụng điện thoại
     - Kết nối cổng USB type C của AI Unit với ứng dụng điện thoại.
     - Kết nối thành công
   * - 3. Config Ai Unit thông qua ứng dụng điện thoại
     - Mở giao diện ứng dụng điện thoại

       Mở giao diện setup AI Unit
     - Giao diện cho phép setup AI Unit được mở ra. Cho phép setup tối thiểu:

       Mode hoạt động của AI Unit (running/stop).

       Thời giữa 2 lần chụp ảnh liên tiếp (intervals).
   * - 4. Gửi ảnh cho ứng dụng điện thoại
     - Ảnh sau khi được thu thập sẽ được gửi cho thiết bị
     - Gửi ảnh overlay, ảnh raw.

       Ảnh sau khi được gửi thành công, tiến hành hóa phía thiết bị phần cứng.

       Lưu log

.. list-table:: **External Flow**
   :widths: 15 30 30
   :header-rows: 1

   * - Step
     - Desciption
     - Business Logic Acceptance Criteria
   * - 1. Update file config
     - Ứng dụng cho phép update file config bằng PC.

     - File config cho phép điều chỉnh thông số của ứng dụng trên thiết bị phần cứng. Tham khảo phụ lục 1.

       Ứng dụng khởi động khi được cấp nguồn, hoạt động với chế độ phát hiện vị trí xe đạp.

       Thời gian giữa 2 lần chụp ảnh liên tiếp: 0.5s.

       Hoạt động với đúng model AI, lưu ảnh vào thư mục yêu cầu.

       File config có thể được update từ PC thông qua ADB Interface.


FR-LW: Location Warning
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

FR-LW-1: Location Detection
*******************************

.. list-table:: **FR-LW-1**
   :widths: 15 10
   :header-rows: 1

   * - Content
     - Detail
   * - Description
     - Tính năng cho phép sử dụng Camera cho phép thu thập hình ảnh trong quá trình User sử dụng xe đạp. Sau đó sử dụng model AI phân tích hình ảnh thu được, từ đó cho phép phát hiện vị trí xe đạp đang di chuyển (vỉa hè/lòng đường).
   * - Input
     - Ảnh thu thập từ Camera.
   * - Output
     - AI unit trả về thông tin vị trí xe đang di chuyển: vỉa hè/lòng đường.
   * - Preconditions
     - Thiết bị được cấp nguồn đầy đủ.
   * - Postconditions
       
       Lưu log chương trình.
       
       Thông tin ảnh overlay.
       
       Thông tin ảnh raw.

.. list-table:: **Business Flow**
   :widths: 15 30 30
   :header-rows: 1

   * - Step
     - Desciption
     - Business Logic Acceptance Criteria
   * - 1. Phát hiện vị trí xe đạp
     - Dựa trên hình ảnh thu được, đưa ra thông tin vị trí xe đạp đang di chuyển (làn đường/vỉa hè).
     - Ứng dụng cho phép phát hiện khu vực vỉa hè, lòng đường. Từ đó xác định được vị trí xe đạp đang di chuyển.

FR-LW-2: Warning Notification
********************************

.. list-table:: **FR-LW-2**
   :widths: 15 10
   :header-rows: 1

   * - Content
     - Detail
   * - Description
     - Tính năng cho phép AI unit gửi thông tin vị trí xe đạp về ứng dụng điện thoại thông qua ADB interface. Ứng dụng điện thoại sẽ phát cảnh báo sau khi phát hiện xe đang di chuyển trên vỉa hè (Y) lần liên tiếp.
   * - Input
     - Thông tin vị trí xe đạp được lấy từ FR-LW-1.
   * - Output
     - Ứng dụng phát âm báo "Xin hãy di chuyển chậm lại" bằng tiếng Nhật.
   * - Preconditions
     - Thiết bị được cấp nguồn đầy đủ.
       
       Thiết bị phần cứng và ứng dụng điện thoại phải được kết nối.
       
       Ứng dụng điện thoại được mở sẵn.
   * - Postconditions
       
       Lưu log tại ứng dụng phần cứng, ứng dụng điện thoại.

       Phát âm thông báo thành công.

.. list-table:: **Business Flow**
   :widths: 15 30 30
   :header-rows: 1

   * - Step
     - Desciption
     - Business Logic Acceptance Criteria
   * - 1. Device gửi thông báo cho điện thoại
     - Sau khi phát hiện vị trí xe đạp di chuyển, ứng dụng gửi thông báo cho điện thoại thông qua abd.
     - Thiết bị, điện thoại gửi nhận thông tin thành công. Số lần gửi/nhận phải khớp về số lượng, nội dung.

       Lưu log gửi/nhận.
   * - 2. Thiết bị phát thông báo
     - **Nếu:**

       Điện thoại nhận được thông tin xe đạp đang di chuyển trên vỉa hè.

       **Thì:**

       Điện thoại phát âm báo: "Xin hãy di chuyển chậm lại"
     - Ứng dụng phát thông báo thành công

       Lưu log

.. list-table:: **External Flow**
   :widths: 15 30 30
   :header-rows: 1

   * - Step
     - Desciption
     - Business Logic Acceptance Criteria
   * - 1. Update config cho phát cảnh báo
     - Mở giao diện setup ứng dụng
     - Mở giao diện thành công
   * - 2. Update thông tin ``số lần liên tiếp sẽ phát cảnh báo``
     - Điền thông tin số lần phát hiện xe đang đi trên vỉa hè liên tiếp để ứng dụng phát cảnh báo.
     - Thời gian giữa 2 lần phát thông báo sẽ là: ``thời gian giữa 2 lần thông báo gần nhất`` x ``số lần phát hiện liên tiếp``
   * - 3. Update thông tin ``thời gian giữa hai lần phát thông báo liên tiếp``
     - Điền thông tin thời gian giữa 2 lần phát thông báo liên tiếp
     - Chỉ cho phép điền khoảng thời gian này là bội số của (``thời gian giữa 2 lần thông báo gần nhất`` x ``số lần phát hiện liên tiếp``). Nếu không sẽ báo lỗi.


FR-CL: Clear Sensitive Infomation
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

FR-CL-1: Image Handling
***********************

.. list-table:: **FR-CL-1**
   :widths: 15 10
   :header-rows: 1

   * - Content
     - Detail
   * - Description
     - AI Unit sẽ chụp ảnh và gửi ảnh sang ứng dụng Android thông qua ADB interface.
     - Ứng dụng Android sẽ thực hiện làm mờ khuôn mặt trước khi lưu trữ 
   * - Input
     - Ảnh thu thập từ camera thiết bị.
   * - Output
     - Ứng dụng Android thực hiện làm mờ khuôn mặt trước khi lưu trữ ảnh.
     -   Xóa toàn bộ ảnh trên AI Unit sau khi gửi thành công sang Ứng dụng Android

   * - Trigger
     - Ngay sau khi thiết bị gửi ảnh cho ứng dụng.
   * - Preconditions
     - Thiết bị được cấp nguồn đầy đủ.
       
       Thiết bị hỗ trợ dây USB Type C, cho phép lấy thông tin qua ADB Interface.
     
     (Nếu muốn hỗ trợ cổng khác, thiết bị cũng cần phải hỗ trợ).
   * - Postconditions
     - Lưu log chương trình.

       Ảnh đã được làm mờ lưu vào bộ nhớ.

       Xóa ảnh phía thiết bị phần cứng.

.. list-table:: **Business Flow**
   :widths: 15 30 30
   :header-rows: 1

   * - Step
     - Desciption
     - Business Logic Acceptance Criteria
   * - 1. Gửi ảnh sau khi đã xử lý AI
     - Ảnh sau khi xử lý AI (phân biệt lòng đường hay vỉa hè) được tự dộng gửi sang ứng dụng Android
     - Tất cả các ảnh (ảnh raw, ảnh overlay sau xử lý)

       Quá trình gửi ảnh phải được mã hóa, chỉ ứng dụng HBLab tự phát triển mới có thể đọc được ảnh.
   * - Điện thoại nhận ảnh
     - Ứng dụng điện thoại tiếp nhập ảnh, lưu vào kho lưu trữ 
     - Lưu ảnh thành công

       Gửi thông báo về cho ứng dụng
   * - Thiết bị xóa ảnh
     - Xóa toàn bộ ảnh trong bộ nhớ
     - Xóa ảnh thành công.

       Lưu thông tin vào log.


FR-CL-2: Clear Sensitive Info
***********************************

.. list-table:: **FR-CL-1**
   :widths: 15 10
   :header-rows: 1

   * - Content
     - Detail
   * - Description
     - Cho phép ứng dụng điện thoại xóa/làm mờ những thông tin nhạy cảm khỏi ảnh (mặt người đi đường) để tránh vi phạm luật tại Nhật.
   * - Input
     - Ảnh nhận được từ thiết bị AI Unit đã được gửi cho ứng dụng điện thoại.
   * - Output
     - Ảnh chứa khuôn mặt người đi đường đã được làm mờ trên ứng dụng điện thoại
   * - Trigger
     - Ngay sau khi ứng dụng điện thoại nhận được ảnh từ device AI Unit
   * - Preconditions
     - Nhận được ảnh từ thiết bị.
   * - Postconditions
     - Lưu log chương trình.

       Ảnh đã được làm mờ lưu vào bộ nhớ.

.. list-table:: **Business Flow**
   :widths: 15 30 30
   :header-rows: 1

   * - Step
     - Desciption
     - Business Logic Acceptance Criteria
   * - 1. Phát hiện khuôn mặt trong ảnh
     - Phát hiện khuôn mặt người đi đường trong ứng dụng Android
     - Phát hiện tất cả các khuôn mặt trong ảnh
   * - 2. Xóa/làm mờ khuôn mặt
     - Làm mờ khuôn mặt
     - Cần làm mờ khuôn mặt hết mức có thể

FR-CC: Connection Checking
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. list-table:: **FR-CC**
   :widths: 15 10
   :header-rows: 1

   * - Content
     - Detail
   * - Description
     - Cho phép kiểm tra, thông báo trạng thái connect giữa thiết bị và điện thoại
   * - Input
     - Thông tin connect
   * - Output
     - Thông báo connect/disconnect.
   * - Preconditions
     - Thiết bị được cấp nguồn đầy đủ.
       
     Required: AI Unit phải có cổng USB Type C thì mới kết nối được với Android Type C cho phép lấy thông tin qua ADB Interface.
     (Nếu muốn hỗ trợ cổng khác, AI unit cũng cần phải hỗ trợ cổng khác).
   * - Postconditions
     - Lưu log chương trình.


.. list-table:: **Business Flow**
   :widths: 15 30 30
   :header-rows: 1

   * - Step
     - Description
     - Business Logic Acceptance Criteria
   * - 1. Khởi động ứng dụng
     - Mở ứng dụng
     - Hiển thị trạng thái điện thoại chưa được kết nối với ứng dụng
   * - 2. Kết nối thiết bị với ứng dụng
     - Kết nối vật lý dây usb
     - Ứng dụng hiển thị thông báo: đã connect với device
   * - 3. Disconnect
     - Ngắt kết nối vật lý dây usb
     - Ứng dụng hiển thị thông báo: đã disconnect với device.


Non-Function Requirement
-----------------------------

* MIoU: 70%
* Accuracy: 70%

Appendix
-------------------

Config file

.. list-table:: **Business Flow**
   :widths: 10 20 30
   :header-rows: 1

   * - Key
     - Desciption
     - Value
   * - ``mode``
     - Chọn cách ứng dụng khởi động
     - Datatype: String

       ``paused`` : ứng dụng khởi động bằng lệnh gửi từ máy tính

       ``running`` : ứng dụng khởi động ngay khi cắm nguồn
   * - ``processing_mode``
     - Chọn chế độ hoạt động
     - Datatype: String

       ``capture-segment`` : ứng dụng chính

       ``capture`` : thu thập ảnh
   * - ``interval``
     - Thời gian giữa 2 lần liên tiếp chụp ảnh
     - Datatype: Float (unit: giây)
   * - ``model_path``
     - Đường dẫn đến folder chưa model
     - Datatype: String

       Sample: "/userdata/models/ddrnet_rk1808.rknn"
   * - ``base_dir``
     - Đường dẫn đến thư mục lưu trữ ảnh
     - Datatype: String

       Sample: "/userdata/captures"
