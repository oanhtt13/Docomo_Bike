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
     - Ảnh thu thập từ Camera
   * - Output
     - Thông tin vị trí xe đang di chuyển: vỉa hè/lòng đường.
   * - Preconditions
     - Thiết bị được cấp nguồn đầy đủ.
     - Bộ nhớ trong chưa đầy.
   * - Postconditions
     - Lưu log chương trình.
     - Thông tin ảnh overlay.
     - Thông tin ảnh raw.

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

          "mode": "paused",

          "processing_mode": "capture-segment",

          "interval": 0.5,

          "model_path": "/userdata/models/ddrnet_rk1808.rknn",

          "base_dir": "/userdata/captures"
          
        }
     - File config cho phép điều chỉnh thông số của ứng dụng trên thiết bị phần cứng. Tham khảo phụ lục 1.

       Dựa trên thông tin trên file config ứng dụng hoạt động theo đúng cài đặt.

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