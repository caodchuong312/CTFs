![image](https://github.com/caodchuong312/CTFs/assets/92881216/630437b0-bbd1-4902-a041-dd62a719cb0f)

![image](https://github.com/caodchuong312/CTFs/assets/92881216/b6289e03-e440-4f49-addf-be812cdef5f5)

Có 3 link tương ứng với 3 enpoind:
- Explore: không có gì chỉ toàn rick roll.
- Render site (`/render`): có thể render ra `iframe` với `src` là `url` được lấy từ URL

![image](https://github.com/caodchuong312/CTFs/assets/92881216/b0ef886e-e450-48b8-a3e0-08f4cfcf15d9)

- Check connection (`/ping`): nhận tham số `host` và có vẻ như với giá trị nào cũng trả về `Status=OK`

![image](https://github.com/caodchuong312/CTFs/assets/92881216/504da08a-45c6-4131-83d5-189e5fb05236)

Xét phần `/render`, đây có vè như lỗ hổng **SSRF** nhưng vẫn chưa biết `flag` nằm đâu:

![image](https://github.com/caodchuong312/CTFs/assets/92881216/92b06025-1c28-4026-91e5-a772f922574b)

Sau khi fuzz 1 lúc vẫn không tìm được đường khai thác.

![image](https://github.com/caodchuong312/CTFs/assets/92881216/5e31d62c-babd-4fd0-8ac7-b86c84aa91ec)

fuzz lại web và nhận thấy có endpoind `secrets`:

![image](https://github.com/caodchuong312/CTFs/assets/92881216/ad7b08f6-db21-4011-9daf-42808e25b044)

![image](https://github.com/caodchuong312/CTFs/assets/92881216/7ae1f548-5586-4c9c-9cc3-37732d2e24a8)

Có vẻ như đây là chức năng chính cần khai thác, nhưng có vẻ được filter nghiêm nghặt :V

![image](https://github.com/caodchuong312/CTFs/assets/92881216/3c5a6af2-07a5-4145-9d35-21149d8b0d34)

Cũng có thể đây là 1 lỗ hống khác như **SQLi**.

![image](https://github.com/caodchuong312/CTFs/assets/92881216/dbd51c9b-c865-452f-976e-b90b86733bd3)

Và đây không phải lỗ hổng gì cả mà là `guess` challenge :V, giá trị `secret_name` là `flag`.

![image](https://github.com/caodchuong312/CTFs/assets/92881216/08831348-a9bd-4180-b429-3eab60f10ad8)

Và tuy nhiên là nó bị filter rồi. Và 1 người anh nói nó là <a href="https://book.hacktricks.xyz/pentesting-web/unicode-injection/unicode-normalization">Unicode Normalization</a>

hmm. 

ký tự `a` tương đương với `%c2%aa` (tìm nó ở <a href="https://appcheck-ng.com/wp-content/uploads/unicode_normalization.html">đây</a>.

![image](https://github.com/caodchuong312/CTFs/assets/92881216/c80ac0a8-6891-40ef-a8a4-b30002552999)


![image](https://github.com/caodchuong312/CTFs/assets/92881216/88416255-9f61-4ba5-818c-59e0be86163f)

>flag: `flag{h0w_d0es_this_even_w0rk}`







