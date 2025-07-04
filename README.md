# **Nhập Môn Xử Lý Ảnh Số - Bài Thực Hành Lab 5**
**Sinh viên thực hiện:** Nguyễn Kim Gia Bách
**MSSV:** 2174802010320
**Môn học:** Nhập môn Xử lý ảnh số  
**Giảng viên:** Đỗ Hữu Quân

## Lab 5: Xác Định Đối Tượng Trong Ảnh (Object Detection & Measurement)

Bài thực hành này tập trung vào các kỹ thuật xác định và phân biệt các đối tượng trong ảnh thông qua phân vùng, gán nhãn, phát hiện cạnh, đo đặc trưng, và so khớp hình ảnh.

### 1. Gán Nhãn Các Vùng Đối Tượng (Region Labeling)
```python
import cv2
import numpy as np
img = cv2.imread("image.jpg", 0)
_, binary = cv2.threshold(img, 127, 255, cv2.THRESH_BINARY)
num_labels, labels = cv2.connectedComponents(binary)
```

### 2. Phát Hiện Cạnh (Edge Detection)
**a. Dò Cạnh Theo Chiều Dọc**
```python
vertical_edges = np.abs(np.diff(img, axis=0))
```
**b. Dò Cạnh Với Sobel Filter**
```python
sobelx = cv2.Sobel(img, cv2.CV_64F, 1, 0)
sobely = cv2.Sobel(img, cv2.CV_64F, 0, 1)
sobel = np.hypot(sobelx, sobely)
```

### 3. Xác Định Góc Đối Tượng (Harris Corner Detection)
```python
gray = cv2.cvtColor(img_color, cv2.COLOR_BGR2GRAY)
corners = cv2.cornerHarris(np.float32(gray), 2, 3, 0.04)
```

### 4. Dò Tìm Hình Dạng Trong Ảnh (Hough Transform)
**a. Đường tròn:**
```python
circles = cv2.HoughCircles(gray, cv2.HOUGH_GRADIENT, 1, 20)
```
**b. Đường thẳng:**
```python
edges = cv2.Canny(gray, 50, 150)
lines = cv2.HoughLines(edges, 1, np.pi / 180, 150)
```

### 5. So Khớp Hình Ảnh (Image Matching)
```python
orb = cv2.ORB_create()
kp1, des1 = orb.detectAndCompute(img1, None)
kp2, des2 = orb.detectAndCompute(img2, None)
bf = cv2.BFMatcher(cv2.NORM_HAMMING, crossCheck=True)
matches = bf.match(des1, des2)
```

### Bài Tập Lab 5:
- Gán nhãn đối tượng
- Dò cạnh bằng Sobel
- Phát hiện góc bằng Harris
- Dò hình bằng Hough Transform
- So khớp ảnh tương tự
- Menu chọn chức năng

---

## Hướng dẫn sử dụng

1. Cài đặt thư viện:
```bash
pip install pillow numpy matplotlib imageio scipy opencv-python
```
2. Mở `main.ipynb` trong Jupyter/Colab
3. Đặt ảnh đầu vào trong thư mục `images/`

---

## Tài liệu tham khảo

- Sách Digital Image Processing - Gonzalez & Woods  
- Tài liệu OpenCV, SciPy, Scikit-Image  
- Slide môn học - Đại học Văn Lang
