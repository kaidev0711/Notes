# **Vector trong Rust**

## **1. Giới thiệu**

`Vec<T>` (Vector) là một cấu trúc dữ liệu động trong Rust, cho phép lưu trữ một
tập hợp các phần tử cùng kiểu dữ liệu. Vector là một trong những cấu trúc dữ
liệu phổ biến nhất trong Rust, được sử dụng rộng rãi nhờ tính linh hoạt và hiệu
suất cao. Nó cung cấp khả năng truy xuất ngẫu nhiên, thêm/xóa phần tử một cách
hiệu quả, và tự động quản lý bộ nhớ.

---

## **2. Cấu trúc của Vector**

### **2.1. Thành phần chính**

Một `Vec<T>` trong Rust bao gồm các thành phần chính sau:

- **Con trỏ (Pointer)**: Trỏ đến vùng nhớ trên Heap, nơi lưu trữ các phần tử.

- **Dung lượng (Capacity)**: Số lượng phần tử tối đa mà Vector có thể chứa mà
  không cần cấp phát lại bộ nhớ.

- **Độ dài (Length)**: Số lượng phần tử hiện tại trong Vector.

```rust
let  vec:  Vec<i32> =  vec![1, 2, 3];
println!("length of vec: {}", vec.len());
println!("capacity of vec: {}", vec.capacity());
println!("address of vec: {:p}", &vec);
println!("size of vec: {}", std::mem::size_of_val(&vec));
println!("vec: {:?}", vec);
//length of vec: 3
//capacity of vec: 3
//address of vec: 0xf602ffc88
//size of vec: 24
//vec: [1, 2, 3]
```

### **2.2. Quản lý bộ nhớ**

- Vector tự động quản lý bộ nhớ bằng cách cấp phát động trên Heap.

- Khi số lượng phần tử vượt quá dung lượng hiện tại, Vector sẽ tự động mở rộng
  bằng cách cấp phát một vùng nhớ mới lớn hơn và sao chép các phần tử sang vùng
  nhớ mới.

```rust
let  mut  vec:  Vec<i32> =  vec![1, 2, 3];//length of vec: 3
vec.push(4);
println!("length of vec: {}", vec.len());
println!("capacity of vec: {}", vec.capacity());
println!("vec: {:?}", vec);
//length of vec: 4
//capacity of vec: 6
//vec: [1, 2, 3, 4]
```

---
## **3. Cách Vector hoạt động**



### **3.1. Khởi tạo Vector**

-  **`Vec::new()`**: Tạo một Vector rỗng.
```rust
let  mut  v: Vec<i32> = Vec::new();
```

-  **`vec![]`**: Tạo một Vector với các phần tử ban đầu.

```rust
let  v = vec![1, 2, 3];
```
### **3.2. Thêm phần tử vào Vector**

-  **`push(value)`**: Thêm một phần tử vào cuối Vector.
```rust
vec.push(4); // Vector sau khi thêm: [1, 2, 3, 4]
```

-  **`extend(iter)`**: Thêm nhiều phần tử từ một iterator.

```rust
vec.extend([5, 6].iter()); // Vector sau khi thêm: [1, 2, 3, 4, 5, 6]
```
-  **`append(&mut vec)`**: Nối một vector khác và làm rỗng vector đó

```rust
let mut a = vec![1, 2];
let mut b = vec![3, 4];
a.append(&mut b);
// a: [1, 2, 3, 4]
// b: [] (đã bị move hết phần tử)
```
  -  **`insert(index, value)`**: Chèn vào vị trí cụ thể

```rust
let mut v = vec![10, 20, 30];
v.insert(1, 15); // chèn 15 vào vị trí 1
// => [10, 15, 20, 30]
```
  -  **`splice(start..end, vec)`**: Chèn hoặc thay thế vùng con

```rust
let mut v = vec![1, 4, 5];
v.splice(1..1, vec![2, 3]); // chèn tại vị trí 1
// => [1, 2, 3, 4, 5]
```

### **3.3. Truy xuất phần tử trong Vector**

-  **Chỉ mục (Indexing)**:

```rust
let  first = v[0]; // Truy xuất phần tử đầu tiên
```

- Lưu ý: Truy xuất bằng chỉ mục sẽ panic nếu chỉ mục nằm ngoài phạm vi.

-  **`get(index)`**: Trả về `Option<&T>`, an toàn hơn khi truy xuất.

```rust
if  let  Some(value) = v.get(2) {
println!("Phần tử thứ 3 là: {}", value);
}
```
-  **`slice`**: Trả về `&vec`, có thể gây panic nếu truy cập phạm vi không hợp lệ.

```rust
let v = vec![10, 20, 30, 40];
let slice = &v[1..3];   // &[20, 30]
```
### **3.4. Xóa phần tử khỏi Vector**

-  **`pop()`**: Xóa và trả về phần tử cuối cùng.

```rust
let  last = v.pop(); // last = Some(6)
```

-  **`remove(index)`**: Xóa phần tử tại chỉ mục cụ thể.

```rust
v.remove(1); // Vector sau khi xóa: [1, 3, 4, 5]
```

-  **`clear()`**: Xóa toàn bộ phần tử trong Vector.
```rust
v.clear(); // Vector rỗng: []
```
-  **`drain()`**: Xóa phần tử theo vùng

