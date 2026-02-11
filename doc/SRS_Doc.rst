SRS Documweentation
======================

Introduction
--------------

Phát triển hệ thống ứng dụng cho phép phát hiện vị trí xe đạp đang di chuyển, từ đó đứa ra cảnh báo người dùng, từ đó giúp nâng cao ý thức người sử dụng xe đạp.

Hệ thống bao gồm:

* Ứng dụng được phát triển trên thiết bị do khách hàng cung cấp
* Ứng dụng điện thoại.

Scope
------

In-Scope
^^^^^^^^^^^^^^
Phát triển ứng dụng trên thiết bị là điện thoại, cho phép phát cảnh báo tới người dùng khi phát hiện người dùng đang di chuyển trên vỉa hè.

Các tính năng chính:

* Xác định vị trí xe đang di chuyển, phát cảnh báo đi chậm
* Kiểm tra connect giữa thiết bị và điện thoại
* Push ảnh từ thiết bị sang điện thoại.
* Xóa thông tin nhạy cảm trong ảnh.
* Mã hóa quá trình gửi bản tin.

Out-of-Scope
^^^^^^^^^^^^^^^^

* Không hỗ trợ ứng dụng Ios.
* Không hỗ trợ lunching app lên Store.
* Ứng dụng không hỗ trợ chạy nền.

Overall Description
---------------------

Product Perspective
^^^^^^^^^^^^^^^^^^^^^^

Hiher-Level user Functions
^^^^^^^^^^^^^^^^^^^^^^^^^^

Contraints
^^^^^^^^^^^^^^

* Phát triển trên thiết bị phần cứng thiết bị do khách hàng cung cấp.
* Model phát triển phải convert sang định dạng RKNN để có thể implement được lên thiết bị phần cứng.

Security

* Mã hóa thông tin gửi từ thiết bị sang ứng dụng, từ đó cho phép chỉ ứng dụng mới có thể đọc được thông tin thiết bị gửi về.

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
     - Ảnh thu thập khách hàng cung cấp
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
   * - 2. Check file config
     - Ứng dụng kiểm tra file config và hoạt động.

       Nội dung file config:

	   {

          "mode": "start",

          "processing_mode": "capture-segment",

          "interval": 0.5,

          "model_path": "/userdata/models/ddrnet_rk1808.rknn",

          "base_dir": "/userdata/captures"

        }
     - File config cho phép điều chỉnh thông số của ứng dụng trên thiết bị phần cứng. Tham khảo phụ lục 1.

       Ứng dụng khởi động khi được cấp nguồn, hoạt động với chế độ phát hiện vị trí xe đạp.

       Thời gian giữa 2 lần chụp ảnh liên tiếp: 0.5s.

       Hoạt động với đúng model AI, lưu ảnh vào thư mục yêu cầu
   * - 4. Gửi ảnh cho ứng dụng điện thoại
     - Ảnh sau khi được thu thập sẽ được gửi cho thiết bị
     - Gửi ảnh overlay, ảnh raw.

       Ảnh sau khi được gửi thành công, tiến hành hóa phía thiết bị phần cứng.

       Lưu log
   * - 3. Dừng chương trình
     - **Nếu:**

       Nhận được lệnh yêu cầu dừng chương trình

       **Thì:**

       Ứng dụng tạm dừng
     - Ứng dụng dừng lại, lưu log.

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
     - Thông tin vị trí xe đang di chuyển: vỉa hè/lòng đường.
   * - Preconditions
     - Thiết bị được cấp nguồn đầy đủ.

       Bộ nhớ trong chưa đầy.
   * - Postconditions
       
     - Lưu log chương trình.
       
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
     - Tính năng cho phép phần cứng gửi thông tin vị trí xe đạp về ứng dụng điện thoại thông qua abd interface.
   * - Input
     - Thông tin vị trí xe đạp được lấy tiwf FR-LW-1.
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

FR-CC: Connect Checking
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. list-table:: **FR-DA**
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
       
       Thiết bị hỗ trợ dây USB Type C, cho phép lấy thông tin qua ADB Interface.
	   
	   (Nếu muốn hỗ trợ cổng khác, thiết bị cũng cần phải hỗ trợ).
   * - Postconditions
     - Lưu log chương trình.


.. list-table:: **Business Flow**
   :widths: 15 30 30
   :header-rows: 1

   * - Step
     - Desciption
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
* Tốc độ xử lý min: 500ms/image

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

       ``start`` : ứng dụng khởi động ngay khi cắm nguồn
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