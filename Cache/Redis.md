# Redis

## 1. Giới thiệu

Ta biết rằng, khi có một request đến cơ sở dữ liệu, server sẽ phải truy xuất dữ liệu từ đĩa cứng với tốc độ rất chậm. Trong khi đó, các thao tác trên bộ nhớ RAM lại nhanh hơn rất nhiều. Redis được thiết kế để tận dụng lợi thế này bằng cách lưu trữ dữ liệu trong bộ nhớ, từ đó:
- Tăng tốc độ truy xuất dữ liệu.
- Giảm tải cho cơ sở dữ liệu chính, giúp server hoạt động hiệu quả hơn.

## 2. Kiến trúc

### 2.1. Nguyên lý hoạt động

Ý tưởng chính của Redis là đưa toàn bộ nội dung dữ liệu của một đối tượng vào RAM, và sử dụng 1 Key/ID để định danh nó, Key/ID này có thể chính là Key của đối tượng đó trong cơ sở dữ liệu. Như vậy, cấu trúc dữ liệu phù hợp được chọn là Map (hay Dictionary) với Key là ID/Key của đối tượng và Value là nội dung dữ liệu của đối tượng đó. Value có thể là một chuỗi JSON, một mảnh, boolean, float, một đối tượng được serialize, hoặc bất kỳ định dạng nào mà ứng dụng của bạn sử dụng để lưu trữ dữ liệu.

`Luồng hoạt động` :

Khi có 1 request đến, hệ thống trước tiên sẽ kiểm tra ID/Key của đối tượng cần truy xuất trong Redis:
- Nếu có, hệ thống sẽ trả về dữ liệu từ Redis ngay lập tức. 
- Nếu không có thì sẽ truy xuất trong cỡ sở dữ liệu chính, sau đó lưu trữ kết quả vào Redis để các request tiếp theo có thể truy xuất nhanh hơn.

`Giải phóng bộ nhớ` :

RAM vừa đắt lại vừa hạn chế, không thế cứ thế đẩy tất cả lên RAM được. Redis có cơ chế giải phóng bộ nhớ khi đạt đến giới hạn đã định trước. Khi đó, Redis sẽ loại bỏ các mục dữ liệu cũ hoặc ít sử dụng nhất để nhường chỗ cho dữ liệu mới. Có các ngưỡng chính:
- TTL (Time To Live): Set expire time cho các mục dữ liệu, sau một khoảng thời gian nhất định, Redis sẽ tự động xóa chúng khỏi bộ nhớ.
- maxmemory-policy: dev setup chính sách giải phóng bộ nhớ khi đạt `maxmemory`, có nhiều lựa chọn như: LRU (Least Recently Used), LFU (Least Frequently Used), Random, v.v.

`Thú vị` :

Có một vấn đề thú vị của hardware khi sử dụng Redis là hiện tượng dữ liệu cache bị phân tán trên thanh RAM, dẫn đến mặc dù dung lượng trống trên RAM còn nhiều những vẫn không đủ chỗ lưu dữ liệu mới. Cụ thể, ô trống trên RAM vốn dĩ được cấp phát theo từng block liên tiếp nhau, nhưng khi dữ liệu được xóa đi, các block trống này nằm rải rác khắp nơi trên RAM, RAM sẽ có thể không tìm được đủ block liên tiếp để lưu trữ dữ liệu mới nếu nó lớn hơn các "khe" trống này. 

Cách xử lí khuyến nghị: 

- Active Defragmentation (tự động, Redis ≥ 4.0)
```
activedefrag yes
active-defrag-ignore-bytes 100mb   # bắt đầu defrag khi lãng phí > 100MB
active-defrag-enabled yes
```

### 2.2. Backups

Vấn đề của RAM là dữ liệu không tồn tại vĩnh viễn, nếu server bị tắt đột ngột hoặc gặp sự cố, dữ liệu trong RAM sẽ bị mất. Để giải quyết vấn đề này, Redis định kì sao lưu dữ liệu xuống đĩa cứng thông qua cơ chế snapshot (RDB) hoặc ghi log (AOF). 
- RDB (Redis Database Backup): Redis sẽ tạo ra một bản sao của toàn bộ dữ liệu tại một thời điểm nhất định và lưu trữ nó dưới dạng file trên đĩa cứng. Cách này nhanh nhưng vẫn có thể mất dữ liệu nếu server gặp sự cố giữa các lần snapshot vì khoảng cách giữa các snapshot thường được set khá lớn do chi phí ghi file .rdb.
- AOF (Append Only File): Redis sẽ ghi lại tất cả các lệnh thay đổi dữ liệu vào một file log. Khi server khởi động lại, Redis sẽ đọc lại file log này kết hợp với snapshot để khôi phục dữ liệu. 