```rust
let mut v = vec![10, 20, 30, 40, 50];
v.drain(1..3); // xóa phần tử tại index 1 và 2 (20, 30)
// v: [10, 40, 50]
```
  -  **`retain()`**: Xóa phần tử theo điều kiện

```rust
let mut v = vec![1, 2, 3, 4, 5];
v.retain(|x| x % 2 == 0); // chỉ giữ số chẵn
// v: [2, 4]
```
  -  **`swap_remove()`**: Đổi chỗ với phần tử cuối rồi xóa

```rust
let mut v = vec![10, 20, 30];
v.swap_remove(1); // xóa index = 1 (nhưng swap với phần tử cuối)
// v: [10, 30]
```
- Nhanh hơn `remove()` vì không cần shift phần tử phía sau
 - Làm thay đổi thứ tự các phần tử
### **3.5. Điều chỉnh kích thước Vector**

-  **`reserve(n)`**: Đảm bảo đủ bộ nhớ cho `n` phần tử tiếp theo.

```rust
v.reserve(10); // Đảm bảo dung lượng cho 10 phần tử
```

-  **`shrink_to_fit()`**: Thu hồi bộ nhớ thừa.

```rust
v.shrink_to_fit(); // Giảm dung lượng về bằng độ dài hiện tại
```



### **3.6. Duyệt Vector**

-  **`for` loop**:

```rust
for  value  in &v {
println!("{}", value);
}
```
-  **`iter()`**: Trả về iterator của các phần tử.
```rust
for (index, value) in  v.iter().enumerate() {
println!("Phần tử {}: {}", index, value);
}
```
---

## **4. Hiệu suất của Vector**

### **4.1. Độ phức tạc thời gian**

- **Truy xuất phần tử**: O(1) (truy xuất ngẫu nhiên).

- **Thêm phần tử**:

- O(1) trung bình (khi không cần mở rộng bộ nhớ).

- O(n) trong trường hợp xấu nhất (khi cần mở rộng bộ nhớ và sao chép các phần
  tử).

- **Xóa phần tử**:

- O(1) khi xóa phần tử cuối cùng (`pop`).

- O(n) khi xóa phần tử ở giữa (`remove`), do cần dịch chuyển các phần tử còn
  lại.

### **4.2. Quản lý bộ nhớ**

- Vector tự động quản lý bộ nhớ và mở rộng khi cần thiết.

- Khi số lượng phần tử vượt quá dung lượng hiện tại, Vector sẽ tăng dung lượng
  lên gấp đôi (hoặc theo một hệ số nhất định) để giảm thiểu số lần cấp phát lại
  bộ nhớ.

---

## **5. Khi nào nên sử dụng Vector?**

### **5.1. Ưu điểm**

- **Truy xuất nhanh**: Vector cung cấp thời gian truy xuất ngẫu nhiên O(1).

- **Linh hoạt**: Có thể thêm/xóa phần tử một cách dễ dàng.

- **Tự động quản lý bộ nhớ**: Vector tự động cấp phát và giải phóng bộ nhớ.

### **5.2. Nhược điểm**

- **Chi phí thêm/xóa phần tử ở giữa**: Việc thêm hoặc xóa phần tử ở giữa Vector
  có chi phí cao (O(n)) do cần dịch chuyển các phần tử còn lại.

- **Không phù hợp cho các thao tác tìm kiếm**: Vector không được tối ưu cho các
  thao tác tìm kiếm (cần sử dụng các cấu trúc dữ liệu khác như `HashMap` hoặc
  `BTreeMap`).

### **5.3. Ứng dụng**

- **Lưu trữ dữ liệu tuần tự**: Ví dụ, lưu trữ danh sách các số nguyên, chuỗi,
  hoặc đối tượng.

- **Đệm dữ liệu**: Ví dụ, lưu trữ dữ liệu tạm thời trong quá trình xử lý.

- **Triển khai các cấu trúc dữ liệu khác**: Ví dụ, stack, queue, hoặc heap.

---
## **6. Kết luận**

`Vec<T>` (Vector) trong Rust là một cấu trúc dữ liệu động mạnh mẽ và linh hoạt, phù hợp cho các ứng dụng cần lưu trữ và truy xuất dữ liệu một cách hiệu quả. Với khả năng tự động quản lý bộ nhớ và hiệu suất cao, Vector là một công cụ không thể thiếu trong hộp công cụ của lập trình viên Rust. Tuy nhiên, việc sử dụng Vector cần cân nhắc dựa trên yêu cầu cụ thể của ứng dụng, đặc biệt là khi cần thực hiện các thao tác thêm/xóa phần tử ở giữa.
---

# ** VecDeque trong Rust**

## **1. Giới thiệu**

`VecDeque<T>` (Vector Double-Ended Queue) là một cấu trúc dữ liệu trong Rust,
kết hợp giữa tính chất của `Vector` và `Queue`. Nó cho phép thêm/xóa phần tử ở
cả hai đầu (đầu và cuối) một cách hiệu quả, với độ phức tạc thời gian trung bình
O(1). `VecDeque` là một công cụ mạnh mẽ và linh hoạt, được sử dụng rộng rãi
trong các ứng dụng cần thao tác nhanh ở cả hai đầu dữ liệu.

