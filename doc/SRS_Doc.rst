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