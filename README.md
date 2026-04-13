BÁO CÁO DỰ ÁN RS-TREE 
1. Giới thiệu đề bài
Đề bài yêu cầu xây dựng một hệ thống quản lý và truy vấn video dựa trên cấu trúc dữ liệu RS-tree.
Hệ thống cho phép tìm kiếm video dựa trên:
- Đối tượng (Object) 
- Hoạt động (Activity) 
- Thuộc tính (Property) 
- Khoảng thời gian (frame) 
Mục tiêu
- Xây dựng RS-tree từ dữ liệu đầu vào (Segment Table) 
- Cài đặt các hàm truy vấn theo yêu cầu đề bài 
- Thiết kế giao diện cho phép người dùng tương tác 
- Minh họa hoạt động hệ thống bằng video 
2. Biểu diễn dữ liệu đầu vào
Dữ liệu đầu vào là một Segment Table, trong đó mỗi phần tử có dạng:
(type, value, start, end)
Trong đó:
- type: loại dữ liệu (object / activity / prop) 
- value: tên (ví dụ: "Người", "Ngồi", "Uống") 
- start: frame bắt đầu 
- end: frame kết thúc 
👉 Ví dụ:
("activity", "Ngồi", 10, 90)
("activity", "Uống", 15, 42)
3. Cấu trúc RS-tree
Mỗi node trong RS-tree có cấu trúc:
Node = (start, end, left, right, entries)
Trong đó:
- start, end: khoảng thời gian node quản lý 
- left, right: node con 
- entries: danh sách các segment thuộc node 
4. Thuật toán xây dựng RS-tree
4.1. Ý tưởng chính
Thuật toán sử dụng phương pháp chia để trị (divide and conquer):
- Chia khoảng thời gian thành 2 phần 
- Phân phối segment vào node hoặc node con 
- Lặp lại cho đến khi đạt điều kiện dừng 
4.2. Các bước thực hiện
Bước 1: Khởi tạo node
Tạo node với khoảng [start, end].
Bước 2: Điều kiện dừng
Nếu:
- (end - start ≤ 5) hoặc 
- số lượng segment ≤ 3 
→ Dừng chia, node trở thành leaf node
Bước 3: Xác định điểm chia
mid = (start + end) / 2
Bước 4: Phân loại segment
- Segment cắt qua mid: 
start ≤ mid AND end > mid
→ Lưu tại entries của node hiện tại
- Segment bên trái: 
start ≤ mid
→ Đưa vào node trái

- Segment bên phải: 
end > mid
→ Đưa vào node phải
Bước 5: Đệ quy
Xây dựng:
left subtree  = buildRSTree(leftSegments)
right subtree = buildRSTree(rightSegments)
4.3. Đặc điểm
- Một segment có thể thuộc: 
onode hiện tại 
onode trái 
onode phải 
- Điều này giúp xử lý tốt các đoạn overlap 
5. Thuật toán truy vấn RS-tree
5.1. Mục tiêu
Tìm các segment thỏa mãn:
- thuộc khoảng thời gian [s, e] 
- thỏa điều kiện (object, activity, …) 
5.2. Các bước thực hiện
Bước 1: Kiểm tra overlap
Nếu:
node.end < s OR node.start > e
→ Node không liên quan → bỏ qua (pruning)
Bước 2: Lấy dữ liệu tại node
Lọc các segment trong entries theo điều kiện:
filterFn(segment)
Bước 3: Đệ quy xuống node con
query(left)
query(right)
Bước 4: Gộp kết quả
- Kết hợp kết quả từ node hiện tại và node con 
- Loại bỏ phần tử trùng lặp 
5.3. Kỹ thuật sử dụng
- DFS (Depth-First Search) 
- Pruning (cắt tỉa) 
6. Độ phức tạp
6.1. Xây dựng cây
- Trung bình: 
O(n log n)
6.2. Truy vấn
- Tốt nhất (cắt tỉa nhiều): 
O(log n)
- Xấu nhất: 
O(n)