---

## **2. Cấu trúc của VecDeque**

### **2.1. Thành phần chính**

Một `VecDeque<T>` trong Rust bao gồm các thành phần chính sau:

- **Con trỏ (Pointer)**: Trỏ đến vùng nhớ trên Heap, nơi lưu trữ các phần tử.

- **Dung lượng (Capacity)**: Số lượng phần tử tối đa mà `VecDeque` có thể chứa
  mà không cần cấp phát lại bộ nhớ.

- **Độ dài (Length)**: Số lượng phần tử hiện tại trong `VecDeque`.

- **Chỉ mục đầu (Front Index)**: Vị trí của phần tử đầu tiên trong `VecDeque`.

- **Chỉ mục cuối (Back Index)**: Vị trí của phần tử cuối cùng trong `VecDeque`.

```rust
let  mut  vd  =  VecDeque::from(vec![1, 2, 3]);
println!("length of vd is {}", vd.len());
println!("capacity of vd is {}", vd.capacity());
let  head  =  vd.front().map(|v|  v  as  *const  i32);
println!("head of vd is {:?}", head.unwrap());
let  tail  =  vd.back().map(|v|  v  as  *const  i32);
println!("tail of vd is {:?}", tail.unwrap());
//length of vd is 2
//capacity of vd is 2
//head of vd is 0x1bb9f467fb0
//tail of vd is 0x1bb9f467fb4
```

```rust
vd.pop_front();
vd.push_back(4);
println!("capacity of vd is {}", vd.capacity());
let  head  =  vd.front().map(|v|  v  as  *const  i32);
println!("head of vd is {:?}", head.unwrap());
let  tail  =  vd.back().map(|v|  v  as  *const  i32);
println!("tail of vd is {:?}", tail.unwrap());
//capacity of vd is 4
//head of vd is 0x1f8ded77fbc
//tail of vd is 0x1f8ded77fb4
```

### **2.2. Quản lý bộ nhớ**

- `VecDeque` sử dụng một mảng vòng (circular buffer) để lưu trữ các phần tử, cho
  phép thêm/xóa phần tử ở cả hai đầu một cách hiệu quả.

- Khi số lượng phần tử vượt quá dung lượng hiện tại, `VecDeque` sẽ tự động mở
  rộng bằng cách cấp phát một vùng nhớ mới lớn hơn và sao chép các phần tử sang
  vùng nhớ mới.

---

## **3. Cách VecDeque hoạt động**

### **3.1. Khởi tạo VecDeque**

- **`VecDeque::new()`**: Tạo một `VecDeque` rỗng.

```rust
let  mut  deque: VecDeque<i32> = VecDeque::new();
```

- **`VecDeque::with_capacity(n)`**: Tạo một `VecDeque` với dung lượng ban đầu là
  `n`.

```rust
let  mut  deque = VecDeque::with_capacity(10);
```

### **3.2. Thêm phần tử vào VecDeque**

- **`push_front(value)`**: Thêm một phần tử vào đầu `VecDeque`.

```rust
deque.push_front(1); // VecDeque sau khi thêm: [1]
```

- **`push_back(value)`**: Thêm một phần tử vào cuối `VecDeque`.

```rust
deque.push_back(2); // VecDeque sau khi thêm: [1, 2]
```

- **`extend(iter)`**: Thêm nhiều phần tử.

```rust
dq.extend([30, 40]);
// dq: [5, 10, 20, 30, 40]
```

- **`insert(index,value)`**: Chèn phần tử vào vị trí index

```rust
dq.insert(2, 15); // chèn 15 vào index 2
// dq: [5, 10, 15, 20, 30, 40]
```

### **3.3. Truy xuất phần tử trong VecDeque**

-

- **Chỉ mục (Indexing)**:

```rust
let  first = v[0]; // Truy xuất phần tử đầu tiên
```

- Lưu ý: Truy xuất bằng chỉ mục sẽ panic nếu chỉ mục nằm ngoài phạm vi.

- **`get(index)`**: Trả về `Option<&T>`, an toàn hơn khi truy xuất.

```rust
if  let  Some(value) = v.get(2) {
println!("Phần tử thứ 3 là: {}", value);
}
```

- **`front()`**: Trả về tham chiếu đến phần tử đầu tiên.

```rust
if  let  Some(value) = deque.front() {
println!("Phần tử đầu tiên là: {}", value);
}
```

- **`back()`**: Trả về tham chiếu đến phần tử cuối cùng.

```rust
if  let  Some(value) = deque.back() {
println!("Phần tử cuối cùng là: {}", value);
}
```

### **3.4. Xóa phần tử khỏi VecDeque**

- **`pop_front()`**: Xóa và trả về phần tử đầu tiên.

```rust
let  first = deque.pop_front(); // first = Some(1)
```

- **`pop_back()`**: Xóa và trả về phần tử cuối cùng.

```rust
let  last = deque.pop_back(); // last = Some(2)
```

- **`remove(index)`**: Xóa phần tử tại chỉ mục cụ thể.

```rust
v.remove(1); // Vector sau khi xóa: [1, 3, 4, 5]
```

- **`clear()`**: Xóa toàn bộ phần tử trong Vector.

```rust
v.clear(); // Vector rỗng: []
```

