# ThucHanhXuLiAnhSo

## LAB3: Biến Đổi Hình Học (Geometric Transformations)

**LAB3_2174802010320_NguyenKimGiaBach.ipynb** gồm các phần thực hành về biến đổi hình học trên ảnh bằng Python và các thư viện như numpy, scipy, imageio, matplotlib.

### Nội dung chính:

1. **Chọn đối tượng trong ảnh**  
   - Cắt một phần của ảnh (ví dụ: cắt quả cam từ ảnh 'fruit.jpg').

2. **Tịnh tiến đơn (Translation)**  
   - Dịch chuyển ảnh bằng scipy.ndimage.shift.

3. **Thay đổi kích thước ảnh (Resizing)**  
   - Phóng to, thu nhỏ ảnh bằng scipy.ndimage.zoom.

4. **Xoay ảnh (Rotation)**  
   - Xoay ảnh với scipy.ndimage.rotate (có tuỳ chọn reshape).

5. **Dilation và Erosion**  
   - Biến đổi nhị phân mở rộng (dilation) và co lại (erosion) trên ảnh nhị phân.

6. **Biến đổi toạ độ (Coordinate Mapping)**  
   - Làm biến dạng ảnh bằng cách thay đổi toạ độ điểm ảnh với scipy.ndimage.map_coordinates.

7. **Biến đổi chung (Generic Transformation)**  
   - Áp dụng biến đổi tổng quát tuỳ ý lên ảnh.

### Yêu cầu chạy LAB3:

- Python 3.x
- Các thư viện: numpy, scipy, imageio, matplotlib
- Các file ảnh: 'fruit.jpg', 'world_cup.jpg' (cần có trong cùng thư mục notebook)

### Cách chạy:

```bash
pip install numpy scipy imageio matplotlib
jupyter notebook LAB3_2174802010320_NguyenKimGiaBach.ipynb
