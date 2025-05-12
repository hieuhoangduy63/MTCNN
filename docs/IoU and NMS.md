# Intersection over Union (IoU)

## 1. Định nghĩa
IoU là độ đo dùng trong các mô hình nhận diện thực thể (object detection). Nó đo mức độ trùng khớp giữa Predicted Bounding Box và Ground Truth Bounding Box.

## 2. Công thức

IoU = Area of Overlap / Area of Union

Trong đó:
- Area of Overlap: Diện tích giao nhau giữa Predicted Bounding Box và Ground Truth Bounding Box.
- Area of Union: Diện tích hợp của cả hai bounding box.

Giá trị IoU nằm trong khoảng [0, 1]:
- IoU = 0: Không có sự trùng khớp giữa hai bounding box
- IoU = 1: Hai bounding box hoàn toàn trùng khớp với nhau

![Công thức IoU](Image/IoU2.jpg)

![Minh họa IoU với khung dự đoán màu đỏ và khung chuẩn màu đen](Image/IoU1.jpg)

## 3. Ứng dụng
Ứng dụng của IoU trong Non-max Suppression (NMS): Trong trường hợp có nhiều khung giới hạn cho cùng một thực thể, chúng ta có thể loại bỏ các khung thừa với thuật toán Non-max Suppression.

# Non-max Suppression (NMS)

## 1. Định nghĩa
Thuật toán Non-Maximum Suppression sinh ra để loại bỏ đi các bounding box dư thừa của cùng một đối tượng trong ảnh.

Ví dụ output của một mô hình face detection:

![Hình minh họa trước khi áp dụng NMS](Image/before.png)

![Hình minh họa sau khi áp dụng NMS](Image/after.png)

## 2. Nội dung thuật toán:

### Input:
- Một mảng các bounding box, mỗi box có dạng (x1, y1, x2, y2, c)
  - (x1, y1): tọa độ điểm trên bên trái của bounding box
  - (x2, y2): tọa độ điểm dưới bên phải của bounding box
  - c: confidence score của bounding box
- Giá trị ngưỡng IoU

### Output:
- Một mảng các bounding box đã được loại bỏ các box dư thừa

### Chi tiết thuật toán:
Các ký hiệu:
- S: bounding box đang xét
- P: Tập các box đầu vào của thuật toán
- thresh_iou: Ngưỡng IoU để loại bỏ các box thừa
- keep: Tập các box sau khi đã loại bỏ các box thừa

![Hình minh họa thuật toán NMS](Image/nms1.jpg)

Thuật toán bao gồm 3 bước:

Bước 1: Chọn box S có confidence score cao nhất trong tập P, loại bỏ box đó ra khỏi tập P và thêm box đó vào tập keep.

![Hình minh họa bước 1](Image/nms2.jpg)

Bước 2: Thực hiện tính toán IoU giữa box S vừa lấy ra ở bước 1 với toàn bộ các box còn lại trong tập P. Nếu có box nào trong P có IoU với box S đang xét mà lớn hơn ngưỡng thresh_iou thì loại bỏ box đó ra khỏi P.

Bước 3: Lặp lại bước 1 cho đến khi P không còn box nào. Sau khi kết thúc thuật toán thì keep chứa toàn bộ những box sau khi đã loại bớt các box thừa.