- **`retain()`**: Xóa phần tử theo điều kiện

```rust
let mut v = vec![1, 2, 3, 4, 5];
v.retain(|x| x % 2 == 0); // chỉ giữ số chẵn
// v: [2, 4]
```

- **`swap_remove()`**: Đổi chỗ với phần tử cuối rồi xóa

```rust
let mut v = vec![10, 20, 30];
v.swap_remove(1); // xóa index = 1 (nhưng swap với phần tử cuối)
// v: [10, 30]
```

- Nhanh hơn `remove()` vì không cần shift phần tử phía sau
- Làm thay đổi thứ tự các phần tử

### **3.5. Điều chỉnh kích thước VecDeque**

- **`reserve(n)`**: Đảm bảo đủ bộ nhớ cho `n` phần tử tiếp theo.

```rust
deque.reserve(10); // Đảm bảo dung lượng cho 10 phần tử
```

- **`shrink_to_fit()`**: Thu hồi bộ nhớ thừa.

```rust
deque.shrink_to_fit(); // Giảm dung lượng về bằng độ dài hiện tại
```

### **3.6. Duyệt VecDeque**

- **`for` loop**:

```rust
for  value  in &deque {
println!("{}", value);
}
```

- **`iter()`**: Trả về iterator của các phần tử.

```rust
for (index, value) in  deque.iter().enumerate() {
println!("Phần tử {}: {}", index, value);
}
```

---
## **4. Hiệu suất của VecDeque**



### **4.1. Độ phức tạc thời gian**

-  **Truy xuất phần tử**: O(1) (truy xuất ngẫu nhiên).

-  **Thêm/xóa phần tử ở đầu hoặc cuối**: O(1) trung bình.

-  **Thêm/xóa phần tử ở giữa**: O(n) (do cần dịch chuyển các phần tử còn lại).



### **4.2. Quản lý bộ nhớ**

-  `VecDeque` tự động quản lý bộ nhớ và mở rộng khi cần thiết.

- Khi số lượng phần tử vượt quá dung lượng hiện tại, `VecDeque` sẽ tăng dung lượng lên gấp đôi (hoặc theo một hệ số nhất định) để giảm thiểu số lần cấp phát lại bộ nhớ.
---

## **5. Khi nào nên sử dụng VecDeque?**

### **5.1. Ưu điểm**

- **Thao tác nhanh ở cả hai đầu**: `VecDeque` cung cấp thời gian thêm/xóa phần
  tử ở cả hai đầu là O(1).

- **Linh hoạt**: Có thể sử dụng như một hàng đợi (queue) hoặc ngăn xếp (stack).

- **Tự động quản lý bộ nhớ**: `VecDeque` tự động cấp phát và giải phóng bộ nhớ.

### **5.2. Nhược điểm**

- **Chi phí thêm/xóa phần tử ở giữa**: Việc thêm hoặc xóa phần tử ở giữa
  `VecDeque` có chi phí cao (O(n)) do cần dịch chuyển các phần tử còn lại.

- **Không phù hợp cho các thao tác tìm kiếm**: `VecDeque` không được tối ưu cho
  các thao tác tìm kiếm (cần sử dụng các cấu trúc dữ liệu khác như `HashMap`
  hoặc `BTreeMap`).

### **5.3. Ứng dụng**

- **Hàng đợi (Queue)**: `VecDeque` có thể được sử dụng để triển khai hàng đợi
  với thao tác thêm/xóa phần tử ở cả hai đầu.

- **Ngăn xếp (Stack)**: `VecDeque` có thể được sử dụng để triển khai ngăn xếp
  với thao tác thêm/xóa phần tử ở một đầu.

- **Lưu trữ dữ liệu tuần tự**: Ví dụ, lưu trữ danh sách các số nguyên, chuỗi,
  hoặc đối tượng.

---

## **6. Kết luận**

`VecDeque<T>` (Vector Double-Ended Queue) trong Rust là một cấu trúc dữ liệu
động mạnh mẽ và linh hoạt, phù hợp cho các ứng dụng cần thao tác nhanh ở cả hai
đầu dữ liệu. Với khả năng tự động quản lý bộ nhớ và hiệu suất cao, `VecDeque` là
một công cụ không thể thiếu trong hộp công cụ của lập trình viên Rust. Tuy
nhiên, việc sử dụng `VecDeque` cần cân nhắc dựa trên yêu cầu cụ thể của ứng
dụng, đặc biệt là khi cần thực hiện các thao tác thêm/xóa phần tử ở giữa.

---

# ** Linked List trong Rust**

## **1. Giới thiệu**

**Linked List** (danh sách liên kết) là một cấu trúc dữ liệu động, trong đó các
phần tử được lưu trữ trong các nút (node) và liên kết với nhau thông qua các con
trỏ. Mỗi nút chứa dữ liệu và một con trỏ trỏ đến nút tiếp theo (hoặc cả nút
trước đó trong trường hợp danh sách liên kết đôi). Linked List là một cấu trúc
dữ liệu linh hoạt, phù hợp cho các thao tác thêm/xóa phần tử ở đầu hoặc cuối
danh sách, nhưng có chi phí truy xuất ngẫu nhiên cao.

Trong Rust, Linked List được triển khai thông qua thư viện chuẩn với kiểu
`LinkedList<T>`.

