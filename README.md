# **Nhập Môn Xử Lý Ảnh Số - Bài Thực Hành Lab 2 & 3**
**Sinh viên thực hiện:** Example  
**MSSV:** Example  
**Môn học:** Nhập môn Xử lý ảnh số  
**Giảng viên:** Đỗ Hữu Quân

## **Giới thiệu tổng quan**

Dự án này tổng hợp các bài thực hành cốt lõi trong môn Nhập môn Xử lý ảnh số, tập trung vào hai mảng kỹ thuật chính: **biến đổi cường độ (Lab 2)** và **biến đổi hình học (Lab 3)**[1][2]. Mục tiêu là giúp sinh viên nắm vững các kỹ thuật nền tảng để tăng cường, phân tích và thao tác trên ảnh kỹ thuật số bằng ngôn ngữ Python[3].

## **Công nghệ sử dụng**

Dự án sử dụng các công nghệ và thư viện phổ biến trong lĩnh vực xử lý ảnh[1][2]:
*   **Python**: Ngôn ngữ lập trình chính được sử dụng[3].
*   **Pillow (PIL)**: Hỗ trợ đọc, chuyển đổi và lưu trữ các định dạng ảnh khác nhau[1].
*   **NumPy**: Nền tảng cho việc xử lý ảnh dưới dạng mảng số học hiệu suất cao[1][2].
*   **SciPy**: Thư viện chứa các hàm cấp cao cho biến đổi hình học như tịnh tiến, xoay, và co giãn[2].
*   **ImageIO**: Dùng để đọc và ghi các định dạng ảnh hiện đại[1][2].
*   **Matplotlib**: Dùng để hiển thị ảnh và các kết quả một cách trực quan[1][2].

---

## **Lab 2: Biến Đổi Cường Độ & Phân Tích Bit-Plane**

Bài thực hành này tập trung vào các phép biến đổi dựa trên giá trị cường độ của từng pixel để cải thiện chất lượng ảnh, làm nổi bật chi tiết và trích xuất đặc trưng[1][4].

### **1. Biến đổi ảnh đảo ngược (Negative/Inverse Transformation)**
*   **Mục đích**: Đảo ngược giá trị pixel (sáng thành tối và ngược lại), giúp làm nổi bật chi tiết trong các vùng tối của ảnh, đặc biệt hữu ích trong y khoa (ảnh X-quang)[1][4].
*   **Công thức**: $$ s = L - 1 - r $$, với $$L$$ là tổng số mức xám (thường là 256) và $$r$$ là giá trị pixel gốc[1].
*   **Code chính**:
    ```python
    import numpy as np
    img_np = np.array(img)
    negative_img = 255 - img_np
    ```

### **2. Biến đổi logarit (Log Transformation)**
*   **Mục đích**: Khuếch đại giá trị các pixel thấp và nén giá trị các pixel cao. Kỹ thuật này làm rõ chi tiết trong vùng tối mà không gây lóa các vùng sáng[1][4].
*   **Công thức**: $$ s = c \cdot \log(1 + r) $$, trong đó $$ c = \frac{255}{\log(1 + \max(r))} $$ là hằng số chuẩn hóa[1].
*   **Code chính**:
    ```python
    c = 255 / np.log(1 + np.max(img_np))
    log_img = c * np.log(1 + img_np)
    log_img = np.array(log_img, dtype=np.uint8)
    ```

### **3. Biến đổi hàm mũ/Gamma (Power-law/Gamma Transformation)**
*   **Mục đích**: Hiệu chỉnh gamma của ảnh, dùng để làm sáng (gamma  1) hình ảnh một cách linh hoạt[1][4].
*   **Công thức**: $$ s = c \cdot r^\gamma $$, với $$\gamma$$ là hệ số gamma[1].
*   **Code chính**:
    ```python
    gamma = 0.5  # Làm sáng ảnh
    img_norm = img_np / 255.0
    gamma_img = np.power(img_norm, gamma)
    gamma_img = np.uint8(gamma_img * 255)
    ```

### **4. Phân tích mặt phẳng bit (Bit-Plane Slicing)**
*   **Mục đích**: Phân tách ảnh 8-bit thành 8 ảnh nhị phân riêng biệt (mỗi ảnh tương ứng một mặt phẳng bit). Kỹ thuật này giúp phân tích sự đóng góp của từng bit vào hình ảnh tổng thể và có thể được dùng để phát hiện thông tin ẩn (steganography)[1].
*   **Nguyên lý**: Các bit có trọng số cao (MSB) chứa thông tin về cấu trúc chính, trong khi các bit trọng số thấp (LSB) thường chứa nhiễu hoặc chi tiết nhỏ[1].
*   **Code chính**:
    ```python
    bit_plane_7 = (img_np >> 7) & 1  # Lấy mặt phẳng bit thứ 7 (MSB)
    bit_plane_7_img = bit_plane_7 * 255
    ```

