---
title: "Các bước để giải 1 bài thuật toán và ví dụ"
categories:
  - algorithm
tags:
  - algorithm
  - python
---
Bài này là những gì rút ra sau khi học khóa [Algorithmic Toolbox trên coursera](https://www.coursera.org/learn/algorithmic-toolbox/)

Hãy cùng xem khi nhận được 1 đề bài thì chúng ta nên làm những bước gì nào.

## 1. Đề bài

Cho 1 dãy số nguyên không âm 0 ≤ a ≤ 2.$10^5$ độ dài 2 ≤ n ≤ 2.$10^5$ (dãy không phải chỉ có những số giống nhau)
Hợp lệ:
* `1 3 1 4 5`
* `2 3`

Không hợp lệ
* `-1 2 3`
* `2 2`

Tìm tích lớn nhất giữa 2 số khác nhau trong dãy

**Sample:**
Input:
```
1 2 3
```
Output:
```
6
```

**Time limits (sec):**
| C | C++ | Java | Python | C# | Haskell | JavaScript | Kotlin | Ruby | Rust | Scala |
|:-:|:---:|:----:|:------:|:--:|:-------:|:----------:|:------:|:----:|:----:|:-----:|
| 1 | 1   | 1.5  | 5      | 1.5| 2       | 5          | 1.5    | 5    | 1    | 3     |
**Memory limit. 512 Mb.**

## 2. Các bước giải
### 2.1. Đọc hiểu kĩ đề bài
>Đọc hiểu kĩ đề bài cho mình giải quyết vấn đề gì, giới hạn thời gian và bộ nhớ khi tính toán và cả sample case.
>
Đối với đề bài trên thì có những điểm cần lưu ý như: `số nguyên không âm`, `độ lớn của số`, `độ dài list`, `validate số trong dãy không được giống nhau toàn bộ`.

### 2.2. Thiết kế thuật toán
>Sau khi vò đầu bức tai một hồi ra thuật toán thì phải ước tính xem thời gian chạy dự kiến của thuật toán đó với đầu vào phức tạp nhất.
>Đối với máy tính xách tay thì thực hiện khoảng $10^8$ đến $10^9$ phép tính trong 1 giây.
>Trong ví dụ trên n = $10^5$ nên với thuật toán O($n^2$) sẽ không được nhưng với giải pháp O(n.log(n)) thì lại được. Tuy nhiên giải thuật O($n^2$) thì lại chạy ổn với n <= 1000, cho nên phải chú ý đến độ phức tạp và time limits của đề bài.

Đề này thì đọc vào là ra thuật toán luôn rồi, lấy từng số trong dãy nhân với những số còn lại (check khác nhau) rồi lấy ra tích lớn nhất trong đó là xong, quá dễ. Với tối kiến đó sẽ có code như sau:
```
max_pairwise_product(list):
  n = len(list)
  product = 0
  for i from 1 to n:
    for j from i + 1 to n:
      if product < A[i] * A[j]:
        product = A[i] * A[j]
  return product
```
Ngon rồi thôi nhảy qua bước 3 code rồi Submit bài làm phát --> failed: time limit exceeded
Thuật toán thì đúng rồi, nhưng mà chậm quá, rất tiếc.

Do với số lượng lớn nhất của list là 2.$10^5$ phần tử thì với thuật toán độ phức tạp O($n^2$) như trên sẽ cần tới 4.$10^{10}$ phép tính. Mà máy tính xách tay chỉ có thể tính khoảng $10^8$ đến $10^9$ phép tính trong 1 giây cho nên thuật toán trên không thể qua được **time limits**

Do vậy chúng ta cần tìm giải pháp tối ưu hơn cho bài này, cụ thể hơn là O(n). Phần này tôi sẽ không viết trong mục này nữa mà sẽ viết ở mục dưới, nếu ai đọc thì hãy dừng ở đây để tự viết cho mình đã nhé.

### 2.3. Viết code
>Implement thuật toán của bạn bằng một trong các ngôn ngữ lập trình.

Đây là code với giải thuật O($n^2$) failed khi nãy.
```
# !python3

def naive(input_list):
    length = len(input_list)
    product = 0
    for i in range(length):
        for j in range(i + 1, length):
            if input_list[i] != input_list[j]:
                product = max(product, input_list[i] * input_list[j])
    return product
```

Như đã nói ở trên thì với đề bài này có thể viết được giải thuật O(n), tức là chỉ duyệt mảng 1 lần duy nhất là ra được. Và nghĩ lại 1 tí thì sao cứ nhất thiết phải nhân hết lại rồi lấy ra số lớn nhất trong đó, tốn thời gian tính toán vãi ra. Tìm 2 số lớn nhất và lớn nhì trong list xong nhân lại là ra kết quả rồi. Và vậy sẽ có code như sau:

```
# !python3

def maximum_pairwise(input_list):
    first_number = float("-inf")
    second_number = float("-inf")
    for number in input_list:
        if number > first_number:
            second_number = first_number
            first_number = number
        elif first_number > number > second_number:
            second_number = number
    return first_number * second_number
```

### 2.4. Testing và Debugging
>Gửi bài lên hệ thống trước khi test thì không nên. Trước tiên phải test chương trình bằng một số data nhỏ xem giải thuật chạy đúng chưa, sau đó nên thử với một số data lớn hơn (viết hàm generate) để check xem thuật toán có đủ nhanh không.
>Sau đó thì cũng nên test với những trường hợp đặc biệt như list 2 phần tử, list toàn những số giống nhau,...
>Khi đã qua hết những cách test trên thì hãy dùng đến phương pháp stress testing. Cách hoạt động của phương pháp này là tạo tự động một dãy số ngẫu nhiên, chạy thuật toán naive (thuật toán failed time limits nhưng chắc chắn đúng) và thuật toán của mình. So sánh 2 kết quả và chạy đến khi nào 2 kết quả khác nhau thì dừng lại. Sau 1 thời gian chạy mà chương trình không dừng thì có nghĩa là thuật toán khá chuẩn rồi.

Đây là ví dụ về **Stress tessting**

```
from random import randrange

def stress_test(max_length=100, max_number=2000):
    while 1:
        length = randrange(2, max_length + 1)
        array = []
        for _ in range(0, length):
            array.append(randrange(0, max_number + 1))

        print(array)

        result1 = naive(length, array)
        result2 = maximum_pairwise(array)

        if result1 == result2:
            print("OK")
        else:
            print("Wrong answer: ", result1, result2)
            return
```

### 2.5. Submit kết quả
>Sau khi submit kết quả thì có thể nhận lại là "Thành công" hoặc time limits,... Còn nếu lỗi do sai một vài test case thì phải đi mò thôi, vì hệ thống thi thường sẽ chả biết fail với dữ liệu nào cả.

## Tất cả những kiến thức trình bày trên đây thuộc khóa học [Algorithmic Toolbox trên coursera](https://www.coursera.org/learn/algorithmic-toolbox/)