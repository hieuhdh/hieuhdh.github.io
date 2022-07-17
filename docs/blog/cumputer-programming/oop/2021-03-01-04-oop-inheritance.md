---
template: overrides/blog.html
icon: material/table-edit
title: Inheritance
description: >
  Một vài điều về tính kế thừa trong phương pháp lập trình hướng đối tượng được thể hiện ở ngôn ngữ lập trình C++
search:
  exclude: true
hide:
  - feedback

tags:
  - OOP
  - C++
---

# __Tính kế thừa trong lập trình hướng đối tượng__

<span>
:octicons-calendar-24: Mar 01, 2021 ·
:octicons-clock-24: ~5 minutes

</span>

---

## __Sơ lược về kế thừa__

### __Mối quan hệ giữa hai lớp đối tượng__

Quan hệ 1-1: Hai lớp đối tượng được gọi là có quan hệ một-một với nhau khi một đối tượng thuộc lớp này quan hệ với một đối tượng thuộc lớp kia và một đối tượng thuộc lớp kia có quan hệ duy nhất với một đối tượng thuộc lớp này. Ví dụ một giáo viên chỉ chủ nhiệm 1 lớp và 1 lớp chỉ có 1 giáo viên chủ nhiệm.

Quan hệ 1-n: Hai lớp đối tượng được gọi là có quan hệ một-nhiều với nhau khi một đối tượng thuộc lớp này quan hệ với nhiều đối tượng thuộc lớp kia và một đối tượng thuộc lớp kia có quan hệ duy nhất với một đối tượng thuộc lớp này. Ví dụ 1 môn học có nhiều giáo viên bộ môn giảng dạy và 1 giáo viên bộ môn chỉ dạy được 1 môn học duy nhất.

Quan hệ n-n: Hai lớp đối tượng được gọi là có quan hệ một-nhiều với nhau khi một đối tượng thuộc lớp này quan hệ với nhiều đối tượng thuộc lớp kia và một đối tượng thuộc lớp kia có quan hệ với nhiều đối tượng thuộc lớp này. Ví dụ 1 giáo viên giảng dạy nhiều học sinh và 1 học sinh có thể được dạy bởi nhiều giáo viên.

Quan hệ is-a (Quan hệ đặc biệt hóa - tổng quát hóa): Hai lớp đối tượng được gọi là có quan hệ `is-a` khi và chỉ khi lớp đối tượng này là trường hợp đặc biệt của lớp đối tượng kia và lớp đối tượng kia là trường hợp tổng quát của lớp đối tượng này. Ví dụ: Hình vuông là trường hợp đặc biệt của hình tứ giác và hình tứ giác là trường hợp tổng quát của hình vuông.

Quan hệ loại is-a cũng là quan hệ mà về mặt logic chúng ta có thể dùng tính kế thừa để thể hiện.

### __Định nghĩa về kế thừa__

Kế thừa là cơ chế cho phép một lớp B có thể có được các thuộc tính cũng như là phương thức của lớp A như thể các thuộc tính và phương thức đó đã được định nghĩa ở lớp B.

Tính kể thừa giữa hai lớp chỉ được sử dụng khi 2 lớp có mối quan hệ đặc biệt hóa - tổng quát hóa. Sự kế thừa là một mức cao hơn của trừu tượng hóa, nó giúp gom nhóm các lớp đối tượng có liên quan với nhau thành một lớp đối tượng tổng quát cho toàn bộ các lớp đối tượng.

Trong ngôn ngữ lập trình C++, cung cấp cho ta nhiều kiểu kế thừa: Kế thừa ảo, kế thừa đơn, kế thừa tầng, đa kế thừa,...

### __Lợi ích của việc kế thừa__

Kế thừa cho phép xây dựng lớp mới từ lớp đã có (giúp giảm bớt thời gian cũng như sự phức tạp của code).

Kế thừa giúp chúng ta dễ dàng sửa chữa, nâng cấp hệ thống.

## __Các loại kế thừa cơ bản__

Trong ngôn ngữ lập trình C++ cung cấp cho ta ba loại kế thừa cơ bản:

* Kế thừa kiểu private: Tất cả các thuộc tính và phương thức của lớp cơ sở sẽ trở thành trạng thái private khi được kế thừa tại lớp dẫn xuất.
* Kế thừa kiểu protected: Các thuộc tính và phương thức của lớp cơ sở đang ở trạng thái public và protected sẽ trở thành trạng thái protected khi được kế thừa tại lớp dẫn xuất.
* Kế thừa kiểu public: Các thuộc tính và phương thức của lớp cơ sở đang ở trạng thái public sẽ trở thành trạng thái public khi được kế thừa tại lớp dẫn xuất. Và các thuộc tính và phương thước ở hai trạng thái private, protected sẽ giữ nguyên trạng thái khi được kế thừa tại lớp dẫn xuất.

