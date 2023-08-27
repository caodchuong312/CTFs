![image](https://github.com/caodchuong312/CTFs/assets/92881216/ae0ed3b4-8fbd-46d4-adcc-07a76e664156)

Bài có form signup và login.

Xem source:
- có vài thông tin quan trọng như `secret key` để sign `JWT` được sử dụng trong bài:

![image](https://github.com/caodchuong312/CTFs/assets/92881216/93c00496-daf8-4618-ae7f-d9f66f5570e6)

- ngoài ra có 2 `decorator`:

  ![image](https://github.com/caodchuong312/CTFs/assets/92881216/efc3017c-c9a4-48ad-86ea-271b01dd6b44)

  2 hàm này sẽ thực thi khi xử lý vởi jwt nhằm mục đính dùng `sub` trong `jwt` để xác định user.

  Giờ vào form signup sẽ có các thông tin: `username`, `name`, `description`, `password`, nhìn vào `profile.html`:

  ![image](https://github.com/caodchuong312/CTFs/assets/92881216/6d14f6d6-2b18-456d-b9a4-19c33f45997e)

Có vẻ như `name` và `description` có thể dẫn đến **SSTI** nhưng không phải vì mình đã fuzz qua nhiều payloads :(.

Việc cho `secret key` có thể set được `jwt` và access vào user khác, và chỉ cần đổi `sub` giá trị khác ví dụ là `1`.

![image](https://github.com/caodchuong312/CTFs/assets/92881216/25bdeb57-d602-42f7-b5ac-6bfd2a05d144)

>flag: `DUCTF{7h3-0n3-p13c3-15-4ll-7h3-fl465-y0u-637-4l0n6-7h3-w4y}`
