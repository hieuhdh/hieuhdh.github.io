---
template: overrides/blog.html
icon: material/table-edit
title: Monoalphabetic cipher
description: >
  
search:
  exclude: true
hide:
  - feedback

tags:
  - Cryptography 
  - Substitution 
---

# __Monoalphabetic cipher__

<span>
:octicons-calendar-24: Mar 01, 2020 ·
:octicons-clock-24: ~5 minutes

</span>

---

__Monoalphabetic cipher__ là loại mật mã thay thế trong đó mỗi kí hiệu (chữ, chữ số, kí tự,...) trong văn bản thuần túy được ánh xạ tới mội kí hiệu (chữ, chữ số, kí tự,...) cố định trong bản mã. Trong đó mật mã Caesar là một mô tả điển hình cho __Monoalphabetic cipher__

__Mật mã Caesar__ là loại mật mã thay thế từng chữ cái (trong bảng chữ cái) trong văn bản thuần túy bởi một chữ cái khác (trong bảng chữ cái).

???+ example "__Ví dụ__"
    Ta có bảng ánh xạ các chữ cái tương ứng bởi bản rõ (plaintext) và bản mã (cipher) như sau:

    | Plaintext | A | B | C | D | E | F | G | H | I | J | K | L | M | N | O | P | Q | R | S | T | U | V | W | X | Y | Z |
    | --------- | - | - | - | - | - | - | - | - | - | - | - | - | - | - | - | - | - | - | - | - | - | - | - | - | - | - |
    | __Cipher__    | __H__ | __I__ | __J__ | __K__ | __L__ | __M__ | __N__ | __O__ | __P__ | __Q__ | __R__ | __S__ | __T__ | __U__ | __V__ | __W__ | __X__ | __Y__ | __Z__ | __A__ | __B__ | __C__ | __D__ | __E__ | __F__ | __G__ |

    Từ bảng ánh xạ trên, với `Cipher = "KLBALYP"` thì ta được `Plaintext = "DEUTERI"`.

Từ ví dụ trên, chú ý kĩ thì ta cũng dễ dàng tạo lập được thuật toán giải mã loại mã hóa này một cách dễ dàng. Gọi $k, 1\le k \le 25$ là độ dời kí tự giữa bản rõ (plaintext) và bản mã (cipher). Ta có thuật toán tìm bản mã khi có bản rõ như sau: 

- $Cipher[i] = E\big(k, Plaintext[i]\big) = \big(Plaintext[i] + k\big) \ mod \ 26$
- $Plaintext[i] = E\big(k, Cipher[i]\big) = \big(Cipher[i] - k\big) \ mod \ 26$

Ngoài ra, người ta còn gọi loại mã hóa như trên là mã hóa __ROT-X[^1]__ với $1\le X \le 25$

 [^1]: Tham khảo thêm tại http://hieuhdh.github.io/blog/cryptography/cryptographic-algorithms/substitution-cipher/monoalphabetic-cipher/2020-12-09-rotx/