Tuy nhiên, riêng trường hợp thuộc tính (lẫn phương thức) đang ở trạng thái `private` ở lớp cơ sở, vẫn sẽ được kế thừa tại lớp dẫn xuất nhưng lớp dẫn xuất `không được quyền sử dụng`. Tức là khi kế thừa, lớp dẫn xuất sẽ được thừa hưởng tất cả các thuộc tính và phương thức của lớp cơ sở, còn việc lớp dẫn xuất có truy cập được hay không là phụ thuộc vào trạng thái thuộc tính (cũng như phương thức) mà lớp cơ sở hiện có.

## __Cú pháp khai báo trong kế thừa__

Để thể hiện mối quan hệ giữa các đối tượng trong thế giới thực, ta cần phải có một sơ đồ thể hiện mối qua hệ đó. Ví dụ: giữa đối tượng cha và đối tượng con có quan hệ kế thừa,...

Giả sử rằng ta có 1 lớp A, ta lại có 1 lớp B và muốn B kế thừa lớp A theo phương thức public, ta code như sau

<div class="result" markdown>

``` c++ linenums="1"
class A{}
class B: public A{}
```

</div>

Khi đó lớp A được gọi là lớp `cơ sở` (base class), lớp B được gọi là lớp `dẫn xuất` (derived class).

Đặc biệt, lớp B lúc này sẽ được kế thừa toàn bộ thuộc tính và phương thức của lớp A nhưng không thể kế thừa phương thức thiết lập (hay còn gọi là contructor).

Một ví dụ khác:

<div class="result" markdown>

``` c++ linenums="1"
class A{}
class B: public A{}
class C: public B{}
```

</div>

Về mặt ngữ nghĩa, lớp C lúc này là lớp dẫn xuất của lớp B và lớp B là lớp cơ sở của lớp C. Việc nhận định lớp C là lớp dẫn xuất của lớp A (vì có sự bắc cầu kế thừa từ lớp A sang lớp B) là chưa đúng.

Về mặt logic, lớp C sẽ kế thừa tất cả thuộc tính và phương thức của lớp B. Hiển nhiên cũng sẽ kế thừa các thuộc tính và phương thức của lớp A (vì có sự bắc cầu kế thừa từ lớp A sang lớp B).

Và ví dụ trên cũng thể hiện cho 1 loại kế thừa tầng phổ biến.

## __Định nghĩa lại các thuộc tính, phương thức trong kế thừa__

Xét đoạn chương trình sau

<div class="result" markdown>

``` c++ linenums="1"
class A{
public:
    int a = 1;
}

class B: public A{
public:
    void change(){
        a = 2;
        std::cout << a;
    }
}
```

</div>

Lúc này chúng ta đã cập nhật giá trị của biến `a` từ giá trị `1` (trong lớp cơ sở) thành giá trị `2` (trong lớp dẫn xuất).

Tương tự, ta hoàn toàn có thể định nghĩa lại các phương thức cho phù hợp với những gì mà lớp dẫn xuất muốn. Và đây cũng là một lưu ý quan trọng trong việc ràng buộc dữ liệu từ lớp cơ sở xuống lớp dẫn xuất

## __Các vấn đề từ việc khai báo đối tượng trong kế thừa__

### __Contructor__

Trong nguyên tắc kế thừa, nếu ta muốn khởi tạo 1 đối tượng giá trị (hoặc đối tượng con trỏ) ở lớp dẫn xuất thì hàm khởi tạo (contructor) sẽ được thực thi tại lớp cơ sở trước, sau đó mới đến lớp dẫn xuất.

Trong trường hợp kế thừa nhiều tầng, thì contructor sẽ thực thi tuần tự từ lớp cơ sở lớn nhất dần đến lớp dẫn xuất cần khởi tạo.

Hãy xem xét và dự đoán đầu ra của đoạn chương trình bên dưới:

<div class="result" markdown>

``` c++ linenums="1"
using namespace std;

class A{
public:
    A(){
        cout << "A";
    }
};

class B: public A{
public:
    B(){
        cout << "B";
    }
};

class C: public B{
public:
    C(){
        cout << "C";
    }
};

int main(){
    C c;
}
```

</div>

Muốn khởi tạo 1 đối tượng giá trị `c` ở lớp đối tượng `C` thì contructor của lớp `B` sẽ được thực thi trước (hay có thể nói rằng đối tượng B sẽ được khởi tạo trước) vì `B` hiện đang là lớp cơ sở của lớp `C`, mà `B` khởi tạo thì contructor của lớp `A` sẽ được thực thi trước vì `A` là lớp cơ sở của `B`. 

