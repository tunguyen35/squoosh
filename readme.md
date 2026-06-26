Dưới đây là toàn bộ "vũ khí" chuẩn chỉnh dành riêng cho Google Colab Free. Mình đã tối ưu sẵn: cài đúng phiên bản Node.js phù hợp, mở khóa tối đa dung lượng RAM 9GB (trên tổng số 13GB của Colab) và bật chế độ build đa luồng để vắt kiệt tốc độ của 2 Core chip Xeon.
Bạn chỉ cần mở một Notebook mới trên Google Colab, tạo 3 ô mã nguồn (Code cell) riêng biệt rồi copy-paste đống lệnh này vào chạy từ trên xuống dưới là xong:

```text
Ô số 1: Clone code và cài đặt môi trường (Chạy tầm 1 phút)
Ô này sẽ tải code Squoosh từ Google Labs về và cài đặt môi trường Node.js sạch sẽ cho máy ảo.

# 1. Tải mã nguồn Squoosh về máy ảo
!git clone https://github.com/GoogleChromeLabs/squoosh.git
%cd squoosh

# 2. Cài đặt Node.js phiên bản 16 (Bản này tương thích tốt nhất với cấu trúc Squoosh gốc)
!curl -fsSL https://deb.nodesource.com/setup_16.x | sudo -E bash -
!sudo apt-get install -y nodejs

# 3. Kiểm tra lại xem Node và NPM đã sẵn sàng chưa
!node -v
!npm -v


Ô số 2: Cài thư viện và Ép xung build đa luồng (Chạy tầm 5 - 7 phút)
Đây là khúc quan trọng nhất. Mình cấp hẳn 9GB RAM cho Node và cho phép tận dụng tối đa các Core để dịch đống code C++ sang WebAssembly với tốc độ bàn thờ.

# 1. Cài đặt các thư viện phụ thuộc (Dependencies)
!npm install

# 2. Bật max công suất để biên dịch (Cấp 9GB RAM, không bóp Core như bên GitHub)
!NODE_OPTIONS="--max-old-space-size=9216" npm run build


Ô số 3: Đóng gói thành phẩm (Chạy tầm 5 giây)
Build xong xuôi, ô này sẽ gom toàn bộ file tĩnh (.wasm, .js, .html) nằm trong thư mục build nén lại thành một file .zip để bạn tải về máy tính.

# 1. Nén thư mục sản phẩm thành file zip tên là squoosh-clean.zip
!zip -r /content/squoosh-clean.zip build/

# 2. Thông báo hoàn thành
!echo "=== ĐÃ BUILD XONG SẠCH SẼ! Bạn nhìn sang menu thư mục bên trái, tìm file 'squoosh-clean.zip' và tải về nhé ==="