# Elements of the Sampling Problem

## 1. Giới thiệu

Lấy mẫu là một khía cạnh cốt lõi trong nghiên cứu khảo sát, mới mục tiêu chính là đưa ra các suy luận về 1 quằn thể dựa trên một phần nhỏ của quần thể đó gọi là mẫu, thường tập chung vào các chỉ số như trung bình, tổng, tỉ lệ kèm theo 1 giới hạn sai số nhất định. 

## 2. Các thuật ngữ cơ bản

- Phần tử: Là đối tượng đơn lẻ cần khảo sát
- Tổng thể: Là tập hợp tất cả các phần tử mà ta quan tâm
- Khung mẫu: Là danh sách tất cả các phần tử trong tổng thể, từ đó ta sẽ chọn mẫu
- Chọn mẫu: Là quá trình chọn ra 1 hoặc một tập hợp các phần từ từ khung mẫu để tạo thành mẫu khảo sát

Tại sao cần lấy mẫu?

- Tiết kiệm thời gian và chi phí
- Bảo toàn đối tượng nghiên cứu (vì có những nghiên cứu yêu cầu phá hủy đối tượng nghiên cứu, i.e. kiểm thử độ bền của vật liệu)
- Khi tổng thể là vô hạn
- Khi đang kiểm định giả thuyết (ví dụ thử một loại thuốc mới, ta không thể thử trên toàn bộ dân số)


## 3. Quy trình lấy mẫu

```mermaid
flowchart LR
	A[1. Xác định tổng thể và phần tử nghiên cứu] --> B[2. Xây dựng khung mẫu]
	B --> C[3. Xác định số lượng mẫu cần thiết]
	C --> D[4. Chọn phương pháp lấy mẫu]
	D --> E[5. Tiến hành lấy mẫu theo phương pháp đã chọn]
```


## 4. Các phương pháp chọn mẫu chính

- `Chọn mẫu xác suất`: Mỗi phần tử trong tổng thể có một xác suất nhất định để được chọn vào mẫu. Các phương pháp phổ biến bao gồm:
  - Chọn mẫu ngẫu nhiên đơn giản (Simple Random Sampling)
  - Chọn mẫu hệ thống (Systematic Sampling)
  - Chọn mẫu phân tầng (Stratified Sampling)
  - Chọn mẫu cụm (Cluster Sampling)
-> Phương pháp này cho ước lượng chính xác hơn và có tính đại diện cao.

- `Chọn mẫu phi xác suất`: Chọn phần tử dựa trên kĩ năng hoặc sự tiện lợi của nhà nghiên cứu, không theo quy luật ngẫu nhiên.

## 5. Sai số trong khảo sát

- `Sai số không quan sát`: Là sai số phát sinh do mẫu không đại diện cho tổng thể, hoặc do đối tượng không phản hồi.
- `Sai số quan sát`: Là sai số phát sinh do lỗi trong quá trình thu thập dữ liệu, như lỗi đo lường, lỗi nhập liệu, hoặc lỗi do người trả lời không trung thực.