Tóm lại đầu ra của đoạn chương trình trên là dòng chữ `ABC`.

### __Destructor__

Tương tự contructor nhưng destructor nó sẽ được thực thi theo thứ tự ngược lại (tức destructor của lớp dẫn xuất sẽ được thực thi trước, sau đó mới đến lượt lớp cơ sở).

Hãy xem xét và dự đoán đầu ra của đoạn chương trình bên dưới:

<div class="result" markdown>

``` c++ linenums="1"
using namespace std;

class A{
public:
    ~A(){
        cout << "A";
    }
};

class B: public A{
public:
    ~B(){
        cout << "B";
    }
};

class C: public B{
public:
    ~C(){
        cout << "C";
    }
};

int main(){
    C c;
}
```

</div>

Đoạn chương trình trên có đầu ra là dòng chữ `CBA`.

Trong phương thức destructor ta còn 1 vấn đề nhỏ là `virtual destructor` và tôi sẽ đề cập sau trong phần lý thuyết về đa hình.

### __Kế thừa ảo__

Với tình huống giả định có nhiều hơn 1 lớp cơ sở ứng với 1 lớp dẫn xuất, thì lúc này sẽ xảy ra trường hợp lỗi mơ hồ tức là lúc này lớp dẫn xuất đang không hiểu nó muốn được kế thứa từ lớp nào. Ta dùng kế thừa ảo để giải quyết trường hợp này bằng việc thêm từ khóa `virtual` trước loại kế thừa. Xem xét đoạn code bên dưới:

<div class="result" markdown>

``` c++ linenums="1"
using namespace std;

class A{
};

class B: public A{
};

class C: public A{
};

class D: public B, public C{
};

int main(){
    A *a = new D;
}
```

</div>

Lúc này dòng `A *a = new D` sẽ bị lỗi mơ hồ vì cụ thể là có một sự ngắt quãng liên kết động giữa con trỏ thuộc lớp A tham chiếu đến lớp D. Để sửa lỗi này, ta chỉ cần thêm từ khóa `virtual` vào trước kiểu kế thừa của lớp B và C đối với lớp A. Xem xét đoạn mã hoàn chỉnh dưới đây:

<div class="result" markdown>

``` c++ linenums="1"
using namespace std;

class A{
};

class B: virtual public A{
};

class C: virtual public A{
};

class D: public B, public C{
};

int main(){
    A *a = new D;
}
```

</div>

Và đây cũng là một `Diamond problem` mà tôi đã đề cập phía dưới.

Trong một số trường hợp, ta phải dùng kế thừa ảo để giải quyết vấn đề và `Diamond problem` là một trường hợp điển hình cho việc làm này.

### __Upcasting__

Xem xét và dự đoán đầu ra của đoạn chương trình bên dưới:

<div class="result" markdown>

``` c++ linenums="1"
using namespace std;

class A{
private:
    int a = 1;
public:
    void action(){
        cout << "A";
    }
};

class B: public A{
public:
    void action(){
        cout << "B";
    }
};

int main(){
    B b;
    b.action();
}
```

</div>

Với đoạn chương trình trên đầu ra sẽ là chữ cái `B`. Điều này không có gì để bàn cãi nhỉ?

__Upcasting__ được hiểu đơn giản là khai báo một con trỏ đối tượng thuộc lớp cơ sở liên kết động với đối tượng ở lớp dẫn xuất hoặc từ một đối tượng con trỏ của lớp cơ sở tham chiếu đến một đối tượng của lớp dẫn xuất. Nghe có vẻ khá là khó hiểu đúng không? Hãy xem xét ví dụ bên dưới:

<div class="result" markdown>

``` c++ linenums="1"
using namespace std;

class A{
private:
    int a = 1;
public:
    virtual void action(){
        cout << "A";
    }
};

class B: public A{
public:
    void action(){
        cout << "B";
    }
};

int main(){
    A *b = new B;
    b->action();
}
```

</div>

Hiển nhiên, với đoạn chương trình trên đầu ra sẽ là chữ cái `B` y hệt chương trình cũ (để giải thích đầu ra cụ thể hơn, tôi sẽ nói rõ ở phần lý thuyết về đa hình). Nhưng rõ ràng trong đoạn code lúc này ta thấy rõ rằng có sự xuất hiện 1 con trỏ `b` của lớp đối tượng A tham chiếu đến lớp đối tượng B (tức con trỏ thuộc lớp cơ sở tham chiếu đến lớp dẫn xuất) 