---

## **2. Cấu trúc của Linked List**

### **2.1. Thành phần chính**

Một `LinkedList<T>` trong Rust bao gồm các thành phần chính sau:

- **Node (Nút)**: Mỗi nút chứa:

- **Dữ liệu**: Giá trị của phần tử.

- **Con trỏ**: Trỏ đến nút tiếp theo (và nút trước đó trong trường hợp danh sách
  liên kết đôi).

- **Head (Đầu)**: Con trỏ trỏ đến nút đầu tiên trong danh sách.

- **Tail (Đuôi)**: Con trỏ trỏ đến nút cuối cùng trong danh sách (nếu có).

```rust
let  mut  list:  LinkedList<i32> =  LinkedList::new();
list.push_back(1);
list.push_front(2);
list.push_back(3);
for (i, v) in  list.iter().enumerate() {
println!("address of element {} : {:p}", i, v);
//address of element 0 : 0x1d373a07900
//address of element 1 : 0x1d373a07ac0
//address of element 2 : 0x1d373a07b20
}
```

### **2.2. Quản lý bộ nhớ**

- Linked List sử dụng bộ nhớ động (Heap) để lưu trữ các nút.

- Mỗi nút được cấp phát độc lập, và các nút được liên kết với nhau thông qua con
  trỏ.

---

## **3. Cách Linked List hoạt động**

### **3.1. Khởi tạo Linked List**

- **`LinkedList::new()`**: Tạo một Linked List rỗng.

```rust
use  std::collections::LinkedList;
let  mut  list: LinkedList<i32> = LinkedList::new();
```

### **3.2. Thêm phần tử vào Linked List**

- **`push_front(value)`**: Thêm một phần tử vào đầu danh sách.

```rust
list.push_front(1); // Danh sách sau khi thêm: [1]
```

- **`push_back(value)`**: Thêm một phần tử vào cuối danh sách.

```rust
list.push_back(2); // Danh sách sau khi thêm: [1, 2]
```

### **3.3. Truy xuất phần tử trong Linked List**

- **`front()`**: Trả về tham chiếu đến phần tử đầu tiên.

```rust
if  let  Some(value) = list.front() {
println!("Phần tử đầu tiên là: {}", value);
}
```

- **`back()`**: Trả về tham chiếu đến phần tử cuối cùng.

```rust
if  let  Some(value) = list.back() {
println!("Phần tử cuối cùng là: {}", value);
}
```

### **3.4. Xóa phần tử khỏi Linked List**

- **`pop_front()`**: Xóa và trả về phần tử đầu tiên.

```rust
let  first = list.pop_front(); // first = Some(1)
```

- **`pop_back()`**: Xóa và trả về phần tử cuối cùng.

```rust
let  last = list.pop_back(); // last = Some(2)
```

### **3.5. Duyệt Linked List**

- **`for` loop**:

```rust
for  value  in &list {
println!("{}", value);
}
```

- **`iter()`**: Trả về iterator của các phần tử.

```rust
for (index, value) in  list.iter().enumerate() {
println!("Phần tử {}: {}", index, value);
}
```

---

## **4. Hiệu suất của Linked List**

### **4.1. Độ phức tạc thời gian**

- **Thêm/xóa phần tử ở đầu hoặc cuối**: O(1).

- **Truy xuất phần tử ở đầu hoặc cuối**: O(1).

- **Truy xuất phần tử ở giữa**: O(n) (do cần duyệt từ đầu hoặc cuối danh sách).

- **Thêm/xóa phần tử ở giữa**: O(n) (do cần duyệt đến vị trí cần thêm/xóa).

### **4.2. Quản lý bộ nhớ**

- Linked List sử dụng bộ nhớ động để lưu trữ các nút, do đó có chi phí quản lý
  bộ nhớ cao hơn so với các cấu trúc dữ liệu khác như `Vec<T>`.

- Mỗi nút được cấp phát độc lập, dẫn đến việc truy xuất bộ nhớ không liên tục và
  có thể ảnh hưởng đến hiệu suất do cache miss.

---

## **5. Khi nào nên sử dụng Linked List?**

### **5.1. Ưu điểm**

- **Thao tác nhanh ở đầu và cuối**: Linked List cung cấp thời gian thêm/xóa phần
  tử ở đầu và cuối là O(1).

- **Linh hoạt**: Có thể dễ dàng thêm/xóa phần tử mà không cần dịch chuyển các
  phần tử khác.

- **Không cần cấp phát bộ nhớ liên tục**: Mỗi nút được cấp phát độc lập, phù hợp
  cho các ứng dụng có yêu cầu bộ nhớ động.

### **5.2. Nhược điểm**

- **Truy xuất ngẫu nhiên chậm**: Truy xuất phần tử ở giữa danh sách có chi phí
  cao (O(n)).

- **Chi phí bộ nhớ cao**: Mỗi nút cần lưu trữ thêm con trỏ, dẫn đến chi phí bộ
  nhớ cao hơn so với các cấu trúc dữ liệu khác.

- **Không tận dụng được bộ nhớ đệm (cache)**: Do dữ liệu không được lưu trữ liên
  tục trong bộ nhớ.

### **5.3. Ứng dụng**