### **Bài tập Lab 2**
Tạo một chương trình menu tương tác cho phép người dùng chọn áp dụng một trong các phép biến đổi trên (Ảnh đảo ngược, Logarit, Gamma, Cân bằng lược đồ xám, Giãn tương phản) lên các hình ảnh cho trước[4].

---

## **Lab 3: Biến Đổi Hình Học**

Bài thực hành này tập trung vào các kỹ thuật thay đổi thuộc tính không gian của ảnh như vị trí, kích thước và hướng của đối tượng[2].

### **1. Chọn đối tượng trong ảnh (Cropping)**
*   **Mục đích**: Trích xuất một vùng ảnh con (Region of Interest - ROI) từ ảnh gốc để xử lý hoặc phân tích riêng biệt[2].
*   **Nguyên lý**: Sử dụng kỹ thuật cắt lát (slicing) của mảng NumPy[2].
*   **Code chính**:
    ```python
    # Cắt ảnh từ hàng 800-1200 và cột 570-980
    orange_img = img_data[800:1200, 570:980]
    ```

### **2. Tịnh tiến ảnh (Translation)**
*   **Mục đích**: Di chuyển toàn bộ ảnh hoặc một đối tượng theo một vector dịch chuyển xác định[2].
*   **Hàm sử dụng**: `scipy.ndimage.shift(input, shift)`[2].
*   **Code chính**:
    ```python
    import scipy.ndimage as nd
    # Dịch chuyển ảnh xuống 100 pixels và sang phải 25 pixels
    translated_img = nd.shift(img_data, (100, 25, 0))
    ```

### **3. Thay đổi kích thước ảnh (Scaling)**
*   **Mục đích**: Phóng to hoặc thu nhỏ kích thước không gian của ảnh, hữu ích trong việc chuẩn hóa dữ liệu hoặc điều chỉnh hiển thị[2].
*   **Hàm sử dụng**: `scipy.ndimage.zoom(input, zoom_factor)`[2].
*   **Code chính**:
    ```python
    # Phóng to ảnh gấp 2 lần
    zoomed_in_img = nd.zoom(img_data, (2, 2, 1))
    ```

### **4. Xoay ảnh (Rotation)**
*   **Mục đích**: Xoay ảnh quanh tâm một góc nhất định để điều chỉnh hướng của đối tượng[2].
*   **Hàm sử dụng**: `scipy.ndimage.rotate(input, angle, reshape=True)`[2].
*   **Code chính**:
    ```python
    # Xoay ảnh 20 độ và tự động thay đổi kích thước để không bị cắt
    rotated_img = nd.rotate(img_data, 20, reshape=True)
    ```

### **Bài tập Lab 3**
1.  Chọn đối tượng từ ảnh và thực hiện tịnh tiến[2].
2.  Chọn nhiều đối tượng và thay đổi màu sắc của chúng[2].
3.  Chọn đối tượng và xoay chúng một góc 45 độ[2].
4.  Chọn đối tượng và phóng to kích thước lên 5 lần[2].
5.  Viết chương trình menu tương tác cho các phép biến đổi hình học (Tịnh tiến, Xoay, Phóng to/Thu nhỏ)[2].

---

## **Cấu trúc dự án**
```
├── main.ipynb
├── images/
│   ├── fruit.jpg
│   ├── world_cup.jpg
│   └── ...
└── README.md
```

## **Hướng dẫn sử dụng**

#### **1. Cài đặt môi trường**
Cài đặt Python và các thư viện cần thiết thông qua pip[1][2]:
```bash
pip install pillow numpy matplotlib imageio scipy
```

#### **2. Chạy chương trình**
*   Mở tệp `main.ipynb` bằng Jupyter Notebook, Visual Studio Code hoặc Google Colab[1][2].
*   Thực thi từng ô mã (cell) để quan sát kết quả của các thuật toán[1][2].
*   Đảm bảo các tệp ảnh được đặt đúng trong thư mục `images/` hoặc cập nhật đường dẫn tương ứng trong mã nguồn[1][2].

## **Tài liệu tham khảo**

*   Sách *Digital Image Processing* của Rafael C. Gonzalez và Richard E. Woods[1].
*   Tài liệu từ thư viện Scikit-image, OpenCV, và SciPy[1][2].
*   Slide bài giảng môn Nhập môn Xử lý ảnh số - Đại học Văn Lang[1][2][4].

[1] https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/62014088/332bd501-f7b5-4450-86e2-49eb0a49472b/README.md
[2] https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/62014088/19608521-8c6f-450d-adbe-3921b438856f/Lab_3_Geometric_Transformation_images.pdf
[3] programming.image_processing
[4] https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/62014088/43aba22b-4f08-4bc8-9d3b-5efc8e3c62df/Lab_2_Image_Enhances_img.pdf