Tóm lại, Upcasting dùng để tạo mối quan hệ giữa lớp cơ sở và lớp dẫn xuất.

### __Downcasting__

**Downcasting** là một quá trình ngược lại đối với **Upcasting**, nếu quá trình upcast là quá trình khai báo một con trỏ đối tượng thuộc lớp cơ sở để thực hiện việc liên kết động với đối tượng ở lớp dẫn xuất thì downcast là việc tạo một liên kết động giữa con trỏ thuộc lớp dẫn xuất sang một đối tượng thuộc lớp cơ sở. Nghe có vẻ vô lý đúng không? Chúng ta cùng xem ví dụ bên dưới:

<div class="result" markdown>

``` c++ linenums="1"
using namespace std;

class A{
private:
    int a = 1;
public:
    virtual void action(){
        cout << "A";
    }
};

class B: public A{
public:
    void action(){
        cout << "B";
    }
};

int main(){
    B *b = new B;
    b->action();
}
```

</div>

Hiển nhiên, đoạn chương trình trên sẽ in ra chữ `B`. 

=== "__Câu hỏi__"
    !!! question "__Câu hỏi__"
        Làm thế nào để đoạn chương trình trên có thể in ra chữ `A` mà không làm xuất hiện lỗi phân giải tĩnh?

    __Lỗi phân giải tĩnh[^1]__: Tôi sẽ đề cập sâu hơn về lỗi này ở phần lý thuyết đa hình, tại thời điểm này chúng ta hiểu đơn giản là không xóa đi chữ `virtual` ở hàm action() trong lớp đối tượng A.
    [^1]: Xem lỗi phân giải tĩnh tại https://stackoverflow.com/questions/41201266/how-does-resolution-of-class-member-identifiers-works-in-c
=== "__Hướng giải quyết__"
    !!! success "__Trả lời câu hỏi__"
        Có rất nhiều cách để chương trình xuất ra chữ `A`. Ví dụ ta có thể thay đổi đối tượng con trỏ `b` thuộc lớp B thành 1 đối tượng `a` thuộc lớp `A`, ta có đoạn mã cho cách giải quyết này như sau:

        <div class="result" markdown>

        ``` c++ linenums="1" hl_lines="20 21"
        using namespace std;

        class A{
        private:
            int a = 1;
        public:
            virtual void action(){
                cout << "A";
            }
        };

        class B: public A{
        public:
            void action(){
                cout << "B";
            }
        };

        int main(){
            A a;
            a.action();
        }
        ```

        </div>

        Hoặc ta có thể dùng đối tượng con trỏ cho lớp đối tượng `A` như sau:

        <div class="result" markdown>

        ``` c++ linenums="1" hl_lines="20 21"
        using namespace std;

        class A{
        private:
            int a = 1;
        public:
            virtual void action(){
                cout << "A";
            }
        };

        class B: public A{
        public:
            void action(){
                cout << "B";
            }
        };

        int main(){
            A *a = new A;
            a->action();
        }
        ```

        </div>

        Và còn nhiều cách nữa...

!!! question "Câu hỏi"

    Làm thế nào để đoạn chương trình trên có thể in ra chữ `A` với __trường hợp dùng đối tượng thuộc lớp đối tượng__ mà không làm xuất hiện lỗi phân giải tĩnh?

Thì để giải quyết trường hợp này, ta có thể dùng cơ chế `__Downcasting__`, và đoạn mã chương trình sẽ như sau:

``` c++ linenums="1"
using namespace std;

class A{
private:
    int a = 1;
public:
    virtual void action(){
        cout << "A";
    }
};

class B: public A{
public:
    void action(){
        cout << "B";
    }
};

int main(){
    B *b = new B;
    A a;
    b = (B *)&a;
    b->action();
}
```

Lúc này, ta đã downcast bằng cú pháp `b = (B *)&a` và đã giải quyết được vấn đề.

=== "__Câu hỏi__"
    !!! question "Câu hỏi"

        Vậy tại sao ta lại phải dùng downcasting phức tạp như vậy?
=== "__Giải quyết câu hỏi__"
    !!! success "Trả lời"
        Câu trả lời cho trường hợp này là tùy theo tình huống cũng như ngữ cảnh mà mình có thể áp dụng đúng cơ chế kế thừa thích hợp:

        * Ví dụ trong ngữ cảnh ta có lớp đối tượng `SinhVien` kế thừa lớp đối tượng `Nguoi` và ta đã định nghĩa một toán tử nào đó cho lớp `Nguoi` và mong muốn đối tượng thuộc lớp đối tượng `SinhVien` sử dụng được thì ta có thể dùng cơ chế Downcasting, hoặc Downcasting trong những vấn đề về nạp chồng hàm (tôi sẽ nói kĩ ở phần lý thuyết đa hình) giữa lớp cơ sở và lớp dẫn xuất.

        * Về phần Upcasting thì khá dễ và được sử dụng rộng rãi.

