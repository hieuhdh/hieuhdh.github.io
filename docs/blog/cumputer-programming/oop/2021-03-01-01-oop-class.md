---
template: overrides/blog.html
icon: material/table-edit
title: Basic concepts of Object-oriented programming
description: >
  Một vài khái niệm cơ bản đối với phương pháp lập trình hướng đối tượng được thể hiện trong ngôn ngữ lập trình C++
search:
  exclude: true
hide:
  - feedback

tags:
  - OOP
  - C++
---

# __Khái niệm cơ bản về OOP trong C++__

<span>
:octicons-calendar-24: Mar 01, 2021 ·
:octicons-clock-24: ~5 minutes

</span>

---

## **Thông tin tổng quan**

### __Lớp đối tượng__

#### __Định nghĩa__

Các đối tượng có đặc tính tương tự nhau được gom vào cùng 1 lớp đối tượng. Hay nói cách khác, lớp là một mô tả trừu tượng của nhóm các đối tượng có cùng bản chất. 

Lớp là cái ta thiết kế để lập trình, đối tượng là cái ta tạo ra từ một lớp tại thời gian chạy.

Một lớp đối tượng được thể hiện bằng từ khóa `class` đặt trước tên lớp đối tượng và một lớp đối tượng được thể hiện bằng các thuộc tính và thao tác (hay còn gọi là hành động).

* Thuộc tính (Attribute): Một thành phần của đối tượng, có giá trị nhất định cho mỗi đối tượng tại mỗi thời điểm trong hệ thống.
* Thao tác (Operation): Thể hiện hành vi của một đối tượng tác động qua lại với các đối tượng khác hoặc ở trạng thái nội tác động (tức tác động với chính nó).
* Mỗi thao tác trên một lớp đối tượng cụ thể được thể hiện tương ứng với một cài đặt cụ thể khác nhau trên lớp đối tượng. Một cài đặt cụ thể đó được gọi là phương thức (method).
* Cùng một phương thức có thể được thể hiện cho nhiều loại đối tượng khác nhau ví dụ cả người và chó (trong trạng thái bình thường) đều có phương thức di chuyển nhưng con người sẽ di chuyển bằng 2 chân còn đối với chó sẽ di chuyển bằng 4 chân. Và vấn đề sinh ra nhiều thể hiện như vậy đối với 1 phương thức, người ta gọi đó là đa hình (polymorphism).
* Một đối tượng cụ thể trong một lớp được gọi là một thể hiện (instance) của lớp đó.

Ta cần phân biệt rõ ràng giữa `đối tượng` và `lớp đối tượng`

#### __Sơ đồ các lớp đối tượng__

Để thể hiện mối quan hệ giữa các đối tượng trong thế giới thực, ta cần phải có một sơ đồ thể hiện mối qua hệ đó. Ví dụ: giữa đối tượng cha và đối tượng con có quan hệ kế thừa,...

Thông thường, 1 lớp đối tượng được thể hiện bởi 1 hình chữ nhật gồm ba phần:
* Phần trên cùng (tức phần đầu): Thể hiện tên lớp đối tượng.
* Phần giữa (tức phần hai): Thể hiện các thuộc tính của đối tượng đó.
* Phần cuối cùng (tức phân ba): Thể hiện các thao tác của đối tượng đó.

Có thể dùng nền tảng <a class = "link_for_hover" href="https://app.diagrams.net/">diagram</a> để thể hiện sơ đồ lớp một cách hiệu quả.

### __Cú pháp khai báo lớp đối tượng__

Ta khai báo một lớp đối tượng đơn thức với các thuộc tính là hệ số (kiểu số nguyên, trạng thái không công khai), số mũ (kiểu số tự nhiên, trạng thái không công khai) và các phương thức nhập (kiểu void, trạng thái công khai), xuất (kiểu void, trạng thái công khai) như sau.

<div class="result" markdown>

``` c++ linenums="1"
class DonThuc{
private:
    int _heso;
    unsigned int _somu;
public:
    DonThuc();
    ~DonThuc();
    void nhap();
    void xuat();
};
```

</div>

### __Phạm vi truy xuất của các thuộc tính, phương thức trong 1 lớp đối tượng__

Trong lập trình hướng đối tượng được thể hiện bằng ngôn ngữ C++, có 3 phạm vi mà thuộc tính và phương thức trong 1 lớp đối tượng có thể nắm giữ:

* Phạm vi truy xuất private: Những phương thức và thuộc tính trong phạm vi này chỉ được quyền truy xuất bên trong nội tại lớp đối tượng đó hoặc truy xuất thông qua hàm bạn, lớp bạn.
* Phạm vi truy xuất protected: Những phương thức và thuộc tính trong phạm vi này được quyền truy xuất nhờ vào lớp dẫn xuất (tức có sự kế thừa) ứng với lớp đang xét và truy cập thông qua hàm bạn, lớp bạn.
* Phạm vi truy xuất public: Những phương thức và thuộc tính trong phạm vi này được quyền truy xuất tại bất kì hàm nào trong chương trình (kể cả trường hợp phân module thư mục).

### __Hàm bạn, lớp bạn__

#### __Hàm bạn__

Hàm bạn của một lớp đối tượng `A` là một hàm không thuộc lớp đối tượng `A` nhưng có thể truy cập đến các thành viên có phạm vi truy cập private, protected của lớp đối tượng `A`.

Bằng việc gán nhãn `friend` trước 1 hàm trong một lớp đối tượng cho trước. Hàm đó sẽ trở thành hàm bạn của lớp đó.

Một lớp đối tượng bất kì có thể có nhiều hàm bạn.

Hàm bạn giúp ta có thể kiểm soát được các truy nhập ở cấp độ lớp - không thể áp đặt hàm bạn cho lớp nếu điều đó không được dự trù trước trong khai báo của lớp.

Về mặc ngữ nghĩa, hàm bạn hiện đang vi phạm tính đóng gói đối tượng nhưng lại góp phần to lớn cho việc đa năng hóa toán tử (tôi sẽ đề cập ở phần đa hình) và những cài đặt quan hệ khác trên cấp độ lớn.

#### __Lớp bạn__

Một lớp đối tượng `A` được gọi là lớp bạn với lớp đối tượng `B` khi và chỉ khi lớp đối tượng `A` có thể truy cập đến các thành phần có thuộc tính private của lớp đối tượng `B`. Hãy xem xét đoạn mã sau

<div class="result" markdown>

``` c++ linenums="1"
class A{
private:
    int a = 9;
public:
    friend class B;
};

class B{
public:
    void action(A x){
        x.a ++;
        cout << x.a;
    }
};
```

</div>

Đối với đoạn mã trên, vì trong lớp đối tượng `A` ta đã khai báo `A` có bạn là lớp đối tượng `B`, do đó trong hàm `action()` của lớp đối tượng `B` có thể truy xuất được thành phần `a` (ở phạm vi truy xuất private) trong lớp đối tượng `A`.

Cũng như hàm bạn, lớp bạn cũng sẽ vi phạm tính đóng gói dữ liệu trong phương pháp lập trình hướng đối tượng được thể hiện bằng ngôn ngữ lập trình C++.

### __Contructor__

#### __Sơ lược__

__Contructor__ là một phương thức đặc biệt dùng để khởi tạo thể hiện của đối tượng.

Về mặt logic, một lớp đối tượng có thể có nhiều contructor (thậm chí nên cân nhắc đến trường hợp vô hạn contructor).

Bất kì một đối tượng nào được khai báo đều phải sử dụng một hàm thiết lập để khởi tạo các giá trị thành phần của đối tượng.

Hàm thiết lập được khai báo giống như một phương thức với tên `trùng với tên lớp đối tượng` và `không có kiểu dữ liệu trả về` (bên trên ta có DonThuc() là một contructor).

Contructor có thể khai báo chồng, có thể khai báo với tham số có giá trị ngầm định như một hàm thông thường. 

Ngoài ra, thông thường chúng ta code C++ không cần phải thiết lập contructor cho 1 lớp đối tượng nào đó nhưng trình biên dịch vẫn chạy bình thường. Điều này không có nghĩa là lớp đối tượng đó không cần contructor mà là nếu chúng ta không thiết lập contructor thì trình biên dịch sẽ tự động sinh ra contructor mặc định cho lớp đối tượng đó để đảm bảo chương trình vẫn chạy bình thường.

Ví dụ đoạn chương trình bên dưới gồm 2 contructor cho cùng 1 lớp đối tượng DonThuc.

<div class="result" markdown>

``` c++ linenums="1"
class DonThuc{
private:
    int _heso;
    unsigned int _somu;
public:
    DonThuc();

    DonThuc(int _heso, unsigned int _somu){
        this -> _heso = _heso;
        this -> _somu = _somu;
    }

    ~DonThuc();
    void nhap();
    void xuat();
};
```

</div>

Về mặc nguyên tắc, contructor phải có phạm vi truy xuất public. Nhưng tùy thuộc vào trình biên dịch sau này, contructor có thể có phạm vi private, protected. Tuy nhiên, khi contructor ở phạm vi private thì điều này đồng nghĩa với việc đối tượng này sẽ không được khởi tạo ở trong bất kì hàm thành phần nào khác (kể cả hàm main()).

#### __Contructor mặc định__