- **Hàng đợi (Queue)**: Linked List có thể được sử dụng để triển khai hàng đợi
  với thao tác thêm/xóa phần tử ở cả hai đầu.

- **Ngăn xếp (Stack)**: Linked List có thể được sử dụng để triển khai ngăn xếp
  với thao tác thêm/xóa phần tử ở một đầu.

- **Lưu trữ dữ liệu động**: Ví dụ, lưu trữ danh sách các tác vụ cần thực hiện
  trong hệ điều hành.

---

## **6. Kết luận**

Linked List là một cấu trúc dữ liệu linh hoạt và mạnh mẽ, phù hợp cho các ứng
dụng cần thao tác nhanh ở đầu và cuối danh sách. Tuy nhiên, nó có chi phí truy
xuất ngẫu nhiên cao và chi phí bộ nhớ lớn hơn so với các cấu trúc dữ liệu khác
như `Vec<T>` hoặc `VecDeque<T>`. Việc sử dụng Linked List cần cân nhắc dựa trên
yêu cầu cụ thể của ứng dụng, đặc biệt là khi cần thực hiện các thao tác thêm/xóa
phần tử ở giữa danh sách.

---

# **HashMap trong Rust**

## **1. Giới thiệu**

`HashMap<K, V>` là một cấu trúc dữ liệu ánh xạ khóa-giá trị (key-value) trong
Rust, được triển khai dựa trên bảng băm (hash table). Nó cho phép truy xuất dữ
liệu nhanh chóng với độ phức tạp trung bình O(1) cho các thao tác chèn, truy vấn
và xóa. `HashMap` là một công cụ mạnh mẽ và linh hoạt, được sử dụng rộng rãi
trong các ứng dụng cần hiệu suất cao.

---

## **2. Cấu trúc của HashMap**

### **2.1. Thành phần chính**

Một `HashMap<K, V>` trong Rust bao gồm các thành phần chính sau:

- **Buckets**: Một mảng chứa các nhóm phần tử, mỗi bucket có thể chứa một hoặc
  nhiều phần tử (trong trường hợp xảy ra va chạm băm - collision)

- **Hàm băm (Hash Function)**: Sử dụng SipHash mặc định để ánh xạ khóa vào các
  bucket

- **Con trỏ (Pointer)**: Trỏ đến vùng nhớ trên Heap nơi lưu trữ dữ liệu

- **Load Factor**: Tỷ lệ giữa số phần tử và số bucket, quyết định khi nào cần mở
  rộng bảng băm.

```rust
let  mut  map:  HashMap<&str, i32> =  HashMap::new();
map.insert("Alice", 25);
map.insert("Bob", 24);
map.insert("Charlie", 27);
println!("capacity {:?}", map.capacity());
println!("hash of Alice: {}", hash(&"Alice"));
for (key, value) in  &map {
let  addr  =  ptr::addr_of!(*value);
println!("Key: '{}' (hash = {:x}) in address: {:p}", key, hash(key), addr);
}
//capacity 3
//hash of Alice: 12903710109527323087
//Key: 'Bob' (hash = 9b3d9114561f9914) in address: 0x283d64ff548
//Key: 'Charlie' (hash = 418e388912d06716) in address: 0x283d64ff530
//Key: 'Alice' (hash = b3132ffa52fe31cf) in address: 0x283d64ff500
```

Trong HashMap thì kiểu **K** bắt buộc phải được implement các **Trait**: `Eq`,
`Hash`,`PartialEq`

- `Hash`: Xác định cách băm giá trị.

- `Eq` và `PartialEq`: Đảm bảo khóa hoạt động đúng khi so sánh.

```rust
#[derive(Debug, Hash, Eq, PartialEq)]
struct CustomKey {
    id: u32,
    name: String,
}
fn  main() {
	let  mut  map:  HashMap<CustomKey, String> =  HashMap::new();
	let  key1  =  CustomKey { id:  1, name:  "Alice".to_string() };
	let  key2  =  CustomKey { id:  2, name:  "Bob".to_string() };
	
	// Chèn dữ liệu vào HashMap
	map.insert(key1, "Engineer".to_string());
	map.insert(key2, "Doctor".to_string());
	println!("map capacity: {:?}", map.capacity());
	// Kiểm tra truy xuất dữ liệu
	let  key_lookup  =  CustomKey { id:  1, name:  "Alice".to_string() };
	if  let  Some(job) =  map.get(&key_lookup) {
		println!("Alice's job: {}", job);
	} else {
		println!("Alice not found");
	}
}
//map capacity: 3
//Alice's job: Engineer
```

### **2.2. Quản lý bộ nhớ**

- HashMap tự động quản lý bộ nhớ bằng cách cấp phát động trên Heap

- Khi số lượng phần tử vượt quá ngưỡng load factor, HashMap sẽ:

1. Tạo bảng băm mới với kích thước lớn hơn.

2. Tái băm (rehash) tất cả các phần tử sang bảng mới.

3. Giải phóng bộ nhớ của bảng cũ.

```rust
map.insert("David".to_string(), 45);
map.insert("Eve".to_string(), 46);
println!("map capacity: {:?}", map.capacity());
for (key, _) in  &map {
	println!(
	"Key: {:<8} | Address of key object: {:p} | Address of key 					   data{:p}",key,key, key.as_ptr()
);
}
```