## __Đa kế thừa__

Đa kế thừa là việc cho phép 1 lớp dẫn xuất có thể kế thừa từ nhiều lớp cơ sở (khác hoàn toàn với việc 1 lớp cơ sở có thể cho nhiều lớp kế thừa). Chính vì điều này mà khiến cho việc đa kế thừa sẽ sinh ra nhiều vấn đề rắc rối khác.

## __Các vấn đề lớn trong đa kế thừa__

### __Diamond problem__

Diamond problem[^2] được hiểu nôm na là trạng thái xung đột giữa một lớp dẫn xuất D được kế thừa từ 2 lớp cơ sở B và C, và lớp B và C lại cùng là lớp dẫn xuất của lớp cơ sở A.

Hãy xem xét ví dụ bên dưới:

``` c++ linenums="1"
using namespace std;

class A{
protected:
    int a = 1;
public:
    void output(){
        cout << "a = " << a << endl;
    }
    
};

class B: public A{
protected:
    int b = 2;
public:
    void output(){
        cout << "b = " << b << endl;
    }
    
};

class C: public A{
public:
    int c = 3;
    void output(){
        cout << "c = " << c << endl;
    }
};

class D: public C, public B{
public:
    int d = 4;
    void output(){
        cout << "d = " << d << endl;
    }
};

int main(){
    A *a = new D;
    a->output();
    return 0;
}
```

Từ đoạn chương trình trên: Dễ hiểu rằng ta đang có 1 lớp cơ sở A, lớp B và lớp C lần lượt là 2 lớp dẫn xuất tương ứng với lớp cơ sở A. Lớp D là lớp dẫn xuất của hai lớp cơ sở B và C (tức đang xét trên phương diện lớp D).

Ta có sơ đồ lớp minh họa cho đoạn code trên như sau:

``` mermaid
classDiagram
  A <|-- B
  A <|-- C
  B <|-- D
  C <|-- D
  A : +String x
  A : +String y
  A : +doSomethingforA()

  class B{
    +doSomethingforB()
  }
  class C{
    +doSomethingforC()
  }
  class D{
    +doSomethingforD()
  }
```

Nhìn thì đoạn code trên có vẻ là hợp lý nhưng điều gì sẽ xảy ra nếu ta biên dịch nó? Đúng vậy, khi biên dịch chúng ta sẽ nhận lỗi mơ hồ (ambiguous) ngay thời điểm ta tạo liên kết động (A *a = new D) vì lúc này lớp D đang kế thừa cả 2 lớp C và B và chưa biết rốt cuộc lớp D sẽ kế thừa thuộc tính và phương thức của lớp C hay là B (vì lúc này trình biên dịch sẽ không hiểu được).

Và bug trên có tên gọi là `Diamond problem`. Để fix bug này, ta chỉ cần dùng `kế thừa ảo` lần lượt giữa lớp C và lớp B ứng với lớp A là ổn thỏa.

Nhưng có vẻ sau khi fix bug `Diamond problem` xong, có vẻ chương trình của chúng ta vẫn còn 1 lỗi nào đó...

Quay trở lại đoạn code, ta đang cung cấp 1 liên kết động giữa lớp cơ sở A cho lớp kế thừa D, và ta có sử dụng con trỏ để trỏ đến phương thức output() nhằm xuất ra thông tin của hàm output() hiện hữu trong lớp D (điều chúng ta mong muốn). Nhưng kết quả chương trình là `a = 1` tức là chỉ chạy hàm output() trong lớp cơ sở A mà hoàn toàn không đụng đến hàm output() trong lớp dẫn xuất D. Vậy vấn đề ở đây là gì? Làm sao để giải quyết?

Vấn đề ở đây là chương trình đang gặp lỗi phân giải tĩnh mà tôi sẽ đề cập ở phần lý thuyết về đa hình.


## __Tham khảo thêm__

[:octicons-arrow-right-24: Xem thêm tính kế thừa][Inheritance]

  [Inheritance]: https://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming)

[^2]: Xem thêm tại [https://github.com/hieuhdh/OOP/tree/master/Theory/Problems/Polymorphism/runTime/DiamondProblem](https://github.com/hieuhdh/OOP/tree/master/Theory/Problems/Polymorphism/runTime/DiamondProblem)