Contructor mặc định là contructor không có bất kì loại tham số nào truyền vào (kể cả tham số mặc nhiên).

Ở ví dụ trên, ta có:

* DonThuc() là 1 contructor mặc định (default contructor) vì đây là 1 contructor không có tham số truyền vào.
* DonThuc(int _heso, unsigned int _somu) không là 1 contructor mặc định.

<div class="result" markdown>

???+ note "__Lưu ý__"

    Một lớp đối tượng nếu ta không thiết lập bất kì contructor nào cho lớp đối tượng đó thì trình biên dịch sẽ tự sinh ra contructor mặc định (đã đề cập ở trên). Nếu ta định nghĩa 1 contructor có tham số truyền vào (tức không phải contructor mặc định) cho 1 lớp thì bắt buộc ta phải định nghĩa 1 __contructor mặc định__ cho lớp đó trước, nếu không thì trình biên dịch sẽ báo lỗi không tìm thấy contructor mặc định.

</div>

  [type qualifier]: #supported-types

#### __Contructor sao chép__

Tình huống giả định: Ta đang có một lớp đối tượng A cùng với một số phương thức và thuộc tính. Ta muốn tạo thêm lớp đối tượng B có một vài phương thức và thuộc tính y hệt lớp đối tượng A. Câu hỏi đặt ra là làm cách nào để tạo lớp đối tượng B sao cho tối ưu nhất?

Với tình huống trên, việc xây dựng contructor sao chép là hoàn toàn thích hợp.

Contructor sao chép là phương thức thiết lập có tham số là tham chiếu đến đối tượng thuộc chính lớp này.

Ta có thể tham khảo thêm ví dụ về hàm khởi tạo sao chép tại 

<a class = "link_for_hover" href="https://www.geeksforgeeks.org/copy-constructor-in-cpp/">https://www.geeksforgeeks.org/copy-constructor-in-cpp/</a>

### __Destructor__

Một lớp đối tượng khi được sinh ra sẽ phải tốn 1 vùng nhớ để lưu trữ đối tượng đó. Câu hỏi đặt ra là làm thế nào để có thể giải phóng toàn bộ vùng nhớ cho đối tượng đó khi kết thúc chương trình?

Để làm được điều trên, ta cần dùng đến phương thức gọi là destructor.

#### __Sơ lược__

Destuctor là phương thức hủy của một lớp đối tượng và chỉ được gọi trước khi đối tượng đó bị hủy.

Một lớp đối tượng chỉ được có một destructor duy nhất (khác với contructor).

Destructor của một lớp có tên trùng với tên lớp đó nhưng không có kiểu dữ liệu trả về cũng như đối số truyền vào mà thay vào đó sẽ có kí hiệu `~` trước destructor.

Destructor sẽ được tự động gọi khi ta xóa đối tượng hoặc khi đối tượng hết phạm vi sử dụng.

Về mặc nguyên tắc, destructor phải có phạm vi truy cập public.

## __Các đặc điểm quan trọng của phương pháp lập trình hướng đối tượng__

Phương pháp lập trình hướng đối tượng có 4 tính chất khá là quan trọng:

* Tính trừu tượng (Abstraction): Trừu tượng hóa các đối tượng ở thực tại vào lớp đối tượng.
* Tính đóng gói (Encapsulation): Đóng gói các thuộc tính của lớp đối tượng nhằm che giấu thông tin của mỗi đối tượng đó.
* Tính kế thừa (Inheritance): Thể hiện sự kế thừa giữa các lớp đối tượng được thể hiện trong máy tính (mỗi sự kế thừa trong máy tính tương ứng với một ánh xạ của sự kế thừa các đối tượng trong thế giới thực).
* Tính đa hình (Polymorphism): Thể hiện việc sử dụng một thao tác cho nhiều lớp đối tượng.

Với mỗi tính chất trên, ta cần phải hiểu rõ và nắm thật rõ. Có rất nhiều thứ để khai thác cũng như khá nhiều vấn đề rất rối sinh ra đối với mỗi tính chất trên.

## __Tham khảo thêm__

[:octicons-arrow-right-24: Xem thêm tính trừu tượng][Abstraction]

[:octicons-arrow-right-24: Xem thêm tính đóng gói][Encapsulation]

[:octicons-arrow-right-24: Xem thêm tính kế thừa][Inheritance]

[:octicons-arrow-right-24: Xem thêm tính đa hình][Polymorphism]

  [Abstraction]: https://en.wikipedia.org/wiki/Abstraction_principle_(computer_programming)
  [Encapsulation]: https://en.wikipedia.org/wiki/Encapsulation_(computer_programming)
  [Inheritance]: https://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming)
  [Polymorphism]: https://en.wikipedia.org/wiki/Polymorphism_(computer_science)