---
## **3. Cách HashMap hoạt động**


### **3.1. Khởi tạo HashMap**

-  **`HashMap::new()`**: Tạo HashMap rỗng

```rust
use  std::collections::HashMap;
let  mut  map: HashMap<String, i32> = HashMap::new();
```

-  **`HashMap::with_capacity(n)`**: Tạo HashMap với dung lượng ban đầu

```rust
let  mut  map = HashMap::with_capacity(100);
```

### **3.2. Thêm phần tử vào HashMap**

-  **`insert(key, value)`**: Thêm cặp key-value

```rust
map.insert("Alice".to_string(), 100);
```

-  **`entry().or_insert()`**: Thêm nếu key chưa tồn tại

```rust
map.entry("Bob".to_string()).or_insert(90);
```



### **3.3. Truy xuất phần tử**

-  **`get(&key)`**: Trả về `Option<&V>`

```rust
if  let  Some(score) = map.get("Alice") {
println!("Score: {}", score);
}
```

-  **`contains_key(&key)`**: Kiểm tra tồn tại

```rust
println!("Contains Alice? {}", map.contains_key("Alice"));
```



### **3.4. Xóa phần tử**

-  **`remove(&key)`**: Xóa theo key

```rust
map.remove("Alice");
```

-  **`retain(|k, v| condition)`**: Xóa có điều kiện

```rust
map.retain(|_, score| *score > 50);
```



### **3.5. Duyệt HashMap**

- Duyệt qua tất cả cặp key-value:

```rust
for (name, score) in &map {
println!("{}: {}", name, score);
}
```

- Duyệt chỉ keys hoặc values:
```rust
for  name  in  map.keys() {
println!("Name: {}", name);
}
```
---

## **4. Hiệu suất của HashMap**

### **4.1. Độ phức tạc thời gian**

| Thao tác | Độ phức tạp |

|----------|-------------|

| `insert()` | O(1) trung bình |

| `get()` | O(1) trung bình |

| `remove()` | O(1) trung bình |

| Duyệt | O(n) |

### **4.2. Quản lý bộ nhớ**

- Tự động điều chỉnh kích thước khi cần

- Có thể dự đoán trước dung lượng với `with_capacity()`

- Giảm bộ nhớ thừa bằng `shrink_to_fit()`

---

## **5. Khi nào nên sử dụng HashMap?**

### **5.1. Ưu điểm**

- Truy xuất cực nhanh với O(1) trung bình

- Linh hoạt với nhiều kiểu dữ liệu làm key

- Tự động mở rộng khi cần thiết

### **5.2. Nhược điểm**

- Không duy trì thứ tự các phần tử

- Chi phí bộ nhớ cao hơn so với Vector

- Key phải triển khai `Hash` và `Eq`

### **5.3. Ứng dụng điển hình**

- Lưu trữ cấu hình (key-value)

- Đếm tần suất xuất hiện

- Xây dựng cache đơn giản

- Từ điển, bảng ánh xạ

---

## **6. Kết luận**

`HashMap<K, V>` là một trong những cấu trúc dữ liệu quan trọng nhất trong Rust,
cung cấp khả năng truy xuất dữ liệu nhanh chóng thông qua cơ chế băm. Với hiệu
suất cao và tính linh hoạt, HashMap là lựa chọn lý tưởng cho các bài toán cần
ánh xạ khóa-giá trị. Tuy nhiên, cần lưu ý về yêu cầu bộ nhớ và việc không duy
trì thứ tự các phần tử khi sử dụng.

# **HashSet trong Rust**

## **1. Giới thiệu**

`HashSet<T>` là một cấu trúc dữ liệu trong Rust, được triển khai dựa trên bảng
băm (hash table). Nó lưu trữ các phần tử duy nhất (không trùng lặp) và cung cấp
các thao tác kiểm tra, thêm, xóa phần tử với độ phức tạc thời gian trung bình
O(1). `HashSet` là một công cụ mạnh mẽ và linh hoạt, được sử dụng rộng rãi trong
các ứng dụng cần kiểm tra sự tồn tại của phần tử một cách nhanh chóng.

---

## **2. Cấu trúc của HashSet**

### **2.1. Thành phần chính**

Một `HashSet<T>` trong Rust bao gồm các thành phần chính sau:

- **Bảng băm (Hash Table)**: Một mảng chứa các bucket, mỗi bucket lưu trữ các
  phần tử.

- **Hàm băm (Hash Function)**: Dùng để ánh xạ các phần tử vào các bucket tương
  ứng.

- **Con trỏ (Pointer)**: Trỏ đến vùng nhớ trên Heap, nơi lưu trữ các phần tử.

### **2.2. Quản lý bộ nhớ**

- `HashSet` tự động quản lý bộ nhớ bằng cách cấp phát động trên Heap.

- Khi số lượng phần tử vượt quá dung lượng hiện tại, `HashSet` sẽ tự động mở
  rộng bằng cách cấp phát một vùng nhớ mới lớn hơn và tái băm (rehash) các phần
  tử.

---

## **3. Cách HashSet hoạt động**

### **3.1. Khởi tạo HashSet**

- **`HashSet::new()`**: Tạo một `HashSet` rỗng.

```rust
use  std::collections::HashSet;
let  mut  set: HashSet<i32> = HashSet::new();
```

