![image](https://github.com/caodchuong312/CTFs/assets/92881216/66c3e17b-ed21-4266-a35a-5340ad0adee0)

Web cho phép nhập `IP` và `Port` và theo source code nó sẽ thực thi `nmap`:

![image](https://github.com/caodchuong312/CTFs/assets/92881216/2e5b1944-b78e-4ecf-b8d9-5f5b40dcae8d)

Nhưng trước nó input đã được xử lý qua `escape_shell_input()` để filter các ký tự như sau:

![image](https://github.com/caodchuong312/CTFs/assets/92881216/2580d61c-e0e1-4d2d-926e-577609f504d3)

Sau đó tách `ip` và `port` và xử lý tiếp:

![image](https://github.com/caodchuong312/CTFs/assets/92881216/b57baa25-c998-409c-849c-3ff7433ad9dd)

Với `ip` thì ổn nhưng còn `port`: 

![image](https://github.com/caodchuong312/CTFs/assets/92881216/d7586bf1-5b16-460f-8c35-2536677043f9)

Nó xử lý qua `to_i` tức là ép kiểu sang `int` và sẽ bypass được chỗ này:

![image](https://github.com/caodchuong312/CTFs/assets/92881216/b21b65e0-bae4-4654-9294-eac3541546b7)

Như vậy là sẽ có thể inject thêm ví dụ như: `127.0.0.1:443%09blabla`.(`%09` tương được `\t` thay thế space). Theo file `Dockerfile`, flag sẽ có tên dạng `flag-[32 ký tự random].txt`:

![image](https://github.com/caodchuong312/CTFs/assets/92881216/a1a391eb-18a2-4e3e-8d10-81b00b167b6e)

Lợi dụng việc `nmap` có thể đọc file lấy host bằng `-iL` và `--log-errors` để ghi lại lỗi kết hợp với `-oN -` để đưa lỗi ra `stdout`. Giải thích chi tiết hơn ở <a href="https://explainshell.com/explain?cmd=nmap+-p+1337+127.0.0.1+-iL+%2Fflag-%3F%3F%3F%3F%3F%3F%3F%3F%3F%3F%3F%3F%3F%3F%3F%3F%3F%3F%3F%3F%3F%3F%3F%3F%3F%3F%3F%3F%3F%3F%3F%3F.txt+--log-errors+-oN+-">đây</a>.

Payload: `127.0.0.1:1337%09-iL%09/flag-????????????????????????????????.txt%09--log-errors%09-oN%09-%09`

![image](https://github.com/caodchuong312/CTFs/assets/92881216/8b91d9fc-a3b0-4d97-955c-7fa1f46f6972)

>flag: `SEKAI{4r6um3n7_1nj3c710n_70_rc3!!}`