- **`HashSet::with_capacity(n)`**: Tạo một `HashSet` với dung lượng ban đầu là
  `n`.

```rust
let  mut  set = HashSet::with_capacity(10);
```

### **3.2. Thêm phần tử vào HashSet**

- **`insert(value)`**: Thêm một phần tử vào `HashSet`. Nếu phần tử đã tồn tại,
  phương thức trả về `false`; ngược lại, trả về `true`.

```rust
set.insert(1); // Thêm phần tử 1
set.insert(2); // Thêm phần tử 2
```

### **3.3. Kiểm tra phần tử trong HashSet**

- **`contains(&value)`**: Kiểm tra xem phần tử có tồn tại trong `HashSet` không.

```rust
if  set.contains(&1) {
println!("Phần tử 1 tồn tại trong HashSet.");
}
```

### **3.4. Xóa phần tử khỏi HashSet**

- **`remove(&value)`**: Xóa phần tử khỏi `HashSet`. Nếu phần tử tồn tại, phương
  thức trả về `true`; ngược lại, trả về `false`.

```rust
set.remove(&1); // Xóa phần tử 1
```

### **3.5. Duyệt HashSet**

- **`for` loop**:

```rust
for  value  in &set {
println!("{}", value);
}
```

- **`iter()`**: Trả về iterator của các phần tử.

```rust
for  value  in  set.iter() {
println!("{}", value);
}
```

### **3.6. Các thao tác tập hợp**

- **`union()`**: Trả về hợp của hai `HashSet`.

```rust
let  set1: HashSet<_> = [1, 2, 3].iter().cloned().collect();
let  set2: HashSet<_> = [3, 4, 5].iter().cloned().collect();
let  union: HashSet<_> = set1.union(&set2).cloned().collect();
```

- **`intersection()`**: Trả về giao của hai `HashSet`.

```rust
let  intersection: HashSet<_> = set1.intersection(&set2).cloned().collect();
```

- **`difference()`**: Trả về hiệu của hai `HashSet`.

```rust
let  difference: HashSet<_> = set1.difference(&set2).cloned().collect();
```

---

## **4. Hiệu suất của HashSet**

### **4.1. Độ phức tạc thời gian**

- **Thêm phần tử**: O(1) trung bình, O(n) trong trường hợp xấu nhất (khi xảy ra
  nhiều xung đột).

- **Kiểm tra phần tử**: O(1) trung bình, O(n) trong trường hợp xấu nhất.

- **Xóa phần tử**: O(1) trung bình, O(n) trong trường hợp xấu nhất.

### **4.2. Quản lý bộ nhớ**

- `HashSet` tự động quản lý bộ nhớ và mở rộng khi cần thiết.

- Khi số lượng phần tử vượt quá dung lượng hiện tại, `HashSet` sẽ tăng dung
  lượng lên gấp đôi (hoặc theo một hệ số nhất định) để giảm thiểu số lần cấp
  phát lại bộ nhớ.

---

## **5. Khi nào nên sử dụng HashSet?**

### **5.1. Ưu điểm**

- **Truy xuất nhanh**: `HashSet` cung cấp thời gian truy xuất trung bình O(1),
  phù hợp cho các ứng dụng cần kiểm tra sự tồn tại của phần tử một cách nhanh
  chóng.

- **Lưu trữ phần tử duy nhất**: `HashSet` đảm bảo các phần tử là duy nhất, không
  trùng lặp.

- **Linh hoạt**: Có thể lưu trữ bất kỳ kiểu dữ liệu nào triển khai trait `Hash`
  và `Eq`.

### **5.2. Nhược điểm**

- **Không duy trì thứ tự**: `HashSet` không duy trì thứ tự các phần tử.

- **Chi phí bộ nhớ**: `HashSet` yêu cầu bộ nhớ lớn hơn so với các cấu trúc dữ
  liệu khác như `Vec<T>` hoặc `BTreeSet<T>`.

### **5.3. Ứng dụng**

- **Kiểm tra sự tồn tại**: Ví dụ, kiểm tra xem một phần tử có tồn tại trong tập
  hợp không.

- **Loại bỏ phần tử trùng lặp**: Ví dụ, loại bỏ các phần tử trùng lặp từ một
  danh sách.

- **Thao tác tập hợp**: Ví dụ, tính hợp, giao, hiệu của các tập hợp.

---

## **6. Kết luận**

`HashSet<T>` trong Rust là một cấu trúc dữ liệu mạnh mẽ và linh hoạt, phù hợp
cho các ứng dụng cần kiểm tra sự tồn tại của phần tử một cách nhanh chóng. Với
khả năng tự động quản lý bộ nhớ và hiệu suất cao, `HashSet` là một công cụ không
thể thiếu trong hộp công cụ của lập trình viên Rust. Tuy nhiên, việc sử dụng
`HashSet` cần cân nhắc dựa trên yêu cầu cụ thể của ứng dụng, đặc biệt là khi cần
duy trì thứ tự các phần tử.

Dưới đây là một bài báo cáo hoàn chỉnh về **BTreeMap** trong Rust, được tổ chức
theo cấu trúc rõ ràng và chi tiết. Bạn có thể sử dụng nó để thuyết trình hoặc
viết tài liệu.
